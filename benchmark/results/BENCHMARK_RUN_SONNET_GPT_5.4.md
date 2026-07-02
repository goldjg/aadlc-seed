# Benchmark Run

## Run metadata

| Field | Value |
|---|---|
| Repository | https://github.com/goldjg/aadlc-seed-sonnet-plan-gpt-5-4-exec |
| Planner model | Claude Sonnet 4.6 |
| Planner mode | |
| Executor model | GPT-5.4 |
| Executor mode | |
| Run date | 2026-04-05|
| Operator | Graham Gold |
| Seed commit | https://github.com/goldjg/aadlc-seed/commit/7c35caa2c09d4c2e4e88180bf9dff013dcc1c0dc |
| Measured branch | benchmark/run-sonnet-plan-gpt-5-4-exec |
| Copilot/Coding agent surface | Web UI |
| Workflow | Sonnet planning, GPT-5.4 execution |
| Additional notes | |

## Credit ledger

| Phase | Planner start | Planner end | Planner delta | Executor start | Executor end | Executor delta | Total delta | Notes |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| 00 Hydration | 3747 | 3695 | 52 | N/A | N/A | N/A | 52 | Sonnet performs hydration directly. Created AADLC artefacts only; no application source changes. |
| 01 CLI feature | 3695 | 3667 | 28 | 3667 | 3584 | 83 | 111 | Sonnet produced `.github/aadlc/plans/phase-01-cli-feature.plan.md`; GPT-5.4 executed it within approved scope. Implementation changed only `src/commands/info.ts` and `src/commands/info.test.ts`; no global CLI formatting, no `greeting`/`create` changes, no config/tooling changes. |
| 02 Validation bug | 3584 | 3555 | 29 | 3555 | 3523 | 22 | 51 | Sonnet produced `.github/aadlc/plans/phase-02-validation-bug.plan.md`; GPT-5.4 executed it within approved scope. Changed only `src/commands/info.ts` and `src/commands/info.test.ts`; added handler-level invalid-format guard and direct handler test. |
| 03 Tests | 3523 | 3491 | 32 | 3491 | 3447 | 44 | 76 | Sonnet produced `.github/aadlc/plans/phase-03-tests.plan.md`; GPT-5.4 executed the test-only plan. Changed only `greeting.test.ts`, `create.test.ts`, `info.test.ts`, and `src/index.test.ts`; no production source/config changes. |
| 04 Refactor | 3447 | 3415 | 32 | 3415 | 3363 | 52 | 84 | Sonnet produced `.github/aadlc/plans/phase-04-refactor.plan.md`; GPT-5.4 executed the refactor within approved scope. Changed only `src/commands/info.ts` and new `src/formatters/info.ts`; no tests, config, benchmark prompts, or unrelated source files changed. |
| 05 Docs | 3363 | 3326 | 37 | 3326 | 3284 | 42 | 79 | Sonnet produced `.github/aadlc/plans/phase-05-docs.plan.md`; GPT-5.4 executed it. Changed `README.md`, `.github/aadlc/memory.md`, and `.github/aadlc/invariants.yml`. Accepted, but scope expanded beyond README-only documentation, so comparability with single-model Phase 05 is limited. |
| 06 Review-hardening | 3284 | 3254 | 30 | 3254 | 3156 | 98 | 128 | Sonnet produced `.github/aadlc/plans/phase-06-review-hardening.plan.md`; GPT-5.4 executed review-hardening with minimal confirmed fixes. Changed `create.ts`, `create.test.ts`, `current-pr-contract.md`, and added `phase-06-findings.md`. |

## Phase outcomes

| Phase | Planner outcome | Executor outcome | Tests | Build | Lint | Human interventions | Review defects | Accepted |
|---|---|---|---|---|---|---:|---:|---|
| 00 Hydration | Completed | N/A | Not run | Not run | Not run | 0 | 0 | yes |
| 01 CLI feature | Plan created | Completed | Pass | Pass | Lint fails pre-existing/no ESLint config | 0 | 0 | yes |
| 02 Validation bug | yes | yes | None | GPT-5.4 followed Sonnet's scoped plan. No changes to `greeting`, `create`, shared utilities, config, benchmark prompts, or unrelated application areas. |
| 03 Tests | Plan created | Completed | Pass | Pass | Lint failed pre-existing/no ESLint config | 0 | 0 | yes |
| 04 Refactor | Plan created | Completed | Pass | Pass | Lint failed pre-existing/no ESLint config | 0 | 0 | yes |
| 05 Docs | Plan created | Completed | Pass | Pass | Lint failed pre-existing/no ESLint config | 0 | 0 | yes, with comparability caveat |
| 06 Review-hardening | Plan created | Completed | Pass | Pass | Lint failed pre-existing/no ESLint config | 0 | 1 confirmed behaviour/docs mismatch fixed | yes |

