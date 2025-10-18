# **Hexfields: Dominion**

# **Software Requirements Specification (SRS)**

# **Version 1.0**

**Revision History**

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 18/Oct/2025 | 1.0 | Initial version of SRS document | A., J., M. |

## **Inhaltsverzeichnis**

- [**Hexfields: Dominion**](#hexfields-dominion)
- [**Software Requirements Specification (SRS)**](#software-requirements-specification-srs)
- [**Version 1.0**](#version-10)
  - [**Inhaltsverzeichnis**](#inhaltsverzeichnis)
  - [1. Einleitung](#1-einleitung)
    - [1.1 Zweck](#11-zweck)
    - [1.2 Umfang](#12-umfang)
    - [1.3 Definitionen, Akronyme und Abkürzungen](#13-definitionen-akronyme-und-abkürzungen)
    - [1.4 Referenzen](#14-referenzen)
    - [1.5 Übersicht](#15-übersicht)
  - [2. Allgemeine Beschreibung](#2-allgemeine-beschreibung)
    - [2.1 Vision](#21-vision)
    - [2.2 Use-Case-Diagramm](#22-use-case-diagramm)
    - [2.3 Technologie-Stack](#23-technologie-stack)
    - [2.4 Teamstruktur und Verantwortlichkeiten](#24-teamstruktur-und-verantwortlichkeiten)
  - [3. Spezifische Anforderungen](#3-spezifische-anforderungen)
    - [3.1 Funktionalität](#31-funktionalität)
      - [Phase 1 (bis Dezember) - Kernfunktionalität:](#phase-1-bis-dezember---kernfunktionalität)
      - [Phase 2 (bis Juni) - Erweiterungen:](#phase-2-bis-juni---erweiterungen)
    - [Funktionale Anforderungen im Detail:](#funktionale-anforderungen-im-detail)
    - [3.2 Benutzbarkeit](#32-benutzbarkeit)
      - [3.2.1 Intuitive Bedienung](#321-intuitive-bedienung)
      - [3.2.2 Responsive Design](#322-responsive-design)
    - [3.3 Zuverlässigkeit](#33-zuverlässigkeit)
      - [3.3.1 Verfügbarkeit](#331-verfügbarkeit)
      - [3.3.2 Datenkonsistenz](#332-datenkonsistenz)
    - [3.4 Leistung](#34-leistung)
      - [3.4.1 Antwortzeiten](#341-antwortzeiten)
      - [3.4.2 Gleichzeitige Benutzer](#342-gleichzeitige-benutzer)
    - [3.5 Wartbarkeit](#35-wartbarkeit)
      - [3.5.1 Clean Code Standards](#351-clean-code-standards)
      - [3.5.2 Testabdeckung](#352-testabdeckung)
    - [3.6 Design-Einschränkungen](#36-design-einschränkungen)
      - [Unterstützte Browser](#unterstützte-browser)
      - [Architektur](#architektur)
    - [3.7 Online-Benutzerdokumentation und Hilfesystem](#37-online-benutzerdokumentation-und-hilfesystem)
    - [3.8 Gekaufte Komponenten](#38-gekaufte-komponenten)
    - [3.9 Schnittstellen](#39-schnittstellen)
      - [3.9.1 Benutzerschnittstellen](#391-benutzerschnittstellen)
      - [3.9.2 Software-Schnittstellen](#392-software-schnittstellen)
      - [3.9.3 Kommunikationsschnittstellen](#393-kommunikationsschnittstellen)
    - [3.10 Lizenzanforderungen](#310-lizenzanforderungen)
    - [3.11 Rechtliche Hinweise, Urheberrecht und Sonstiges](#311-rechtliche-hinweise-urheberrecht-und-sonstiges)
    - [3.12 Anwendbare Standards](#312-anwendbare-standards)
  - [4. Unterstützende Informationen](#4-unterstützende-informationen)

## 1. Einleitung

### 1.1 Zweck

Diese Software-Anforderungsspezifikation dient zur Definition von funktionellen und nicht funktionellen Anforderungen für das Videospiel “Hexfields: Dominion”. Dieses Dokument ist für die Entwickler, es bietet eine detaillierte Beschreibung des Spielverhaltens und des Designs, um ein gemeinsames Verständnis des Systems zu gewährleisten.

*(Dieses SRS beschreibt das vollständige externe Verhalten der Anwendung sowie nicht-funktionale Anforderungen, Design-Einschränkungen und andere Faktoren für eine umfassende Beschreibung der Software-Anforderungen.)*

### 1.2 Umfang

Das Projekt wird als responsive Webanwendung realisiert, die in modernen Browsern läuft.

**Akteure der Anwendung:**

- Spieler (registriert und als Gast)
- Lobby-Ersteller (Host)
- Administratoren

**Geplante Subsysteme:**

- **Account-Management**: Registrierung, Login, Gast-Zugang, Profilverwaltung
- **Lobby-Management**: Spielerstellung, Beitritt per Code, Rollenzuweisung, Spielstart
- **Spiel-Engine**: Vollständige Implementierung der Siedler von Catan Spielmechaniken
- **Echtzeit-Kommunikation**: Datenaustausch zwischen Frontend und Backend während des Spiels
- **Benutzeroberfläche**: Responsive UI mit Spielfeld, Spielzustandsanzeige und Menüsystem

*(Dieser Abschnitt beschreibt die Software-Anwendung, auf die sich das SRS bezieht, sowie alle Teilsysteme und Funktionen, die von diesem Dokument betroffen oder beeinflusst werden.)*

### 1.3 Definitionen, Akronyme und Abkürzungen

| Bezeichnung | Definition |
| :---- | :---- |
| Ressource | Ein Material, das für den Bau von verschiedenen Gebäuden zwischen den Feldern benötigt wird. |
| Gebäude | Verschiedene Gebäude können zwischen den Feldern gebaut werden. Sie ermöglichen es, Ressourcen zu erhalten. |
| Bank | Die Bank ist eine Funktion des Spiels, über die ein Spieler Ressourcen tauschen kann. *Spieler können immer mit der Bank handeln, allerdings mit relativ hohen Kosten. Durch das Bauen von mindestens einem Hafen können diese gesenkt werden.* |
| Feld | Eine Hexagon-förmige Fläche, die visuell eine Ressource darstellt und diese auch an einen Spieler abgeben kann. Jedes individuelle Feld besitzt von Spielbeginn an eine Zahl. |
| Spielfeld | Das Spielfeld besteht aus mehreren Feldern, die direkt aneinander zusammengefügt sind.  |
| Siegespunkt | Ein Spieler sammelt Siegpunkte durch das Erreichen von Zielen. Erreicht ein Spieler eine festgelegte Anzahl von Siegespunkten, gewinnt dieser das Spiel. |
| Ziel | Jedes Ziel besteht aus einer festgelegten Bedingung. Solange ein Spieler diese Bedingung erfüllt, erhält er für das Erreichen des Ziels Siegpunkte. |
| Entwicklung |  |
| Modus | Ein Satz von Spielregeln, es wird beim Starten des Spiels ausgewählt. |
| Würfeln | Jeder Spieler würfelt automatisch am Anfang seines Zuges zwei Würfel. |
| Spielzugphase | Einteilung der aufeinander folgenden Aktionen, wenn ein neuer Spieler am Zug ist. Es gibt die Ertrags-, Handels- und Bauphase. |

*( Dieses Unterkapitel sollte Definitionen aller Begriffe, Akronyme und Abkürzungen enthalten, die zum korrekten Verständnis des SRS erforderlich sind.)*

### 1.4 Referenzen

| Titel | Änderungsdatum | Organisation |
| :---- | :---- | :---- |
| [GitHub Organisation & Blog](https://github.com/Hexfields-Studio) | 18/Oct/2025 | Hexfields Studio |
| [GitHub Repository: Frontend](https://github.com/Hexfields-Studio/HexfieldsDominion) | 18/Oct/2025 | Hexfields Studio |
| [GitHub Repository: Backend](https://github.com/Hexfields-Studio/HexfieldsDominion-Backend) | 18/Oct/2025 | Hexfields Studio |
| [GitHub Repository: Artefakte](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts) | 18/Oct/2025 | Hexfields Studio |
| [GitHub Pages: Webseite](https://hexfields-studio.github.io/HexfieldsDominion/) | 18/Oct/2025 | Hexfields Studio |

*(Enthält eine vollständige Liste aller im SRS referenzierten Dokumente mit Titel, Datum und herausgebender Organisation.)*

### 1.5 Übersicht

 Kapitel 2 bietet eine allgemeine Beschreibung des Projekts mit Vision, Teamstruktur und Technologie-Stack. Das dritte Kapitel (Spezifische Anforderungen) liefert detaillierte Informationen zu den funktionalen und nicht-funktionalen Anforderungen, strukturiert nach den beiden Entwicklungsphasen.

*(Beschreibt den Inhalt und die Organisation des restlichen SRS-Dokuments.)*

## 2. Allgemeine Beschreibung

### 2.1 Vision

“Hexfields: Dominion” ist ein eigenständiges Videospiel im Multiplayer, das im Webbrowser spielbar ist und somit auf jedem Computer und Smartphone erreichbar ist. Das Spiel soll eine im Web spielbare und leicht zugängliche Alternative zum Brettspiel “Siedler von Catan” sein. Matches zwischen Spielern können sowohl in Echtzeit als auch zugabhängig stattfinden. Dabei bieten sie neben zufälligen Mitspielern auch die Möglichkeit, mit Freunden zusammen zu spielen. Bekannte Bestandteile des originalen Spiels werden mit erweiterten Funktionen und Mechaniken (Mods) ergänzt, sodass ein neues Spielerlebnis entsteht.

*(Dieser Abschnitt beschreibt die allgemeinen Faktoren, die das Produkt und seine Anforderungen beeinflussen, ohne spezifische Anforderungen zu nennen.)*

### 2.2 Use-Case-Diagramm

*[Use-Case-Diagramm des gesamten Projekts](./use_case_gesamt.jpg "Use-Case-Diagramm des gesamten Projekts")*

- **Grün (Phase 1 - bis Dezember):** Account Management, Lobby Management, Kern-Spielmechaniken
- **Gelb (Phase 2 - bis Juni):** Erweiterte UI-Komponenten, Mods, Skins, Design-Verbesserungen

### 2.3 Technologie-Stack

**Frontend:**

- React mit TypeScript
- Vite als Build-Tool
- bun/npm als Package Manager

**Backend:**

- Java mit Spring Framework
- Gradle als Build-Tool
- PostgreSQL als Datenbank

**Development & Operations:**

- IDEs: IntelliJ IDEA & VSCode
- Versionsverwaltung: Git/GitHub/GH-Pages
- Projektmanagement: Jira mit Scrum-Methodik
- Testing: JUnit, React Testing Library

### 2.4 Teamstruktur und Verantwortlichkeiten

| Teammitglied | Rolle | Verantwortlichkeiten |
|--------------|-------|---------------------|
| **Alex ([A1exHorst](https://github.com/A1exHorst))** | Product Owner & Frontend Lead | Feature-Priorisierung, Frontend-Architektur, React-Entwicklung |
| **Jona ([JaskerX](https://github.com/JaskerX))** | Use-Case-Reviewer & Backend Lead | Backend-Architektur, Technical Design, Java Spring Entwicklung |
| **Marcel ([ultra-ms](https://github.com/ultra-ms))** | Scrum Master & Infrastructure | Projektorganisation, DevOps, Support, Dokumentation |

## 3. Spezifische Anforderungen

*(Kein Text hier!)*

~~Wenn ein Spieler in einem Match leaved (ohne Account) dann kann er nur wieder mit dem Lobbycode beitreten. Der Leader des Spiels muss diesem Spieler dann wieder zuweisen, wer er war. Wenn der Leader leaved dann wird immer irgendjemand random die Leader Rolle bekommen (Lobby und im Match)~~

*(Dieses Kapitel enthält alle Software-Anforderungen in ausreichender Detailtiefe, um Designern das Entwerfen und Testern das Testen des Systems zu ermöglichen. Es umfasst funktionale und nicht-funktionale Anforderungen.)*

### 3.1 Funktionalität

*(Beschreibt die funktionalen Anforderungen des Systems, einschließlich Funktionsumfang, Fähigkeiten und Sicherheitsaspekte.)*

#### Phase 1 (bis Dezember) - Kernfunktionalität:

- **3.1.1 Account Management**  
  Registrierung, Login, Gast-Zugang, Logout
- **3.1.2 Lobby Management**  
  Lobby erstellen, per Code beitreten, Rollenzuweisung, Spiel starten
- **3.1.3 Spielmechaniken**  
  Vollständige Implementierung der klassischen Siedler von Catan Regeln
- **3.1.4 Grundlegende UI**  
  Funktionales Spielfeld, Ressourcenverwaltung, Bau-Interface

#### Phase 2 (bis Juni) - Erweiterungen:

- **3.1.5 Erweiterte UI-Komponenten**  
  Pause-Menu, Home Page, verbesserte Match Page
- **3.1.6 Mods-System**  
  Doppeltes Würfeln, Völker-Fähigkeiten, Riesiges Spielfeld
- **3.1.7 Skins**  
  Antike, Moderne, Magisch, Fliegende Inseln
- **3.1.8 Design-Verbesserungen**  
  Light/Dark Mode, verbesserte Visualisierung

### Funktionale Anforderungen im Detail:

```
(((ich weiß noch nicht wie wir das hier richtig implementieren)))

2. ### **Account Management**

   1. ### *…*

   3. ### **Lobby Management**

      1. ### *…*

   4. **User Interface**

      1. ### *Laufendes Match*

Durch das Drücken der Starttaste in einer Lobby wird ein Match gestartet. Das Match wird nur durch den Sieg eines Spielers oder nach Abstimmung der Spieler beendet.

1. ### *Spielzug ausführen*

Am Anfang jedes Spielzuges würfelt der Spieler automatisch zwei Paare. Danach kann der Spieler eine von beiden Paaren auswählen. Sei die Summe S aus dem ausgewählten Paar. Alle Spieler, die ein ressourcen-sammelndes Gebäude an einem Feld mit der Zahl S gebaut haben, erhalten nun die Ressource des Feldes.  
Nach dem Austeilen der Ressourcen darf der Spieler Gebäude bauen, mit Spielern oder unter bestimmten Bedingungen mit der Bank handeln. Der Spielzug wird nach dem Befehl des Spielers oder nach dem Ablauf einer festgelegten Zeit automatisch beendet.

[Link zu github]

2. ### *Aktualisierung und Datenaustausch*

Jede Aktion, die den Spielstand verändert, muss an alle beteiligten Spielern mitgesendet werden, sodass auch ihre Clients die Änderung anzeigen können.

2. ### **Match-Page**

Die **Match-Page** umfasst alle Funktionen, die das eigentliche Spielen ermöglichen.

1. ### *Spielfeld*

Das Spielfeld besteht aus aneinander gereihten Feldern. Jeder Spieler muss einen Überblick über das Spielfeld haben können.

2. ### *Spielstand Anzeige*

Die **Spielfeld Anzeige** umfasst alle visuellen Funktionen, die einem Spieler helfen sich über den Spielstand zu informieren und um bei einem Spielzug gewisse Aktionen wie das Handeln ausführen zu können.

3. ### *Spiel-Menü (ESC)*

Ähnlich wie bei jedem Videospiel soll das **Spiel-Menü** das Einstellen vom Spielerlebnis wie z. B. das Toggeln von Light/Dark-Mode ermöglichen.
```

### 3.2 Benutzbarkeit

*(Enthält Anforderungen zur Benutzerfreundlichkeit, wie z.B. Einarbeitungszeit, Aufgabenzeiten für typische Aufgaben und Konformität mit Usability-Standards.)*

*BEISPIELTEXT*
#### 3.2.1 Intuitive Bedienung
Die Oberfläche orientiert sich am originalen Brettspiel-Design, um erfahrenen Siedler-Spielern ein vertrautes Gefühl zu vermitteln.
#### 3.2.2 Responsive Design
Die Webanwendung ist auf Desktop- und Mobilgeräten gleichermaßen gut bedienbar.

### 3.3 Zuverlässigkeit

*(Kein Text hier!)*

*(Spezifiziert Anforderungen zur Systemzuverlässigkeit, einschließlich Verfügbarkeit, MTBF, MTTR, Genauigkeit und Fehlerraten.)*

*BEISPIELTEXT*
#### 3.3.1 Verfügbarkeit
Der Server soll zu 95% der Zeit verfügbar sein. Wartungsarbeiten werden außerhalb der Hauptspielzeiten durchgeführt.
#### 3.3.2 Datenkonsistenz
Spielstände werden regelmäßig gesichert. Bei Verbindungsabbrüchen können Spieler an ihre vorherige Position zurückkehren.

### 3.4 Leistung

*(Kein Text hier!)*

*(Beschreibt die Leistungsmerkmale des Systems, einschließlich Antwortzeiten, Durchsatz, Kapazität und Ressourcennutzung.)*

*BEISPIELTEXT*
#### 3.4.1 Antwortzeiten
Spielaktionen werden innerhalb von 1 Sekunde visualisiert. Ladezeiten für neue Spiele unter 3 Sekunden.
#### 3.4.2 Gleichzeitige Benutzer
Das System unterstützt mindestens 200 gleichzeitige Benutzer und 50 parallel laufende Spiele.

### 3.5 Wartbarkeit

*(Kein Text hier!)*

*(Enthält Anforderungen zur Verbesserung der Wartbarkeit, einschließlich Coding-Standards, Namenskonventionen und Wartungszugang.)*

*BEISPIELTEXT*
#### 3.5.1 Clean Code Standards
Verwendung etablierter TypeScript und Java Coding Conventions. Regelmäßige Code-Reviews im Team.
#### 3.5.2 Testabdeckung
Mindestens 80% Testabdeckung im Backend, 70% im Frontend. Wöchentliche Integrationstests.

### 3.6 Design-Einschränkungen

*(Kein Text hier!)*

*(Zeigt Design-Einschränkungen auf, die eingehalten werden müssen, wie z.B. Programmiersprachen, Entwicklungsprozesse oder Architekturbeschränkungen.)*

*BEISPIELTEXT*
#### Unterstützte Browser
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
#### Architektur
- Frontend: React mit TypeScript
- Backend: Java Spring mit REST-API
- Datenbank: PostgreSQL
- Responsive Web Design für Mobile Compatibility

### 3.7 Online-Benutzerdokumentation und Hilfesystem

*(Beschreibt die Anforderungen für Online-Benutzerdokumentation, Hilfesysteme und Hilfebanner.)*

*"Integriertes Regelwerk und FAQ für neue Spieler. Context-sensitive Hilfe in der Spieloberfläche."*

### 3.8 Gekaufte Komponenten

*(Beschreibt gekaufte Komponenten, Lizenzbeschränkungen und Kompatibilitätsstandards.)*

*"Aktuell sind keine gekauften Komponenten geplant."*

### 3.9 Schnittstellen

*(Definiert die von der Anwendung zu unterstützenden Schnittstellen mit ausreichender Spezifität für Entwicklung und Verifikation.)*

#### 3.9.1 Benutzerschnittstellen

*BEISPIELTEXT*
- **Home Page**: Lobby-Übersicht und Spielerstellung
- **Match Page**: Spielfeld, Spielzustandsanzeige, Spielmenü (ESC)
- **Account Management**: Login, Registrierung, Profilverwaltung
- **Pause Menu**: Spielunterbrechung mit Fortsetzungsoption

#### 3.9.2 Software-Schnittstellen

*BEISPIELTEXT*
RESTful API zwischen Frontend und Backend, WebSockets für Echtzeit-Spielupdates.

#### 3.9.3 Kommunikationsschnittstellen

*BEISPIELTEXT*

HTTPS für API-Kommunikation, WebSockets für Echtzeit-Spielzustandsupdates.

### 3.10 Lizenzanforderungen

*(Definiert Lizenzdurchsetzungsanforderungen und Nutzungsbeschränkungen.)*

*BEISPIELTEXT*
Die Anwendung wird als Educational Project unter MIT-Lizenz veröffentlicht.

### 3.11 Rechtliche Hinweise, Urheberrecht und Sonstiges

*(Beschreibt rechtliche Haftungsausschlüsse, Garantien, Urheberrechtshinweise und Markenkonformität.)*

*BEISPIELTEXT*
Dieses Projekt ist ein Bildungsprojekt und nicht kommerziell. "Siedler von Catan" ist eine Marke von Asmodee. Das Logo von Hexfields: Dominion ist Eigentum des Entwicklungsteams.

### 3.12 Anwendbare Standards

*(Beschreibt per Referenz alle anwendbaren Standards und deren spezifische Abschnitte.)*

*BEISPIELTEXT*
- Web Standards des W3C
- WCAG 2.1 Level AA für Barrierefreiheit
- REST-API Best Practices
- Clean Code Principles

## 4. Unterstützende Informationen

*(Enthält Inhaltsverzeichnis, Index und Anhänge wie Use-Case-Storyboards oder UI-Prototypen.)*

*BEISPIELTEXT*
Für weitere Informationen kontaktieren Sie das Hexfields Studio Team oder besuchen Sie unsere [GitHub Organisation](https://github.com/Hexfields-Studio).

*Dokument basierend auf dem Rational Unified Process SRS-Template und IEEE 830-1998 Standard.*
