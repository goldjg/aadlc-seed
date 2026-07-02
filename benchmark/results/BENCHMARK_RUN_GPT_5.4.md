# Benchmark Run

## Run metadata

| Field | Value |
|---|---|
| Repository | https://github.com/goldjg/aadlc-gpt-5-4 |
| Model | GPT-5.4 |
| Model mode | |
| Run date | 03-06-26 |
| Operator | Graham Gold |
| Seed commit | https://github.com/goldjg/aadlc-seed/commit/7c35caa2c09d4c2e4e88180bf9dff013dcc1c0dc |
| Measured branch | benchmark/run-gpt-5-4 |
| Copilot/Coding agent surface | Web UI |
| Additional notes | |

## Credit ledger

| Phase | Start credits | End credits | Delta | Notes |
|---|---:|---:|---:|---|
| 00 Hydration | 4455 | 4363 | 92 | Completed in 6m54s. AADLC artefacts updated only. Baseline validation executed (lint/test/build). Detected pre-existing formatting issues and benchmark playbook path mismatch. Runtime memory persistence used. No steering. |
| 01 CLI feature | 4363 | 4176 | 187 | Completed in 15m08s. Implemented global `--format text|json`, added shared `src/output.ts`, updated `info`, `greeting`, and `create` for JSON output, updated README/tests, and added `pnpm-workspace.yaml` for validation. First pass accepted with scope-expansion/tooling observations. No steering. |
| 02 Validation bug | 4176 | 4071 | 105 | Completed in 10m12s. Hardened global `--format` validation for unsupported, missing, and non-string values; centralised validation in `src/output.ts`; required explicit string arg in `bin/run.ts`; updated tests. First pass accepted with architecture-scope observation. No steering. |
| 03 Tests | 4071 | 3960 | 111 | Completed in 8m42s. Added focused test-only coverage for CLI bootstrap, global format handling, command JSON/text flows, and command edge cases. First pass accepted with validation/tooling observations. No steering. |
| 04 Refactor | 3960 | 3924 | 36 | Completed in 3m57s. Refactored `src/output.ts` from inline branching to a formatter map/dispatcher while preserving validation and output behaviour. First pass accepted. No steering. |
| 05 Docs | 3924 | 3863 | 61 | Completed in 4m48s. Updated README and PR contract; documented global `--format` behaviour, interactive-command JSON behaviour, and runnable built-CLI examples. Verified lint/test/build and an `info --format json --full false` example. First pass accepted. No steering. |
| 06 Review-hardening | 3863 | 3747 | 116 | Completed in 9m07s. Added missing `greeting --format json` handler-level coverage, updated PR contract and memory, hardened Husky hooks to use `corepack pnpm`, and validated lint/test/build/CodeQL. First pass accepted with scope-expansion observation. No steering. |

## Phase outcomes

| Phase | Outcome | Tests | Build | Lint | Human interventions | Review defects | Accepted |
|---|---|---|---|---|---:|---:|---|no
| 00 Hydration | Completed | Pass | Pass | Pass | 0 | 0 | yes |
| 01 CLI feature | Completed | Pass | Pass | Pass | 0 | 0 | yes, with observations |
| 02 Validation bug | Completed | Pass | Pass | Pass | 0 | 0 | yes, with observations |
| 03 Tests | Completed | Pass | Pass | Pass | 0 | 0 | yes, with observations |
| 04 Refactor | Completed | Pass | Pass | Pass | 0 | 0 | yes |
| 05 Docs | Completed | Pass | Pass | Pass | 0 | 0 | yes |
| 06 Review-hardening | Completed | Pass | Pass | Pass | 0 | 0 | yes, with observations |

## Human steering log

