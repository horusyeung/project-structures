# React Native Expo Project Structure

> Production-ready project structure for Expo with Expo Router. Opinionated, scalable, and battle-tested.

## When to Use This

| Use Expo when... | Use React Native CLI when... |
| --- | --- |
| Managed workflow covers your needs | You need full control over native modules |
| You want file-based routing (Expo Router) | The app targets mainland China (no Google Play Services) |
| EAS Build + OTA updates suit your pipeline | You need complex custom native code |
| You want less native code maintenance (CNG) | You maintain complex native dependencies |

> **Note:** This structure shares common conventions with the Next.js and React Native CLI projects where genuinely platform-agnostic (state management, shared code, locales, providers). Expo Router and Next.js App Router share real architectural DNA — file-based routing, route groups, layouts — so alignment between the two is natural, not forced.

### Why Expo?

Expo has evolved from a prototyping tool into the recommended framework for production React Native apps. Key advantages over bare React Native CLI:

- **Expo Router** — file-based routing with deep linking, universal links, and typed routes out of the box
- **EAS (Expo Application Services)** — managed build, submit, and OTA update pipeline
- **Continuous Native Generation (CNG)** — `expo prebuild` generates native projects from config, reducing native code maintenance
- **Expo Modules API** — write native modules in Swift/Kotlin without bridging boilerplate
- **Hermes v1** — available with major performance improvements
- **Full native access** — `expo-dev-client` gives you the same native access as bare RN CLI

> **Note:** Expo SDK 55 (RN 0.83 / React 19.2) drops Legacy Architecture support entirely. All projects run on the New Architecture only.

### When NOT to use Expo

Expo is not recommended when the app targets **mainland China** as a primary market. Chinese Android devices do not ship with Google Play Services, which breaks FCM (push notifications), Google Maps, and Google Play distribution. While workarounds exist (custom dev clients, Chinese push SDKs), they negate Expo's main advantage of simplicity. For China-targeting apps, use **React Native CLI** for full control over native module swaps. See the [React Native CLI Project Structure](../react-native-cli/).

---

## Project Structure

- The `src/app/` folder uses **Expo Router's file-based routing**. Every file in `app/` becomes a route.
- The `src/common/` folder stores shared components, utilities, hooks, and configurations.
- The `src/features/` folder contains feature-scoped Redux slices, separated from the route tree.
- The `src/providers/` folder centralises all context/store providers.
- Files prefixed with `_` (e.g. `_components/`) are private and excluded from routing.
- Route groups wrapped in `()` share layouts without affecting the URL.

```
.
├── config/
│   ├── .env.development
│   ├── .env.staging
│   ├── .env.uat
│   ├── .env.production
│   └── .gitignore
│
├── src/
│   ├── app/                                     # Expo Router — file-based routing
│   │   ├── _layout.tsx                          # root layout
│   │   ├── +not-found.tsx
│   │   │
│   │   ├── (auth)/                              # route group — auth flow
│   │   │   ├── _layout.tsx
│   │   │   ├── _components/
│   │   │   │   └── __tests__/
│   │   │   ├── _hooks/
│   │   │   ├── _styles/
│   │   │   ├── _types/
│   │   │   ├── login.tsx
│   │   │   └── register.tsx
│   │   │
│   │   ├── (tabs)/                              # route group — main tab navigator
│   │   │   ├── _layout.tsx
│   │   │   ├── index.tsx
│   │   │   ├── feature1/
│   │   │   │   ├── _layout.tsx
│   │   │   │   ├── _components/
│   │   │   │   │   └── __tests__/
│   │   │   │   ├── _hooks/
│   │   │   │   ├── _styles/
│   │   │   │   ├── _types/
│   │   │   │   ├── index.tsx                    # feature1 list screen
│   │   │   │   └── [id].tsx                     # feature1 detail (dynamic route)
│   │   │   │
│   │   │   └── feature2/
│   │   │       └── ...
│   │   │
│   │   └── (modals)/                            # route group — modal screens
│   │       ├── _layout.tsx
│   │       └── settings.tsx
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
│   │   ├── assets/
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
│   │   ├── theme/
│   │   ├── types/
│   │   └── utils/
│   │
│   ├── providers/                               # centralised providers
│   │   ├── ReduxProvider.tsx
│   │   ├── ThemeProvider.tsx
│   │   └── I18nProvider.tsx
│   │
│   └── i18n.config.ts
│
├── tests/
│   ├── e2e/                                     # Maestro or Detox
│   └── integration/
│
├── app.json                                     # Expo config (or app.config.ts)
├── babel.config.js
├── eslint.config.mjs                            # ESLint flat config
├── .gitignore
├── .prettierrc.js
├── jest.config.ts
├── metro.config.js
├── package.json
├── README.md
├── tsconfig.json
└── yarn.lock
```

