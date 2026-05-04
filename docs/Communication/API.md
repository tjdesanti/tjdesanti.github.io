# Final Node API Specification  
## Message Compliance Verification System  
**Author:** Tim Desanti  
 

---

## 1. Overview

This MicroPython script runs on an ESP32-S3-WROOM-1 and operates as a motor control node in a multi-device UART communication chain. Each device has a unique letter ID (W, B, F, T, H) and communicates by passing structured 64-byte packets over a physical UART connection, formatted as `AZ + SRC + DST + MSG + BY`. Incoming packets are validated, and then handled based on destination — packets addressed to this node (`T`) are acted on, packets for others are forwarded along the chain, and broadcast packets (`X`) are acted on locally and forwarded.

When a valid packet is received, the message body is checked for motor commands — `FWD` runs the motor forward, `REV` runs it in reverse, and `STOP` disables the outputs, all driven through an IFX9201 H-bridge over GPIO. The onboard button (Pin 13) also allows local motor state cycling for testing. A failsafe monitors the BOOT button (Pin 0) and safely shuts down the motor and exits if held for more than 1.5 seconds.

---

## 2. Team Communication Chain

```
Rylee (W) → Tim (T) → Bryce (B) → Riley (F) → Hattie (H)

```

| ID | Name   |
|----|--------|
| W  | Rylee  |
| B  | Bryce  |
| F  | Riley  |
| T  | Tim    |
| H  | Hattie |
| X  | Everyone (Broadcast) |

---

## 3. Message Structure (Byte-Level)

### 3.1 Packet Format

| Byte Index | Field  | Size (Bytes) | Description                      |
|------------|--------|-------------|-----------------------------------|
| 0–1        | PREFIX | 2           | Always `AZ` (0x41, 0x5A)         |
| 2          | SRC    | 1           | Sender ID (e.g. `B` = 0x42)      |
| 3          | DST    | 1           | Destination ID (e.g. `T` = 0x54) |
| 4–61       | MSG    | 58          | Message body (UTF-8, null padded) |
| 62–63      | SUFFIX | 2           | Always `BY` (0x42, 0x59)         |

### 3.2 Total Packet Size
```
64 bytes (fixed, null-padded)
```

---

## 4. Field Definitions

### 4.1 PREFIX
| Value | Description           |
|-------|-----------------------|
| `AZ`  | Start of every packet |

### 4.2 SRC
Single ASCII character identifying the sender node.

| Value | Sender |
|-------|--------|
| `W`   | Rylee  |
| `B`   | Bryce  |
| `F`   | Riley  |
| `T`   | Tim    |
| `H`   | Hattie |

### 4.3 DST
Single ASCII character identifying the destination node. Use `X` for broadcast.

### 4.4 MSG (Bytes 4–61)
- UTF-8 encoded text, up to 58 bytes
- Remaining bytes padded with `0x00`
- Motor command strings:

| Message | Action          |
|---------|-----------------|
| `FWD`   | Motor Forward   |
| `REV`   | Motor Reverse   |
| `STOP`  | Motor Stop      |
| `ACK`   | Acknowledgement |

### 4.5 SUFFIX
| Value | Description        |
|-------|--------------------|
| `BY`  | End of every packet |

---

## 5. Example Packet

### Bryce sending `FWD` to Tim:

| Byte Index | Value | Description     |
|------------|-------|-----------------|
| 0          | 0x41  | PREFIX `A`      |
| 1          | 0x5A  | PREFIX `Z`      |
| 2          | 0x42  | SRC `B` (Bryce) |
| 3          | 0x54  | DST `T` (Tim)   |
| 4          | 0x46  | MSG `F`         |
| 5          | 0x57  | MSG `W`         |
| 6          | 0x44  | MSG `D`         |
| 7–61       | 0x00  | Padding         |
| 62         | 0x42  | SUFFIX `B`      |
| 63         | 0x59  | SUFFIX `Y`      |

### Packet string representation:
```
AZBTFWDBY
```

---

## 6. Compliance Rules

A message is **VALID** only if:

### 6.1 Structural
- Exactly 64 bytes
- Bytes 0–1 are `AZ`
- Bytes 62–63 are `BY`

### 6.2 Field Validity
- SRC is a known ID (`W`, `B`, `F`, `T`, `H`, `X`)
- SRC is not equal to DST (no self-addressed packets)
- SRC is not this node's own ID (loop prevention)

### 6.3 Message Body
- MSG body matches a known command (`FWD`, `REV`, `STOP`) for motor actuation
- Unknown commands are accepted but logged; motor remains unchanged

---

## 7. System Behavior

