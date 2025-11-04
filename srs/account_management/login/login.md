# Use-Case Spezifikation: Login

## 1. Login

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es einem User, sich mit seinem Konto anzumelden.

### 1.2 Mockup

![login_mockup](./login_mockup.drawio.png "login_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.2. Grundlegender Ablauf

Dieser Ablauf beschreibt den Prozess, der von einem Spieler für den Log In mit einem Account ausgeführt wird. Der Prozess besteht aus diesen Schritten in dieser Reihenfolge:

1. Der Spieler gibt seine Anmeldedaten im Client ein und betätigt die "Log in" Schaltfläche
2. Der Client leitet die Anmeldedaten an den Server weiter und startet ein Timeout
3. Der Server übermittelt die Anmeldedaten zur Überprüfung an die Datenbank
4. Die Datenbank sucht nach einem User mit den angegebenen Anmeldedaten und bestätigt somit die Existenz
5. Der Server benachrichtigt die erfolgreiche Anmeldung an den Client
6. Der Client stoppt das Timeout, speichert den Anmeldetoken lokal und öffnet das "Start Menü"

Sollte der Timeout ablaufen, soll der Client eine Fehlermeldung anzeigen, dass der Server momentan nicht verfügbar ist.

#### Sequenzdiagramm (Mermaid)

```mermaid
sequenceDiagram
# Login Sequence Diagram

title Login Prozess mit Account

participant Spieler
participant Frontend
participant Timeout
participant Backend
participant Datenbank

activate Spieler
Spieler->Frontend:Anmeldedaten eingegeben und auf Schaltfläche "Log in" drücken
activate Frontend
alt !areLoginCredentialsValid()
Frontend-->Spieler:Fehlermeldung anzeigen: Bad Credentials
else 
Frontend->>Timeout: resetTimeout(5s)
activate Timeout
  Timeout->>Frontend: serverTimeout()
deactivate Timeout
activate Frontend
  Frontend->>Spieler: Fehlermeldung anzeigen: Keine Verbindung mit Server
deactivate Frontend

Frontend->>Backend:POST /auth/login {username: ..., password: ...}
deactivate Frontend
activate Backend
  Backend->>Datenbank:SELECT COUNT(-) as user_count FROM users WHERE USERNAME = username AND PASSWORD = password;
deactivate Backend
activate Datenbank


Datenbank-->>Backend:user_count
deactivate Datenbank
activate Backend
alt user_count
Backend-->Frontend:200 OK (Token mitsenden?)
activate Frontend
  Frontend->Spieler:Start Menü anzeigen
deactivate Frontend
else
Backend-->Frontend:400 Bad Request
deactivate Backend
activate Frontend
Frontend->Spieler:Fehlermeldung anzeigen: Account mit den angegebenen Daten existiert nicht
deactivate Frontend

end
end

Spieler->Frontend:Schaltfläche "als Gast beitreten"\ngedrückt
activate Frontend
  Frontend->>Timeout: resetTimeout(5s)
  activate Timeout
    Timeout->>Frontend: serverTimeout()
  deactivate Timeout
  activate Frontend
    Frontend->>Spieler: Fehlermeldung anzeigen: Keine Verbindung mit Server
  deactivate Frontend
  Frontend->>Backend:POST /auth/login {username:"guest", password:"cookie"}
  deactivate Frontend
  activate Backend
    Backend-->Frontend: 200 OK (Token mitsenden?)
  deactivate Backend
  activate Frontend
  Frontend->Spieler: Start Menü anzeigen
  deactivate Frontend
deactivate Spieler
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
- **Gast-Anmeldung**: *[siehe Use-Case Gast-Login](../gast_login/gast_login.md)*

## 3. Besondere Anforderungen

- Der User hat bereits ein Konto eingerichtet

## 4. Vorbedingungen

- Die Home Page ist geöffnet.
- Der User ist abgemeldet.

## 5. Nachbedingungen

- Der User ist angemeldet.
- Die Seite führt zum Start Menü.

## 6. Story Points

n/a
