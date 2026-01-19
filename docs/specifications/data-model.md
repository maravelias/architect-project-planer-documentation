# Data Model

---

## 1. Purpose

!!! info "Purpose"
    This document defines the **canonical data model** of the system.
    It specifies the structure, relationships, and constraints that govern architectural work.

    It specifies:
    - all core entities
    - their attributes
    - their relationships
    - structural constraints
    - invariants that must always hold

This model is **implementation-agnostic** but **structurally strict**.

---

## 2. Modeling Principles

!!! abstract "Modeling Principles"
    The data model adheres to the following principles:

    1. Deliverables are the primary unit of value
    2. Ownership and accountability are explicit
    3. Time records reality, never intent
    4. Structure prevents misuse
    5. Data favors clarity over optimization

---

## 3. Core Entities Overview

| Entity | Purpose |
|------|--------|
| Project | Contractual container for work |
| Phase | Professional grouping of deliverables |
| Deliverable | Contractual unit of value |
| Task | Temporary execution unit |
| Time Log | Record of actual effort |
| Weekly Capacity | Availability constraint |
| Role Assignment | Responsibility mapping |
| Priority | Relative importance indicator |
| Status | Explicit state representation |

---

## 4. Entity Definitions

---

## 5. Project

### 5.1 Description

!!! info "Project Definition"
    A **Project** represents a contractual or internal engagement under which work is performed.
    All work must belong to exactly one project.

---

### 5.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| project_id | UUID | ✅ | Unique identifier |
| name | String | ✅ | Project name |
| description | Text | ❌ | Contextual description |
| client | String / Reference | ❌ | Client or stakeholder |
| start_date | Date | ❌ | Informational only |
| end_date | Date | ❌ | Informational only |
| status | Enum | ✅ | Project state |
| created_at | Timestamp | ✅ | System metadata |
| updated_at | Timestamp | ✅ | System metadata |

---

### 5.3 Constraints

!!! note "Project Constraints"
    - A project must exist before any deliverable, task, or time log is created.
    - Project dates must not drive scheduling logic.
    - Deleting a project requires archival, not hard deletion.

---

## 6. Phase

### 6.1 Description

!!! info "Phase Definition"
    A **Phase** groups deliverables according to recognized professional stages.
    Phases provide structure, not sequence.

---

### 6.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| phase_id | UUID | ✅ | Unique identifier |
| project_id | UUID | ✅ | Parent project |
| name | String | ✅ | Phase name |
| description | Text | ❌ | Clarifying intent |
| order_index | Integer | ❌ | Display ordering only |
| status | Enum | ✅ | Phase state |

---

### 6.3 Constraints

!!! note "Phase Constraints"
    - A phase must belong to exactly one project.
    - Phases may overlap in time.
    - Phase status does not constrain deliverable status.

---

## 7. Deliverable

### 7.1 Description

!!! info "Deliverable Definition"
    A **Deliverable** is a contractual unit of value with explicit completion criteria and ownership.
    It is the **primary driver of planning, progress, and reporting**.

---

### 7.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| deliverable_id | UUID | ✅ | Unique identifier |
| project_id | UUID | ✅ | Parent project |
| phase_id | UUID | ❌ | Optional grouping |
| name | String | ✅ | Deliverable name |
| description | Text | ✅ | Scope definition |
| completion_criteria | Text | ✅ | Definition of done |
| owner_id | UUID | ✅ | Accountable owner |
| priority | Enum / Reference | ✅ | Relative importance |
| status | Enum | ✅ | Deliverable state |
| estimated_effort | Duration | ❌ | Approximate effort |
| created_at | Timestamp | ✅ | System metadata |
| completed_at | Timestamp | ❌ | Set on completion |

---

### 7.3 Constraints

!!! note "Deliverable Constraints"
    - Every deliverable must have exactly one owner.
    - Completion requires explicit confirmation.
    - Deliverables may exist without tasks.
    - Deliverable status is authoritative for progress.

---

## 8. Task

### 8.1 Description

!!! info "Task Definition"
    A **Task** is a temporary unit of execution that supports a deliverable.
    Tasks are optional and disposable.

---

### 8.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| task_id | UUID | ✅ | Unique identifier |
| deliverable_id | UUID | ✅ | Parent deliverable |
| name | String | ✅ | Task description |
| description | Text | ❌ | Execution detail |
| assignee_id | UUID | ❌ | Contributor |
| status | Enum | ✅ | Task state |
| created_at | Timestamp | ✅ | System metadata |
| completed_at | Timestamp | ❌ | Informational |

