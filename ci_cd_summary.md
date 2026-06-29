# CI/CD-Pipeline-Struktur - Hexfields: Dominion - Version 1.0

## Änderungsverzeichnis

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 29/Jun/2025 | 1.0 | Initiale Erstellung | Marcel |

## Inhaltsverzeichnis

- [CI/CD-Pipeline-Struktur - Hexfields: Dominion - Version 1.0](#cicd-pipeline-struktur---hexfields-dominion---version-10)
  - [Änderungsverzeichnis](#änderungsverzeichnis)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Zweck des Dokuments](#zweck-des-dokuments)
  - [Betroffene Repositories](#betroffene-repositories)
  - [Geltungsbereich](#geltungsbereich)
  - [Frontend-Repository `HexfieldsDominion`](#frontend-repository-hexfieldsdominion)
    - [Workflow-Datei](#workflow-datei)
    - [Ziel innerhalb der Entwicklung](#ziel-innerhalb-der-entwicklung)
    - [Trigger](#trigger)
    - [Berechtigungen und Ausführungssteuerung](#berechtigungen-und-ausführungssteuerung)
    - [Job-Struktur](#job-struktur)
      - [Job `build`](#job-build)
      - [Job `test` (`needs: build`)](#job-test-needs-build)
      - [Job `deploy` (`needs: [build, test]`)](#job-deploy-needs-build-test)
    - [Erweiterungspunkte (technisch)](#erweiterungspunkte-technisch)
  - [Backend-Repository `HexfieldsDominion-Backend`](#backend-repository-hexfieldsdominion-backend)
    - [Workflow-Datei](#workflow-datei-1)
    - [Ziel innerhalb der Entwicklung](#ziel-innerhalb-der-entwicklung-1)
    - [Trigger](#trigger-1)
    - [Berechtigungen](#berechtigungen)
      - [Job `build`](#job-build-1)
      - [Job `test`](#job-test)
    - [Job-Struktur](#job-struktur-1)
      - [Job `build`](#job-build-2)
      - [Job `test`](#job-test-1)
    - [Erweiterungspunkte (technisch)](#erweiterungspunkte-technisch-1)

## Zweck des Dokuments

Dieses Dokument beschreibt die bestehende CI/CD-Pipeline-Struktur.  
Ziel ist die technische Nachvollziehbarkeit von Aufbau, Ablauf, Verantwortlichkeiten und Erweiterungspunkten der vorhandenen Workflows.

## Betroffene Repositories

- `Hexfields-Studio/HexfieldsDominion`: [Direkt-Link zur Pipeline](https://github.com/Hexfields-Studio/HexfieldsDominion/blob/main/.github/workflows/ci-cd.yml)
- `Hexfields-Studio/HexfieldsDominion-Backend`: [Direkt-Link zur Pipeline](https://github.com/Hexfields-Studio/HexfieldsDominion/blob/main/.github/workflows/ci-cd.yml)

## Geltungsbereich

Dokumentiert werden ausschließlich die aktuell vorhandenen GitHub-Actions-Workflows der oben genannten Repositories.

* * *

## Frontend-Repository `HexfieldsDominion`

### Workflow-Datei

- `.github/workflows/ci-cd.yml`
- Workflow-Name: `CI/CD Pipeline`

### Ziel innerhalb der Entwicklung

- Sicherstellen, dass Frontend-Änderungen auf `main` baubar und testbar bleiben.
- Deployment-Artefakt für GitHub Pages standardisiert erzeugen.
- Deployment auf `main` nur nach erfolgreichem Build- und Testlauf.

### Trigger

- `push` auf `main`
- `pull_request` auf `main`

### Berechtigungen und Ausführungssteuerung

- `contents: read`
- `pages: write`
- `id-token: write`
- `concurrency` aktiv mit Gruppe `pages-${{ github.ref }}` und `cancel-in-progress: true`

### Job-Struktur

#### Job `build`

**Laufzeitumgebung:** `ubuntu-latest`

**Ablauf:**
1. Checkout (`actions/checkout@v4`)
2. Bun-Setup (`oven-sh/setup-bun@v1`, `bun-version: latest`)
3. Abhängigkeiten installieren (`bun install`)
4. Build ausführen (`bun run build`) mit `VITE_API_URL` aus Secrets
5. Build-Artefakt `dist` hochladen (`actions/upload-artifact@v4`)

**Ergebnis:**
- Reproduzierbares Frontend-Build-Artefakt `dist` für Folgeschritte.

#### Job `test` (`needs: build`)

**Laufzeitumgebung:** `ubuntu-latest`

**Ablauf:**
1. Checkout (`actions/checkout@v4`)
2. Bun-Setup (`oven-sh/setup-bun@v1`)
3. Abhängigkeiten installieren (`bun install`)
4. Build-Artefakt `dist` herunterladen (`actions/download-artifact@v4`)
5. Type-Check (`bun run type-check`)
6. Tests ausführen (`bun run test`)

**Ergebnis:**
- Technische Validierung des Builds durch statische Typprüfung und Tests.

#### Job `deploy` (`needs: [build, test]`)

**Bedingung:**
- Nur bei `github.ref == 'refs/heads/main'`

**Environment:**
- `github-pages` (inkl. URL aus Deployment-Output)

**Ablauf:**
1. Build-Artefakt `dist` herunterladen
2. Pages konfigurieren (`actions/configure-pages@v4`)
3. Pages-Artefakt hochladen (`actions/upload-pages-artifact@v3`)
4. Deployment ausführen (`actions/deploy-pages@v4`)

**Ergebnis:**
- Automatisiertes Deployment der statischen Frontend-Build-Ausgabe auf GitHub Pages.

### Erweiterungspunkte (technisch)

- Vor `test`: separater Lint-Job als zusätzliches Gate.
- Vor `deploy`: Security-/Dependency-Checks (z. B. Audit) als harte Bedingung.
- Optional: PR-Preview-Deployments über separates Environment.
- Optional: Reusable Workflow für standardisierte Frontend-Pipelines.

* * *

## Backend-Repository `HexfieldsDominion-Backend`

### Workflow-Datei

- `.github/workflows/gradle.yml`
- Workflow-Name: `Java CI with Gradle`

### Ziel innerhalb der Entwicklung

- Sicherstellen, dass Backend-Änderungen auf CI-Ebene kompilierbar sind.
- Testausführung, Coverage-Generierung und SonarCloud-Analyse im Workflow verankern.
- Prüfergebnisse als Artefakt bereitstellen.

### Trigger

- `push` (ohne Branch-Einschränkung)
- `pull_request` bei Typen:
  - `opened`
  - `synchronize`
  - `reopened`

### Berechtigungen

#### Job `build`
- `contents: read`

#### Job `test`
- `contents: read`
- `pull-requests: write` (für SonarCloud-Kommentierung in PR-Kontexten)

### Job-Struktur

#### Job `build`

**Laufzeitumgebung:** `ubuntu-latest`

**Ablauf:**
1. Checkout (`actions/checkout@v4`)
2. JDK 21 Setup (`actions/setup-java@v4`, Distribution `temurin`)
3. Gradle Setup (`gradle/actions/setup-gradle@...` pinned)
4. Wrapper ausführbar machen (`chmod +x ./gradlew`)
5. Build ohne Tests (`./gradlew build -x test`)

**Ergebnis:**
- Sicherstellung der Build-Fähigkeit (ohne Testlauf) als separater CI-Schritt.

#### Job `test`

**Laufzeitumgebung:** `ubuntu-latest`

**Ablauf:**
1. Checkout mit `fetch-depth: 0`
2. JDK 21 Setup
3. Gradle Setup
4. Wrapper ausführbar machen
5. Sonar-Cache wiederverwenden (`~/.sonar/cache`)
6. Test, Coverage und SonarCloud ausführen:
   - `./gradlew test jacocoTestReport sonar --info`
7. JaCoCo-Report hochladen (`actions/upload-artifact@v4`, `build/reports/jacoco/`)

**Ergebnis:**
- Automatisierte Testausführung.
- Coverage-Erstellung.
- [SonarCloud-Analyse](https://sonarcloud.io/summary/overall?id=Hexfields-Studio_HexfieldsDominion-Backend&branch=main) innerhalb des CI-Laufs.
- Persistente Bereitstellung des Coverage-Reports als Artefakt.

### Erweiterungspunkte (technisch)

- `needs: build` am `test`-Job zur expliziten Reihenfolgekontrolle.
- Branch-Filter für `push` zur gezielteren Ressourcennutzung.
- Ergänzung einer Deploy-Stage (z. B. Staging) als separater Job.
- Zusätzliche Gates für Mindest-Coverage und Security-Scanning.
- Optional: Reusable Workflow für Java-Services mit identischem Qualitätsprofil.