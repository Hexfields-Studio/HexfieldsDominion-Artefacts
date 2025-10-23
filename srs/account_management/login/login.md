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
    title Login Seqeuenzdiagramm

    participant User
    participant Client
    participant Valid as Eingabe-Validierung
    participant Auth as Authentifizierungs-Service
    participant DB as Datenbank
    participant Sesh as Session-Manager

    User->>Client: Anmeldedaten eingeben
    User->>Client: "Anmelden" klicken
    activate Client
    
    Client->>Valid: validateCredentials(email, password)
    activate Valid
    Valid->>Valid: if (!email.includes("@")) return false
    Valid->>Valid: if (password.length < 8) return false
    Valid-->>Client: validationResult {isValid: true}
    deactivate Valid
    
    Client->>Auth: POST /api/auth/login
    note over Client,Auth: {email: "user@example.com", password: "hashed_password"}
    activate Auth
    
    Auth->>DB: SELECT * FROM users WHERE email = ?
    activate DB
    alt User nicht gefunden
        DB-->>Auth: HTTP 404 Not Found
        Auth-->>Client: HTTP 401 Unauthorized {error: "Invalid credentials"}
        %%deactivate Auth
        Client->>Client: displayErrorMessage("Ungültige Anmeldedaten")
    else User inaktiv
        DB-->>Auth: user {id: 123, status: "inactive"}
        Auth-->>Client: HTTP 403 Forbidden {error: "Account deactivated"}
        %%deactivate Auth
        Client->>Client: displayErrorMessage("Konto deaktiviert")
    else User aktiv
        DB-->>Auth: user {id: 123, email: "user@example.com", passwordHash: "..."}
        deactivate DB
        
        Auth->>Auth: verifyPassword(inputPassword, storedHash)
        alt Passwort falsch
            Auth->>DB: incrementFailedAttempts(userId)
            Auth-->>Client: HTTP 401 Unauthorized {error: "Invalid credentials"}
            %%deactivate Auth
            Client->>Client: displayErrorMessage("Ungültige Anmeldedaten")
        else Passwort korrekt
            Auth->>DB: resetFailedAttempts(userId)
            Auth->>Sesh: createSession(user.id)
            activate Sesh
            Sesh->>Sesh: generateSessionToken()
            Sesh-->>Auth: session {token: "abc123", userId: 123}
            deactivate Sesh
            
            Auth-->>Client: HTTP 200 OK {sessionToken: "abc123", user: {id: 123, email: "user@example.com"}}
            deactivate Auth
            
            Client->>Client: localStorage.setItem("session", "abc123")
            Client->>Client: redirectToHomepage()
            Client-->>User: Startseite mit User-Dashboard anzeigen
        end
    end
    deactivate Client
```

#### Aktivitätsdiagramm (Mermaid)

```mermaid
---
title: Login Aktivitätsdiagramm
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
    
    I -- Nein --> J[Fehlermeldung: Falsche Anmeldedaten]
    I -- Ja --> K[Anmeldung bestätigen]
    
    K --> L[Anmeldedaten lokal speichern]
    L --> M[Zur Startseite navigieren]
    M --> N([Erfolgreich angemeldet])
    
    G --> C
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
