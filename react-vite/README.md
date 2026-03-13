# React.js (Vite) Project Structure

> Production-ready project structure for React.js with Vite. Opinionated, scalable, and battle-tested.

## When to Use This

| Use React.js (Vite) when... | Use Next.js when... |
| --- | --- |
| Building a **SPA** (single-page application) | You need **SSR / SSG / ISR** |
| Internal tools, dashboards, admin panels | SEO matters (public-facing marketing, content pages) |
| The app lives behind authentication | You need Server Components or Server Actions |
| You need a lightweight, fast build with no server | You need middleware, edge functions, or API routes |
| Embedding React inside a larger non-React system | You need file-based routing with layouts |

---

## Project Structure

- The `src/pages/` folder contains page-level components organised by feature, each with co-located components, hooks, styles, and types.
- The `src/router/` folder defines all route configuration using React Router.
- The `src/common/` folder stores shared components, utilities, hooks, and configurations.
- The `src/features/` folder contains feature-scoped Redux slices, separated from the page tree.
- The `src/providers/` folder centralises all context/store providers.

```
.
├── .husky/
├── public/
│   └── assets/
│
├── src/
│   ├── pages/                                   # page components organised by feature
│   │   ├── auth/
│   │   │   ├── __tests__/
│   │   │   ├── components/                      # page-specific components
│   │   │   │   └── __tests__/
│   │   │   ├── hooks/
│   │   │   ├── styles/
│   │   │   ├── types/
│   │   │   ├── LoginPage.tsx
│   │   │   └── RegisterPage.tsx
│   │   │
│   │   ├── feature1/
│   │   │   ├── __tests__/
│   │   │   ├── components/
│   │   │   │   └── __tests__/
│   │   │   ├── hooks/
│   │   │   ├── styles/
│   │   │   ├── types/
│   │   │   ├── Feature1ListPage.tsx
│   │   │   └── Feature1DetailPage.tsx
│   │   │
│   │   └── feature2/
│   │       └── ...
│   │
│   ├── router/                                  # React Router config
│   │   ├── __tests__/
│   │   ├── index.tsx                            # createBrowserRouter / RouterProvider
│   │   ├── routes.tsx                           # route definitions
│   │   ├── guards/
│   │   │   ├── AuthGuard.tsx                    # protected route wrapper
│   │   │   └── GuestGuard.tsx                   # redirect if authenticated
│   │   └── layouts/
│   │       ├── AuthLayout.tsx                   # layout for auth pages
│   │       ├── DashboardLayout.tsx              # layout for main app
│   │       └── MinimalLayout.tsx                # layout for error / standalone pages
│   │
│   ├── features/                                # feature-scoped Redux slices
│   │   ├── feature1/
│   │   │   ├── slices/
│   │   │   │   ├── __tests__/
│   │   │   │   └── feature1Slice.ts             # createAsyncThunk + reducers
│   │   │   ├── selectors.ts
│   │   │   └── types/
│   │   └── feature2/
│   │       └── ...
│   │
│   ├── common/                                  # shared code
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
│   │   ├── lib/
│   │   │   └── redux/
│   │   │       ├── store.ts
│   │   │       └── hooks.ts                     # typed useAppDispatch / useAppSelector
│   │   ├── locales/
│   │   │   ├── en.json
│   │   │   └── zh_HK.json
│   │   ├── styles/
│   │   │   └── css/
│   │   ├── theme/
│   │   ├── types/
│   │   └── utils/
│   │
│   ├── providers/                               # centralised providers
│   │   ├── ReduxProvider.tsx
│   │   ├── ThemeProvider.tsx
│   │   └── I18nProvider.tsx
│   │
│   ├── i18n.config.ts
│   ├── App.tsx                                  # root component (wraps providers + router)
│   ├── main.tsx                                 # Vite entry point
│   └── vite-env.d.ts
│
├── tests/
│   ├── e2e/                                     # Playwright
│   ├── smoke/
│   └── regression/
│
├── .env.development
├── .env.staging
├── .env.uat
├── .env.production
├── eslint.config.mjs                            # ESLint flat config
├── .gitignore
├── .prettierignore
├── .prettierrc.json
├── index.html                                   # Vite HTML entry
├── package.json
├── README.md
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── vitest.config.ts
└── yarn.lock
```

---

## Environments

| Environment | Description |
| --- | --- |
| Development | Local development |
| Staging | Internal QA testing |
| Production | Live production |

