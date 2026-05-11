---
stepsCompleted:
  - "step-01-init"
  - "step-02-discovery"
  - "step-02b-vision"
  - "step-02c-executive-summary"
  - "step-03-success"
  - "step-04-journeys"
  - "step-01b-continue"
  - "step-05-domain"
  - "step-06-innovation"
  - "step-07-project-type"
  - "step-08-scoping"
  - "step-09-functional"
  - "step-10-nonfunctional"
  - "step-11-polish"
  - "step-12-complete"
inputDocuments:
  - "_bmad-output/planning-artifacts/product-brief-Hexalith.AI.Project.md"
  - "_bmad-output/planning-artifacts/research/technical-codex-claude-code-plugins-skills-agents-commands-research-2026-04-26.md"
documentCounts:
  productBriefs: 1
  research: 1
  brainstorming: 0
  projectDocs: 0
classification:
  projectType: "developer_tool"
  domain: "ai-assisted software development / internal developer automation"
  complexity: "medium"
  projectContext: "greenfield"
workflowType: "prd"
releaseMode: "phased"
---

# Product Requirements Document - Hexalith.AI.Project

**Author:** Jerome
**Date:** 2026-04-26

## Executive Summary

Hexalith.AI.Tools is a versioned developer toolchain that automates BMAD delivery workflows inside Hexalith application repositories. It enables project leads and AI coding agents to advance validated work items through durable, resumable workflow steps with less manual orchestration and more reliable artifact quality.

The product addresses a specific failure mode in AI-assisted software delivery: agents can generate useful work, but humans still have to schedule each workflow step, pass the right context, preserve state, validate outputs, and decide what happens next. Hexalith.AI.Tools turns that repeated prompt choreography into a shared automation kernel with explicit workflow contracts, checkpoint state, deterministic validation, dry-run support, and structured escalation when human judgment is required.

The initial product focus is a narrow but complete BMAD state transition: generating a valid story from existing planning context, recording durable state, validating the artifact, and proving equivalent invocation from Codex and Claude Code adapters. This establishes the reusable pattern for later `dev-story`, `review-story`, and `epic-retrospective` workflows.

### What Makes This Special

Hexalith.AI.Tools is not a prompt library, a general agent platform, or a replacement for BMAD. Its differentiator is durable, validated autonomy: AI agents can work longer only when every step is tied to a permitted BMAD transition, persisted checkpoint state, deterministic validation evidence, and a clear next action.

The core insight is that autonomous agent work is valuable only when progress survives the chat session and can be inspected, resumed, and trusted by the team. The runtime-neutral kernel owns workflow behavior, schemas, validation, checkpointing, and result contracts. Codex and Claude Code adapters stay thin, translating runtime context into the same shared contract instead of forking product logic.

Users should choose Hexalith.AI.Tools when they want AI coding agents to advance delivery work safely across supported tools, while preserving Hexalith-specific BMAD standards, artifact paths, validation gates, and escalation behavior.

## Project Classification

Hexalith.AI.Tools is a greenfield developer tool in the AI-assisted software development and internal developer automation domain. The product has medium complexity: it is not a regulated industry product, but it has meaningful technical risk around workflow state, artifact validation, cross-runtime adapter parity, safe mutation controls, resumability, and autonomous stop conditions.

The initial PRD should therefore prioritize precise contracts over broad workflow coverage. MVP requirements should define the `create-story` state machine, checkpoint format, validation rules, host repository configuration, adapter input/output contract, dry-run behavior, failure taxonomy, escalation packet format, and golden test scenarios.

## Success Criteria

### User Success

A project lead succeeds when, given a Hexalith repository with valid BMAD planning artifacts, they can run `create-story`, leave the agent to work, and return to either a validated story artifact with a clear next action or a structured escalation packet with a resume point.

The created story must include source artifact references, acceptance criteria, implementation notes or constraints, validation status, unresolved questions, and the next recommended action. The workflow must stop safely when inputs are missing, stale, ambiguous, or invalid.

### Business Success

MVP business success means one real Hexalith application repository uses Hexalith.AI.Tools as a pinned submodule to create at least one accepted story from existing planning inputs, and that story can be handed to a dev agent without manual reconstruction.

Three-month success means at least five real stories across one or two Hexalith repositories are generated, pass deterministic validation, and require no more than one human correction cycle before becoming dev-ready.

Twelve-month success means the validated automation pattern has expanded beyond `create-story` into implementation, review, and retrospective workflows without forking core workflow logic between Codex and Claude Code.

### Technical Success

MVP success requires the `create-story` workflow to complete all golden scenarios through the runtime-neutral kernel with deterministic checkpointing, dry-run support, schema-valid result JSON, validated story artifact output, and structured escalation on blocked paths.

Codex and Claude Code adapters must invoke the same kernel contract and pass adapter parity tests without contract-specific workflow forks. Adapter-specific presentation is acceptable; result contracts, checkpoint transitions, validation outcomes, and escalation semantics must not drift.

The workflow terminal states are `completed_valid`, `blocked_escalated`, `dry_run_complete`, `failed_validation`, and `adapter_error`.

### Measurable Outcomes

