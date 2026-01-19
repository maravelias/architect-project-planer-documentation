# API Contract

---

## 1. Purpose

!!! info "Purpose"
    This document defines the **authoritative API contract** for the system.

    It specifies:

    - endpoints
    - request/response semantics
    - validation rules
    - permission enforcement
    - error behavior
    - versioning guarantees

    This document is binding for all implementations and consumers.

---

## 2. Scope

!!! abstract "Scope"
    This contract applies to:

    - all external APIs
    - all internal service APIs
    - all integrations
    - all environments

    Any behavior not explicitly permitted here is forbidden.

---

## 3. API Style

!!! info "API Style"

    - RESTful, resource-oriented
    - JSON request/response bodies
    - Explicit versioning in URL
    - No implicit behavior
    - No side-effects outside declared resources

---

## 4. Base Conventions

### 4.1 Versioning

All endpoints MUST be prefixed with: `/api/v1`

Breaking changes require a new version.

---

### 4.2 Authentication

!!! note "Authentication"

    - All endpoints require authentication unless explicitly stated
    - Authentication is token-based
    - Tokens identify a single user

---

### 4.3 Authorization

!!! note "Authorization"

    - Authorization is enforced per endpoint
    - Ownership and role checks are mandatory
    - Failure MUST return `403 Forbidden`

---

### 4.4 Idempotency

!!! info "Idempotency"

    - `GET` is idempotent
    - `POST` is not idempotent unless stated
    - `PUT` is idempotent
    - `DELETE` performs archival, not hard deletion

---

## 5. Standard Response Envelope

All responses MUST follow this structure:

```json
{
  "data": { },
  "meta": {
    "requestId": "uuid",
    "timestamp": "ISO-8601"
  },
  "errors": []
}
```

Errors MUST populate the `errors` array and set `data` to `null`.

### 5.1 Standard Error Object

```json
{
  "code": "STRING_CODE",
  "message": "Human-readable explanation",
  "details": { }
}
```

---

## 6. Project Endpoints

### 6.1 Create Project

`POST /api/v1/projects`

Create a project.

**Permission:** Authenticated user

**Request:**

```json
{
  "name": "string",
  "status": "string",
  "metadata": { }
}
```

**Rules:**

- Name is required.
- Project is created in the active state unless specified.

---

### 6.2 Retrieve Project

`GET /api/v1/projects/{projectId}`

Retrieve project details.

**Permission:** Project Viewer or higher

---

### 6.3 Update Project

`PATCH /api/v1/projects/{projectId}`

Update project metadata or status.

**Permission:** Project Owner

**Rules:**
- Status change must be explicit.
- Does not cascade.

---

### 6.4 Archive Project

`DELETE /api/v1/projects/{projectId}`

Archive project.

**Permission:** Project Owner

**Rules:**

- Archival only.
- Idempotent.

---

## 7. Phase Endpoints

### 7.1 Create Phase

`POST /api/v1/projects/{projectId}/phases`

Create a phase.

**Permission:** Project Owner

---

### 7.2 Remove Phase

`DELETE /api/v1/phases/{phaseId}`

Remove a phase.

**Permission:** Project Owner

---

## 8. Deliverable Endpoints

### 8.1 Create Deliverable

`POST /api/v1/projects/{projectId}/deliverables`

Create a deliverable.

**Permission:** Project Owner

**Request:**

```json
{
  "description": "string",
  "completionCriteria": "string",
  "ownerId": "userId",
  "priority": "integer",
  "status": "string"
}
```

---

### 8.2 Retrieve Deliverable

`GET /api/v1/deliverables/{deliverableId}`

Retrieve deliverable details.

**Permission:** Project Viewer or higher

---

### 8.3 Update Deliverable

`PATCH /api/v1/deliverables/{deliverableId}`

Update deliverable.

**Permission:** Deliverable Owner or Project Owner

**Rules:**
- Ownership change must be explicit.
- Completion requires confirmation endpoint.

---

### 8.4 Complete Deliverable

`POST /api/v1/deliverables/{deliverableId}/complete`

