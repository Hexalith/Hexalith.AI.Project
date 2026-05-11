---
title: "Product Brief: Hexalith.AI.Tools"
status: "draft"
created: "2026-04-26T17:12:17.0717278+02:00"
updated: "2026-04-26T17:22:57.1714341+02:00"
inputs:
  - "User discovery conversation on 2026-04-26"
  - "https://github.com/bmad-code-org/BMAD-METHOD"
  - "_bmad-output/planning-artifacts/research/technical-codex-claude-code-plugins-skills-agents-commands-research-2026-04-26.md"
  - "BMAD party-mode review by John, Winston, Amelia, and Murat on 2026-04-26"
---

# Product Brief: Hexalith.AI.Tools

## Executive Summary

Hexalith.AI.Tools is a versioned BMAD automation toolchain for Hexalith application repositories. Future project teams add it as a git submodule to their application repository, then use it to advance BMAD work through story creation, implementation, review, and retrospective steps with less human babysitting and more consistent outputs.

The product promise is autonomy with validity: when a BMAD backlog item is ready, the agent system should move it through the next valid workflow steps, preserve state, validate outputs, and stop only for real decisions, unsafe conditions, or missing context. The goal is not simply to call agents faster. The goal is to let AI coding agents work longer while producing BMAD artifacts that teams can trust.

The first version should prove this with a small, testable automation kernel: portable BMAD skills, deterministic scripts, versioned schemas, durable checkpoints, and thin Codex and Claude Code adapters. The runtime adapters should not own workflow logic. They should invoke the same core contracts so Hexalith teams get reproducible behavior across supported tools.

The first proof point is not a full autonomous development lifecycle. It is one reliable BMAD state transition: taking valid planning context, producing a valid story, recording durable state, and proving that both Codex and Claude Code can invoke the same workflow contract. Once that transition is reliable, the same pattern can expand to implementation, review, and retrospective workflows.

In plain terms, Hexalith.AI.Tools should let a project lead start an AI delivery run, walk away longer, and return to validated BMAD progress instead of a stalled chat session.

## The Problem

BMAD provides strong planning and delivery workflows, but future project teams still need a human to repeatedly decide what to run next, pass the right artifact paths, preserve context, and verify that outputs are usable. That manual orchestration interrupts AI coding sessions before the agent has reached a meaningful boundary.

The recurring pain is specific:

- A team has BMAD planning artifacts, but the next story still has to be manually generated or selected.
- An agent can implement a story, but the handoff into review is usually a separate prompt.
- Review findings may not reliably drive the next dev action.
- Retrospectives happen only if someone remembers to trigger them.
- Partial runs are hard to resume because state lives in chat history instead of durable project files.

This makes BMAD adoption dependent on individual discipline. It also reduces the value of AI coding tools because the human becomes the workflow scheduler.

## The Solution

Hexalith.AI.Tools gives each application repository a repeatable BMAD automation loop. The user experience should feel like giving an AI agent a durable mission:

> Advance the current BMAD work item as far as safely possible. Record progress. Validate every artifact. Escalate only when a human decision is genuinely required.

The toolchain coordinates the recurrent development flow:

- Create story from existing epics, PRD, architecture, and planning context.
- Dev story by executing implementation work against the active story.
- Review story using structured quality and code review gates.
- Run epics retrospective after a meaningful set of stories or an epic completes.

The workflow must be resumable. Every run should write checkpoint state, generated artifact references, validation results, assumptions made without human input, and a clear next action.

## First User And Job To Be Done

The primary user is the application project lead responsible for keeping a Hexalith application moving through BMAD delivery. They may delegate execution to Codex or Claude Code, but they remain accountable for whether the resulting artifacts and code are usable.

Core job to be done:

> When I have BMAD planning context or a backlog item ready, I want the AI toolchain to advance it through the next valid delivery step with minimal interruption, so my team receives usable stories, implementation outputs, reviews, and retrospectives without manually orchestrating every agent call.

