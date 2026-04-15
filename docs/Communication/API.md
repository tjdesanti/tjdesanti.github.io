# Final Node API Specification  
## Message Compliance Verification System  

**Author:** Tim Desanti  
**Course:** EGR 314 – Embedded Systems Design  

---

## 1. Overview
The Final Node validates incoming messages at the **byte level** and executes motor control only if the message is compliant.

---

## 2. Message Structure (Byte-Level)

### 2.1 Packet Format

| Byte Index | Field        | Size (Bytes) | Description |
|-----------|-------------|-------------|------------|
| 0         | START       | 1           | Start byte (0x7E) |
| 1         | NODE_ID     | 1           | Sender node ID |
| 2–5       | TIMESTAMP   | 4           | Unix timestamp |
| 6         | COMMAND     | 1           | Control command |
| 7         | CHECKSUM    | 1           | Error detection |
| 8         | STOP        | 1           | Stop byte (0x7F) |

---

### 2.2 Total Packet Size
```
9 bytes
```

---

## 3. Field Definitions

### 3.1 START Byte
| Value | Description |
|------|------------|
| 0x7E | Indicates start of message |

---

### 3.2 NODE_ID
| Value | Description |
|------|------------|
| 0x01–0xFF | Unique node identifier |

---

### 3.3 TIMESTAMP (4 Bytes)
- 32-bit integer  
- Big-endian format  

Example:
```
0x00 0x00 0x30 0x39 → 12345
```

---

### 3.4 COMMAND

| Value | Meaning |
|------|--------|
| 0x01 | ON |
| 0x00 | OFF |

---

### 3.5 CHECKSUM

Checksum is computed as:

```
checksum = (sum of bytes 1 through 6) % 256
```

Includes:
- NODE_ID  
- TIMESTAMP (all 4 bytes)  
- COMMAND  

---

### 3.6 STOP Byte

| Value | Description |
|------|------------|
| 0x7F | End of message |

---

## 4. Example Packet

### 4.1 Input Values
- NODE_ID = 0x03  
- TIMESTAMP = 12345  
- COMMAND = ON  

---

### 4.2 Byte Representation

| Byte | Value | Description |
|-----|------|------------|
| 0 | 0x7E | START |
| 1 | 0x03 | NODE_ID |
| 2 | 0x00 | TIMESTAMP |
| 3 | 0x00 | TIMESTAMP |
| 4 | 0x30 | TIMESTAMP |
| 5 | 0x39 | TIMESTAMP |
| 6 | 0x01 | COMMAND (ON) |
| 7 | 0x6D | CHECKSUM |
| 8 | 0x7F | STOP |

---

## 5. Compliance Rules

A message is **VALID** only if:

### 5.1 Structural
- Correct packet length (9 bytes)  
- Valid START and STOP bytes  

### 5.2 Field Validity
- NODE_ID is within range  
- COMMAND is 0x00 or 0x01  

### 5.3 Checksum
- Matches computed value  

---

## 6. System Behavior

| Condition | Motor | LED | Status |
|----------|------|-----|--------|
| VALID + ON | ON | ON | VALID |
| VALID + OFF | OFF | ON | VALID |
| INVALID | OFF | OFF | INVALID |

---

## 7. State Machine

```
IDLE → RECEIVE → VALIDATE → ACT → REPORT → IDLE
```

---

## 8. Test Example

### Raw Packet (Hex)
```
7E 03 00 00 30 39 01 6D 7F
```

---

## 9. Compliance Summary
This implementation enforces:
- Byte-level validation  
- Deterministic checksum verification  
- Fixed packet structure  
- Reliable actuation control  

---
