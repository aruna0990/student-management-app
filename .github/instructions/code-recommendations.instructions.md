---
description: "Use when writing, reviewing, or modifying JavaScript, HTML, or CSS for the student-management-app. Covers localStorage patterns, security, UI conventions, and code style."
applyTo: "**/*.js,**/*.html,**/*.css"
---

# Code Recommendations — Student Management App

## JavaScript

- Always use `const` / `let`; never use `var`.
- Use `crypto.randomUUID()` for generating unique IDs.
- Prefer `textContent` over `innerHTML` for plain-text values; use DOM methods (`createElement`, `appendChild`) to build dynamic elements instead of string-interpolated `innerHTML`.
- Validate inputs at the boundary (form submit handler) before calling core functions.
- Core functions (`addStudent`, `listStudents`, `deleteStudent`) must remain pure data-layer functions — no DOM access inside them.
- Use `parseInt(value, 10)` (always pass radix 10) for numeric parsing.
- Email validation must use the regex `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`.
- Prevent duplicate records by checking for existing email (case-insensitive) before saving.
- Store data under a named constant key (`STORAGE_KEY`) — never hard-code the localStorage key string inline.
- Always `JSON.parse` / `JSON.stringify` when reading from / writing to `localStorage`. Wrap reads in a try/catch and default to `[]`.

## HTML

- Every `<input>` must have a matching `<label>` using `for` / `id` pairing (accessibility).
- Use semantic elements: `<section>`, `<h1>`–`<h3>`, `<form>`, `<button type="submit">`.
- External CSS and JS are linked via `<link rel="stylesheet">` and `<script src>` at the end of `<body>`.
- Set `lang` attribute on `<html>` and include `<meta charset="UTF-8">` and viewport meta.

## CSS

- Use CSS custom properties (variables) for repeated colors/spacing when the palette grows.
- Prefer `rem` / `em` for font sizes; `px` for borders and fixed radii.
- Use `box-sizing: border-box` globally.
- Card hover states must use `transition` for smooth animation — no instant jumps.
- Responsive breakpoints via `@media (max-width: 480px)` — mobile-first where practical.
- Avoid `!important`; increase specificity instead.

## localStorage

- Always guard against corrupt/missing data:
  ```js
  function getStudentsFromStorage() {
    try {
      const data = localStorage.getItem(STORAGE_KEY);
      return data ? JSON.parse(data) : [];
    } catch {
      return [];
    }
  }
  ```
- Never store sensitive data (passwords, tokens) in `localStorage`.

## Security (OWASP)

- Prefer `textContent` and DOM methods so no user data is ever passed through an HTML parser.
- If `innerHTML` must be used, escape all dynamic values with `escapeHtml()` first.
- Validate and sanitize on the client before storage; re-validate any data read back from storage before display.

## Naming Conventions

| Element | Convention | Example |
|---|---|---|
| JS functions | camelCase | `addStudent`, `renderStudentList` |
| JS constants | UPPER_SNAKE_CASE | `STORAGE_KEY` |
| CSS classes | kebab-case | `.student-card`, `.delete-btn` |
| HTML ids | camelCase | `studentName`, `formMessage` |

## File Structure

```
student-management-app/
├── index.html       # Markup only — no inline JS or styles
├── style.css        # All styles
└── app.js           # All logic + UI rendering
```
