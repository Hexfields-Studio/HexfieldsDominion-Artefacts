# Code Conventions (Frontend)

Version 1 – 14.04.2026  
Affected Repository: [Hexfields-Studio/HexfieldsDominion](https://github.com/Hexfields-Studio/HexfieldsDominion)

This document defines the coding conventions for the frontend codebase. All contributors are expected to follow these guidelines.

---

## 1. File Management

### Directory Structure

- Place all source code that directly implements application logic under `src/`.
- Store static website assets (images, fonts, etc.) in `public/`.
- Group multiple files of the same style or purpose into a dedicated folder.
- Configuration files, setup scripts, and README files may reside in the repository root.
- Use the `.github` and `.vscode` folders for CI/CD and editor infrastructure configurations.

### Naming Conventions

- Files and folders must be named using CamelCase.  
  - Do: `underscore_naming`, `ALLCAPS`, `kebab-case`  
  - Don't: `MyComponent.tsx`, `useAuth.ts`, `ApiClient/`
- Exceptions:
  - `node_modules` (package manager standard)
  - `README.md` (conventional filename)

### Security & Ignored Files

- Never commit secrets (e.g., environment files, API keys).  
  Add them to `.gitignore` and use environment variable placeholders (e.g., `.env.example`).

---

## 2. Language & Comments

- Code must be written in English with clear, self-explanatory naming.  
  - Do: `DashboardPage`
  - Don't: `MySuperCoolDashPage`
- Comments are written in English. German expressions are acceptable only for terms without a direct English equivalent (e.g., domain‑specific vocabulary).

---

## 3. Version Control – Commits & Branches

- Do not push directly to the `main` branch except in urgent emergency cases.
- Always work in a dedicated branch:
  - `feature/*` for new functionality
  - `bugfix/*` for issue resolutions
- Open a Pull Request (PR) for your branch. Resolve any merge conflicts cleanly before merging.
- Keep the Git history clean and understandable.
- Commit in meaningful batches and avoid multiple tiny commits that clutter.

---

## 4. JavaScript / TypeScript Guidelines

### Variable & Component Naming

- Use CamelCase for all variables, functions, and React functional components (`React.FC`).

### Spacing & Punctuation

- No trailing spaces anywhere in the code.
- Always use trailing commas in multi‑line arrays, objects, and function parameters.
- Always terminate statements with semicolons.
- This is usually handled by the ESLint configuration.

### Type Definitions

- Declare TypeScript types with care and precision. Avoid unnecessary `any` usage.

---

## 5. General Principles

- Exceptions to any rule are allowed with prior communication to the rest of the development team.
- Follow the KISS principle (Keep It Simple, Stupid).
- These conventions are living documents and may be updated at any time.
