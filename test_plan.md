# Test Plan - Hexfields: Dominion - Version 1.2

## Versionsübersicht

| Datum | Version | Beschreibung | Autor |
|-------|---------|-------------|--------|
| 28/Apr/2026 | 1.0 | Dokument erstellt | Alex, Jona, Marcel |
| 06/May/2026 | 1.1 | Aktualisierung Metriken | Marcel |
| 29/Jun/2026 | 1.2 | Ergänzung Kennzahlen | Marcel |

## Inhaltsverzeichnis

- [Test Plan - Hexfields: Dominion - Version 1.2](#test-plan---hexfields-dominion---version-12)
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
  - [6. Kennzahlen aus finaler Präsentation (Handout)](#6-kennzahlen-aus-finaler-präsentation-handout)
    - [6.1 Backend (implementiert)](#61-backend-implementiert)
    - [6.2 Frontend (nicht implementiert)](#62-frontend-nicht-implementiert)
    - [6.3 Verwendung der Kennzahlen im Entwicklungsprozess](#63-verwendung-der-kennzahlen-im-entwicklungsprozess)

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

- Backend: Mindestens 80% Abdeckung mit Fokus auf:
  - Spiellogik, Regelverarbeitung
  - Authentifizierungssystem (Login, Logout, Token-Validierung)
  - Zugriffskontrolle
  - Lobbyverwaltung

- Frontend: Teststruktur vorgesehen, aktuell nicht implementiert
  - Zielbild: 10-20% Abdeckung als Einstieg
  - Fokus auf Grundmuster und Best Practices
  - v. A. Schnittstellenverhalten bei Authentifizierung zum Backend

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
| Backend | Gradle, JUnit, Mockito | Automatisierte Unit- und Integrationstests |
| Frontend | (geplant) Jest/Vitest + Testing Library | Teststruktur vorgesehen, aktuell noch nicht umgesetzt |

#### Test-Verwaltung

- Automatisierte Pipeline: GitHub Actions führt Backend-Tests bei jedem Push automatisch aus
- Lokale Entwicklung: Backend-Tests können lokal in IntelliJ/Gradle ausgeführt werden
- Fehlerbehandlung: Unkorrekte Commits können durch Revert von Main rückgängig gemacht werden
- Testausführung: Im Falle von unbehobenen Fehlern kann zu vorherigen Versionen zurückgegangen werden

### 3.2 Testtypen

#### 3.2.1 Function Testing

Spiellogik - Regelverarbeitung

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Die Spiellogik hält die festgelegten Spielregeln bei der Fortsetzung des Spielverlaufs ein | Verschiedene Eingabedaten decken verschiedene Szenarien ab (inkl. ungültiger Spielzüge) | Alle definierten Regeltests laufen erfolgreich durch | Fokus auf Kernregeln und Grenzfälle |

Benutzer-Authentifizierung

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Nutzer können sich mit einem Account oder als Gast authentifizieren | Szenarien mit validen/invaliden Access- und Refresh-Token | Alle Auth-Tests erfolgreich | Token-Lebenszyklus und Fehlerpfade werden mit geprüft |

#### 3.2.2 Security and Access Control Testing

Authentifizierter Spielzugriff (Application-Level Security)

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Nutzer können nur mit dem Spiel interagieren, solange sie authentifiziert sind | Trennung zwischen authentifizierten und unauthentifizierten Requests | Unautorisierte Zugriffe werden zuverlässig abgeblockt | Fokus auf API-/Endpoint-Schutz |

Lobbyadmin-Privilegien

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Nur Lobbyadmins dürfen Lobbyregeln ändern | Rollenspezifische Testfälle (Admin vs. Mitspieler) | Nicht-Admins können keine Admin-Aktionen ausführen | Berechtigungen auf Aktionsniveau geprüft |

#### 3.2.3 Configuration Testing

Lobbies - Maximale Anzahl

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Bestätigung, dass die festgelegte maximale Anzahl an existierenden Lobbies eingehalten wird | Wiederholtes Erstellen von Lobbies über die Grenze hinaus | Neue Lobbies werden oberhalb der Grenze blockiert | Grenzwertverhalten unter Last-sequenziellen Requests |

#### 3.2.4 User Interface Testing

Optionsmenü

| Testziel | Technik | Abschluss-Kriterium | Besonderheiten |
|---------|--------|-----------------|-----------------|
| Bestätigung, dass alle Nutzer das Optionsmenü öffnen können | Simuliertes Öffnen des Dialogs und Anwenden der Optionen | Optionsdialog öffnet und übernimmt Änderungen | Frontend-Tests hierfür aktuell noch nicht implementiert |

---

## 4. Ressourcen

### 4.1 Rollen

| Rolle | Verantwortlichkeiten |
|------|---------------------|
| Testleiter | Übersicht über Testplanung und -ausführung |
| Entwickler (Backend) | Implementierung und Pflege von Unit-/Integrationstests |
| Entwickler (Frontend) | Vorbereitung und spätere Implementierung der Frontend-Teststruktur |
| QA-Ingenieur | Durchführung von Integrationstests und Validierung |

### 4.2 Testumgebung

- Backend-Testumgebung: Lokale Gradle-Build-Umgebung mit JUnit
- Frontend-Testumgebung: vorgesehen, aktuell nicht in Betrieb
- Datenbank: Konfigurierte Test-Datenbank mit kontrollierten Daten
- Continuous Integration: GitHub Actions Pipeline für automatisierte Backend-Testausführung

---

## 5. Liefergegenstände

### Testmodelle und Testergebnisse

- Unit-Test-Suites für Backend-Komponenten
- Integrations-Test-Suites (Backend)
- Test-Berichte aus der GitHub CI/CD Pipeline

### Testausgaben

- Test-Logs vom automatisierten Build-Prozess
- Fehlerberichte mit Reproduktionsschritten
- Testabdeckungsberichte

### Defekt-Berichte

- Detaillierte Defektbeschreibungen mit Severity-Einstufung
- Reproduktionsschritte und erwartetes Verhalten
- Zugeordnete Prioritäten und Zuständige

---

## 6. Kennzahlen aus finaler Präsentation (Handout)

Die final präsentierten Kennzahlen werden als Referenz für den Projektabschluss und als Ausgangspunkt für Folgeiteration verwendet.

### 6.1 Backend (implementiert)

- Metrik-/Testauswertung basiert auf der CI-Ausführung mit:
  - `test`
  - `jacocoTestReport`
  - `sonar`
- Die [im Handout ausgewiesenen (siehe Abschnitt **e. Metriken**)](./presentation-final/SoftwareEngineering-Final-Handout.pdf) Backend-Werte (Teststatus, Coverage, Qualitätsindikatoren) sind maßgeblich für die Bewertung des aktuellen Stands. Mittlerweile beträgt die Testabdeckung etwa 50%, weiter steigend.
- Interpretation im Team erfolgt kombiniert:
  1. Testerfolg (grün/rot in CI),
  2. Coverage-Entwicklung (JaCoCo),
  3. Qualitätsbewertung (SonarQube/SonarCloud).

### 6.2 Frontend (nicht implementiert)

- [Im Handout ausgewiesene (siehe Abschnitt **e. Metriken**)](./presentation-final/SoftwareEngineering-Final-Handout.pdf) Frontend-Testkennzahlen sind als Ziel-/Planwerte zu lesen.
- Aktuell besteht keine operative Frontend-Testpipeline im selben Reifegrad wie im Backend.
- Für die nächste Ausbaustufe sind vorgesehen:
  - initiale Unit-/Komponententests,
  - schrittweise Erhöhung der Abdeckung,
  - Integration in die bestehende Frontend-CI.

### 6.3 Verwendung der Kennzahlen im Entwicklungsprozess

- Kennzahlen dienen als Entscheidungsgrundlage für:
  - Refactoring-Priorisierung,
  - Testausbau,
  - Freigabeentscheidungen vor Merge/Release.
- Änderungen an Metrik- oder Testzielen werden versionsgeführt in diesem Dokument nachgetragen.