| Criterion | Measurement | MVP Target | Evidence Source |
| --- | --- | --- | --- |
| Golden scenario pass rate | Passed golden scenarios / total golden scenarios | 100% before MVP release | Automated test report |
| Result schema compliance | Schema-valid result JSON / total runs | 100% | Contract tests |
| Resume success | Resumed runs reaching valid or escalated terminal state / total resumed runs | >= 95% | Resume fixture tests |
| Adapter parity | Matching kernel request/result contracts across Codex and Claude fixtures | 100% for golden scenarios | Adapter parity tests |
| Dry-run safety | Dry-runs with no repo mutation / total dry-runs | 100% | Git diff verification |
| Validation reliability | Same inputs and toolchain version produce same validation result | 100% | Determinism tests |
| Human correction cycles | Manual correction cycles before dev-ready story | <= 1 per story in pilot | Pilot review log |
| Failure classification | Typed failures / total failures | 100% | Result JSON and logs |
| Structured next action | Terminal exits with next action / total terminal exits | 100% | Result JSON |

## Product Scope

### MVP - Minimum Viable Product

The MVP must prove one complete BMAD state transition: planning context to validated story. Required scope includes the `create-story` state machine, host repository artifact discovery, checkpoint file format, dry-run mode, story validation rules, result JSON schema, escalation packet format, Codex and Claude Code adapter entrypoints, contract tests, release smoke tests, and golden test repository scenarios.

Golden scenarios must cover clean greenfield repo, missing PRD, ambiguous artifact discovery, invalid story output, interrupted run and resume, adapter parity, and dry-run no-write verification.

The MVP explicitly excludes `dev-story` execution, automated code changes, multi-agent orchestration, remote control plane, cross-repo analytics, custom GUI/IDE experience, autonomous GitHub PR management, and semantic quality scoring beyond deterministic validation.

### Growth Features (Post-MVP)

Post-MVP growth should add `dev-story` once story creation state, validation, and adapter contracts are stable. `review-story` should follow after implementation outputs can be linked to evidence and review decisions. `epic-retrospective` should follow once completed story and review history can be summarized reliably.

Growth scope may also include richer validation schemas, compatibility matrix checks, generated runtime adapter projections, checkpoint migration tooling, release packaging automation, and stronger repository configuration support.

### Vision (Future)

The long-term vision is a governed automation layer for BMAD software delivery workflows. Hexalith.AI.Tools should eventually coordinate planning, story creation, implementation, review, test automation, CI repair, release preparation, and retrospectives across compatible AI coding tools.

## User Journeys

### Journey 1: Pilot Repo Owner Adopts the Toolchain

Elena owns a real Hexalith application repository and wants to prove Hexalith.AI.Tools can be adopted without turning the host repo into a custom integration project. She adds the toolchain as a pinned submodule, configures the expected BMAD artifact paths, and runs a smoke check before trusting it with a real story.

The value moment is a clean first run: the tool discovers the host repository context, reports whether required artifacts exist, confirms the active toolchain version, and identifies any missing configuration without mutating project files unexpectedly.

If setup fails, the tool must explain which configuration or artifact path is missing, whether the failure is recoverable, and what command or file change should happen next.

This journey reveals requirements for submodule adoption, host configuration, version reporting, setup validation, smoke tests, and safe first-run diagnostics.

### Journey 2: Project Lead Starts a Valid `create-story` Run

Elena is leading a Hexalith application project with PRD and architecture artifacts already in the repository. Her immediate problem is not that BMAD lacks structure; it is that she has to keep re-orchestrating the next agent step by hand. She knows the backlog needs a dev-ready story, but she does not want another chat session where context, state, and validation live only in the conversation.

She invokes `create-story` from the host repository. The toolchain discovers the BMAD planning artifacts, confirms the active workflow state, creates an explicit checkpoint, and begins the story-generation transition. Elena steps away expecting the tool to continue only as far as it can safely go.

The value moment happens when she returns and sees one of two clear outcomes: a validated story artifact with source references and a next recommended command, or a structured escalation packet explaining the blocker, failed rule, required decision, and resume point. The run must end in one of the named terminal states: `completed_valid`, `blocked_escalated`, `dry_run_complete`, `failed_validation`, or `adapter_error`.

This journey reveals requirements for artifact discovery, workflow state detection, checkpointing, validated story output, terminal states, structured next action, and escalation packets.

### Journey 3: Project Lead Recovers from Missing or Ambiguous Inputs

Elena starts `create-story` in a repository where planning artifacts are incomplete or ambiguous. The toolchain finds multiple candidate PRDs, stale architecture notes, or missing epic context. In the old workflow, the agent might guess, generate a weak story, or ask a vague question that forces Elena to inspect files manually.

Instead, Hexalith.AI.Tools stops safely. It classifies the run as `blocked_escalated`, records the last valid checkpoint, lists the conflicting or missing artifacts, identifies the decision needed, and recommends the safest next action. If the issue can be resolved by adding or selecting an artifact, the escalation packet gives Elena a specific recovery path.

The value moment is a failure that is still useful. Elena trusts the tool more because it refuses to invent missing product intent. The escalation packet must be useful without rerunning discovery: it must include missing or conflicting artifacts, failed rule IDs, attempted recovery, required human decision, checkpoint reference, and suggested next command.

This journey reveals requirements for ambiguity detection, missing-context handling, typed failure classification, no partial artifact corruption, resumable checkpoints, and human-readable escalation evidence.

### Journey 4: Developer Consumes the Generated Story

Amelia is the developer or AI coding agent operator receiving a story generated by the toolchain. Her problem is downstream ambiguity: a story can look complete but still omit source context, acceptance criteria, implementation constraints, unresolved questions, or validation status.

