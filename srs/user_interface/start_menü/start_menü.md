# Use-Case Anforderung: Start Menü

# 1. Start Menü

## 1.1 Kurze Beschreibung
Dieses Use-Case dient dazu, dass User einer Lobby und damit einem Match beitreten können. Dabei können sie entweder selbst eine Lobby erstellen oder einer bestehenden Lobby beitreten.

## 1.2 Mockup
![Mockup Start Menü](start_menü.png)

# 2. Ablauf von Ereignissen

## 2.1 Ereignisse
- Die User klicken auf "Lobby erstellen"
- Das Frontend öffnet das Fenster zur Erstellung einer Lobby

### Sequenzdiagramm
![Sequenzdiagramm Start Menü 1](start_menü_seqdg1.png)

## 2.2 Alternative Abläufe
- Die User klicken "Lobby beitreten"
- Das Frontend öffnet das Fenster zum Beitritt einer Lobby

### Sequenzdiagramm
![Sequenzdiagramm Start Menü 2](start_menü_seqdg2.png)

# 3. Spezielle Anforderungen
n/a

# 4. Vorbedingungen
1. Die User haben die Anwendung geöffnet.
2. Die User haben sich mit Account oder als Gast angemeldet.

# 5. Nachbedingungen
Usern wird je nach Auswahl das entsprechende Fenster angezeigt:
1. Menü zur Erstellung einer Lobby mit Einstellungsmöglichkeiten
2. Eingabemöglichkeit eines Lobbycodes, um einer bestehenden Lobby beizutreten

# 6. Aufwandsschätzung
Story Points: 3