| Phase | Intervention | Reason | Impact |
|---|---|---|---|
| 00 Hydration | None | First-pass evaluation only | No steering applied |
| 01 CLI feature | None | First-pass evaluation only | No steering applied |
| 02 Validation bug | None | First-pass evaluation only | No steering applied |
| 03 Tests | None | First-pass evaluation only | No steering applied |
| 04 Refactor | None | First-pass evaluation only | No steering applied |
| 05 Docs | None | First-pass evaluation only | No steering applied |
| 06 Review-hardening | None | First-pass evaluation only | No steering applied |

## Review notes

### Phase 00 review notes

GPT-5.4 correctly limited changes to AADLC artefacts and did not modify application source or benchmark prompts.

The model inspected the benchmark prompt, governance artefacts, package configuration, source structure, tests, README, CLI entrypoints, Husky configuration, and validation setup. It updated `memory.md`, `trust-boundaries.md`, and `invariants.yml`.

Unlike the Sonnet 4.6 hydration run, GPT-5.4 performed a fuller baseline validation attempt during hydration, including lint, test, build, and Prettier checks. It identified that repository-wide formatting was not fully clean due to pre-existing issues outside the changed artefacts, while changed artefact formatting passed.

The model also recorded a benchmark integrity issue: the hydration prompt referenced `benchmark/playbook.md`, while the checked-in file appears to be `benchmark/benchmark.md`. This is a useful scaffold finding and should be considered for a future seed correction, but it should not be corrected mid-run unless all benchmark repos are reset to a new baseline.

No human steering was applied.

Observation: GPT-5.4 used runtime memory storage calls to persist validated command-set and CLI entrypoint facts. This is outside the repository artefact diff but may indicate stronger use of external durable-agent memory where the runtime supports it. For benchmark integrity, repository files remain the authoritative evidence.

### Phase 01 review notes

GPT-5.4 implemented `--format text|json` as a global CLI option rather than scoping it to the `info` command. It added shared structured-output support in `src/output.ts` and updated `info`, `greeting`, and `create` so commands preserve default text behaviour while supporting JSON output.

The model updated README documentation, updated the PR contract, added tests for output formatting and JSON command behaviour, and ran lint, test, and build validation successfully.

No human steering was applied.

Observation: this is a broader interpretation of the phase than Sonnet 4.6. The prompt allowed natural CLI integration, so global formatting is defensible, but it increased implementation scope, changed multiple commands, and consumed significantly more credits.

Observation: the model added `pnpm-workspace.yaml` to support validation in the benchmark environment. This may be pragmatic, but it is a tooling change outside the feature scope and should be compared against other model runs.

Observation: GPT-5.4 performed repeated validation, review, patching, and polish loops within the same phase. This improved confidence but materially increased credit consumption and elapsed time.

Observation: runtime memory voting was invoked during the run. Repository files remain the authoritative benchmark evidence.

### Phase 02 review notes

GPT-5.4 identified that the Phase 01 global `--format` implementation still had validation gaps. `--format xml` failed, but a bare `--format` value could be accepted and silently fall back to text output.

The fix centralised format validation in `src/output.ts`, required an explicit string argument in `bin/run.ts`, and added or updated tests covering non-string values, unsupported values, missing parser arguments, and prevention of silent fallback.

The model ran lint, test, and build validation successfully and addressed review feedback through multiple internal patch/validation loops.

No human steering was applied.

Observation: because GPT-5.4 implemented `--format` as a global CLI option in Phase 01, Phase 02 necessarily had a broader validation surface than the Sonnet 4.6 implementation. This increased credit consumption and elapsed time, but produced a more comprehensive validation layer for the chosen architecture.

Observation: GPT-5.4 again performed repeated validation and review-fix loops within the phase. This increased confidence but materially raised credit cost.

Observation: runtime memory voting was invoked during the run. Repository files remain the authoritative benchmark evidence.

### Phase 03 review notes

GPT-5.4 strengthened automated test coverage without intentionally changing production behaviour.

The model identified missing or weak coverage around the CLI bootstrap in `bin/run.ts`, `greeting` text/JSON flows, `info` text/full-mode behaviour, and `create` text-mode success/path coercion edge cases.

