# Monorepo (Turborepo) Project Structure

> Production-ready monorepo structure for multi-app projects. Opinionated, scalable, and battle-tested.

## When to Use This

Use a monorepo when you have **multiple applications** that share code, components, or configurations. This structure is ideal for:

- Multiple web apps sharing a UI library and theme
- Apps with shared business logic or API clients
- Teams that need consistent tooling across projects
- Projects where code promotion (app-specific → shared) happens naturally

---

## Top-Level Structure

```
project-root/
├── .husky/                          # Git hooks (pre-commit lint-staged)
├── apps/
│   ├── app1/                        # First application
│   ├── app2/                        # Second application
│   └── app3/                        # Third application
├── packages/
│   ├── common/                      # Shared components, hooks, utils, theme
│   └── config/
│       ├── eslint-config/           # @repo/eslint-config
│       ├── jest-config/             # @repo/jest-config
│       ├── prettier-config/         # @repo/prettier-config
│       └── ts-config/               # @repo/ts-config
├── public/
│   └── assets/
├── eslint.config.mjs                # Root ESLint (imports @repo/eslint-config)
├── next.config.ts                   # Root Next.js config (static export)
├── package.json                     # Root workspace config
├── tsconfig.json                    # Root TS config (extends @repo/ts-config)
├── turbo.json                       # Turborepo pipeline
├── .prettierrc.json
├── .yarnrc.yml                      # Yarn 4 config (nodeLinker: node-modules)
└── yarn.lock
```

---

## Workspace Configuration

Workspaces are defined in the root `package.json`:

```json
{
  "workspaces": [
    "apps/*",
    "packages/config/*",
    "packages/common"
  ]
}
```

Each workspace is an independently versioned package with its own `package.json`.

---

## Application Layer (`apps/`)

Each app follows the same Next.js 15 App Router structure described in the [Next.js 15 template](../nextjs-15/), with one key difference: shared code is imported from `packages/common/` instead of being duplicated.

| App | Package Name | Purpose |
| --- | --- | --- |
| app1 | `@repo/app1` | First application |
| app2 | `@repo/app2` | Second application |
| app3 | `@repo/app3` | Third application |

### Per-App Structure

```
apps/app1/
├── src/
│   ├── app/
│   │   └── [locale]/
│   │       ├── (auth)/                          # Auth route group
│   │       │   └── feature/
│   │       │       ├── __tests__/
│   │       │       ├── _components/
│   │       │       │   └── __tests__/
│   │       │       ├── _hooks/
│   │       │       ├── _lib/
│   │       │       ├── _styles/
│   │       │       ├── _types/
│   │       │       ├── page.tsx
│   │       │       └── layout.tsx
│   │       │
│   │       ├── (dashboard)/                     # Dashboard route group
│   │       │   └── feature/
│   │       │       └── ...                      # same structure as above
│   │       │
│   │       ├── layout.tsx                       # root locale layout
│   │       └── page.tsx
│   │
│   ├── common/                                  # App-specific shared code
│   │   ├── api/RESTful/                         # Axios instances & interceptors
│   │   ├── components/                          # App-specific shared components
│   │   ├── hooks/                               # App-specific hooks
│   │   ├── layouts/                             # App layout & menu data
│   │   ├── libs/
│   │   │   ├── redux/                           # Store config, typed hooks
│   │   │   └── i18n/                            # i18n setup
│   │   ├── theme/                               # MUI theme overrides
│   │   └── utils/
│   │
│   ├── features/                                # Redux slices (feature-scoped)
│   │   └── featureName/
│   │       ├── slices/
│   │       │   ├── __tests__/
│   │       │   └── featureNameSlice.ts
│   │       ├── selectors.ts
│   │       └── types/
│   │
│   ├── providers/                               # Centralised providers
│   └── middleware.ts
│
├── jest.config.mjs                              # Extends @repo/jest-config
├── next.config.ts
├── package.json
└── tsconfig.json                                # Extends @repo/ts-config
```

### App-Specific vs. Shared Code

| Code Type | Location | Example |
| --- | --- | --- |
| Cross-app shared | `packages/common/src/` | Theme, base components, hooks, locales |
| App-specific shared | `apps/{app}/src/common/` | API clients, layouts, menu data |
| Feature-scoped | `apps/{app}/src/app/[locale]/(group)/feature/` | Page components, local hooks, styles |
| Redux slices | `apps/{app}/src/features/` | `createAsyncThunk` + reducers |

**Decision rule:** Code stays in `apps/{app}/src/common/` until it's needed by **2+ apps**. Only then promote it to `packages/common/`.

---

## Shared Packages Layer (`packages/`)

### `packages/common/` — Shared Code Library

The core shared package consumed by all apps via the `@packages/common/*` path alias.

