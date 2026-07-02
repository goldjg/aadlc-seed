# Benchmark Run

## Run metadata

| Field | Value |
|---|---|
| Repository | https://github.com/goldjg/aadlc-seed-sonnet-4-6 |
| Model | Claude Sonnet 4.6 |
| Model mode | |
| Run date | 03-06-26 |
| Operator | Graham Gold |
| Seed commit | https://github.com/goldjg/aadlc-seed/commit/7c35caa2c09d4c2e4e88180bf9dff013dcc1c0dc |
| Measured branch | benchmark/run-sonnet-4-6 |
| Copilot/Coding agent surface | Web UI |
| Additional notes | First-pass evaluation only. No human steering applied during measured phases. |

## Credit ledger

| Phase | Start credits | End credits | Delta | Notes |
|---|---:|---:|---:|---|
| 00 Hydration | 4896 | 4827 | 69 | Completed in 3m47s. No application source changes. AADLC artefacts updated only. Dependencies installed after initial note that validation was not run; validation commands were not executed because phase was documentation-only. |
| 01 CLI feature | 4827 | 4726 | 101 | Completed in 7m29s. Added `--format text|json` to `info`; opened PR #2. First pass accepted. No steering. |
| 02 Validation bug | 4726 | 4681 | 45 | Completed in 5m10s. Added handler-level validation for invalid `format` values and tests for direct/programmatic handler calls. First pass accepted. No steering. |
| 03 Tests | 4681 | 4604 | 77 | Completed in 7m16s. Expanded CLI test coverage from 10 to 33 tests across 4 suites. Added tests for `info`, `create`, `greeting`, and command exports. First pass accepted with scope-breadth observation. No steering. |
| 04 Refactor | 4604 | 4554 | 50 | Completed in 4m45s. Extracted `info` output formatting into `src/commands/info.formatter.ts`; behaviour preserved and tests pass. First pass accepted. No steering. |
| 05 Docs | 4554 | 4515 | 39 | Completed in 2m20s. Updated README usage examples for `info`, `greeting`, and `create`; documented JSON output shape and `--no-full`. First pass accepted. No steering. |
| 06 Review-hardening | 4515 | 4455 | 60 | Completed in 4m11s. Performed review-hardening, fixed one Prettier/lint issue in `src/index.test.ts`, documented review findings, and confirmed tests/lint clean. First pass accepted. No steering. |

## Phase outcomes

| Phase | Outcome | Tests | Build | Lint | Human interventions | Review defects | Accepted |
|---|---|---|---|---|---:|---:|---|
| 00 Hydration | Completed | Not run | Not run | Not run | 0 | 0 | yes |
| 01 CLI feature | Completed | Pass: 7/7 | Pass | Pass | 0 | 0 | yes |
| 02 Validation bug | Completed | Pass: 10/10 | Not reported | Not reported | 0 | 0 | yes |
| 03 Tests | Completed | Pass: 33/33 | Not reported | Not reported | 0 | 0 | yes |
| 04 Refactor | Completed | Pass | Not reported | Not reported | 0 | 0 | yes |
| 05 Docs | Completed | Not run | Not run | Not run | 0 | 0 | yes |
| 06 Review-hardening | Completed | Pass: 33/33 | Not reported | Pass | 0 | 1 low-severity hygiene issue fixed | yes |

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

Sonnet 4.6 correctly limited changes to AADLC artefacts and populated memory, invariants, and trust boundaries with repository-specific truths discovered from inspection.

Useful additions included CLI command inventory, validation command inventory, benchmark prompt immutability, CLI public interface stability, and runtime trust boundaries for argv, environment variables, and external template download via `giget`.

No application source files were changed.

Minor observation: the agent initially stated validation was not run because dependencies were not installed, then dependencies were installed later, but validation commands were still not run. This is acceptable for the hydration phase because no application behaviour changed, but should be recorded as a validation-behaviour signal.

### Phase 01 review notes

