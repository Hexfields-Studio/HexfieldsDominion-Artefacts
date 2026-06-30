# Refactoring-Zusammenfassung - Hexfields: Dominion - Version 1.0

## Änderungsverzeichnis

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 30/Jun/2026 | 1.0 | Initiale Erstellung | Marcel |

## Inhaltsverzeichnis

- [Refactoring-Zusammenfassung - Hexfields: Dominion - Version 1.0](#refactoring-zusammenfassung---hexfields-dominion---version-10)
  - [Änderungsverzeichnis](#änderungsverzeichnis)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Zweck des Dokuments](#zweck-des-dokuments)
  - [Betroffene Repositories](#betroffene-repositories)
  - [Geltungsbereich](#geltungsbereich)
  - [Refactoring 1: Frontend-Repository-Hooks auf React-Standard umstellen](#refactoring-1-frontend-repository-hooks-auf-react-standard-umstellen)
    - [Ausgangslage](#ausgangslage)
    - [Vorheriger Ansatz](#vorheriger-ansatz)
    - [Neuer Ansatz mit `useSyncExternalStore`](#neuer-ansatz-mit-usesyncexternalstore)
    - [Technische Auswirkungen](#technische-auswirkungen)
    - [Clean-Code-Prinzip](#clean-code-prinzip)
  - [Refactoring 2: Redundanzen in der Backend-Authentikation entfernen](#refactoring-2-redundanzen-in-der-backend-authentikation-entfernen)
    - [Ausgangslage](#ausgangslage-1)
    - [Umsetzung](#umsetzung)
    - [Technische Auswirkungen](#technische-auswirkungen-1)
    - [Clean-Code-Prinzip](#clean-code-prinzip-1)

## Zweck des Dokuments

Dieses Dokument fasst die im [Blogpost Woche 13](https://github.com/orgs/Hexfields-Studio/discussions/50) durchgeführten Refactorings zusammen.
Ziel ist die nachvollziehbare Dokumentation von Motivation, technischer Umsetzung und Auswirkungen auf Wartbarkeit und Lesbarkeit.

## Betroffene Repositories

- `Hexfields-Studio/HexfieldsDominion` (Frontend)
- `Hexfields-Studio/HexfieldsDominion-Backend` (Backend)

## Geltungsbereich

Dokumentiert werden zwei konkrete Refactorings:
1. Umstellung von Repository-Subscriptions im Frontend auf React-native Hook-Mechanik.
2. Entfernung redundanter Authentifizierungslogik im Backend durch Funktions-Extraktion.

* * *

## Refactoring 1: Frontend-Repository-Hooks auf React-Standard umstellen

### Ausgangslage

Das Frontend verwendet ein Repository-Modell als Abstraktionsschicht zwischen Datenzugriff und Anwendungslogik.  
GUI-Komponenten benötigen aktuelle Repository-Daten (z. B. Spielstatus, Ressourcen, Zugreihenfolge), sodass Änderungen im Repository zuverlässig Re-Renders auslösen müssen.

### Vorheriger Ansatz

Bisher wurde pro Komponente wiederholt Boilerplate-Code eingesetzt:
- Repository über Context beziehen
- Lokalen State initialisieren
- Manuelles Subscribing via `keepMeUpdated(setStateAction)`
- Zusätzliche `useEffect`-Kopplungen für abgeleitete Zustände

Im Repository wurden `setState`-Dispatcher gesammelt und bei Datenänderungen iteriert aufgerufen.  
Dieser Ansatz funktionierte, war aber repetitiv und ohne Kontext schwer verständlich.

### Neuer Ansatz mit `useSyncExternalStore`

Die Subscriptions wurden auf Reacts native API `useSyncExternalStore` umgestellt.

Dazu wurden im Repository die für externe Stores benötigten Funktionen ergänzt:
- `subscribe`
- `getMatchData`
- `emitChange`

Darauf aufbauend wurden domänenspezifische, wiederverwendbare Hooks erstellt (z. B. `useIsMyTurn`, `useMatchData`, `useMyRessources`), die in Komponenten direkt einzeilig verwendet werden können.

### Technische Auswirkungen

- Deutliche Reduktion wiederholter Hook-/Subscription-Boilerplate in GUI-Komponenten.
- Klare Trennung von Zustandsanbindung (Hook) und UI-Darstellung (Komponente).
- Höhere Lesbarkeit und schnellere Verständlichkeit im Komponenten-Code.
- Bessere React-Konformität durch Nutzung einer nativen Subscription-Schnittstelle.

### Clean-Code-Prinzip

**KISS (Keep it simple, stupid):**  
Komponenten verwenden nun einfache, selbsterklärende Custom-Hooks statt manueller Subscription-Mechanik.

* * *

## Refactoring 2: Redundanzen in der Backend-Authentifizierung entfernen

### Ausgangslage

In der Authentifizierung des Backend-Repositories war mehrfach verwendete Logik redundant an mehreren Stellen vorhanden.

### Umsetzung

Im Commit  
`7db20c2b958ed56a1587bc57f510598a5d565f09`  
wurde die wiederkehrende Authentifizierungslogik in eine gemeinsame Funktion extrahiert und zentralisiert.

### Technische Auswirkungen

- Reduktion des Quellcodes um **24 Zeilen**.
- Einheitlichere Implementierung der Authentifizierungsschritte.
- Verbesserte Wartbarkeit bei zukünftigen Änderungen.
- Geringeres Risiko für inkonsistente Fehlerbehebungen in duplizierten Codepfaden.

### Clean-Code-Prinzip

**DRY (Don’t Repeat Yourself):**  
Mehrfach vorhandene Logik wurde konsolidiert, um Redundanz und Pflegeaufwand zu reduzieren.
