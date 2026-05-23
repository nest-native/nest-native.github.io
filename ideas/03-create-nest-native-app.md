# Create Nest Native App

**Project type:** New project, later.

**Suggested repository:** `nest-native/create-nest-native-app`

## Summary

Build a project generator or CLI that scaffolds production-minded Nest Native
applications.

This should not be the next immediate project. It becomes valuable after the
reference app and more samples prove which architecture choices are stable.

## Community Pain

Starting a serious Nest app involves many decisions:

- database integration
- transaction pattern
- migrations
- validation style
- OpenAPI or tRPC
- testing
- CI
- deployment
- health checks
- package manager
- folder structure

Most starters are either too minimal or too opinionated. A Nest Native
generator could give users a curated, production-shaped starting point.

## Possible User Flow

```bash
npm create nest-native-app
```

Prompts:

- project name
- package manager
- database driver
- Drizzle only, tRPC only, or both
- REST/OpenAPI, tRPC, or mixed
- class-validator DTOs or Zod opt-in
- include Docker/deployment files
- include GitHub Actions CI
- include example auth/context

## First Template

Start with one strong template instead of many weak ones:

- NestJS 11
- Node 20+
- `nest-drizzle-native`
- SQLite/libSQL local development
- optional PostgreSQL production recipe
- migrations
- health checks
- tests
- CI

Add tRPC only after the Drizzle-only template feels excellent.

## What It Should Generate

- `src/app.module.ts`
- database module
- schema/migrations folder
- health endpoints
- one feature module with repository/service/controller or router
- test setup
- CI workflow
- README with exact commands
- deployment notes

## Risks

- CLI maintenance can become expensive.
- Too many options can make the generated app inconsistent.
- Templates can drift from library best practices.
- Users may expect the CLI to solve application architecture decisions that
  should remain app-owned.

## Readiness Criteria

Do not start this until:

- the production reference app exists
- core samples are stable
- docs have settled
- common project structure decisions are clear
- release and update process for templates is understood

## Success Criteria

- A new user can generate an app and run tests in minutes.
- Generated code looks like something we would maintain ourselves.
- The template reinforces Nest Native philosophy instead of hiding it.

## Why This Is Worth Doing

If the ecosystem grows, a generator can dramatically reduce onboarding
friction. But it should come after the architecture is proven, not before.
