# Use-Case Spezifikation: Guest Login

## 1. Gast Login

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es einem User, sich auf der Startseite ohne Konto mit einen anonymen Gast-Konto anzumelden.

### 1.2 Mockup

![gast_login_mockup](./gast_login_mockup.drawio.png "gast_login_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.1 Grundlegender Ablauf

- Der User ist nicht angemeldet und befindet sich auf der Startseite.
- Der User klickt auf den Knopf „Log in as guest“.
- Die Anmeldedaten des User werden aus dem lokalen Speicher entfernt.
- Die App kehrt zum Anmeldebildschirm zurück.

#### Sequenzdiagramm (Mermaid)

```mermaid
sequenceDiagram
    title Gast-Login Sequenzdiagramm

    participant User
    participant Client
    participant Server
    participant Datenbank

    User->>Client: "Als Gast anmelden" klicken
    activate Client
    Client->>Client: clearLocalStorage()
    
    Client->>Server: POST /api/auth/guest-login
    activate Server
    
    Server->>Datenbank: generateGuestUser()
    activate Datenbank
    Datenbank->>Datenbank: createUser(role: "guest", temp: true)
    Datenbank-->>Server: guestUser {id: "guest_123", temp: true}
    deactivate Datenbank
    
    Server->>Server: createSession(guestUser.id)
    Server-->>Client: HTTP 200 OK {sessionToken: "xyz", user: {id: "guest_123", role: "guest"}}
    deactivate Server
    
    Client->>Client: localStorage.setItem("session", "xyz")
    Client->>Client: redirectToHomepage()
    Client-->>User: Startseite mit Gast-Status anzeigen
    deactivate Client
```

### 2.2 Alternative Abläufe

- **Server nicht erreichbar**: Fehlermeldung wird angezeigt und die Anmeldung schlägt fehl.
- **Bereits als Gast angemeldet**: Bestehende Gast-Session wird beibehalten.

## 3. Besondere Anforderungen

n/a

## 4. Vorbedingungen

- Die Startseite ist geöffnet.
- Der User ist mit keinem Konto angemeldet.

## 5. Nachbedingungen

- Der User ist als Gast angemeldet.
- Die Seite kehrt zur Startseite zurück.

## 6. Story Points

n/a
