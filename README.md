# Runtime-Workspace-Framework

**Runtime, Workspace, and External Knowledge Sources: the minimal primitives of agent systems.**

A framework analysis derived from five streams of evidence, with **eight testable predictions** — each carrying an operational acceptance criterion — over an observation window ending in late 2028.

**The whole argument compresses into one sentence: the model will not devour everything, but it will keep replacing acceptance-executable, non-physical components.**

## The framework in one screen

Three primitives, each derived from an empirical regularity:

| Primitive | Derived from | One-line job |
|---|---|---|
| **Runtime** | closed-loop structure law | Execution process layer: load, syscall, schedule, interrupt. The model is a function; the runtime is the process. |
| **Workspace** | closed-loop structure law | Persistent state layer: the only structure that survives across sessions and base-model generations. Eval sets live here. |
| **External knowledge source** | collapse law | The world term: real signals from outside the closed loop, with their own collection cadence, entering the workspace via an explicit loading path. |

Four structural corollaries follow:

1. **The runtime thins.** Its judgment half (loading strategy, scheduling, routing) is absorbed by base-model iteration; its enforcement half (sandbox, permissions, credentials) persists, because a model of any strength needs boundaries. The runtime converges toward an OS kernel.
2. **Distillation is a conditional branch, not a default route.** Triggered only when four conjunctive conditions hold: high call frequency, stable distribution, automatically verifiable outputs, and sustained holdout superiority over "latest base model + workspace".
3. **Asset hierarchy: model < runtime < workspace.** The only preservation operations are backup, lineage, and evaluation-set maintenance.
4. **Handover to AI is ordered by acceptance executability.** Humans converge to the two ends: specification and acceptance adjudication. Where acceptance is not executable, "humans write requirements" degrades into "humans make wishes."

## Eight testable predictions (window: to late 2028)

| # | Prediction | Acceptance criterion (abridged) |
|---|---|---|
| P1 | Context loading moves from runtime config to model-driven retrieval within two base-model generations | Flagship runtimes' default path is model-driven retrieval, officially documented |
| P2 | Cross-runtime benchmark spread (currently 48 pts, single grade-B source) narrows | Two or more independent reproducible measurements under half the current spread |
| P3 | Enforcement persists; the surviving runtime converges toward a kernel | Sandbox/permission/credential layers stay backward-compatible, never natively replaced |
| P4 | Graph-coded orchestration frameworks marginalize within 12–24 months | Vendor pivot, acquisition, or sustained public decline in activity and deployments |
| P5 | Distillation degrades to a condition-triggered action | Products expose distillation behind explicit triggers (incl. holdout re-run on new base models), not defaults |
| P6 | L5 (end-to-end autonomous successor-model training) has no reproducible public implementation in the window | Third-party reproducibility on public code and data, else "not crossed" |
| P7 | Handover depth decreases with acceptance executability; evaluation-set design becomes a mainstream job form | Job-volume decline is steeper for acceptance-automatable roles; "evaluation-set design" appears in mainstream job requirements |
| P8 | Workspace operations become a distinct product category | Standalone tools/services for backup, lineage, and eval-set maintenance form an identifiable market |

Scoring happens by the criterion at expiry, not by impression. The full criteria are in the paper.

## Full paper

[`Runtime-Workspace-and-External-Knowledge-Sources.md`](./Runtime-Workspace-and-External-Knowledge-Sources.md)

Includes: the five evidence streams, four empirical regularities, formal definitions and propositions, the derivation chain, full acceptance criteria for all eight predictions, source grading (the eight 2026 arXiv IDs were individually verified against their abstract pages), and 30+ references with links.

## Status

- v1.0, October 2026
- Author's core judgments are preserved verbatim, marked "author's view" (translated from the original Chinese).
- Open problems are documented honestly (loop dynamics, cross-repository migration, curation path formalization) — these are incomplete parts of the framework, not counterexamples.
