# Use-Case Anforderung: Match starten

# 1. Match starten

## 1.1 Kurze Beschreibung
Dieses Use-Case dient dazu, dass ein User (der Lobbyanführer) ein Match starten kann. Dabei wird eine bestehende Lobby benötigt, auf der das Match aufbaut.

## 1.2 Mockup
![Mockup Match starten](match_starten.png)

# 2. Ablauf von Ereignissen

## 2.1 Grundlegender Ablauf
Dieser Ablauf beschreibt den Prozess, der von einem Spieler (den Lobbyanführer) das starten eines Matches ausgeführt wird. Der Prozess besteht aus diesen Schritten in dieser Reihenfolge:
1. Ein User klickt auf "Match starten"
2. Das Frontend sendet eine Anfrage an das Backend um ein Match auf Grundlage der Lobby zu starten
3. Solange das Match läuft, überträgt das Backend die Daten des matches an alle Clients innerhalb der Lobby

## 2.2 Alternative Abläufe
Im Falle eines Fehlers (sollte der Spieler nicht der Lobbyanführer sein) sollte eine entsprechende Fehlermeldung zurückgesendet werden.

Das folgende Sequenzdiagramm beschreibt beide Abläufe:
```mermaid
sequenceDiagram
title Match Starten

participant Alle Frontends
participant Spieler
participant Frontend
participant Backend
participant Timer


Spieler->>Frontend:"Match starten" Taste drücken
activate Frontend
  Frontend->Backend: POST /startMatch {...settings}
  activate Backend
    alt !player.isLobbyLeader()
      Backend-->Frontend: 403 Forbidden
    else
      Backend-->Frontend: 200 OK
      deactivate Frontend
      note over Backend:Match Initialisieren
      Backend->>Alle Frontends: event: loadMatch\ndata: {players: {...}, map_layout: {...}}
        note over Alle Frontends:Zur "Match Page" welchseln über React Router,\ndie empfangenen Daten nutzen, um den\nSpielstand darzustellen.
      loop !anyPlayerHasWon()
        Backend->Backend: nextPlayerTheRightToMove()
        activate Backend
          Backend-->Backend:
        deactivate Backend
        Backend->>Timer: resetTimer(60s)
        activate Timer
          note over Timer: Nach Ablauf des Timers\ndie nächste Schleifeniteration\nerzwingen.
          Timer->>Backend: continue
        deactivate Timer
        Backend ->> Alle Frontends: event: nextPlayerIsPlaying\ndata: {player: {...}}
        note over Alle Frontends: Darstellen, dass der nächste\nSpieler im "player" Feld\nam Spielzug ist.
        Backend->Backend: throwDiceAndDistributeRessources()
        activate Backend
          Backend ->> Alle Frontends: event: currentPlayerRessources\ndata:{ressources:{...}}
          note over Alle Frontends:Die Ressourcenbestände\nder Spieler aktualisieren
          Backend --> Backend:
        deactivate Backend
        
        Spieler ->> Frontend: Einen beliebigen Spielzug\nausführen
        activate Frontend
          Frontend->Backend: POST /makeMove/{match_id}
          activate Backend
            alt hasRightForTurn(player) && moveIsValid(move)
              Backend->Backend: executeMove(move)
              activate Backend
                Backend ->> Alle Frontends: event: newGameState\ndata: {...}
                Backend-->Backend:
                
              deactivate Backend
              alt isMoveToEndTurn(move)
                Backend->Backend: continue
              end
              Frontend-->Backend: 200 Ok
            else
              Frontend-->Backend: 400 Bad Request
              deactivate Backend
              deactivate Frontend
            end
      end
      Backend->>Alle Frontends: event:matchEnd\ndata:{}
      deactivate Backend
    end
```

# 3. Spezielle Anforderungen
n/a

# 4. Vorbedingungen
Für das Starten des Matches gelten folgende Vorbedingungen:
1. Der User hat die Rolle des Lobbyanführers
2. Es befinden sich genügend Spieler innerhalb der Lobby
3. Die Konfigurationen sind valide

# 5. Nachbedingungen
1. Ein Match wurde auf Grundlage der Lobby erstellt.
2. Alle Mitglieder der Lobby wurden zu diesem Match weitergeleitet und habe.

# 6. Aufwandsschätzung
Story Points: 8
