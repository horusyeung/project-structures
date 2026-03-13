# i18n Conventions

> Shared internationalisation standards across all project templates.

## General Rules

- **Never use raw text in components.** Always reference a translation key.
- Add keys to **all language files** before using them in code.
- Locale files live in `src/common/locales/` (`en.json`, `zh_HK.json`) — same location across all projects.

## i18n Libraries by Platform

| Platform | Library | Notes |
| --- | --- | --- |
| React (Vite) | react-i18next + i18next + i18next-browser-languagedetector | Browser locale detection |
| Next.js 15 | next-intl | With a translation management platform (e.g., Phrase, Crowdin, Lokalise) |
| React Native CLI | i18next + react-i18next + react-native-localize | Device locale detection |
| Expo | i18next + react-i18next + react-native-localize | Device locale detection |
| Monorepo | next-intl (per app) | Shared locale files in `packages/common/src/locales/` |

> **Note:** React Native projects require the `intl-pluralrules` polyfill for Hermes pluralisation support.

---

## Key Naming Convention

Use a **nested JSON structure** with **kebab-case** keys. The top-level keys group translations by page or scope (`common` for shared components). Nested keys use kebab-case and describe the component or element.

### Structure Pattern

```
{
  "page-name": {
    "component-description": "Translation value"
  },
  "common": {
    "component-description": "Translation value"
  }
}
```

### Access Pattern

Access keys using dot notation: `t('login-page.submit-btn')`, `t('common.confirm-btn')`.

### Rules

- Always use **kebab-case** for all keys — no camelCase, no snake_case.
- **Top-level keys** are page names (or `common` for shared components).
- **Nested keys** describe the component or element (`submit-btn`, `email-input-placeholder`, `confirmation-title`).
- Keep keys **descriptive but concise** — translators should understand the context without seeing the UI.
- Nesting should be **at most 2 levels deep** to keep access patterns simple.

---

## Example Locale File

`en.json`:

```json
{
  "common": {
    "confirm-btn": "Confirm",
    "cancel-btn": "Cancel",
    "network-error-message": "Network error. Please try again.",
    "delete-confirmation-title": "Are you sure you want to delete?"
  },
  "login-page": {
    "submit-btn": "Sign In",
    "email-input-placeholder": "Enter your email",
    "email-input-error": "Please enter a valid email address",
    "forgot-password-link": "Forgot your password?"
  },
  "dashboard-page": {
    "total-revenue-title": "Total Revenue"
  },
  "feature1-list-page": {
    "empty-state-description": "No items found"
  },
  "feature1-detail-page": {
    "header-title": "Feature 1 Detail"
  }
}
```

---

## Locale File Location

| Project Type | Location |
| --- | --- |
| React (Vite) | `src/common/locales/` |
| Next.js 15 | `src/common/locales/` |
| React Native CLI | `src/common/locales/` |
| Expo | `src/common/locales/` |
| Monorepo (shared) | `packages/common/src/locales/` |
| Monorepo (app-specific) | `apps/{app}/src/common/libs/i18n/` |
