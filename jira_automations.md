Falls als "berücksichtigte Workitems" 100 angezeigt wird, existieren vmtl. mehr als 100 relevante Workitems und es muss z.B. eine zeitliche Einteilung erfolgen.

Die Ausführung muss mindestens initial direkt über ein Workitem und nicht über den Automations-Button über dem Bord erfolgen, damit Automations angezeigt werden.

# Zeiterfassung
## Summe Zeiterfassung pro Phase
*Argumente*: Name der Phase, z.B. "construction" (also nicht der ganze Tag wie "phase-construction")
## Summe Zeiterfassung pro Workflow
*Argumente*: Name des Workflows, z.B. "implementation" (also nicht der ganze Tag wie "workflow-implementation")
## (NUR ASSIGNED) [alltime] Summe von "Zeiterfassung" pro User
*Argumente*: Name des Users  
Summe der getrackten Zeit aller Workitems, die dem angegebenen User zugewiesen sind.
## (NUR ASSIGNED) [4. Sem.] Summe von "Zeiterfassung" pro User
-> Siehe Automation für "alltime", aber begrenzt auf Workitems, deren Status innerhalb von 2026 auf "Fertig" geändert wurde.

# Sonstige
## Blog Story bei neuem Sprint erstellen
Bei Erstellung eines neuen Sprints im Backlog wird diesem Sprint automatisch eine Story hinzugefügt. Sie enthält einen Task für den Blogeintrag und zwei Tasks für die Kommentare. Diese Tasks werden entsprechend der Sprintnummer (mod 3) zugewiesen.
