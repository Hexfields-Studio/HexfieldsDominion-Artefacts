# Use-Case Spezifikation: Registration

## 1. Registration

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es einem neuen User, sich ein Konto zu erstellen.

### 1.2 Mockup

![registration_mockup](./registration_mockup.drawio.png "registration_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.1 Grundlegender Ablauf

- Der User ist abgemeldet und befindet sich auf der Anmeldeseite.
- Der User klickt auf "Sign up" oder "Registrieren".
- Der User gibt in den Feldern seine Daten ein (Username, E-Mail, Passwort, Passwort bestätigen).
- Der User klickt auf "Register".
- Die Daten des User werden geprüft (Eindeutigkeit, Passwortstärke, etc.).
- Ein neues Konto wird erstellt.
- Die Anmeldedaten werden gespeichert.
- Die App kehrt zur Startseite zurück.

#### Sequenz Diagramm

```mermaid
sequenceDiagram
    title Registrierung Sequenzdiagramm

    participant User
    participant Client
    participant Server
    participant Datenbank

    User->>Client: Registrierungsdaten eingeben & "Registrieren" klicken
    activate Client
    
    Client->>Server: POST /auth/register {username: "newuser", email: "new@example.com", password: "..."}
    activate Server
    
    Server->>Datenbank: checkExistingUser(username, email)
    activate Datenbank
    alt Username existiert
        Datenbank-->>Server: {usernameExists: true}
        Server-->>Client: HTTP 409 Conflict {error: "Username already taken"}
    else E-Mail existiert
        Datenbank-->>Server: {emailExists: true}
        Server-->>Client: HTTP 409 Conflict {error: "Email already registered"}
    else Daten eindeutig
        Datenbank-->>Server: {usernameExists: false, emailExists: false}
        deactivate Datenbank
        
        Server->>Datenbank: createUser(userData)
        activate Datenbank
        Datenbank->>Datenbank: hashPassword(password)
        Datenbank-->>Server: newUser {id: 456, username: "newuser", email: "new@example.com"}
        deactivate Datenbank
        
        Server->>Server: generateSessionToken()
        Server-->>Client: HTTP 201 Created {sessionToken: "def456", user: {id: 456, username: "newuser"}}
    end
    deactivate Server
    
    alt Erfolg
        Client->>Client: localStorage.setItem("session", "def456")
        Client->>Client: redirectToHomepage()
        Client-->>User: Willkommensnachricht auf Startseite anzeigen
    else Fehler
        Client->>Client: displayErrorMessage("Registrierung fehlgeschlagen")
    end
    deactivate Client
```

### 2.2 Alternative Abläufe

- **Username bereits vergeben**: Fehlermeldung wird angezeigt, User wird aufgefordert einen anderen Usernamen zu wählen.
- **E-Mail bereits registriert**: Fehlermeldung wird angezeigt, User wird informiert dass die E-Mail bereits existiert.
- **Passwort zu schwach**: Fehlermeldung wird angezeigt mit Anforderungen an das Passwort.

## 3. Besondere Anforderungen

- Der User hat noch kein Konto in der App.
- Das Passwort muss bestimmten Sicherheitsanforderungen entsprechen.

## 4. Vorbedingungen

- Die Registrierungsseite ist geöffnet.
- Der User ist abgemeldet.

## 5. Nachbedingungen

- Der User ist angemeldet.
- Ein neues Konto wurde erstellt.
- Die Seite kehrt zur Startseite zurück.

## 6. Story Points

n/a
