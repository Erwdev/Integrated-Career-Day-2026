# Integrated Career Day

[![Release](https://img.shields.io/github/v/release/Erwdev/Integrated-Career-Day-2026?style=flat-square&label=release)](https://github.com/Erwdev/Integrated-Career-Day-2026/releases)
[![CI](https://img.shields.io/github/actions/workflow/status/Erwdev/Integrated-Career-Day-2026/ci.yml?style=flat-square&label=ci)](https://github.com/Erwdev/Integrated-Career-Day-2026/actions/workflows/ci.yml)
[![Last commit](https://img.shields.io/github/last-commit/Erwdev/Integrated-Career-Day-2026?style=flat-square)](https://github.com/Erwdev/Integrated-Career-Day-2026/commits)

![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Zustand](https://img.shields.io/badge/Zustand-433E38?style=flat-square)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-FF4154?style=flat-square&logo=reactquery&logoColor=white)
![Zod](https://img.shields.io/badge/Zod-3E67B1?style=flat-square&logo=zod&logoColor=white)
![Vitest](https://img.shields.io/badge/Vitest-6E9F18?style=flat-square&logo=vitest&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)
![ESLint](https://img.shields.io/badge/ESLint-4B32C3?style=flat-square&logo=eslint&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-5FA04E?style=flat-square&logo=nodedotjs&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

## Table of contents

- [About](#about)
- [Project structure](#project-structure)
- [Tech stack](#tech-stack)
- [Getting started](#getting-started)
- [Available scripts](#available-scripts)
- [Architecture rules](#architecture-rules)
- [Team workflow and ownership](#team-workflow-and-ownership)
- [Working with AI agents](#working-with-ai-agents)
- [Code quality](#code-quality)
- [Testing](#testing)
- [CI/CD](#cicd)
- [Security](#security)
- [Releases](#releases)
- [Contributing](#contributing)
- [License](#license)

## About

Integrated Career Days is a web platform connecting companies, job seekers, and event organizers. It enables companies to showcase vacancies and meet candidates, job seekers to explore opportunities and manage profiles, and organizers to manage events, participants, and schedules.

The platform centralizes UGM ICD information, including event schedules, participating companies, career activities, registration details, and announcements. Built with Next.js, TypeScript, and Tailwind CSS, it follows a modular architecture that supports parallel development, code reusability, and seamless collaboration across developers and AI coding agents.

## Project structure

```text
Integrated-Career-Day-2026/
  .agents/                     # AI agent workspace
    prompts/                   # Prompt templates for UI and feature generation
    skills/                    # Agent skills, including React Doctor
    rules.md                   # Coding rules and import restrictions
    architecture-graph.json    # Allowed dependencies, used to detect import violations
  .github/
    workflows/                 # CI/CD pipelines (ci.yml)
    dependabot.yml             # Automated dependency updates
  docs/
    decisions/                 # Architecture decision records
    ownership/                 # Who owns which feature or component
    design-system.md           # Token sync rules from Figma
  e2e/                         # Playwright tests, isolated from src/
    fixtures/                  # Test data and setup
    page-objects/              # Page Object Models
    specs/                     # One spec file per user journey
  public/                      # Fonts, icons, global images
  src/
    app/                       # Layer 1: routing only, no logic
      (public)/                # Public route group, each page.tsx renders one view
      api/                     # Route handlers for webhooks and third-party integrations
    views/                     # Layer 2: page composition, no business logic
    features/                  # Layer 3: closed domain modules, one owner per feature
      _template/               # Copy this to start a new feature
        actions/               # Server Actions for mutations
        api/                   # Fetchers and TanStack Query hooks
        components/            # UI used only by this feature
        hooks/                 # UI logic hooks, no data fetching
        schemas/               # Zod schemas for forms and payloads
        store/                 # Feature-local Zustand store
        types/                 # Internal types
        index.ts               # Public API of the feature
      auth/                    # Example feature
      user-profile/            # Example feature
    components/                # Layer 4: design system
      ui/                      # Primitives, one folder per component
        button/                # button.tsx, button.variants.ts, button.test.tsx, index.ts
        input/
        modal/
      layout/                  # Header, footer, not-found content
      shared/                  # Components reused by more than one feature
    stores/                    # Global Zustand stores (UI state, session state)
    styles/                    # Layer 5: Tailwind v4 and design tokens
      tokens/                  # Colors, spacing, radius, typography, shadows
      base/                    # Global base styles
      globals.css              # Main CSS entry point, imports Tailwind and all tokens
    lib/                       # Layer 6: infrastructure
      api-client/              # HTTP client and interceptors
      query/                   # QueryClient configuration
      telemetry/               # Monitoring wrappers
      utils/                   # Helpers such as cn()
    providers/                 # Unavoidable providers only (TanStack Query)
    config/                    # Environment variable validation with Zod
    types/                     # Shared TypeScript types
    test/                      # Shared test infrastructure
      mocks/                   # MSW handlers
      test-utils.tsx           # Custom render function
  AGENTS.md                    # Entry point for AI agents
  package.json                 # Dependencies and scripts, exact versions pinned
  package-lock.json            # Committed lockfile
  .npmrc                       # save-exact and engine-strict
  .nvmrc                       # Pinned Node.js version
  .env.example                 # Environment variable template
```

Border radius tokens live in `src/styles/tokens/radius.css`. All other style values follow the same rule: one token file per category.

## Tech stack

| Area | Tool | Purpose |
| --- | --- | --- |
| Framework | Next.js (App Router) | Routing, rendering, route handlers, and server actions |
| UI library | React | Component model |
| Language | TypeScript | Typed application code |
| Styling | Tailwind CSS v4 | CSS-first utility styling driven by design tokens |
| Variants | class-variance-authority | Typed component variants bound to tokens |
| App state | Zustand | Global and feature-local client state, no Context providers |
| Server state | TanStack Query | Data fetching, caching, and mutations |
| Validation | Zod | Forms, API payloads, and environment variables |
| Unit tests | Vitest and Testing Library | Tests colocated with source code |
| API mocking | MSW | Mock handlers shared by unit tests |
| E2E tests | Playwright | Browser tests in `e2e/` |
| Code quality | ESLint, React Doctor | Static linting and React anti-pattern detection |
| CI/CD | GitHub Actions | Install, audit, lint, typecheck, test, build, and E2E |

## Getting started

### Prerequisites

- Git
- Node.js, using the version defined in `.nvmrc`
- npm, bundled with that Node.js version

If you use nvm, run `nvm use` in the repository root to switch to the correct version.

### Clone the repository

```bash
git clone https://github.com/Erwdev/Integrated-Career-Day-2026.git
cd Integrated-Career-Day-2026
```

### Install dependencies

```bash
npm ci
```

Always use `npm ci` instead of `npm install`. It installs exactly what is recorded in `package-lock.json` and fails if the lockfile and `package.json` disagree. This keeps installs reproducible and makes sure the reviewed, patched dependency versions are the ones that run on your machine and in CI.

Use `npm install` only when you intentionally add or update a dependency, then commit the updated `package-lock.json` in the same pull request.

### Configure environment variables

Copy the example file and fill in the values:

```bash
cp .env.example .env.local
```

| Name | Description | Required |
| --- | --- | --- |
| `NEXT_PUBLIC_APP_URL` | Public base URL of the site | Yes |
| `NEXT_PUBLIC_API_URL` | Base URL of the backend API | Yes |

Environment variables are validated at startup with Zod in `src/config`. The app fails fast if a required variable is missing or invalid. Never commit `.env.local` or real secrets.

### Run the development server

```bash
npm run dev
```

Open http://localhost:3000 in your browser.

### Build and run for production

```bash
npm run build
npm run start
```

## Available scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the development server |
| `npm run build` | Create a production build |
| `npm run start` | Run the production build |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | Run the TypeScript compiler without emitting files |
| `npm run test` | Run unit tests once with Vitest |
| `npm run test:watch` | Run unit tests in watch mode |
| `npm run test:e2e` | Run Playwright end-to-end tests |
| `npm run doctor` | Run React Doctor on the whole project |
| `npm run doctor:diff` | Run React Doctor on changed files only |

## Architecture rules

These rules apply to every contributor and every AI agent. They are enforced by review, ESLint import rules, and React Doctor.

1. `src/app` contains routing only. Each `page.tsx` renders one view from `src/views` and nothing else.
2. `src/views` composes features and shared components. It contains no business logic.
3. `src/features/<name>` is a closed module with a public API through `index.ts`. A feature must not import from another feature. Share code through `src/components/shared` or `src/lib` instead.
4. `src/components/ui` holds design system primitives. Each component has its own folder with its component file, variants file, test file, and `index.ts`.
5. Styling values come only from tokens in `src/styles/tokens`. Do not hardcode colors, spacing, font sizes, shadows, or border radius in components. Border radius tokens live in `src/styles/tokens/radius.css`.
6. Global app state lives in `src/stores` and uses Zustand. Feature-local state lives in `src/features/<name>/store`. Avoid React Context unless a library requires it.
7. Data fetching lives in `src/features/<name>/api`. UI logic hooks live in `src/features/<name>/hooks`. Do not mix data fetching with UI logic.
8. Before creating a new component, search `src/components/ui` and `src/components/shared` for an existing one. Do not duplicate.
9. Server mutations go through Server Actions in `src/features/<name>/actions`. Route handlers in `src/app/api` are for webhooks and third-party integrations only.
10. Validate all external input with Zod schemas from `src/features/<name>/schemas`.

## Team workflow and ownership

The workflow follows the pre-agentic model: each person owns one unit of work, so changes rarely collide.

- One person owns one feature folder or one component folder.
- Ownership is recorded in `docs/ownership`. Update it when work is reassigned.
- Views in `src/views` are integration points. Only the integration owner edits them.
- Branch names: `feat/<name>`, `fix/<name>`, `chore/<name>`, `docs/<name>`, `test/<name>`.
- Commit messages follow Conventional Commits, for example `feat(auth): add login form`.
- Every pull request has a clear scope, a linked issue, screenshots for visible UI changes, and passing checks. Keep pull requests small.

Start a new feature by copying the template:

```bash
cp -r src/features/_template src/features/<new-feature>
```

Then rename the module, update its `index.ts`, and add your name to `docs/ownership`.

## Working with AI agents

AI coding agents are welcome, within strict boundaries.

- `AGENTS.md` at the repository root is the entry point for every agent and links to `.agents/`.
- `.agents/rules.md` holds coding rules and import restrictions.
- `.agents/prompts/` holds approved prompt templates for generating UI and features.
- `.agents/skills/` holds agent skills, including the React Doctor workflow.
- `.agents/architecture-graph.json` describes allowed dependencies so import violations can be detected.

Rules for agents and the people who drive them:

1. Work only inside the folder you own.
2. Check `src/components/ui` and `src/components/shared` before writing a new component.
3. Use tokens from `src/styles/tokens`. Never hardcode style values.
4. Run React Doctor and the full check suite before committing.
5. Do not add dependencies, change the lockfile, or edit files outside your ownership without review.

## Code quality

Run lint:

```bash
npm run lint
```

Run the type check:

```bash
npm run typecheck
```

Run React Doctor on changed files before every commit:

```bash
npm run doctor:diff
```

Run React Doctor on the whole project and print the score, as CI does:

```bash
npx -y react-doctor@latest . --score
```

[React Doctor](https://github.com/millionco/react-doctor) is a deterministic scanner that finds state and effect problems, performance issues, architecture problems, security issues, and accessibility issues that are common in AI-generated React code.

## Testing

| Type | Location | Tooling |
| --- | --- | --- |
| Unit and component tests | Next to the source file, for example `button.test.tsx` | Vitest, Testing Library |
| Shared setup and mocks | `src/test/` | MSW, custom render in `test-utils.tsx` |
| End-to-end tests | `e2e/specs/`, organized by user journey | Playwright |
| Page objects and fixtures | `e2e/page-objects/` and `e2e/fixtures/` | Playwright |

Run unit tests:

```bash
npm run test
```

Run unit tests in watch mode:

```bash
npm run test:watch
```

Run end-to-end tests:

```bash
npm run test:e2e
```

## CI/CD

Pipelines live in `.github/workflows`. The main workflow, `ci.yml`, runs on every pull request and on pushes to `main`. Jobs run in this order:

1. **Install**: `npm ci` using the committed lockfile.
2. **Audit**: `npm audit` fails the build on unresolved vulnerabilities.
3. **Lint**: `npm run lint`.
4. **Typecheck**: `npm run typecheck`.
5. **React Doctor**: `npx -y react-doctor@latest . --score`.
6. **Unit tests**: `npm run test`.
7. **Build**: `npm run build`.
8. **End-to-end tests**: `npm run test:e2e`.

A pull request can be merged only when every job passes.

## Security

- `package-lock.json` is always committed and never ignored.
- Dependency versions are pinned exactly through `save-exact=true` in `.npmrc`.
- `engine-strict=true` in `.npmrc` makes installs fail on unsupported Node.js versions.
- CI runs `npm audit` on every pull request.
- Dependabot opens pull requests for npm packages and GitHub Actions.
- Keep Next.js on the latest patched release. Next.js publishes security advisories regularly, so review the official advisories before each release.
- Never commit secrets. Keep local values in `.env.local`, which is ignored by Git.

To report a vulnerability, contact the maintainers privately instead of opening a public issue.

## Releases

Releases follow [Semantic Versioning](https://semver.org) and [Conventional Commits](https://www.conventionalcommits.org).

| Commit type | Version bump |
| --- | --- |
| `fix:` | Patch |
| `feat:` | Minor |
| `feat!:` or a `BREAKING CHANGE` footer | Major |

To cut a release:

1. Make sure `main` is green in CI.
2. Update `CHANGELOG.md` with the changes since the last release.
3. Update the version in `package.json` and commit.
4. Tag and publish:

```bash
git tag v0.1.0
git push origin v0.1.0
gh release create v0.1.0 --generate-notes
```

All releases are listed on the [Releases page](https://github.com/Erwdev/Integrated-Career-Day-2026/releases).

## Contributing

1. Open an issue or pick an existing one.
2. Confirm which folder you own in `docs/ownership`.
3. Create a branch from `main` using the naming convention above.
4. Build your change inside your own folder and follow the architecture rules.
5. Run `npm run lint`, `npm run typecheck`, `npm run test`, and `npm run doctor:diff`.
6. Open a pull request with a clear description and screenshots for UI changes.

## License

No license is declared yet. Add a `LICENSE` file before the first public release and update this section.