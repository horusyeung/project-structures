# Git Workflow

> Shared git workflow standards across all project templates.

## Branching Strategy

| Branch | Purpose | Merges Into |
| --- | --- | --- |
| `master` | Production-ready code | — |
| `staging` | Pre-production testing and QA | `master` |
| `develop` | Integration branch for features | `staging` |
| `feature/{name}` | New feature development | `develop` |
| `fix/{name}` | Bug fixes | `develop` |
| `hotfix/{name}` | Critical production fixes | `master` and `develop` |

---

## Workflow

1. Create a **feature branch** from `develop`.
2. Develop and commit with clear, descriptive messages.
3. Open a **pull request** to `develop`.
4. Every PR must be **reviewed and approved** before merging.
5. Ensure all tests pass and linting is clean before requesting review.
6. When `develop` is stable, merge into `staging` for QA.
7. When `staging` is verified, merge into `master` for production release.

---

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

```
type(scope): description

[optional body]

[optional footer]
```

### Types

| Type | Description |
| --- | --- |
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `style` | Code style changes (formatting, no logic change) |
| `refactor` | Code refactoring (no feature or fix) |
| `test` | Adding or updating tests |
| `chore` | Build process, tooling, or dependency updates |
| `perf` | Performance improvement |

### Examples

```
feat(auth): add login page with email validation
fix(dashboard): resolve chart rendering on mobile
refactor(common): extract date formatting utility
test(feature1): add unit tests for list screen
chore(deps): update React to 19.1
```

---

## Pre-commit Hooks

All projects use **Husky** for git hooks:

- **Pre-commit**: Run `lint-staged` to lint and format staged files.
- **Pre-push** (optional): Run type checking and tests.

---

## Pull Request Guidelines

| Rule | Detail |
| --- | --- |
| Keep PRs focused | One feature or fix per PR |
| Write a clear description | What changed, why, and how to test |
| Include screenshots | For UI changes |
| Link to issues | Reference related tickets or issues |
| Resolve all review comments | Before requesting re-review |
| Squash merge | Keep `develop` history clean |

---

## Code Review Checklist

- [ ] Code follows project coding conventions
- [ ] No `any` types
- [ ] No raw text (i18n keys used)
- [ ] No inline styles
- [ ] Tests added/updated for changes
- [ ] No API calls in common components
- [ ] TypeScript types are correct and complete
- [ ] No shared styles across features
- [ ] `data-testid` / `testID` added for new components
