---
title: Tim's Code
---
## Description 

The code for this project was developed for the ESP32 microcontroller using the ESP-IDF framework. 
It handles all communication between subsystems, including sensor data collection, motor control, 
and wireless data transmission via MQTT. The code is structured into modular components for 
readability and maintainability.

### Code Overview

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


### Final Code (PDF)
- Code PDF: [Download Here](https://github.com/user-attachments/files/27375853/MotorDrive_PythonCode.pdf)


### Final Code Project (.zip)
- Code Project (.zip): [Download Here](https://github.com/user-attachments/files/27375816/EGR314_Thonny_Motor.zip)

