# Blogbeitrag - Weekly 14

## Weekly 14

Diese Woche stand die Erweiterung des Testplans, das Hinzufügen neuer Tests und die Weiterentwicklung des Projekts im Fokus.

## Erledigte Punkte

- **Testplan erweitert (TestPlan.md v1.1):**
  - 25 konkrete Testfall-Spezifikationen hinzugefügt (TC-001 bis TC-025) mit Beschreibung, Priorität, Voraussetzungen, Schritten und erwarteten Ergebnissen.
  - Testfälle abgedeckt: Authentication (TC-001–TC-004), Recipe CRUD inkl. Update (TC-005–TC-015), Categories (TC-016–TC-017), Frontend Unit Tests (TC-018–TC-022), Edge Cases (TC-023–TC-025).
  - Test-Schedule mit Sprint-Übersicht (Sprint 13–16) ergänzt.
  - Defect-Severity-Schema (S1–S4) mit Kldefinitions und Auflösungszeilen definiert.
  - API-Contract-Tabelle um `PUT /recipes/{id}` erweitert.
  - Requirements Traceability für UC6 Editing Recipes aktualisiert (Priorität auf „High" erhöht).

- **Neue Frontend-Tests (4 Dateien, 43 neue Tests):**
  - `editRecipeService.test.ts` — 10 Tests für Mapping und API-Aufrufe (getRecipe, updateRecipe).
  - `useRecipeListStore.test.ts` — 7 Tests für den Recipe-List-Store.
  - `navigation.test.ts` — 13 Tests für alle definierten Router-Routes inkl. neuem Edit-Route.
  - `i18n.test.ts` — 9 Tests für Locale-Key-Konsistenz (DE/EN).

- **Neue Backend-Tests (4 Dateien):**
  - `RecipeServiceTest.java` — Unit-Tests für create, findById, updateRecipe, deleteRecipe inkl. Ownership-Checks (Mockito).
  - `RecipeControllerTest.java` — Integrationstests mit MockMvc + Testcontainers PostgreSQL für alle CRUD-Endpunkte.
  - `CategoryControllerTest.java` — Integrationstests für GET /categories (8 SuperCategories, 73 Categories).
  - `AuthIntegrationTest.java` — Tests für /auth/me mit OAuth2-Mock und fehlender Authentifizierung.

- **Rezept bearbeiten — neues Feature (Backend + Frontend):**
  - Backend: `PUT /recipes/{id}` Endpoint in RecipeController und `updateRecipe()` in RecipeService implementiert, inkl. Ownership-Check und Orphan-Cleanup für Ingredients.
  - Backend: `GET /recipes/{id}` Endpoint zum Laden einzelner Rezepte ergänzt.
  - Frontend: `editRecipeService.ts` mit `getRecipe()`, `updateRecipe()` und `mapResponseToState()` erstellt.
  - Frontend: `useCreateRecipeStore` um Edit-Modus erweitert (`editingRecipeId`, `loadRecipe()`, `isEditMode`).
  - Frontend: `EditRecipeView.vue` als neue View mit automatischem Laden beim Mount.
  - Frontend: Router um `/recipes/:id/edit` Route ergänzt.
  - Frontend: PreviewFrame zeigt im Edit-Modus „Rezept aktualisieren" statt „Rezept erstellen".
  - i18n: Deutsche und englische Übersetzungen für Edit-Modus ergänzt.

## Validierung

- Frontend-Checks lokal ausgeführt:
  - `npm ci` ✓
  - `npx vitest run` — **54 Tests grün** (7 Testdateien)
  - `npx vue-tsc --noEmit` ✓
  - `npx eslint src/` ✓
  - `npx vite build` ✓ (253 Modules, 2.7s)
- Backend-Tests: Strukturiert nach gleichem Pattern wie bestehender Smoke-Test (Testcontainers + MockMvc). Laufen über CI (Java 25 + Docker erforderlich).

## Geänderte Dateien

**Docs:**
- `docs/TestPlan.md` — v1.1 mit Testfällen, Schedule, Defect-Schema

**Backend:**
- `RecipeController.java` — GET /{id}, PUT /{id} hinzugefügt
- `RecipeService.java` — findById(), updateRecipe() hinzugefügt

**Frontend:**
- `editRecipeService.ts` — neuer Service
- `useCreateRecipeStore.ts` — Edit-Modus erweitert
- `EditRecipeView.vue` — neue View
- `router/index.ts` — Edit-Route hinzugefügt
- `PreviewFrame.vue` — Edit-Modus Button-Text
- `de.json` / `en.json` — i18n Keys ergänzt

**Tests:**
- Frontend: 4 neue Testdateien (editRecipeService, useRecipeListStore, navigation, i18n)
- Backend: 4 neue Testdateien (RecipeServiceTest, RecipeControllerTest, CategoryControllerTest, AuthIntegrationTest)
