# Use-Case Anforderung: Lobby erstellen und beitreten

# 1. Lobby erstellen und beitreten

## 1.1 Kurze Beschreibung
Dieses Use-Case dient dazu, dass User eine neue Lobby erstellen können. Außerdem sollen sie einer bestehenden Lobby beitreten können.  
Dabei ist egal, ob sie mit einem Account oder als Gast angemeldet sind und für den Beitritt benötigen sie lediglich den entsprechenden Lobbycode.

## 1.2 Mockup
![Mockup Lobby erstellen](lobby_erstellen.png)
![Mockup Lobby beitreten](lobby_beitreten.png)

## 1.3 Aktivitätsdiagramm
![Aktivitätsdiagramm Lobby erstellen und beitreten](aktivitätsdiagramm.png)

# 2. Ablauf von Ereignissen

## 2.1 Grundlegender Ablauf
1. Ein User klickt auf “Erstellen”
2. Das Frontend fragt beim Backend an, um eine freie Lobby zu finden und die vom User angegebenen Konfigurationen anzuwenden.
3. Der User wird zur Lobby Seite weitergeleitet

### Sequenzdiagramm
```mermaid
sequenceDiagram
title Lobby erstellen

participant Spieler
participant Frontend
participant Backend

Spieler->Frontend: Klickt "Erstellen"
activate Spieler
activate Frontend
Frontend->>Backend: GET /createLobby {configs: {...}}
deactivate Frontend
activate Backend
  alt emptyLobbyIsAvailable()
    Backend-->>Frontend: 200 OK {lobby_id: ..., code: ..., players:{...}, ...}
    activate Frontend
    Frontend --> Spieler: Lobby Raum anzeigen
    deactivate Frontend
  else
    Backend-->>Frontend: 503 Service Unavailable
    activate Frontend
      Frontend --> Spieler: Fehlermeldung anzeigen
    deactivate Frontend
    deactivate Backend
    deactivate Spieler
  end
```


## 2.2 Alternative Abläufe
1. Der User klickt auf "Beitreten"
2. Das Frontend fragt beim Backend die zum Code zugehörige Lobby ab
3. Der User wird zur Lobby Seite weitergeleitet

### Sequenzdiagramm
```mermaid
sequenceDiagram
title Lobby beitreten

participant Spieler
participant Frontend
participant Backend

activate Spieler
Spieler->Frontend: Klickt "Beitreten" oder verwendet eine URL mit Code
activate Frontend
  Frontend->>Backend: GET /lobby/[code]
  
deactivate Frontend
activate Backend
  alt lobbyExistsWithCode(code)
  Backend-->>Frontend: 200 OK {lobby_id: ..., code: ..., players:{...}, ...}
  activate Frontend
  Frontend --> Spieler: Lobby Raum anzeigen
  deactivate Frontend
  else
  Backend-->>Frontend: 404 Not Found
  deactivate Backend
  activate Frontend
  Frontend --> Spieler: Fehlermeldung anzeigen
  deactivate Frontend
  deactivate Spieler
  end

```

### Vorbedingungen
1. Die User haben die Anwendung geöffnet.
2. Die User haben sich mit Account oder als Gast angemeldet.
3. Die User haben im Start Menü auf "Lobby beitreten" geklickt.

### Nachbedingungen
1. Die User werden zur entsprechenden Lobby weitergeleitet.
2. Falls die Lobby bereits in einem Match ist, werden die User zu dem entsprechenden Match weitergeleitet.

# 3. Spezielle Anforderungen
n/a

# 4. Vorbedingungen
1. Die User haben die Anwendung geöffnet.
2. Die User haben sich mit Account oder als Gast angemeldet.
3. Die User haben im Start Menü auf "Lobby erstellen" geklickt.

# 5. Nachbedingungen
Die User werden zur erstellten Lobby weitergeleitet.

# 6. Aufwandsschätzung
Story Points: 13
