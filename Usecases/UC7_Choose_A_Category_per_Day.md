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
    DB-->>BE: Return current weekplan
    deactivate DB
    BE-->>FE: Send weekplan data (JSON)
    deactivate BE
    FE-->>User: Display week overview (Mon–Sun with categories)
    deactivate FE

    User->>FE: Select a day to assign a category
    activate FE
    FE->>BE: GET /api/categories
    activate BE
    BE->>DB: SELECT * FROM categories
    activate DB
    DB-->>BE: Return list of categories
    deactivate DB
    BE-->>FE: Send category list (JSON)
    deactivate BE
    FE-->>User: Display category selection menu
    deactivate FE

    User->>FE: Choose category for selected day
    activate FE
    FE->>BE: PUT /api/weekplan/{day} { categoryId }
    activate BE
    BE->>DB: UPDATE weekplan SET category_id = {categoryId} WHERE day = {day}
    activate DB
    DB-->>BE: Confirm update
    deactivate DB
    BE-->>FE: 200 OK (updated weekplan)
    deactivate BE
    FE-->>User: Show confirmation and updated week overview
    deactivate FE
```
### Activity Diagram
```mermaid
flowchart LR
  subgraph UI [Frontend Vue]
    direction TB
    UC7_UI_S[Start]
    UC7_UI_A[Wochenuebersicht oeffnen]
    UC7_UI_B[Weekplan laden]
    UC7_UI_C{Alle Tage gesetzt}
    UC7_UI_D[Tag auswaehlen]
    UC7_UI_E[Kategorie Auswahl oeffnen]
    UC7_UI_F[Kategorie waehlen]
    UC7_UI_G[Uebersicht anzeigen]
    UC7_UI_Z[Ende]
  end

  subgraph CTRL [WeekPlan Controller]
    direction TB
    UC7_CT_A[API getWeek]
    UC7_CT_B[API setCategoryForDay]
    UC7_CT_C[Antwort 200]
  end

  subgraph SVC [WeekPlan Service]
    direction TB
    UC7_SV_A[getOrCreateWeek]
    UC7_SV_B[setCategoryForDay]
  end

  subgraph REPO [WeekPlan Repository]
    direction TB
    UC7_RP_A[findByStart]
    UC7_RP_B[save week]
  end

  subgraph DB [Postgres DB]
    direction TB
    UC7_DB_A[(table week_plans)]
  end

  UC7_UI_S --> UC7_UI_A --> UC7_CT_A --> UC7_SV_A --> UC7_RP_A --> UC7_DB_A --> UC7_UI_B --> UC7_UI_C
  UC7_UI_C -- Ja   --> UC7_UI_G --> UC7_UI_Z
  UC7_UI_C -- Nein --> UC7_UI_D --> UC7_UI_E --> UC7_UI_F --> UC7_CT_B --> UC7_SV_B --> UC7_RP_B --> UC7_DB_A --> UC7_CT_C --> UC7_UI_A

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