## Plan adherence log

| Phase | Plan created | Plan followed | Deviations | Notes |
|---|---|---|---|---|
| 00 Hydration | N/A | N/A | N/A | Sonnet hydration run; no executor phase |
| 01 CLI feature | yes | yes | None | GPT-5.4 followed Sonnet's scoped plan. It implemented `--format text|json` for `info` only and did not expand into global formatting. |
| 02 Validation bug | yes | yes | None | GPT-5.4 followed Sonnet's scoped plan. No changes to `greeting`, `create`, shared utilities, config, benchmark prompts, or unrelated application areas. |
| 03 Tests | yes | yes | Minor test-design deviation | GPT-5.4 followed the test-only plan. For create builder coercion coverage, it used `--path` input instead of bare positional input because isolated builder tests do not bind positional arguments without full command registration. No production code was changed. |
| 04 Refactor | yes | yes | None | GPT-5.4 followed the approved plan. It modified only `src/commands/info.ts` and created `src/formatters/info.ts`; generated `dist/` changes from compile were reverted. |
| 05 Docs | yes | yes | None | GPT-5.4 followed the approved plan exactly. The plan itself expanded docs scope into AADLC memory/invariants, so the phase is accepted but not strictly apples-to-apples with prior Phase 05 runs. |
| 06 Review-hardening | yes | yes | None | GPT-5.4 followed the approved plan. It created the findings log, updated stale PR contract state, fixed the confirmed `create [path]` mismatch, updated tests, and did not introduce new features, dependencies, or opportunistic refactoring. |

## Human steering log

| Phase | Intervention | Reason | Impact |
|---|---|---|---|
| 00 Hydration | None | First-pass evaluation only | No steering applied |
| 01 CLI feature | None | First-pass planner/executor evaluation only | No steering applied |
| 02 Validation bug | None | First-pass planner/executor evaluation only | No steering applied |
| 03 Tests planning | None | First-pass evaluation only | No steering applied |
| 03 Tests | None | First-pass planner/executor evaluation only | No steering applied |


## Review notes

### Phase 00 review notes

Sonnet 4.6 completed the hydration phase directly for the planner/executor benchmark repo.

The model created the expected AADLC artefacts: `.github/copilot-instructions.md`, `.github/aadlc/memory.md`, `.github/aadlc/trust-boundaries.md`, `.github/aadlc/invariants.yml`, and `.github/aadlc/current-pr-contract.md`.

The artefacts captured repository stack, source layout, command contract, validation expectations, trust boundaries, and durable invariants. No application source code was modified.

No human steering was applied.

Observation: Phase 00 consumed 52 credits, lower than both the Sonnet-only hydration run (69 credits) and the GPT-5.4 hydration run (92 credits). This may reflect run variance, but it establishes a lower-cost hydration baseline for the planner/executor workflow.

### Phase 01 review notes

Sonnet 4.6 produced a dedicated execution plan for GPT-5.4 in `.github/aadlc/plans/phase-01-cli-feature.plan.md`.

The plan deliberately constrained the task to the `info` command and explicitly ruled out global CLI formatting, changes to `greeting`, changes to `create`, shared output utilities, package-manager/build/Husky/workspace configuration changes, benchmark prompt changes, and unrelated AADLC artefact changes.

GPT-5.4 executed the plan successfully. The implementation added `--format text|json` to `info`, preserved default text output, wrote JSON directly to stdout for `--format json`, and added tests for text output, JSON output, and invalid format rejection.

The executor changed only `src/commands/info.ts` and `src/commands/info.test.ts`.

Validation results:

- `corepack pnpm compile` passed.
- `corepack pnpm test` passed.
- Manual smoke tests passed for default text output, `--format text`, `--format json`, `--format json --full`, and invalid `--format xml` non-zero exit.
- `corepack pnpm lint` failed before and after changes because the repository has no ESLint config file.
- `parallel_validation` timed out/cancelled and was not rerun.

No human steering was applied.

Observation: the Sonnet plan successfully constrained GPT-5.4 to the intended `info`-scoped implementation. This avoided the global CLI output-formatting architecture that GPT-5.4 produced in its single-model run.

