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
    FE->>BE: GET /api/categories
    BE->>DB: SELECT * FROM categories
    DB-->>BE: Return category list
    BE-->>FE: Send category list (JSON)
    FE-->>User: Display categories on dashboard

    User->>FE: Select a category to edit
    FE->>BE: GET /api/categories/{id}
    BE->>DB: SELECT * FROM categories WHERE id = {id}
    DB-->>BE: Return category details
    BE-->>FE: Send category details (JSON)
    FE-->>User: Show editable form with current data

    User->>FE: Modify category fields\n(name, description, etc.)
    User->>FE: Click "Save changes"
    FE->>BE: PUT /api/categories/{id} { updatedData }
    BE->>DB: UPDATE categories SET ... WHERE id = {id}
    DB-->>BE: Update confirmation
    BE-->>FE: Return success response (200 OK)
    FE-->>User: Show success message\nand updated category view
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