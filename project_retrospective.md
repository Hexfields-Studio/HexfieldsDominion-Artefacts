## Retrospektive des Projekts **Hexfields: Dominion**

## Zusammenfassung:

Da die Anzahl der "weiter" Punkte deutlich größer ist als die der "stopp" Punkte, haben wir insgesamt eher einen positiven Blick auf die Arbeit am Projekt. Dazu gehören beispielsweise Arbeitsverteilung/-suche oder die Einbindung des Jira Boards.
Andererseits haben wir zu Beginn des Projekts zu wenig am Projekt gearbeitet, sodass wir nun viel Zeit in die Fertigstellung stecken müssen. Auch fehlende Transparenz/Statuseinbindung von Tools (Logs des Backend sind nicht für alle einsehbar, teilweise fehlende Benachrichtigungen zu neuen PRs/Kommentaren usw.) haben die Arbeit erschwert oder verlangsamt.

Ändern könnte man, dass früher mit dem Bearbeiten von Aufgaben begonnen werden könnte. Auch gäbe es die Möglichkeit, dass alle für PR-Updates die GitHub-App installieren, E-Mail Benachrichigungen aktivieren oder ein Discord-Webhook erstellt wird.

Daraus haben sich folgende ToDos ergeben:

    1. Die Priorität sollte auf den MVP gesetzt werden (alle zusätzlichen Features auslassen, Fokus auf Kern-Features)
    -> siehe 3.
    2. Es sollte abgewogen werden, ob Priorität auf das Lernen für Klausuren oder auf die Perfektionierung des Projekts gelegt wird
    -> Ergebnis: Mittelding, aber wegen begrenzter Zeit steht Projekt mglw. eher im Vordergrund
    3. Es sollte definiert werden, wann das Projekt als "fertig" gilt und welche Features dazu gehören
    -> Ergebnis: Es fehlen noch Siegpunkte (Basis fast fertig), Bauen von Straßen und Siedlungen, Ressourcenvergabe nach Würfeln
    4. Für den nächsten Blogpost sollte jetzt entschieden werden, ob jeder dem Ersteller schickt oder der Ersteller nachfragt
    -> Ergebnis: Postersteller fragt nach (wurde für diesen Blogpost bereits umgesetzt)

## Langfassung

Datum: Dienstag, 25.05.2026
Uhrzeit: 19:30-20:30 Uhr
Teilnehmer: @ultra-ms, @A1exHorst, @JaskerX

### ✅ Weiter (z.B. "Was hat uns vorangebracht?") 

- Die strikte Einhaltung von Scrum mit korrekt gepflegtem Jira-Board wurde beibehalten. [@ultra-ms]
- Die Hilfsbereitschaft untereinander, insbesondere das Erklären von Entscheidungen gegenüber Co-Entwicklern und das Nutzen von Pull-Requests, war förderlich. [@ultra-ms]
- Eine flexible Planung und Projektstruktur mit gerechter Aufgabenverteilung und spontanen Terminen wurde praktiziert. [@ultra-ms]
- Die individuellen Kompetenzen der Projektmitglieder wurden berücksichtigt. [@ultra-ms]
- Verbesserungsvorschläge über Pull-Request-Kommentare wurden eingebracht. [@ultra-ms]
- Automatisierungen (Setup-Skript für Backend und Datenbank, Jira-Board-Blogpost-Automatisierung) wurden umgesetzt. [@ultra-ms]
- Das Bestreben, alle Aufgaben des Sprints vor der Deadline zu erledigen, war erkennbar. [@ultra-ms]
- Regelmäßige Kommunikation über die Aufgabenverteilung (Sprint Planning, nach Vorlesungen, über Discord) half, Überschneidungen zu vermeiden. [@A1exHorst]
- Zeitdruck steigerte die Produktivität (bei einzelnen Mitgliedern). [@A1exHorst]
- Pull Requests verbesserten den Einblick in die Arbeit der anderen und hielten die Codebase transparenter. [@A1exHorst]
- Häufige wöchentliche Plannings ermöglichten einen regelmäßigen Austausch über den Entwicklungsstand und die Zuweisung neuer Aufgaben aus dem Backlog. [@JaskerX]
- Die Abwägung zwischen gemeinsamer und unabhängiger Bearbeitung der wöchentlichen Aufgaben reduzierte den Bedarf an langen gemeinsamen Terminen. [@JaskerX]
- Das selbstständige Suchen und Erkennen neuer Aufgaben (z. B. Anlegen einer Story im Backlog bei fehlenden Aufgaben oder neuen Erkenntnissen) vermied Leerlauf. [@JaskerX]
- Die Definition einiger Guidelines im Laufe der Entwicklung führte zu einheitlicherer Arbeit. [@JaskerX]
- Die Jira-Automation für die Zuweisung von Blogpost- und Kommentar-Aufgaben sorgte für bessere Sichtbarkeit auf dem Board. [@JaskerX]