---

## Tech Stack & Dependencies

### Core

| Category | Tool / Library | Notes |
| --- | --- | --- |
| Build Tool | [Vite](https://vite.dev/) | Fast HMR, native ESM, optimised production builds |
| React Version | **React 19** | Latest stable |
| Language | [TypeScript](https://www.typescriptlang.org/) | First-class Vite support |
| Routing | [React Router 7](https://reactrouter.com/) | Data router with `createBrowserRouter` |
| State Management | [Redux Toolkit](https://redux-toolkit.js.org/) | `createAsyncThunk` for async, `useState` for local UI |
| UI Library | [MUI (Material UI)](https://mui.com/material-ui/) | Consistent UI library across web projects |
| Localisation | react-i18next + i18next | See Localisation section below |
| API Integration | [Axios](https://axios-http.com/) | REST client with interceptors |

### Why Vite (not CRA)

Create React App (CRA) is **deprecated and unmaintained** as of 2023. The React team officially recommends using a framework (Next.js, Remix) or Vite for standalone SPAs. Vite provides faster dev server startup, instant HMR, native TypeScript support, and optimised production builds via Rollup.

### Localisation

| Package | Version | Purpose |
| --- | --- | --- |
| `i18next` | ^23.11+ | Core i18n framework |
| `react-i18next` | ^14.1+ | React bindings for i18next |
| `i18next-browser-languagedetector` | latest | Detects browser locale |
| `i18next-http-backend` | latest | Loads locale JSON files lazily (optional) |

Locale JSON files live in `src/common/locales/` (`en.json`, `zh_HK.json`), same location across all projects.

### Testing

| Tool | Purpose |
| --- | --- |
| [Vitest](https://vitest.dev/) | Unit testing (Jest-compatible, native Vite integration) |
| [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) | Component testing |
| [Playwright](https://playwright.dev/) | E2E testing |

> **Note:** Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state. `redux-mock-store` is unnecessary.

### Linting & Formatting

| Tool | Purpose |
| --- | --- |
| [ESLint](https://eslint.org/) (flat config) | Linting with `eslint.config.mjs` |
| [Prettier](https://prettier.io/) | Code formatting |
| [Husky](https://typicode.github.io/husky/) | Git hooks (pre-commit lint/format) |

---

## Routing

React Router 7 with the **data router** pattern (`createBrowserRouter`) is the recommended approach. Routes are defined declaratively in `src/router/routes.tsx`.

### Route structure

```tsx
// src/router/routes.tsx
import { lazy } from 'react';
import { AuthGuard } from './guards/AuthGuard';
import { GuestGuard } from './guards/GuestGuard';
import { DashboardLayout } from './layouts/DashboardLayout';
import { AuthLayout } from './layouts/AuthLayout';

const LoginPage = lazy(() => import('@/pages/auth/LoginPage'));
const Feature1ListPage = lazy(() => import('@/pages/feature1/Feature1ListPage'));
const Feature1DetailPage = lazy(() => import('@/pages/feature1/Feature1DetailPage'));

export const routes = [
  {
    element: <GuestGuard><AuthLayout /></GuestGuard>,
    children: [
      { path: '/login', element: <LoginPage /> },
      { path: '/register', element: <RegisterPage /> },
    ],
  },
  {
    element: <AuthGuard><DashboardLayout /></AuthGuard>,
    children: [
      { path: '/', element: <Feature1ListPage /> },
      { path: '/feature1/:id', element: <Feature1DetailPage /> },
    ],
  },
];
```

### Route guards

- **`AuthGuard`** — redirects unauthenticated users to `/login`
- **`GuestGuard`** — redirects authenticated users away from auth pages
- Guards are wrapper components, not middleware — React Router pattern

### Layouts

Layouts live in `src/router/layouts/` and use React Router's `<Outlet />` for nested rendering. This replaces Next.js's `layout.tsx` convention.

---

## TypeScript Guidelines

- Prefer `type` over `interface` for type definitions. Only use `interface` when declaration merging is explicitly needed (rare).
- Never use the `any` type. Use `unknown` and narrow with type guards when the type is uncertain.
- Use the `satisfies` operator for better type inference with validation.
- Use Vite's path aliases via `tsconfig.json` (e.g. `@/common/*`, `@/features/*`).

---

## State Management

- **Global / async state**: Use Redux Toolkit. API calls must be handled using `createAsyncThunk` within Redux slices inside `src/features/`.
- **UI / local input state**: Use `useState` inside React components. For complex local logic, extract into custom hooks under the feature's `hooks/` folder.
- **Typed hooks**: Always use typed `useAppDispatch` and `useAppSelector` from `src/common/lib/redux/hooks.ts`.

---

## Component Guidelines

- **No API calls in common components.** Common components must be pure and reusable.
- **No raw text.** All user-facing strings must use i18n keys. Add keys for every supported language before using them.
- **No inline styles.** Use MUI's `sx` prop, styled components, or feature-scoped CSS modules under `styles/`.
- **No shared styles across features.** Each feature must have its own `styles/` folder. Never share classnames across different feature pages.
- **Add a `data-testid` attribute** to components for unit test targeting.
- **Document new common components** in your team's documentation platform with usage examples and props description.

---

## Testing

### What to test

| Target | Location |
| --- | --- |
| Pages | `src/pages/featureName/__tests__/` |
| Feature components | `src/pages/featureName/components/__tests__/` |
| Common components | `src/common/components/__tests__/` |
| Common layouts | `src/common/layouts/__tests__/` |
| Redux slices | `src/features/featureName/slices/__tests__/` |
| Router | `src/router/__tests__/` |

### Testing practices

- Use `data-testid` for querying elements.
- Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state.
- Mock API responses — never make real API calls in unit tests.
- Use `__mocks__/` folders for shared mock data.

---

## Localisation

- Managed via **i18next** + **react-i18next** + **i18next-browser-languagedetector**.
- Locale files live in `src/common/locales/` (`en.json`, `zh_HK.json`) — same location across all projects.
- **Never use raw text in components.** Always reference a translation key.
- Add keys to all language files before using them in code.

### i18n Key Naming Convention

See [i18n Conventions](../standards/i18n-conventions.md) for the full naming convention shared across all templates.

---

## Secrets & Credentials

- Client-accessible env vars use `VITE_` prefix (e.g. `VITE_API_URL`). Vite exposes only `VITE_`-prefixed variables to the client bundle.
- Never commit secrets to the repository.
- Use `.env.development` for local development (gitignored).

> **Warning:** Unlike server-rendered frameworks, all `VITE_` env vars are bundled into the client JS and visible to anyone inspecting the source. Never put API keys, tokens, or secrets in `VITE_` variables.

---

## Best Practices Summary

| Rule | Detail |
| --- | --- |
| Unit tests required | Write tests for components, pages, and Redux slices |
| No API calls in common components | Common components must be presentation-only |
| API calls via `createAsyncThunk` | All async data fetching lives in Redux slices under `src/features/` |
| Local state via `useState` | UI state stays in components; complex logic goes into custom hooks |
| No shared styles across features | Each feature owns its own `styles/` folder |
| Modular code | Keep code sectioned, clean, and reusable |
| No `any` type | Use proper types or `unknown` with type guards |
| No raw text | All strings use i18n keys with kebab-case naming convention |
| `type` over `interface` | Use `interface` only when declaration merging is needed |
| No inline styles | Use MUI `sx`, styled components, or scoped CSS modules |
| Lazy load pages | Use `React.lazy()` for all page components in route definitions |
| Barrel exports sparingly | Prefer direct imports to avoid tree-shaking issues |

---

## Key Differences: React.js (Vite) vs. Next.js

| Aspect | React.js (Vite) | Next.js 15 |
| --- | --- | --- |
| Rendering | Client-side only (SPA) | SSR / SSG / ISR / Client |
| Routing | React Router (manual config) | File-based (App Router) |
| Page folder | `src/pages/` | `src/app/` |
| Route config | `src/router/routes.tsx` | File system conventions |
| Layouts | `src/router/layouts/` + `<Outlet />` | `layout.tsx` per route segment |
| Route guards | `AuthGuard` / `GuestGuard` wrapper components | `middleware.ts` |
| Server Components | Not available | Default for all components |
| Server Actions | Not available | `actions.ts` per feature |
| Build tool | Vite | Next.js (Turbopack / Webpack) |
| Env variables | `VITE_` prefix | `NEXT_PUBLIC_` prefix |
| i18n | react-i18next | next-intl |
| Unit testing | Vitest | Vitest |
| E2E testing | Playwright | Playwright |
| Private folders | Not needed (no file-based routing) | `_` prefix excludes from routing |
| Route groups | Not applicable | `()` syntax for shared layouts |
