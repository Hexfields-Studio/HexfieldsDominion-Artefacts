# Use-Case Anforderung: Lobby erstellen und beitreten

# 1. Lobby erstellen und beitreten

## 1.1 Kurze Beschreibung
Dieses Use-Case dient dazu, dass User eine neue Lobby erstellen können. Dafür müssen sie mit einem Account angemeldet sein.  
Außerdem sollen sie einer bestehenden Lobby beitreten können. Dabei ist egal, ob sie mit einem Account oder als Gast angemeldet sind, sie benötigen lediglich den entsprechenden Lobbycode.

## 1.2 Mockup
![Mockup Lobby erstellen](lobby_erstellen.png)
![Mockup Lobby beitreten](lobby_beitreten.png)

## 1.3 Aktivitätsdiagramm
![Aktivitätsdiagramm Lobby erstellen und beitreten](aktivitätsdiagramm.png)

# 2. Ablauf von Ereignissen

## 2.1 Grundlegender Ablauf
1. Die User klicken auf "Erstellen"
2. Das Frontend fragt beim Backend an, dass die Lobby mit der gewählten Konfiguration erstellt wird
3. Das Backend gibt die erstellte Lobby und die ID zurück
4. Die User werden zur Lobby Seite weitergeleitet

### Sequenzdiagramm
![Sequenzdiagramm Lobby erstellen](lobby_erstellen_seqdg.png)

## 2.2 Alternative Abläufe
1. Die User klicken auf "Beitreten"
2. Das Frontend fragt beim Backend die zum Code zugehörige Lobby ab
3. Das Backend gibt die zugehörige Lobby und die ID zurück
4. Die User werden zur Lobby Seite weitergeleitet

### Sequenzdiagramm
![Sequenzdiagramm Lobby beitreten](lobby_beitreten_seqdg.png)

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
2. Die User haben sich mit einem Account angemeldet.
3. Die User haben im Start Menü auf "Lobby erstellen" geklickt.

# 5. Nachbedingungen
Die User werden zur erstellten Lobby weitergeleitet.

# 6. Aufwandsschätzung
Story Points: 13
