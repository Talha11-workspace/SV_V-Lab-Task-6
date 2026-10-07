# Task 1 — Identify Constraints

## Automated Railway Level-Crossing Control System (ARLCCS)

## Scope and assumptions

This is a scenario-based educational verification model, not a certified railway design. Proposed failure policies and interlocks below require approval against the actual railway specification. The scenario does not provide numerical warning, closure, fault-response, or clearance deadlines; these must be specified before timed testing.

Rules apply at evaluated control-cycle checkpoints: after the relevant response deadline, except command guards, which apply when commands are issued. Propositional logic describes allowed snapshots; it does not prove response timing or event order.

Train_Present is ground-truth crossing occupancy, not merely a sensor bit. Clearance_Confirmed means fresh, validated evidence that every tracked train has cleared; startup with no train requires verified clear occupancy, not a fabricated departure event. Train_Permission means a software authorization output, not physical ability to stop a moving train. Barriers_Closed_Confirmed requires all relevant closed-position feedback, not a command acknowledgment. Barrier_Open means at least one barrier is physically fully open. A partly open barrier is not confirmed closed and is covered by C4/C6.

Fail_Safe_Active means protective control mode: suppress opening and train permission, request road STOP/warnings where functioning, retain barriers closed when physically possible, and record/escalate the fault. It does not prove a failed actuator moved or that a train can stop. Obstacle entrapment, track braking distances, redundancy, power-loss rules, and authorized emergency recovery need additional specification.

| Constraint ID | Constraint in simple English | Why necessary | Basis |
| --- | --- | --- | --- |
| C1 | A barrier must not be open while a train occupies the crossing. | Prevents road traffic entering an occupied crossing. | Scenario-derived |
| C2 | When a train is detected approaching, warning lights and the audible alarm must be active after the warning-response deadline. | Warns road users before barrier closure. | Scenario-derived; deadline unspecified |
| C3 | A close command may be issued only after the advance warning stage is complete. | Gives road users warning before the barriers descend. | Proposed refinement of scenario sequence |
| C4 | While a train is passing, every road barrier must be confirmed closed after the closure deadline. | Keeps the road route blocked for the full train passage. | Scenario-derived |
| C5 | An opening command is allowed only after current valid clearance is confirmed and no other train is approaching or present. | Prevents release on a stale clearance or between two trains. | Scenario-derived clearance rule; multiple-train refinement |
| C6 | Train passage permission may be granted only when all barriers are confirmed closed, warnings are active, and the road signal is STOP. | Avoids authorizing train movement before crossing protection is established. | Proposed interlock; train authority interface assumed |
| C7 | A detected train-sensor failure must inhibit barrier opening and activate the fail-safe response after the fault deadline. | Loss of detection must not be interpreted as proof of an empty crossing. | Proposed abnormal-response rule |
| C8 | A detected barrier failure must inhibit train permission and activate the fail-safe response after the fault deadline. | A faulty barrier cannot provide reliable road protection. | Proposed abnormal-response rule |
| C9 | Communication loss must activate local fail-safe operation and suppress remote opening requests. | Prevents stale or unverified control-center messages causing unsafe release. | Proposed communication-loss policy |
| C10 | Invalid or contradictory sensor readings must not establish clearance and must invoke fail-safe handling. | Prevents uncertain evidence being treated as a safe-to-open result. | Proposed validation rule |
| C11 | A crossing emergency must inhibit train permission and barrier opening and activate fail-safe handling. | Prevents normal release while an unresolved emergency exists. | Proposed emergency policy; emergency types unspecified |
| C12 | The road signal must show STOP whenever a train approaches/passes or any barrier is not confirmed fully open. | Prevents road traffic being released into a train movement or moving barrier. | Proposed road-signal interlock |
