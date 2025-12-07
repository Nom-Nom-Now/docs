# ASR-Übersicht für NomNomNow

Dieses Verzeichnis dokumentiert die *Architecturally Significant Requirements* (ASR) für das Projekt NomNomNow nach dem 3‑Schritt‑Ansatz aus der Vorlesung.

## Inhalte

1. [Utility Tree & Qualitätsattribut-Szenarien](./asr-utility-tree.md)  
   - Projektkontext  
   - Qualitätsattribute & Priorisierung  
   - Utility Tree (Tabelle + Mermaid-Diagramm)  
   - 6‑Part‑Szenarien für alle priorisierten Qualitätsattribute  

2. [Architekturentscheidungen, Taktiken & Entwurfsmuster](./asr-architecture-decisions.md)  
   - Taktiken je Qualitätsattribut (Performance, Availability, Usability, Modifiability, Security)  
   - Gewählter Architekturstil und grobe Struktur  
   - Eingesetzte Muster (Controller–Service–Repository, DTOs, etc.)  
   - Mapping von Szenarien ↔ Taktiken ↔ Architektur

## Kurzüberblick zum Projekt

- Endnutzer:innen können Rezepte erstellen, eine Woche planen und daraus einen Einkaufszettel generieren.
- Stack: Spring Boot (Java) + Spring Data JPA, PostgreSQL, Vue + TypeScript, Docker‑Deployment auf Linux‑Rootserver.
- Wichtigste Qualitätsattribute: Performance, Availability, Usability; zusätzlich Modifiability & Security (wegen erwarteter Änderungen und Benutzeraccounts).

