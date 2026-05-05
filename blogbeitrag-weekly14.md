# Blogbeitrag - Weekly 14

## Weekly 14: Testplan gestrafft, Recipe Update umgesetzt, CI stabilisiert

Diese Woche lag der Fokus auf der Überarbeitung des Testplans, dem Ausbau automatisierter Tests, der Implementierung des Recipe-Update-Features sowie der Stabilisierung der CI-Pipeline. Ziel war es, den Testumfang an das reale 5-Stunden-Budget anzupassen und gleichzeitig die wichtigsten Backend- und Frontend-Funktionen zuverlässig abzusichern.

## Erledigte Punkte

### Testplan (TestPlan.md v1.2)

- Testplan gestreamlined auf 5-Stunden-Budget: von 665 auf 371 Zeilen gekürzt (~43%).
- E2E-, Contract- und UI-Tests aus Scope genommen (begründete Exclusions-Tabelle hinzugefügt, deferriert auf Sprint 15+).
- Fokus auf Happy-Path-Tests für RecipeService und Controller-Endpunkte.
- Coverage-Ziel angepasst: "Fokus auf das erfolgreiche Testen der Haupt-Services" statt harter Prozentzahlen.
- Requirements Traceability für UC5/UC6 mit konkreten Testfall-Referenzen ergänzt.

### Neue Backend-Tests (4 Dateien, rund 30 Testfälle)

- **RecipeServiceTest.java**: 10 Unit-Tests (Mockito) für create, findById, findAll, updateRecipe, deleteRecipe inkl. Ownership-Checks und Orphan-Cleanup.
- **RecipeControllerTest.java**: 11 Integrationstests (MockMvc + Testcontainers PostgreSQL) für POST, GET, PUT, DELETE /recipes inkl. Validierungen und 403-Fälle.
- **CategoryControllerTest.java**: 4 Tests für GET /categories mit Testcontainers-Setup.
- **AuthIntegrationTest.java**: 4 Integrationstests für /auth/me mit oauth2Login und fehlender Authentifizierung.

### Neue Frontend-Tests (4 Dateien, rund 40 Testfälle)

- **editRecipeService.test.ts**: 10 Tests für Mapping und API-Aufrufe (getRecipe, updateRecipe).
- **useRecipeListStore.test.ts**: 7 Tests für den Recipe-List-Store.
- **navigation.test.ts**: 13 Tests für alle definierten Router-Routes inkl. neuem Edit-Route.
- **i18n.test.ts**: 9 Tests für Locale-Key-Konsistenz (DE/EN).

### Rezept bearbeiten: neues Feature (Backend + Frontend)

- Backend: PUT /recipes/{id} und GET /recipes/{id} Endpoints implementiert, inkl. Ownership-Check und atomarem Orphan-Cleanup für Ingredients.
- Frontend: editRecipeService.ts, erweiterter useCreateRecipeStore (Edit-Modus), EditRecipeView.vue, Router-Ergänzung, i18n Keys.

### CI-Fehlerbehebung (Spring Boot 4.x)

Die Umstellung auf Spring Boot 4.x brachtinge mit sich, dass sich mehrere Packages verschoben haben. Dadurch mussten Tests mehrfach angepasst werden, bis die CI-Pipeline wieder stabil lief:

- **@AutoConfigureMockMvc**: Package-Wechsel in `org.springframework.boot.webmvc.test.autoconfigure`, Dependency `spring-boot-webmvc-test` statt `spring-boot-test-autoconfigure`.
- **OAuth2 in MockMvc**: `SecurityContextHolder` reicht nicht durch den Filter-Chain. Umstellung auf `SecurityMockMvcRequestPostProcessors.oauth2Login()` mit `spring-security-test` Dependency.
- **Test-Datenbank-Schema**: `CREATE SCHEMA IF NOT EXISTS app` + `hibernate.default_schema=app` in `src/test/resources/application.yaml`.
- **Test-Isolation**: `@BeforeEach` mit `deleteAll()` verhindert Daten-Leakage zwischen Tests.

Dadurch sind die Kernpfade (Rezept erstellen, auflisten, bearbeiten, löschen) nun automatisiert getestet und die CI-Pipeline läuft zuverlässig grün.

### Code-Review-Feedback umgesetzt

Copilot-Review im PR hat 4 Verbesserungen aufgezeigt, die wir direkt umgesetzt haben:

- **Null-safe Ownership Check**: `recipe.getOwner()` kann null sein (legacy rows) → NPE verhindert.
- **Atomare Orphan-Cleanup**: Check-Then-Delete Loop (race-prone) durch `@Modifying` JPQL-Query ersetzt.
- **Synthetic 500 Test entfernt**: Test hat nur Test-Harness-Verhalten getestet, nicht echte App-Logik.
- **@WebMvcTest für Categories**: Von `@SpringBootTest` + Testcontainers auf lightweight Slice-Test umgestellt.

## Validierung

- Frontend: `npx vitest run` → **54 Tests grün** (7 Testdateien)
- Frontend: `npx vue-tsc --noEmit` ✓
- Frontend: `npx eslint src/` ✓
- Backend: `./mvnw verify` → **CI grün** (Spring Boot 4.x + Java 25 + Testcontainers)

## Geänderte Dateien

**Docs:**
- `docs/TestPlan.md` — v1.2 gestreamlined (665→371 Zeilen)
- `docs/blogbeitrag-weekly14.md` — aktualisiert

**Backend:**
- `RecipeController.java` — GET /{id}, PUT /{id}
- `RecipeService.java` — updateRecipe(), null-safe ownership, atomarer Orphan-Cleanup
- `IngredientRepository.java` — `deleteOrphanedByIds()` Bulk-Query
- `RecipeComponentRepository.java` — bereinigt
- `pom.xml` — `spring-boot-webmvc-test`, `spring-security-test`
- `src/test/resources/application.yaml` — Test-Config (ddl-auto, schema, frontend-url)
- `src/test/resources/schema-init.sql` — `CREATE SCHEMA IF NOT EXISTS app`
- 4 neue Testdateien (RecipeServiceTest, RecipeControllerTest, CategoryControllerTest, AuthIntegrationTest)

**Frontend:**
- `editRecipeService.ts` — neuer Service
- `useCreateRecipeStore.ts` — Edit-Modus erweitert
- `EditRecipeView.vue` — neue View
- `router/index.ts` — Edit-Route
- `PreviewFrame.vue` — Edit-Modus Button-Text
- `de.json` / `en.json` — i18n Keys
- 4 neue Testdateien (editRecipeService, useRecipeListStore, navigation, i18n)
