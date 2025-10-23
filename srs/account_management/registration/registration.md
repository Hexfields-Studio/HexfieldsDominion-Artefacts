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
    participant Valid as Eingabe-Validierung
    participant Auth as Authentifizierungs-Service
    participant DB as Datenbank

    User->>Client: Registrierungsdaten eingeben
    User->>Client: "Registrieren" klicken
    activate Client
    
    Client->>Valid: validateRegistration(formData)
    activate Valid
    Valid->>Valid: if (username.length < 3) return error
    Valid->>Valid: if (password !== confirmPassword) return error
    Valid->>Valid: if (!isStrongPassword(password)) return error
    Valid-->>Client: validationResult {isValid: true, errors: []}
    deactivate Valid
    
    Client->>Auth: POST /api/auth/register
    note over Client,Auth: {username: "newuser", email: "new@example.com", password: "hashed"}
    activate Auth
    
    Auth->>DB: checkExistingUser(username, email)
    activate DB
    alt Username existiert
        DB-->>Auth: {usernameExists: true, emailExists: false}
        %%deactivate DB
        Auth-->>Client: HTTP 409 Conflict {error: "Username already taken"}
        %%deactivate Auth
        Client->>Client: displayErrorMessage("Username bereits vergeben")
    else E-Mail existiert
        DB-->>Auth: {usernameExists: false, emailExists: true}
        %%deactivate DB
        Auth-->>Client: HTTP 409 Conflict {error: "Email already registered"}
        %%deactivate Auth
        Client->>Client: displayErrorMessage("E-Mail bereits registriert")
    else Daten eindeutig
        DB-->>Auth: {usernameExists: false, emailExists: false}
        deactivate DB
        
        Auth->>DB: createUser(userData)
        activate DB
        DB->>DB: hashPassword(password)
        DB->>DB: insert into users (username, email, password_hash, status)
        DB-->>Auth: newUser {id: 456, username: "newuser", email: "new@example.com"}
        deactivate DB
        
        Auth->>Auth: generateVerificationToken()
        Auth->>DB: storeVerificationToken(userId, token)
        
        Auth-->>Client: HTTP 201 Created {user: {id: 456, username: "newuser"}, message: "Registration successful"}
        deactivate Auth
        
        Client->>Client: autoLoginUser(sessionToken)
        Client->>Client: redirectToHomepage()
        Client-->>User: Willkommensnachricht auf Startseite anzeigen
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
