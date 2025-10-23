# Use-Case Spezifikation: Light-Dark Mode

## 1. Light-Dark Mode

### 1.1 Beschreibung

Dieses Use-Case ermöglicht es dem User, zwischen Hell- und Dunkelmodus der Applikation zu wechseln.

### 1.2 Mockup

![light_dark_mode_mockup](./light_dark_mode_mockup.drawio.png "light_dark_mode_mockup")

### 1.3 Screenshot

n/a

## 2. Ablauf von Ereignissen

### 2.1 Grundlegender Ablauf

- Der User befindet sich auf der Home-, Start- oder Spielseite.
- Der User klickt auf das Einstellungs-Icon in der oberen rechten Ecke.
- Das Einstellungsmenü wird geöffnet.
- Der User klickt auf "Light Mode" bzw. "Dark Mode".
- Das Farbschema der Applikation wechselt sofort.
- Die Einstellung wird gespeichert.
- Das Einstellungsmenü schließt sich automatisch nach kurzer Zeit oder nach Klick außerhalb.

#### Sequenz-Diagramm

![light_dark_mode_sequence](./light_dark_mode_sequence.png "light_dark_mode_sequence")

#### Aktivitäts-Diagramm (Mermaid)

```mermaid
flowchart TD
    A([Start]) --> B[Einstellungs-Icon klicken]
    B --> C[Einstellungsmenü öffnen]
    C --> D{Modus bereits gespeichert?}
    D -- Ja --> E[Gespeicherten Modus anzeigen]
    D -- Nein --> F[Standard/System-Modus anzeigen]
    E --> G{Benutzeraktion}
    F --> G
    G --> H[Light Mode auswählen]
    G --> I[Dark Mode auswählen]
    G --> J[Systempräferenz folgen]
    H --> K[Farbschema sofort wechseln]
    I --> K
    J --> L[Modus an OS anpassen]
    L --> K
    K --> M[Einstellung lokal speichern]
    M --> N[Visuelle Bestätigung anzeigen]
    N --> O[Einstellungsmenü schließen]
    O --> P([Ende])
```

### 2.2 Alternative Abläufe

- **System-Preference folgen**: Automatische Anpassung an Systemeinstellung des Betriebssystems

## 3. Besondere Anforderungen

- Der Modus muss auf allen Bildschirmen (Home, Start, Spiel) konsistent angewendet werden
- Die Einstellung muss persistent gespeichert werden
- Sofortige visuelle Rückmeldung bei Moduswechsel
- Barrierefreiheit: Ausreichender Kontrast in beiden Modi

## 4. Vorbedingungen

- Die Applikation ist geöffnet
- Der User befindet sich auf Home-, Start- oder Spielseite

## 5. Nachbedingungen

- Das Farbschema wurde geändert
- Die Einstellung wurde gespeichert
- Die Applikation behält den gewählten Modus bei zukünftigen Starts

## 6. Story Points

n/a
