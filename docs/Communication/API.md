# Final Node API Specification  
## Message Compliance Verification System  

**Author:** Tim Desanti  
**Course:** EGR 314 – Embedded Systems Design  

---

## 1. Overview
The Final Node is responsible for **message validation and actuation** within the system.  

It ensures that only **compliant messages**:
- Follow the required schema  
- Pass checksum validation  
- Contain valid command values  

are allowed to control hardware outputs.

---

## 2. System Responsibilities
- Subscribe to incoming MQTT messages  
- Validate message structure  
- Verify checksum correctness  
- Execute motor control logic  
- Publish validation result  

---

## 3. Hardware Interface

### 3.1 Pin Configuration

| Signal | Pin | Description |
|--------|-----|------------|
| SCK | 10 | SPI Clock |
| MOSI | 12 | SPI Data Out |
| MISO | 8 | SPI Data In |
| CS | 11 | Chip Select |
| DIS | 9 | Motor Enable |
| PWM | 18 | Motor Speed |
| DIR | 16 | Motor Direction |
| BUTTON | 13 | User Input |
| LED | 4 | Status Indicator |

---

## 4. Communication Interface

### 4.1 Protocol
- MQTT over TCP/IP  

### 4.2 Broker
```
broker.hivemq.com
```

### 4.3 Topics

| Direction | Topic |
|----------|------|
| Subscribe | `team301/final/input` |
| Publish | `team301/final/status` |

---

## 5. Message Specification

### 5.1 Input Message Format

```json
{
  "node_id": "string",
  "timestamp": 12345,
  "payload": {
    "command": "ON" | "OFF"
  },
  "checksum": 123
}
```

---

## 6. Compliance Requirements

### 6.1 Structural Requirements
The message MUST include:
- `node_id`
- `timestamp`
- `payload.command`
- `checksum`

---

### 6.2 Data Constraints

| Field | Requirement |
|-------|------------|
| node_id | Non-empty string |
| timestamp | Integer > 0 |
| command | "ON" or "OFF" |

---

### 6.3 Checksum Definition

```
checksum = (sum of ASCII values of command) % 256
```

#### Example
```
"ON" → 79 + 78 = 157 → checksum = 157
```

---

## 7. System Behavior

| Condition | Motor | LED | Status Output |
|----------|------|-----|---------------|
| VALID + ON | ON | ON | VALID |
| VALID + OFF | OFF | ON | VALID |
| INVALID | OFF | OFF | INVALID |

---

## 8. State Machine

```
IDLE → RECEIVE → VALIDATE → ACT → REPORT → IDLE
```

---

## 9. Test Procedure

### 9.1 Valid Message

```bash
mosquitto_pub -h broker.hivemq.com -t team301/final/input -m '{"node_id":"node3","timestamp":12345,"payload":{"command":"ON"},"checksum":157}'
```

---

### 9.2 Expected Result
- Motor turns ON  
- LED turns ON  
- MQTT publishes: `VALID`  

---

## 10. Compliance Summary
This implementation provides:
- Deterministic validation logic  
- Strict schema enforcement  
- Verified checksum computation  
- Observable system behavior  

---