Confirm deliverable completion.

**Permission:** Deliverable Owner

**Rules:**

- Records timestamp.
- Irreversible without an audit event.

---

### 8.5 Archive Deliverable

`DELETE /api/v1/deliverables/{deliverableId}`

Archive deliverable.

**Permission:** Deliverable Owner

---

## 9. Task Endpoints

### 9.1 Create Task

`POST /api/v1/deliverables/{deliverableId}/tasks`

Create task.

**Permission:** Project Contributor or Deliverable Owner

**Request:**

```json
{
  "description": "string",
  "assigneeId": "userId",
  "status": "string"
}
```

---

### 9.2 Update Task

`PATCH /api/v1/tasks/{taskId}`

Update task.

**Permission:** Task Creator or Deliverable Owner

---

### 9.3 Delete Task

`DELETE /api/v1/tasks/{taskId}`

Delete task.

**Permission:** Task Creator or Deliverable Owner

---

## 10. Time Log Endpoints

### 10.1 Create Time Log

`POST /api/v1/time-logs`

Create time log.

**Permission:** Authenticated user

**Request:**

```json
{
  "projectId": "id",
  "deliverableId": "id?",
  "taskId": "id?",
  "date": "YYYY-MM-DD",
  "durationMinutes": 60,
  "notes": "string"
}
```

**Rules:**
- duration > 0
- date not in future
- task implies deliverable

---

### 10.2 Edit Time Log

`PATCH /api/v1/time-logs/{timeLogId}`

Edit time log.

**Permission:** Time log owner

**Rules:**
- Original preserved in audit log.

---

## 11. Capacity Endpoints

### 11.1 Set Weekly Capacity

`PUT /api/v1/capacity/{userId}/{week}`

Set weekly capacity.

**Permission:** Self or authorized manager

---

### 11.2 View Capacity

`GET /api/v1/capacity/{userId}`

View capacity.

**Permission:** Self or Project Owner (read-only)

---

## 12. Reporting Endpoints

### 12.1 Deliverable Report

`GET /api/v1/reports/deliverables`

Deliverable-centric report.

**Permission:** Project Viewer or higher

**Query Params:**
- `projectId`
- `ownerId`
- `status`
- `priority`

---

### 12.2 Time Report

`GET /api/v1/reports/time`

Time report.

**Permission:** Project Viewer or higher

---

## 13. Audit Endpoints

### 13.1 Retrieve Audit Logs

`GET /api/v1/audit`

Retrieve audit logs.

**Permission:** Auditor or System Administrator

---

## 14. Export Endpoints

### 14.1 Export Project Data

`GET /api/v1/export/project/{projectId}`

Export project data.

**Permission:** Project Owner or Auditor

---

## 15. Error Handling Rules

!!! warning "Error Handling Rules"
    **ERR-01: Validation Errors**
    Return `400 Bad Request` with explicit error details.

    **ERR-02: Authorization Errors**
    Return:

    - `401 Unauthorized` if unauthenticated
    - `403 Forbidden` if unauthorized

    **ERR-03: Not Found**
    Return `404 Not Found` only if entity is invisible to the caller.

---

## 16. Prohibited API Behaviors

!!! danger "Prohibited API Behaviors"
    The API MUST NOT:

    - infer user intent
    - auto-transition status
    - return derived productivity metrics
    - expose internal identifiers without purpose

---

## 17. Backward Compatibility Guarantees

!!! info "Backward Compatibility Guarantees"

    - Existing endpoints MUST NOT change meaning
    - New fields must be additive
    - Deprecated endpoints must be documented

---

## 18. Acceptance Criteria

!!! success "Acceptance Criteria"
    The API is compliant only if:

    - every endpoint maps to functional requirements
    - every permission maps to the Permission Model
    - every error is explicit
    - no hidden behavior exists

---

## 19. Final Statement

!!! quote "Final Statement"
    This API Contract ensures:

    - the system cannot “do things behind the user’s back”
    - all power is visible at the boundary
    - integrations cannot subvert methodology

---



