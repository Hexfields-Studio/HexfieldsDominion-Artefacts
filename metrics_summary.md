# Softwaremetriken - Hexfields: Dominion - Version 1.0

## Änderungsverzeichnis

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 29/Jun/2025 | 1.0 | Initiale Erstellung | Marcel |

## Inhaltsverzeichnis

- [Softwaremetriken - Hexfields: Dominion - Version 1.0](#softwaremetriken---hexfields-dominion---version-10)
  - [Änderungsverzeichnis](#änderungsverzeichnis)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Zweck des Dokuments](#zweck-des-dokuments)
  - [Betroffene Repositories](#betroffene-repositories)
  - [Geltungsbereich](#geltungsbereich)
  - [Backend-Repository `HexfieldsDominion-Backend`](#backend-repository-hexfieldsdominion-backend)
    - [Aktueller Implementierungsstand](#aktueller-implementierungsstand)
    - [Messkette in der CI](#messkette-in-der-ci)
    - [JaCoCo: Funktion](#jacoco-funktion)
    - [SonarQube/SonarCloud: Funktion](#sonarqubesonarcloud-funktion)
    - [Kombination JaCoCo + SonarQube (Pflichtpunkt)](#kombination-jacoco--sonarqube-pflichtpunkt)
    - [MetricsTree als manuelle Prüfung vor Commit (Pflichtpunkt)](#metricstree-als-manuelle-prüfung-vor-commit-pflichtpunkt)
    - [Kennzahlenbasis (finale Präsentation)](#kennzahlenbasis-finale-präsentation)
  - [Frontend-Repository `HexfieldsDominion`](#frontend-repository-hexfieldsdominion)
    - [Aktueller Implementierungsstand (Pflichtpunkt)](#aktueller-implementierungsstand-pflichtpunkt)
    - [Zielstruktur für spätere Metrikerhebung](#zielstruktur-für-spätere-metrikerhebung)
      - [Test-/Coverage-Erhebung](#test-coverage-erhebung)
      - [Statische Analyseplattform](#statische-analyseplattform)
      - [Kombinierte Qualitätsbewertung](#kombinierte-qualitätsbewertung)
      - [Manuelle Vorabprüfung](#manuelle-vorabprüfung)
    - [CI-Integrationspunkte (Zielbild)](#ci-integrationspunkte-zielbild)
  - [Hinweise für Entwickler](#hinweise-für-entwickler)

## Zweck des Dokuments

Dieses Dokument beschreibt die Erhebung und Verwendung der Softwaremetriken im Projekt.  
Fokus ist die bestehende Metrik-Pipeline im Backend sowie die vorgesehene (aber aktuell nicht implementierte) Struktur für das Frontend.

## Betroffene Repositories

- `Hexfields-Studio/HexfieldsDominion`: [Direkt-Link zur Pipeline](https://github.com/Hexfields-Studio/HexfieldsDominion/blob/main/.github/workflows/ci-cd.yml)
- `Hexfields-Studio/HexfieldsDominion-Backend`: [Direkt-Link zur Pipeline](https://github.com/Hexfields-Studio/HexfieldsDominion/blob/main/.github/workflows/ci-cd.yml)

## Geltungsbereich

Dokumentiert werden:
- automatisierte Metrikerhebung über CI im Backend,
- Zusammenspiel von JaCoCo und [SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main)/SonarCloud,
- manuelle Vorabprüfung mit MetricsTree vor Commit,
- vorgesehene Frontend-Struktur für eine spätere Metrik-Integration.

* * *

## Backend-Repository `HexfieldsDominion-Backend`

### Aktueller Implementierungsstand

Die Metrikerhebung ist im Backend aktiv implementiert und an die CI-Pipeline gekoppelt.

### Messkette in der CI

Workflow-Datei:
- `.github/workflows/gradle.yml`

Metrikrelevanter Build-/Testschritt:
- `./gradlew test jacocoTestReport sonar --info`

Dadurch werden in einem Lauf:
1. Unit-/Integrationstests ausgeführt,
2. JaCoCo-Coverage erzeugt,
3. [SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main)/SonarCloud-Analyse durchgeführt.

### JaCoCo: Funktion

JaCoCo ist im Backend die technische Quelle für Coverage-Metriken.

Erzeugte Kerninformationen:
- Instruction-/Line-/Branch-Coverage (abhängig von Konfiguration im Build),
- Report-Ausgabe unter `build/reports/jacoco/`,
- CI-Artefakt-Upload für Nachvollziehbarkeit.

Zweck im Entwicklungsprozess:
- objektive Aussage über Testabdeckung,
- Erkennung ungetesteter oder schwach getesteter Codebereiche,
- Grundlage für Quality Gates und Trendbeobachtung.

### SonarQube/SonarCloud: Funktion

[SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main)/SonarCloud verarbeitet statische Code- und Qualitätsmetriken.

Verwendete Funktionen im Projektkontext:
- Analyse von Code Smells und Maintainability-Risiken,
- Erkennung von Sicherheits- und Reliability-Auffälligkeiten (regelbasiert),
- Auswertung im PR-/Branch-Kontext.

Zweck im Entwicklungsprozess:
- kontinuierliche Sichtbarkeit von Qualitätszustand und technischer Schuld,
- standardisierte Qualitätsbewertung über Commits/PRs hinweg.

### Kombination JaCoCo + SonarQube (Pflichtpunkt)

Die Kombination ist zentraler Bestandteil der aktuellen Backend-Metrikstrategie:

- **JaCoCo** liefert die messbare Testabdeckung (Coverage-Quelle).
- **[SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main)/SonarCloud** konsumiert/korreliert diese Coverage mit statischer Analyse.
- Ergebnis ist eine einheitliche Qualitätsansicht, in der Testabdeckung und Codequalität gemeinsam bewertet werden.

Entwicklernutzen:
- Coverage-Werte stehen nicht isoliert, sondern im Kontext von Code Smells/Bugs/Vulnerabilities.
- Entscheidungen zu Refactoring, Testausbau und Merge-Freigaben können konsistent getroffen werden.
- PR-Reviews erhalten eine datenbasierte Grundlage statt rein subjektiver Einschätzung.

### MetricsTree als manuelle Prüfung vor Commit (Pflichtpunkt)

Zusätzlich zur CI wird **MetricsTree** als manuelle Vorabprüfung vor Commit eingesetzt.

Rolle im Ablauf:
1. Lokale Entwicklung und Änderung.
2. Manuelle MetricsTree-Prüfung vor Commit.
3. Commit/Push.
4. CI-Validierung über JaCoCo + [SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main).

Zweck:
- frühe Erkennung auffälliger Struktur-/Komplexitätsindikatoren,
- Vermeidung, dass offensichtliche Qualitätsprobleme erst in CI sichtbar werden,
- schnellere Feedbackschleife für Entwickler vor Übergabe in PR/CI.

Hinweis:
- MetricsTree ist als **manuelles Qualitäts-Gate vor Commit** zu verstehen, nicht als Ersatz für die CI-Metriken.

### Kennzahlenbasis (finale Präsentation)

Die in der finalen Präsentation dargestellten Backend-Metriken bilden die Referenz für:
- Bewertung des aktuellen Qualitätsstands,
- Vergleich über Zeitpunkte/Sprints,
- Ableitung von Maßnahmen (z. B. Testausbau, Refactoring-Prioritäten).

Für die operative Entwicklung werden diese Kennzahlen inhaltlich aus:
- JaCoCo-Reports,
- [SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main)/SonarCloud-Auswertungen,
- und der lokalen MetricsTree-Prüfung
zusammengeführt und interpretiert.

* * *

## Frontend-Repository `HexfieldsDominion`

### Aktueller Implementierungsstand (Pflichtpunkt)

Für das Frontend ist aktuell **keine Softwaremetrikerhebung implementiert**.  
Die folgende Struktur ist als Zielbild dokumentiert, um eine spätere Integration konsistent vorzubereiten.

### Zielstruktur für spätere Metrikerhebung

#### Test-/Coverage-Erhebung

Geplantes Äquivalent zu JaCoCo im Frontend:
- Testlauf mit Coverage-Ausgabe (z. B. über das verwendete Frontend-Test-Framework),
- standardisierte Report-Generierung in CI.

#### Statische Analyseplattform

Geplantes Äquivalent zur Backend-Sonar-Auswertung:
- Anbindung an [SonarQube](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main)/SonarCloud für TypeScript/Frontend-Regeln,
- Einbindung in PR- und Branch-Analyse.

#### Kombinierte Qualitätsbewertung

Ziel ist dieselbe Bewertungslogik wie im Backend:
- Coverage + statische Analyse in einer gemeinsamen Qualitätsansicht,
- Nutzung als Grundlage für Review- und Merge-Entscheidungen.

#### Manuelle Vorabprüfung

Analog zum Backend soll vor Commit eine manuelle Metrik-/Strukturprüfung vorgesehen werden (entsprechendes Frontend-Werkzeug bzw. definierter Prüfschritt).

### CI-Integrationspunkte (Zielbild)

Die Frontend-Metriken sollen in die bestehende Frontend-CI so integriert werden, dass:
- Build/Test weiterhin deterministisch laufen,
- Metrikartefakte erzeugt und verfügbar gemacht werden,
- einheitliche Qualitätskriterien zu Backend-Metriken hergestellt werden.

* * *

## Hinweise für Entwickler

- Änderungen an Metrikdefinitionen (Schwellenwerte, Gates, Reports) sind versionsgeführt in diesem Dokument nachzupflegen.
- Metrikänderungen im Backend müssen mit CI-Workflow und Sonar-Konfiguration konsistent sein.
- Bei Einführung der Frontend-Metriken sind die Abschnitte „Aktueller Implementierungsstand“ und „Zielstruktur“ auf „implementiert“ umzustellen und mit konkreten Jobs/Kommandos zu belegen.