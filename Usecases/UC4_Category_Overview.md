# Use-Case Specification: Overview of the categories

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to get an overview of all categories. 

## 1.2 Mockup
![Mockup overview of categories](mockups/Kategorien.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the category overview

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
    FE-->>User: Display list of all categories
    deactivate FE
```

### Activity Diagramm
```mermaid
flowchart LR
  subgraph UI [Frontend Vue]
    direction TB
    UC4_UI_S[Start]
    UC4_UI_A[App oeffnen]
    UC4_UI_B[Kategorien Uebersicht oeffnen]
    UC4_UI_C{Gibt es Kategorien}
    UC4_UI_D[Leerer Zustand mit Create CTA]
    UC4_UI_E[Liste aller Kategorien anzeigen]
    UC4_UI_Z[Ende]
  end

  subgraph CTRL [Category Controller]
    direction TB
    UC4_CT_A[API listCategories]
    UC4_CT_B[Antwort 200]
  end

  subgraph SVC [Category Service]
    direction TB
    UC4_SV_A[findAll]
  end

  subgraph REPO [Category Repository]
    direction TB
    UC4_RP_A[findAll]
  end

  subgraph DB [Postgres DB]
    direction TB
    UC4_DB_A[(table categories)]
  end

  UC4_UI_S --> UC4_UI_A --> UC4_UI_B --> UC4_CT_A --> UC4_SV_A --> UC4_RP_A --> UC4_DB_A --> UC4_CT_B --> UC4_UI_C
  UC4_UI_C -- Nein --> UC4_UI_D --> UC4_UI_Z
  UC4_UI_C -- Ja   --> UC4_UI_E --> UC4_UI_Z

```


## 2.2 Alternative Flows
n/a

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has navigated to the overview of categories

# 5. Postconditions
n/a

### 5.1 Save changes / Sync with server
n/a

# 6. Story Points
6