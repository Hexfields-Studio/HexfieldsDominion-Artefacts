# Use-Case Spezifikation: Login

## 1. Login

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es einem User, sich mit seinem Konto anzumelden.

### 1.2 Mockup

![login_mockup](./login_mockup.drawio.png "login_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.1 Grundlegender Ablauf

- Der User ist abgemeldet und befindet sich auf der Anmeldeseite.
- Der User gibt in den Feldern seine Anmeldedaten ein und klickt auf „Log in“.
- Die Anmeldedaten des User werden geprüft.
- Die Anmeldedaten werden gespeichert
- Die App kehrt zur Startseite zurück.

#### Sequenz-Diagramm

![login_sequence](./login_sequence.png "login_sequence")

#### Aktivitäts-Diagramm (Mermaid)

```mermaid
---
title: Login Prozess - Aktivitätsdiagramm
---
flowchart TD
    A([Start]) --> B[Anmeldeseite öffnen]
    B --> C[Anmeldedaten eingeben]
    C --> D[Login-Button klicken]
    D --> E[Anmeldedaten validieren]
    E --> F{Server erreichbar?}
    
    F -- Nein --> G[Fehlermeldung: Server nicht erreichbar]
    F -- Ja --> H[Anmeldedaten an Server senden]
    
    H --> I{Anmeldedaten korrekt?}
    
    I -- Nein --> J{Account gesperrt?}
    J -- Ja --> K[Fehlermeldung: Konto gesperrt]
    J -- Nein --> L[Fehlermeldung: Falsche Anmeldedaten]
    
    I -- Ja --> M[Anmeldung bestätigen]
    M --> N[Anmeldedaten lokal speichern]
    N --> O[Zur Startseite navigieren]
    O --> P([Erfolgreich angemeldet])
    
    G --> C
    K --> C
    L --> C
    
    style A fill:#green
    style P fill:#red
    style G fill:#orange
    style K fill:#orange
    style L fill:#orange
```

### 2.2 Alternative Abläufe

- **Server nicht erreichbar**: Fehlermeldung wird angezeigt und die Anmeldung schlägt fehl.
- **Falsche Anmeldedaten**: Fehlermeldung wird angezeigt, User kann Daten erneut eingeben

## 3. Besondere Anforderungen

- Der User hat bereits ein Konto eingerichtet

## 4. Vorbedingungen

- Die Anmeldeseite ist geöffnet.
- Der User ist abgemeldet.

## 5. Nachbedingungen

- Der User ist angemeldet.
- Die Seite kehrt zur Startseite zurück.

## 6. Story Points

n/a
