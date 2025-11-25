# Software Architecture Document (SAD) - Hexfields: Dominion - Version 1.0

## Versionsübersicht

| Date | Version | Description | Author |
| ----- | ----- | ----- | ----- |
| 25/Nov/2025 | 1.0 | Dokument erstellt | Alex, Jona, Marcel |

## Inhaltsverzeichnis

- [Software Architecture Document (SAD) - Hexfields: Dominion - Version 1.0](#software-architecture-document-sad---hexfields-dominion---version-10)
  - [Versionsübersicht](#versionsübersicht)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [1. Einleitung](#1-einleitung)
    - [1.1 Zweck](#11-zweck)
    - [1.2 Umfang](#12-umfang)
      - [Akteure der Anwendung](#akteure-der-anwendung)
      - [Geplante Subsysteme](#geplante-subsysteme)
    - [1.3 Definitionen, Akronyme und Abkürzungen](#13-definitionen-akronyme-und-abkürzungen)
    - [1.4 Referenzen](#14-referenzen)
    - [1.5 Überblick](#15-überblick)
  - [2. Architektonische Repräsentation](#2-architektonische-repräsentation)
  - [3. Architektonische Ziele und Einschränkungen](#3-architektonische-ziele-und-einschränkungen)
    - [3.1 Lobby-Code in der URL](#31-lobby-code-in-der-url)
    - [3.2 Match-UUID in der URL](#32-match-uuid-in-der-url)
    - [3.3 Speichern von Ressourcen und Baurezepten](#33-speichern-von-ressourcen-und-baurezepten)
    - [3.4 Heatbeat](#34-heatbeat)
    - [3.5 Caching von Anmeldetoken](#35-caching-von-anmeldetoken)
    - [3.6 Arbeitsspeicher-Management von Matches](#36-arbeitsspeicher-management-von-matches)
    - [3.7 Tauschgeschäft-Spielzug](#37-tauschgeschäft-spielzug)
  - [4. Use-Case-Ansicht](#4-use-case-ansicht)
  - [5. Logische Ansicht](#5-logische-ansicht)
    - [5.1 Übersicht](#51-übersicht)
    - [5.2 Architektonisch Signifikante Designpakete](#52-architektonisch-signifikante-designpakete)
    - [5.3 Use-Case-Realisierungen](#53-use-case-realisierungen)
  - [6. Prozessansicht](#6-prozessansicht)
    - [Sequenzdiagramm: Login](#sequenzdiagramm-login)
    - [Sequenzdiagramm: Logout](#sequenzdiagramm-logout)
    - [Sequenzdiagramm: Passwort-Reset](#sequenzdiagramm-passwort-reset)
    - [Sequenzdiagramm: Registration](#sequenzdiagramm-registration)
    - [Sequenzdiagramm: Match Starten](#sequenzdiagramm-match-starten)
    - [Sequenzdiagramm: Lobby erstellen](#sequenzdiagramm-lobby-erstellen)
    - [Sequenzdiagramm: Lobby beitreten](#sequenzdiagramm-lobby-beitreten)
    - [Sequenzdiagramm: Light/Dark Mode](#sequenzdiagramm-lightdark-mode)
    - [Sequenzdiagramm: Start Menü](#sequenzdiagramm-start-menü)
  - [7. Einsatzansicht](#7-einsatzansicht)
  - [8. Implementationsansicht](#8-implementationsansicht)
  - [9. Datenansicht *(optional)*](#9-datenansicht-optional)
  - [10. Größe und Leistung](#10-größe-und-leistung)
  - [11. Qualität](#11-qualität)
  - [12. Unterstützende Informationen](#12-unterstützende-informationen)

## 1. Einleitung

### 1.1 Zweck

Dieses Dokument bietet einen umfassenden architektonischen Überblick des Videospiels „Hexfields: Dominion“. Es stellt die wesentlichen architektonischen Entscheidungen dar und dient den Entwicklern als Grundlage für ein gemeinsames Verständnis des Systems.

### 1.2 Umfang

Das Projekt wird als responsive Webanwendung realisiert, die in modernen Browsern läuft.

#### Akteure der Anwendung

- Spieler (registriert und als Gast)
- Lobby-Ersteller (Host)
- Administratoren

#### Geplante Subsysteme

- Account-Management: Registrierung, Login, Gast-Zugang, Profilverwaltung
- Lobby-Management: Spielerstellung, Beitritt per Code, Rollenzuweisung, Spielstart
- Spiel-Engine: Vollständige Implementierung der Siedler von Catan Spielmechaniken
- Echtzeit-Kommunikation: Datenaustausch zwischen Frontend und Backend während des Spiels
- Benutzeroberfläche: Responsive UI mit Spielfeld, Spielzustandsanzeige und Menüsystem

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

### 1.4 Referenzen

| Titel | Änderungsdatum | Organisation |
| :---- | :---- | :---- |
| [GitHub Organisation & Blog](https://github.com/Hexfields-Studio) | Nov/2025 | Hexfields Studio |
| [GitHub Repository: Frontend](https://github.com/Hexfields-Studio/HexfieldsDominion) | Nov/2025 | Hexfields Studio |
| [GitHub Repository: Backend](https://github.com/Hexfields-Studio/HexfieldsDominion-Backend) | Nov/2025 | Hexfields Studio |
| [GitHub Repository: Artefakte](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts) | Nov/2025 | Hexfields Studio |
| [GitHub Pages: Webseite](https://hexfields-studio.github.io/HexfieldsDominion/) | Nov/2025 | Hexfields Studio |

### 1.5 Überblick

Dieses Dokument enthält die Architekturanalyse und erläutert die begründeten Entscheidungen für die Implementierung der Systemkomponenten. Das dritte Kapitel bietet eine detaillierte Darstellung der verschiedenen Architekturansichten, um die wesentlichen Strukturen und Abläufe des Systems zu veranschaulichen.

## 2. Architektonische Repräsentation

*[This section describes what software architecture is for the current system, and how it is represented. Of the Use-Case, Logical, Process, Deployment, and Implementation Views, it enumerates the views that are necessary, and for each view, explains what types of model elements it contains.]*

## 3. Architektonische Ziele und Einschränkungen

### 3.1 Lobby-Code in der URL

Eine URL, die zu einer Spiel-Lobby führt, hat den Aufbau [https://hexfields-studio.github.io/HexfieldsDominion/lobby/[Lobbycode]](https://hexfields-studio.github.io/HexfieldsDominion/lobby/ABE431E). So ist ein einfacher Beitritt zur Lobby möglich, da lediglich der Lobbycode benötigt wird. Die Daten sind außerdem konsistent, sodass sowohl beim Beitritt per Lobbycode im Startmenü als auch durch Klicken auf einen geteilten Link zu einer Lobby mit denselben Werten gearbeitet wird. Damit wird möglicher Verwirrung von Spielern vorgebeugt, die beispielsweise den Code aus der Url kopieren, der dann aber gar kein gültiger Lobbycode wäre. Da die Lobbies bereits fest bestehen und bei der Erstellung nur neu vergeben werden, haben sie eine feste ID. Das macht es notwendig, den Code als zusätzliche Spalte in die Datenbank und als zusätzliche Attribute zu Lobbyobjekten im Backend hinzuzufügen. Das ist auch ein Grund dafür, dass in der URL die Lobby anders als mit deren ID identifiziert werden muss. Andernfalls könnten kurze IDs wie “1” unästhetisch wirken.

### 3.2 Match-UUID in der URL

Eine Match URL hat den Aufbau [https://hexfields-studio.github.io/HexfieldsDominion/match/[uuid]](https://hexfields-studio.github.io/HexfieldsDominion/match/asdf-asdf-asdf-asdf). Mithilfe der UUID kann ein Match eindeutig identifiziert werden. Da die Ids für Matches komplett unabhängig gewählt werden können, war die Idee, die URL mit einer längeren ID, hier einer UUID, ästhetischer und professioneller wirken zu lassen. Da die Spieler über die Lobby beitreten können und höchstens die gesamte Match URL zum Teilen kopieren, ist es hierbei auch nicht notwendig, dass die ID besonders einfach abzutippen o.ä. ist.

### 3.3 Speichern von Ressourcen und Baurezepten

Ressourcen und Baurezepte von Gebäuden werden in einer Datenbank gespeichert. Da diese, z.B. zum Balancing, hin und wieder angepasst werden müssen, sollten sie aus dem Code ausgelagert werden. Außerdem können auf diese Weise auch Ressourcen ohne Programmierkenntnisse hinzugefügt und in Baurezepten genutzt werden. Dabei muss beachtet werden, dass diese Daten aus der Datenbank mit dem Backend gleichwertig gehalten werden müssen. Möglicherweise ist hierzu ein Neustart des Backends oder ein Neuladen der Datenbank durch das Backend erforderlich.

### 3.4 Heatbeat

Clients sollen alle 5 Sekunden einen Heartbeat, der ihr Anmeldetoken beinhaltet, an das Backend senden, sodass das Backend erkennen kann, ob ein Client noch aktiv am Spiel beteiligt ist. Diese Information ist für das Schonen von Ressourcen relevant, weil dadurch z. B. Lobbies freigelegt oder Matches gelöscht werden können, in denen sowieso kein Client aktiv ist. Auf der Seite des Backends sollen also Account-Objekte, die für eine Zeit von 10 Sekunden keinen Heartbeat erhalten haben, gelöscht werden.

### 3.5 Caching von Anmeldetoken

Zusätzlich verwaltet das Backend eine Hashmap, in der die öffentliche IP-Adresse eines Clients mit dem zuletzt über ein Heartbeat gesendeten Anmeldetoken verbunden wird. Wenn ein Client erneut einen Heartbeat sendet, kann mithilfe der Hashmap schnell sichergestellt werden, dass ein Client nur Heartbeats für einen Account sendet. Somit wird ein potenzieller Angriffsvektor vermieden, in dem sich ein Client z. B. mehrere Gastzugänge erstellen lassen und dann mit diesen mehreren Gastzugängen Lobbys erstellen, um unnötige Ressourcen zu verbrauchen. Den Angreifer würden wir hierbei jedoch erwischen, da wir einfach den zuletzt gesendeten Anmeldetoken aus der Hashmap erhalten und auf eine Differenz vergleichen können. Unterscheiden sich beide Tokens, schließt das Backend sofortig die Verbindung mit dem Client, um eine Überladung des Backends zu vermeiden.

### 3.6 Arbeitsspeicher-Management von Matches

Wenn ein Match mit mindestens einem registrierten Spieler gestartet wird, dann wird der Spielstand sowohl im Arbeitsspeicher als auch in der Datenbank festgehalten. Wenn ein Match gestartet wird, wobei kein Spieler ein registriertes Konto hat bzw. alle Spieler beim Start des Matches einen Gast-Login benutzt haben, so wird das Spiel ausschließlich im Arbeitsspeicher des Backends festgehalten. Mithilfe eines Accounts können wiederkehrende Spieler eindeutig zugeordnet werden. Wenn also kein Spieler der Lobby einen Account beim Start des Spieles aufweisen konnte, kann keinem Spieler das Match und dessen Position darin zugeordnet werden. Damit ist auch das Verhalten der Lobby beim Verbindungsabbruch aller Spieler festgelegt:

- a. Wenn es ein Spiel ohne registrierte Spieler ist, wird es nach dem Verbindungsabbruch und einem Countdown das Match beendet und die Lobby freigegeben.  
- b. Bei einem Spiel mit registrierten Spielern dagegen wird bei Verbindungsabbruch das Match abgespeichert und erst dann die Lobby freigegeben. Sofern dann der registrierte Spieler das Spiel wieder aufnimmt (durch Eingabe der UUID), werden die Daten des Matches aus der Datenbank auf der Server geladen und das Spiel kann weitergehen, wo es aufgehört hat. Die restlichen Spieler werden automatisch zugeordnet, sofern möglich. Sonst müssen die Spieler ihre Positionen selbst finden und einnehmen.

### 3.7 Tauschgeschäft-Spielzug

Wenn als Spielzug ein Tausch von Ressourcen zwischen Spielern gewählt wird, so wird diese Handelsanfrage folgendermaßen festgehalten:

```json
{    
    "type":...,  
    "sessionId":...,  
    "destPublicId":...,  
    "offered": [{"resource": "...", "amount": 2}, {"resource": "...", "amount": 1}],  
    "requested": [{"resource": "...", "amount": 1}]  
}
```

Diese JSON-Struktur ermöglicht eindeutige Zuordnung und Bestimmung von den beteiligten Spielern und Materialien im Tausch. Im Frontend gibt es dann ein Fenster, bei welchem die beteiligten Spieler abwechselnd das Tauschangebot annehmen, bearbeiten oder ablehnen können. Wenn beide Spieler annehmen, wird der Tausch abgeschlossen. Wenn ein Spieler ablehnt, dann wird das Tauschangebot gelöscht. Die Spieler können abwechselnd den Tausch bearbeiten, um sich auf ein korrektes Geschäft zu einigen. Wenn eine Variation des Tauschangebots mehrmals vorgeschlagen wird, so kann das Tauschgeschäft automatisch auslaufen.

## 4. Use-Case-Ansicht

*[This section lists use cases or scenarios from the use-case model if they represent some significant, central functionality of the final system, or if they have a large architectural coverage - they exercise many architectural elements, or if they stress or illustrate a specific, delicate point of the architecture.]*

## 5. Logische Ansicht

*[This section describes the architecturally significant parts of the design model, such as its decomposition into subsystems and packages. And for each significant package, its decomposition into classes and class utilities. You should introduce architecturally significant classes and describe their responsibilities, as well as a few very important relationships, operations, and attributes.]*

### 5.1 Übersicht

*[This subsection describes the overall decomposition of the design model in terms of its package hierarchy and layers.]*

### 5.2 Architektonisch Signifikante Designpakete

*[For each significant package, include a subsection with its name, its brief description, and a diagram with all significant classes and packages contained within the package. For each significant class in the package, include its name, brief description, and, optionally a description of some of its major responsibilities, operations and attributes.]*

### 5.3 Use-Case-Realisierungen

*[This section illustrates how the software actually works by giving a few selected use-case (or scenario) realizations, and explains how the various design model elements contribute to their functionality.]*

## 6. Prozessansicht

*[Hier sind alle Sequenzdiagramme verlinkt. Sicher dass man hier noch etwas hinschreiben muss?]*

### [Sequenzdiagramm: Login](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/account_management/login/login.md#sequenzdiagramm-mermaid)

### [Sequenzdiagramm: Logout](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/account_management/logout/logout.md#sequenzdiagramm-mermaid)

### [Sequenzdiagramm: Passwort-Reset](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/account_management/password_reset/password_reset.md#sequenzdiagramm-mermaid)

### [Sequenzdiagramm: Registration](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/account_management/registration/registration.md#sequenz-diagramm)

### [Sequenzdiagramm: Match Starten](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/game/match_starten/match_starten.md#sequenzdiagramm)

### [Sequenzdiagramm: Lobby erstellen](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/lobby_management/lobby_erstellen_beitreten/lobby_erstellen_beitreten.md#sequenzdiagramm)

### [Sequenzdiagramm: Lobby beitreten](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/lobby_management/lobby_erstellen_beitreten/lobby_erstellen_beitreten.md#sequenzdiagramm)

### [Sequenzdiagramm: Light/Dark Mode](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/user_interface/light_dark_mode/light_dark_mode.md#sequenz-diagramm)

### [Sequenzdiagramm: Start Menü](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/srs/user_interface/start_men%C3%BC/start_men%C3%BC.md#sequenzdiagramm)

## 7. Einsatzansicht

*[This section describes one or more physical network (hardware) configurations on which the software is deployed and run. It is a view of the Deployment Model. At a minimum for each configuration it should indicate the physical nodes (computers, CPUs) that execute the software, and their interconnections (bus, LAN, point-to-point, and so on.) Also include a mapping of the processes of the Process View onto the physical nodes.]*

## 8. Implementationsansicht

[Klassendiagramm](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts/blob/main/class_diagram_backend.md#klassendiagramm-des-backend)

## 9. Datenansicht *(optional)*

*[A description of the persistent data storage perspective of the system. This section is optional if there is little or no persistent data, or the translation between the Design Model and the Data Model is trivial.]*

## 10. Größe und Leistung

*[A description of the major dimensioning characteristics of the software that impact the architecture, as well as the target performance constraints.]*

## 11. Qualität

Modifiability erleichtert den Entwicklern die Implementation neuer Features. Dies erreichen wir durch das Abstrahieren von Code in Klassen und/oder Interfaces.

Die Effizienz soll zum Sparen von Ressourcen wie Arbeitsspeicher und Bandbreite führen. Dies erreichen wir z. B. durch das Freilegen von ungenutzten Lobbies oder das Löschen von Matches, in denen keine Spieler aktiv sind.

Außerdem muss Interoperabilität gewährleistet sein, sodass bei Verbindungsabbrüchen sowohl der Client als auch das Backend darüber Bescheid wissen und darauf reagieren können.

Auch Usability hat einen hohen Stellenwert. So muss beispielsweise die Steuerung intuitiv gestaltet sein oder Spielern eindeutiges Feedback bei Fehlern gegeben werden (z.B. ungültige Eingaben).

Grundsätzlich muss auch die Verfügbarkeit des Systems, also Availability, Priorität haben. Dazu gilt es, Risikos für potenzielle DOS-Angriffe zu vermindern.

## 12. Unterstützende Informationen

Für weitere Informationen kontaktieren Sie das Hexfields Studio Team oder besuchen Sie unsere [GitHub Organisation](https://github.com/Hexfields-Studio).

*Dokument basierend auf dem Rational Unified Process SAD-Template.*
