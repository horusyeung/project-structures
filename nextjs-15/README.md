# Next.js 15 Project Structure (App Router)

> Production-ready project structure for Next.js 15 with the App Router. Opinionated, scalable, and battle-tested.

## When to Use This

Use Next.js 15 when you need SSR/SSG/ISR, SEO, Server Components, Server Actions, middleware, or file-based routing. For pure SPAs behind authentication, see the [React.js (Vite)](../react-vite/) template instead.

---

## Project Structure

- The `src/app/` folder contains route segments organised by feature.
- The `src/common/` folder stores shared components, utilities, hooks, and configurations.
- The `src/features/` folder contains feature-scoped Redux slices, separated from the route tree.
- The `src/providers/` folder centralises all context/store providers.
- Everything outside `app/` is not routable.
- A `page.tsx` or `route.ts` must exist in a folder for it to become a route segment.
- Folders prefixed with `_` (e.g. `_components/`) are private by Next.js convention and excluded from routing.
- Folders wrapped in `()` (e.g. `(dashboard)/`) are route groups — they share layouts without affecting the URL.

```
.
├── .husky/
├── public/
│   └── assets/
├── src/
│   ├── app/
│   │   └── [locale]/
│   │       ├── (auth)/                          # route group — shared auth layout
│   │       │   ├── login/
│   │       │   │   └── page.tsx
│   │       │   └── register/
│   │       │       └── page.tsx
│   │       │
│   │       ├── (dashboard)/                     # route group — shared dashboard layout
│   │       │   ├── feature1/
│   │       │   │   ├── __tests__/
│   │       │   │   ├── _components/             # private folder (excluded from routing)
│   │       │   │   │   └── __tests__/
│   │       │   │   ├── _hooks/
│   │       │   │   ├── _lib/
│   │       │   │   ├── _styles/
│   │       │   │   ├── _types/
│   │       │   │   ├── actions.ts               # Server Actions
│   │       │   │   ├── loading.tsx              # streaming / suspense UI
│   │       │   │   ├── error.tsx                # error boundary
│   │       │   │   ├── layout.tsx
│   │       │   │   └── page.tsx
│   │       │   │
│   │       │   └── feature2/
│   │       │       └── ...
│   │       ├── (public)/                        # public pages
│   │       │   └── feature3/
│   │       │       └── ...
│   │       ├── layout.tsx                       # root locale layout
│   │       ├── page.tsx
│   │       ├── not-found.tsx
│   │       └── global-error.tsx
│   │
│   ├── common/
│   │   ├── __mocks__/
│   │   ├── api/
│   │   │   ├── rest/
│   │   │   │   ├── client.ts                    # axios instance
│   │   │   │   └── interceptors.ts
│   │   │   └── graphql/
│   │   ├── components/
│   │   │   └── __tests__/
│   │   ├── config/
│   │   ├── constants/
│   │   ├── hooks/
│   │   ├── layouts/
│   │   │   └── __tests__/
│   │   ├── locales/
│   │   │   ├── en.json
│   │   │   └── zh_HK.json
│   │   ├── lib/
│   │   │   └── redux/
│   │   │       ├── store.ts
│   │   │       └── hooks.ts                     # typed useAppDispatch / useAppSelector
│   │   ├── styles/
│   │   │   └── css/
│   │   ├── theme/
│   │   ├── types/
│   │   └── utils/
│   │
│   ├── features/                                # feature-scoped Redux slices
│   │   ├── feature1/
│   │   │   ├── slices/
│   │   │   │   └── feature1Slice.ts             # createAsyncThunk + reducers
│   │   │   ├── selectors.ts
│   │   │   └── types/
│   │   └── feature2/
│   │       └── ...
│   │
│   ├── providers/                               # centralised providers
│   │   ├── ReduxProvider.tsx
│   │   ├── ThemeProvider.tsx
│   │   └── I18nProvider.tsx
│   │
│   ├── i18n.ts
│   └── middleware.ts
│
├── tests/
│   ├── e2e/                                     # Playwright
│   ├── smoke/
│   └── regression/
│
├── .env.local
├── eslint.config.mjs                            # ESLint flat config
├── .gitignore
├── .prettierignore
├── .prettierrc.json
├── vitest.config.ts                             # Vitest (replaces Jest)
├── next-env.d.ts
├── next.config.ts                               # TypeScript config (Next 15)
├── package.json
├── README.md
├── tsconfig.json
└── yarn.lock
```

---

## Environments

| Environment | Description |
| --- | --- |
| UAT | User acceptance testing |
| Production | Live production environment |

---

## Tech Stack & Tooling

