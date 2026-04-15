## Final Node Implementation

### Overview

This node is the **final stage** in the message chain. It verifies message compliance and controls hardware based on validated input.

---

### System Responsibilities

* Receive MQTT message
* Validate structure and checksum
* Execute command (motor ON/OFF)
* Publish validation result

---

### Pin Configuration (ESP32)

| Signal             | Pin |
| ------------------ | --- |
| SCK                | 10  |
| MOSI               | 12  |
| MISO               | 8   |
| CS                 | 11  |
| DIS (Motor Enable) | 9   |
| PWM                | 18  |
| DIR                | 16  |
| Button             | 13  |
| LED                | 4   |

---

### MQTT Configuration

**Broker:**

```
broker.hivemq.com
```

**Topics:**

```
Subscribe: team301/final/input  
Publish:   team301/final/status
```

---

### Message Format

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

### Compliance Rules

A message is considered **VALID** only if:

1. All required fields exist:

   * `node_id`
   * `timestamp`
   * `payload.command`
   * `checksum`

2. Field constraints:

   * `node_id` is not empty
   * `timestamp > 0`
   * `command` is either `"ON"` or `"OFF"`

3. Checksum is correct:

```
checksum = (sum of ASCII values of command) % 256
```

**Example:**

```
"ON" → 79 + 78 = 157 → VALID checksum = 157
```

---

### System Behavior

| Condition   | Motor | LED | Status Published |
| ----------- | ----- | --- | ---------------- |
| VALID + ON  | ON    | ON  | VALID            |
| VALID + OFF | OFF   | ON  | VALID            |
| INVALID     | OFF   | OFF | INVALID          |

---

### State Machine

```
IDLE → RECEIVE → VALIDATE → ACT → REPORT → IDLE
```

---

### Example Test Message

```
mosquitto_pub -h broker.hivemq.com -t team301/final/input -m '{"node_id":"node3","timestamp":12345,"payload":{"command":"ON"},"checksum":157}'
```

---

### Expected Output

* Motor turns ON
* LED turns ON
* MQTT publishes:

```
VALID
```

---

### Compliance Summary

This node ensures:

* Strict message validation
* Deterministic checksum verification
* Controlled actuation
* Observable system output

---

### Author

Tim Desanti
Team 301 – Embedded Systems Design

