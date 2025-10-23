# Use-Case Spezifikation: Passwort Reset

## 1. Passwort Reset

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es einem User, sein Passwort zurückzusetzen, falls er es vergessen hat.

### 1.2 Mockup

![password_reset_mockup](./password_reset_mockup.drawio.png "password_reset_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.1 Grundlegender Ablauf

- Der User ist abgemeldet und befindet sich auf der Anmeldeseite.
- Der User klickt auf "Passwort vergessen".
- Der User gibt seine E-Mail-Adresse ein.
- Der User klickt auf "Passwort zurücksetzen".
- Eine E-Mail wird an die angegebene Adresse geschickt, mit einem zufällig generiertem Passwort.
- Der User wird zur Anmeldeseite weitergeleitet.

#### Sequenzdiagramm (Mermaid)

```mermaid
sequenceDiagram
    title Passwort-Reset Sequenzdiagramm

    participant User
    participant Client
    participant Valid as Eingabe-Validierung
    participant Auth as Authentifizierungs-Service
    participant DB as Datenbank
    participant Mails as E-Mail-Service

    User->>Client: "Passwort vergessen" klicken
    Client->>Client: Passwort-Reset-Formular anzeigen
    User->>Client: E-Mail-Adresse eingeben
    User->>Client: "Passwort zurücksetzen" klicken
    activate Client
    
    Client->>Valid: validateEmail("user@example.com")
    activate Valid
    Valid->>Valid: const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    Valid-->>Client: isValid: true
    deactivate Valid
    
    Client->>Auth: POST /api/auth/password-reset
    note over Client,Auth: {email: "user@example.com"}
    activate Auth
    
    Auth->>DB: SELECT id, email FROM users WHERE email = ?
    activate DB
    alt E-Mail nicht gefunden
        DB-->>Auth: null
        %%deactivate DB
        Auth-->>Client: HTTP 200 OK {message: "If email exists, reset link sent"}
        %%deactivate Auth
    else E-Mail gefunden
        DB-->>Auth: user {id: 123, email: "user@example.com"}
        deactivate DB
        
        Auth->>Auth: generateResetToken()
        Auth->>DB: storeResetToken(userId, token, expiresIn: "1h")
        activate DB
        DB-->>Auth: success
        deactivate DB
        
        Auth->>Mails: sendResetEmail(user.email, resetToken)
        activate Mails
        Mails->>Mails: constructResetLink(token)
        Mails-->>Auth: emailQueued
        deactivate Mails
        
        Auth-->>Client: HTTP 200 OK {message: "If email exists, reset link sent"}
        deactivate Auth
    end
    
    Client->>Client: showSuccessMessage()
    Client-->>User: "E-Mail mit Reset-Anleitung prüfen"
    deactivate Client
```

### 2.2 Alternative Abläufe

- **Ungültiges Passwort**: Fehlermeldung wird angezeigt mit Anforderungen an das Passwort.

## 3. Besondere Anforderungen

- Der User muss Zugriff auf die registrierte E-Mail-Adresse haben.
- Der Reset-Link hat eine begrenzte Gültigkeitsdauer.

## 4. Vorbedingungen

- Die Passwort-zurücksetzen-Seite ist geöffnet.
- Der User ist abgemeldet.

## 5. Nachbedingungen

- Das Passwort wurde geändert.
- Der User wird zur Anmeldeseite weitergeleitet.

## 6. Story Points

n/a
