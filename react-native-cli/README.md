# React Native CLI Project Structure

> Production-ready project structure for React Native CLI. Opinionated, scalable, and battle-tested.

## When to Use This

| Use React Native CLI when... | Use Expo when... |
| --- | --- |
| You need full control over native modules | Managed workflow is sufficient |
| The app targets mainland China (no Google Play Services) | The app doesn't need custom native module swaps |
| You need custom native code (Swift/Kotlin bridges) | EAS Build + OTA updates cover your needs |
| You maintain complex native dependencies | You want file-based routing (Expo Router) |

> **Note:** This structure shares common conventions with the Next.js and Expo projects where genuinely platform-agnostic (state management, shared code, locales, providers), but follows React Native CLI's own idioms for navigation and screen organisation.

---

## Project Structure

- The `src/screens/` folder contains screen components organised by feature, each with co-located components, hooks, styles, and types.
- The `src/navigation/` folder defines all navigator configuration (stacks, tabs, drawers) using React Navigation.
- The `src/common/` folder stores shared components, utilities, hooks, and configurations.
- The `src/features/` folder contains feature-scoped Redux slices, separated from the screen tree.
- The `src/providers/` folder centralises all context/store providers.

```
.
├── .bundle/
├── android/
├── ios/
├── config/
│   ├── .env.development
│   ├── .env.staging
│   ├── .env.production
│   └── .gitignore
│
├── src/
│   ├── screens/                                 # screens organised by feature
│   │   ├── auth/
│   │   │   ├── __tests__/
│   │   │   ├── components/                      # screen-specific components
│   │   │   │   └── __tests__/
│   │   │   ├── hooks/
│   │   │   ├── styles/
│   │   │   ├── types/
│   │   │   ├── LoginScreen.tsx
│   │   │   └── RegisterScreen.tsx
│   │   │
│   │   ├── feature1/
│   │   │   ├── __tests__/
│   │   │   ├── components/
│   │   │   │   └── __tests__/
│   │   │   ├── hooks/
│   │   │   ├── styles/
│   │   │   ├── types/
│   │   │   ├── Feature1ListScreen.tsx
│   │   │   └── Feature1DetailScreen.tsx
│   │   │
│   │   └── feature2/
│   │       └── ...
│   │
│   ├── navigation/                              # React Navigation config
│   │   ├── __tests__/
│   │   ├── RootNavigator.tsx                    # entry navigator
│   │   ├── AuthNavigator.tsx                    # auth stack
│   │   ├── MainTabNavigator.tsx                 # bottom tabs
│   │   ├── Feature1StackNavigator.tsx           # feature1 stack
│   │   ├── linking.ts                           # deep link config
│   │   └── types.ts                             # navigation param types
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
│   ├── e2e/                                     # Detox or Maestro
│   └── integration/
│
├── .eslintrc.js                                 # or eslint.config.mjs (flat config)
├── .gitignore
├── .prettierrc.js
├── .watchmanconfig
├── app.json
├── App.tsx
├── babel.config.js
├── Gemfile
├── Gemfile.lock
├── index.js
├── jest.config.ts
├── metro.config.js
├── package.json
├── README.md
├── tsconfig.json
└── yarn.lock
```

---

## Tech Stack & Dependencies

### Core

