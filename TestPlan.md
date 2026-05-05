# Test Plan

**Project:** Nom Nom Now  
**Document type:** Iteration Test Plan (RUP-oriented)  
**Version:** 1.2  
**Status:** Streamlined for Sprint 14 (5-hour budget)  
**Repository scope:** [`nom-nom-now-frontend`](https://github.com/Nom-Nom-Now/nom-nom-now-frontend), [`nom-nom-now-backend`](https://github.com/Nom-Nom-Now/nom-nom-now-backend), [`docs`](https://github.com/Nom-Nom-Now/docs)

## Revision History

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 28/04/2026 | 1.0 | Initial test plan for the current Nom Nom Now frontend and backend iteration | Nom Nom Now Team |
| 05/05/2026 | 1.1 | Extended test plan: added 25 concrete test cases (TC-001 to TC-025), test schedule, defect severity classification, PUT /recipes/{id} contract | Nom Nom Now Team |
| 05/05/2026 | 1.2 | Streamlined plan for Sprint 14: removed E2E, contract, and UI tests; focused on happy-path unit and integration tests for RecipeService core; aligned with actual test implementation | Nom Nom Now Team |

## Table of Contents

- [1. Introduction](#1-introduction)
- [2. Evaluation Mission and Test Motivation](#2-evaluation-mission-and-test-motivation)
- [3. Target Test Items](#3-target-test-items)
- [4. Outline of Planned Tests](#4-outline-of-planned-tests)
- [5. Test Approach](#5-test-approach)
- [6. Deliverables](#6-deliverables)
- [7. Testing Workflow](#7-testing-workflow)
- [8. Environmental Needs](#8-environmental-needs)
- [9. Risks and Constraints](#9-risks-and-constraints)
- [10. Entry and Exit Criteria](#10-entry-and-exit-criteria)

## 1. Introduction

### 1.1 Purpose

The purpose of this Iteration Test Plan is to define the testing approach for Sprint 14 of the Nom Nom Now application. It focuses on the most critical backend services and frontend data-mapping logic, within a realistic 5-hour time budget.

This test plan supports the following objectives:

- Identify the most important application parts to test first.
- Define the motivation and risk areas behind the selected tests.
- Outline the test strategy for backend unit/integration and frontend unit tests.
- Provide a basis for future automated test expansion.

### 1.2 Scope

Nom Nom Now is a web-based meal planning application. The current implementation consists of a Vue/Vite/TypeScript frontend and a Spring Boot backend with PostgreSQL persistence.

**In scope for this iteration:**

- Backend unit tests for `RecipeService` (the core business logic).
- Backend integration tests for `POST /recipes` and `GET /recipes` (happy path).
- Frontend unit tests for services and mapping logic (data preparation layer).
- CI quality gates (type-check, lint, build, `./mvnw verify`).

**Out of scope for this iteration (deferred to later sprints):**

- E2E / smoke testing (Playwright/Cypress) — requires deployed environment setup.
- API contract tests — will be added once the API stabilizes.
- Component/UI tests — require extensive mocking of Material Web components.
- Load testing, penetration testing, formal accessibility audits.

### 1.3 Document Terminology and Acronyms

| Abbreviation | Explanation |
|--------------|-------------|
| API | Application Programming Interface |
| CI | Continuous Integration |
| DTO | Data Transfer Object |
| E2E | End-to-End |
| FE | Frontend |
| BE | Backend |
| RUP | Rational Unified Process |
| SRS | Software Requirements Specification |

### 1.4 References

- [Software Requirements Specification](SRS/SoftwareRequirementsSpecification.md)
- [Use Case Specifications](Usecases/)
- [Frontend repository](https://github.com/Nom-Nom-Now/nom-nom-now-frontend)
- [Backend repository](https://github.com/Nom-Nom-Now/nom-nom-now-backend)

## 2. Evaluation Mission and Test Motivation

### 2.1 Evaluation Mission

The evaluation mission is to verify that the core recipe CRUD functionality works correctly end-to-end. The test effort focuses on the highest-risk area: `RecipeService`, which handles recipe creation, update, deletion, and ingredient management.

### 2.2 Test Motivators

1. **Functional correctness** — Users must be able to create, list, edit, and delete recipes with proper ownership checks.

2. **Data integrity** — Recipe data contains nested components with ingredients, quantities, and units. Orphaned ingredients must be cleaned up correctly.

3. **Security and ownership** — Recipe operations require authentication. Users must not be able to modify or delete other users' recipes.

4. **Regression prevention** — Automated tests in CI prevent broken builds from being merged.

## 3. Target Test Items

### Frontend

- `recipeService.ts` / `editRecipeService.ts` — data mapping and API payload construction.
- `useCreateRecipeStore.ts` — state management for recipe creation/editing.

### Backend

- `RecipeService` — core business logic (create, update, delete, ingredient management).
- `RecipeController` — `POST /recipes`, `GET /recipes`, `PUT /recipes/{id}`, `DELETE /recipes/{id}` endpoints.
- `RecipeRepository`, `IngredientRepository`, `RecipeComponentRepository` — persistence layer.

### Documentation and CI/CD

- GitHub Actions CI workflows (frontend and backend).
- `./mvnw verify` for backend verification.

## 4. Outline of Planned Tests

### 4.1 Test Inclusions

**Backend unit tests** (Mockito, no database):

- `RecipeService.create()` — recipe is saved with correct owner, components, and ingredients.
- `RecipeService.findById()` — returns recipe or throws `ResourceNotFoundException`.
- `RecipeService.findAll()` — delegates to repository with pagination.
- `RecipeService.updateRecipe()` — updates metadata and components, ownership check.
- `RecipeService.deleteRecipe()` — deletes recipe and cleans up orphaned ingredients.

**Backend integration tests** (Testcontainers, real PostgreSQL):

- `POST /recipes` — creates a recipe with valid data (happy path).
- `POST /recipes` — rejects empty name (400) and empty components (400).
- `GET /recipes` — returns paginated results.
- `GET /recipes/{id}` — returns recipe by ID.
- `PUT /recipes/{id}` — updates owned recipe (happy path).
- `DELETE /recipes/{id}` — deletes owned recipe (happy path).

**Frontend unit tests** (Vitest):

- Recipe list data mapping.
- Recipe browse data handling.
- Navigation routing.
- Localization keys.
- Recipe creation flow (store state management).

### 4.2 Test Case Specifications

#### Backend — Unit Tests

| Test Case | Description | Method Under Test |
|-----------|-------------|-------------------|
| TC-U01 | Create recipe sets owner and saves with components | `RecipeService.create()` |
| TC-U02 | Create recipe creates new ingredient when not found by name | `RecipeService.create()` |
| TC-U03 | Create recipe reuses existing ingredient by ID | `RecipeService.create()` |
| TC-U04 | FindById returns recipe or throws ResourceNotFoundException | `RecipeService.findById()` |
| TC-U05 | FindAll delegates to repository | `RecipeService.findAll()` |
| TC-U06 | UpdateRecipe updates metadata and components | `RecipeService.updateRecipe()` |
| TC-U07 | UpdateRecipe throws for non-owner | `RecipeService.updateRecipe()` |
| TC-U08 | DeleteRecipe removes recipe and cleans up orphaned ingredients | `RecipeService.deleteRecipe()` |
| TC-U09 | DeleteRecipe skips ingredient deletion if still used | `RecipeService.deleteRecipe()` |
| TC-U10 | DeleteRecipe throws for non-owner | `RecipeService.deleteRecipe()` |

#### Backend — Integration Tests

| Test Case | Description | Endpoint |
|-----------|-------------|----------|
| TC-I01 | POST /recipes returns 200 with created recipe | `POST /recipes` |
| TC-I02 | POST /recipes returns 400 when name is blank | `POST /recipes` |
| TC-I03 | POST /recipes returns 400 when components empty | `POST /recipes` |
| TC-I04 | GET /recipes returns paginated results | `GET /recipes` |
| TC-I05 | GET /recipes/{id} returns recipe | `GET /recipes/{id}` |
| TC-I06 | GET /recipes/{id} returns 404 when not found | `GET /recipes/{id}` |
| TC-I07 | PUT /recipes/{id} updates recipe | `PUT /recipes/{id}` |
| TC-I08 | PUT /recipes/{id} returns 403 for non-owner | `PUT /recipes/{id}` |
| TC-I09 | DELETE /recipes/{id} returns 204 | `DELETE /recipes/{id}` |
| TC-I10 | DELETE /recipes/{id} returns 403 for non-owner | `DELETE /recipes/{id}` |
| TC-I11 | GET /auth/me returns user info when authenticated | `GET /auth/me` |

#### Frontend — Unit Tests

| Test Case | Description | Component/Service |
|-----------|-------------|-------------------|
| TC-F01 | Recipe list renders correctly with mocked data | `recipeList.test.ts` |
| TC-F02 | Recipe browse handles API response | `recipeBrowse.test.ts` |
| TC-F03 | Navigation routes are accessible | `navigation.test.ts` |
| TC-F04 | i18n keys exist for all routes and labels | `i18n.test.ts` |
| TC-F05 | Recipe creation store manages state correctly | `createRecipeFlow.test.ts` |

### 4.3 Test Exclusions

The following test types are excluded from this iteration due to the 5-hour time budget:

| Test Type | Reason for Exclusion |
|-----------|---------------------|
| **End-to-End / Smoke Tests** | Require a running Docker Compose environment, OAuth mocking setup, and Playwright/Cypress configuration. Deferred to Sprint 15. |
| **API Contract Tests** | API is still evolving; contracts will be formalized once the API stabilizes. Deferred to Sprint 15+. |
| **Component/UI Tests** | Vue component testing requires extensive Material Web component mocking. Low ROI for current sprint. Deferred to later iterations. |
| **Load / Stress Tests** | Not relevant at current scale. |
| **Penetration Tests** | Formal security testing deferred to later iterations. |
| **Browser Compatibility Matrix** | Not feasible within current time budget. |

## 5. Test Approach

### 5.1 Testing Techniques and Types

#### 5.1.1 Unit Testing

Unit testing verifies that isolated parts of the source code behave as expected.

| | Description |
|---|---|
| **Technique Objective** | Verify individual functions, services, and business logic in isolation. |
| **Technique** | Use Vitest for frontend units and JUnit 5 + Mockito for backend units. Mock all external dependencies (repositories, user services, fetch calls). |
| **Oracles** | Expected return values, thrown exceptions, verified mock interactions. |
| **Required Tools** | Vitest, Vue Test Utils, JUnit 5, Mockito, Maven, npm. |
| **Success Criteria** | All unit tests pass locally and in CI. |

#### 5.1.2 Backend Integration Testing

Backend integration testing verifies Spring Boot components together with a real PostgreSQL database.

| | Description |
|---|---|
| **Technique Objective** | Verify that controllers, services, repositories, validation, and persistence work together. |
| **Technique** | Use `@SpringBootTest`, `@AutoConfigureMockMvc`, Testcontainers PostgreSQL, and `oauth2Login()` for security test support. |
| **Oracles** | HTTP status codes, JSON response bodies, persisted database state. |
| **Required Tools** | JUnit 5, Spring Boot Test, Spring Security Test, Testcontainers, PostgreSQL Docker image. |
| **Success Criteria** | All integration tests pass with an isolated test database per test class. |
| **Special Considerations** | Each test class cleans up database state in `@BeforeEach` to prevent data leakage between tests. |

#### 5.1.3 Build, Static Checks, and CI Testing

| | Description |
|---|---|
| **Technique Objective** | Ensure source code compiles, types are valid, linting passes, and builds succeed. |
| **Technique** | Execute repository scripts in GitHub Actions and locally before merges. |
| **Oracles** | Exit codes and CI job status. |
| **Required Tools** | GitHub Actions, npm, TypeScript, ESLint, Maven. |
| **Success Criteria** | CI workflows stay green on protected branches and pull requests. |

Current automated checks:

- Frontend: `npm ci`, `npm run type-check`, `npm run lint`, `npm run test:unit`, `npm run build`
- Backend: `./mvnw -B verify`

### 5.2 Test Case Priorities

| Priority | Meaning | Examples |
|----------|---------|----------|
| High | Must pass before merging. | Recipe CRUD happy path, ownership checks, build checks. |
| Medium | Important regression coverage. | Validation errors (400), 404 handling, pagination. |
| Low | Nice-to-have for later. | Edge cases (special characters, max length, concurrency). |

### 5.3 Test Schedule

| Sprint | Period | Focus | Deliverables |
|--------|--------|-------|-------------|
| Sprint 13 | Week 13–14 | Test plan creation, initial test infrastructure | TestPlan.md v1.0, Vitest/JUnit setup |
| **Sprint 14 (Current)** | **Week 15–16** | **Focused unit + integration tests, Recipe Update feature** | **TestPlan v1.2, RecipeService tests, Controller integration tests, PUT endpoint** |
| Sprint 15 | Week 17–18 | Search/filter, Recipe Detail View, API contract tests | Contract tests, search tests, detail page tests |
| Sprint 16 | Week 19–20 | E2E smoke tests, full regression suite | Playwright setup, full CI coverage report |

### 5.4 Requirements Traceability

| Requirement / Use Case | Relevant Test Focus | Priority |
|------------------------|--------------------|----------|
| UC5 Creating Recipe | Recipe form validation, `POST /recipes`, ingredient handling, persistence. TC-U01 to TC-U03, TC-I01 to TC-I03. | High |
| UC6 Editing Recipes | `PUT /recipes/{id}`, ownership checks, component updates. TC-U06, TC-U07, TC-I07, TC-I08. | High |
| UC4 Category Overview | `GET /categories` (covered by existing `CategoryControllerTest`). | Medium |
| Account Management | OAuth redirect, current user endpoint. TC-I11. | High |
| Security | Ownership checks for recipe deletion and updates. TC-U07, TC-U10, TC-I08, TC-I10. | High |

### 5.5 Total Test Coverage

**Focus:** Successful testing of the main services, especially `RecipeService`.

- Backend: Focus on `RecipeService` unit tests and `RecipeController` integration tests. Generic controllers, authentication boilerplate, and simple DTOs are not explicitly tested in this iteration due to time constraints.
- Frontend: Focus on service/mapper data tests. Component rendering tests are deferred.
- Coverage targets are intentionally not set as hard percentages for this iteration. The goal is meaningful coverage of high-risk business logic, not chasing numbers on simple boilerplate code.

These targets can be raised in future iterations once the application stabilizes and more testing infrastructure is in place.

## 6. Deliverables

### 6.1 Test Evaluation Summaries

Each CI run provides a clear result for build, type-checking, linting, and test execution. Failed workflows block merging into protected branches.

### 6.2 Reporting on Test Coverage

Coverage reports can be enabled when ready:

- Frontend: Vitest coverage via `@vitest/coverage-v8`.
- Backend: JaCoCo Maven plugin.

### 6.3 Defect Severity Classification

| Severity | Label | Definition | Resolution Timeline |
|----------|-------|-----------|-------------------|
| S1 | Critical | Application crashes, data loss, security breach. | Must be fixed before merge. |
| S2 | High | Major feature broken, incorrect business logic. | Fix within current sprint. |
| S3 | Medium | Minor functional issue, UI inconsistency. | Fix within next sprint. |
| S4 | Low | Cosmetic issues, typos, minor UX polish. | Fix when convenient. |

Each defect should be tracked as a GitHub Issue with:
- Title with severity prefix: `[S1]`, `[S2]`, `[S3]`, or `[S4]`
- Steps to reproduce
- Expected vs. actual behavior
- Affected repository (frontend / backend / docs)

## 7. Testing Workflow

1. **Local development** — Developers run relevant tests before committing.
2. **Pull request creation** — CI checks execute automatically.
3. **Code review** — Reviewers check test evidence and potential API changes.
4. **Merge** — Main branch builds trigger CD workflows.
5. **Post-deployment** — Verify that frontend loads and backend API is available.

## 8. Environmental Needs

### 8.1 Hardware

| Resource | Quantity | Notes |
|----------|:--------:|-------|
| CI runner | 1+ | GitHub-hosted Ubuntu runner |
| Local developer machine | 1 per developer | Node.js, Java 25, Docker |
| Test database | 1 per test class | PostgreSQL Testcontainer |

### 8.2 Software

| Software | Purpose |
|----------|---------|
| Node.js 20+ | Frontend build and test |
| Java 25 (Temurin) | Backend build and test |
| Maven Wrapper | Backend build verification |
| Docker | Testcontainers and deployment |

## 9. Risks and Constraints

| Risk | Mitigation |
|------|-----------|
| Testcontainers require Docker | GitHub-hosted runners include Docker. Local fallback: run tests only when Docker is available. |
| `oauth2Login()` in MockMvc bypasses `findOrCreate()` | Pre-create test users in `@BeforeEach` to ensure they exist in the database. |
| Recipe ownership depends on non-null owner | Null-safe ownership checks added to `RecipeService` (see commit history). |
| Time budget (5 hours) limits scope | Focus exclusively on `RecipeService` happy path and core controller endpoints. |

### Constraints

- The project is developed in a course context with limited time (5 hours for Sprint 14 testing).
- Some planned features from the SRS are not fully implemented yet.
- Test coverage targets are staged and will grow over future iterations.

## 10. Entry and Exit Criteria

### 10.1 Entry Criteria

Testing for a feature can begin when:

- The user story or use case is documented.
- The relevant frontend/backend code is available on a branch.
- The local project can be built and started.

### 10.2 Exit Criteria

A feature is considered test-complete for the iteration when:

- All planned high-priority tests pass.
- CI build and quality checks pass.
- Known defects are fixed or documented with an accepted severity.

### 10.3 Suspension and Resumption Criteria

Testing should be suspended if the application cannot be built or required dependencies (database, Docker) are unavailable. Testing resumes when the blocker is resolved and a minimal smoke test confirms that execution is meaningful.
