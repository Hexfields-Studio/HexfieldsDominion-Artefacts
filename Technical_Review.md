## Technisches Review zu **Hexfields: Dominion**

Datum: Dienstag, 25.05.2026
Uhrzeit: 18:30-19:30 Uhr

### Teilnehmer

#### Team Hexfields-Studio (aka. "Siedler")

- Marcel: Protokollant, (Frontend-) Entwickler
- Alex: Moderator (Video-Stream), (Sr.-) Entwickler
- Jona: (Backend-) Entwickler

#### Team Ceasar's Gambit (aka. "Risiko")

- David: Externer Entwickler, Reviewer

### Thema des Review

- Technische Aspekte des Frontend und Backend, Fokus auf SSE und Informationsaustausch

### Walkthrough im Code

- Vorstellung des Projekts an David, Demo des aktuellen Stands (Work-In-Progress)
- Diskussion von Performanz von react-konva
  -> Firefox hat kurze Leistungseinbrüche, Chromium-basierte Browser wie Edge nicht.
  -> Cache-Handling ist unterschiedlich und kann auf Firefox nur mit Workarounds verbessert werden.
- Diskussionen zu Handling von Subscribe und Unsubscribe des SSE
  -> Spieler, die eine neue Seite besuchen, werden mit dem react-router korrekt abgemeldet von Lobby-SSE usw.
  -> TODO: Fehlermeldungen Backend an Frontend, z.B.: Error "Match not found" an Frontend weitergeben (Existierender PR wird erweitert).
- Diskussion Persistenz: Bleibt Spiel erhalten, wenn Host/Alle das Spiel verlässt?
  -> Solange ein Nutzer vorhanden ist, wird das Spiel erhalten.
  -> Angemeldete Nutzer haben gegenüber Gästen das Privileg, dass das Spiel bei Lobby-Total-Offline Persistent gespeichert wird.
  -> Gäste sind nicht zuordbar, deswegen werden verlassene Spiele mit nur Gästen beendet und RAM freigegeben.
- Diskussion Heartbeat: Wie werden Spieler mit spontanem Disconnect ermittelt?
  -> Mit Subscribe zur Lobby fängt der Client an, alle 4s einen Heartbeat zu schicken. Kommt kein Heartbeat in 10s beim Backend an, dann wird unsubscribed
- Diskussion zu Datenübertragung: Wie werden Lobby-Daten und Spieler-Daten ausgetauscht?
  -> Einfache JSON POSTs
  -> TODO (lobby.tsx): Daten validieren von Frontend>Backend und Backend>Frontend, malformed Daten neu anfordern?
- Diskussion zu sseListener in Lobby.tsx: Warum useMemo()?
  -> React lädt pro Frame SSE-Listener neu und würde zu großen Performance-Einbrüchen führen, useMemo() behält bestehende Objekte bei statt neu erstellen