| Condition               | Motor    | LED | Action               |
|-------------------------|----------|-----|----------------------|
| DST = `T`, MSG = `FWD`  | Forward  | ON  | ACK sent to sender   |
| DST = `T`, MSG = `REV`  | Reverse  | ON  | ACK sent to sender   |
| DST = `T`, MSG = `STOP` | Stop     | OFF | ACK sent to sender   |
| DST = `X` (Broadcast)   | Acted on | —   | Forwarded down chain |
| DST = other node        | No change| —   | Forwarded down chain |
| Invalid packet          | No change| —   | Discarded, logged    |

---

## 8. State Machine

```
IDLE → RECEIVE → VALIDATE → ROUTE → ACT / FORWARD → IDLE
```

---

## 9. Node Code

```python
from machine import UART, Pin, SPI
import time

# ==============================================================
# IDs
# ==============================================================
MY_ID        = ord('T')
BROADCAST_ID = ord('X')

KNOWN_IDS = {
    ord('W'), ord('B'), ord('F'),
    ord('T'), ord('H'), ord('X'),
}

ID_NAMES = {
    ord('W'): 'Rylee',
    ord('B'): 'Bryce',
    ord('F'): 'Riley',
    ord('T'): 'Tim',
    ord('H'): 'Hattie',
    ord('X'): 'Everyone',
}

# ==============================================================
# UART Setup
# ==============================================================
uart = UART(2, 9600, tx=43, rx=44)
uart.init(9600, bits=8, parity=None, stop=1)

# ==============================================================
# LED
# ==============================================================
led = Pin(4, Pin.OUT)

# ==============================================================
# Motor / SPI Setup
# ==============================================================
spi = SPI(2, baudrate=1_000_000, polarity=0, phase=1,
          sck=Pin(10), mosi=Pin(12), miso=Pin(8))

cs      = Pin(11, Pin.OUT, value=1)
dis     = Pin(9,  Pin.OUT, value=1)
pwm_pin = Pin(18, Pin.OUT, value=0)
dir_pin = Pin(16, Pin.OUT, value=0)

# ==============================================================
# Motor State
# ==============================================================
MOTOR_STOP = 0
MOTOR_FWD  = 1
MOTOR_REV  = 2
motor_state = MOTOR_STOP

def apply_motor_state(state):
    global motor_state
    motor_state = state
    if state == MOTOR_FWD:
        pwm_pin.value(0); dis.value(1); time.sleep_ms(100)
        dir_pin.value(1); time.sleep_ms(50)
        dis.value(0);     time.sleep_ms(10)
        pwm_pin.value(1); led.value(1)
        print("Motor: FORWARD")
    elif state == MOTOR_REV:
        pwm_pin.value(0); dis.value(1); time.sleep_ms(100)
        dir_pin.value(0); time.sleep_ms(50)
        dis.value(0);     time.sleep_ms(10)
        pwm_pin.value(1); led.value(1)
        print("Motor: REVERSE")
    else:
        pwm_pin.value(0); dis.value(1)
        led.value(0)
        print("Motor: STOP")

# ==============================================================
# Debug Button (Pin 13) — cycles STOP -> FWD -> REV
# ==============================================================
debug_btn      = Pin(13, Pin.IN, Pin.PULL_UP)
last_btn_state = 1
last_press_ms  = 0
DEBOUNCE_MS    = 50
CYCLE = [MOTOR_STOP, MOTOR_FWD, MOTOR_REV]

def poll_debug_button():
    global last_btn_state, last_press_ms
    now   = time.ticks_ms()
    state = debug_btn.value()
    if state == 0 and last_btn_state == 1:
        if time.ticks_diff(now, last_press_ms) > DEBOUNCE_MS:
            last_press_ms = now
            next_state = CYCLE[(motor_state + 1) % len(CYCLE)]
            print("BTN:", ["STOP","FWD","REV"][next_state])
            apply_motor_state(next_state)
    last_btn_state = state

# ==============================================================
# Failsafe (BOOT = Pin 0)
# ==============================================================
boot_btn        = Pin(0, Pin.IN, Pin.PULL_UP)
boot_hold_start = None

def safe_exit(msg):
    print(msg)
    apply_motor_state(MOTOR_STOP)
    led.value(0)
    time.sleep(1)
    raise SystemExit

time.sleep_ms(300)
if boot_btn.value() == 0:
    safe_exit("SAFE MODE: BOOT held at startup")

def poll_failsafe():
    global boot_hold_start
    if boot_btn.value() == 0:
        if boot_hold_start is None:
            boot_hold_start = time.ticks_ms()
        elif time.ticks_diff(time.ticks_ms(), boot_hold_start) > 1500:
            safe_exit("SAFE MODE: BOOT held during runtime")
    else:
        boot_hold_start = None

# ==============================================================
# Packet Constants
# ==============================================================
PREFIX     = b'AZ'
SUFFIX     = b'BY'
FRAME_SIZE = 64
MSG_START  = 4
MSG_END    = 62

def build_packet(dst, message=""):
    frame = bytearray(FRAME_SIZE)
    frame[0:2]  = PREFIX
    frame[2]    = MY_ID
    frame[3]    = dst
    msg_bytes   = message.encode('utf-8')[:MSG_END - MSG_START]
    frame[MSG_START:MSG_START + len(msg_bytes)] = msg_bytes
    frame[62:64] = SUFFIX
    return frame

def extract_message(pkt):
    return pkt[MSG_START:MSG_END].rstrip(b'\x00').decode('utf-8', 'ignore')

def packet_to_string(pkt):
    return "AZ" + chr(pkt[2]) + chr(pkt[3]) + extract_message(pkt) + "BY"

def send_ack(dst):
    pkt = build_packet(dst, "ACK")
    print(">> Sending ACK to", ID_NAMES.get(dst, chr(dst)))
    uart.write(pkt)

# ==============================================================
# Handle Incoming Packet
# ==============================================================
def handle_packet(pkt):
    if pkt[0:2] != PREFIX or pkt[62:64] != SUFFIX:
        print("ERROR: Malformed packet — discarded")
        return

    src      = pkt[2]
    dst      = pkt[3]
    msg      = extract_message(pkt)
    body     = pkt[MSG_START:MSG_END].rstrip(b'\x00')
    src_name = ID_NAMES.get(src, chr(src))
    dst_name = ID_NAMES.get(dst, chr(dst))

    print("SRC:", chr(src), "DST:", chr(dst))
    print("Packet string:", packet_to_string(pkt))

    if src not in KNOWN_IDS:
        print("ERROR: Unknown sender '{}' — discarded".format(chr(src)))
        return

    if src == MY_ID:
        print("Dropped own message (loop prevention)")
        return

    if dst == MY_ID:
        print(">> Message FOR ME (Tim) from {}".format(src_name))
        print(">> Content: '{}'".format(msg))
        led.value(1)
        send_ack(src)

        if body == b'FWD':
            apply_motor_state(MOTOR_FWD)
        elif body == b'REV':
            apply_motor_state(MOTOR_REV)
        elif body == b'STOP':
            apply_motor_state(MOTOR_STOP)
        else:
            print(">> Unknown command '{}' — motor unchanged".format(msg))
        return

    if dst == BROADCAST_ID:
        print("Broadcast from {}: '{}'".format(src_name, msg))
        if body == b'FWD':    apply_motor_state(MOTOR_FWD)
        elif body == b'REV':  apply_motor_state(MOTOR_REV)
        elif body == b'STOP': apply_motor_state(MOTOR_STOP)
        print("Forwarding broadcast...")
        uart.write(pkt)
        return

    print("Forwarding: {} -> {}: '{}'".format(src_name, dst_name, msg))
    uart.write(pkt)

# ==============================================================
# Buffer
# ==============================================================
buffer = bytearray()

# ==============================================================
# Startup
# ==============================================================
print("Tim (T) — Motor Node Ready")
apply_motor_state(MOTOR_STOP)

# ==============================================================
# Main Loop
# ==============================================================
while True:
    poll_failsafe()
    poll_debug_button()

    if uart.any():
        data = uart.read(uart.any())
        print("RAW RX bytes:", len(data), data)
        if data:
            buffer += data

        while len(buffer) >= 2 and buffer[0:2] != PREFIX:
            buffer = buffer[1:]

        while len(buffer) >= FRAME_SIZE:
            if buffer[0:2] == PREFIX and buffer[62:64] == SUFFIX:
                handle_packet(bytes(buffer[:FRAME_SIZE]))
                buffer = buffer[FRAME_SIZE:]
            else:
                buffer = buffer[1:]

        if len(buffer) > FRAME_SIZE * 2:
            print("ERROR: Buffer overflow — cleared")
            buffer = bytearray()

    time.sleep(0.01)
```

---

## 10. Compliance Summary

This implementation enforces:
- 64-byte fixed packet structure with `AZ` / `BY` framing
- Known sender ID verification
- Loop prevention (own packets dropped)
- Byte-level motor command matching (`FWD`, `REV`, `STOP`)
- ACK response on every message addressed to this node
- Failsafe shutdown via BOOT button hold