```
packages/common/
├── src/
│   ├── api/                         # Base Axios config
│   ├── assets/
│   │   ├── icons/                   # Shared SVG/PNG icons
│   │   └── locales/                 # Shared i18n keys
│   ├── components/                  # Shared MUI components
│   │   ├── SearchBar/
│   │   ├── Dropdown/
│   │   ├── DatePicker/
│   │   ├── Table/
│   │   ├── Modal/
│   │   ├── ConfirmationModal/
│   │   ├── StatsCard/
│   │   ├── PageHeader/
│   │   ├── Button/
│   │   ├── Tab/
│   │   └── Fields/
│   ├── constants/
│   ├── errors/
│   ├── hooks/
│   │   ├── usePermissions.tsx
│   │   ├── useLocalStorage.ts
│   │   ├── useSessionStorage.ts
│   │   ├── useDebounce.ts
│   │   ├── useComponentPermission.ts
│   │   └── useOutsideClickHandler.ts
│   ├── locales/
│   │   ├── en.json
│   │   └── zh_HK.json
│   ├── styles/                      # Global SCSS
│   ├── theme/                       # Base MUI theme
│   └── utils/                       # Shared utilities
├── package.json
└── tsconfig.json
```

**Rules for `packages/common/`:**

- Must **not** import from any app (`apps/*`). Dependencies flow one way: **apps → common**.
- Must **not** contain app-specific business logic.
- Components must be **pure and reusable** — no API calls, no Redux store access.
- All new components must be **documented** in your team's documentation platform with usage examples.
- Build with `tsc --build` before apps can consume changes.

### `packages/config/` — Shared Tooling Configs

Centralised configurations consumed by all workspaces.

| Package | Name | Exports |
| --- | --- | --- |
| eslint-config | `@repo/eslint-config` | ESLint flat config with Next.js + Prettier + React rules |
| jest-config | `@repo/jest-config` | `createAppJestConfig(appName, customConfig)` factory |
| prettier-config | `@repo/prettier-config` | Prettier config object |
| ts-config | `@repo/ts-config` | Base `tsconfig.json` with path aliases |

**Usage in apps:**

```jsx
// apps/app1/jest.config.mjs
import { createAppJestConfig } from '@repo/jest-config';
export default createAppJestConfig('app1');
```

```json
// apps/app1/tsconfig.json
{ "extends": "@repo/ts-config/tsconfig.json" }
```

---

## TypeScript Path Aliases

Defined in `@repo/ts-config` and available in all workspaces:

| Alias | Resolves To | Usage |
| --- | --- | --- |
| `@src/*` | `src/*` | App-internal imports |
| `@common/*` | `src/common/*` | App-specific shared code |
| `@packages/common/*` | `../../packages/common/src/*` | Cross-app shared code |

```tsx
// Import from shared package
import { usePermissions } from '@packages/common/hooks/usePermissions';

// Import from app-specific common
import { apiClient } from '@common/api/RESTful/client';

// Import within the app
import { UserCard } from '@src/app/[locale]/(auth)/users/_components/UserCard';
```

---

## Turborepo Pipeline

Build orchestration is configured in `turbo.json`:

```json
{
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "out/**"]
    },
    "lint": {
      "outputs": []
    },
    "test": {
      "dependsOn": ["^build"],
      "outputs": []
    },
    "dev": {
      "cache": false
    }
  }
}
```

Key behaviours:

- `build` runs dependency builds first (`^build`) — `packages/common` builds before apps.
- Build outputs (`.next/`, `out/`) are cached by Turbo for faster rebuilds.
- `dev` is never cached (live development).
- `test` depends on `^build` to ensure shared packages are compiled.

### Root Scripts

```json
{
  "dev:app1": "turbo dev --filter=@repo/app1",
  "dev:app2": "turbo dev --filter=@repo/app2",
  "dev:app3": "turbo dev --filter=@repo/app3",
  "build": "turbo build",
  "lint": "turbo lint",
  "test": "turbo test",
  "format": "prettier --config ./packages/config/prettier-config/prettier.config.mjs --write ."
}
```

- Run a single app: `yarn dev:app1`
- Build all apps: `yarn build`
- Run all tests: `yarn test`

---

## Environments

| Environment | Description |
| --- | --- |
| Development | Local development |
| Staging | User acceptance testing |
| Production | Live production |

---

## Tech Stack (Monorepo-Specific)

| Category | Tool / Library |
| --- | --- |
| Package Manager | Yarn 4 (Berry) with Workspaces |
| Build Orchestration | Turborepo 2.x |
| Node.js | 22 |
| Shared UI | `packages/common` (MUI components, hooks, utils) |
| Shared Config | `packages/config` (ESLint, Jest, Prettier, TypeScript) |
| Feature Flags | Feature flag service (e.g., GrowthBook, LaunchDarkly, Flagsmith) |
| RBAC | `usePermissions` / `useComponentPermission` hooks |
| Form Validation | Formik + Yup (via common) |

For per-app tech stack (MUI, Redux Toolkit, next-intl, Vitest, Playwright, etc.), see the [Next.js 15 template](../nextjs-15/).

---

## TypeScript Guidelines

- Prefer `type` over `interface` for type definitions. Only use `interface` when declaration merging is explicitly needed (rare).
- Never use the `any` type. Use `unknown` and narrow with type guards when the type is uncertain.
- Use the `satisfies` operator for better type inference with validation.