---

### 8.3 Constraints

!!! note "Task Constraints"
    - Tasks must always belong to a deliverable.
    - Tasks may be deleted without historical impact.
    - Task completion does not affect deliverable completion.

---

## 9. Time Log

### 9.1 Description

!!! info "Time Log Definition"
    A **Time Log** records actual time spent on work.
    It reflects reality after the fact.

---

### 9.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| time_log_id | UUID | ✅ | Unique identifier |
| project_id | UUID | ✅ | Redundant for integrity |
| deliverable_id | UUID | ❌ | Preferred reference |
| task_id | UUID | ❌ | Optional reference |
| user_id | UUID | ✅ | Who performed the work |
| date | Date | ✅ | Work date |
| duration | Duration | ✅ | Time spent |
| notes | Text | ❌ | Contextual detail |
| created_at | Timestamp | ✅ | System metadata |

---

### 9.3 Constraints

!!! note "Time Log Constraints"
    - Time logs must reference a project.
    - Time logs must not reference both a task and unrelated deliverable.
    - Duration must be positive.
    - Time logs must not be pre-filled.

---

## 10. Weekly Capacity

### 10.1 Description

!!! info "Weekly Capacity Definition"
    **Weekly Capacity** defines available working time per person per week.
    It constrains planning without enforcing utilization.

---

### 10.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| capacity_id | UUID | ✅ | Unique identifier |
| user_id | UUID | ✅ | Person |
| week_start | Date | ✅ | Week reference |
| available_hours | Duration | ✅ | Availability |
| notes | Text | ❌ | Adjustments |

---

### 10.3 Constraints

!!! note "Weekly Capacity Constraints"
    - Capacity is informational and advisory.
    - Missing capacity defaults to “unknown”, not zero.

---

## 11. Role Assignment

### 11.1 Description

!!! info "Role Assignment Definition"
    Role assignments define responsibility relationships between people and entities.

---

### 11.2 Attributes

| Field | Type | Required | Description |
|------|----|---------|-------------|
| role_id | UUID | ✅ | Unique identifier |
| entity_type | Enum | ✅ | Project / Deliverable |
| entity_id | UUID | ✅ | Target entity |
| user_id | UUID | ✅ | Assigned person |
| role | Enum | ✅ | Role type |

---

### 11.3 Constraints

!!! note "Role Assignment Constraints"
    - Deliverables must have exactly one owner role.
    - Projects must have at least one accountable role.

---

## 12. Enumerations

### 12.1 Status (Deliverable / Task / Project)

| Value | Meaning |
|----|--------|
| Not Started | Work not begun |
| In Progress | Active work |
| Blocked | Cannot proceed |
| Under Review | Awaiting feedback |
| Complete | Finished |

---

### 12.2 Priority

| Value | Meaning |
|----|--------|
| High | Immediate attention |
| Normal | Default |
| Low | Defer if needed |

---

## 13. Relationship Diagram

```kroki-plantuml
@startuml
skinparam linetype ortho
skinparam shadowing false
skinparam class {
    BackgroundColor White
    BorderColor #333333
}

entity Project {
    * project_id
    --
    name
    status
}

entity Phase {
    * phase_id
    --
    project_id
    name
}

entity Deliverable {
    * deliverable_id
    --
    project_id
    phase_id
    owner_id
    status
}

entity Task {
    * task_id
    --
    deliverable_id
    status
}

entity TimeLog {
    * time_log_id
    --
    project_id
    deliverable_id
    task_id
    duration
}

Project ||--o{ Phase : contains
Project ||--o{ Deliverable : owns
Phase ||--o{ Deliverable : groups
Deliverable ||--o{ Task : supported by
Task ||--o{ TimeLog : records
Deliverable ||--o{ TimeLog : records
@enduml
```

---

## 14. Invariants Summary

!!! warning "System Invariants"
    The following must always be true to maintain system integrity:

    - No work exists outside a project
    - No deliverable exists without ownership
    - Time logs record past reality only
    - Tasks do not define progress
    - Capacity constrains, never mandates

---

## 15. Closing Statement

!!! quote "Closing Statement"
    This data model embeds the methodology into structure.
    If implemented faithfully, it becomes difficult to misuse the system — which is the intent.

