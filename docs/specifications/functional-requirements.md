# Functional Requirements

---

## 1. Purpose

!!! info "Purpose"
    This document defines **what the system must do**.

    It specifies:

    - required system behaviors
    - allowed and disallowed actions
    - rules governing data creation, modification, and state transitions
    - guarantees the system must provide to users

    All requirements are **mandatory unless explicitly stated otherwise**.

---

## 2. Scope

!!! abstract "Scope"
    These requirements cover:

    - project setup and governance
    - deliverable-centric work management
    - task execution support
    - time logging
    - capacity awareness
    - prioritization and status management
    - visibility and reporting (functional, not visual)

    Non-functional qualities (performance, security, scalability) are defined separately.

---

## 3. Requirement Notation

!!! note "Requirement Notation"
    Each requirement is identified as: `FR-Category-Number`

    Example: `FR-PROJ-01`

---

## 4. Project Management

### FR-PROJ-01: Project Creation

The system **must allow** users to create a project with:

- name
- status
- optional descriptive metadata

A project must be created **before** any deliverables, tasks, or time logs can exist.

---

### FR-PROJ-02: Project Status Management

The system **must support** explicit project statuses.

Project status changes:

- must be intentional
- must not automatically alter deliverable or task status

---

### FR-PROJ-03: Project Archival

The system **must allow** projects to be archived.

Archived projects:

- remain readable
- disallow creation of new deliverables, tasks, or time logs
- preserve all historical data

Hard deletion is not permitted.

---

## 5. Phase Management

### FR-PHASE-01: Phase Definition

The system **must allow** phases to be defined per project.

Phases:

- are optional
- may be added, modified, or removed
- serve organizational purposes only

---

### FR-PHASE-02: Phase Independence

The system **must not** enforce:

- phase sequencing
- phase duration
- phase completion dependencies

---

## 6. Deliverable Management

### FR-DEL-01: Deliverable Creation

The system **must allow** users to create deliverables with:

- description
- completion criteria
- owner
- priority
- status

---

### FR-DEL-02: Ownership Enforcement

The system **must enforce** exactly one accountable owner per deliverable.

Ownership:

- must be assigned at creation
- must be transferable with explicit action

---

### FR-DEL-03: Deliverable Status Transitions

The system **must support** explicit deliverable status transitions.

Completion:

- must require explicit confirmation
- must record completion timestamp

---

### FR-DEL-04: Deliverable Independence From Tasks

The system **must allow** deliverables to:

- exist without tasks
- progress without task creation

---

### FR-DEL-05: Deliverable Priority

The system **must require** deliverables to have a relative priority.

Priority:

- must be editable
- must not be algorithmically derived

---

## 7. Task Management

### FR-TASK-01: Task Creation

The system **must allow** tasks to be created **only** within a deliverable context.

Tasks must not exist independently.

---

### FR-TASK-02: Task Mutability

The system **must allow** tasks to be:

- modified
- reassigned
- deleted

Deletion must not affect deliverable integrity.

---

### FR-TASK-03: Task Status Handling

The system **must support** task statuses.

Task status:

- does not influence deliverable status
- exists for execution coordination only

---

## 8. Time Logging

### FR-TIME-01: Time Log Creation

The system **must allow** users to log time:

- after work is performed
- against a project
- optionally against a deliverable or task

---

### FR-TIME-02: Time Log Integrity

The system **must enforce** that:

- duration is positive
- dates are not in the future
- logs represent actual effort

---

### FR-TIME-03: Time Log Independence

The system **must not**:

- auto-generate time logs
- infer time from task status
- require time logs for progress

---

### FR-TIME-04: Time Log Editing

The system **must allow** time logs to be corrected,
while preserving an audit trail where required.

---

## 9. Capacity Awareness

### FR-CAP-01: Capacity Definition

The system **must allow** weekly capacity to be defined per user.

Capacity:

- represents availability
- is optional
- may vary week to week

---

### FR-CAP-02: Capacity Constraint Visibility

The system **must surface** when planned or expected work exceeds capacity.

This indication:

- is advisory
- must not block work

---

### FR-CAP-03: No Utilization Enforcement

The system **must not**:

- require full capacity usage
- rank or score users by utilization

---

## 10. Priority and Planning

### FR-PLAN-01: Deliverable-First Planning

The system **must support** planning views that:

- prioritize deliverables
- show associated tasks optionally

---

### FR-PLAN-02: Priority Mutability

The system **must allow** priorities to change at any time.

Changes:

- do not require re-planning cascades
- must not invalidate historical data

---

## 11. Status and Progress

### FR-STAT-01: Explicit Status Representation

The system **must represent** status explicitly for:

- projects
- deliverables
- tasks

---

### FR-STAT-02: Progress Determination

The system **must determine progress** based on:

- deliverable completion
- qualitative state

The system **must not calculate** progress percentages.

---

## 12. Change Management

### FR-CHG-01: Deliverable Modification

The system **must allow** deliverables to be:

- modified
- split
- replaced

Changes must preserve history.

---

### FR-CHG-02: Scope Expansion Handling

The system **must support** adding new deliverables at any time.

The system must not absorb scope implicitly.

---

## 13. Visibility and Reporting

### FR-REP-01: Deliverable-Centric Reporting

The system **must provide** reports centered on:

- deliverables
- status
- ownership
- priority

---

### FR-REP-02: Time Reporting

The system **must allow** time data to be viewed:

- by project
- by deliverable
- by user
- by time period

---

### FR-REP-03: Capacity Awareness Reporting

The system **must surface** capacity constraints in reports,
without turning them into performance metrics.

---

## 14. Permissions (Functional)

