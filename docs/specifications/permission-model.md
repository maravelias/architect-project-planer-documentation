# Permission Model

---

## 1. Purpose

!!! info "Purpose"
    This document defines the **authorization rules** of the system.

    It specifies:
    - roles
    - privileges
    - ownership-based permissions
    - prohibited actions
    - escalation constraints

    This document is **authoritative** for all access control decisions.

---

## 2. Scope

!!! abstract "Scope"
    These permissions apply to:

    - all projects
    - all deliverables
    - all tasks
    - all time logs
    - all reports
    - all environments

    Authentication is assumed; this document governs **authorization only**.

---

## 3. Permission Notation

!!! note "Permission Notation"
    Permissions are expressed as: `<Actor> MAY <Action> <Entity> [UNDER <Conditions>]` 

    **Example:**
    `Deliverable Owner MAY confirm completion OF Deliverable`

---

## 4. Roles

### 4.1 System Roles (Global)

| Role | Description |
|----|------------|
| System Administrator | Manages system configuration and user lifecycle |
| Auditor | Read-only access for compliance and review |
| Standard User | Default authenticated user |

System roles apply across all projects.

---

### 4.2 Project Roles (Scoped)

| Role | Description |
|----|------------|
| Project Owner | Accountable for project governance |
| Project Contributor | Participates in project work |
| Project Viewer | Read-only project access |

A user may hold multiple project roles.

---

## 5. Ownership Principle

### 5.1 PERM-OWN-01: Ownership Supremacy

!!! info "PERM-OWN-01"
    Ownership **overrides** role permissions **only** for:

    - the owned deliverable
    - its direct tasks

    Ownership does not grant system-wide authority.

---

### 5.2 PERM-OWN-02: Single Owner Enforcement

!!! info "PERM-OWN-02"
    Each deliverable **must have exactly one owner** at all times.

    Ownership transfer must be explicit.

---

## 6. System Administrator Permissions

### 6.1 PERM-SYS-01: Administrative Authority

!!! abstract "PERM-SYS-01"
    System Administrator MAY:

    - create, deactivate, or reactivate user accounts
    - assign or revoke system roles
    - configure global system policies

---

### 6.2 PERM-SYS-02: Content Restrictions

!!! warning "PERM-SYS-02"
    System Administrator MUST NOT:

    - modify project data content
    - complete deliverables on behalf of users
    - alter audit logs

---

## 7. Auditor Permissions

### 7.1 PERM-AUD-01: Read Access

!!! abstract "PERM-AUD-01"
    Auditor MAY:

    - view all projects
    - view all deliverables, tasks, and time logs
    - access audit trails
    - export reports

---

### 7.2 PERM-AUD-02: Write Restrictions

!!! warning "PERM-AUD-02"
    Auditor MUST NOT:

    - create or modify any entity
    - assign roles
    - interact with operational workflows

---

## 8. Project Owner Permissions

### 8.1 PERM-PROJ-OWN-01: Project Governance

!!! abstract "PERM-PROJ-OWN-01"
    Project Owner MAY:

    - create and archive projects they own
    - assign project roles
    - define project phases
    - view all project data

---

### 8.2 PERM-PROJ-OWN-02: Deliverable Management

!!! abstract "PERM-PROJ-OWN-02"
    Project Owner MAY:

    - create, modify, or archive deliverables
    - assign deliverable owners
    - change deliverable priority

---

### 8.3 PERM-PROJ-OWN-03: Operational Restrictions

!!! warning "PERM-PROJ-OWN-03"
    Project Owner MUST NOT:

    - complete deliverables they do not own
    - edit time logs of other users (except correction workflows if authorized)
    - override audit constraints

---

## 9. Project Contributor Permissions

### 9.1 PERM-PROJ-CONTR-01: Operational Participation

!!! abstract "PERM-PROJ-CONTR-01"
    Project Contributor MAY:

    - view all project deliverables
    - create tasks under assigned deliverables
    - log time against authorized projects

---

### 9.2 PERM-PROJ-CONTR-02: Task Management

!!! abstract "PERM-PROJ-CONTR-02"
    Project Contributor MAY:

    - modify or delete tasks they created
    - update task status

---

### 9.3 PERM-PROJ-CONTR-03: Governance Restrictions

!!! warning "PERM-PROJ-CONTR-03"
    Project Contributor MUST NOT:

    - confirm deliverable completion (unless owner)
    - reassign deliverable ownership
    - change deliverable priority

---

## 10. Project Viewer Permissions

### 10.1 PERM-PROJ-VIEW-01: Visibility Only

!!! abstract "PERM-PROJ-VIEW-01"
    Project Viewer MAY:

    - view project data
    - view reports

---

### 10.2 PERM-PROJ-VIEW-02: Action Restrictions