### ❌ Stopp (z.B. "Was hat uns zurückgehalten?")

- Eine sehr konservative Aufgabenplanung führte dazu, dass im ersten Semester wenig am Projekt entwickelt wurde. [@ultra-ms]
- Ungewollte Aktionen (z. B. versehentliches Mergen) sollten reduziert werden. [@ultra-ms]
- Feature-Creep war erkennbar (viele Ideen, aber kaum umgesetzt). [@ultra-ms]
- Undokumentierte Änderungen an Grundstrukturen und Abläufen führten dazu, dass die Dokumentation nicht aktuell gehalten wurde. [@ultra-ms]
- Die begrenzte Transparenz von Tools (Render, Jira, SonarQube) erschwerte die Arbeit. [@ultra-ms]
- Dass Render-Logs nicht geteilt werden können, kostete Zeit bei der Kommunikation (musste jeweils selbst gemacht werden). [@A1exHorst]
- Fehlende Synchronisation bezüglich des Status von Pull Requests erschwerte den Überblick (z. B. ob neue PRs erstellt, Kommentare beantwortet oder Änderungen genehmigt wurden). [@JaskerX]
- Ein zu später oder fehlender Austausch über den Inhalt des Blogposts (was wurde gemacht) trat auf. [@JaskerX]

### ❔ Überlegen (z.B. "Wie könnten wir es anders machen?")

- Es sollte überlegt werden, ob anfallende Aufgaben konsequent als Bug/Story notiert und im nächsten Sprint behandelt werden (Priorisierung von Features). [@ultra-ms]
- Es sollte überlegt werden, ob auch Aufgaben aufgenommen werden, die nicht gestartet wurden oder an denen nicht aktiv gearbeitet wird. [@ultra-ms]
- Zeitdruck ließe sich vermeiden, indem früher mit der Entwicklung begonnen wird. [@ultra-ms]
- Früheres Loslegen könnte die Produktivität verbessern. [@A1exHorst]
- Ein Zeitplan mit Meilensteinen zu Beginn könnte helfen, das Projekt nicht zu vergessen und rechtzeitig abzuschließen. [@A1exHorst]
- Jedes Mitglied könnte die GitHub-App installieren oder E-Mail-Benachrichtigungen aktivieren (Alternativ: Discord-Webhook für Updates in einem Channel). [@JaskerX]
- Jedes Mitglied könnte dem Blogpost-Ersteller bis zu einer bestimmten Zeit (z. B. Dienstag 18 Uhr) eine Nachricht mit seinen Aktivitäten schicken. Oder der Ersteller fragt rechtzeitig bei den anderen nach. [@JaskerX]

### ❕ Handeln (z.B. "Was sollten wir als nächstes tun?")

- Die Priorität sollte auf den MVP gesetzt werden (alle zusätzlichen Features auslassen, Fokus auf Kern-Features). [@ultra-ms]
- Es sollte abgewogen werden, ob Priorität auf das Lernen für Klausuren oder auf die Perfektionierung des Projekts gelegt wird. [@A1exHorst]
- Es sollte definiert werden, wann das Projekt als "fertig" gilt und welche Features dazu gehören. [@A1exHorst]
- Für den nächsten Blogpost sollte jetzt entschieden werden, ob jeder dem Ersteller schickt oder der Ersteller nachfragt. -> nachfragen [@JaskerX]
