# Use-Case-Realization Specification: Creating a Recipe

**Nom Nom Now Team**  
**Project:** Nom Nom Now – Webbasierte Essensplanungs-App  
Version: `1.0`

---

## Revision History

| Date | Version | Description | Author |
|------|----------|--------------|---------|
| 26/10/2025 | 1.0 | Initial version for Use Case "Creating a Recipe" | Nom Nom Now Team |

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
The purpose of this Use-Case Realization Specification is to describe how the *Creating a Recipe* use case is realized within the Nom Nom Now system.  
It details the collaboration between frontend, backend, and database components that enables a user to create new recipes associated with one or more categories.

### 1.2 Scope
This specification applies to the *Creating a Recipe* functionality within the Nom Nom Now application.  
It covers the workflow from user input through validation to data persistence in the database and focuses on the interaction between components implemented in Vue.js, Spring Boot, and PostgreSQL.

### 1.3 Definitions, Acronyms, and Abbreviations

| Term | Definition |
|------|-------------|
| FE | Frontend (Vue.js) |
| BE | Backend (Spring Boot) |
| DB | Database (PostgreSQL) |
| DTO | Data Transfer Object |
| API | Application Programming Interface |

### 1.4 References
- Use-Case Specification: *Creating a Recipe* (`UC5_Creating_Recipe.md`)  
- UML Activity Diagram and Sequence Diagram for *Creating a Recipe*  
- Rational Unified Process (RUP) Use-Case-Realization Specification Template (`rup_ucrs.dot`)  
- Project documentation: [Nom Nom Now](https://github.com/NomNomNow)

### 1.5 Overview
This document provides:
- An overview of the use case and its realization (Section 1)  
- Detailed flow of events with UML diagrams (Section 2)  
- Derived non-functional and implementation requirements (Section 3)

---

## 2. Flow of Events

The *Creating a Recipe* use case enables users to add new recipes within a selected category.  
The design illustrates the interactions between the user interface, controller, service, repository, and database.

### 2.1 Description

1. The user opens the Nom Nom Now app and navigates to a category detail view.  
2. The frontend displays existing recipes and the option to create a new one.  
3. The user clicks **“Create New Recipe”**.  
4. The recipe creation form is shown.  
5. The user fills in the fields (name, ingredients, category, steps).  
6. On clicking **Save**, the frontend sends a `POST /api/recipes` request with the entered data.  
7. The backend validates the input.  
   - If invalid, it returns a `400 Bad Request` response.  
   - If valid, it creates a new recipe entry in the database.  
8. The backend returns a `201 Created` response containing the new recipe data.  
9. The frontend updates the UI with the new recipe and displays a success message.

---

### 2.2 Sequence Diagram

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend (Vue.js)
    participant BE as Backend (Spring Boot)
    participant DB as Database (PostgreSQL)

    User->>FE: Open App (Dashboard loads)
    activate FE
    FE->>BE: GET /api/recipes
    activate BE
    BE->>DB: SELECT * FROM recipes
    activate DB
    DB-->>BE: Return list of recipes
    deactivate DB
    BE-->>FE: Send recipe data (JSON)
    deactivate BE
    FE-->>User: Display recipe overview
    deactivate FE

    User->>FE: Click "Create New Recipe"
    activate FE
    FE-->>User: Display recipe creation form
    deactivate FE

    User->>FE: Enter recipe data and click "Save"
    activate FE
    FE->>BE: POST /api/recipes { name, ingredients, category, steps }
    activate BE
    BE->>BE: Validate input
    BE->>DB: INSERT INTO recipes (name, ingredients, category, steps)
    activate DB
    DB-->>BE: Return new recipe ID
    deactivate DB
    BE-->>FE: 201 Created + recipe data
    deactivate BE
    FE-->>User: Show success message and updated list
    deactivate FE
