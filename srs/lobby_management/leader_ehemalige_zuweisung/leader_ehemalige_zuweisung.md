# Use-Case Anforderung: Leader und (ehemalige) Spieler zuweisen

# 1. Leader und (ehemalige) Spieler zuweisen

## 1.1 Kurze Beschreibung
Dieses Use-Case dient einerseits dazu, dass Leader einer Lobby die Leader-Rolle an andere Spieler weitergeben können. Außerdem sollen Leader Spielern Spieldaten von ehemaligen Spielern zuweisen können, falls diese versehentlich das Spiel verlassen haben und als Gast gespielt haben.

## 1.2 Mockup
![Mockup Leader und (ehemalige) Spieler Zuweisung](leader_ehemalige_zuweisung.png)

# 2. Ablauf von Ereignissen

## 2.1 Ereignisse
...

### Sequenzdiagramm
...

## 2.2 Alternative Abläufe
n/a

# 3. Spezielle Anforderungen
n/a

# 4. Vorbedingungen
1. Die User haben die Anwendung geöffnet.
2. Die User sind Leader in dem Match, in welchem sie Aktionen vornehmen wollen.

# 5. Nachbedingungen
1. Wenn die Leader-Rolle neu zugewiesen wurde, besitzt nur der neue Leader diese Rolle.
2. Falls Spieldaten ehemaliger Spieler Spielern im Match zugewiesen wurden, können diese nicht mehr weiteren Spielern zugewiesen werden. Außerdem wurden dann dem Spieler im Match alle Daten übertragen, die für das Match relevant sind.

# 6. Aufwandsschätzung
Story Points: 8
