# Test Plan

**Project:** Nom Nom Now  
**Document type:** Iteration Test Plan (RUP-oriented)  
**Version:** 1.0  
**Status:** Initial version  
**Repository scope:** [`nom-nom-now-frontend`](https://github.com/Nom-Nom-Now/nom-nom-now-frontend), [`nom-nom-now-backend`](https://github.com/Nom-Nom-Now/nom-nom-now-backend), [`docs`](https://github.com/Nom-Nom-Now/docs)

## Revision History

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 28/04/2026 | 1.0 | Initial test plan for the current Nom Nom Now frontend and backend iteration | Nom Nom Now Team |
| 05/05/2026 | 1.1 | Extended test plan: added 25 concrete test cases (TC-001 to TC-025), test schedule, defect severity classification, PUT /recipes/{id} contract | Nom Nom Now Team |

## Table of Contents

- [1. Introduction](#1-introduction)
  - [1.1 Purpose](#11-purpose)
  - [1.2 Scope](#12-scope)
  - [1.3 Document Terminology and Acronyms](#13-document-terminology-and-acronyms)
  - [1.4 References](#14-references)
- [2. Evaluation Mission and Test Motivation](#2-evaluation-mission-and-test-motivation)
  - [2.1 Evaluation Mission](#21-evaluation-mission)
  - [2.2 Test Motivators](#22-test-motivators)
- [3. Target Test Items](#3-target-test-items)
- [4. Outline of Planned Tests](#4-outline-of-planned-tests)
  - [4.1 Outline of Test Inclusions](#41-outline-of-test-inclusions)
  - [4.2 Outline of Test Exclusions](#42-outline-of-test-exclusions)
- [5. Test Approach](#5-test-approach)
  - [5.1 Testing Techniques and Types](#51-testing-techniques-and-types)
  - [5.2 Test Case Priorities](#52-test-case-priorities)
  - [5.3 Requirements Traceability](#53-requirements-traceability)
  - [5.4 Total Test Coverage](#54-total-test-coverage)
- [6. Deliverables](#6-deliverables)
- [7. Testing Workflow](#7-testing-workflow)
- [8. Environmental Needs](#8-environmental-needs)
- [9. Risks, Dependencies, Assumptions, and Constraints](#9-risks-dependencies-assumptions-and-constraints)
- [10. Entry and Exit Criteria](#10-entry-and-exit-criteria)

## 1. Introduction

### 1.1 Purpose

The purpose of this Iteration Test Plan is to collect the information needed to plan, execute, and control testing for the Nom Nom Now application. It identifies the relevant frontend and backend test items, defines the testing approach, and lists the tools and environments required for quality assurance.

This test plan supports the following objectives:

- Identify the application parts that should be targeted by tests.
- Define the motivation and risk areas behind the selected tests.
- Outline the test strategy for frontend, backend, and integration behavior.
- Provide a basis for future automated test implementation and CI/CD checks.

### 1.2 Scope

Nom Nom Now is a web-based meal planning application. The current implementation consists of a Vue/Vite/TypeScript frontend and a Spring Boot backend with PostgreSQL persistence.

This test plan covers tests for:

- Frontend routing, layout, localization, forms, and user interaction flows.
- Backend controllers, services, DTO validation, security rules, and persistence behavior.
- Integration between frontend and backend REST APIs.
- CI/CD quality gates, including type checking, linting, unit tests, build checks, and backend verification.

The main functional areas considered are:

- User login via Google OAuth.
- Viewing and searching recipes.
- Creating recipes with ingredients, quantities, units, preparation text, images/categories where implemented.
- Loading available recipe categories.
- Deleting recipes with ownership checks.
- Planning a week and generating recipe suggestions/shopping lists once implemented.

Not covered in detail by this iteration are full-scale performance tests, penetration tests, formal usability studies, and production monitoring.

### 1.3 Document Terminology and Acronyms

| Abbreviation | Explanation |
|--------------|-------------|
| API | Application Programming Interface |
| CI | Continuous Integration |
| CD | Continuous Delivery/Deployment |
| DB | Database |
| DTO | Data Transfer Object |
| E2E | End-to-End |
| FE | Frontend |
| BE | Backend |
| RUP | Rational Unified Process |
| SRS | Software Requirements Specification |
| UI | User Interface |
| UC | Use Case |

### 1.4 References

- [Software Requirements Specification](SRS/SoftwareRequirementsSpecification.md)
- [Use Case Specifications](Usecases/)
- [Use Case Realizations](Use-Case-Realization%20Specification/)
- [Architecture Significant Requirements](ASR/asr-overview.md)
- [Frontend repository](https://github.com/Nom-Nom-Now/nom-nom-now-frontend)
- [Backend repository](https://github.com/Nom-Nom-Now/nom-nom-now-backend)
- [RUP Test Plan template](https://files.defcon.no/RUP/webtmpl/templates/test/rup_tstpln_mstr.htm)

## 2. Evaluation Mission and Test Motivation

### 2.1 Evaluation Mission

The evaluation mission is to detect critical defects early and to build confidence that Nom Nom Now behaves correctly for the most important user flows. Testing should especially protect the interaction between the Vue frontend, the Spring Boot REST API, and the PostgreSQL database.

The test effort focuses on:

- Correct handling of user input and validation errors.
- Correct API behavior for recipes, categories, authentication, and user-specific actions.
- Stable rendering and navigation in the frontend.
- Preventing regressions during continuous development.
- Verifying that implemented use cases remain aligned with the SRS and use-case documentation.

### 2.2 Test Motivators

1. **Functional correctness**  
   Users must be able to sign in, create recipes, view recipes, browse categories, and later plan their week without data loss or inconsistent UI behavior.

2. **Technical integration risks**  
   The frontend and backend use separate repositories and communicate through REST/JSON. API contract mismatches must be detected quickly.

3. **Security and ownership risks**  
   Recipes are user-related data. Protected operations such as recipe creation and deletion must require authentication and must not allow users to modify other users' data.

4. **Validation and data quality**  
   Recipe data contains nested components, quantities, units, categories, and optional fields. Invalid payloads must be rejected or handled gracefully.

5. **Continuous delivery risk**  
   Both repositories contain GitHub Actions workflows. Automated checks must prevent broken builds from being deployed.

## 3. Target Test Items

The following software and supporting items are targets for testing:

### Frontend

- Vue 3 + Vite + TypeScript single-page application.
- Vue Router routes:
  - `/` login page.
  - `/home` home page.
  - `/plan` weekly plan page.
  - `/recipes` recipe list.
  - `/recipes/create` new recipe flow.
  - `/recipes/:id/edit` edit existing recipe.
  - `/recipes/oldcreate` legacy recipe form.
  - `/browse/listall` recipe browsing/list page.
- Components:
  - Navigation shell and title frame.
  - Recipe creation progress flow.
  - Recipe list/search cards.
  - Material Web input and button components.
- Services:
  - `recipeService.ts` for creating recipes and categories.
  - Category API mapping in the recipe creation feature.
- Localization:
  - German and English messages in `src/locales`.
- Build and quality tooling:
  - `npm run type-check`
  - `npm run lint`
  - `npm run test:unit`
  - `npm run build`

### Backend

- Spring Boot application.
- Controllers:
  - `RecipeController` (`POST /recipes`, `GET /recipes`, `PUT /recipes/{id}`, `DELETE /recipes/{id}`)
  - `CategoryController` (`GET /categories`)
  - `AuthController` (`GET /auth/me`)
- Services:
  - `RecipeService`
  - `CategoryService`
  - `CurrentUserService`
  - `AppUserService`
- Persistence:
  - Spring Data JPA repositories for recipes, recipe components, ingredients, and users.
  - PostgreSQL database, including Testcontainers-based integration testing.
- Security:
  - Google OAuth login.
  - CORS configuration.
  - Public GET endpoints for recipes/categories.
  - Authenticated write/delete operations.
- Build and quality tooling:
  - `./mvnw verify`
  - GitHub Actions backend CI and CD workflows.

### Documentation and CI/CD

- Documentation repository structure and links.
- GitHub Actions frontend CI/CD workflow.
- GitHub Actions backend CI/CD workflow.
- Docker Compose deployment files.

## 4. Outline of Planned Tests

### 4.1 Outline of Test Inclusions

**Frontend tests** include:

- Component tests for rendering major views and navigation elements.
- Unit tests for frontend services and mapping logic.
- Form validation tests for recipe creation.
- Localization tests for route titles and key UI labels.
- Integration-style tests using mocked `fetch` calls for backend API communication.
- Build, type-check, and lint checks in CI.

**Backend tests** include:

- Unit tests for service logic, mapping, and validation behavior.
- Controller tests for HTTP status codes and payload handling.
- Integration tests with PostgreSQL via Testcontainers.
- Security tests for authenticated and unauthenticated requests.
- Repository tests for recipe, ingredient, and ownership behavior.
- Maven verification in CI.

**Frontend-backend integration tests** include:

- Contract tests for request/response payloads.
- Smoke tests for the main user journey: login, view recipes, create recipe, view created recipe.
- API compatibility tests for category and recipe structures.

### 4.2 Detailed Test Case Specifications

The following table lists concrete test cases for the current iteration. Each test case includes an identifier, description, preconditions, steps, and expected results.

#### 4.2.1 Authentication Tests

| Test Case ID | Description | Priority | Preconditions | Steps | Expected Result |
|-------------|-------------|----------|---------------|-------|-----------------|
| TC-001 | Login page renders Google sign-in button | High | App loaded, user not authenticated | 1. Navigate to `/` | Login page with Google sign-in button visible |
| TC-002 | OAuth redirect works | High | Backend running | 1. Click Google sign-in button | Browser redirects to Google OAuth endpoint |
| TC-003 | `/auth/me` returns user info when authenticated | High | User logged in | 1. Call `GET /auth/me` | 200 OK with user id, name, email |
| TC-004 | `/auth/me` rejects unauthenticated requests | High | User not logged in | 1. Call `GET /auth/me` without token | 401 Unauthorized or redirect to login |

#### 4.2.2 Recipe CRUD Tests

| Test Case ID | Description | Priority | Preconditions | Steps | Expected Result |
|-------------|-------------|----------|---------------|-------|-----------------|
| TC-005 | Create recipe with valid data | High | User authenticated | 1. `POST /recipes` with name, instructions, cookingTime, categoryIds, components | 201 Created with recipe id |
| TC-006 | Create recipe rejects empty name | High | User authenticated | 1. `POST /recipes` with empty name | 400 Bad Request |
| TC-007 | Create recipe rejects missing ingredients | High | User authenticated | 1. `POST /recipes` with empty components array | 400 Bad Request or validation error |
| TC-008 | List recipes returns paginated results | High | At least 1 recipe exists | 1. `GET /recipes?page=0&size=20` | 200 OK with paginated list, total count |
| TC-009 | List recipes respects max page size | Medium | Backend running | 1. `GET /recipes?page=0&size=100` | Response limited to max configured size (50) |
| TC-010 | Delete recipe as owner | High | User owns the recipe | 1. `DELETE /recipes/{id}` | 204 No Content, recipe removed |
| TC-011 | Delete recipe rejected for non-owner | High | User does not own the recipe | 1. `DELETE /recipes/{id}` | 403 Forbidden |
| TC-012 | Update recipe metadata | High | User owns the recipe | 1. `PUT /recipes/{id}` with updated name, instructions, cookingTime | 200 OK with updated recipe |
| TC-013 | Update recipe components | High | User owns the recipe | 1. `PUT /recipes/{id}` with modified components | 200 OK, components updated |
| TC-014 | Update recipe rejected for non-owner | High | User does not own the recipe | 1. `PUT /recipes/{id}` | 403 Forbidden |
| TC-015 | Update recipe with invalid data | Medium | User owns the recipe | 1. `PUT /recipes/{id}` with empty name | 400 Bad Request |

#### 4.2.3 Category Tests

| Test Case ID | Description | Priority | Preconditions | Steps | Expected Result |
|-------------|-------------|----------|---------------|-------|-----------------|
| TC-016 | List all categories | High | Backend running | 1. `GET /categories` | 200 OK with super categories and categories |
| TC-017 | Category response structure matches frontend expectations | Medium | Backend running | 1. `GET /categories` | Response has `superCategories[]` and `categories[]` with required fields |

#### 4.2.4 Frontend Unit Tests

| Test Case ID | Description | Priority | Preconditions | Steps | Expected Result |
|-------------|-------------|----------|---------------|-------|-----------------|
| TC-018 | categoryMapper maps API response correctly | Medium | Test environment | 1. Call `mapCategoriesResponseToLists` with fixture data | Returns correctly mapped lists |
| TC-019 | createRecipeService sends correct payload | High | Test environment | 1. Mock fetch, call `createRecipe` | Correct POST body sent (name trimmed, components filtered) |
| TC-020 | useCreateRecipeStore manages state correctly | Medium | Test environment | 1. Create store, call actions | State updates correctly, validation works |
| TC-021 | Recipe list store loads recipes | Medium | Test environment | 1. Mock fetch, call load action | Recipes stored, pagination state updated |
| TC-022 | Navigation renders correct routes | Medium | Test environment | 1. Mount app with router | All defined routes accessible |

#### 4.2.5 Edge Case Tests

| Test Case ID | Description | Priority | Preconditions | Steps | Expected Result |
|-------------|-------------|----------|---------------|-------|-----------------|
| TC-023 | Recipe name with special characters | Medium | User authenticated | 1. Create recipe with name `Über Suppe & Co. (wöchentlich)` | Recipe created successfully |
| TC-024 | Maximum field length handling | Low | User authenticated | 1. Create recipe with 500+ char name | Either truncated or rejected with clear error |
| TC-025 | Concurrent recipe creation | Low | Two users authenticated | 1. Both create recipes simultaneously | Both succeed independently |

### 4.3 Outline of Test Exclusions

The following test types are not planned for the current iteration because of time and resource constraints:

- High-volume load tests and stress tests.
- Formal penetration testing.
- Browser compatibility matrix across all possible browsers and devices.
- Formal accessibility audit with external tooling.
- Long-running production reliability monitoring.
- Manual user studies with measured usability metrics.

## 5. Test Approach

### 5.1 Testing Techniques and Types

#### 5.1.1 Unit Testing

Unit testing verifies that isolated parts of the source code behave as expected.

| | Description |
|---|---|
| Technique Objective | Verify individual functions, components, services, mappers, validators, and domain logic. |
| Technique | Use Vitest for frontend units and JUnit/Mockito for backend units. Mock external dependencies such as `fetch`, repositories, and current-user providers. |
| Oracles | Expected return values, rendered DOM state, thrown validation errors, and expected method calls. |
| Required Tools | Vitest, Vue Test Utils, JUnit 5, Mockito, Maven, npm. |
| Success Criteria | All unit tests pass locally and in CI. |
| Special Considerations | Frontend service tests should explicitly verify JSON payload shape to prevent API contract drift. |

Planned unit test examples:

- `recipeService.ts` trims recipe and ingredient input before sending API requests.
- `recipeService.ts` rejects responses without an `id`.
- Recipe creation form rejects missing recipe name, category, ingredients, time, or instructions.
- `CategoryService` returns all super categories and categories from the enums.
- `RecipeMapper` maps components in deterministic order.
- `RecipeService` reuses existing ingredients case-insensitively.

#### 5.1.2 Component and UI Testing

Component/UI tests verify that the application behaves correctly from a user's perspective without necessarily running the complete backend.

| | Description |
|---|---|
| Technique Objective | Verify route rendering, visible labels, form behavior, navigation, and UI state changes. |
| Technique | Render Vue components with Vue Test Utils and mocked router/i18n where needed. Use DOM assertions for visible elements and interactions. |
| Oracles | Expected pages, text labels, buttons, validation messages, and emitted actions are present. |
| Required Tools | Vitest, Vue Test Utils, jsdom, Vue Router test setup. |
| Success Criteria | Important views render without runtime errors and critical user interactions behave as specified. |
| Special Considerations | Material Web components may require jsdom-safe mocks or wrapper components in tests. |

Planned UI test examples:

- Login page shows the Google login button and redirects to `/oauth2/authorization/google` using the configured backend URL.
- Shell navigation is hidden on the login route and visible on app routes.
- Recipe list fetches recipes and filters them by search query.
- Recipe creation progress shows steps for ingredients, preparation, categories, image, and preview.
- German and English locale keys exist for the main navigation and recipe creation flow.

#### 5.1.3 Backend Integration Testing

Backend integration testing verifies the behavior of Spring Boot components together with a real database.

| | Description |
|---|---|
| Technique Objective | Verify that controllers, services, repositories, validation, transactions, and PostgreSQL persistence work together. |
| Technique | Use `@SpringBootTest`, MockMvc/WebTestClient, and Testcontainers PostgreSQL. |
| Oracles | HTTP status codes, JSON response bodies, persisted database state, and thrown exceptions. |
| Required Tools | JUnit 5, Spring Boot Test, Testcontainers, PostgreSQL Docker image, Maven. |
| Success Criteria | All integration tests pass with an isolated test database. |
| Special Considerations | Tests requiring authenticated users need security context setup or OAuth test doubles. |

Planned backend integration test examples:

- `GET /categories` returns super categories and category entries.
- `GET /recipes` returns a paginated response and respects the configured maximum page size.
- `POST /recipes` rejects invalid payloads with `400 Bad Request`.
- `POST /recipes` creates recipe, components, and ingredients for authenticated users.
- `DELETE /recipes/{id}` removes the recipe for the owner.
- `DELETE /recipes/{id}` rejects deletion attempts by non-owners.
- Orphaned ingredients are removed only when no recipe component uses them anymore.

#### 5.1.4 API Contract Testing

API contract tests ensure that frontend expectations match backend responses.

| | Description |
|---|---|
| Technique Objective | Detect mismatches between frontend request/response shapes and backend DTOs. |
| Technique | Define representative JSON fixtures for recipes, categories, and authentication responses. Verify frontend mapping and backend serialization against these fixtures. |
| Oracles | JSON field names, data types, required fields, and response structure. |
| Required Tools | Vitest, Spring Boot JSON tests, OpenAPI/Swagger documentation where available. |
| Success Criteria | Contract tests pass and any intentional API changes are reflected in both repositories. |
| Special Considerations | Current code should align category creation expectations: the frontend contains a category creation service, while the backend currently exposes category listing. This mismatch should be clarified before relying on category creation tests. |

Important API contracts:

| Endpoint | Method | Expected behavior |
|----------|--------|-------------------|
| `/categories` | GET | Returns available super categories and categories. |
| `/recipes` | GET | Returns paginated recipe data. |
| `/recipes` | POST | Creates a recipe for an authenticated user. |
| `/recipes/{id}` | DELETE | Deletes an owned recipe and returns `204 No Content`. |
| `/recipes/{id}` | PUT | Updates an owned recipe (name, instructions, cookingTime, categories, components). Returns `200 OK`. |
| `/auth/me` | GET | Returns current authenticated user information. |
| `/oauth2/authorization/google` | GET | Starts Google OAuth login flow. |

#### 5.1.5 End-to-End and Smoke Testing

End-to-end tests verify the most important user journeys in a deployed or locally composed system.

| | Description |
|---|---|
| Technique Objective | Verify that frontend, backend, database, authentication, and routing work together in a realistic setup. |
| Technique | Use Playwright or Cypress against a Docker Compose environment. Seed test data where required. |
| Oracles | Visible UI state, successful network responses, persisted data after refresh, and absence of critical console errors. |
| Required Tools | Playwright or Cypress, Docker Compose, seeded test database. |
| Success Criteria | Main smoke test suite passes before deployment. |
| Special Considerations | OAuth should be mocked or bypassed in test environments to keep E2E tests deterministic. |

Planned smoke test scenarios:

1. The application starts and renders the login page.
2. A test user can access the app shell after mocked authentication.
3. A user can open the recipe overview.
4. A user can create a recipe with ingredients and see it in the recipe list.
5. A user can search/filter existing recipes.
6. A user can open the weekly plan page once the feature is implemented.

#### 5.1.6 Build, Static Checks, and CI Testing

Static checks and build tests prevent regressions before runtime.

| | Description |
|---|---|
| Technique Objective | Ensure source code compiles, types are valid, linting passes, and production bundles/images can be built. |
| Technique | Execute repository scripts in GitHub Actions and locally before merges. |
| Oracles | Exit codes and CI job status. |
| Required Tools | GitHub Actions, npm, TypeScript, ESLint, Maven, Docker Buildx. |
| Success Criteria | CI workflows stay green on protected branches and pull requests. |
| Special Considerations | Frontend CI currently runs on `main` and `develop`; backend CI currently runs on pull requests to `main`. |

Current automated checks:

- Frontend:
  - `npm ci`
  - `npm run type-check`
  - `npm run lint`
  - `npm run test:unit`
  - `npm run build`
- Backend:
  - `./mvnw -B verify`

### 5.2 Test Case Priorities

| Priority | Meaning | Examples |
|----------|---------|----------|
| High | Must pass before merging or deploying. | Authentication, recipe creation, recipe listing, backend validation, build checks. |
| Medium | Important regression coverage, but not always blocking for early development. | Search/filter behavior, localization, category mapping, UI step navigation. |
| Low | Nice-to-have or later-stage quality checks. | Visual regression, broad browser matrix, non-critical layout details. |

### 5.5 Test Schedule

| Sprint / Phase | Period | Focus | Deliverables |
|---------------|--------|-------|-------------|
| Sprint 13 (Current) | Week 13–14 | Test plan creation, initial test infrastructure | TestPlan.md v1.0, Vitest/JUnit setup, initial tests |
| Sprint 14 | Week 15–16 | Test plan extension, new tests, Recipe Update feature | Updated TestPlan v1.1, backend+frontend tests, PUT endpoint |
| Sprint 15 | Week 17–18 | Search/filter, Recipe Detail View, E2E smoke tests | Playwright setup, search tests, detail page tests |
| Sprint 16 | Week 19–20 | Weekly plan feature, full regression suite | Plan feature tests, full CI coverage report |

Each sprint should end with:
- All high-priority test cases passing in CI.
- No critical (S1) or high (S2) open defects on protected branches.
- Test coverage meeting or exceeding the targets defined in Section 5.4.

### 5.6 Requirements Traceability

| Requirement / Use Case | Relevant test focus | Priority |
|------------------------|--------------------|----------|
| UC1 Creating Category | Category form validation, category API availability, list update after creation when backend support exists. | Medium |
| UC2 Category | Category overview/details rendering and API retrieval. | Medium |
| UC3 Editing Category | Edit form behavior and update API once implemented. | Low/Medium |
| UC4 Category Overview | `GET /categories`, category grouping, localized category labels. | High |
| UC5 Creating Recipe | Recipe form validation, `POST /recipes`, ingredient handling, persistence, UI feedback. | High |
| UC6 Editing Recipes | Edit form, `PUT /recipes/{id}` API, ownership checks, component updates. TC-012 to TC-015. | High |
| UC7 Choose A Category per Day | Weekly plan UI and plan persistence once implemented. | Medium |
| UC8 Overview Of The Week | Week overview and recipe generation once implemented. | Medium |
| Account Management/Login | OAuth redirect, current user endpoint, authenticated-only operations. | High |
| Security S1 | Ownership checks for recipe deletion and protected data. | High |
| Performance P1 | Recipe list response time and pagination limits. | Medium |

### 5.4 Total Test Coverage

The project should aim for pragmatic coverage targets that fit the course context and current implementation maturity.

Recommended targets:

- Backend line/branch coverage: at least **60%** for service and controller code.
- Frontend unit/component coverage: at least **50%** for services, forms, and critical views.
- Critical user flows: **100% smoke-test coverage** for login, recipe listing, and recipe creation once authentication test setup is available.

These thresholds can be raised later when the application stabilizes. For the current iteration, meaningful coverage of risky areas is more important than chasing high percentages on simple boilerplate code.

## 6. Deliverables

### 6.1 Test Evaluation Summaries

Each CI run should provide a clear result for build, type-checking, linting, and test execution. Failed workflows are visible in GitHub Actions and should block merging into protected branches.

### 6.2 Reporting on Test Coverage

Coverage reports should be generated by the test runners once coverage tooling is enabled:

- Frontend: Vitest coverage via `@vitest/coverage-v8`.
- Backend: JaCoCo Maven plugin.

Coverage reports should be published as CI artifacts or integrated into a code quality platform such as SonarCloud if the team decides to use it.

### 6.3 Perceived Quality Reports

Perceived quality is monitored through:

- Failed CI/CD workflows.
- Manual review findings in pull requests.
- Bug reports from team members during sprint testing.
- Visible runtime errors or broken user flows during smoke testing.

### 6.4 Incident Logs and Change Requests

Defects should be tracked as GitHub issues or Jira tickets. Each issue should include:

- A short summary.
- Steps to reproduce.
- Expected behavior.
- Actual behavior.
- Screenshots/logs where helpful.
- Severity and affected repository.

### 6.6 Defect Severity Classification

| Severity | Label | Definition | Resolution Timeline |
|----------|-------|-----------|-------------------|
| S1 | Critical | Application crashes, data loss, security breach, or complete feature failure. | Must be fixed before merge/deploy. |
| S2 | High | Major feature broken, incorrect business logic, or API returning wrong data. | Fix within current sprint. |
| S3 | Medium | Minor functional issue, UI inconsistency, or degraded performance that doesn't block usage. | Fix within next sprint or document as known issue. |
| S4 | Low | Cosmetic issues, typos, minor UX polish, or non-functional improvements. | Fix when convenient or backlog. |

Each GitHub Issue used for defect tracking should include:

- **Title** with severity prefix: `[S1]`, `[S2]`, `[S3]`, or `[S4]`
- **Steps to reproduce**
- **Expected vs. actual behavior**
- **Affected repository** (frontend / backend / docs)
- **Screenshots or logs** where applicable

### 6.7 Smoke Test Suite and Supporting Test Scripts

The smoke test suite should be executable locally and in CI. It should validate the core deployment candidate before release:

1. Start frontend, backend, and database.
2. Load the application.
3. Verify the main pages render.
4. Verify recipe API availability.
5. Verify basic recipe creation and retrieval with test data.

## 7. Testing Workflow

1. **Local development**
   - Developers run relevant unit tests before committing.
   - Frontend developers run `npm run type-check`, `npm run lint`, and `npm run test:unit`.
   - Backend developers run `./mvnw verify`.

2. **Pull request creation**
   - Changes are opened as pull requests.
   - CI checks execute automatically.
   - Reviewers check test evidence and potential API contract changes.

3. **Integration testing**
   - For changes touching both repositories, frontend/backend API compatibility is checked manually or with contract tests.
   - Docker Compose can be used for local smoke testing.

4. **Merge and deployment**
   - Main branch builds trigger CD workflows.
   - Docker images are built and pushed to GHCR.
   - Deployment workflows recreate/update the corresponding services.

5. **Post-deployment smoke check**
   - Verify that frontend loads.
   - Verify backend health/API availability.
   - Verify a minimal user journey if test credentials or mocked auth are available.

## 8. Environmental Needs

### 8.1 Base System Hardware

| Resource | Quantity | Name and Type |
|----------|:--------:|---------------|
| CI runner | 1+ | GitHub-hosted Ubuntu runner |
| Local developer machine | 1 per developer | Notebook/PC capable of Node.js, Java, Docker |
| Test database | 1 per integration test run | PostgreSQL Testcontainer |
| Deployment server | 1 | Linux server running Docker Compose |

### 8.2 Base Software Elements in the Test Environment

| Software Element Name | Type and Notes |
|-----------------------|----------------|
| Node.js | Frontend build and test runtime. CI currently uses Node.js 20. |
| npm | Frontend dependency installation and scripts. |
| Java 25 | Backend build and test runtime. |
| Maven Wrapper | Backend build and verification. |
| Docker | Required for Testcontainers and deployment builds. |
| PostgreSQL | Database for backend persistence tests and production. |
| Google OAuth configuration | Required for real authentication tests; should be mocked in automated tests. |

### 8.3 Productivity and Support Tools

| Tool Category or Type | Tool Brand Name |
|-----------------------|-----------------|
| Repository hosting | GitHub |
| CI/CD | GitHub Actions |
| Frontend testing | Vitest, Vue Test Utils, jsdom |
| Backend testing | JUnit 5, Spring Boot Test, Mockito, Testcontainers |
| E2E testing | Playwright or Cypress (recommended) |
| Build and deployment | Docker, Docker Buildx, Docker Compose, GHCR |
| IDE | IntelliJ IDEA, Visual Studio Code |
| Issue tracking | GitHub Issues or Jira |

## 9. Risks, Dependencies, Assumptions, and Constraints

| Risk | Mitigation Strategy | Contingency if Risk is Realized |
|------|---------------------|---------------------------------|
| Frontend and backend API contracts diverge. | Add contract tests and shared example payloads. Review DTO changes across both repos. | Update frontend service/backend DTO and add regression test. |
| OAuth makes automated E2E tests hard to run. | Use mocked authentication or test-only security configuration. | Limit E2E to public pages and cover authenticated behavior with backend integration tests. |
| Testcontainers require Docker availability. | Document Docker requirement and run backend CI on GitHub-hosted runners. | Use H2 or mocked repositories for a reduced test suite if Docker is unavailable. |
| Category creation is present in frontend service but not currently exposed by backend. | Clarify feature scope and align endpoints before adding full category creation tests. | Mark category creation tests pending until backend endpoint exists. |
| Low current automated test count. | Add tests around highest-risk use cases first: recipe API, validation, authentication, recipe service. | Use manual smoke tests until automation catches up. |
| Material Web components may be difficult in jsdom. | Mock web components or focus tests on wrappers and business behavior. | Cover remaining behavior with E2E tests. |
| Deployment failures due to Docker/GHCR/secrets. | Keep CI build separate from deployment and verify deployment secrets. | Roll back to previous image tag and inspect GitHub Actions logs. |

### Assumptions

- GitHub Actions is the central CI/CD tool for the project.
- PostgreSQL is the target database for integration tests and deployment.
- Frontend and backend are versioned independently but should be kept API-compatible.
- Protected branch rules should require successful CI checks before merges.
- New functionality should include tests in the repository where the behavior is implemented.

### Constraints

- The project is developed in a course context with limited time.
- Some planned features from the SRS are not fully implemented yet.
- Test coverage targets are therefore staged and should grow over future iterations.

## 10. Entry and Exit Criteria

### 10.1 Entry Criteria

Testing for a feature can begin when:

- The user story or use case is documented.
- Acceptance criteria are known.
- The relevant frontend/backend code is available on a branch.
- Required test data or mocks can be created.
- The local project can be built and started.

### 10.2 Exit Criteria

A feature is considered test-complete for the iteration when:

- All planned high-priority tests for the feature pass.
- CI build and quality checks pass.
- Known defects are fixed or documented with an accepted severity.
- API contract changes are reflected in both frontend and backend.
- The feature has been smoke-tested in an integrated environment where applicable.

### 10.3 Suspension and Resumption Criteria

Testing should be suspended if:

- The application cannot be built.
- Required dependencies such as database or authentication mocks are unavailable.
- A critical blocker prevents execution of most planned tests.

Testing resumes when:

- The blocker is resolved.
- The affected environment is stable again.
- A minimal smoke test confirms that test execution is meaningful.
