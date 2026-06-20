---
layout: post
title: "Event Sourcing for Financial Data: Why Immutable Logs Beat CRUD"
date: 2026-06-18
description: >-
  An introduction to event sourcing — storing state as a sequence of
  immutable events — and why it fits financial and audit-heavy systems
  better than plain CRUD.
---

Most applications store state the same way: a row represents the current truth, and an `UPDATE` overwrites it. That works fine until someone asks "what was this account's balance last Tuesday?" or "who changed this value, and why?" — questions CRUD can't answer, because the previous state is gone.

Event sourcing solves this by changing what you store: not the current state, but every event that led to it.

## The Core Idea

Instead of a `balance` column you update in place, you store an append-only log of events:

```
AccountOpened    { account: "acct_1" }
FundsDeposited   { account: "acct_1", amount: 1000 }
FundsWithdrawn   { account: "acct_1", amount: 200 }
FundsDeposited   { account: "acct_1", amount: 50 }
```

The current balance isn't stored anywhere — it's *derived* by replaying the events: `0 + 1000 - 200 + 50 = 850`. The events are the source of truth; every view of "current state" is a projection computed from them.

## Why This Fits Financial Systems

- **Audit trail for free.** Regulators and auditors don't want "the balance is 850" — they want to know how it got there. With event sourcing, the full history is the data model, not a separate audit log bolted on afterward.
- **Point-in-time reconstruction.** Replaying events up to a given timestamp reproduces the exact state at that moment, which is exactly what reconciliation and historical reporting need.
- **Corrections without overwrites.** If a deposit was recorded in error, you don't edit history — you append a `FundsDepositReversed` event. The mistake and its correction both remain visible.

## Snapshotting

Replaying years of events on every read doesn't scale. The common fix is snapshotting: periodically persist the derived state (e.g., the balance after every 1,000 events), and on read, load the nearest snapshot and replay only the events after it.

## CQRS

Event sourcing pairs naturally with Command Query Responsibility Segregation (CQRS): writes append events, while reads are served from one or more projections (materialized views) built by consuming the event stream. This lets you build a fast "current balance" table for queries while keeping the event log as the durable, authoritative history.

## The Trade-offs

It isn't free:

- **Complexity.** You're building an event store, projections, and replay logic instead of an `UPDATE` statement.
- **Eventual consistency.** If projections are updated asynchronously, a read immediately after a write can be stale.
- **Schema evolution.** Old events were written under old assumptions; your event handlers need to keep understanding them as the system evolves.

## When It's Worth It

Event sourcing earns its complexity when the history itself is a business requirement — ledgers, payment systems, trading platforms, anything with compliance or audit obligations — rather than systems where "what is the value right now" is the only question anyone ever asks.