She opens the generated story and sees source PRD and architecture references, acceptance criteria, implementation notes, test notes, unresolved questions, validation status, validation rule set version, checkpoint ID, and the next recommended action. If the story passed validation, she can hand it to a dev agent without manually reconstructing missing intent.

The value moment is confidence at handoff. The story is not just prose; it is a validated work package with enough context for implementation to begin.

This journey reveals requirements for story schema, required story sections, source traceability, validation metadata, dev-agent readiness, and downstream handoff quality.

### Journey 5: Toolchain Maintainer Releases a Compatible Version

Noah maintains Hexalith.AI.Tools. He needs to evolve schemas, validators, skills, scripts, and runtime adapters without breaking consuming repositories. His risk is compatibility drift: Codex and Claude Code adapters could start behaving differently, checkpoints could become unreadable, or a release could pass a demo while failing real repository fixtures.

Before release, Noah runs contract tests, golden scenarios, adapter parity tests, checkpoint compatibility checks, dry-run no-write verification, and release smoke tests. The release is blocked if result JSON is not schema-valid, if adapter contracts drift, if existing compatible checkpoints cannot be read, or if migration guidance is missing.

The value moment is a release that can be pinned by application teams with known behavior and evidence. The maintainer must prove backward compatibility or provide structured migration guidance. Existing checkpoints and result JSON from compatible versions must remain readable or fail with a typed migration error.

This journey reveals requirements for semantic versioning, schema versioning, checkpoint compatibility, migration guidance, adapter parity tests, release smoke tests, golden fixtures, and compatibility policy.

### Journey 6: Troubleshooter Investigates a Failed Run

Murat is asked why a `create-story` run failed in a consuming repository. He does not have the original chat context and cannot rely on memory. He needs the run evidence to explain what happened and whether it is safe to resume.

He opens the result JSON, logs, checkpoint file, and escalation packet. The failure has a terminal state such as `failed_validation` or `adapter_error`, a typed failure class, failed rule IDs, attempted recovery steps, relevant artifacts, runtime metadata, and the suggested next command. If the run is recoverable, the checkpoint identifies where to resume. If it is not recoverable, the packet explains why.

The value moment is diagnosis without archaeology. Murat can distinguish product ambiguity, validation failure, runtime adapter failure, unsafe mutation risk, and unrecoverable state from the recorded evidence.

This journey reveals requirements for structured logging, typed errors, checkpoint references, validation rule IDs, adapter/runtime metadata, failed recovery evidence, and actionable support diagnostics.

### Journey 7: Runtime Adapter Consumer Invokes Codex or Claude Code

Rina uses Codex in one repository and Claude Code in another. She expects Hexalith.AI.Tools to behave consistently because the workflow is a shared product capability, not a runtime-specific prompt.

She invokes `create-story` through either adapter. Each adapter translates runtime context into the same kernel request contract. For the same fixture inputs, Codex and Claude Code produce equivalent kernel requests, result JSON, checkpoint transitions, validation outcomes, terminal states, and escalation semantics. Adapter-specific presentation can differ, but workflow behavior cannot.

The value moment is portability. Rina can trust the submodule toolchain rather than learning two separate automation products.

This journey reveals requirements for runtime-neutral kernel contracts, thin adapters, adapter parity tests, shared result schema, and no adapter-specific business workflow logic.

### Journey Requirements Summary

The journeys reveal these required capability areas:

- First-run setup validation for pinned submodule adoption.
- Host repository configuration checks and safe diagnostics.
- Host repository artifact discovery for PRD, architecture, epics, stories, and configuration.
- `create-story` workflow state machine with explicit terminal states.
- Durable checkpoint creation, resume, and compatibility behavior.
- Deterministic validation for story structure, artifact references, workflow state, and result schemas.
- Structured result JSON with schema version, checkpoint ID, validation status, terminal state, and next action.
- Dry-run mode with no repository mutation and diffable planned changes.
- Escalation packets with blocker, failed rule IDs, attempted recovery, required human decision, relevant artifacts, runtime metadata, and resume point.
- Generated story schema with source references, acceptance criteria, implementation notes, test notes, unresolved questions, validation status, and next recommended action.
- Failure classes that separate missing input, ambiguity, validation failure, unsafe mutation, adapter failure, and unrecoverable state.
- Adapter parity between Codex and Claude Code through a shared kernel contract.
- Backward compatibility and migration behavior for checkpoints, schemas, and result JSON.
- Evidence requirements that let support diagnose a failed run without chat history.
- Release validation through golden scenarios, contract tests, checkpoint compatibility tests, adapter parity tests, and smoke tests.

## Domain-Specific Requirements

### Compliance & Governance

- The product must support repo-level or organization-level automation policy configuration with versioned schemas.
- Every run must produce an auditable run record containing run ID, toolchain version, adapter version, repo reference, dirty-state summary, selected workflow, mutation plan, validation results, terminal state, and failure reason.
- Logs, prompts, escalation packets, and stored artifacts must apply redaction rules for secrets and sensitive data.

### Technical Constraints