| Category | Tool / Library |
| --- | --- |
| UI Framework | [MUI (Material UI)](https://mui.com/material-ui/) |
| Localisation | next-intl with a translation management platform (e.g., Phrase, Crowdin, Lokalise) |
| State Management | Redux Toolkit + React hooks (`useState`, `useReducer`) |
| Unit Testing | Vitest + React Testing Library |
| E2E Testing | Playwright |
| Linting | ESLint (flat config) |
| Formatting | Prettier |
| Config Language | TypeScript (`next.config.ts`, `vitest.config.ts`) |

---

## TypeScript Guidelines

- Prefer `type` over `interface` for type definitions. Only use `interface` when declaration merging is explicitly needed (rare).
- Never use the `any` type. Use `unknown` and narrow with type guards when the type is uncertain.
- Use the `satisfies` operator for better type inference with validation:

```tsx
// Preferred
const config = { ... } satisfies AppConfig;

// Avoid — loses narrowed type info
const config: AppConfig = { ... };
```

---

## State Management

- **Global / async state**: Use Redux Toolkit. API calls must be handled using `createAsyncThunk` within Redux slices inside `src/features/`.
- **UI / local input state**: Use `useState` inside React components. For complex local logic, extract into custom hooks under the feature's `_hooks/` folder.
- **Typed hooks**: Always use typed `useAppDispatch` and `useAppSelector` from `src/common/lib/redux/hooks.ts`.

---

## Component Guidelines

- **Server Components by default.** Only add `'use client'` when you need interactivity, hooks, or browser APIs.
- **No API calls in common components.** Common components must be pure and reusable.
- **No raw text.** All user-facing strings must use i18n keys. Add keys for every supported language before using them.
- **No inline styles.** Use MUI's `sx` prop, styled components, or feature-scoped CSS modules under `_styles/`.
- **No shared styles across features.** Each feature must have its own `_styles/` folder. Never share classnames across different feature pages.
- **Add a `data-testid` attribute** to components for unit test targeting.
- **Document new common components** in your team's documentation platform with usage examples and props description.

---

## Testing

### What to test

| Target | Location |
| --- | --- |
| Pages | `src/app/[locale]/(group)/feature/__tests__/` |
| Feature components | `src/app/[locale]/(group)/feature/_components/__tests__/` |
| Common components | `src/common/components/__tests__/` |
| Common layouts | `src/common/layouts/__tests__/` |
| Redux slices | `src/features/featureName/slices/__tests__/` |

### Testing practices

- Use `data-testid` for querying elements.
- Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state.
- Mock API responses — never make real API calls in unit tests.
- Use `__mocks__/` folders for shared mock data.

---

## Localisation

- Managed via **next-intl** with translations synced from a translation management platform (e.g., Phrase, Crowdin, Lokalise).
- Locale files live in `src/common/locales/` (`en.json`, `zh_HK.json`).
- **Never use raw text in components.** Always reference a translation key.
- Add keys to all language files before using them in code.

### i18n Key Naming Convention

See [i18n Conventions](../standards/i18n-conventions.md) for the full naming convention shared across all templates.

---

## Secrets & Credentials

- Never commit secrets to the repository.
- Use `.env.local` for local development only (gitignored).
- Client-accessible env vars use the `NEXT_PUBLIC_` prefix.

> **Warning:** All `NEXT_PUBLIC_` env vars are bundled into the client JS and visible to anyone inspecting the source. Never put API keys, tokens, or secrets in `NEXT_PUBLIC_` variables.

---

## Best Practices Summary

| Rule | Detail |
| --- | --- |
| Unit tests required | Write tests for components, pages, layouts, and Redux slices |
| No API calls in common components | Common components must be presentation-only |
| API calls via `createAsyncThunk` | All async data fetching lives in Redux slices under `src/features/` |
| Local state via `useState` | UI state stays in components; complex logic goes into custom hooks |
| No shared styles across features | Each feature owns its own `_styles/` folder |
| Modular code | Keep code sectioned, clean, and reusable |
| No `any` type | Use proper types or `unknown` with type guards |
| No raw text | All strings use i18n keys |
| `type` over `interface` | Use `interface` only when declaration merging is needed |
| No inline styles | Use MUI `sx`, styled components, or scoped CSS modules |
| Server Components by default | Only use `'use client'` when interactivity is required |
| Barrel exports sparingly | Prefer direct imports to avoid tree-shaking issues |

---

## Changes from Next.js 14

| Change | Why |
| --- | --- |
| `_` prefixed private folders (`_components/`, `_hooks/`) | Next.js convention to exclude from routing — clearer than relying on team knowledge |
| Route groups `(auth)`, `(dashboard)` | Share layouts without affecting URL structure |
| `actions.ts` for Server Actions | Stable in Next 15 — use for mutations where client-side caching isn't needed |
| `src/features/` for Redux slices | Separates state management from route tree — independently testable |
| `src/providers/` directory | Centralises providers instead of scattering across route segments |
| `next.config.ts` | TypeScript config natively supported in Next 15 |
| `eslint.config.mjs` (flat config) | `.eslintrc.*` format is deprecated |
| Vitest over Jest | Faster, native ESM/TS support, Jest-compatible API |
| Playwright for E2E | Recommended over Cypress for Next.js projects |
