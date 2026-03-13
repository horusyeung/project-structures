# Testing Strategy

> Shared testing standards across all project templates.

## Testing Tools by Platform

| Platform | Unit Testing | Component Testing | E2E Testing |
| --- | --- | --- | --- |
| React (Vite) | Vitest | React Testing Library | Playwright |
| Next.js 15 | Vitest | React Testing Library | Playwright |
| React Native CLI | Jest | @testing-library/react-native | Detox or Maestro |
| Expo | Jest | @testing-library/react-native | Maestro (recommended) or Detox |

> **Note:** `@testing-library/jest-native` has been merged into `@testing-library/react-native` v12.4+. Use the unified package for React Native projects.

---

## What to Test

### Web Projects (React / Next.js)

| Target | Location |
| --- | --- |
| Pages | `src/pages/featureName/__tests__/` (Vite) or `src/app/[locale]/(group)/feature/__tests__/` (Next.js) |
| Feature components | Co-located `components/__tests__/` or `_components/__tests__/` |
| Common components | `src/common/components/__tests__/` |
| Common layouts | `src/common/layouts/__tests__/` |
| Redux slices | `src/features/featureName/slices/__tests__/` |
| Router / Navigation | `src/router/__tests__/` (Vite) |
| E2E tests | `tests/e2e/` |

### Mobile Projects (React Native CLI / Expo)

| Target | Location |
| --- | --- |
| Screens | `src/screens/featureName/__tests__/` (CLI) or `src/app/(group)/feature/__tests__/` (Expo) |
| Feature components | Co-located `components/__tests__/` or `_components/__tests__/` |
| Common components | `src/common/components/__tests__/` |
| Common layouts | `src/common/layouts/__tests__/` |
| Redux slices | `src/features/featureName/slices/__tests__/` |
| Navigation | `src/navigation/__tests__/` (CLI only) |
| E2E tests | `tests/e2e/` |

### Monorepo

| Target | Location |
| --- | --- |
| App pages / features | `apps/{app}/src/app/[locale]/(group)/feature/__tests__/` |
| App-specific common | `apps/{app}/src/common/components/__tests__/` |
| Redux slices | `apps/{app}/src/features/featureName/slices/__tests__/` |
| Shared components | `packages/common/src/components/__tests__/` |
| Shared hooks | `packages/common/src/hooks/__tests__/` |

---

## Testing Practices

| Practice | Detail |
| --- | --- |
| Use test identifiers | `data-testid` (web) or `testID` (React Native) for querying elements |
| Mock Redux store | Use `configureStore` from `@reduxjs/toolkit` with preloaded state |
| Mock API responses | Never make real API calls in unit tests |
| Shared mock data | Use `__mocks__/` folders for shared mock data |
| Co-locate tests | Tests live next to the code they test in `__tests__/` folders |
| No `redux-mock-store` | Use the real `configureStore` with preloaded state instead |

---

## Test File Naming

| Convention | Example |
| --- | --- |
| Unit tests | `ComponentName.test.tsx` |
| Integration tests | `featureName.integration.test.tsx` |
| E2E tests | `featureName.e2e.test.ts` (Playwright) or `featureName.yaml` (Maestro) |