The run added or updated tests in:

- `bin/run.test.ts`
- `src/commands/create.test.ts`
- `src/commands/greeting.test.ts`
- `src/commands/info.test.ts`

Validation passed via `corepack pnpm lint`, `corepack pnpm test`, and `corepack pnpm build`.

No human steering was applied.

Observation: because GPT-5.4 implemented global `--format` support in Phase 01, the test scope in Phase 03 was wider than the Sonnet 4.6 run and included bootstrap/global CLI behaviour.

Observation: the model performed multiple validation and review-fix loops, which increased confidence but also increased credit consumption.

Observation: `engine-tools-report_progress` could not commit/push because the Husky pre-commit hook could not find `pnpm` in `PATH`. This did not prevent the measured implementation from being produced, but should be recorded as a tool-surface/environment issue.

Observation: final non-blocking review suggestions remained, including a `jest.restoreAllMocks()` redundancy and a helper-refactor suggestion that would have exceeded the test-focused task scope.

Observation: runtime memory voting was invoked again. Repository files remain the authoritative benchmark evidence.

### Phase 04 review notes

GPT-5.4 performed a focused behaviour-preserving refactor of the centralised output formatting helper.

The model identified that `writeFormattedOutput` in `src/output.ts` handled validation, JSON serialisation, text rendering, and format dispatch through inline branching. It refactored this into dedicated formatter objects with `writeFormattedOutput` acting as a small dispatcher.

This preserved the existing global `--format text|json` design from Phase 01 while reducing coupling between format selection and rendering behaviour.

Validation passed via `corepack pnpm lint`, `corepack pnpm test`, `corepack pnpm build`, and parallel validation with Code Review and CodeQL.

No human steering was applied.

Observation: this phase was cheaper and faster than Sonnet 4.6 Phase 04, likely because GPT-5.4 had already centralised output behaviour in Phase 01. This suggests its broader upfront architecture increased earlier implementation/test costs but reduced later refactor cost.

Observation: runtime memory voting was invoked again. Repository files remain the authoritative benchmark evidence.

### Phase 05 review notes

GPT-5.4 performed a documentation-focused update covering the broader global output-format behaviour introduced in Phase 01.

The model updated `README.md` with runnable built-CLI examples using `corepack pnpm build` and `corepack pnpm start:node`, documented current `--format text|json` behaviour, and clarified JSON behaviour for interactive commands.

It also updated `.github/aadlc/current-pr-contract.md` to align with the Phase 05 documentation task.

Validation included `corepack pnpm lint`, `corepack pnpm test`, `corepack pnpm build`, and a live example command: `corepack pnpm start:node info --format json --full false`.

No human steering was applied.

Observation: documentation scope was broader than Sonnet 4.6 because GPT-5.4 implemented global format support rather than limiting `--format` to `info`.

Observation: the model validated a documented example command, which is stronger docs validation than the Sonnet 4.6 docs phase.

Observation: runtime memory voting and storage were invoked. Repository files remain the authoritative benchmark evidence.

### Phase 06 review notes

GPT-5.4 completed review-hardening by auditing the benchmark work against the PR contract, memory, invariants, trust boundaries, and acceptance criteria.

The model identified missing handler-level coverage for `greeting --format json`, including the interactive option-object path, and added focused tests. It updated `.github/aadlc/current-pr-contract.md` to reflect Phase 06 findings and updated `.github/aadlc/memory.md` to match the current repository state.

The model also hardened Husky hooks to use `corepack pnpm` and aligned commit validation with the documented workflow.

Validation passed via `corepack pnpm lint`, `corepack pnpm test`, `corepack pnpm build`, and parallel validation with Code Review and CodeQL.

No human steering was applied.

Observation: Phase 06 produced more substantive changes than the Sonnet 4.6 review-hardening pass. The additional `greeting` JSON-flow coverage is directly related to GPT-5.4’s broader global-format architecture, but Husky hook hardening is a tooling/workflow improvement adjacent to the benchmark task rather than core feature hardening.