| Category | Tool / Library | Notes |
| --- | --- | --- |
| Framework | [React Native CLI](https://reactnative.dev/) | New Architecture mandatory from RN 0.82+ |
| React Native Version | **0.83+** (latest stable) | New Architecture only — legacy arch is frozen |
| React Version | **React 19** | Shipped with RN 0.78+ |
| Language | [TypeScript](https://www.typescriptlang.org/) | First-class support with improved type definitions |
| Navigation | [React Navigation 7](https://reactnavigation.org/) | Static API for type-safe navigation |
| State Management | [Redux Toolkit](https://redux-toolkit.js.org/) | `createAsyncThunk` for async, `useState` for local UI state |
| UI Library | [React Native Elements](https://reactnativeelements.com/) | Consistent with MUI on web — familiar component API |
| Localisation | i18next + react-i18next + react-native-localize | See Localisation section below |
| API Integration | [Axios](https://axios-http.com/) | REST client with interceptors |

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
| [Detox](https://wix.github.io/Detox/) or [Maestro](https://maestro.mobile.dev/) | E2E testing |

> **Note:** `@testing-library/jest-native` has been merged into `@testing-library/react-native` v12.4+. Use the unified package.

> **Note:** Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state. `redux-mock-store` is unnecessary.

### Storage

| Tool | Purpose |
| --- | --- |
| [MMKV](https://github.com/mrousavy/react-native-mmkv) | Fast key-value storage (replaces AsyncStorage and Realm for simple data) |

> **Note:** MMKV is synchronous and ~30x faster than AsyncStorage. Use it for all key-value persistence needs.

### Config & Secrets

| Tool | Purpose |
| --- | --- |
| [react-native-config](https://github.com/luggit/react-native-config) | Environment variables |
| [react-native-keychain](https://github.com/oblador/react-native-keychain) | Secure credential storage |

### Native Frameworks (for native modules)

| Platform | UI | Networking |
| --- | --- | --- |
| iOS | [SwiftUI](https://developer.apple.com/xcode/swiftui/) | [Alamofire](https://github.com/Alamofire/Alamofire) |
| Android | [Jetpack Compose](https://developer.android.com/jetpack/compose) | [Retrofit](https://github.com/square/retrofit) |

---

## New Architecture

Starting with React Native 0.82, the **New Architecture is always enabled and cannot be disabled**. The legacy architecture was frozen in June 2025.

Key components:

- **Fabric** — new rendering system with synchronous layout
- **TurboModules** — lazy-loaded native modules with JSI (no bridge serialisation)
- **JSI (JavaScript Interface)** — direct JS <> native communication
- **Codegen** — generates type-safe native bindings from TypeScript specs
- **Hermes v1** — default JS engine with significant performance improvements

All new projects must target the New Architecture. Check [React Native Directory](https://reactnative.directory/) for library compatibility.

---

## TypeScript Guidelines

- Prefer `type` over `interface` for type definitions. Only use `interface` when declaration merging is explicitly needed (rare).
- Never use the `any` type. Use `unknown` and narrow with type guards when the type is uncertain.
- Use the `satisfies` operator for better type inference with validation.

---

## State Management

- **Global / async state**: Use Redux Toolkit. API calls must be handled using `createAsyncThunk` within Redux slices inside `src/features/`.
- **UI / local input state**: Use `useState` inside React components. For complex local logic, extract into custom hooks under the feature's `hooks/` folder.
- **Typed hooks**: Always use typed `useAppDispatch` and `useAppSelector` from `src/common/lib/redux/hooks.ts`.

---

## Component Guidelines

- **No API calls in common components.** Common components must be pure and reusable.
- **No raw text.** All user-facing strings must use i18n keys. Add keys for every supported language before using them.
- **No inline styles.** Use StyleSheet or themed styles via React Native Elements' `ThemeProvider`.
- **No shared styles across features.** Each feature must have its own `styles/` folder.
- **Add a `testID` prop** to components for testing.
- **Document new common components** in your team's documentation platform with usage examples and props description.

---

## Testing

### What to test

| Target | Location |
| --- | --- |
| Screens | `src/screens/featureName/__tests__/` |
| Feature components | `src/screens/featureName/components/__tests__/` |
| Common components | `src/common/components/__tests__/` |
| Common layouts | `src/common/layouts/__tests__/` |
| Redux slices | `src/features/featureName/slices/__tests__/` |
| Navigation | `src/navigation/__tests__/` |

### Testing practices

- Use `testID` for querying elements.
- Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state.
- Mock API responses — never make real API calls in unit tests.
- Use `__mocks__/` folders for shared mock data.

---

## Localisation

- Managed via **i18next** + **react-i18next** + **react-native-localize**.
- `intl-pluralrules` polyfill is required for Hermes pluralisation support.
- Locale files live in `src/common/locales/` (`en.json`, `zh_HK.json`) — same location across all projects.
- **Never use raw text in components.** Always reference a translation key.
- Add keys to all language files before using them in code.

### i18n Key Naming Convention

See [i18n Conventions](../standards/i18n-conventions.md) for the full naming convention shared across all templates.

---

## Secrets & Credentials

- Environment-specific config managed via `react-native-config` (`.env.*` files).
- Sensitive credentials stored securely using `react-native-keychain`.
- Never commit secrets to the repository.

---

## Best Practices Summary

| Rule | Detail |
| --- | --- |
| Unit tests required | Write tests for components, screens, and Redux slices |
| No API calls in common components | Common components must be presentation-only |
| API calls via `createAsyncThunk` | All async data fetching lives in Redux slices under `src/features/` |
| Local state via `useState` | UI state stays in components; complex logic goes into custom hooks |
| No shared styles across features | Each feature owns its own `styles/` folder |
| Modular code | Keep code sectioned, clean, and reusable |
| No `any` type | Use proper types or `unknown` with type guards |
| No raw text | All strings use i18n keys |
| `type` over `interface` | Use `interface` only when declaration merging is needed |
| No inline styles | Use StyleSheet or themed styles |
| New Architecture only | Mandatory from RN 0.82+ |
