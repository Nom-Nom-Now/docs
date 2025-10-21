# Use-Case Specification: Choose a category per day

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to see an overview of the week and choose a category for each day.

## 1.2 Mockup
![Mockup week overview](mockups/Übersicht.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the overview
- User selects the categories for the week

### Activity Diagram
```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend (Vue.js)
    participant BE as Backend (Spring Boot)
    participant DB as Database (PostgreSQL)

    User->>FE: Open App (Dashboard loads)
    FE->>BE: GET /api/weekplan
    BE->>DB: SELECT * FROM weekplan
    DB-->>BE: Return current weekplan
    BE-->>FE: Send weekplan data (JSON)
    FE-->>User: Display week overview\n(Monday–Sunday with categories)

    User->>FE: Select a day to assign a category
    FE->>BE: GET /api/categories
    BE->>DB: SELECT * FROM categories
    DB-->>BE: Return list of categories
    BE-->>FE: Send category list (JSON)
    FE-->>User: Display category selection menu

    User->>FE: Choose category for selected day
    FE->>BE: PUT /api/weekplan/{day} { categoryId }
    BE->>DB: UPDATE weekplan SET category_id = {categoryId} WHERE day = {day}
    DB-->>BE: Confirm update
    BE-->>FE: 200 OK (updated weekplan)
    FE-->>User: Show confirmation and updated week overview
```

## 2.2 Alternative Flows
n/a

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has navigated to the overview of a week
3. At least one category with one recipe was created

# 5. Postconditions
The Postconditions for this use case are:
1. The user has selected his categories

### 5.1 Save changes / Sync with server
n/a

# 6. Story Points
4