- Repository content, generated artifacts, previous run logs, issue text, PR comments, and external tool output must be treated as untrusted input and must not override workflow policy, tool permissions, or stop conditions.
- Repository mutations must be classified by risk level: read-only, generated-artifact write, allowlisted edit, dependency/config change, external publication, destructive operation, or history-changing operation.
- These mutation categories describe the broader lifecycle model; MVP mutation classes are narrowed in the scoping section.
- Each mutation class must define required controls such as dry-run preview, explicit approval, checkpoint, audit record, rollback guidance, or hard block.
- Dry-run mode must produce a materially equivalent mutation plan and artifact diff preview without mutating the repository outside the designated dry-run output location.
- Checkpoints must be replay-safe, resumable, idempotent, and able to refuse continuation when schema, workspace state, or toolchain version is incompatible.

### Integration Requirements

- Adapters must preserve shared workflow semantics, state transitions, mutation policy, result contracts, validation behavior, escalation packet shape, and terminal states.
- Codex and Claude Code parity must be validated through shared fixtures and mocked model/tool responses, without requiring identical generated prose.
- Contract compatibility must be governed by a compatibility matrix covering toolchain version, skill version, schema version, adapter version, and consuming repository template/version.

### Risk Mitigations

- Stop conditions must include destructive mutation requests, untracked user changes in target files, schema incompatibility, missing required skills, repeated tool failure, ambiguous story state, adapter capability mismatch, and validation failure after retry limits.
- Retries must be bounded, visible in the run record, tied to classified failure causes, and must not repeat mutating actions unless mutation state is known.
- Escalation packets must be self-contained and include blocked action, risk category, last safe checkpoint, evidence references, attempted recovery, required human decision, recommended next action, and safe resume context.
- Golden test repositories must cover clean repo, dirty repo, missing artifacts, submodule mismatch, malformed output, interrupted resume, conflicting generated files, malicious prompt content, unsupported adapter capability, and dry-run no-write verification.

## Innovation & Novel Patterns

### Detected Innovation Areas

Hexalith.AI.Tools is innovative less because it uses AI agents and more because it reframes AI coding assistance as safe repo automation with durable workflow state. The core novelty is the combination of BMAD workflow semantics, checkpoint-backed autonomy, deterministic validation, mutation policy, and cross-runtime adapter parity.

The primary innovation areas are:

- Runtime-neutral BMAD workflow kernel that owns state transitions, validation, schemas, checkpointing, and result contracts.
- Checkpoint state as the control plane for agent autonomy, instead of relying on chat history.
- Safe repository automation model with mutation risk classification, dry-run planning, explicit approval boundaries, and structured escalation.
- Cross-runtime behavior parity across Codex and Claude Code through shared contracts and conformance fixtures.
- Validated story creation as the first narrow proof of a broader autonomous BMAD delivery loop.

### Market Context & Competitive Landscape

The broader market is moving toward reusable agent capabilities through skills, plugins, MCP servers, and runtime-specific adapters. Existing tools can package prompts, expose tools, or run coding agents, but they generally do not provide a Hexalith-specific BMAD automation layer with durable checkpoints, workflow contracts, validation gates, and explicit mutation policy.

Hexalith.AI.Tools should therefore avoid competing as a general agent platform. Its differentiation is internal fit: it operationalizes Hexalith BMAD delivery standards inside consuming repositories while preserving consistent behavior across supported AI coding runtimes.

### Validation Approach

The innovation should be validated through one complete BMAD transition before broader workflow expansion: planning context to validated story.

Validation evidence should include:

- Golden repositories covering clean, dirty, missing-context, malicious-input, interrupted-run, and adapter-mismatch scenarios.
- Contract tests proving schema-valid result JSON, terminal-state exclusivity, mutation policy enforcement, and escalation packet shape.
- Adapter parity tests showing equivalent state transitions and result contracts for Codex and Claude Code using shared fixtures.
- Dry-run no-write verification with diffable planned operations.
- Resume tests proving checkpoint idempotency and safe refusal on incompatible state.

### Risk Mitigation

The main innovation risk is over-trusting agent autonomy before workflow controls are mature. The MVP mitigates this by narrowing scope to `create-story`, enforcing validation before handoff, and requiring structured escalation when ambiguity or unsafe mutation appears.

The fallback if the innovation does not fully work is still valuable: Hexalith.AI.Tools can operate as a validated workflow assistant that produces explicit plans, dry-run outputs, and escalation packets, even before longer autonomous execution is trusted.

## Developer Tool Specific Requirements

### Project-Type Overview

Hexalith.AI.Tools is a developer automation toolchain consumed by application repositories as a pinned git submodule. The primary developer job is to add the toolchain to an application repository, install the BMAD Method, verify the setup, and run AI automation through Claude Code or Codex without hand-wiring runtime-specific workflows.

The MVP is greenfield adoption. Application developers add `Hexalith.AI.Tools` to a consuming repository, install or initialize BMAD Method in that repository, configure the expected BMAD artifact paths and automation policy, run a smoke check, and then run the first `create-story` workflow.

### Technical Architecture Considerations

The preferred implementation language for the core is C#. C# owns core orchestration, workflow contracts, state transitions, checkpointing, validation contracts, and result schemas where practical. PowerShell, Python, and Node.js may be used for setup automation, validation utilities, adapter glue, or ecosystem-specific tooling when they are the pragmatic fit.

The toolchain should separate:

- Core workflow orchestration and state transitions.
- BMAD artifact discovery and validation.
- Checkpoint and resume management.
- Runtime adapter invocation for Claude Code and Codex.
- Deterministic setup, validation, and smoke-check utilities.
- Documentation and workflow examples for consuming repository adoption.