Sonnet 4.6 implemented the requested `--format text|json` behaviour on the `info` command with `text` as the default, preserving existing human-readable output. JSON mode writes structured output directly to stdout and tests cover JSON validity, required keys, `--full` / `--no-full`, text fallback, and builder option registration.

The model updated the PR contract before implementation, added focused tests, updated README documentation, and recorded the durable feature fact in `memory.md`.

All reported validation passed: build, compile, test, lint, format, and CodeQL.

No human steering was applied.

Observation: the implementation scoped `--format` to the `info` command rather than introducing a global CLI-wide output format. This appears acceptable for the phase because the existing CLI only has one naturally structured informational output command, but should be compared against other model interpretations.

Observation: yargs `.choices()` validation for invalid format values was implemented during Phase 01, which may partially consume the intended scope of Phase 02.

### Phase 02 review notes

Sonnet 4.6 correctly identified that Phase 01 had already added yargs `choices` validation for CLI usage, but that direct/programmatic calls to `handler()` could still supply invalid `format` values and silently fall through to text mode.

The fix added a handler-level `VALID_FORMATS` guard and focused tests covering invalid values, error-message content, and ensuring stdout is not written before validation failure.

This is a good example of defensive validation across trust boundaries: CLI parser validation exists, but the command handler no longer relies solely on upstream enforcement.

No human steering was applied.

Observation: validation scope remained focused and did not introduce unrelated refactoring.

### Phase 03 review notes

Sonnet 4.6 strengthened automated CLI coverage without changing production application behaviour.

The model identified several under-tested areas: `info` text-mode logging behaviour, `info` builder coverage for the `full` flag, lack of tests for `create`, lack of tests for `greeting`, and placeholder coverage in `src/index.test.ts`.

The test suite increased from 10 to 33 tests across 4 suites. New coverage includes command metadata, command registration, `info` logger behaviour, `create` path handling and template download outcomes, `greeting` prompt flow, and command export integrity.

No human steering was applied.

Observation: the phase prompt requested CLI behaviour coverage, so adding tests for `create` and `greeting` is defensible. However, this is broader than a narrow interpretation focused only on the new `info --format` feature. Record as a scope-breadth observation rather than a defect.

### Phase 04 review notes

Sonnet 4.6 performed a focused behaviour-preserving refactor of the `info` command output formatting.

The model identified that `info.ts` mixed routing, validation, process data collection, JSON serialisation, and coloured text rendering. It extracted output mechanics into `src/commands/info.formatter.ts` using `outputInfoAsJson()` and `outputInfoAsText()`, leaving `info.ts` focused on command definition, validation, and dispatch.

The refactor preserved the existing test surface and all tests passed without requiring test rewrites, which is a good signal that behaviour was maintained.

No human steering was applied.

Observation: the new formatter imports `OutputFormat` from `./info`, while `info.ts` imports formatter functions from `./info.formatter`. This is a potential type-only circular dependency shape. It may be harmless after TypeScript erasure, but it is worth comparing against other model implementations and reviewing during Phase 06.

### Phase 05 review notes

Sonnet 4.6 performed a documentation-only update focused on README usage coverage.

The update added concrete `pnpm start info` examples for default text output, JSON without process config, and JSON with full process config. It documented the JSON output shape, clarified that `processConfig` is conditional on `--full`, and added usage notes for `--no-full`.

The model also improved documentation for `greeting` by describing the interactive prompt flow, and for `create` by documenting the `<path>` positional argument, default value, relative-path behaviour, and an example invocation.

No application source changes were made.

No human steering was applied.

Observation: validation commands were not run, which is acceptable for a documentation-only phase if the examples are syntactically reasonable. If strict documentation validation is desired in future benchmark versions, add an explicit docs-example validation requirement.

### Phase 06 review notes

Sonnet 4.6 completed the review-hardening phase by performing a full validation pass across the benchmark implementation and associated artefacts.