Observation: combined Phase 01 planner/executor cost was 111 credits, compared with 187 credits for GPT-5.4 alone. The 28-credit Sonnet planning cost appears to have reduced GPT execution cost and scope expansion materially.

Observation: the combined run was only 10 credits more expensive than Sonnet 4.6 alone for Phase 01 while retaining GPT-5.4 as the implementation model.

### Phase 02 review notes

Sonnet 4.6 produced a dedicated execution plan for GPT-5.4 in `.github/aadlc/plans/phase-02-validation-bug.plan.md`.

The plan identified the remaining validation gap after Phase 01: yargs rejects invalid CLI values through `choices`, but direct/programmatic calls to the `info` handler could still bypass parser-level validation. The plan constrained the fix to `src/commands/info.ts` and `src/commands/info.test.ts`.

GPT-5.4 executed the plan successfully. It added a handler-level guard for invalid `format` values and added a direct handler test for the invalid-format path.

The executor changed only `src/commands/info.ts` and `src/commands/info.test.ts`.

Validation results:

- `corepack pnpm test` passed.
- `corepack pnpm compile` passed.
- `corepack pnpm lint` failed before and after changes because the repository has no ESLint config file.
- `parallel_validation` passed: code review found no issues and CodeQL found no alerts.
- Generated `dist/` changes from compile were reverted so only planned source/test files remained changed.

No human steering was applied.

Observation: GPT-5.4 followed the Sonnet plan without scope expansion. It did not modify `greeting`, `create`, shared utilities, configuration, benchmark prompts, or unrelated application areas.

Observation: combined Phase 02 planner/executor cost was 51 credits, compared with 105 credits for GPT-5.4 alone. The 29-credit planning cost appears to have reduced GPT execution cost and prevented global validation-surface expansion.

Observation: GPT-5.4 executor-only cost was 22 credits, materially lower than both GPT-5.4 direct Phase 02 execution (105 credits) and Sonnet 4.6 direct Phase 02 execution (45 credits). This suggests GPT-5.4 can be highly efficient when execution ambiguity is removed.

### Phase 03 review notes

Sonnet 4.6 produced `.github/aadlc/plans/phase-03-tests.plan.md` as a dedicated execution plan for GPT-5.4.

The plan interpreted Phase 03 as a focused test-only coverage improvement across CLI behaviour. It allowed creation of `src/commands/greeting.test.ts`, creation of `src/commands/create.test.ts`, updates to `src/commands/info.test.ts`, and replacement of placeholder coverage in `src/index.test.ts`. It explicitly forbade production source changes, configuration changes, dependency changes, AADLC artefact changes, benchmark prompt changes, E2E tests, coverage tooling changes, and unrelated refactoring.

GPT-5.4 executed the plan successfully. It added the planned test coverage and made no production code changes.

Validation results:

- `corepack pnpm test` passed.
- `corepack pnpm compile` passed.
- `corepack pnpm lint` failed because the repository has no ESLint config file; this was pre-existing and not introduced by the phase.

No human steering was applied.

Deviation: for `create` builder coercion coverage, the executor used `--path` input instead of bare positional input because `builder(...)` in isolation does not bind the positional argument without full command registration. This is an acceptable test-design deviation and did not require production changes.

Observation: the planner/executor Phase 03 total was 76 credits across 7m12s, compared with 77 credits across 7m16s for Sonnet 4.6 alone and 111 credits across 8m42s for GPT-5.4 alone. The workflow achieved Sonnet-level cost and runtime while retaining GPT-5.4 as executor.

Observation: GPT-5.4 execution cost was 44 credits once the test scope and non-goals were pre-bounded by Sonnet.

### Phase 04 review notes

Sonnet 4.6 produced `.github/aadlc/plans/phase-04-refactor.plan.md` as a dedicated execution plan for GPT-5.4.

The plan constrained the refactor to extracting `info` command output formatting from `src/commands/info.ts` into `src/formatters/info.ts`. It explicitly allowed only two file changes: modifying `src/commands/info.ts` and creating `src/formatters/info.ts`.

GPT-5.4 executed the approved plan successfully. It extracted text and JSON output formatting into the dedicated formatter module, kept `src/commands/info.ts` focused on validation, data collection, and delegation, and preserved existing `--full`, `--format text`, and `--format json` behaviour.

Validation results:

- `corepack pnpm compile` passed.
- `corepack pnpm test` passed.
- `corepack pnpm lint` failed before and after the change because the repository has no ESLint config file in the workspace.
- Generated `dist/` artefacts from compile were reverted and not included in the final change set.

No human steering was applied.

