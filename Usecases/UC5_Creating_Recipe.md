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

### Activity Diagram
```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend (Vue.js)
    participant BE as Backend (Spring Boot)
    participant DB as Database (PostgreSQL)

    User->>FE: Open App (Dashboard loads)
    FE->>BE: GET /api/recipes
    BE->>DB: SELECT * FROM recipes
    DB-->>BE: Return list of recipes
    BE-->>FE: Send recipe data (JSON)
    FE-->>User: Display recipe overview

    User->>FE: Click "Create New Recipe"
    FE-->>User: Display recipe creation form

    User->>FE: Enter recipe data\n(name, ingredients, category, steps)
    User->>FE: Click "Save Recipe"
    FE->>BE: POST /api/recipes { name, ingredients, category, steps }
    BE->>BE: Validate input data
    BE->>DB: INSERT INTO recipes (name, ingredients, category, steps)
    DB-->>BE: Confirmation (new recipe ID)
    BE-->>FE: 201 Created + recipe data
    FE-->>User: Show success message\nand updated recipe list
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