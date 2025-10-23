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

#### Sequenzdiagramm (Mermaid)

```mermaid
sequenceDiagram
    title Login Sequenzdiagramm

    participant User
    participant Client
    participant Server
    participant Datenbank

    User->>Client: Anmeldedaten eingeben & "Anmelden" klicken
    activate Client
    
    Client->>Server: POST /api/auth/login {email: "user@example.com", password: "..."}
    activate Server
    
    Server->>Datenbank: SELECT * FROM users WHERE email = ?
    activate Datenbank
    
    alt User nicht gefunden
        Datenbank-->>Server: null
        Server-->>Client: HTTP 401 Unauthorized {error: "Invalid credentials"}
    else User inaktiv
        Datenbank-->>Server: user {id: 123, status: "inactive"}
        Server-->>Client: HTTP 403 Forbidden {error: "Account deactivated"}
    else User aktiv
        Datenbank-->>Server: user {id: 123, email: "user@example.com", passwordHash: "..."}
        deactivate Datenbank
        
        Server->>Server: verifyPassword(inputPassword, storedHash)
        alt Passwort falsch
            Server->>Datenbank: incrementFailedAttempts(userId)
            Server-->>Client: HTTP 401 Unauthorized {error: "Invalid credentials"}
        else Passwort korrekt
            Server->>Datenbank: resetFailedAttempts(userId)
            Server->>Server: generateSessionToken()
            Server-->>Client: HTTP 200 OK {sessionToken: "abc123", user: {id: 123, email: "user@example.com"}}
        end
    end
    deactivate Server
    
    alt Erfolg
        Client->>Client: localStorage.setItem("session", "abc123")
        Client->>Client: redirectToHomepage()
        Client-->>User: Startseite mit User-Dashboard anzeigen
    else Fehler
        Client->>Client: displayErrorMessage("Ungültige Anmeldedaten")
    end
    deactivate Client
```

#### Aktivitätsdiagramm (Mermaid)

```mermaid
---
title: Login Aktivitätsdiagramm
---
flowchart TD
    A([Start]) --> B[Home Page öffnen]
    B --> C[Anmeldedaten eingeben]
    B --> X[Als Gast beitreten]
    X --> Y{Server erreichbar?}
    Y -- Nein --> G[Fehlermeldung: Server nicht erreichbar]
    Y -- Ja --> K[Anmeldung bestätigen]
    C --> D[Login-Button klicken]
    D --> E[Anmeldedaten validieren]
    E --> F{Server erreichbar?}
    
    F -- Nein --> G
    F -- Ja --> H[Anmeldedaten an Server senden]
    
    H --> I{Anmeldedaten korrekt?}
    
    I -- Nein --> J[Fehlermeldung: Falsche Anmeldedaten]
    I -- Ja --> K
    
    K --> L[Anmeldedaten lokal speichern]
    L --> M[Zur Startseite navigieren]
    M --> N([Erfolgreich angemeldet])
    
    G --> B
    J --> C
    
    style A fill:#green
    style N fill:#red
    style G fill:#orange
    style J fill:#orange
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
