# Use-Case Spezifikation: Logout

## 1. Logout

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es einem angemeldeten User, sich auf der Startseite von seinem Konto abzumelden.

### 1.2 Mockup

![logout_mockup](./logout_mockup.drawio.png "logout_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.1 Grundlegender Ablauf

- Der User ist angemeldet.
- Der User klickt auf das Kontosymbol und anschließend auf „Log out“.
- Die Anmeldedaten des User werden aus dem lokalen Speicher entfernt.
- Die App kehrt zur Startseite zurück.

#### Sequenzdiagramm (Mermaid)

```mermaid
sequenceDiagram
    title Logout Sequenzdiagramm

    participant User
    participant Client
    participant Auth as Authentifizierungs-Service
    participant Sesh as Session-Manager
    participant Local as Lokaler Speicher

    User->>Client: Usermenü öffnen
    User->>Client: "Abmelden" klicken
    activate Client
    
    Client->>Local: getItem("sessionToken")
    activate Local
    Local-->>Client: "abc123"
    deactivate Local
    
    Client->>Auth: POST /api/auth/logout
    note over Client,Auth: Authorization: Bearer abc123
    activate Auth
    
    Auth->>Sesh: invalidateSession("abc123")
    activate Sesh
    Sesh->>Sesh: deleteSession("abc123")
    Sesh-->>Auth: success
    deactivate Sesh
    
    Auth-->>Client: HTTP 200 OK
    deactivate Auth
    
    Client->>Local: removeItem("sessionToken")
    Client->>Local: removeItem("userData")
    Client->>Client: clearUserState()
    Client->>Client: redirectToHomepage()
    Client-->>User: Startseite mit Login-Optionen anzeigen
    deactivate Client
```

### 2.2 Alternative Abläufe

- **Automatischer Logout bei Inaktivität**: Session wird nach Timeout automatisch beendet
- **Browser schließen ohne Logout**: Session bleibt aktiv für spätere Wiederaufnahme

## 3. Besondere Anforderungen

- Der User hat bereits ein Konto eingerichtet

## 4. Vorbedingungen

- Die Startseite ist geöffnet.
- Der User ist in seinem Konto angemeldet.

## 5. Nachbedingungen

- Die Sitzungsdaten werden entfernt.
- Die Seite kehrt zur Startseite zurück.

## 6. Story Points

n/a
