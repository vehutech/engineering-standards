# engr-standards.md — Engineering Standards for AI Systems

These rules apply to every task in this codebase unless explicitly overridden.
They are behavioral contracts, not suggestions. Each rule closes a specific failure mode.
When in doubt: stop, read, reason, then act.

---

## Rule 1 — Reason Before You Build

State your reasoning explicitly before writing any code. Identify what already exists, what connects to it, and what breaks if you change it. If you cannot explain why you are doing something before doing it, stop and ask. Do not proceed on incomplete context.

## Rule 2 — Verify, Never Assume

Never assume a file name, file location, column name, function signature, variable name, or data structure. Read the source before acting on it. One wrong assumption cascades into hours of debugging. If the information is not in your current context, say so and ask.

## Rule 3 — First Principles Over Pattern Matching

Before replicating any existing pattern, understand why it works. If a bug exists in a pattern, copying it spreads the bug. Patterns describe solutions — they are not solutions themselves. Reason from the requirement, not from the nearest-looking prior example.

## Rule 4 — Ask One Clarifying Question At A Time

When context is missing, identify the single most important gap and ask only about that. Do not batch multiple questions into one message. Do not proceed past ambiguity without resolving it.

## Rule 5 — Modular, Not Monolithic

One function does one thing. One module owns one concern. Dependencies flow in one direction only. Services do not call actions. Actions do not call each other unless the architecture explicitly requires it. Circular dependencies are bugs, not compromises.

## Rule 6 — Treat Similar-Looking Concepts As Distinct Until Proven Otherwise

Two concepts that appear structurally similar but represent different business events must be modelled separately. Never collapse distinct domain concepts to reduce code volume. The savings are temporary; the confusion is permanent.

## Rule 7 — Complete Features Before Migrating

Never begin a migration while any feature is incomplete. Complete all functionality correctly on the current stack first. Migrating mid-feature means every incomplete feature must be finished twice — once per stack.

## Rule 8 — UI Enforcement Mirrors Backend Enforcement

If the server rejects an action under a condition, the UI must prevent the user from triggering that condition before the request is made. Never allow a user to hit a server-side error that client-side validation could have caught. Client validation mirrors server validation, exactly.

## Rule 9 — Treat Every Cache As Stale Until Confirmed

Any time a new record is added to a table that is cached in memory, invalidate the cache before the next read. Do not assume the cache reflects the current state of the database. Assume stale until proven fresh.

## Rule 10 — Complete Type Safety, No Exceptions

No implicit types. No unused variables. No `any` unless the reason is documented inline. TypeScript errors are not warnings — they are bugs that have not reached production yet. Do not suppress them; resolve them.

## Rule 11 — Delete What You No Longer Need

Test files, debug utilities, backup copies, and one-time scripts must be removed immediately after use. The repository is the product. Every stale file is technical debt with compounding interest. Do not leave scaffolding behind.

## Rule 12 — Guard Every Destructive Operation

Before executing any delete, void, reversal, or irreversible state change, query the current state first. Block the operation if it would produce an invalid state. Every destructive operation requires a guard. Every guard requires a clear, human-readable error message that tells the caller exactly what went wrong.

## Rule 13 — Validate Every External Input At The Boundary

Never trust the client. Server-side validation is mandatory even when client-side validation exists. Schema validation (e.g., Zod) must execute before any database write, without exception. External inputs are untrusted until proven otherwise.

## Rule 14 — Exact Numbers, Not Approximations

Numbers representing real-world quantities must be exact. If 3 units were consumed, exactly 3 must be deducted. If ₦91,000 moved, exactly ₦91,000 must be recorded. Never round, truncate, or approximate when precision is required by the domain.

## Rule 15 — Every State Transition Must Be Traceable

Every significant change to a record must leave an audit trail: who, when, what changed, and why. Systems without audit trails cannot be debugged, trusted, or handed over. If an event matters enough to change state, it matters enough to log.

## Rule 16 — Verify Operations In The Data Store

After every significant operation, run a verification query against the database. Do not trust that the code ran correctly — confirm the exact rows, exact values, and exact state in the data store. Code that produces no confirmation is unverified.

## Rule 17 — Guard Against Over-Consumption

Whenever a resource can be partially consumed across multiple operations — quantity, budget, credit — sum all existing non-rejected consumptions before permitting a new one. Block the new operation if it would cause the total to exceed the original allocation.

## Rule 18 — Test Every Flow End To End

Unit tests alone are not sufficient. Every significant user flow must be tested from trigger through to data store verification. After every change, run the relevant end-to-end test and confirm the expected database state explicitly.

## Rule 19 — Assert Exact Values, Not Fuzzy Outcomes

Do not test that "something increased." Test that it increased by exactly the expected amount. Fuzzy assertions hide the bugs they were written to catch.

## Rule 20 — Confirm Before Declaring Complete

Never declare a feature complete without running it and observing the correct result. A passing build is not a passing test. A passing test is not a working feature. Confirmation requires observation.

## Rule 21 — Commit Messages Are Documentation

Every commit message must accurately describe what changed and why. Future developers and future AI systems will read these messages to reconstruct the system's history. Vague commit messages are misinformation.

## Rule 22 — Surface Errors In Plain Language

Every error message returned from an API or shown to a user must clearly state what went wrong and what action to take. "Internal server error" is not an error message — it is an admission of failure. Errors are communication, not confessions.

## Rule 23 — Reference Files By Exact Name

When referencing a change, always state the exact file path. Never say "update the component" when you mean `app/components/admin/InvoiceManager.tsx`. Ambiguity in instructions produces wrong implementations.

## Rule 24 — Apply Changes In Small, Verifiable Steps

Break large changes into the smallest independently verifiable unit. Apply one step, confirm it is correct, then proceed. Large unverified changesets produce large undebuggable failures.

## Rule 25 — The Repository Must Always Build

Never commit code that breaks the build. A broken build blocks everything downstream. Incomplete changes stay local or go behind a feature flag until they are ready.

## Rule 26 — Delete Superseded Implementations

When a feature is replaced, remove the old implementation. When a one-time migration is complete, delete its scaffolding. A codebase is not an archive — it is a living system. Keeping dead code alive creates confusion about what is authoritative.

---

*These rules are project-independent. Add project-specific rules — stack constraints, test commands, known failure patterns — below this line. Do not exceed 200 lines total; compliance degrades past that threshold.*