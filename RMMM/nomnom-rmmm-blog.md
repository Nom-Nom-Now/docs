# Nom Nom Now — RMMM Tabelle & Projektfortschritt

## RMMM — Risk Mitigation, Monitoring and Management

| Risk ID | Kategorie | Risiko-Beschreibung | Wahrscheinlichkeit | Auswirkung | Risk Score | Mitigations-Strategie | Indikator | Contingency Plan | Verantwortlich | Status | Zuletzt geändert |
|---------|-----------|---------------------|-------------------|-----------|-----------|----------------------|-----------|-----------------|---------------|--------|----------------|
| R-01 | Technisch | **Authentifizierung und Autorisierung**: Google OAuth2 Integration schlägt fehl oder Token-Refresh funktioniert nicht, wodurch authentifizierte Endpunkte nicht erreichbar sind | Mittel | Hoch | **6** | Sicherheitskonfiguration mit Defense-in-Depth: JWT-Token-Validierung, Fallback-Login-Route, Token-Refresh-Mechanismus implementiert; SecurityConfig mit Role-Based Access Control | Login-Rate im Monitoring steigt; Nutzer können nicht auf `/recipes` POST zugreifen; `/auth/me` 401 | OAuth2 deaktivieren und Basic Auth als Fallback bereitstellen; Debug-Logging für Security-Filter aktivieren | Backend-Team | Offen | 2026-04-13 |
| R-02 | Technisch | **Datenbank-Migrationen**: Flyway-Migrationen schlagen fehl bei Deployment, besonders wenn bereits Daten vorhanden sind (Schema-Changes, Data-Loss) | Mittel | Hoch | **6** | Versionierte Migrationen (V1-V4); `${appUserPassword}`-Placeholder; Migration nur via `docker compose --profile migrate`; Backup-Strategie vor jedem Deployment | Flyway-Migration schlägt fehl im CI/CD; Backend startet nicht; Fehler in Application-Logs | Rollback auf vorherige Migration; `docker compose down -v` und Neustart mit sauberer DB; Manuelle SQL-Korrektur | Backend-Team | Offen | 2026-04-13 |
| R-03 | Technisch | **API-Konsistenz zwischen Frontend und Backend**: Frontend erwartet DTOs/Endpoints die im Backend noch nicht oder anders implementiert sind; Datentyp-Mismatches (String vs. Long für IDs) | Hoch | Mittel | **6** | DTO-Konventionen dokumentiert; Request/Response-Objekte strikt getrennt; RecipeMapper für Entity↔DTO; Frontend-Service-Layer (`recipeService.ts`) als zentrale API-Schnittstelle | Frontend zeigt Fehler bei API-Calls; TypeScript-Typen stimmen nicht mit Backend überein; 4xx/5xx Errors im Browser-Netzwerk-Tab | API-Doku mit Swagger/OpenAPI generieren; Frontend-Tests mit Mock-API; Pair-Programming bei API-Änderungen | Full-Stack-Team | Offen | 2026-04-13 |

---

## Größtes technisches Risiko: Authentifizierung (R-01)

### Was ist das Risiko?

Die Nom Nom Now App nutzt **Google OAuth2** als einzige Authentifizierungsmethode. Das bedeutet: Wenn der OAuth2-Flow fehlerhaft ist, kann sich niemand einloggen, und alle geschützten Endpunkte (Rezept erstellen, Kategorien bearbeiten) sind unreachable.

Die `SecurityConfig.java` (54 Zeilen) steuert, welche Endpunkte öffentlich und welche authentifiziert sind. Der `AuthController` und `CurrentUserService` verarbeiten den OAuth2-Callback und den Token-Refresh. Das `AppUser`-Entity speichert die User-Daten in der Datenbank (Flyway Migration V4).

### Warum ist es das größte Risiko?

1. **Single Point of Failure**: Ohne funktionierende Auth ist die App halbiert. Nur Rezepte browsen geht noch.
2. **Externe Abhängigkeit**: Google OAuth2 hängt von externer Infrastruktur ab (Google-Server, API-Quotas, Network). Ausfälle sind nicht kontrollierbar.
3. **Hohe Komplexität**: OAuth2 beinhaltet Redirect-Flows, JWT-Token-Management, CSRF-Schutz und CORS-Konfiguration. Fehler sind schwer zu debuggen.
4. **Frontend-Backend-Koordination**: Der Login-Flow erfordert exakte Abstimmung zwischen Vue-Router, Google-Client-Library und Spring Security.

### Mitigation Strategy

- **Defense in Depth:** SecurityConfig ist mehrschichtig aufgebaut — öffentliche vs. geschützte Routes sind klar getrennt
- **Konfiguration via `.env`:** Google Client ID/Secret sind nicht im Code hartcodiert, sondern über Environment-Variables konfigurierbar
- **Documentation:** `docs/google-login.md` dokumentiert den gesamten OAuth2-Setup-Prozess Schritt für Schritt
- **CORS-Konfiguration:** `CorsConfig.java` stellt sicher, dass nur der Frontend-Origin Anfragen an die API stellen darf
- **Defense Defaults:** Spring Security filtert alle Requests standardmäßig, nur explizit freigegebene Endpunkte sind öffentlich

### Contingency Plan

Wenn der OAuth2-Flow ausfällt:
1. **Sofort:** OAuth2 in `.env` deaktivieren (`GOOGLE_CLIENT_ID=disabled`)
2. **Kurzfristig:** Basic Auth als Fallback in SecurityConfig einrichten (schnell zu implementieren, sichert den Entwicklungsbetrieb)
3. **Mittelfristig:** Lokale Registrierung (E-Mail + Passwort) implementieren, um unabhängig von Google zu sein
4. **Debugging:** Erweitertes Logging für Security-Filter aktivieren; OAuth2-Callback-URL in Google Console verifizieren

### Aktueller Status

Die Google OAuth2 Integration ist implementiert (AuthController, AppUser-Entity, SecurityConfig, Migration V4). Frontend hat eine LoginPage.vue. Der Flow wurde erfolgreich getestet. Das Risiko bleibt **Offen** weil die Stabilität in Produktion noch nicht validiert wurde.

---

## Projektfortschritt seit Halbzeitpräsentation

### Implementierte Features

**Backend (Spring Boot 4 · Java 25 · PostgreSQL):**
- Rezept-CRUD (Create, Read, Update, Delete) über REST API
- Kategorie-System mit Many-to-Many Relation zu Rezepten
- Google OAuth2 Authentifizierung
- Flyway-Datenbankmigrationen
- Pagination auf Rezept-Listing
- Docker Compose Setup (PostgreSQL, Flyway, Backend)
- GitHub Actions CI/CD Pipeline
- DTO-Layer mit RecipeMapper
- Globale Exception Handling (BadRequestException, ResourceNotFoundException)

**Frontend (Vue 3 · TypeScript · Material Web):**
- Login-Seite mit Google OAuth2
- Rezept-Erstellung 
- Rezept-Browsing
- i18n-Support (Deutsch/Englisch via vue-i18n)
- Navigations-Framework mit NavigationFrame und Router