## Technical Approach

The architecture should be a shared deterministic core with runtime adapters at the edge.

Core toolchain responsibilities:

- Discover BMAD artifacts and active work items in the host repository.
- Read and write versioned workflow checkpoint state.
- Validate generated BMAD artifacts against schemas and structural rules.
- Normalize outputs so downstream steps receive predictable inputs.
- Produce structured result JSON, logs, and escalation packets.
- Support dry-run mode before mutating host project files.

Runtime adapter responsibilities:

- Translate Codex or Claude Code invocation context into the shared contract.
- Call the same scripts and skills used by the other runtime.
- Return structured success, pause, validation failure, or runtime failure states.
- Avoid business workflow logic inside adapter-specific files.

The submodule should behave like a pinned toolchain, not shared mutable app code. `Hexalith.AI.Tools` contains automation logic, templates, schemas, validators, tests, and adapter entrypoints. The consuming application repository owns BMAD documents, project config overrides, generated stories, code changes, and review outputs.

### MVP Implementation Strategy

The MVP should be kernel-first, not adapter-first. Skills and adapters make the toolchain convenient to invoke, but the core value depends on a runtime-neutral kernel that can discover work, manage state, validate artifacts, and produce structured results.

The first workflow, `create-story`, should exercise the full kernel contract before other workflows are added. This keeps the MVP narrow while proving the hard parts: submodule path resolution, host repository configuration, checkpointing, validation, dry-run behavior, and Codex/Claude Code invocation parity.

## Key Product Decisions

The initial product direction rests on five decisions:

- `Hexalith.AI.Tools` is a versioned toolchain consumed as a submodule, not shared mutable application code.
- The host application repository owns project artifacts, generated stories, code changes, reviews, and retrospectives.
- The runtime-neutral core owns workflow behavior, validation, schemas, checkpointing, and result contracts.
- Codex and Claude Code adapters translate runtime context into the shared contract but do not implement business workflow logic.
- Checkpoint state is the control plane for autonomy; chat history is useful context but not a source of truth.

## What Makes This Different

Hexalith.AI.Tools is not a replacement for BMAD and not a general agent platform. It operationalizes BMAD inside Hexalith repositories.

The differentiators are:

- Versioned submodule adoption: application teams pin a known toolchain version.
- BMAD-first sequencing: automation starts from BMAD artifacts and workflow state.
- Durable autonomy: checkpoints make long runs resumable without relying on chat memory.
- Validity gates: generated outputs must pass deterministic checks before the next step starts.
- Runtime portability: Codex and Claude Code use the same core workflow contract.
- Internal fit: artifact paths, stop conditions, assumptions, and review expectations can match Hexalith standards.

## MVP Scope

The MVP should prove the automation kernel before broadening workflow coverage.

Phase 1 MVP:

- Implement `create-story` end to end.
- Prove one complete BMAD state transition before expanding the loop: planning context to validated story.
- Add checkpoint/resume for interrupted runs.
- Add dry-run mode.
- Validate generated story artifacts for required BMAD structure.
- Run from a consuming application repository where `Hexalith.AI.Tools` is a git submodule.
- Expose the same workflow through Codex and Claude Code adapters.

Phase 1 should establish the folder and contract shape:

- `Hexalith.AI.Tools/skills/`
- `Hexalith.AI.Tools/scripts/`
- `Hexalith.AI.Tools/adapters/codex/`
- `Hexalith.AI.Tools/adapters/claude-code/`
- `Hexalith.AI.Tools/schema/`
- `Hexalith.AI.Tools/tests/`

Next workflow increments:

- Add `dev-story` once create-story state, validation, and adapter contracts are stable.
- Add `review-story` once implementation outputs can be linked to review evidence.
- Add `epic-retrospective` once completed story and review history can be summarized reliably.

Out of scope for the first version:

