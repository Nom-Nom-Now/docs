# Use-Case Specification: Editing a recipe

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to edit a existing recipe. 

## 1.2 Mockup
![Mockup editing a recipe](mockups/RezeptEdit.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the overview of a category
- User right clicks a recipe to edit it
- User changes something at the recipe

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
    BE-->>FE: Send recipe list (JSON)
    FE-->>User: Display all recipes

    User->>FE: Select recipe to edit
    FE->>BE: GET /api/recipes/{id}
    BE->>DB: SELECT * FROM recipes WHERE id = {id}
    DB-->>BE: Return recipe details
    BE-->>FE: Send recipe data (JSON)
    FE-->>User: Display editable recipe form

    User->>FE: Modify recipe fields\n(name, ingredients, steps, etc.)
    User->>FE: Click "Save changes"
    FE->>BE: PUT /api/recipes/{id} { updatedRecipeData }
    BE->>BE: Validate data and process update
    BE->>DB: UPDATE recipes SET ... WHERE id = {id}
    DB-->>BE: Confirm update
    BE-->>FE: 200 OK (updated recipe)
    FE-->>User: Show success message\nand updated recipe view
```

## 2.2 Alternative Flows
n/a

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has created a recipe
3. The user has navigated to the overview of one category
4. The user has choosen to edit one of the recipes
5. The user has changed an attribute of the recipe

# 5. Postconditions
The Postconditions for this use case are:
1. The changes are saved.

### 5.1 Save changes / Sync with server
The changes are saved in the DB

# 6. Story Points
4