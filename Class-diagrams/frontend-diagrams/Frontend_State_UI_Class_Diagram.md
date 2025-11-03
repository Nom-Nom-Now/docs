# Frontend State & UI (UML Class Diagram)

```mermaid
classDiagram
direction TB

class CategoryStore {
  +categories: CategoryModel[]
  +isLoading: boolean
  +loadAll(): Promise~void~
  +getById(id: string): CategoryModel
  +create(dto: CategoryWriteDto): Promise~void~
  +update(id: string, dto: CategoryWriteDto): Promise~void~
}

class RecipeStore {
  +recipes: RecipeModel[]
  +isLoading: boolean
  +loadAll(): Promise~void~
  +getById(id: string): RecipeModel
  +create(dto: RecipeWriteDto): Promise~void~
  +update(id: string, dto: RecipeWriteDto): Promise~void~
}

class WeekPlanStore {
  +week: WeekPlanModel
  +loadWeek(weekStart: string): Promise~void~
  +setCategoryForDay(weekStart: string, day: DayOfWeek, categoryId: string): Promise~void~
  +generateWeek(weekStart: string): Promise~void~
}

class AuthStore {
  +user: UserModel
  +token: TokenDto
  +register(dto: RegisterDto): Promise~void~
  +login(credentials: LoginDto): Promise~void~
  +logout(): Promise~void~
  +isAuthenticated(): boolean
}

class CategoryListView
class CategoryDetailView
class CategoryForm
class RecipeListView
class RecipeDetailView
class RecipeForm
class WeekOverviewView
class WeekPlanBoard
class Navbar

CategoryStore --> CategoryService
RecipeStore --> RecipeService
WeekPlanStore --> WeekPlanService
AuthStore --> AuthService

CategoryListView --> CategoryStore
CategoryDetailView --> CategoryStore
CategoryDetailView --> RecipeStore
CategoryForm --> CategoryStore

RecipeListView --> RecipeStore
RecipeDetailView --> RecipeStore
RecipeForm --> RecipeStore

WeekOverviewView --> WeekPlanStore
WeekOverviewView --> CategoryStore
WeekPlanBoard --> WeekPlanStore
Navbar --> AuthStore
```

