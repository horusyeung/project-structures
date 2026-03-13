# Project Starter Templates

Production-ready project structure guidelines for modern web and mobile development. Built from experience shipping 10+ web platforms and 2 mobile apps across 15 global markets.

## Why This Exists

Most "starter templates" give you a todo app. These guidelines come from leading distributed engineering teams and shipping real products at scale. They cover not just folder structure, but coding standards, testing strategy, state management patterns, and i18n conventions that work across teams.

## Templates

| Template | Use Case | Key Stack |
|----------|----------|-----------|
| [React.js (Vite)](./react-vite/) | SPAs, dashboards, admin panels | React 19 + Vite + Redux Toolkit + MUI |
| [Next.js 15](./nextjs-15/) | SSR/SSG, SEO, public-facing apps | Next.js 15 App Router + TypeScript + MUI |
| [React Native CLI](./react-native-cli/) | Mobile apps (full native control) | RN 0.83+ + React Navigation 7 + Redux Toolkit |
| [React Native Expo](./react-native-expo/) | Mobile apps (managed workflow) | Expo SDK 55 + Expo Router + Redux Toolkit |
| [NestJS Backend](./nestjs-backend/) | Backend APIs | NestJS + PostgreSQL + Prisma + GraphQL + MongoDB |
| [Monorepo](./monorepo-turborepo/) | Multi-app projects | Turborepo + Yarn 4 + Next.js 15 |

## Shared Standards

These conventions are consistent across all templates:

| Standard | Description |
|----------|-------------|
| [Coding Conventions](./standards/coding-conventions.md) | TypeScript rules, component guidelines, state management |
| [Naming Conventions](./standards/naming-conventions.md) | Files, folders, variables, components |
| [i18n Conventions](./standards/i18n-conventions.md) | Translation key patterns, locale file structure |
| [Testing Strategy](./standards/testing-strategy.md) | What to test, where, and how |
| [Git Workflow](./standards/git-workflow.md) | Branching, PRs, code review |

## Configs

Ready-to-use configuration files in [`/configs`](./configs/):

- `.cursorrules` — AI-augmented development rules for Cursor
- `.coderabbit.yaml` — Automated code review config
- `eslint.config.mjs` — ESLint flat config (2024+)
- `tsconfig.base.json` — Strict TypeScript config
- `.prettierrc.json` — Formatting rules
- `ci.yml` — GitHub Actions CI template

## Key Decisions

### Why `type` over `interface`?

`type` is preferred for all type definitions. `interface` is only used when declaration merging is explicitly needed (rare). This keeps the codebase consistent and avoids the subtle differences between the two.

### Why Redux Toolkit over alternatives?

`createAsyncThunk` provides a standardized pattern for async operations across all projects. Combined with typed hooks (`useAppDispatch`, `useAppSelector`), it scales well for distributed teams where consistency matters more than individual preference.

### Why feature-based structure?

Grouping by feature (not by type) keeps related code together. When you work on "authentication", everything you need is in one folder — not scattered across `components/`, `hooks/`, `styles/`, and `types/`.

### Why MUI + React Native Elements?

MUI on web and React Native Elements on mobile share similar component APIs and theming patterns. This reduces context-switching for developers working across web and mobile codebases.

## Who This Is For

- Team leads setting up new projects
- Architects standardizing across multiple apps
- Developers who want opinionated, production-tested structures
- Teams transitioning from CRA to Vite or Next.js

## License

MIT