---

## Environments

| Environment | Description |
| --- | --- |
| Development | Local development |
| Staging | Internal QA testing |
| UAT | User acceptance testing |
| Production | Live production |

---

## Tech Stack & Dependencies

### Core

| Category | Tool / Library | Notes |
| --- | --- | --- |
| Framework | [Expo](https://expo.dev/) | SDK 55 (RN 0.83.1 / React 19.2) |
| Routing | [Expo Router](https://docs.expo.dev/router/introduction/) | File-based routing with typed routes |
| Language | [TypeScript](https://www.typescriptlang.org/) | First-class support |
| State Management | [Redux Toolkit](https://redux-toolkit.js.org/) | `createAsyncThunk` for async, `useState` for local UI state |
| UI Library | [React Native Elements](https://reactnativeelements.com/) | Consistent with MUI on web — familiar component API |
| Localisation | i18next + react-i18next + react-native-localize | See Localisation section below |
| API Integration | [Axios](https://axios-http.com/) | REST client with interceptors |
| Build | [EAS Build](https://docs.expo.dev/build/introduction/) | Cloud builds for iOS and Android |
| OTA Updates | [EAS Update](https://docs.expo.dev/eas-update/introduction/) | Over-the-air JS bundle updates |
| Submission | [EAS Submit](https://docs.expo.dev/submit/introduction/) | Automated App Store and Google Play submission |

### UI Library

**React Native Elements** (`@rneui/themed`) maintains consistency with **MUI (Material UI)** used in web projects. Both libraries follow a similar component API and theming pattern, making it easier for developers to work across web and mobile codebases.

### Localisation

| Package | Version | Purpose |
| --- | --- | --- |
| `i18next` | ^23.11+ | Core i18n framework |
| `react-i18next` | ^14.1+ | React bindings for i18next |
| `react-native-localize` | latest | Detects device locale, number/date formats |
| `intl-pluralrules` | ^2.0+ | Polyfill for `Intl.PluralRules` (Hermes support) |

Locale JSON files live in `src/common/locales/` (`en.json`, `zh_HK.json`), same location across all projects.

### Testing

| Tool | Purpose |
| --- | --- |
| [Jest](https://jestjs.io/) | Unit testing framework |
| [@testing-library/react-native](https://callstack.github.io/react-native-testing-library/) | Component testing |
| [Maestro](https://maestro.mobile.dev/) (recommended) or [Detox](https://wix.github.io/Detox/) | E2E testing |

> **Note:** `@testing-library/jest-native` has been merged into `@testing-library/react-native` v12.4+. Use the unified package.

> **Note:** Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state. `redux-mock-store` is unnecessary.

### Storage

| Tool | Purpose |
| --- | --- |
| [MMKV](https://github.com/mrousavy/react-native-mmkv) | Fast key-value storage (replaces AsyncStorage and Realm) |

> **Note:** MMKV is synchronous and ~30x faster than AsyncStorage. Use it for all key-value persistence needs.

### Config & Secrets

| Tool | Purpose |
| --- | --- |
| Built-in Expo `.env` support | Environment variables (`EXPO_PUBLIC_*`) |
| [expo-secure-store](https://docs.expo.dev/versions/latest/sdk/securestore/) | Secure runtime credential storage |

> **Note:** Unlike bare RN CLI, Expo has built-in `.env` file support without needing `react-native-config`.

---

## New Architecture

Expo SDK 55 uses React Native 0.83, where the **New Architecture is always enabled and cannot be disabled**.

Key components:

- **Fabric** — new rendering system with synchronous layout
- **TurboModules** — lazy-loaded native modules via JSI
- **JSI (JavaScript Interface)** — direct JS <> native communication
- **Hermes v1** — default JS engine with significant performance improvements
- **Expo Modules API** — write native modules in Swift/Kotlin with automatic Codegen

Run `npx expo-doctor` to validate all dependencies are compatible with the New Architecture.

---

## TypeScript Guidelines

- Prefer `type` over `interface`. Only use `interface` when declaration merging is explicitly needed.
- Never use the `any` type. Use `unknown` and narrow with type guards.
- Use `satisfies` for better type inference with validation.
- Expo Router supports `tsconfig.json` path aliases (e.g. `@/common/*`) natively via Metro.

---

## State Management

- **Global / async state**: Use Redux Toolkit. API calls via `createAsyncThunk` in `src/features/` slices.
- **UI / local input state**: Use `useState`. Complex local logic goes into custom hooks in the feature `_hooks/` folder.
- **Typed hooks**: Use typed `useAppDispatch` and `useAppSelector` from `src/common/lib/redux/hooks.ts`.

---

## Component Guidelines

- **No API calls in common components.** Common components must be pure and reusable.
- **No raw text.** All user-facing strings must use i18n keys.
- **No inline styles.** Use StyleSheet or themed styles via React Native Elements' `ThemeProvider`.
- **No shared styles across features.** Each feature owns its own `_styles/` folder.
- **Add a `testID` prop** to components for testing.
- **Document new common components** in your team's documentation platform with usage examples and props description.

---

## Testing

### What to test

| Target | Location |
| --- | --- |
| Screens | `src/app/(group)/feature/__tests__/` |
| Feature components | `src/app/(group)/feature/_components/__tests__/` |
| Common components | `src/common/components/__tests__/` |
| Common layouts | `src/common/layouts/__tests__/` |
| Redux slices | `src/features/featureName/slices/__tests__/` |

### Testing practices

- Use `testID` for querying elements.
- Mock Redux store using `configureStore` with preloaded state.
- Mock API responses — never make real API calls in unit tests.
- Use `__mocks__/` folders for shared mock data.

---

## Localisation

- Managed via **i18next** + **react-i18next** + **react-native-localize**.
- `intl-pluralrules` polyfill is required for Hermes pluralisation support.
- Locale files live in `src/common/locales/` (`en.json`, `zh_HK.json`) — same location as all other projects.
- **Never use raw text in components.** Always reference a translation key.
- Add keys to all language files before using them in code.

### i18n Key Naming Convention

See [i18n Conventions](../standards/i18n-conventions.md) for the full naming convention shared across all templates.

---

## Secrets & Credentials

- Client-accessible env vars use `EXPO_PUBLIC_` prefix in `.env.*` files.
- Runtime secrets stored securely using `expo-secure-store`.
- Never commit secrets to the repository.

---

## Best Practices Summary

| Rule | Detail |
| --- | --- |
| Unit tests required | Write tests for components, screens, and Redux slices |
| No API calls in common components | Common components must be presentation-only |
| API calls via `createAsyncThunk` | All async data fetching lives in Redux slices under `src/features/` |
| Local state via `useState` | UI state stays in components; complex logic goes into custom hooks |
| No shared styles across features | Each feature owns its own `_styles/` folder |
| Modular code | Keep code sectioned, clean, and reusable |
| No `any` type | Use proper types or `unknown` with type guards |
| No raw text | All strings use i18n keys with kebab-case naming convention |
| `type` over `interface` | Use `interface` only when declaration merging is needed |
| No inline styles | Use StyleSheet or themed styles |
| New Architecture only | Mandatory from SDK 55 / RN 0.82+ |
| Run `npx expo-doctor` | Validate dependency compatibility regularly |

---

## Key Differences: Expo vs. React Native CLI

| Aspect | Expo | RN CLI |
| --- | --- | --- |
| Routing | File-based (Expo Router) | Manual (React Navigation) |
| Native code management | CNG via `expo prebuild` | Manual `android/` and `ios/` maintenance |
| Build pipeline | EAS Build (cloud) | Local Xcode / Gradle builds |
| OTA updates | EAS Update (built-in) | CodePush or custom solution |
| Env variables | Built-in `.env` support | Requires `react-native-config` |
| Secure storage | `expo-secure-store` | `react-native-keychain` |
| Dev client | `expo-dev-client` (full native access) | Standard Metro dev server |
| Native modules | Expo Modules API (Swift/Kotlin) | TurboModules / legacy bridge |
| E2E testing | Maestro (recommended) | Detox or Maestro |
| China compatibility | Limited (Google Play Services dependency) | Full control over native module swaps |
