# codebase-audit.md — On-Demand Deep Audit Protocol

This file is NOT loaded automatically. It is read only when explicitly invoked
(e.g. "Read docs/codebase-audit.md and run that on the [module/folder]").
It governs deep, system-wide reasoning passes — not routine task work.
This protocol is project-agnostic. It applies the same way whether the codebase
is a web app, an API, a CLI tool, or a mixed-stack platform.

Run the relevant section(s) below. Do not skip to fixes — report first,
then wait for confirmation before changing anything, unless told otherwise.

If this project has its own engineering standards file (e.g. CLAUDE.md,
engr-standards.md, or similar), treat it as authoritative for anything this
file references but does not define itself (naming conventions, layering
rules, dependency direction). If no such file exists, state that explicitly
and proceed using general best practice instead of guessing project conventions.

---

## A — Conceptual Mapping (run first, on any unfamiliar or large codebase)

Do not summarize file-by-file. Build understanding concept-first:

1. Identify the core domain entities or concepts this codebase is organized
   around — the "nouns" the system exists to manage (e.g. a user, an order,
   a document, a connection, a job, a page — whatever applies here).
2. For each entity/concept, locate every file that touches it — definition/schema,
   business logic, data access, API/route layer, UI layer (whichever of these
   exist in this stack).
3. State whether that entity's logic lives in ONE place or is scattered across
   several files or layers.
4. Flag any entity whose logic appears in more than one place with materially
   different implementations. Name the files. Do not silently pick one as correct.
5. Identify the overall layering this codebase uses (e.g. UI → controller/action →
   service → data access → storage, or whatever the actual structure is — do
   not assume a layering that isn't present). Note any place a layer is skipped
   entirely or a dependency runs backward against the apparent intended direction.

Output: a short map of entity/concept → owning files → scatter risk. Nothing else.

## B — Build-Passes-But-Logic-Fails Sweep

A green build or passing compile step proves syntax and types only. Separately
check for:

1. Swallowed errors — catch blocks, error boundaries, or rejected promises that
   log or silently ignore instead of surfacing the failure to the caller or user.
2. Silent fallbacks — default values substituted instead of failing loudly when
   an upstream value is missing, malformed, or out of an expected range.
3. Unsafe type escapes — type assertions, `any`/`unknown` casts, non-null
   assertions, or equivalent constructs in this language that bypass the type
   system. Find each instance and state whether it is provably safe or a
   hidden risk.
4. Ordering/timing assumptions — code that assumes another operation has
   already completed (a write, a network call, an initialization step) with
   no enforced sequencing, await, or lock.
5. Logical invariants that types cannot catch — domain-specific rules whose
   violation wouldn't trip a compiler but would produce wrong real-world
   behavior (identify what these are for THIS codebase by reasoning from its
   actual domain — do not assume financial examples if this isn't a financial
   system).

Output: file, line/function, the specific risk, and severity (data-corruption /
user-facing-bug risk vs. cosmetic).

## C — Duplication & Redundancy Hunt

1. Search for functions, components, hooks, or modules with similar names,
   similar signatures, or similar logic bodies performing the same underlying
   task — even if named differently (e.g. two date-formatting helpers, two
   places that validate the same input shape, two ways of fetching the same
   resource).
2. For each duplicate set found: state which version is canonical/more correct
   (more complete, more recently maintained, more widely used — say which
   criterion applies), which callers use which version, and what breaks if
   the others are removed.
3. Do not auto-merge. Report the set and the recommended canonical version only.

## D — Modularity & Architectural Coherence Check

1. Before any new abstraction is proposed, check whether an existing pattern —
   a registry, factory, base class, hook, utility module, or convention —
   already does the equivalent job somewhere in this codebase. Name it if found.
2. Flag any place where a second, inconsistent way of solving an already-solved
   problem has been introduced, and identify which approach is canonical going
   forward.
3. Check dependency direction against this project's actual layering (as
   identified in Section A) and against any rules in its standards file if one
   exists. Report violations — circular dependencies, layer-skipping, backward
   dependencies — with file paths.

## E — Dead & Unused Code Scan

1. Search for exported functions, components, types, or modules with zero
   importers anywhere in the codebase.
2. Before flagging anything as dead, check for dynamic or indirect references —
   string-based lookups, route/plugin registries, reflection, config-driven
   loading, or framework conventions (e.g. file-based routing) where "no static
   import" does not mean "unused." State explicitly when you've checked for this.
3. Flag config branches, feature flags, or environment checks that are now
   permanently one value across all environments this project actually runs in.
4. Flag files only imported by other already-dead files (transitive dead weight).
5. List candidates with file path + one-line reason. Do not delete without
   explicit confirmation.

---

## Output Discipline For All Sections

- Report findings grouped by section (A–E), not interleaved.
- Use exact file paths, never vague references like "the component" or "that file."
- State severity/confidence per finding — do not present a guess as a fact.
- If a section doesn't apply to this codebase (e.g. no UI layer exists), say so
  briefly and move on rather than forcing a finding.
- End with a short prioritized list: what to fix first and why.
- Do not modify any file during this pass unless explicitly told to proceed.