- Public marketplace distribution.
- A visual workflow UI.
- Replacement of upstream BMAD modules.
- A generalized workflow engine.
- Runtime-specific workflow forks.
- Deep external integrations unless required for the core loop.

## Definition Of Autonomous Valid Work

Autonomous work is valid only when the toolchain completes a permitted BMAD state transition.

A valid transition has:

- Known input artifacts.
- A declared workflow action.
- A durable checkpoint before and after the action.
- Generated or updated artifacts in expected host-repository locations.
- Deterministic validation results.
- A clear next state: continue, pause for human input, retry safely, or fail with evidence.

This prevents the product from optimizing for time spent by an AI agent instead of trustworthy forward progress.

## Success Criteria

The north-star metric is autonomous valid work: how far the AI can advance BMAD delivery without human interaction while still producing artifacts that pass validation and review.

MVP metrics:

- Median uninterrupted workflow duration.
- Number of autonomous workflow steps completed before human input.
- Percentage of runs completing without avoidable human intervention.
- First-pass artifact validation rate.
- Resume success rate after interruption.
- Human intervention count per completed story.
- Rework rate after human review.
- Adapter parity rate for equivalent Codex and Claude Code scenarios.

The product is working when interruptions become meaningful. Humans should be asked for decisions, approvals, or missing context, not for repeated prompt choreography.

## Quality Gates And Stop Conditions

Autonomy must be treated as a system under test. The PRD should define machine-checkable gates before expanding workflow breadth.

Required gates include:

- Required BMAD sections are present.
- Placeholder text is not left unresolved.
- Generated artifacts reference real source artifacts where claimed.
- Story outputs include acceptance criteria, tasks, dev notes, and test notes.
- Review outputs include findings, severity, evidence, and pass/fail decision.
- Retrospective outputs include outcomes, lessons, follow-ups, and owners.
- Checkpoint state records workflow phase, artifact paths, completed steps, assumptions, validation status, runtime metadata, and next action.

Stop conditions should include malformed output, missing context, failed validation, dirty or conflicting repository state, repeated retry loops, unsafe mutation, adapter drift, and any decision that changes product or engineering intent.

When stopped, the toolchain should produce a structured escalation packet: current state, last successful checkpoint, failed gate, evidence, recommended options, and safest default action.

### Failure Classification

Failures should be classified before escalation:

- **Recoverable:** the toolchain can continue after refreshing state or re-reading artifacts.
- **Retryable:** the same action may be attempted again within a defined retry limit.
- **Human-blocked:** the toolchain needs a decision, missing context, approval, or product judgment.
- **Terminal:** the toolchain cannot safely continue because state, artifacts, or repository conditions are invalid.

Every stopped run should include the failure class in its escalation packet.

## PRD Decisions Still Needed

The PRD should resolve:

- Exact workflow state machine for `create-story`.
- Required story schema and validation rules.
- Checkpoint file format, versioning, and location.
- Host repository configuration format.
- Adapter input/output contract.
- Dry-run behavior and mutation permissions.
- Error taxonomy and retry limits.
- Human escalation packet format.
- Golden test repository scenarios.
- Whether Codex and Claude Code parity is required for MVP release or measured as a staged target.
- Story readiness contract: the exact conditions that make a generated story valid enough for the next workflow step.
- Failure classification rules for recoverable, retryable, human-blocked, and terminal failures.

## Vision

If successful, Hexalith.AI.Tools becomes the internal automation backbone for BMAD-based application development. New Hexalith projects inherit a proven delivery loop by adding one submodule, while `Hexalith.AI.Project` continuously improves the shared tooling.

Over two to three years, the toolchain can grow from create-story automation into a governed internal development operating system: story execution, multi-agent review, test automation, CI repair, release preparation, retrospectives, and cross-runtime validation coordinated through BMAD-aware agents that work across Codex, Claude Code, and future compatible coding tools.
