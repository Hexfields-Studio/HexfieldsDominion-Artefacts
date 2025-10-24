# Use-Case Anforderung: Aktualisierung und Datenaustausch

# 1. Aktualisierung und Datenaustausch

## 1.1 Kurze Beschreibung
Dieses Use-Case dient zur Anzeige des aktuellen Spielstandes auf der Seite des Clients. Demnach ist sie rein technisch basiert und wird im Hintergrund aktiviert. 
Zu den Ereignissen, die die Aktualisierung auslösen, gehören unter anderem:
- Den Wechsel des spielenden Spielers
- Das erhalten von neuen Ressourcen nach dem Würfeln
- Das ausführen eines Spielzugs
- Übertragung der Rolle des Spielleiters
- Wenn ein Spieler das Match beitritt oder verlässt
- etc.

## 1.2 Mockup 
n/a

# 2. Ablauf von Ereignissen

## 2.1 Grundlegender Ablauf
Dieser Ablauf beschreibt den vom Backend ausgeführten Prozess, um sicherzustellen, dass alle Clients immer den aktuellen Spielzustand anzeigen. Der Prozess besteht aus diesen Schritten in dieser Reihenfolge:
1. Der Spielzustand ändert sich
2. Der Server sendet das update an allen beteiligten Spielern
3. Die Clients nehmen die neuen Daten auf, evtl. ändert sich die "Match Page"

Im folgenden Sequenzdiagram sind unteranderem auch die Funktionalitäten dieses Use-Case enthalten:

*Kopie aus [match_starten.md: Sequenzdiagramm](./../match_starten/match_starten.md#sequenzdiagramm)*
```mermaid
sequenceDiagram
title Match Starten

participant Alle Frontends
participant Spieler
participant Frontend
participant Backend
participant Timeout


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
        Backend->>Timeout: resetTimeout(60s)
        activate Timeout
          note over Timeout: Nach Ablauf des Timeout\ndie nächste Schleifeniteration\nerzwingen.
          Timeout->>Backend: continue
        deactivate Timeout
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

## 2.2 Alternative Abläufe
n/a

# 3. Besondere Anforderungen
n/a

# 4. Vorbedingungen
- Mindestens ein Client muss mit dem Match verbunden sein, an den das Backend die Daten senden kann
- eine Veränderung des Spielstandes muss auftreten, um den Datenaustausch auszulösen

# 5. Nachbedingungen
Jeder Client muss (immer) den aktuellen Spielzustand für den Spieler anzeigen.

# 6. Story Points
