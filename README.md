# SV&V Lab Task 6

## Railway Level-Crossing Control System

Students: 

Talha Khalil 133
Abdullah Siddiqui 094
M. Mubashir 124

Scenario-based analysis of requirements, formal safety constraints, violations, and expected protective responses. Prepared with AI assistance for student review and explanation. No live railway software was tested.

| Task | Deliverable | Coverage |
|---|---|---|
| 1 | [Identify Constraints](Constraints/Identify_Constraints.md) | 12 rules, reasons, and source/assumption labels |
| 2 | [Formalize Constraints](Formalization/Formal_Constraints.md) | 12 expressions with variable definitions and modeling limits |
| 3 | [Constraint Violations](Verification/Constraint_Violations.md) | 12 counterexamples, valuations, failure explanations and expected responses |

Read Task 1 assumptions before interpreting Tasks 2 and 3. The scenario leaves numerical deadlines and exact failure policies unspecified. This work is an educational logical model, not a certified railway safety specification or proof of implementation correctness.

## Key reasoning

For A → B, a violation requires A = TRUE and B = FALSE. Issuing a close command does not prove physical closure; loss of a sensor signal does not prove clearance. A failed barrier cannot be repaired merely by setting a software flag.
