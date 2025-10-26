# Use-Case Specification: Creating a recipe

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to create a recipe. 

## 1.2 Mockup
![Mockup creating a recipe](mockups/neuesRezept.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the overview of a single category
- User clicks on the "new" button
- User fills out the create recipe form
- User clicks on create

### Sequence Diagram
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

    User->>FE: Enter recipe data (name, ingredients, category, steps)
    User->>FE: Click "Save Recipe"
    activate FE
    FE->>BE: POST /api/recipes { name, ingredients, category, steps }
    activate BE
    BE->>BE: Validate input data
    BE->>DB: INSERT INTO recipes (name, ingredients, category, steps)
    activate DB
    DB-->>BE: Confirmation (new recipe ID)
    deactivate DB
    BE-->>FE: 201 Created + recipe data
    deactivate BE
    FE-->>User: Show success message and updated recipe list
    deactivate FE
```
### Activity Diagram
```mermaid
flowchart LR
  subgraph UI [Frontend Vue]
    direction TB
    UC5_UI_S[Start]
    UC5_UI_A[Kategorie Detail oeffnen]
    UC5_UI_B[New Recipe klicken]
    UC5_UI_C[Formular ausfuellen]
    UC5_UI_D[Save klicken]
    UC5_UI_E[Validierungsfehler anzeigen]
    UC5_UI_F[Erfolg anzeigen und Liste aktualisieren]
    UC5_UI_Z[Ende]
  end

  subgraph CTRL [Recipe Controller]
    direction TB
    UC5_CT_A[API createRecipe]
    UC5_CT_B{Eingaben valide}
    UC5_CT_C[Antwort 400]
    UC5_CT_D[Antwort 201]
  end

  subgraph SVC [Recipe Service]
    direction TB
    UC5_SV_A[create dto]
  end

  subgraph REPO [Recipe Repository]
    direction TB
    UC5_RP_A[save entity]
  end

  subgraph DB [Postgres DB]
    direction TB
    UC5_DB_A[(table recipes)]
  end

  UC5_UI_S --> UC5_UI_A --> UC5_UI_B --> UC5_UI_C --> UC5_UI_D --> UC5_CT_A
  UC5_CT_A --> UC5_CT_B
  UC5_CT_B -- Nein --> UC5_CT_C --> UC5_UI_E --> UC5_UI_C
  UC5_CT_B -- Ja   --> UC5_SV_A --> UC5_RP_A --> UC5_DB_A --> UC5_CT_D --> UC5_UI_F --> UC5_UI_Z

```
## 2.2 Alternative Flows
n/a

# 3. Special Requirements
One Recipe can be connected to multiple categories.

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has created a category

# 5. Postconditions
The Postconditions for this use case are:
1. The new recipe is saved

### 5.1 Save changes / Sync with server
The new recipe is saved in the DB.

# 6. Story Points
6