Internal scripts, classes, and helper utilities are not stable public APIs unless explicitly documented as part of the command or adapter contract.

### Language Matrix

| Area | Preferred / Allowed Technology | Notes |
| --- | --- | --- |
| Core workflow and contracts | C# | Preferred for orchestration, state, contracts, validation, and result schemas. |
| Setup and repository automation | PowerShell | Allowed for install, diagnostics, and Windows-friendly automation. |
| Validation or utility scripts | Python | Allowed when existing BMAD scripts or simple cross-platform utilities make it pragmatic. |
| Adapter or ecosystem tooling | Node.js | Allowed when required by Claude Code, Codex, plugin, or package ecosystem needs. |
| Host application code | Not prescribed | Consuming application repositories keep their own technology choices. |

### Submodule Installation Contract

MVP installation is through a pinned git submodule. Consuming repositories must pin a specific commit or tag and must not depend on implicit global mutable state for normal operation.

The installation guide must cover:

- Required prerequisites: Git, supported shell, required .NET runtime or SDK, BMAD Method, Claude Code, and Codex.
- Submodule add, initialize, update, pin, and rollback commands.
- Expected submodule location and host repository layout.
- Where host repository automation configuration lives.
- Where generated stories, run records, result JSON, logs, checkpoints, and escalation packets are written.
- What generated files should be committed, ignored, or treated as local run evidence.
- Setup verification and smoke-check commands.

Package manager installation, public marketplace distribution, GUI installation, hosted service operation, and implicit auto-updates are out of scope for MVP.

### MVP Command Surface

The MVP public surface is workflow-oriented, not library-oriented. Required command or invocation contracts must cover:

- Setup or smoke-check validation.
- `create-story` execution.
- `create-story` dry-run execution.
- Resume from checkpoint or run ID.
- Result JSON generation.
- Escalation packet generation when blocked.
- Claude Code adapter invocation.
- Codex adapter invocation.

Each public command or adapter invocation must define required inputs, optional inputs, output paths, stdout/stderr behavior, deterministic exit codes, result JSON schema version, and failure behavior.

Non-interactive execution must be supported for setup checks, dry-runs, and CI-style validation. CI mode must not prompt for human input and must emit structured failure evidence instead.

### Adapter Invocation Contracts

Claude Code and Codex are required MVP integrations. Each adapter must:

- Expose documented invocation for setup/smoke check, `create-story`, dry-run, resume, result JSON, and escalation packet generation.
- Translate runtime context into the shared workflow contract.
- Preserve shared workflow semantics, state transitions, mutation policy, validation behavior, result JSON shape, and terminal states.
- Keep adapter-specific behavior documented separately from shared workflow behavior.
- Avoid owning business workflow logic that belongs in the core toolchain.

### Result JSON Schema

The result JSON schema is part of the MVP contract and must be documented. At minimum, it must include:

- `runId`
- `status`
- `startedAt`
- `completedAt`
- `toolchainVersion`
- `adapter`
- `repoRef`
- `workflowId`
- `inputs`
- `outputs`
- `warnings`
- `errors`
- `validation`
- `nextAction`
- `escalationPacketPath`

The schema must be versioned and validated before downstream handoff.

### Documentation and Workflow Examples

The MVP documentation set must include:

- Quick start guide.
- Submodule installation guide.
- Greenfield adoption guide.
- Claude Code invocation guide.
- Codex invocation guide.
- Workflow examples.
- Result JSON schema reference.
- Escalation packet reference.
- Upgrade and rollback guidance.

The quick start must let a developer complete the happy path from a clean consuming repository in under 10 minutes: add the submodule, initialize/update it, install or verify BMAD Method, run setup validation, run first dry-run automation, inspect result JSON, and understand the next action.

Workflow examples must show realistic command flows, minimal inputs, expected output shape, success criteria, common failure cases, and recovery paths.

### Greenfield Adoption and Upgrade Guide

MVP is greenfield-only. The guide must explain how a consuming application repository starts using the toolchain:

- Add `Hexalith.AI.Tools` as a pinned submodule.
- Install or initialize BMAD Method in the consuming repository.
- Confirm required BMAD planning artifacts exist.
- Configure automation policy and artifact paths.
- Run setup validation before the first automation workflow.
- Run `create-story` in dry-run mode before mutation.
- Inspect result JSON and escalation packets.
- Update or roll back the pinned submodule version deliberately.

Automated migration from existing `_bmad` project layouts is out of scope for MVP.

### MVP Acceptance Criteria

- `AC-DT-001`: A consuming repository can add and pin `Hexalith.AI.Tools` as a git submodule.
- `AC-DT-002`: A developer can install or verify BMAD Method from the consuming repository setup path.
- `AC-DT-003`: The setup or smoke-check command detects required prerequisites and emits result JSON.
- `AC-DT-004`: The Claude Code adapter can run the smoke workflow and emit the shared result JSON shape.
- `AC-DT-005`: The Codex adapter can run the smoke workflow and emit the shared result JSON shape.
- `AC-DT-006`: `create-story --dry-run` emits a mutation plan and schema-valid result JSON without mutating repository files outside the designated dry-run output location.
- `AC-DT-007`: A blocked or failed workflow emits a self-contained escalation packet.
- `AC-DT-008`: Resume from checkpoint does not duplicate generated files, run records, status transitions, or escalation packets.
- `AC-DT-009`: Public command and adapter contracts document inputs, outputs, exit codes, schema versions, and failure behavior.
- `AC-DT-010`: MVP release is blocked unless install, adapter, dry-run, resume, and failure-packet smoke tests pass.

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** Platform MVP focused on one workflow transition.