### FR-PERM-01: Role-Based Access

The system **must support** role-based permissions at:

- project level
- deliverable level

---

### FR-PERM-02: Ownership Privileges

Deliverable owners **must be able** to:

- update deliverable status
- confirm completion
- reassign tasks

---

## 15. Prohibited Behaviors

### FR-PROH-01: Time-Driven Control

The system **must not**:

- schedule work automatically
- enforce deadlines
- block progress due to time variance

---

### FR-PROH-02: Metric-Driven Evaluation

The system **must not**:

- compute productivity scores
- rank individuals by output
- enforce utilization targets

---

## 16. Traceability Matrix (Excerpt)

| Requirement Area | Data Model Entity |
|------------------|-------------------|
| Project Mgmt     | Project           |
| Deliverable Mgmt | Deliverable       |
| Task Execution   | Task              |
| Time Tracking    | Time Log          |
| Capacity         | Weekly Capacity   |

---

## 17. Validation Criteria

!!! abstract "Validation Criteria"
    The system is considered compliant if:

    - all required behaviors are supported
    - all prohibited behaviors are impossible or discouraged by design
    - system outputs align with the methodology

---

## 18. Closing Statement

!!! quote "Closing Statement"
    These functional requirements ensure the system **does what it should — and refuses to do what it should not**.

    They translate the methodology into enforceable system behavior.

---

## 19. Auditability & Historical Integrity

### FR-AUD-01: Historical Preservation

The system **must preserve** historical records for:

- deliverables
- tasks
- time logs
- ownership changes
- status changes

Historical data:

- must not be silently overwritten
- must remain attributable to the original actor

---

### FR-AUD-02: Change Traceability

The system **must record** when the following change:

- deliverable owner
- deliverable scope or completion criteria
- deliverable status (especially completion)
- time log edits

Each record must include:

- timestamp
- actor
- type of change

---

### FR-AUD-03: Non-Destructive Editing

The system **must prefer**:

- versioning
- superseding records

over destructive updates for critical data.

---

## 20. Ownership & Accountability Enforcement

### FR-OWN-01: Owner-Only Completion Authority

The system **must restrict** deliverable completion confirmation to:

- the deliverable owner
- or an explicitly authorized role

---

### FR-OWN-02: Ownership Visibility

The system **must clearly expose** deliverable ownership in all:

- planning views
- reporting outputs
- deliverable detail contexts

Ownership must never be implicit.

---

### FR-OWN-03: Orphan Prevention

The system **must prevent**:

- removal of an owner
- archival of an owner account

if it would leave a deliverable without ownership.

---

## 21. Data Integrity & Validation Rules

### FR-DATA-01: Required Field Enforcement

The system **must enforce** presence of required fields defined in the Data Model.

Creation must fail if required data is missing.

---

### FR-DATA-02: Referential Integrity

The system **must enforce** referential integrity between:

- projects → deliverables
- deliverables → tasks
- deliverables / tasks → time logs

Dangling references are prohibited.

---

### FR-DATA-03: Cross-Project Contamination Prevention

The system **must prevent**:

- tasks from referencing deliverables in another project
- time logs from mixing project contexts

---

## 22. User Behavior Safeguards

### FR-SAFE-01: Overwork Signalization

The system **must allow** identification of:

- sustained capacity overruns
- unusually high logged time patterns

This must be:

- informational
- non-punitive
- non-automated in response

---

### FR-SAFE-02: Transparency Protection

The system **must not penalize** users for:

- reporting blocked status
- logging less time
- adjusting estimates

---

### FR-SAFE-03: No Forced Optimizations

The system **must not**:

- recommend speed-ups
- auto-compress scope
- suggest “efficiency improvements” based on metrics

---

## 23. System Behavior Under Incomplete Information

### FR-INC-01: Graceful Incompleteness

The system **must function** when:

- estimates are missing
- capacity is undefined
- tasks are absent

Missing data must not block work.

---

### FR-INC-02: Explicit Unknowns

Where data is missing, the system **must display**:

- “Unknown”
- “Not defined”

instead of inferred values.

---

## 24. Export, Interoperability & Portability

### FR-EXP-01: Data Export

The system **must allow** export of:

- projects
- deliverables
- tasks
- time logs

Exports must preserve identifiers and relationships.

---

### FR-EXP-02: Methodology Preservation on Export

Exported data **must not**:

- flatten deliverables into tasks
- convert deliverables into milestones
- infer progress metrics

---

## 25. Error Handling & User Feedback (Functional)

### FR-ERR-01: Constraint Violation Feedback

When an action violates a rule, the system **must**:

- block the action
- explain *why* it is blocked
- reference the violated constraint in human terms

---

### FR-ERR-02: No Silent Failure

The system **must not** fail silently.

All rejected actions must return explicit feedback.

---

## 26. System Guarantees (Non-Negotiable)

!!! warning "System Guarantees"
    The system **guarantees** that:

    - no work exists without context
    - no value exists without ownership
    - no time represents intention
    - no metric replaces judgment
    - no optimization overrides sustainability

---

## 27. Compliance Checklist (Summary)

!!! success "Compliance Checklist"
    The system is compliant only if:

    - deliverables are the dominant planning unit
    - ownership is explicit and enforced
    - time is observational, not prescriptive
    - capacity constrains without coercion
    - reports describe reality, not performance

---

## 28. Final Statement

!!! quote "Final Statement"
    These Functional Requirements are **intentionally restrictive**.

    If the system feels “less automated” or “less optimized” than typical tools,
    that indicates correct implementation.

    The system exists to:

    - support professional judgment
    - protect people
    - preserve truth

---
