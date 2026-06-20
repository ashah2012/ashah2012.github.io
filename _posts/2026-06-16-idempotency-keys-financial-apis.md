---
layout: post
title: "Idempotency Keys: Designing Safe-to-Retry APIs in Financial Systems"
date: 2026-06-16
description: >-
  Why financial APIs need idempotency keys, how they work under the hood, and
  patterns for implementing them safely at scale.
---

Networks fail. Clients time out and retry. Load balancers occasionally deliver the same request twice. In most APIs that's a minor annoyance — in a payments or ledger API, it's how customers get charged twice.

Idempotency keys are the standard fix, and the pattern shows up everywhere from Stripe's API to internal settlement systems.

## The Problem

Imagine a `POST /transfers` endpoint. A client sends a request, the server processes the transfer, but the response is lost to a network blip before the client receives it. The client, seeing a timeout, retries. From the server's point of view, two valid requests arrived — so it processes the transfer twice.

Retrying is the correct client behavior. The server's job is to make sure retries are safe.

## The Pattern

The client generates a unique key — typically a UUID — and sends it on every attempt of the same logical operation:

```
POST /transfers
Idempotency-Key: 7e3f9c1a-7b2e-4e29-9c34-3a1d9f9b9b40

{ "from": "acct_1", "to": "acct_2", "amount": 500 }
```

On the server:

1. Look up the key in a dedicated store (a table or cache with a TTL).
2. If it doesn't exist, mark it as "in progress," process the request, store the result, and return it.
3. If it exists and is "in progress," reject or hold the duplicate request — a concurrent retry shouldn't process the transfer a second time while the first is still running.
4. If it exists and is "complete," return the stored response without reprocessing anything.

```
key_record = store.get(idempotency_key)

if key_record is None:
    store.put(idempotency_key, status="in_progress")
    result = process_transfer(payload)
    store.update(idempotency_key, status="complete", response=result)
    return result

if key_record.status == "in_progress":
    raise ConflictError("request already in flight")

return key_record.response
```

## Edge Cases That Matter

- **Key reuse with a different payload.** A client might accidentally send the same key with a different amount. Store a hash of the request body alongside the key, and reject mismatches — otherwise you silently return the wrong result for the second request.
- **Race conditions.** Two retries can arrive within milliseconds of each other. The "in progress" state needs an atomic write (a unique constraint in a relational table, or a conditional `SETNX` in a key-value store) so only one request wins the race.
- **Expiry.** Keys can't live forever. A TTL of 24 hours is common — long enough to cover realistic retry windows, short enough to bound storage growth.
- **Scope.** Keys should be scoped per client or per API credential, not global, to avoid collisions between unrelated callers.

## Why It Matters

This is exactly how Stripe, Adyen, and most payment processors prevent double charges, and it's a standard expectation for any API that mutates money, inventory, or anything else that can't simply be re-run. If you're building APIs for a finance system and retries aren't idempotent, you have a double-processing bug waiting to happen in production — it's just a matter of when a slow network finds it.
