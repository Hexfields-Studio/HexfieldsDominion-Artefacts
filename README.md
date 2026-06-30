# HexfieldsDominion-Artefacts

Dies ist der Speicherort aller Artefakte des Softwareprojekts [Hexfields: Dominion](https://github.com/Hexfields-Studio/HexfieldsDominion).

## Was ist "Hexfields: Dominion"?

*Hexfields: Dominion* ist ein webbasiertes Multiplayer-Spiel mit dem Ziel, die Spielidee von "Siedler von Catan" als online Mehrspieler-Spiel umzusetzen. Damit kann es unabhängig vom Betriebssystem der Clients gespielt werden. Implementiert wurde das Projekt mit einem React-Frontend in TypeScript und React Vite, einem Java-Spring-Backend mit Gradle, sowie PostgreSQL als Datenbank. Für die Kommunikation zwischen Frontend und Backend werden eine REST-API und Server-Sent-Events (SSE) eingesetzt. 
Zum fachlichen Umfang gehören Account-Management mit Registrierung, Login, Gastzugang und Profilverwaltung, Lobby-Management mit Erstellen und Beitreten per Code sowie eine Spiel-Engine für Spielzüge, Ressourcen, Gebäude, Handel und Match-Start. Die Oberfläche umfasst Startbildschirm, Home-/Anmeldeseiten, Spielfeld und Spielmenü sowie einen Light/Dark Mode. 
Architektonisch ist das System als client-server-basierte Anwendung mit getrennten Repositories für Frontend und Backend aufgebaut, ergänzt um eine persistente Datenbank für Spielstände und Konfigurationen. Das Frontend wird über GitHub Pages ausgeliefert, während das Backend als separater Service betrieben wird und mit Heartbeats die Verbindung zu Clients prüft und mit Tokens die Sicherheit garantiert.

## Dokumentationsliste

- [Projekthandout](./presentation-final/SoftwareEngineering-Final-Handout.pdf)
- [Präsentationsfolien](./presentation-final/SoftwareEngineering-Final-Slides.pdf)
- [RUP SRS Dokument](./srs.md)
- [RUP SAD Dokument](./sad.md)
- Qualitätsbericht
  - [Refactoring Zusammenfassung](./refactoring_summary.md)
  - [Technical Review Meetingbericht](./technical_review.md)
  - [Test Report](./test_plan.md)
  - [Softwaremetriken](./metrics_summary.md)
- [CI/CD Setup](./ci_cd_summary.md)
- [Risk Management Table](./risk_management_table.md)
- [Zusammenfassung Retrospektive](./project_retrospective.md#zusammenfassung)

### Weiterführende Links
- [Repository Frontend](https://github.com/Hexfields-Studio/HexfieldsDominion)
- [Repository Backend](https://github.com/Hexfields-Studio/HexfieldsDominion-Backend)
- [Repository Artifacts](https://github.com/Hexfields-Studio/HexfieldsDominion-Artefacts)
- [Jira](https://dh-siedler.atlassian.net/jira/software/projects/HEX/boards/1)

