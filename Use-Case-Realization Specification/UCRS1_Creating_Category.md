# Use-Case-Realization Specification: Creating a Category

**Nom Nom Now Team**  
**Project:** Nom Nom Now Webbasierte Essensplanungs-App  
Version: `1.0`

---

## Revision History

| Date | Version | Description | Author |
|------|----------|--------------|---------|
| 26/10/2025 | 1.0 | Initial version for Use Case "Creating a Category" | Nom Nom Now Team |

---

## Table of Contents
1. [Introduction](#1-introduction)  
   1.1 [Purpose](#11-purpose)  
   1.2 [Scope](#12-scope)  
   1.3 [Definitions, Acronyms, and Abbreviations](#13-definitions-acronyms-and-abbreviations)  
   1.4 [References](#14-references)  
   1.5 [Overview](#15-overview)  
2. [Flow of Events](#2-flow-of-events--design)  

---

## 1. Introduction

### 1.1 Purpose
The purpose of this Use-Case Realization Specification is to describe how the *Creating a Category* use case is realized within the Nom Nom Now system.  
It focuses on the collaboration between the frontend, backend, and database components to allow a user to create new recipe categories.

### 1.2 Scope
This specification applies to the *Creating a Category* use case from the Nom Nom Now web application.  
It describes how user interactions in the Vue.js frontend trigger backend operations in the Spring Boot service layer and how new category entities are persisted in the PostgreSQL database.

### 1.3 Definitions, Acronyms, and Abbreviations

| Term | Definition |
|------|-------------|
| FE | Frontend (Vue.js) |
| BE | Backend (Spring Boot) |
| DB | Database (PostgreSQL) |
| DTO | Data Transfer Object |
| API | Application Programming Interface |

### 1.4 References
- Use-Case Specification: *Creating a Category* (`UC1_Creating_Category.md`)  
- UML Activity Diagram and Sequence Diagram for *Creating a Category*  
- Rational Unified Process (RUP) Use-Case-Realization Specification Template (`rup_ucrs.dot`)  
- Project documentation: [Nom Nom Now](https://github.com/NomNomNow)

### 1.5 Overview
This document is organized as follows:
- Section 1 introduces the context and purpose.  
- Section 2 describes the design realization with diagrams and component interactions.  
- Section 3 lists non-functional and derived requirements related to this use case.

---

## 2. Flow of Events: Design

The *Creating a Category* use case allows users to add new food categories.  
The following describes the object collaboration between UI, controller, service, repository, and database layers.

### 2.1 Description

1. The user opens the “Category Overview” page.  
2. The frontend displays a button to create a new category.  
3. The user fills in the form (name, description) and submits it.  
4. The frontend sends a `POST /api/categories` request to the backend.  
5. The backend validates the input.  
   - If invalid, a `400 Bad Request` response is returned.  
   - If the category already exists, a `409 Conflict` response is returned.  
6. If valid, the backend saves the category entity via the repository to the PostgreSQL database.  
7. The backend responds with `201 Created` and returns the created category object.  
8. The frontend updates the UI and displays a success message.

---

### 2.2 Sequence Diagram

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend (Vue.js)
    participant BE as Backend (Spring Boot)
    participant DB as Database (PostgreSQL)

    User->>FE: Open "Create Category" page
    activate FE
    FE->>User: Show creation form
    deactivate FE

    User->>FE: Fill form and click "Create"
    activate FE
    FE->>BE: POST /api/categories { name, description }
    activate BE
    BE->>DB: Insert new category
    activate DB
    DB-->>BE: Return ID
    deactivate DB
    BE-->>FE: 201 Created + category data
    deactivate BE
    FE-->>User: Show success message and update list
    deactivate FE
