# Frontend Infrastructure (UML Class Diagram)

```mermaid
classDiagram
direction TB

class HttpClient {
  <<interface>>
  +get(url: string, params?: any): Promise~any~
  +post(url: string, body: any): Promise~any~
  +put(url: string, body: any): Promise~any~
  +del(url: string): Promise~any~
}

class FetchHttpClientImpl {
  -baseUrl: string
  +withAuth(tokenProvider): HttpClient
}

class CategoryApi {
  <<interface>>
  +list(): Promise~CategoryDto[]~
  +get(id: string): Promise~CategoryDto~
  +create(dto: CategoryCreateDto): Promise~CategoryDto~
  +update(id: string, dto: CategoryUpdateDto): Promise~CategoryDto~
}

class CategoryApiImpl {
  +list(): Promise~CategoryDto[]~
  +get(id: string): Promise~CategoryDto~
  +create(dto: CategoryCreateDto): Promise~CategoryDto~
  +update(id: string, dto: CategoryUpdateDto): Promise~CategoryDto~
}

class RecipeApi {
  <<interface>>
  +list(): Promise~RecipeDto[]~
  +get(id: string): Promise~RecipeDto~
  +create(dto: RecipeCreateDto): Promise~RecipeDto~
  +update(id: string, dto: RecipeUpdateDto): Promise~RecipeDto~
}

class RecipeApiImpl {
  +list(): Promise~RecipeDto[]~
  +get(id: string): Promise~RecipeDto~
  +create(dto: RecipeCreateDto): Promise~RecipeDto~
  +update(id: string, dto: RecipeUpdateDto): Promise~RecipeDto~
}

class WeekPlanApi {
  <<interface>>
  +getCurrent(): Promise~WeekPlanDto~
  +setCategoryForDay(day: DayOfWeek, categoryId: string): Promise~WeekPlanDto~
  +generateWeek(): Promise~WeekPlanDto~
}

class WeekPlanApiImpl {
  +getCurrent(): Promise~WeekPlanDto~
  +setCategoryForDay(day: DayOfWeek, categoryId: string): Promise~WeekPlanDto~
  +generateWeek(): Promise~WeekPlanDto~
}

class AuthApi {
  <<interface>>
  +register(dto: RegisterDto): Promise~UserDto~
  +login(credentials: LoginDto): Promise~TokenDto~
  +logout(): Promise~void~
}

class AuthApiImpl {
  +register(dto: RegisterDto): Promise~UserDto~
  +login(credentials: LoginDto): Promise~TokenDto~
  +logout(): Promise~void~
}

FetchHttpClientImpl --> HttpClient

CategoryApiImpl --> HttpClient
RecipeApiImpl --> HttpClient
WeekPlanApiImpl --> HttpClient
AuthApiImpl --> HttpClient

CategoryApiImpl --> CategoryApi
RecipeApiImpl --> RecipeApi
WeekPlanApiImpl --> WeekPlanApi
AuthApiImpl --> AuthApi
```