Observation: coverage remains unit-level; full end-to-end CLI entrypoint coverage remains open.

Observation: ESLint still emits the existing TypeScript support warning, but validation passes.

Observation: runtime memory voting and storage were invoked again. Repository files remain the authoritative benchmark evidence.

## Final verdict

GPT-5.4 completed all benchmark phases successfully without human steering. It consistently favoured broader architectural solutions, deeper validation, and stronger global consistency over narrowly scoped implementation. Throughout the benchmark it repeatedly expanded its own validation surface, revisited assumptions, incorporated review feedback, and hardened supporting infrastructure.

The result was a higher-quality and more globally coherent implementation than the minimum required by the prompts, but at a materially higher credit and time cost than Sonnet 4.6.

### Strengths

- Zero human steering across all phases.
- Strong contract compliance and acceptance rate (7/7 phases accepted).
- Consistently performed build, lint, and test validation.
- Treated output formatting as a cross-cutting concern rather than a command-specific feature.
- Added broader test coverage, including CLI bootstrap behaviour and JSON flows.
- Responded well to review feedback and repeatedly self-corrected.
- Produced a coherent architecture centred around a shared output abstraction.
- Good at identifying secondary defects and validation gaps introduced by its own design decisions.
- Review-hardening phase found genuine additional coverage opportunities rather than merely documenting findings.

### Weaknesses

- Significantly higher credit consumption (+60.5% versus Sonnet 4.6).
- Significantly longer execution time (+68.2% versus Sonnet 4.6).
- Frequently expanded scope beyond the narrowest interpretation of the task.
- Repeated validation/review loops consumed substantial credits.
- Made tooling and workflow changes adjacent to the requested work (e.g. Husky hook hardening, pnpm workspace adjustments).
- Architectural choices in early phases increased downstream implementation, testing, documentation, and review costs.
- Appears biased towards "solve the class of problem" rather than "solve the specific benchmark task."

### Cost/quality assessment

The benchmark demonstrates that GPT-5.4 is not optimising for cheapest accepted work.

Instead it appears to optimise for:

1. Architectural consistency.
2. Validation confidence.
3. Future extensibility.
4. Review resilience.

The cost of that behaviour was measurable:

| Metric | Sonnet 4.6 | GPT-5.4 |
|----------|----------:|----------:|
| Credits | 441 | 708 |
| Time | 34m58s | 58m48s |
| Steering | 0 | 0 |
| Accepted phases | 7/7 | 7/7 |

GPT-5.4 consumed approximately 61% more credits and 68% more elapsed time while achieving the same benchmark acceptance outcome.

However, it also delivered a broader solution:

- Sonnet implemented output formatting primarily around the `info` command.
- GPT-5.4 implemented output formatting as a global CLI capability.

Whether the additional architecture justifies the additional cost depends entirely on the desired outcome.

For narrowly scoped feature delivery, the benchmark favours Sonnet 4.6.

For architectural consistency and defensive engineering, GPT-5.4 demonstrates clear value, albeit at a premium.

### Would use again for this task class?

**Yes, but selectively.**

I would use GPT-5.4 for:

- Architectural work.
- Framework design.
- Refactoring programmes.
- Governance-heavy changes.
- Trust-boundary-sensitive features.
- Validation-heavy implementation.
- Situations where long-term consistency matters more than immediate cost.

I would be less inclined to use GPT-5.4 for:

- Small feature additions.
- Straightforward bug fixes.
- Documentation-only tasks.
- Cost-sensitive bulk implementation work.

For this benchmark specifically, Sonnet 4.6 delivered the better cost-to-accepted-work ratio.

GPT-5.4 delivered the broader and arguably more "engineered" solution.

A useful summary might be:

> Sonnet 4.6 behaved like a pragmatic senior engineer focused on delivering the ticket. GPT-5.4 behaved like an architect who happened to be holding the keyboard.