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

```plantuml
@startuml
skinparam handwritten false
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