The review identified a single actionable issue: a superfluous trailing blank line in `src/index.test.ts` that triggered a `prettier/prettier` ESLint failure. The issue was corrected and validation was rerun successfully.

The model also documented several non-actionable observations:

- Runtime `format` validation in `info.ts` is technically redundant because yargs `.choices()` already enforces valid values at parse time, but the additional guard provides harmless defence-in-depth for programmatic callers.
- `InfoArgv` exists in both `info.ts` and `info.formatter.ts`; this introduces minor duplication but does not currently justify additional abstraction.
- The `create` command trust boundary remains acceptable because path handling is normalised through yargs coercion before being passed to downstream functions.
- README documentation accurately reflects implemented behaviour and examples.
- No contract violations or acceptance-criteria gaps were identified.

The final implementation passed validation and required only a one-line hygiene correction.

No human steering was applied.

## Final verdict

Sonnet 4.6 completed the full AADLC phased benchmark run without human steering.

All seven phases were accepted first-pass. The model produced durable AADLC artefacts, implemented the `info --format text|json` feature, added handler-level validation, strengthened CLI test coverage, performed a focused formatter refactor, updated documentation, and completed a review-hardening pass.

Total measured consumption was 441 credits across 34m58s.

The run showed strong contract compliance, good validation behaviour, and disciplined phase boundaries. The main observations were that Phase 03 broadened test coverage beyond the immediate `info` feature, and Phase 04 introduced acceptable duplication of `InfoArgv` across `info.ts` and `info.formatter.ts`. Phase 06 correctly documented these as no-action findings and made only a minimal lint/prettier fix.

This run provides a strong baseline for comparing no-AADLC and other-model runs.

### Strengths

- Excellent contract compliance throughout the benchmark.
- Required no human steering across all seven phases.
- Consistently produced small, reviewable changes aligned with phase goals.
- Demonstrated good awareness of trust boundaries and validation requirements.
- Added meaningful automated test coverage rather than superficial assertions.
- Performed sensible defensive programming in Phase 02 by protecting programmatic callers rather than relying solely on CLI-layer validation.
- Review-hardening behaviour was disciplined and resisted unnecessary refactoring.
- Maintained documentation, tests, implementation, and governance artefacts in sync.

### Weaknesses

- Phase 03 broadened scope beyond the immediate feature area by adding tests for unrelated commands. While beneficial, it exceeded the narrowest interpretation of the task.
- Phase 04 introduced minor type duplication between `info.ts` and `info.formatter.ts`.
- Some validation reporting was inconsistent between phases, requiring review notes to reconstruct exactly what had been executed.
- The model occasionally favoured quality expansion over strict minimal-diff execution, which may increase cost in larger repositories.
- Hydration artefacts were updated effectively, but their direct influence on later phases cannot yet be isolated without a no-AADLC control run.

### Cost/quality assessment

This was a high-quality run with moderate credit consumption.

Total measured cost was 441 credits across 34m58s for a complete seven-phase workflow covering repository hydration, feature implementation, validation hardening, test expansion, refactoring, documentation updates, and final review-hardening.

The resulting implementation was accepted first-pass in every phase without human steering, suggesting a strong cost-to-validated-work ratio.

While some phases consumed credits on quality improvements beyond the minimum required implementation, those credits generally translated into tangible engineering value through additional tests, documentation, validation, and maintainability improvements.

The overall outcome suggests that structured phased delivery combined with governance artefacts can produce predictable and reviewable work without excessive credit consumption.

### Would use again for this task class?

Yes.

For small-to-medium engineering tasks requiring implementation, testing, documentation, and review-hardening under governance constraints, Claude Sonnet 4.6 performed strongly.

The model demonstrated reliable contract adherence, low supervision requirements, good validation behaviour, and a preference for producing production-quality artefacts rather than merely satisfying the minimum acceptance criteria.

Future comparison runs against GPT-5.4, MAI-Code-1-Flash, no-AADLC controls, and big-bang workflows will determine whether these strengths justify the measured credit consumption relative to competing approaches.
