# Task 2 — Formalize Constraints

## Scope and assumptions

This is a scenario-based educational verification model, not a certified railway design. Proposed failure policies and interlocks below require approval against the actual railway specification. The scenario does not provide numerical warning, closure, fault-response, or clearance deadlines; these must be specified before timed testing.

Rules apply at evaluated control-cycle checkpoints: after the relevant response deadline, except command guards, which apply when commands are issued. Propositional logic describes allowed snapshots; it does not prove response timing or event order.

Train_Present is ground-truth crossing occupancy, not merely a sensor bit. Clearance_Confirmed means fresh, validated evidence that every tracked train has cleared; startup with no train requires verified clear occupancy, not a fabricated departure event. Train_Permission means a software authorization output, not physical ability to stop a moving train. Barriers_Closed_Confirmed requires all relevant closed-position feedback, not a command acknowledgment. Barrier_Open means at least one barrier is physically fully open. A partly open barrier is not confirmed closed and is covered by C4/C6.

Fail_Safe_Active means protective control mode: suppress opening and train permission, request road STOP/warnings where functioning, retain barriers closed when physically possible, and record/escalate the fault. It does not prove a failed actuator moved or that a train can stop. Obstacle entrapment, track braking distances, redundancy, power-loss rules, and authorized emergency recovery need additional specification.

## Logical notation

∧ = AND; ∨ = inclusive OR; ¬ = NOT; → = implication. A → B is false exactly when A is TRUE and B is FALSE. Each rule is required at every applicable checkpoint.

| Variable | TRUE means |
| --- | --- |
| Train_Present | At least one train physically occupies the crossing. |
| Barrier_Open | At least one barrier is physically fully open. |
| Train_Approaching | An approaching train is detected and the approach latch remains active. |
| Warning_Lights_On | Warning-light activation is verified. |
| Audible_Alarm_On | Audible-alarm activation is verified. |
| Close_Command | A barrier-closing command is issued in this cycle. |
| Warning_Stage_Complete | Warnings are verified and the configured advance-warning interval is complete. |
| Train_Passing | A train is in its crossing passage phase. |
| Barriers_Closed_Confirmed | Every relevant barrier has valid closed-position feedback. |
| Open_Command | A barrier-opening command is issued in this cycle. |
| Clearance_Confirmed | Fresh validated clearance evidence is available for all trains. |
| Train_Permission | The controller currently grants permission for train passage. |
| Road_Stop | The road traffic signal is verified at STOP. |
| Sensor_Failure | A train-detection sensor fault has been detected. |
| Fail_Safe_Active | The defined protective control mode is active. |
| Barrier_Failure | A barrier actuator or position-feedback failure is detected. |
| Communication_Lost | The control-center link has exceeded its timeout. |
| Remote_Open_Accepted | A remote barrier-opening request is accepted in this cycle. |
| Invalid_Readings | Required sensor readings are implausible, stale, or contradictory. |
| Emergency_Active | An unresolved crossing emergency input is active. |
| All_Barriers_Open_Confirmed | All relevant barriers are confirmed fully open. |

## Formal expressions

| Constraint ID | Formal expression | Interpretation |
| --- | --- | --- |
| C1 | Train_Present → ¬Barrier_Open | A barrier must not be open while a train occupies the crossing. |
| C2 | Train_Approaching → (Warning_Lights_On ∧ Audible_Alarm_On) | When a train is detected approaching, warning lights and the audible alarm must be active after the warning-response deadline. |
| C3 | Close_Command → Warning_Stage_Complete | A close command may be issued only after the advance warning stage is complete. |
| C4 | Train_Passing → Barriers_Closed_Confirmed | While a train is passing, every road barrier must be confirmed closed after the closure deadline. |
| C5 | Open_Command → (Clearance_Confirmed ∧ ¬Train_Approaching ∧ ¬Train_Present) | An opening command is allowed only after current valid clearance is confirmed and no other train is approaching or present. |
| C6 | Train_Permission → (Barriers_Closed_Confirmed ∧ Warning_Lights_On ∧ Audible_Alarm_On ∧ Road_Stop) | Train passage permission may be granted only when all barriers are confirmed closed, warnings are active, and the road signal is STOP. |
| C7 | Sensor_Failure → (¬Open_Command ∧ Fail_Safe_Active) | A detected train-sensor failure must inhibit barrier opening and activate the fail-safe response after the fault deadline. |
| C8 | Barrier_Failure → (¬Train_Permission ∧ Fail_Safe_Active) | A detected barrier failure must inhibit train permission and activate the fail-safe response after the fault deadline. |
| C9 | Communication_Lost → (Fail_Safe_Active ∧ ¬Remote_Open_Accepted) | Communication loss must activate local fail-safe operation and suppress remote opening requests. |
| C10 | Invalid_Readings → (¬Clearance_Confirmed ∧ Fail_Safe_Active) | Invalid or contradictory sensor readings must not establish clearance and must invoke fail-safe handling. |
| C11 | Emergency_Active → (¬Train_Permission ∧ ¬Open_Command ∧ Fail_Safe_Active) | A crossing emergency must inhibit train permission and barrier opening and activate fail-safe handling. |
| C12 | (Train_Approaching ∨ Train_Passing ∨ ¬All_Barriers_Open_Confirmed) → Road_Stop | The road signal must show STOP whenever a train approaches/passes or any barrier is not confirmed fully open. |
