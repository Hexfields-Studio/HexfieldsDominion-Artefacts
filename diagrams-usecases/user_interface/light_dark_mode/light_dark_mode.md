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
- Das Einstellungsmenü schließt sich automatisch.

#### Sequenz-Diagramm

```mermaid
sequenceDiagram
    title Light-Dark Mode Wechsel - Detaillierte Implementierung

    participant User
    participant UI as Benutzeroberfläche
    participant Storage as Lokaler Speicher

    User->>UI: Seitenaufruf
    activate UI
    UI->>Storage: getItem('theme')
    activate Storage
    Storage-->>UI: theme ("light"/"dark"/null)
    deactivate Storage
    
    alt theme = "light"/"dark"
        UI->>UI: Zeigt gespeicherten Modus an
    else theme = null
        UI->>UI: Ermittelt System-Präferenz
        UI->>UI: Wendet System-Präferenz an
    end
    deactivate UI

    User->>UI: Klickt Einstellungs-Icon
    activate UI
    UI-->>User: Einstellungsmenü öffnen

    User->>UI: Wählt hellen/dunklen Modus
    UI->>UI: Farbschema wechseln
    UI-->>User: Neues Farbschema angezeigt
    UI->>Storage: setItem('theme', newTheme)
    activate Storage
    Storage-->>UI: (OK)
    deactivate Storage
    
    User->>UI: Klickt Einstellungs-Icon
    UI-->>User: Einstellungsmenü schließen
    deactivate UI
```

#### Aktivitäts-Diagramm (Mermaid)

```mermaid
---
title: Light/Dark Mode Aktivitätsdiagramm
---
flowchart TD
    A([Start]) --> B[Einstellungs-Icon klicken]
    B --> C[Einstellungsmenü öffnen]
    C --> D{Modus bereits gespeichert?}
    D -- Ja --> E[Gespeicherten Modus anzeigen]
    D -- Nein --> F[System-Modus anzeigen]
    E --> G{Useraktion}
    F --> G
    G --> H[Hellen Modus auswählen]
    G --> I[Dunklen Modus auswählen]
    H --> K[Farbschema sofort wechseln]
    I --> K[Einstellung lokal speichern]
    K --> M[Einstellungsmenü schließen]
    M --> N([Ende])
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
