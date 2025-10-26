# Use-Case Specification: Creating a category

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to create a category of food. 

## 1.2 Mockup
![Mockup creating a category](mockups/KategorieErstellen.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the Category overview
- The user clicks on the button create
- User inputs the Category data and presses create

### Sequence Diagram

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
```

### Activity Diagram

```mermaid
flowchart LR
  subgraph UI [Frontend Vue]
    direction TB
    UC1_UI_S[Start]
    UC1_UI_A[Kategorie Uebersicht oeffnen]
    UC1_UI_B[Create klicken]
    UC1_UI_C[Formular ausfuellen]
    UC1_UI_D[Formular absenden]
    UC1_UI_E[Validierungsfehler anzeigen]
    UC1_UI_F[Erfolg anzeigen und Liste aktualisieren]
    UC1_UI_Z[Ende]
  end

  subgraph CTRL [Category Controller]
    direction TB
    UC1_CT_A[API createCategory]
    UC1_CT_B{Eingaben valide}
    UC1_CT_C[Antwort 400]
    UC1_CT_D[Antwort 409]
    UC1_CT_E[Antwort 201]
  end

  subgraph SVC [Category Service]
    direction TB
    UC1_SV_A{Kategorie existiert}
    UC1_SV_B[create dto]
  end

  subgraph REPO [Category Repository]
    direction TB
    UC1_RP_A[save entity]
  end

  subgraph DB [Postgres DB]
    direction TB
    UC1_DB_A[(table categories)]
  end

  UC1_UI_S --> UC1_UI_A --> UC1_UI_B --> UC1_UI_C --> UC1_UI_D --> UC1_CT_A
  UC1_CT_A --> UC1_CT_B
  UC1_CT_B -- Nein --> UC1_CT_C --> UC1_UI_E --> UC1_UI_C
  UC1_CT_B -- Ja --> UC1_SV_A
  UC1_SV_A -- Ja --> UC1_CT_D --> UC1_UI_C
  UC1_SV_A -- Nein --> UC1_SV_B --> UC1_RP_A --> UC1_DB_A --> UC1_CT_E --> UC1_UI_F --> UC1_UI_Z

```
## 2.2 Alternative Flows
n/a

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user startet the App
2. The user has navigated to the category overview
3. The category does not already exist
# 5. Postconditions
The Postconditions for this use case are:
1. The user wants to see the overview
2. The user wants to be able to use and see the new category

### 5.1 Save changes / Sync with server
The category should be saved in the DB
# 6. Story Points
Storie Points: 4