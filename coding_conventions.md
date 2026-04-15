# Code Conventions (Frontend)

## Preamble / Präambel

Version 2 - 15.04.2026  
Repository: [Hexfields-Studio/HexfieldsDominion](https://github.com/Hexfields-Studio/HexfieldsDominion)

- EN: This document defines the coding conventions for the frontend codebase. All contributors are expected to follow these guidelines.
- DE: Dieses Dokument definiert die Codierungsrichtlinien für den Frontend-Code. Alle Beitragenden werden gebeten, diese Richtlinien einzuhalten.

---

## Table of Contents / Inhaltsverzeichnis

- [Code Conventions (Frontend)](#code-conventions-frontend)
  - [Preamble / Präambel](#preamble--präambel)
  - [Table of Contents / Inhaltsverzeichnis](#table-of-contents--inhaltsverzeichnis)
  - [EN - English Version](#en---english-version)
    - [1. File Management](#1-file-management)
      - [Directory Structure](#directory-structure)
      - [Naming Conventions](#naming-conventions)
      - [Security \& Ignored Files](#security--ignored-files)
    - [2. Language \& Comments](#2-language--comments)
    - [3. Version Control – Commits \& Branches](#3-version-control--commits--branches)
    - [4. JavaScript / TypeScript Guidelines](#4-javascript--typescript-guidelines)
      - [Variable \& Component Naming](#variable--component-naming)
      - [Spacing \& Punctuation](#spacing--punctuation)
      - [Type Definitions](#type-definitions)
    - [5. General Principles](#5-general-principles)
  - [DE - Deutsche Version](#de---deutsche-version)
    - [1. Dateiverwaltung](#1-dateiverwaltung)
      - [Verzeichnisstruktur](#verzeichnisstruktur)
      - [Benennungskonventionen](#benennungskonventionen)
      - [Sicherheit \& ignorierte Dateien](#sicherheit--ignorierte-dateien)
    - [2. Sprache \& Kommentare](#2-sprache--kommentare)
    - [3. Versionsverwaltung – Commits \& Branches](#3-versionsverwaltung--commits--branches)
    - [4. JavaScript / TypeScript Richtlinien](#4-javascript--typescript-richtlinien)
      - [Variablen- \& Komponentenbenennung](#variablen---komponentenbenennung)
      - [Abstände \& Satzzeichen](#abstände--satzzeichen)
      - [Typdefinitionen](#typdefinitionen)
    - [5. Allgemeine Grundsätze](#5-allgemeine-grundsätze)

---

## EN - English Version

### 1. File Management

#### Directory Structure

- Place all source code that directly implements application logic under `src/`.
- Store static website assets (images, fonts, etc.) in `public/`.
- Group multiple files of the same style or purpose into a dedicated folder.
- Configuration files, setup scripts, and README files may reside in the repository root.
- Use the `.github` and `.vscode` folders for CI/CD and editor infrastructure configurations.

#### Naming Conventions

- Files and folders must be named using CamelCase.  
  - Do: `underscore_naming`, `ALLCAPS`, `kebab-case`  
  - Don't: `MyComponent.tsx`, `useAuth.ts`, `ApiClient/`
- Exceptions:
  - `node_modules` (package manager standard)
  - `README.md` (conventional filename)

#### Security & Ignored Files

- Never commit secrets (e.g., environment files, API keys).  
  Add them to `.gitignore` and use environment variable placeholders (e.g., `.env.example`).

### 2. Language & Comments

- Code must be written in English with clear, self-explanatory naming.  
  - Do: `DashboardPage`
  - Don't: `MySuperCoolDashPage`
- Comments are written in English. German expressions are acceptable only for terms without a direct English equivalent (e.g., domain‑specific vocabulary).

### 3. Version Control – Commits & Branches

- Do not push directly to the `main` branch except in urgent emergency cases.
- Always work in a dedicated branch:
  - `feature/*` for new functionality
  - `bugfix/*` for issue resolutions
- Open a Pull Request (PR) for your branch. Resolve any merge conflicts cleanly before merging.
- Keep the Git history clean and understandable.
- Commit in meaningful batches and avoid multiple tiny commits that clutter.

### 4. JavaScript / TypeScript Guidelines

#### Variable & Component Naming

- Use CamelCase for all variables, functions, and React functional components (`React.FC`).

#### Spacing & Punctuation

- No trailing spaces anywhere in the code.
- Always use trailing commas in multi‑line arrays, objects, and function parameters.
- Always terminate statements with semicolons.
- This is usually handled by the ESLint configuration.

#### Type Definitions

- Declare TypeScript types with care and precision. Avoid unnecessary `any` usage.

### 5. General Principles

- Exceptions to any rule are allowed with prior communication to the rest of the development team.
- Follow the KISS principle (Keep It Simple, Stupid).
- These conventions are living documents and may be updated at any time.

---

## DE - Deutsche Version

### 1. Dateiverwaltung

#### Verzeichnisstruktur

- Lege allen Quellcode, der direkt Anwendungslogik implementiert, unter `src/` ab.
- Speichere statische Website-Assets (Bilder, Schriftarten usw.) in `public/`.
- Fasse mehrere Dateien gleichen Stils oder Zwecks in einem eigenen Ordner zusammen.
- Konfigurationsdateien, Setup-Skripte und README-Dateien können im Repository-Root liegen.
- Verwende die Ordner `.github` und `.vscode` für CI/CD- und Editor-Konfigurationen.

#### Benennungskonventionen

- Dateien und Ordner müssen mit CamelCase benannt werden.  
  - Richtige Beispiele: `underscore_naming`, `ALLCAPS`, `kebab-case`  
  - Falsche Beispiele: `MyComponent.tsx`, `useAuth.ts`, `ApiClient/`
- Ausnahmen:
  - `node_modules` (Package-Manager-Standard)
  - `README.md` (konventioneller Dateiname)

#### Sicherheit & ignorierte Dateien

- Commits mit Geheimnissen (z. B. Umgebungsdateien, API-Schlüssel) sind zu vermeiden.  
  Füge sie zu `.gitignore` hinzu und verwende Platzhalter für Umgebungsvariablen (z. B. `.env.example`).

### 2. Sprache & Kommentare

- Der Code muss auf Englisch geschrieben sein und klare, selbsterklärende Namen verwenden.  
  - Richtige Beispiele: `DashboardPage`  
  - Falsche Beispiele: `MySuperCoolDashPage`
- Kommentare sind auf Englisch zu verfassen. Deutsche Ausdrücke sind nur für Begriffe zulässig, die im Domänenkontext keinen direkten englischen Ersatz haben.

### 3. Versionsverwaltung – Commits & Branches

- Nicht direkt in den `main`-Branch pushen, außer in dringenden Notfällen.
- Arbeite stets in einem eigenen Branch:
  - `feature/*` für neue Funktionalität
  - `bugfix/*` für Fehlerbehebungen
- Eröffne einen Pull Request (PR) für deinen Branch. Löst Merge-Konflikte sauber, bevor du ihn zusammenführst.
- Halte die Git-Historie sauber und verständlich.
- Commits sollten sinnvoll gebündelt sein; vermeide viele winzige Commits, die das Repository unübersichtlich machen.

### 4. JavaScript / TypeScript Richtlinien

#### Variablen- & Komponentenbenennung

- Verwende CamelCase für alle Variablen, Funktionen und React-Funktionskomponenten (`React.FC`).

#### Abstände & Satzzeichen

- Keine überflüssigen Leerzeichen am Zeilenende.
- Verwende in mehrzeiligen Arrays, Objekten und Funktionsparametern immer abschließende Kommata.
- Beende jede Anweisung mit einem Semikolon.
- Dies wird in der Regel von der ESLint-Konfiguration durchgesetzt.

#### Typdefinitionen

- Deklariere TypeScript-Typen sorgfältig und präzise. Vermeide unnötige Verwendung von `any`.

### 5. Allgemeine Grundsätze

- Ausnahmen von Regeln sind möglich, aber nur nach vorheriger Abstimmung mit dem Team.
- Folge dem KISS-Prinzip (Keep It Simple, Stupid).
- Diese Richtlinien sind lebende Dokumente und können jederzeit angepasst werden.