The MVP product promise is narrow: given valid BMAD planning context in a consuming repository, Hexalith.AI.Tools can safely produce a validated dev-ready story through `create-story`, with traceability, resumability, deterministic result JSON, dry-run safety, and structured escalation when blocked.

The MVP should prove repeatable workflow infrastructure, not broad story-writing intelligence or full BMAD lifecycle automation.

### MVP Feature Set (Phase 1)

**Core User Journeys Supported:**

- Pilot repo owner adopts the pinned submodule and validates setup.
- Project lead runs `create-story` from valid BMAD planning context.
- Project lead receives either a validated story or a structured escalation packet.
- Developer consumes a generated story with source references, acceptance criteria, validation metadata, and next action.
- Toolchain maintainer validates release compatibility through golden scenarios and adapter parity tests.
- Troubleshooter diagnoses failed runs using result JSON, logs, checkpoints, and escalation packets.
- Runtime adapter consumer invokes equivalent deterministic behavior through Codex or Claude Code.

**User-Visible MVP Capabilities:**

- Pinned git submodule adoption for consuming repositories.
- BMAD Method installation or verification path.
- Setup and smoke-check validation.
- `create-story` normal execution.
- `create-story` dry-run execution.
- Resume from checkpoint or run ID.
- Result JSON output.
- Escalation packet output when blocked.
- Claude Code and Codex invocation paths.

**Kernel Capabilities Required for MVP:**

- `create-story` workflow state machine.
- Host repository BMAD artifact discovery.
- Host repository configuration for artifact paths and automation policy.
- Story artifact generation with required schema fields and source traceability.
- Checkpoint persistence, resume, compatibility checking, and idempotency.
- Dry-run no-write verification and mutation plan output.
- MVP mutation classification limited to `none`, `docs-only`, `workflow-artifact`, `repo-config`, or `blocked`.
- Result JSON schema validation.
- Escalation packet generation with deterministic failure categories.
- Adapter contract for Claude Code and Codex.
- Deterministic contract parity across adapters for state transitions, result JSON shape, artifact writes, dry-run behavior, escalation classification, and terminal states.

**MVP Mutation Boundary:**

MVP may write only BMAD/story artifacts, workflow run records, result JSON, logs, checkpoints, dry-run outputs, escalation packets, and approved repository automation configuration. MVP must not modify application source code.

### MVP Exit Criteria

`create-story` is MVP-complete only when:

- Valid planning context produces a schema-valid dev-ready story.
- The generated story includes source artifact references, acceptance criteria, implementation notes or constraints, test notes, unresolved questions if any, validation metadata, and next recommended action.
- Missing, malformed, ambiguous, or incompatible planning context produces an escalation packet instead of partial story output.
- Dry-run produces a mutation plan and writes zero repository files outside the designated dry-run output location.
- Resume after interruption is idempotent and does not duplicate generated files, run records, status transitions, or escalation packets.
- Claude Code and Codex adapters produce equivalent deterministic contracts for the same golden scenarios, without requiring identical generated prose.
- Golden repo release smoke tests pass from a clean clone.

**Minimum MVP Test Matrix:**

- 2-3 golden repositories: clean minimal host repo, realistic planning-context repo, and misconfigured repo for escalation paths.
- 2 adapters: Claude Code and Codex.
- 3 run modes: normal, dry-run, interrupted/resumed.
- 4 outcome classes: success, validation failure, blocked/escalated, idempotent no-op.

**Required Negative Paths:**

- Missing BMAD Method installation.
- Missing or ambiguous PRD/planning artifacts.
- Malformed planning context.
- Dirty repository with conflicting target files.
- Interrupted run and resume.
- Schema violation.
- Attempted out-of-bound mutation.
- Adapter capability mismatch.

### Post-MVP Features

**Phase 2 (Post-MVP):**

- `dev-story` workflow once `create-story` contracts and validation are stable.
- `review-story` workflow once implementation outputs can be linked to evidence and review decisions.
- Checkpoint migration tooling.
- Richer validation schemas and compatibility matrix checks.
- Generated runtime adapter projections if manual adapter maintenance starts to drift.
- Stronger repository configuration support.

**Phase 3 (Expansion):**

- `epic-retrospective` workflow.
- Test automation and CI repair workflow support.
- Release preparation automation.
- Cross-runtime validation harness expansion.
- Broader BMAD delivery automation across compatible AI coding tools.

**Explicitly Out of Scope for MVP:**

- `dev-story` execution.
- `review-story` execution.
- `epic-retrospective` execution.
- Automated application code changes.
- Multi-agent orchestration.
- Remote or hosted control plane.
- Cross-repo analytics.
- GUI or custom IDE experience.
- Package manager distribution.
- Public marketplace distribution.
- Autonomous GitHub PR management.
- Semantic quality scoring beyond deterministic schema and source validation.

### Risk Mitigation Strategy

**Technical Risks:**  
The riskiest assumptions are checkpoint correctness, adapter contract parity, safe mutation controls, reliable artifact discovery, and deterministic validation. MVP mitigates these by limiting scope to `create-story`, enforcing no-write dry-run checks, requiring idempotent resume behavior, and validating both adapters against the same golden scenarios.