Observation: GPT-5.4 followed the plan without scope expansion. It did not modify tests, other commands, configuration, dependencies, AADLC artefacts, or benchmark prompts.

Observation: combined Phase 04 planner/executor cost was 84 credits, compared with 50 credits for Sonnet 4.6 alone and 36 credits for GPT-5.4 alone. This phase is more expensive in the planner/executor workflow because the Sonnet plan deliberately constrained GPT away from the global output abstraction it had created in the GPT-only run.

Observation: this is a useful counterexample to the earlier phases: planning improved scope control, but the added planning cost did not reduce total phase cost for a small, already-clear refactor.

### Phase 05 review notes

Sonnet 4.6 produced `.github/aadlc/plans/phase-05-docs.plan.md` as a dedicated execution plan for GPT-5.4.

The plan identified documentation gaps in `README.md` and also included AADLC artefact alignment for `.github/aadlc/memory.md` and `.github/aadlc/invariants.yml`, reflecting the formatter module introduced in Phase 04.

GPT-5.4 executed the approved plan successfully. It updated `README.md`, `.github/aadlc/memory.md`, and `.github/aadlc/invariants.yml`. No source, test, benchmark, or configuration files remained changed in the final diff.

Validation results:

- `corepack pnpm compile` passed.
- `corepack pnpm test` passed.
- Optional spot-checks passed for:
  - `corepack pnpm start info --format json --full`
  - `corepack pnpm start info --format json --no-full`
  - `corepack pnpm start create --help`
- `parallel_validation` passed; CodeQL skipped as trivial docs-only change.
- `corepack pnpm lint` failed because the repository has no ESLint config; this was pre-existing and not introduced by the phase.

No human steering was applied.

Observation: GPT-5.4 followed the Sonnet plan without executor drift.

Observation: the Sonnet plan expanded Phase 05 scope beyond the original README-focused documentation intent by including AADLC memory and invariant updates. This is defensible from an AADLC-governance perspective, but it reduces direct comparability with the Sonnet-only and GPT-only Phase 05 results.

Observation: combined Phase 05 planner/executor cost was 79 credits, compared with 39 credits for Sonnet 4.6 alone and 61 credits for GPT-5.4 alone. The higher cost reflects governance-complete documentation rather than a like-for-like README-only task.

### Phase 06 review notes

Sonnet 4.6 produced `.github/aadlc/plans/phase-06-review-hardening.plan.md` as a dedicated execution plan for GPT-5.4.

The plan scoped Phase 06 as a minimal review-hardening pass over phases 01–05. It required findings to be recorded, classified, and addressed only where a confirmed issue justified a targeted fix.

GPT-5.4 executed the plan successfully. It created `.github/aadlc/plans/phase-06-findings.md`, updated `.github/aadlc/current-pr-contract.md` to reflect Phase 06, and fixed a confirmed behaviour/documentation mismatch in the `create` command.

The main fix changed `create` from requiring a positional path to accepting an optional path via `create [path]`, aligning runtime behaviour with the documented default path. Tests were updated to validate real positional parsing.

Validation results:

- `npm run compile` passed.
- `npm run test` passed.
- `npm run lint` failed because ESLint cannot find a repository config file; this was pre-existing and left unchanged because fixing ESLint configuration would exceed the approved Phase 06 scope.
- `parallel_validation` passed.
- Code review found no issues.
- CodeQL found no alerts.

No human steering was applied.

Observation: GPT-5.4 followed the approved plan and made a targeted fix for a confirmed review finding rather than broadening into unrelated improvement work.

Observation: Phase 06 consumed 128 combined credits, compared with 60 credits for Sonnet 4.6 alone and 116 credits for GPT-5.4 alone. The planner/executor workflow was more expensive here because it included both planning overhead and a substantive behaviour/docs consistency fix.

Observation: the `create [path]` fix means the final planner/executor result is not strictly identical in scope to the earlier single-model runs. It is arguably higher quality, but direct cost comparison should carry a scope caveat.

## Comparative metrics

| Metric | Planner (Sonnet 4.6) | Executor (GPT-5.4) | Combined |
|---|---:|---:|---:|
| Credits consumed | 240 | 341 | 581 |
| Elapsed time | 17m55s | 30m23s | 48m18s |
| Human interventions | 0 | 0 | 0 |
| Accepted phases | 7/7 | 6/6 | 7/7 |
| Review defects | N/A | 1 confirmed behaviour/docs mismatch fixed | 1 |

## Key observations

### Planning effectiveness

