# Formalize Constraints – Railway Level-Crossing Control System

## Logical Notation

| Symbol | Meaning           |
| ------ | ----------------- |
| `∧`    | AND               |
| `∨`    | OR                |
| `¬`    | NOT               |
| `→`    | Implies / If-Then |

### Variables Used

| Variable | Meaning                                        |
| -------- | ---------------------------------------------- |
| `T`      | Train is present at the crossing               |
| `A`      | Train is approaching the crossing              |
| `B`      | Barrier is open                                |
| `C`      | Barrier is closed                              |
| `W`      | Warning signals are active                     |
| `K`      | Train has completely cleared the crossing      |
| `S`      | Train sensor is working correctly              |
| `F`      | Barrier has failed                             |
| `M`      | Communication with control center is available |
| `E`      | Emergency condition exists                     |
| `R`      | Road traffic is allowed to cross               |
| `V`      | Sensor readings are valid/reliable             |
| `D`      | Safety violation/failure is detected           |

---

## Formalized Constraints

### C1 – Barrier must not open while a train is present

**Simple English:**
The barrier cannot be open when a train is present at the crossing.

**Formal expression:**

```text
T → ¬B
```

**Meaning:**
If a train is present, the barrier must not be open.

---

### C2 – Barrier can open only after the train has cleared the crossing

**Simple English:**
The barrier may open only when the train has completely cleared the crossing.

**Formal expression:**

```text
B → K
```

**Meaning:**
If the barrier is open, the train must already have completely cleared the crossing.

---

### C3 – Warning signals must be active when a train is approaching

**Simple English:**
Whenever a train is approaching, warning lights and alarms must be active.

**Formal expression:**

```text
A → W
```

**Meaning:**
If a train is approaching, the warning system must be active.

---

### C4 – Road traffic must not be allowed when a train is approaching

**Simple English:**
Road traffic must not be allowed to cross when a train is approaching.

**Formal expression:**

```text
A → ¬R
```

**Meaning:**
If a train is approaching, vehicles must not be given permission to cross.

---

### C5 – If the train is not confirmed to have cleared, the barrier remains closed

**Simple English:**
The barrier must remain closed whenever the system cannot confirm that the train has cleared the crossing.

**Formal expression:**

```text
¬K → C
```

**Meaning:**
If the train has not been confirmed as completely clear, the barrier must be closed.

---

### C6 – Sensor failure must result in a safe state

**Simple English:**
If the train-detection sensor fails, the system must prevent unsafe crossing access.

**Formal expression:**

```text
¬S → ¬R
```

**Meaning:**
If the sensor is not working correctly, road traffic must not be allowed to cross.

---

### C7 – Barrier failure must trigger a safety response

**Simple English:**
If the barrier fails while a train is approaching, the system must activate a safety response and alert the control center.

Let:

* `F` = Barrier failure
* `A` = Train approaching
* `W` = Warning system active
* `D` = Failure/violation reported

**Formal expression:**

```text
(F ∧ A) → (W ∧ D)
```

**Meaning:**
If the barrier fails while a train is approaching, warnings must remain active and the failure must be reported.

---

### C8 – Communication loss must not make the system unsafe

**Simple English:**
If communication with the control center is lost while a train is approaching, the system must keep the crossing in a safe state.

**Formal expression:**

```text
(¬M ∧ A) → ¬R
```

**Meaning:**
If communication is lost and a train is approaching, road traffic must not be allowed to cross.

---

## Additional Constraint

### C9 – Invalid sensor readings must not cause the barrier to open

**Simple English:**
The system must not open the barrier when sensor readings are unreliable.

**Formal expression:**

```text
¬V → ¬B
```

**Meaning:**
If sensor information is invalid or unreliable, the barrier must not open.

---

## Summary

| Constraint | Formal Expression   | Main Safety Rule                                             |
| ---------- | ------------------- | ------------------------------------------------------------ |
| C1         | `T → ¬B`            | Train present → Barrier cannot open                          |
| C2         | `B → K`             | Barrier open → Train must be clear                           |
| C3         | `A → W`             | Train approaching → Warnings active                          |
| C4         | `A → ¬R`            | Train approaching → Traffic not allowed                      |
| C5         | `¬K → C`            | Train not confirmed clear → Barrier closed                   |
| C6         | `¬S → ¬R`           | Sensor failure → Traffic not allowed                         |
| C7         | `(F ∧ A) → (W ∧ D)` | Barrier failure + train approaching → Safety response        |
| C8         | `(¬M ∧ A) → ¬R`     | Communication loss + train approaching → Traffic not allowed |
| C9         | `¬V → ¬B`           | Invalid sensor reading → Barrier cannot open                 |

## Key Verification Idea

A formal constraint describes a condition that **must always be true**.

For example:

```text
T → ¬B
```

If:

```text
T = TRUE
B = TRUE
```

then the constraint is **violated**, because a train is present while the barrier is open.

If:

```text
T = TRUE
B = FALSE
```

then the constraint is **satisfied**.

Therefore, formal constraints make safety requirements **precise, unambiguous, and easier to verify through test cases or formal analysis**.

