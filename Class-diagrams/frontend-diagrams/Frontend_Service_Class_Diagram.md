# Frontend Services & Mapping (UML Class Diagram)

```mermaid
classDiagram
direction TB

class CategoryService {
  <<interface>>
  +loadAll(): Promise~CategoryModel[]~
  +getById(id: string): Promise~CategoryModel~
  +create(dto: CategoryCreateDto): Promise~CategoryModel~
  +update(id: string, dto: CategoryUpdateDto): Promise~CategoryModel~
}

class CategoryServiceImpl {
  +loadAll(): Promise~CategoryModel[]~
  +getById(id: string): Promise~CategoryModel~
  +create(dto: CategoryCreateDto): Promise~CategoryModel~
  +update(id: string, dto: CategoryUpdateDto): Promise~CategoryModel~
}

class RecipeService {
  <<interface>>
  +loadAll(): Promise~RecipeModel[]~
  +getById(id: string): Promise~RecipeModel~
  +create(dto: RecipeCreateDto): Promise~RecipeModel~
  +update(id: string, dto: RecipeUpdateDto): Promise~RecipeModel~
}

class RecipeServiceImpl {
  +loadAll(): Promise~RecipeModel[]~
  +getById(id: string): Promise~RecipeModel~
  +create(dto: RecipeCreateDto): Promise~RecipeModel~
  +update(id: string, dto: RecipeUpdateDto): Promise~RecipeModel~
}

class WeekPlanService {
  <<interface>>
  +loadCurrentWeek(): Promise~WeekPlanModel~
  +setCategoryForDay(day: DayOfWeek, categoryId: string): Promise~WeekPlanModel~
  +generateWeek(): Promise~WeekPlanModel~
}

class WeekPlanServiceImpl {
  +loadCurrentWeek(): Promise~WeekPlanModel~
  +setCategoryForDay(day: DayOfWeek, categoryId: string): Promise~WeekPlanModel~
  +generateWeek(): Promise~WeekPlanModel~
}

class AuthService {
  <<interface>>
  +register(dto: RegisterDto): Promise~UserModel~
  +login(credentials: LoginDto): Promise~TokenDto~
  +logout(): Promise~void~
}

class AuthServiceImpl {
  +register(dto: RegisterDto): Promise~UserModel~
  +login(credentials: LoginDto): Promise~TokenDto~
  +logout(): Promise~void~
}

class CategoryValidator {
  +validateCreate(dto: CategoryCreateDto)
  +validateUpdate(dto: CategoryUpdateDto)
}

class RecipeValidator {
  +validateCreate(dto: RecipeCreateDto)
  +validateUpdate(dto: RecipeUpdateDto)
}

class CategoryMapper {
  +toModel(dto: CategoryDto): CategoryModel
  +toDto(model: CategoryModel): CategoryDto
}

class RecipeMapper {
  +toModel(dto: RecipeDto): RecipeModel
  +toDto(model: RecipeModel): RecipeDto
}

class WeekPlanMapper {
  +toModel(dto: WeekPlanDto): WeekPlanModel
  +toDto(model: WeekPlanModel): WeekPlanDto
}

CategoryServiceImpl --> CategoryService
RecipeServiceImpl --> RecipeService
WeekPlanServiceImpl --> WeekPlanService
AuthServiceImpl --> AuthService

CategoryServiceImpl --> CategoryApi
RecipeServiceImpl --> RecipeApi
WeekPlanServiceImpl --> WeekPlanApi
AuthServiceImpl --> AuthApi

CategoryServiceImpl --> CategoryValidator
CategoryServiceImpl --> CategoryMapper
RecipeServiceImpl --> RecipeValidator
RecipeServiceImpl --> RecipeMapper
WeekPlanServiceImpl --> WeekPlanMapper
```
