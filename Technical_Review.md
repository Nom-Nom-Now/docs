# Technical Review: Weekly Task 8

- Datum 25.5.2026
- Startzeit: 20:23; Endzeit:
- Teilnehmer:
    - Rafael zeigt code, moderiert
    - Jonah von BetCeption als externer Reviewer
    - Dylan protokolliert, moderiert

## Ziel/Fokus

Das Ziel ist die Qualität in unserem Softwareprojekt zu steigern. Vor allem werden hier die Bereiche betrachtet, die noch entwickelt werden oder einfach zu ändern sind.

- Überprüfung vom Backend
    - Rest Schnittstelle
    - Datenmodell

- Überprüfung Frontend
    - generelle Codequalität
    - lesbarkeit der Codebasis

## Review Backend

### Datenmodell

- Kategorien sind nicht relational modelliert
- Sonst gutes und sinnvolles Datenmodell
- DB-Constraints sind schwächer als die API-Validierung. Beispiele: price_per_person hat keinen Check, recipe_component.quantity und unit sind in der Migration nullable, obwohl DTO/Entity teils @NotNull/nullable=false erwarten.

### Rest Schnittstelle

- Create Enpunkte verhalten sich nicht konventionell. Bei POST /recipes gibt 200 zurück und nicht 201.
- Die Ressource ist unvollständig modelliert: Es gibt GET /recipes, PUT /recipes/{id}, DELETE /recipes/{id} und GET /recipes/{id}/image, aber kein GET /recipes/{id}.

## Review Frontend

### Codequalität und Lesbarkeit

- API zugriffe sind über mehrere Dateien dupliziert
- Es existieren viele Starter/-Platzhalterreste im produktiven Routing
- i18n ist inkonsequent.
- Die Codebasis ist klein und gut testbar
- Die Namen der Komponenten sind verständlich und logisch

## Reviewmethodik

- Es wurden konkrete Codezeilen und Dateien betrachtet, um die Punkte zu belegen
- Es wurden konkrete Verbesserungsvorschläge gemacht, z. B. API-Zugriffe in einem Service zentralisieren, i18n konsequent anwenden, REST-Konventionen befolgen
- Es wurde auf die Balance zwischen kurzfristiger Machbarkeit und langfristiger Wartbarkeit geachtet

## Kriterien für Review

- Codequalität
- Skalierbarkeit
- Wartbarkeit

## Ergebnis

  Im Review wurden konkrete Verbesserungsmaßnahmen abgeleitet. Im Backend sollen REST-Konventionen stärker eingehalten werden,
  z. B. 201 Created bei POST /recipes und ein zusätzlicher GET /recipes/{id} Endpunkt. Außerdem sollen Datenbank-Constraints an
  die API-Validierung angepasst und Kategorien langfristig relational modelliert werden.

  Im Frontend sollen API-Zugriffe zentralisiert, Platzhalterreste entfernt und i18n konsequenter genutzt werden. Als Best
  Practices wurden mitgenommen: klare Schnittstellen, keine duplizierte API-Logik, konsistente Validierung zwischen API und
  Datenbank sowie eine saubere Trennung von UI, Store und Service-Logik.