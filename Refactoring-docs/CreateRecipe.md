# Zusammenfassung: Umbau der Rezept-Erstellung

## 1. Was wurde geändert?
Die Funktion zum Erstellen von Rezepten wurde von einer einzigen langen Seite auf einen geführten Prozess mit mehreren Schritten umgestellt.

## 2. Wichtige Dateien
* **Alte Version**: Der ursprüngliche Code wurde zur Sicherung nach `src/feature/create-recipe-old/` verschoben.
* **Neue Hauptseite**: `CreateRecipeView.vue` ist der neue Startpunkt.
* **Fortschritts-Steuerung**: `CreateRecipeProgressBar.vue` regelt die Anzeige der einzelnen Schritte.
* **Navigations-Buttons**: `StepNavigationButton.vue` wird für die Steuerung der Schritte genutzt.

## 3. Die 5 neuen Schritte
Der Prozess ist nun in logische Phasen unterteilt:
1. **Zutaten (Ingredients)**: Eingabe der benötigten Lebensmittel.
2. **Zubereitung (Preparation)**: Anleitung zum Kochen.
3. **Kategorien (Categories)**: Auswahl der passenden Gruppen.
4. **Bild (Image)**: Hinzufügen eines Fotos.
5. **Vorschau (Preview)**: Letzte Kontrolle vor dem Speichern.

## 4. Vorteile
* **Einfachheit**: Nutzer sehen nur das, was gerade wichtig ist, was die Bedienung erleichtert.
* **Sauberer Code**: Durch die Aufteilung in kleine Bausteine ist das Programm leichter zu pflegen und Fehler sind schneller zu finden.
* **Sprachunterstützung**: Texte können nun einfacher für verschiedene Sprachen vorbereitet werden.