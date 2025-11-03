# Frontend Domain Model (UML Class Diagram)

```mermaid
classDiagram
direction TB

class CategoryModel {
  +id: string
  +name: string
  +description: string
}

class IngredientModel {
  +name: string
  +amount: string
  +unit: string
}

class StepModel {
  +order: number
  +text: string
}

class RecipeModel {
  +id: string
  +name: string
  +description: string
  +cookingTime: string
  +ingredients: IngredientModel[]
  +steps: StepModel[]
  +categoryIds: string[]
}

class DayOfWeek {
  <<enumeration>>
  MON
  TUE
  WED
  THU
  FRI
  SAT
  SUN
}

class DayAssignmentModel {
  +day: DayOfWeek
  +categoryId: string
  +recipeId: string?
}

class WeekPlanModel {
  +weekStart: string
  +assignments: DayAssignmentModel[]
}

class UserModel {
  +id: string
  +email: string
  +displayName: string
  +roles: string[]
}

class CategoryDto {
  +id: string
  +name: string
  +description: string
}

class CategoryWriteDto {
  +name: string
  +description: string
}

class IngredientDto {
  +name: string
  +amount: string
  +unit: string
}

class StepDto {
  +order: number
  +text: string
}

class RecipeDto {
  +id: string
  +name: string
  +description: string
  +cookingTime: string
  +ingredients: IngredientDto[]
  +steps: StepDto[]
  +categoryIds: string[]
}

class RecipeWriteDto {
  +name: string
  +description: string
  +cookingTime: string
  +ingredients: IngredientDto[]
  +steps: StepDto[]
  +categoryIds: string[]
}

class DayAssignmentDto {
  +day: DayOfWeek
  +categoryId: string
  +recipeId: string?
}

class WeekPlanDto {
  +weekStart: string
  +assignments: DayAssignmentDto[]
}

class RegisterDto {
  +email: string
  +password: string
  +displayName: string
}

class LoginDto {
  +email: string
  +password: string
}

class UserDto {
  +id: string
  +email: string
  +displayName: string
  +roles: string[]
}

class TokenDto {
  +accessToken: string
  +expiresAt: string
}

RecipeModel "*" -- "*" CategoryModel : categorizes
RecipeModel *-- IngredientModel
RecipeModel *-- StepModel
WeekPlanModel *-- DayAssignmentModel

RecipeDto "*" -- "*" CategoryDto
RecipeDto *-- IngredientDto
RecipeDto *-- StepDto
WeekPlanDto *-- DayAssignmentDto

```

