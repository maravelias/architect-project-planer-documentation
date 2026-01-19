# Core Concepts

This section defines the **fundamental building blocks** of the Architect Project Planner.

These concepts form a **shared language** between:

- architects using the system,
- developers implementing it,
- and stakeholders evaluating it.

They are intentionally **simple, stable, and non-technical**.

---

## 1. Purpose of Core Concepts

Before discussing workflows, estimation, or software implementation, it is necessary to clearly define **what exists in the system**.

Core Concepts answer the question:

!!! abstract "The Core Question"
    *What are the things an architect actually manages?*

They provide:

- **Conceptual clarity** across all project stages
- **Consistent terminology** for all stakeholders
- **A foundation** for the methodology and technical specifications

---

## 2. Conceptual Hierarchy

Architectural work is organized around a clear hierarchy of responsibility. Each level contains the next, ensuring that all effort is tied back to a specific professional deliverable.

```kroki-plantuml
@startuml
!option handwritten true
skinparam packageStyle rectangle
skinparam shadowing false
skinparam defaultFontName "Verdana"

card Project #f9f9f9 [
  **Project**
  --
  The overall architectural appointment
]

card Phase #f9f9f9 [
  **Phase**
  --
  Stage of work (e.g., Concept, Planning)
]

card Deliverable #f9f9f9 [
  **Deliverable**
  --
  The primary output (e.g., Drawing Set, Report)
]

card Task #f9f9f9 [
  **Task**
  --
  Specific action required for a deliverable
]

card TimeLog #f9f9f9 [
  **Time Log**
  --
  Recorded effort against a task
]

Project *-- Phase
Phase *-- Deliverable
Deliverable *-- Task
Task *-- TimeLog

@enduml
```

This hierarchy reflects:

- professional responsibility
- contractual structure
- and real decision-making boundaries

It is the backbone of the entire system.

---

## 3. Overview of Core Concepts

### Project
!!! info "Project"
    A **Project** represents a contractual engagement with a client.
    It defines the overall scope, timeline, and responsibility.

    Projects exist to **produce outcomes**, not to manage daily activity.

---

### Phase
!!! info "Phase"
    A **Phase** groups work according to professional or regulatory structure
    (e.g. Concept Design, Planning Approval, Detailed Design).

    Phases exist to **structure responsibility and sequencing**, not to schedule tasks.

---

### Deliverable
!!! info "Deliverable"
    A **Deliverable** is a concrete, reviewable output:
    a drawing package, submission, report, or response.

    Deliverables are the **primary unit of planning and control** in this system.

---

### Task
!!! info "Task"
    A **Task** is an internal unit of work required to complete a deliverable.

    Tasks exist to support execution,  
    but they do not define success on their own.

---

### Time Log
!!! info "Time Log"
    A **Time Log** records actual effort spent on a task or deliverable.

    Time logs exist to **create feedback**, not surveillance.

---

### Weekly Capacity
!!! info "Weekly Capacity"
    **Weekly Capacity** represents the realistic amount of work a person or team
    can commit to in a week.

    It exists to prevent over-commitment and protect sustainability.

---

## 4. Stability Over Time

Core Concepts are designed to:
- change rarely,
- remain valid across projects,
- and survive different implementations.

Workflows, tools, and interfaces may evolve —  
these concepts should not.

---

## 5. Relationship to Other Sections

- **Vision** explains *why* these concepts matter
- **Methodology** explains *how* they are used
- **Estimation** explains *how much* effort they require
- **Specifications** explain *how* they are structured in software

Core Concepts define **what exists** — nothing more, nothing less.

---

## 6. How to Read This Section

Each concept is documented independently and follows a consistent structure:

- Definition
- Purpose
- Responsibilities
- Boundaries (what it is / is not)
- Relationships
- Rules and examples

Readers are encouraged to start with **Deliverables**,  
as they represent the center of the system.

---

## 7. Guiding Principle

!!! quote "Guiding Principle"
    Clarity at the concept level prevents complexity everywhere else.

This section exists to establish that clarity.

---