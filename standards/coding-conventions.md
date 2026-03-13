# Coding Conventions

> Shared coding standards across all project templates.

## TypeScript

| Rule | Detail |
| --- | --- |
| Prefer `type` over `interface` | Only use `interface` when declaration merging is explicitly needed (rare) |
| Never use `any` | Use `unknown` and narrow with type guards when the type is uncertain |
| Use `satisfies` operator | Better type inference with validation |
| Path aliases | Use `@/`, `@common/`, `@features/` etc. configured in `tsconfig.json` |

```tsx
// Preferred
const config = { ... } satisfies AppConfig;

// Avoid — loses narrowed type info
const config: AppConfig = { ... };
```

---

## State Management

| State Type | Approach | Location |
| --- | --- | --- |
| Global / async state | Redux Toolkit (`createAsyncThunk`) | `src/features/{feature}/slices/` |
| UI / local input state | `useState` | Inside component or custom hook |
| Complex local logic | Custom hook | Feature's `hooks/` or `_hooks/` folder |

- Always use typed `useAppDispatch` and `useAppSelector` from `src/common/lib/redux/hooks.ts`.
- Mock Redux store using `configureStore` from `@reduxjs/toolkit` with preloaded state. `redux-mock-store` is unnecessary.

---

## Component Guidelines

| Rule | Detail |
| --- | --- |
| No API calls in common components | Common components must be pure and reusable |
| No raw text | All user-facing strings must use i18n keys |
| No inline styles | Use the project's styling approach (MUI `sx`, StyleSheet, CSS modules) |
| No shared styles across features | Each feature must have its own styles folder |
| Test identifiers | Add `data-testid` (web) or `testID` (React Native) to components |
| Document new common components | Add documentation with usage examples and props description |
| Lazy load pages | Use `React.lazy()` (Vite) or dynamic imports for page/screen components |
| Barrel exports sparingly | Prefer direct imports to avoid tree-shaking issues |

---

## Server Components (Next.js / Expo Router)

- **Server Components by default.** Only add `'use client'` when you need interactivity, hooks, or browser APIs.
- This applies to Next.js 15 and Expo Router projects.

---

## Secrets & Environment Variables

| Platform | Prefix | Notes |
| --- | --- | --- |
| Vite | `VITE_` | Bundled into client JS — never put secrets here |
| Next.js | `NEXT_PUBLIC_` | Bundled into client JS — never put secrets here |
| Expo | `EXPO_PUBLIC_` | Bundled into client JS — never put secrets here |
| React Native CLI | `react-native-config` | Environment-specific `.env.*` files |

- Never commit secrets to the repository.
- Use `.env.local` or `.env.development` for local development (gitignored).
- Store production secrets in a secrets manager (e.g., AWS Secrets Manager, Vault, Doppler).
