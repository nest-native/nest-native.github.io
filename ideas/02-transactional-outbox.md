# Transactional Outbox Pattern

**Project type:** New project, with integration samples in existing projects.

**Possible future project:** `nest-native/nest-outbox-native`

**Integration targets:** `nest-drizzle-native`, `nest-trpc-native`, and ordinary
NestJS applications.

## Summary

Explore a Nest-native transactional outbox package for reliable side-effect
orchestration.

This goes beyond `nest-drizzle-native` scope. The Drizzle library can prove the
database transaction foundation, and the tRPC library can prove a typed
mutation boundary, but outbox processing itself is a separate concern.

## Community Pain

Backend teams often need to perform a database write and then trigger a side
effect:

- publish an event
- send an email
- enqueue a job
- sync data to another system
- call a webhook

The dangerous version is:

1. write to the database
2. send the message immediately
3. later discover the transaction rolled back

Or the opposite:

1. commit the database change
2. crash before sending the message

The transactional outbox pattern solves this by writing the side effect intent
inside the same transaction, then processing it after commit.

## Why It Fits Nest Native

The outbox pattern fits the broader Nest Native philosophy because it connects
several production concerns without hiding them:

- tRPC or REST receives the typed application command
- `@Transactional()` owns the workflow boundary
- repositories write domain rows and outbox rows in the same transaction
- a worker processes committed outbox records
- retries and idempotency are explicit
- no side effects happen before the database commit is durable

## Proposed First Proof

Start with a small proof app or reference-app slice, not a new package API:

- `nest-trpc-native` exposes a mutation
- `nest-drizzle-native` persists the domain row and outbox row
- an app-owned worker processes committed events
- tests prove rollback safety and retry behavior

Scenario:

- create an account
- insert an outbox event in the same transaction
- simulate rollback and prove no event is processed
- commit and prove the event is picked up
- mark event processed
- retry failed events safely

## Minimal Schema

- `accounts`
- `outbox_events`

Outbox fields:

- `id`
- `topic`
- `payload`
- `status`
- `attempts`
- `available_at`
- `processed_at`
- `created_at`

## Documentation Topics

- why not publish inside the transaction method
- how to keep payloads safe and versioned
- idempotency keys
- retry strategy
- worker ownership
- poison message handling
- observability
- when a queue is still needed

## Possible Public API

Only after proof-first validation:

- `OutboxModule`
- `OutboxRepository`
- `OutboxProcessor`
- `@OutboxHandler('topic')`
- retry and idempotency helpers

But this should not be rushed. A premature package could become too opinionated
about queues, retries, serialization, and deployment.

## Success Criteria

- The proof proves rollback safety with a real database.
- The docs explain the pattern without pretending the library owns every queue
  strategy.
- The implementation remains small enough that application teams can adapt it
  before a reusable package exists.

## Why This Is Worth Doing

This solves a real production problem that sits between database transactions,
API boundaries, workers, and side effects. It deserves its own project if the
proof shows enough repeated shape to justify a package.