**Market / Adoption Risks:**  
The main adoption risk is that application developers may find setup too heavy or outputs too hard to trust. MVP mitigates this through pinned submodule adoption, smoke checks, clear result JSON, structured escalation packets, and quick-start documentation that works from a clean consuming repository.

**Resource Risks:**  
The MVP can shrink only by reducing breadth, not by weakening safety contracts. If resources are constrained, defer workflow expansion, richer migration tooling, and nonessential repository configuration features before reducing checkpointing, validation, dry-run, result JSON, escalation packet, or adapter contract quality.

## Functional Requirements

### Repository Adoption & Setup

- FR1: Application developers can add Hexalith.AI.Tools to a consuming repository as a pinned git submodule.
- FR2: Application developers can verify that the BMAD Method is installed or initialized for the consuming repository.
- FR3: Application developers can configure expected BMAD artifact paths for the consuming repository.
- FR4: Application developers can configure repository automation policy for allowed workflow behavior.
- FR5: Application developers can run a setup validation that reports missing prerequisites.
- FR6: Application developers can run a smoke check before trusting the toolchain with a real workflow.
- FR7: Application developers can identify the active toolchain version used by a consuming repository.
- FR8: Application developers can update or roll back the pinned toolchain version deliberately.

### Create-Story Workflow Execution

- FR9: Project leads can invoke `create-story` from a consuming repository.
- FR10: Project leads can run `create-story` in dry-run mode before allowing repository mutation.
- FR11: Project leads can resume an interrupted workflow from a checkpoint or run ID.
- FR12: The system can determine whether the current repository has enough valid planning context to start `create-story`.
- FR13: The system can stop `create-story` when required planning context is missing, malformed, stale, ambiguous, or incompatible.
- FR14: The system can end every workflow run in exactly one terminal state.
- FR15: The system can recommend the next action at the end of every workflow run.

### Artifact Discovery & Story Output

- FR16: The system can discover BMAD planning artifacts in configured host repository locations.
- FR17: The system can identify ambiguous or conflicting candidate planning artifacts.
- FR18: The system can generate a dev-ready story from valid planning context.
- FR19: The system can include source artifact references in generated stories.
- FR20: The system can include acceptance criteria in generated stories.
- FR21: The system can include implementation notes or constraints in generated stories.
- FR22: The system can include test notes in generated stories.
- FR23: The system can include unresolved questions in generated stories when needed.
- FR24: The system can include validation metadata and next recommended action in generated stories.
- FR25: Developers can consume generated stories without reconstructing missing product intent from chat history.

### Run State, Checkpoints & Evidence

- FR26: The system can create durable checkpoints during workflow execution.
- FR27: The system can validate checkpoint compatibility before resuming.
- FR28: The system can resume without duplicating generated files, run records, status transitions, or escalation packets.
- FR29: The system can produce a run record for every workflow run.
- FR30: The system can produce schema-valid result JSON for every terminal workflow outcome.
- FR31: The system can include workflow inputs, outputs, warnings, errors, validation status, terminal state, and next action in result JSON.
- FR32: Troubleshooters can diagnose failed runs using result JSON, logs, checkpoints, and escalation packets without requiring chat history.

### Safety, Mutation Control & Escalation

- FR33: The system can classify planned repository mutations before applying them.
- FR34: The system can restrict MVP writes to BMAD/story artifacts, run evidence, dry-run outputs, escalation packets, checkpoints, and approved automation configuration.
- FR35: The system can block attempted application source code mutation during MVP workflows.
- FR36: The system can produce a dry-run mutation plan without writing outside the designated dry-run output location.
- FR37: The system can detect dirty or conflicting target files before writing.
- FR38: The system can treat repository content, generated artifacts, prior logs, issue text, PR comments, and external tool output as untrusted input.
- FR39: The system can prevent untrusted input from overriding workflow policy, permissions, or stop conditions.
- FR40: The system can generate a structured escalation packet when blocked.
- FR41: Escalation packets can include blocked action, risk category, last safe checkpoint, evidence references, attempted recovery, required human decision, recommended next action, and safe resume context.
- FR42: The system can apply redaction rules to logs, prompts, escalation packets, and stored artifacts.

### Runtime Adapter Support

- FR43: Claude Code users can invoke setup validation, dry-run, `create-story`, resume, result JSON generation, and escalation packet generation.
- FR44: Codex users can invoke setup validation, dry-run, `create-story`, resume, result JSON generation, and escalation packet generation.
- FR45: Runtime adapters can translate runtime context into the shared workflow contract.
- FR46: Runtime adapters can preserve shared state transitions, mutation policy, validation behavior, result JSON shape, escalation classification, and terminal states.
- FR47: Runtime adapters can expose adapter-specific behavior separately from shared workflow behavior.
- FR48: The system can validate deterministic contract parity across Claude Code and Codex for golden scenarios.

### Validation & Release Confidence

- FR49: Toolchain maintainers can validate generated stories against required story structure.
- FR50: Toolchain maintainers can validate result JSON against its schema.
- FR51: Toolchain maintainers can validate escalation packets against required fields.
- FR52: Toolchain maintainers can run golden repository scenarios covering success, validation failure, blocked escalation, and idempotent no-op outcomes.
- FR53: Toolchain maintainers can verify dry-run no-write behavior through repository state comparison.
- FR54: Toolchain maintainers can verify adapter parity using shared fixtures.
- FR55: Toolchain maintainers can block release when install, adapter, dry-run, resume, failure-packet, or golden-repo smoke tests fail.

