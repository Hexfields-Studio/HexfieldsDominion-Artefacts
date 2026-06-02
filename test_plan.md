# Test Plan - Hexfields: Dominion - Version 1.0

## Versionsübersicht

| Datum | Version | Beschreibung | Autor |
|-------|---------|-------------|--------|
| 28/Apr/2026 | 1.0 | Dokument erstellt | Alex, Jona, Marcel |
| 06/May/2026 | 1.1 | Aktualisierung Metriken | Marcel |

## Inhaltsverzeichnis

- [Test Plan - Hexfields: Dominion - Version 1.0](#test-plan---hexfields-dominion---version-10)
  - [Versionsübersicht](#versionsübersicht)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [1. Einleitung](#1-einleitung)
    - [1.1 Zweck](#11-zweck)
    - [1.2 Hintergrund](#12-hintergrund)
    - [1.3 Umfang](#13-umfang)
      - [Testphasen](#testphasen)
      - [Testabdeckungsziele](#testabdeckungsziele)
      - [Zu testende Funktionen](#zu-testende-funktionen)
      - [Nicht zu testende Funktionen](#nicht-zu-testende-funktionen)
      - [Annahmen](#annahmen)
    - [1.4 Projektidentifikation](#14-projektidentifikation)
  - [2. Testanforderungen](#2-testanforderungen)
  - [3. Teststrategie](#3-teststrategie)
    - [3.1 Testtechniken und Werkzeuge](#31-testtechniken-und-werkzeuge)
      - [Automatisierte Tests](#automatisierte-tests)
      - [Test-Verwaltung](#test-verwaltung)
    - [3.2 Testtypen](#32-testtypen)
      - [3.2.1 Function Testing](#321-function-testing)
      - [3.2.2 Security and Access Control Testing](#322-security-and-access-control-testing)
      - [3.2.3 Configuration Testing](#323-configuration-testing)
      - [3.2.4 User Interface Testing](#324-user-interface-testing)
  - [4. Ressourcen](#4-ressourcen)
    - [4.1 Rollen](#41-rollen)
    - [4.2 Testumgebung](#42-testumgebung)
  - [5. Liefergegenstände](#5-liefergegenstände)
    - [Testmodelle und Testergebnisse](#testmodelle-und-testergebnisse)
    - [Testausgaben](#testausgaben)
    - [Defekt-Berichte](#defekt-berichte)

---

## 1. Einleitung

### 1.1 Zweck

Dieses Testplan-Dokument für das Projekt Hexfields: Dominion unterstützt folgende Ziele:

- Identifizierung von Testanforderungen basierend auf den Anforderungsspezifikationen und Use Cases
- Definition der empfohlenen Teststrategien und Tester-Methoden
- Dokumentation der relevanten Testtypen mit Fokus auf Spiellogik und Authentifizierungssystem
- Festlegung der Kriterien für erfolgreiche Testdurchführung
- Bereitstellung von Testrichtlinien für Backend und Frontend

### 1.2 Hintergrund

Hexfields: Dominion ist ein browserbasiertes Mehrspielerspiel mit folgenden Kernfunktionalitäten:

- Authentifizierungssystem: Benutzer können sich mit Account oder als Gast anmelden
- Spiellogik: Regelbasiertes Spielsystem mit verschiedenen Spielzügen und Interaktionen
- Lobbyverwaltung: Erstellen und Beitreten zu Lobbies mit Administrationsfunktionen
- Benutzeroberfläche: Responsive UI mit Light/Dark Mode Unterstützung

### 1.3 Umfang

#### Testphasen

Die Testabdeckung erfolgt in folgenden Phasen:

- Unit Tests: Isolierte Tests einzelner Funktionen und Methoden
- Integrationstests: Tests der Zusammenarbeit zwischen Komponenten

#### Testabdeckungsziele

- Backend: Nahe 100% Abdeckung mit Fokus auf:
  - Spiellogik, Regelverarbeitung
  - Authentifizierungssystem (Login, Logout, Token-Validierung)
  - Zugriffskontrolle
  - Lobbyverwaltung

- Frontend: Etwa 10-20% Abdeckung
  - Fokus auf Grundmuster und Best Practices
  - Vereinfachte Tests für Funktionen, die schwer zu testen sind
  - v.A. Authentifizierung zum Backend

#### Zu testende Funktionen

- Spielzugausführung und Regelverarbeitung
- Benutzer-Authentifizierung (Account und Gast)
- Lobbyverwaltung (Erstellen, Beitreten, Admin-Funktionen)
- Zugriffskontrollen und Berechtigungen
- Optionsmenü und UI-Funktionalität

#### Nicht zu testende Funktionen

- Komplexe Rendering-Tests
- Browser-spezifische Kompatibilität (wird manuell bewertet)

#### Annahmen

- Die GitHub Pipeline führt automatisch Tests bei jedem Push aus
- Bei kritischen Fehlern kann eine Version reverted werden
- Lokale Testausführung ist während der Entwicklung möglich
- Die IDE (IntelliJ) zeigt Testergebnisse an

### 1.4 Projektidentifikation

| Dokument | Verfügbar | Geprüft | Anmerkungen |
|-|-|-|-|
| Requirements Specification | Ja | Ja | SRS dokumentiert |
| Functional Specification | Ja | Ja | Use Cases definiert |
| Software Architecture Document | Ja | Ja | SAD verfügbar |
| Architecture Significant Requirements | Ja | Ja | ASR definiert |
| Use-Case Reports | Ja | Ja | Umfassend dokumentiert |
| Design Specifications | Ja | Ja | Implementierungsdetails vorhanden |

---

## 2. Testanforderungen

Die Testanforderungen sind direkt mit den folgenden Use Cases und funktionalen Anforderungen verknüpft:

1. Authentifizierung: Login, Logout, Passwort-Reset, Registrierung, Gast-Login
2. Spiellogik: Spielzugausführung, Regelverarbeitung, Spielfortsetzung
3. Lobbyverwaltung: Lobby erstellen, Lobby beitreten, Admin-Funktionen
4. Zugriffskontrolle: Berechtigungsprüfung, Rollenbasierter Zugriff
5. Benutzeroberfläche: Optionsmenü, Light/Dark Mode, Spielinteraktionen

---

## 3. Teststrategie

### 3.1 Testtechniken und Werkzeuge

#### Automatisierte Tests

| Komponente | Werkzeug | Beschreibung |
|-----------|----------|-------------|
| Backend | Gradle, JUnit | Automatisierte Unit- und Integrationstests |
| Frontend | Gherkin / Jest | BDD und Unit-Tests (optional) |

#### Test-Verwaltung

- Automatisierte Pipeline: GitHub Actions führt Tests bei jedem Push automatisch aus
- Lokale Entwicklung: Tests können lokal in IntelliJ ausgeführt werden
- Fehlerbehandlung: Unkorrekte Commits können durch Revert von Main rückgängig gemacht werden
- Testausführung: Im Falle von unbehobenen Fehlern kann zu vorherigen Versionen zurückgegangen werden

### 3.2 Testtypen

#### 3.2.1 Function Testing

Spiellogik - Regelverarbeitung

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Die Spiellogik hält die festgelegten Spielregeln bei der Fortsetzung des Spielverlaufs ein | Verschiedene Eingabedaten abdecken verschiedene Szenarien, z.B. Versuch eines Spielzugs wenn der Spieler nicht am Zug ist vs. wenn er am Zug ist | Alle Tests erfolgreich ausgeführt | Keine |

Benutzer-Authentifizierung

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Nutzer können sich mit einem Account oder als Gast authentifizieren | Verschiedene Eingabedaten abdecken Szenarien mit validen/invaliden Refresh- und Access Tokens | Alle Tests erfolgreich ausgeführt | Token-Validierung auf Gültigkeit prüfen |

#### 3.2.2 Security and Access Control Testing

Authentifiziertes Spielzugriff (Application-Level Security)

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Nutzer können nur mit dem Spiel interagieren, solange sie authentifiziert sind | Unterscheidung zwischen authentifizierten und unauthentifizierten Nutzern (invalide AccessToken oder fehlender Token). Unauthentifizierte Nutzer haben keinen Zugriff auf Spielfunktionen | Alle Akteure haben Zugriff auf für sie freigegebene Daten und Funktionen | Nutzer werden vom Backend als Akteure zugewiesen |

Lobbyadmin-Privilegien

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Nutzer innerhalb einer Lobby dürfen nur die Lobbyregeln ändern, wenn sie der Lobbyadmin sind | Unterscheidung zwischen Lobby-Admins und Mitspielern. Der Lobbyadmin hat spezielle Rechte (Spielregeln modifizieren, Spiel starten, Mitspieler rauswerfen, Admin-Status weitergeben) | Alle Akteure haben Zugriff auf für sie freigegebene Funktionen | Nutzer werden vom Backend als Akteure zugewiesen |

#### 3.2.3 Configuration Testing

Lobbies - Maximale Anzahl

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Bestätigung, dass die festgelegte maximale Anzahl an existierenden Lobbies eingehalten wird | Wiederholtes Erstellen von Lobbies über die Grenze hinaus | Neue Lobbies können nicht erstellt werden, wenn das Limit erreicht wurde | Das Lobbylimit gilt nur für Gäste. Es verhindert DOS-Attacken durch Arbeitsspeicher-Überlastung |

#### 3.2.4 User Interface Testing

Optionsmenü

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Bestätigung, dass alle Nutzer das Optionsmenü öffnen können | Simuliertes Klicken auf das Zahnrad-Symbol; Anwendung der Optionen im Dialog | Optionsdialog wurde geöffnet; Vorgenommene Änderungen wurden umgesetzt | Initial ist das Theme vom Browser abhängig. Außer auf der Startseite befindet sich ein "Logout"-Knopf im Dialog |

---

## 4. Ressourcen

### 4.1 Rollen

| Rolle | Verantwortlichkeiten |
|------|---------------------|
| Testleiter | Übersicht über Testplanung und -ausführung |
| Entwickler (Backend) | Implementierung und Unit-Tests für Backend-Logik |
| Entwickler (Frontend) | Implementierung und Integration-Tests für UI |
| QA-Ingenieur | Durchführung von Integrationstests und Validierung |

### 4.2 Testumgebung

- Backend-Testumgebung: Lokale Gradle-Build-Umgebung mit JUnit
- Frontend-Testumgebung: Node.js mit Jest / Gherkin
- Datenbank: Konfigurierte Test-Datenbank mit kontrollierten Daten
- Continuous Integration: GitHub Actions Pipeline für automatisierte Testausführung

---

## 5. Liefergegenstände

### Testmodelle und Testergebnisse

- Unit-Test-Suites für Backend-Komponenten
- Integrations-Test-Suites
- Test-Berichte aus der GitHub CI/CD Pipeline

### Testausgaben

- Test-Logs vom automatisierten Build-Prozess
- Fehlerberichte mit Reproduktionsschritten
- Testabdeckungsberichte

### Defekt-Berichte

- Detaillierte Defektbeschreibungen mit Severity-Einstufung
- Reproduktionsschritte und erwartetes Verhalten
- Zugeordnete Prioritäten und Zuständige
