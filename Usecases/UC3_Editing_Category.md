# Use-Case Specification: Editing a category

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to edit a category. 

## 1.2 Mockup
![Mockup editing a category](mockups/KategorienEdit.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the category overview
- User navigates to a single category overview
- User clicks on edit category
- User changes something at the category

### Activity Diagram
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
    DB-->>BE: Return category list
    deactivate DB
    BE-->>FE: Send category list (JSON)
    deactivate BE
    FE-->>User: Display categories on dashboard
    deactivate FE

    User->>FE: Select a category to edit
    activate FE
    FE->>BE: GET /api/categories/{id}
    activate BE
    BE->>DB: SELECT * FROM categories WHERE id = {id}
    activate DB
    DB-->>BE: Return category details
    deactivate DB
    BE-->>FE: Send category details (JSON)
    deactivate BE
    FE-->>User: Show editable form with current data
    deactivate FE

    User->>FE: Modify category fields (name, description, etc.)
    User->>FE: Click "Save changes"
    activate FE
    FE->>BE: PUT /api/categories/{id} { updatedData }
    activate BE
    BE->>DB: UPDATE categories SET ... WHERE id = {id}
    activate DB
    DB-->>BE: Update confirmation
    deactivate DB
    BE-->>FE: Return success response (200 OK)
    deactivate BE
    FE-->>User: Show success message and updated category view
    deactivate FE
```

## 2.2 Alternative Flows
n/a

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has navigated to the category overview of a single category
3. The user has clicked on the "edit category" button

# 5. Postconditions
The Postconditions for this use case are:
1. The changes are saved.

### 5.1 Save changes / Sync with server
The changes have to be saved in the DB.

# 6. Story Points
4