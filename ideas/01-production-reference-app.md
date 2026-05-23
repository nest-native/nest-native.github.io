# Production Reference App

**Project type:** New project.

**Suggested repository:** `nest-native/reference-app` or `nest-native/nest-native-reference`

## Summary

Build a real production-shaped application that uses the Nest Native libraries
together, especially `nest-drizzle-native` and `nest-trpc-native`.

The goal is not another small sample. The goal is a credible reference app that
answers the question maintainers and teams actually ask:

> Can I trust this stack in a serious NestJS application?

## Community Pain

Most library samples are too small. They prove an API call, but not the
operational shape of a real backend:

- feature modules
- auth and authorization
- request-scoped context
- transactions across services
- database migrations
- health checks
- tests with real database behavior
- deployment steps
- typed client consumption
- production documentation

Developers can understand a library API and still hesitate because they cannot
see how the pieces behave together in an application.

## Proposed App Shape

A compact but realistic SaaS-style application:

- accounts or organizations
- users and memberships
- roles or permissions
- projects/tasks/tickets
- audit log or activity feed
- transactional workflows
- admin-only endpoints
- typed frontend or typed client smoke checks

The app should avoid demo-only gimmicks. It should feel like a small backend a
team could adapt.

## Libraries Demonstrated

- `nest-drizzle-native` for database registration, repositories, transactions,
  migrations, named clients if needed, and testing utilities
- `nest-trpc-native` for decorator-first routers, procedures, enhancers, typed
  client generation, and Nest dependency injection
- standard NestJS guards, pipes, interceptors, filters, request scope, and
  lifecycle hooks

## What It Should Prove

- A transaction can span multiple services without passing `tx` through every
  method.
- DTO validation and tRPC/Zod style validation can coexist without making one
  mandatory everywhere.
- Repository classes remain thin homes for query code, not an Active Record
  rewrite.
- Health/readiness, migrations, CI, and release checks have a clear production
  path.
- The typed client story remains ergonomic.

## Suggested Milestones

1. Scaffold a minimal Nest app with Drizzle and tRPC.
2. Add database schema, migrations, and seed data.
3. Add auth context and request-scoped user/tenant providers.
4. Add core modules: users, organizations, projects, audit log.
5. Add transactional workflow: invite user, create project, record audit event.
6. Add tests: unit, integration, real DB, typed client checks.
7. Add deployment documentation and Docker/Kubernetes examples.
8. Add a README that explains architecture decisions without becoming a book.

## Success Criteria

- A new user can clone the app, run tests, and understand the architecture in
  under an hour.
- The app reveals real improvements or gaps in the existing libraries.
- Any library fixes discovered are handled in separate PRs, not mixed into the
  reference app.

## Why This Is Worth Doing

This is probably the highest-value next step. It strengthens both existing
libraries and creates a proof artifact for the whole Nest Native philosophy.

It also protects us from building unnecessary APIs. If the reference app feels
good with the current libraries, avoid adding helpers. If it feels repetitive in
specific places, those are the right candidates for future API design.
