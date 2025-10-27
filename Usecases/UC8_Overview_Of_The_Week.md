# Use-Case Specification: Overview of the week

# 1. Getting an overview

## 1.1 Brief Description
This use case allows a user to see an overview of the week and the recipes for each day. 

## 1.2 Mockup
![Mockup finished week overview](mockups/Übersicht%202.png)

# 2. Flow of Events

## 2.1 Basic Flow
- User navigates to the week overview
- User selects the categories for every day of the week
- User generates the recipes for the week

### Sequence Diagram
```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend (Vue.js)
    participant BE as Backend (Spring Boot)
    participant DB as Database (PostgreSQL)

    User->>FE: Open App (Dashboard loads)
    activate FE
    FE->>BE: GET /api/weekplan
    activate BE
    BE->>DB: SELECT * FROM weekplan
    activate DB
    DB-->>BE: Return weekplan with assigned categories
    deactivate DB
    BE-->>FE: Send weekplan data (JSON)
    deactivate BE
    FE-->>User: Display week overview (Mon–Sun with categories)

    loop For each day in week
        FE->>BE: GET /api/recipes?categoryId={categoryId}
        activate BE
        BE->>DB: SELECT * FROM recipes WHERE category_id = {categoryId}
        activate DB
        DB-->>BE: Return recipes for category
        deactivate DB
        BE-->>FE: Send recipe data (JSON)
        deactivate BE
        FE-->>User: Display recipes for that day
    end
    deactivate FE
```
### Activity Diagram
```mermaid
flowchart LR
  subgraph UI [Frontend Vue]
    direction TB
    UC8_UI_S[Start]
    UC8_UI_A[Wochenuebersicht oeffnen]
    UC8_UI_B[Weekplan laden und anzeigen]
    UC8_UI_C{Generate Recipes geklickt}
    UC8_UI_D[Rezepte der Woche anzeigen und Shopping List anbieten]
    UC8_UI_Z[Ende]
  end

  subgraph CTRL [WeekPlan Controller]
    direction TB
    UC8_CT_A[API getWeek]
    UC8_CT_B[API generateWeekRecipes]
    UC8_CT_C[Antwort 200]
  end

  subgraph SVC [WeekPlan Service]
    direction TB
    UC8_SV_A[getWeek]
    UC8_SV_B[generateRecipesForWeek]
  end

  subgraph REPO [WeekPlan Repository]
    direction TB
    UC8_RP_A[findByStart]
    UC8_RP_B[save week with recipes]
  end

  subgraph DB [Postgres DB]
    direction TB
    UC8_DB_A[(table week_plans)]
  end

  UC8_UI_S --> UC8_UI_A --> UC8_CT_A --> UC8_SV_A --> UC8_RP_A --> UC8_DB_A --> UC8_UI_B --> UC8_UI_C
  UC8_UI_C -- Nein --> UC8_UI_Z
  UC8_UI_C -- Ja   --> UC8_CT_B --> UC8_SV_B --> UC8_RP_B --> UC8_DB_A --> UC8_CT_C --> UC8_UI_D --> UC8_UI_Z

```

## 2.2 Alternative Flows
- User navigates to the week overview
- User selects the categories for every day of the week
- User generates the recipes for the week
- User regenerates the recipes for the week

# 3. Special Requirements
n/a

# 4. Preconditions
The Preconditions for this use case are:
1. The user has started the App
2. The user has navigated to the overview of a week
3. At least one category with one recipe was created

# 5. Postconditions
The Postconditions for this use case are:
1. The user wants to see the recipes of the weeks
2. The user wants to be able to generate recipes for the week
3. The user wants to be able to get a shopping list

### 5.1 Save changes / Sync with server
The generated recipes for a week should be saved in the DB.

# 6. Story Points
10