!!! warning "PERM-PROJ-VIEW-02"
    Project Viewer MUST NOT:

    - create or modify any entity
    - log time
    - access audit logs

---

## 11. Deliverable Owner Permissions

### 11.1 PERM-DEL-OWN-01: Deliverable Control

!!! abstract "PERM-DEL-OWN-01"
    Deliverable Owner MAY:

    - update deliverable status
    - confirm deliverable completion
    - modify deliverable description and completion criteria

---

### 11.2 PERM-DEL-OWN-02: Execution Control

!!! abstract "PERM-DEL-OWN-02"
    Deliverable Owner MAY:

    - create, modify, or delete tasks under the deliverable
    - reassign tasks within the deliverable

---

### 11.3 PERM-DEL-OWN-03: Accountability

!!! info "PERM-DEL-OWN-03"
    Deliverable Owner MUST:

    - ensure deliverable has clear completion criteria
    - explicitly confirm completion

---

## 12. Task-Level Permissions

### 12.1 PERM-TASK-01: Creation Rights

!!! abstract "PERM-TASK-01"
    Task Creator MAY:

    - modify task details
    - delete the task

---

### 12.2 PERM-TASK-02: Assignment Rights

!!! abstract "PERM-TASK-02"
    Task Assignee MAY:

    - update task status
    - log time against the task

---

### 12.3 PERM-TASK-03: Structural Restrictions

!!! warning "PERM-TASK-03"
    No user MAY:

    - move a task to a different deliverable without authorization
    - create tasks outside a deliverable

---

## 13. Time Log Permissions

### 13.1 PERM-TIME-01: Self-Logging

!!! abstract "PERM-TIME-01"
    User MAY:

    - create time logs for themselves
    - edit their own time logs (within policy)

---

### 13.2 PERM-TIME-02: Third-Party Restrictions

!!! warning "PERM-TIME-02"
    User MUST NOT:

    - create time logs for another user
    - edit another user’s time logs

---

### 13.3 PERM-TIME-03: Management Rights

!!! abstract "PERM-TIME-03"
    Authorized roles MAY:

    - request correction of time logs
    - annotate time logs for audit purposes

---

## 14. Role Assignment & Revocation

### 14.1 PERM-ROLE-01: Project Level

!!! abstract "PERM-ROLE-01"
    Only Project Owners MAY:

    - assign or revoke project roles within their project

---

### 14.2 PERM-ROLE-02: System Level Overrides

!!! warning "PERM-ROLE-02"
    System Administrators MAY:

    - override role assignments only for compliance or recovery

    Overrides must be logged.

---

## 15. Archival & Deletion Permissions

### 15.1 PERM-ARCH-01: Project Archival

!!! abstract "PERM-ARCH-01"
    Project Owner MAY:

    - archive projects

---

### 15.2 PERM-ARCH-02: Deliverable Archival

!!! abstract "PERM-ARCH-02"
    Deliverable Owner MAY:

    - archive deliverables they own

---

### 15.3 PERM-ARCH-03: Deletion Prohibitions

!!! warning "PERM-ARCH-03"
    No role MAY:

    - hard delete projects, deliverables, tasks, or time logs

---

## 16. Escalation & Override Constraints

### 16.1 PERM-ESC-01: Global Prohibitions

!!! warning "PERM-ESC-01"
    No user MAY:

    - bypass ownership to complete work
    - impersonate another user
    - suppress audit records

---

### 16.2 PERM-ESC-02: Emergency Protocols

!!! note "PERM-ESC-02"
    Emergency overrides:

    - must be explicit
    - must be time-bound
    - must be audited

---

## 17. Permission Conflict Resolution

### 17.1 PERM-CONF-01: Resolution Order

!!! info "PERM-CONF-01"
    If multiple permissions apply:

    1. Prohibitions override allowances
    2. Ownership overrides project role (within scope)
    3. System role overrides project role (except audit restrictions)

---

## 18. Permission Transparency

### 18.1 PERM-TRAN-01: Feedback Requirements

!!! info "PERM-TRAN-01"
    The system MUST:

    - clearly communicate why an action is forbidden
    - indicate required role or ownership for access

---

## 19. Prohibited Permission Models

### 19.1 PERM-PROH-01: Prohibited Behaviors

!!! warning "PERM-PROH-01"
    The system MUST NOT:

    - infer permissions from activity
    - grant permissions based on time logged
    - auto-escalate privileges

---

## 20. Acceptance Criteria

!!! success "Acceptance Criteria"
    The Permission Model is compliant if:

    - every action maps to an explicit permission
    - no action relies on implicit authority
    - ownership is enforced consistently
    - auditability is preserved

---

## Final Statement

!!! quote "Final Statement"
    This Permission Model ensures:
    - authority is explicit
    - accountability is unavoidable
    - power cannot drift silently
    - trust is enforced by design

---

