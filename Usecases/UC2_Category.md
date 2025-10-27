# Use-Case Specification: Category

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to inspect a created category. 

## 1.2 Mockup
![Mockup viewing a category](mockups/Kategorie.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the dashboard
- User navigates to the Category overview
- User navigates to category
- User sees all recipes of category

### Sequence Diagram
```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend (Vue.js)
    participant BE as Backend (Spring Boot)
    participant DB as Database (PostgreSQL)

    User->>FE: Open App (Dashboard loads)
    activate FE
    FE->>BE: GET /api/categories
    activate BE
    BE->>DB: SELECT * FROM categories
    activate DB
    DB-->>BE: Return list of categories
    deactivate DB
    BE-->>FE: Send category data (JSON)
    deactivate BE
    FE-->>User: Display list of categories
    deactivate FE

    User->>FE: Click on a specific category
    activate FE
    FE->>BE: GET /api/categories/{id}
    activate BE
    BE->>DB: SELECT * FROM categories WHERE id = {id}
    activate DB
    DB-->>BE: Return category details
    deactivate DB
    BE-->>FE: Send category details (JSON)
    deactivate BE
    FE-->>User: Show category details (name, recipes, etc.)
    deactivate FE
```
### Activity Diagramm
```mermaid

flowchart LR
  subgraph UI [Frontend Vue]
    direction TB
    UC2_UI_S[Start]
    UC2_UI_A[Dashboard oeffnen]
    UC2_UI_B[Kategorien Uebersicht oeffnen]
    UC2_UI_C[Kategorie auswaehlen]
    UC2_UI_D[Details View oeffnen]
    UC2_UI_E[Fehler anzeigen Not Found]
    UC2_UI_F[Details anzeigen]
    UC2_UI_Z[Ende]
  end

  subgraph CTRL [Category Controller]
    direction TB
    UC2_CT_A[API getCategoryById]
    UC2_CT_B{Gefunden}
    UC2_CT_C[Antwort 404]
    UC2_CT_D[Antwort 200]
  end

  subgraph SVC [Category Service]
    direction TB
    UC2_SV_A[findById]
  end

  subgraph REPO [Category Repository]
    direction TB
    UC2_RP_A[findById]
  end

  subgraph DB [Postgres DB]
    direction TB
    UC2_DB_A[(table categories)]
  end

  UC2_UI_S --> UC2_UI_A --> UC2_UI_B --> UC2_UI_C --> UC2_UI_D --> UC2_CT_A
  UC2_CT_A --> UC2_SV_A --> UC2_RP_A --> UC2_DB_A --> UC2_CT_B
  UC2_CT_B -- Nein --> UC2_CT_C --> UC2_UI_E --> UC2_UI_Z
  UC2_CT_B -- Ja  --> UC2_CT_D --> UC2_UI_F --> UC2_UI_Z


```
## 2.2 Alternative Flows
n/a

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has navigated to the dashboard
3. The user has navigated to the category

# 5. Postconditions
n/a

### 5.1 Save changes / Sync with server
n/a
# 6. Story Points
3