### Documentation & Operator Guidance

- FR56: Application developers can follow a quick start to add the toolchain, verify setup, run dry-run automation, inspect result JSON, and understand next action.
- FR57: Application developers can follow installation guidance for prerequisites, submodule setup, version pinning, update, and rollback.
- FR58: Application developers can follow adoption guidance for repository layout, configuration, generated artifacts, and commit/ignore expectations.
- FR59: Claude Code users can follow invocation guidance for supported workflow commands.
- FR60: Codex users can follow invocation guidance for supported workflow commands.
- FR61: Developers and troubleshooters can interpret result JSON using documented schema guidance.
- FR62: Developers and troubleshooters can interpret escalation packets using documented field guidance.
- FR63: Developers can follow recovery guidance for missing context, failed validation, blocked mutation, adapter mismatch, and checkpoint resume.

## Non-Functional Requirements

### Reliability & Correctness

- NFR1: Every workflow run must end in exactly one terminal state.
- NFR2: Resume after interruption must be idempotent and must not duplicate generated files, run records, status transitions, or escalation packets.
- NFR3: Dry-run execution must produce zero repository file mutations outside the designated dry-run output location.
- NFR4: Missing, malformed, ambiguous, or incompatible planning context must produce a structured escalation packet instead of partial story output.
- NFR5: Checkpoint resume must refuse continuation when checkpoint schema, workspace state, or toolchain version is incompatible without a documented recovery path.
- NFR6: Golden repository smoke tests must pass from a clean clone before MVP release.

### Security & Safety

- NFR7: Repository content, generated artifacts, previous run logs, issue text, PR comments, and external tool output must be treated as untrusted input.
- NFR8: Untrusted input must not override workflow policy, tool permissions, stop conditions, or mutation boundaries.
- NFR9: MVP workflows must not modify application source code.
- NFR10: Planned repository mutations must be classified before write using the MVP mutation classes: `none`, `docs-only`, `workflow-artifact`, `repo-config`, or `blocked`.
- NFR11: Logs, prompts, result JSON, escalation packets, and stored artifacts must apply secret and sensitive-data redaction rules.
- NFR12: Destructive operations, history-changing git operations, external publication, and application code mutation must be blocked in MVP workflows.

### Validation & Quality Gates

- NFR13: Generated stories must be schema-validated before downstream handoff.
- NFR14: Result JSON must be schema-validated for every terminal workflow outcome.
- NFR15: Escalation packets must be validated against required fields before persistence.
- NFR16: Claude Code and Codex adapters must produce equivalent deterministic contracts for the same golden scenarios, without requiring identical generated prose.
- NFR17: MVP release must be blocked if install, adapter, dry-run, resume, escalation-packet, or golden-repo smoke tests fail.
- NFR18: Golden scenarios must cover success, validation failure, blocked escalation, and idempotent no-op outcomes.

### Observability & Auditability

- NFR19: Every run record must include run ID, toolchain version, adapter, repository reference, workflow ID, terminal state, validation result, and next action.
- NFR20: Failed or blocked runs must include evidence references sufficient for troubleshooting without chat history.
- NFR21: Escalation packets must include blocked action, risk category, last safe checkpoint, attempted recovery, required human decision, recommended next action, and safe resume context.
- NFR22: Result JSON and escalation packet paths must be discoverable from the terminal workflow output.
- NFR23: Toolchain maintainers must be able to trace release validation evidence to golden scenario results.

### Integration Compatibility

- NFR24: Claude Code and Codex adapter behavior must preserve shared state transitions, mutation policy, validation behavior, result JSON shape, escalation classification, and terminal states.
- NFR25: Adapter-specific behavior must be documented separately from shared workflow behavior.
- NFR26: Toolchain, schema, skill, adapter, and consuming repository compatibility must be governed by a documented compatibility matrix.
- NFR27: Unsupported adapter capability or incompatible contract version must produce a typed failure or escalation packet.

### Performance & Developer Feedback

- NFR28: Setup validation and smoke checks should complete within 2 minutes on a clean consuming repository under normal local conditions.
- NFR29: Dry-run output must provide actionable mutation and validation feedback before real execution.
- NFR30: Failed setup, validation, adapter, or workflow runs must report a specific next action rather than a generic failure.
- NFR31: The quick start must allow a developer to complete the happy path from a clean consuming repository in under 10 minutes.

### Portability & Installation

- NFR32: Normal operation must not depend on implicit global mutable state beyond documented prerequisites.
- NFR33: Consuming repositories must be able to pin, update, and roll back the toolchain version deliberately.
- NFR34: Setup guidance must identify required prerequisites for Git, supported shell, .NET runtime or SDK, BMAD Method, Claude Code, and Codex.
- NFR35: Public command and adapter contracts must define inputs, outputs, exit codes, schema versions, and failure behavior.

### Maintainability

- NFR36: Shared workflow behavior must remain in the core toolchain contract rather than being duplicated in runtime adapters.
- NFR37: Internal scripts, classes, and helper utilities must not be treated as stable APIs unless explicitly documented.
- NFR38: Breaking changes to result schemas, checkpoint formats, adapter contracts, or repository configuration must be reflected in versioning and compatibility guidance.
- NFR39: MVP documentation must include install, configure, run, dry-run, resume, result JSON interpretation, escalation handling, and upgrade/rollback guidance.
