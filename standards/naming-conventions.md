# Naming Conventions

> Shared naming standards across all project templates.

## Files & Folders

| Type | Convention | Example |
| --- | --- | --- |
| Components | PascalCase | `LoginPage.tsx`, `AuthGuard.tsx` |
| Screens (React Native) | PascalCase + `Screen` suffix | `LoginScreen.tsx`, `Feature1ListScreen.tsx` |
| Pages (React/Next.js) | PascalCase + `Page` suffix | `LoginPage.tsx`, `Feature1ListPage.tsx` |
| Hooks | camelCase with `use` prefix | `useAuth.ts`, `useAppDispatch.ts` |
| Redux slices | camelCase + `Slice` suffix | `feature1Slice.ts` |
| Selectors | camelCase | `selectors.ts` |
| Types | camelCase | `types/index.ts` |
| Utilities | camelCase | `formatDate.ts`, `parseQuery.ts` |
| Constants | camelCase (file), UPPER_SNAKE_CASE (values) | `constants/api.ts` → `API_BASE_URL` |
| Test folders | `__tests__/` | Co-located with source |
| Mock folders | `__mocks__/` | Co-located with source |
| Styles | camelCase or kebab-case | `styles/auth.module.css` |
| Config files | kebab-case or dotfiles | `eslint.config.mjs`, `.prettierrc.json` |

## Feature Folders

| Platform | Pattern | Example |
| --- | --- | --- |
| React (Vite) | `src/pages/{feature}/` | `src/pages/auth/` |
| Next.js 15 | `src/app/[locale]/(group)/{feature}/` | `src/app/[locale]/(dashboard)/orders/` |
| React Native CLI | `src/screens/{feature}/` | `src/screens/auth/` |
| Expo | `src/app/(group)/{feature}/` | `src/app/(tabs)/orders/` |
| Monorepo | `apps/{app}/src/app/[locale]/(group)/{feature}/` | Same as Next.js per app |

## Private Folders (File-based Routing)

In Next.js and Expo Router projects, folders prefixed with `_` are excluded from routing:

- `_components/` — feature-specific components
- `_hooks/` — feature-specific hooks
- `_styles/` — feature-specific styles
- `_types/` — feature-specific types
- `_lib/` — feature-specific utilities

## Route Groups (File-based Routing)

Folders wrapped in `()` are route groups — they share layouts without affecting the URL:

- `(auth)/` — auth-related routes
- `(dashboard)/` — dashboard routes
- `(public)/` — public pages
- `(tabs)/` — tab navigator (Expo)
- `(modals)/` — modal screens (Expo)

## TypeScript Types

| Convention | Example |
| --- | --- |
| Type aliases in PascalCase | `type UserProfile = { ... }` |
| Props types with `Props` suffix | `type LoginFormProps = { ... }` |
| Generic type parameters in PascalCase | `type ApiResponse<T> = { ... }` |
| Prefer `type` over `interface` | Unless declaration merging is explicitly needed |

## Redux

| Convention | Example |
| --- | --- |
| Slice file | `{feature}Slice.ts` |
| Action creators | camelCase with verb prefix | `fetchUsers`, `updateProfile` |
| Selectors | `select` prefix | `selectCurrentUser`, `selectIsLoading` |
| Typed hooks | `useAppDispatch`, `useAppSelector` |

## Navigation

| Platform | Convention | Example |
| --- | --- | --- |
| React Router | `{Feature}Page` components | `Feature1ListPage` |
| React Navigation | `{Feature}Screen` + `{Feature}StackNavigator` | `LoginScreen`, `AuthNavigator` |
| Expo Router | File names as routes | `login.tsx`, `[id].tsx` |
| Next.js | `page.tsx` per route segment | `feature1/page.tsx` |