Sonnet planning materially reduced GPT-5.4’s ambiguity-driven scope expansion in the implementation-heavy phases. The clearest examples were Phase 01 and Phase 02, where GPT-5.4 stayed scoped to the `info` command instead of building a global CLI formatting architecture.

### Plan adherence

GPT-5.4 followed the approved Sonnet plans consistently. Across executor phases, there was no material scope drift, no unapproved architecture expansion, and no human steering required.

### Scope control

The planner/executor workflow was strongest where ambiguity was high. It constrained GPT-5.4 effectively in feature implementation, validation hardening, and test expansion. Later phases showed that planning can also expand scope if the plan itself broadens the task, as seen in Phase 05 documentation and Phase 06 review-hardening.

### Architectural cost propagation

The run demonstrated that architecture decisions affect downstream cost. GPT-5.4 alone spent heavily after choosing a global output-formatting architecture. In the planner/executor run, Sonnet constrained the architecture up front, reducing downstream implementation and validation cost in earlier phases.

### Validation behaviour

GPT-5.4 remained validation-heavy, but the Sonnet plans made that validation more targeted. Lint failures were consistently pre-existing due to missing ESLint configuration and were correctly left unresolved as out of scope.

## Final verdict

The Sonnet planner / GPT-5.4 executor workflow completed all phases successfully with zero human steering and strong plan adherence.

It was not cheaper than Sonnet 4.6 alone, but it was substantially cheaper and more controlled than GPT-5.4 alone. The strongest result is that GPT-5.4 became much more cost-efficient when Sonnet removed ambiguity and converted the work into precise execution contracts.

### Strengths

- Zero human steering across all phases.
- Strong plan adherence from GPT-5.4.
- Materially cheaper than GPT-5.4 alone.
- Avoided GPT-5.4’s earlier global architecture expansion.
- Produced scoped, reviewable changes.
- Demonstrated that planning can reduce executor ambiguity and downstream cost.
- Phase 06 found and fixed a real behaviour/documentation mismatch.

### Weaknesses

- More expensive than Sonnet 4.6 alone.
- Planning overhead was not always recovered.
- Phase 05 and Phase 06 expanded beyond strict comparability with the single-model runs.
- Planner quality directly shaped task scope, for better and worse.
- Some phases became governance-complete rather than strictly like-for-like.

### Cost/quality assessment

| Run | Credits | Time | Steering | Accepted phases |
|---|---:|---:|---:|---:|
| Sonnet 4.6 only | 441 | 34m58s | 0 | 7/7 |
| GPT-5.4 only | 708 | 58m48s | 0 | 7/7 |
| Sonnet plan + GPT-5.4 exec | 581 | 48m18s | 0 | 7/7 |

The planner/executor workflow used 127 fewer credits than GPT-5.4 alone, but 140 more credits than Sonnet 4.6 alone.

The key economic finding is not that planner/executor is universally cheaper. It is cheaper where ambiguity would otherwise cause architectural expansion.

### Would use again for this task class?

Yes, selectively.

Use Sonnet planning with GPT-5.4 execution when the task has ambiguity, architectural risk, trust-boundary sensitivity, or a high chance of scope expansion.

For small, already well-bounded implementation tasks, Sonnet alone currently appears to provide the best cost-to-accepted-work ratio.

## Comparison with single-model runs

### Versus Sonnet 4.6

Sonnet 4.6 alone remained the cheapest and fastest complete run. It delivered focused, pragmatic, accepted work with zero steering.

The planner/executor workflow did not beat Sonnet on cost, but it produced a stronger demonstration of controlled multi-model orchestration.

### Versus GPT-5.4

The planner/executor workflow significantly outperformed GPT-5.4 alone on cost and scope control.

GPT-5.4 alone tended to convert ambiguity into architecture. GPT-5.4 executing Sonnet plans stayed bounded and materially cheaper.

### Overall ranking

1. Sonnet 4.6 only -- best cost-to-accepted-work ratio.
2. Sonnet plan + GPT-5.4 exec -- best controlled GPT-5.4 workflow; strong scope control.
3. GPT-5.4 only -- broadest architecture and strongest validation posture, but highest cost.

## Conclusion

> Did Sonnet planning reduce GPT-5.4 implementation cost, scope expansion, and review effort while preserving quality?

**Answer: Yes, compared with GPT-5.4 alone.**

Sonnet planning reduced GPT-5.4’s scope expansion and overall credit consumption while preserving quality and requiring zero human steering. However, the workflow did not beat Sonnet 4.6 alone on cost. The result suggests planner/executor orchestration is valuable when ambiguity is expensive, but not automatically cheaper for every task.