---

## State Management

- **Global / async state**: Use Redux Toolkit. API calls must be handled using `createAsyncThunk` within Redux slices inside `apps/{app}/src/features/`.
- **UI / local input state**: Use `useState` inside React components. For complex local logic, extract into custom hooks under the feature's `_hooks/` folder.
- **Typed hooks**: Always use typed `useAppDispatch` and `useAppSelector` from `apps/{app}/src/common/libs/redux/`.

> **Note:** Each app has its **own Redux store**. There is no shared store across apps. Shared hooks like `usePermissions` live in `packages/common/` but do not access Redux directly.

---

## Component Guidelines

- **Server Components by default.** Only add `'use client'` when you need interactivity, hooks, or browser APIs.
- **No API calls in common components.** Both `packages/common/` and `apps/{app}/src/common/` components must be pure and reusable.
- **No raw text.** All user-facing strings must use i18n keys. Add keys for every supported language before using them.
- **No inline styles.** Use MUI's `sx` prop, styled components, or feature-scoped CSS modules under `_styles/`.
- **No shared styles across features.** Each feature must have its own `_styles/` folder.
- **Add a `data-testid` attribute** to components for unit test targeting.
- **Document new common components** in your team's documentation platform with usage examples and props description.

---

## Testing

### What to test and where

| Target | Location |
| --- | --- |
| Pages | `apps/{app}/src/app/[locale]/(group)/feature/__tests__/` |
| Feature components | `apps/{app}/src/app/[locale]/(group)/feature/_components/__tests__/` |
| App-specific common | `apps/{app}/src/common/components/__tests__/` |
| Redux slices | `apps/{app}/src/features/featureName/slices/__tests__/` |
| Shared components (common) | `packages/common/src/components/__tests__/` |
| Shared hooks (common) | `packages/common/src/hooks/__tests__/` |

### Testing practices

- Use `data-testid` for querying elements.
- Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state.
- Mock API responses — never make real API calls in unit tests.
- Use `__mocks__/` folders for shared mock data.
- Shared hooks and components are tested in `packages/common/`. Feature-specific logic is tested in the app's `__tests__/` folders.

---

## Localisation

- Managed via **next-intl** with translations synced from a translation management platform (e.g., Phrase, Crowdin, Lokalise).
- **Shared locale files** live in `packages/common/src/locales/` (`en.json`, `zh_HK.json`) for cross-app keys.
- **App-specific locale files** can live in `apps/{app}/src/common/libs/i18n/` if needed.
- **Never use raw text in components.** Always reference a translation key.
- Add keys to all language files before using them in code.

### i18n Key Naming Convention

See [i18n Conventions](../standards/i18n-conventions.md) for the full naming convention shared across all templates.

---

## Secrets & Credentials

- Never commit secrets to the repository.
- Use `.env.local` for local development only (gitignored).

---

## Monorepo Best Practices

| Rule | Detail |
| --- | --- |
| One-way dependency flow | Apps import from packages. Packages **never** import from apps. |
| No cross-app imports | `apps/app1` must never import from `apps/app2` or `apps/app3`. Share via `packages/common`. |
| Build common first | Always run `packages/common` build before app builds. Turbo handles this via `^build`. |
| Scope new dependencies | Install in the correct workspace, not the root. Use `yarn workspace @repo/app1 add <pkg>`. |
| Root deps are shared tooling only | Only Turbo, TypeScript, Prettier, ESLint, and Husky belong at root. |
| Extend shared configs | Apps must extend `@repo/ts-config`, `@repo/eslint-config`, and `@repo/jest-config` — never define their own from scratch. |
| Feature flags for cross-app rollouts | Use a feature flag service to toggle features across apps without redeployment. |
| Keep common package lean | Only promote code to `packages/common` when it's used by **2+ apps**. Single-app code stays in `apps/{app}/src/common/`. |
| Test at the right level | Shared hooks/components → test in `packages/common`. Feature-specific logic → test in the app's `__tests__/` folders. |
| No circular dependencies | Use `turbo build` to catch circular dependency issues early. |
| No `any` type | Use proper types or `unknown` with type guards. |
| No raw text | All strings use i18n keys with kebab-case naming convention. |
| `type` over `interface` | Use `interface` only when declaration merging is needed. |

---

## Adding a New App

1. Create `apps/{new-app}/` with `package.json` (name: `@repo/{new-app}`).
2. Extend shared configs (`tsconfig.json`, `jest.config.mjs`, `eslint.config.mjs`).
3. Add `@packages/common` as a dependency.
4. Add `dev`/`build`/`serve` scripts to root `package.json`.
5. Follow the same App Router structure: `src/app/[locale]/(group)/feature/`.

## Adding a New Shared Package

1. Create `packages/{name}/` with `package.json` (name: `@repo/{name}`).
2. Add to root `workspaces` array in `package.json`.
3. Export via explicit entry points — avoid barrel exports for tree-shaking.
4. Add build step and register in `turbo.json` if it produces build artifacts.
5. Document the package API in your team's documentation platform.
