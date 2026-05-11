---
stepsCompleted: [1, 2, 3, 4, 5, 6]
inputDocuments: ['https://gemini.google.com/share/2ac740e81d6f']
workflowType: 'research'
lastStep: 6
research_type: 'technical'
research_topic: 'Codex and Claude Code plugin, skill, agent, and command compatibility'
research_goals: 'Identify latest best practices for creating plugins, skills, agents, and commands compatible with both Codex and Claude Code.'
user_name: 'Jerome'
date: '2026-04-26'
web_research_enabled: true
source_verification: true
---

# Research Report: technical

**Date:** 2026-04-26
**Author:** Jerome
**Research Type:** technical

---

## Research Overview

This technical research evaluates how to create plugins, skills, agents, and command-like workflows that are compatible with both OpenAI Codex and Claude Code. The research scope covers shared standards, platform-specific adapters, plugin packaging, subagent formats, MCP integration boundaries, security controls, validation practices, and release operations. The work uses current primary documentation from OpenAI, Anthropic/Claude Code, the Agent Skills specification, the Model Context Protocol specification, and OWASP LLM security guidance, with unresolved third-party input tracked separately in the Additional Input Sources section.

The central finding is that cross-tool compatibility should be designed as a shared core with thin adapters: author reusable workflows as Agent Skills, keep tool integrations behind MCP where live systems or side effects are required, and generate or maintain separate Codex and Claude Code manifests for packaging and distribution. Agents, hooks, monitors, app integrations, and legacy command files should be treated as platform-specific projections rather than portable source-of-truth behavior. See the Research Synthesis section for the final executive summary, decision framework, roadmap, and source verification notes.

## Additional Input Sources

- https://gemini.google.com/share/2ac740e81d6f - Added at user request on 2026-04-26. Content was not accessible during automated fetch; Google returned a consent/sign-in interstitial, so this source is tracked as unresolved until the share text is provided or access is available.

---

<!-- Content will be appended sequentially through research workflow steps -->

## Technical Research Scope Confirmation

**Research Topic:** Codex and Claude Code plugin, skill, agent, and command compatibility
**Research Goals:** Identify latest best practices for creating plugins, skills, agents, and commands compatible with both Codex and Claude Code.

**Technical Research Scope:**

- Architecture Analysis - design patterns, frameworks, system architecture
- Implementation Approaches - development methodologies, coding patterns
- Technology Stack - languages, frameworks, tools, platforms
- Integration Patterns - APIs, protocols, interoperability
- Performance Considerations - scalability, optimization, patterns

**Research Methodology:**

- Current web data with rigorous source verification
- Multi-source validation for critical technical claims
- Confidence level framework for uncertain information
- Comprehensive technical coverage with architecture-specific insights

**Scope Confirmed:** 2026-04-26

## Technology Stack Analysis

### Programming Languages

The compatibility layer is primarily document- and manifest-driven, not application-framework-driven. The portable core should be written in Markdown plus YAML frontmatter because both Codex and Claude Code consume `SKILL.md` skills using the Agent Skills model. The Agent Skills specification defines a skill as a directory with required `SKILL.md` metadata and instructions, plus optional `scripts/`, `references/`, and `assets/` folders. It requires `name` and `description`, constrains skill names to lowercase alphanumerics and hyphens, and recommends keeping the main `SKILL.md` concise while moving large material into referenced files. Source: https://agentskills.io/specification

JSON is the shared plugin-manifest language. Codex requires `.codex-plugin/plugin.json` as the plugin entry point and can reference skills, MCP servers, apps, and interface assets from that manifest. Claude Code uses `.claude-plugin/plugin.json` for plugin metadata and component paths, and `.claude-plugin/marketplace.json` for distribution catalogs. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugins-reference

TOML is Codex-specific for project and custom subagent configuration. Codex custom agents are standalone TOML files under `~/.codex/agents/` or `.codex/agents/`, with global agent settings under `[agents]`. Claude Code subagents are Markdown files with YAML frontmatter under `.claude/agents/` or `~/.claude/agents/`. Source: https://developers.openai.com/codex/subagents and https://docs.anthropic.com/en/docs/claude-code/sub-agents

_Popular Languages:_ Markdown, YAML frontmatter, JSON, TOML, shell scripts, Python, and JavaScript/TypeScript.
_Emerging Languages:_ Not a language trend; the trend is toward open, file-based agent manifests.
_Language Evolution:_ Claude Code custom commands have been merged into skills while keeping `.claude/commands/` compatibility; Codex exposes enabled skills in slash command lists and direct `$skill` invocation rather than a separate cross-surface custom command format. Source: https://code.claude.com/docs/en/slash-commands and https://developers.openai.com/codex/app/commands
_Performance Characteristics:_ Keep activation metadata short. Codex caps its initial skill list to roughly 2% of model context or 8,000 characters when unknown, then loads the full `SKILL.md` only after selecting a skill. Source: https://developers.openai.com/codex/skills

### Development Frameworks and Libraries

The main framework is the open Agent Skills standard. Both OpenAI and Claude Code document skills as portable, progressively loaded instruction packages with optional scripts and supporting files. Claude Code explicitly states its skills follow the Agent Skills open standard and extend it with invocation controls, subagent execution, and dynamic context injection. Codex documents skills as task-specific packages that build on the same open standard. Source: https://code.claude.com/docs/en/slash-commands and https://developers.openai.com/codex/skills

MCP is the integration framework for external tools, live data, and reusable prompt templates. The MCP specification defines three server primitives: prompts, resources, and tools. Its current server overview classifies prompts as user-controlled, resources as application-controlled, and tools as model-controlled. This maps cleanly to cross-agent design: prompts can become slash-command-like workflows, resources provide context, and tools perform actions. Source: https://modelcontextprotocol.io/specification/2025-11-25/server/index

_Major Frameworks:_ Agent Skills for procedural workflows; MCP for tool/resource/prompt integration; AGENTS.md for repository-level operating instructions.
_Micro-frameworks:_ Flat `.claude/commands/*.md` remains a Claude-compatible legacy/shortcut command format, but new portable commands should be modeled as skills.
_Evolution Trends:_ Skills are becoming the shared compatibility unit; plugins are becoming the packaging and distribution unit.
_Ecosystem Maturity:_ High for documented skill/plugin patterns in both tools; medium for plugin cross-compatibility because Codex and Claude use different plugin manifests and namespaces.

### Database and Storage Technologies

There is no database requirement for cross-compatible skills or plugins. The storage model is filesystem-first and version-control-friendly.

Codex reads repository skills from `.agents/skills` along the path from current directory up to the repository root; it also supports user/admin/system locations. Claude Code reads project skills from `.claude/skills/<skill-name>/SKILL.md`, personal skills from `~/.claude/skills/`, enterprise-managed skills, and plugin-bundled skills. Source: https://developers.openai.com/codex/skills and https://code.claude.com/docs/en/slash-commands

For plugins, both ecosystems use local marketplaces and git-backed catalogs. Codex can read marketplace files from the official directory, `$REPO_ROOT/.agents/plugins/marketplace.json`, `$REPO_ROOT/.claude-plugin/marketplace.json`, and `~/.agents/plugins/marketplace.json`. Claude Code marketplace files live at `.claude-plugin/marketplace.json` and can be distributed by GitHub, git URL, remote JSON URL, or local path. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugin-marketplaces

_Relational Databases:_ Not applicable for the extension package itself.
_NoSQL Databases:_ Not applicable; JSON manifests are files, not a runtime data store.
_In-Memory Databases:_ Not applicable.
_Data Warehousing:_ Not applicable.
_Storage Recommendation:_ Commit portable skills to both `.agents/skills/<name>/SKILL.md` and `.claude/skills/<name>/SKILL.md` when you need native project discovery in both tools; package distributable bundles with both `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json` when maintaining a shared plugin source tree.

### Development Tools and Platforms

Codex authoring tools include `$skill-creator` for skills and `$plugin-creator` for plugins. OpenAI recommends `$plugin-creator` for scaffolding `.codex-plugin/plugin.json` and a local marketplace entry, and documents manual plugin structure when needed. Source: https://developers.openai.com/codex/plugins/build

Claude Code authoring tools include `/agents` for subagent creation and management, `/plugin` for plugin management, `claude plugin validate .` or `/plugin validate .` for plugin validation, and `/skill-name` invocation for skills. Claude Code’s marketplace validator checks `plugin.json`, skill/agent/command frontmatter, and hook schema. Source: https://docs.anthropic.com/en/docs/claude-code/sub-agents and https://code.claude.com/docs/en/plugin-marketplaces

_IDE and Editors:_ Any editor works because the assets are Markdown, JSON, YAML, and TOML.
_Version Control:_ Git is the natural source of truth. AGENTS.md explicitly targets version-controlled repository guidance and nested monorepo instructions. Source: https://agents.md/
_Build Systems:_ Usually none. Add scripts only when deterministic automation improves reliability.
_Testing Frameworks:_ Use `skills-ref validate ./my-skill` for Agent Skills conformance where available; use `claude plugin validate .` for Claude plugin packaging. For Codex plugins, test through a repo or local marketplace and install from `/plugins` or `codex plugin marketplace add`.

### Cloud Infrastructure and Deployment

Distribution is git- and marketplace-oriented. Claude Code supports marketplaces hosted in GitHub, arbitrary git repositories, direct `marketplace.json` URLs, and local directories; marketplace sources can be installed at user, project, or local scope. Codex similarly supports GitHub shorthand, HTTPS/SSH Git URLs, local marketplace roots, optional `--ref` pinning, and sparse paths for Git-backed marketplaces. Source: https://code.claude.com/docs/en/plugin-marketplaces and https://developers.openai.com/codex/plugins/build

For tool integrations, use MCP servers rather than tool-specific prompt instructions when the agent needs live systems, external APIs, or reusable typed actions. MCP is standardized around resources, prompts, and tools, and its security model requires explicit user consent and control for data access and operations. Source: https://modelcontextprotocol.io/specification/2025-11-25

_Major Cloud Providers:_ Not directly relevant unless the plugin bundles cloud-specific MCP servers or deployment scripts.
_Container Technologies:_ Useful for reproducible MCP servers or validators, but not required for skill/plugin packaging.
_Serverless Platforms:_ Useful when hosting remote MCP servers, not for local skills.
_CDN and Edge Computing:_ Usually irrelevant except for public documentation, assets, or hosted marketplace JSON.

### Technology Adoption Trends

The strongest interoperability trend is convergence on three layers:

1. `AGENTS.md` for repository-level, agent-readable project guidance. The public AGENTS.md site describes it as a simple open format, separate from README content, with nested files for monorepos and closest-file precedence. Source: https://agents.md/
2. Agent Skills for task-specific reusable procedures. Both Codex and Claude Code document the same `SKILL.md`-centered structure and progressive-disclosure model. Source: https://developers.openai.com/codex/skills and https://code.claude.com/docs/en/slash-commands
3. MCP for integrations that need live data, tools, or reusable prompt templates. Source: https://modelcontextprotocol.io/specification/2025-11-25/server/index

_Migration Patterns:_ Convert repeated prompts and legacy `.claude/commands/*.md` files into `SKILL.md` skills first. Keep `.claude/commands/` only for backward compatibility or Claude-only convenience. Use plugins only after the workflow needs versioned distribution, bundled MCP/app integration, or team/community sharing.
_Emerging Technologies:_ Cross-agent plugin marketplaces are emerging, but Codex and Claude still require different manifest paths and some different metadata fields.
_Legacy Technology:_ Tool-specific command files are becoming less strategic than skills. Claude keeps command compatibility, while Codex app/CLI surfaces focus on built-in slash controls and skill invocation.
_Community Trends:_ File-based standards are winning because they are reviewable, portable, and easy to package: `AGENTS.md`, `SKILL.md`, JSON plugin manifests, and MCP config.

## Integration Patterns Analysis

### API Design Patterns

The cross-compatible API design should be layered, not monolithic:

1. **Instruction API:** `AGENTS.md` for repository-wide operating rules.
2. **Workflow API:** `SKILL.md` for reusable procedures and commands.
3. **Packaging API:** plugin manifests and marketplace catalogs.
4. **Tool API:** MCP servers for live external systems.

Codex and Claude Code both support skills as the reusable workflow layer, but they expose invocation differently. Codex supports explicit skill invocation through `$` mentions and `/skills`, and can implicitly select a skill from its description. Claude Code exposes skills as slash commands such as `/skill-name`, while plugin skills are namespaced as `/plugin-name:skill-name`. Source: https://developers.openai.com/codex/skills and https://code.claude.com/docs/en/slash-commands

_RESTful APIs:_ Use REST only behind MCP tools or helper scripts. Do not expose REST details directly in a skill unless the skill’s job is to call a specific API.
_GraphQL APIs:_ Same pattern as REST: wrap in MCP or deterministic scripts when the agent needs live data.
_RPC and gRPC:_ MCP is the relevant agent-facing RPC layer; the current MCP specification uses JSON-RPC 2.0, stateful connections, and capability negotiation between host, client, and server. Source: https://modelcontextprotocol.io/specification/2025-11-25
_Webhook Patterns:_ Claude Code hooks provide event-driven extension points. They can be command, HTTP, MCP tool, prompt, or agent hooks depending on event type. Codex also documents hooks in its configuration area, but a shared plugin should treat hooks as tool-specific extensions, not portable core behavior. Source: https://code.claude.com/docs/en/hooks

### Communication Protocols

For tool interoperability, MCP is the primary protocol. The specification defines hosts, clients, and servers, with servers providing resources, prompts, and tools, and clients optionally providing sampling, roots, and elicitation. This is the only integration protocol in the researched sources that is explicitly designed to span multiple AI applications. Source: https://modelcontextprotocol.io/specification/2025-11-25

For package discovery, both tools use marketplace catalogs but with different schemas and paths. Codex reads repo marketplaces from `$REPO_ROOT/.agents/plugins/marketplace.json` and can also read Claude-style `$REPO_ROOT/.claude-plugin/marketplace.json`. Claude Code uses `.claude-plugin/marketplace.json` and supports GitHub shorthand, git URLs, remote JSON URLs, and local paths. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugin-marketplaces

_HTTP/HTTPS Protocols:_ Use for remote marketplace sources and remote MCP transports where supported.
_WebSocket Protocols:_ Not central to plugin/skill compatibility based on the current primary docs.
_Message Queue Protocols:_ Not part of the shared plugin/skill layer; expose queue operations through MCP tools if needed.
_gRPC and Protocol Buffers:_ Not the compatibility baseline here. Prefer MCP JSON-RPC unless a backing service already uses gRPC.

### Data Formats and Standards

The shared data formats are:

- Markdown for `AGENTS.md`, `SKILL.md`, Claude command files, and Claude subagents.
- YAML frontmatter for Agent Skills and Claude subagents.
- JSON for plugin manifests, marketplace catalogs, MCP config, hooks, and Claude plugin settings.
- TOML for Codex custom agent definitions and Codex config.

Agent Skills define the most portable workflow format: `SKILL.md` must contain YAML frontmatter followed by Markdown, with `name` and `description` required and optional `scripts/`, `references/`, and `assets/` directories. Source: https://agentskills.io/specification

Claude Code plugin reference identifies default component locations: `skills/`, `commands/`, `agents/`, `hooks/hooks.json`, `.mcp.json`, `.lsp.json`, `bin/`, and `settings.json`, all at plugin root except `.claude-plugin/plugin.json`. Source: https://code.claude.com/docs/en/plugins-reference

_JSON and XML:_ JSON is required for plugin and marketplace metadata. XML is not relevant to current compatibility patterns.
_Protobuf and MessagePack:_ Not used in the primary compatibility layer.
_CSV and Flat Files:_ Useful for bundled reference data in `assets/`, but not a standard extension format.
_Custom Data Formats:_ Avoid inventing custom formats unless they are consumed by deterministic helper scripts. Prefer documented JSON schemas and Markdown conventions.

### System Interoperability Approaches

The safest interoperability approach is **shared core, thin adapters**:

- Put reusable procedural behavior in standards-compliant `skills/<name>/SKILL.md`.
- Mirror or generate that skill into `.agents/skills/<name>/SKILL.md` and `.claude/skills/<name>/SKILL.md` for repository-native discovery.
- Package a distributable plugin with both `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json` when one source tree must support both ecosystems.
- Keep tool integrations in MCP servers or plugin-bundled `.mcp.json` instead of hardcoding app-specific calls in the skill body.

Codex plugin docs describe plugins as bundles of skills, app integrations, and MCP servers. Claude Code plugin docs describe plugins as shared extensions containing skills, agents, hooks, MCP servers, and LSP servers. The overlap is skills plus MCP; agents/hooks/LSP/apps require adapter logic. Source: https://developers.openai.com/codex/plugins and https://code.claude.com/docs/en/plugins

_Point-to-Point Integration:_ Acceptable for a single personal workflow, but brittle for cross-tool distribution.
_API Gateway Patterns:_ MCP server as gateway is the best fit when wrapping multiple APIs behind one tool surface.
_Service Mesh:_ Not applicable to local skill/plugin packaging; potentially useful only for production MCP server fleets.
_Enterprise Service Bus:_ Not relevant to authoring skills; can be hidden behind MCP tools if enterprise systems require it.

### Microservices Integration Patterns

For this domain, “microservice” integration mainly applies to MCP servers and external service connectors. Treat each MCP server as a bounded integration service with explicit capabilities and least-privilege tool definitions. The MCP spec’s server primitives let authors separate user-invoked prompts, contextual resources, and model-invoked tools, which is a better boundary than a single broad tool. Source: https://modelcontextprotocol.io/specification/2025-11-25/server/index

Codex custom agents can include `mcp_servers` and `skills.config` in TOML agent files, inheriting omitted settings from the parent session. Claude Code subagents can inherit MCP tools when the `tools` field is omitted or can list specific tools. This means agent-to-tool access must be designed separately per client. Source: https://developers.openai.com/codex/subagents and https://docs.anthropic.com/en/docs/claude-code/sub-agents

_API Gateway Pattern:_ Use one MCP server to front a coherent external domain, such as GitHub, Jira, or internal docs.
_Service Discovery:_ Marketplace catalogs discover plugins; MCP clients discover server capabilities.
_Circuit Breaker Pattern:_ Implement in MCP servers or scripts, not in prompt-only skills.
_Saga Pattern:_ Avoid asking the model to coordinate irreversible multi-system transactions purely from instructions. Use explicit tools, confirmations, and rollback-aware scripts.

### Event-Driven Integration

Event-driven integration is mostly Claude-specific today. Claude Code hooks run outside the model loop and can respond to events such as `PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `FileChanged`, `InstructionsLoaded`, and MCP elicitation events. Hooks can invoke commands, HTTP endpoints, MCP tools, prompt checks, or agent verifiers depending on event support. Source: https://code.claude.com/docs/en/hooks

For cross-compatible design, keep event automation optional and tool-specific:

- Put the durable workflow in a skill.
- Put deterministic event behavior in Claude hooks only when targeting Claude Code.
- For Codex, prefer explicit skills, MCP tools, or documented Codex hooks where available rather than assuming Claude hook parity.

_Publish-Subscribe Patterns:_ Not a native skill/plugin pattern. Use external infrastructure behind MCP if required.
_Event Sourcing:_ Not applicable to skills; useful only in backing systems.
_Message Broker Patterns:_ Wrap broker access through MCP tools or scripts with clear safety checks.
_CQRS Patterns:_ Not applicable to extension packaging, but MCP can separate read-only resources from mutating tools.

### Integration Security Patterns

Security needs to be designed at every layer because skills and plugins can steer agents toward shell commands, external services, and tool calls.

MCP’s current specification explicitly warns that the protocol enables arbitrary data access and code execution paths. It requires user consent and control for data access and operations, consent before exposing user data to servers, caution around tool descriptions because they can be untrusted, and explicit approval for tool invocation and LLM sampling requests. Source: https://modelcontextprotocol.io/specification/2025-11-25

Claude Code plugin installation copies marketplace plugins into a local plugin cache rather than using them in place, and installed plugins cannot reference files outside their directory through path traversal. Claude documents `${CLAUDE_PLUGIN_ROOT}` as the right way to reference plugin paths in plugin MCP/hook contexts. Source: https://code.claude.com/docs/en/plugins-reference

_OAuth 2.0 and JWT:_ Use inside MCP servers or external app integrations; do not place tokens in skills or plugin manifests.
_API Key Management:_ Keep secrets in each tool’s supported auth mechanism, environment, or settings layer. Never commit keys in `SKILL.md`, plugin manifests, or marketplace catalogs.
_Mutual TLS:_ Relevant for enterprise MCP servers, not local skills.
_Data Encryption:_ Use HTTPS for remote marketplaces and MCP HTTP transports; rely on platform auth for app integrations.

### Integration Quality Assessment

Confidence is high for the core recommendation: **skills + MCP + AGENTS.md** are the stable cross-tool primitives. Confidence is medium for dual plugin packaging because both Codex and Claude Code document plugin manifests and marketplaces, but their manifests are not identical and their plugin component sets differ. Confidence is low for a single universal “agent” file because Codex uses TOML custom agent files while Claude Code uses Markdown/YAML subagent files.

## Architectural Patterns and Design

### System Architecture Patterns

The recommended architecture is a shared-core plus thin-adapter pattern. The shared core contains portable instructions and capabilities: `AGENTS.md` for repository-wide operating guidance, `skills/<name>/SKILL.md` for reusable workflows, optional `scripts/`, `references/`, and `assets/` inside each skill, and MCP servers for executable tools, live data, and prompt/resource surfaces. Vendor-specific adapters then expose that core through `.codex-plugin/plugin.json` for Codex and `.claude-plugin/plugin.json` for Claude Code.

This pattern matches the current platform split. Codex documents `SKILL.md` skills as the authoring format for reusable workflows and plugins as the installable distribution unit; it requires `.codex-plugin/plugin.json` and keeps `skills/`, `.mcp.json`, `.app.json`, and `assets/` at the plugin root. Claude Code documents plugins as self-contained directories of skills, agents, hooks, MCP servers, LSP servers, and monitors, with `.claude-plugin/plugin.json` as the manifest and all component directories at the plugin root. Source: https://developers.openai.com/codex/skills, https://developers.openai.com/codex/plugins/build, https://code.claude.com/docs/en/plugins, https://code.claude.com/docs/en/plugins-reference

The trade-off is deliberate duplication at the manifest layer. A single universal plugin manifest is not currently documented by either vendor. The lowest-risk architecture is therefore not to force one manifest, but to generate or maintain two small manifests from one source package. Codex additionally reads Claude-style marketplaces at `$REPO_ROOT/.claude-plugin/marketplace.json`, which helps distribution, but Codex still requires its own `.codex-plugin/plugin.json` for Codex-native plugin packages. Source: https://developers.openai.com/codex/plugins/build

### Design Principles and Best Practices

Design skills as narrow, progressively loaded capability packages. Codex states that skills use progressive disclosure: the initial context receives each skill's name, description, and file path, and the full `SKILL.md` is loaded only when selected. Codex also recommends focused skills, explicit inputs and outputs, imperative steps, and scripts only when deterministic behavior or external tooling is needed. Claude Code states that its skills follow the open Agent Skills standard and extend it with invocation controls, subagent execution, and dynamic context injection. Source: https://developers.openai.com/codex/skills and https://code.claude.com/docs/en/slash-commands

For cross-tool compatibility, keep the portable `SKILL.md` subset conservative: YAML frontmatter, clear description, task-specific instructions, and optional support folders. Put Claude-specific controls such as `disable-model-invocation` or dynamic context assumptions behind Claude adapter files or document them as optional enhancements. Put Codex-specific policy and dependency metadata behind Codex adapter files when the same skill must remain portable to Claude Code and other Agent Skills hosts. Source: https://agentskills.io/specification, https://developers.openai.com/codex/skills, https://code.claude.com/docs/en/slash-commands

Use `AGENTS.md` for project-level conventions and operating rules, not for reusable plugin behavior. The AGENTS.md format is a cross-agent Markdown convention for setup commands, tests, code style, security considerations, and nested monorepo guidance. It complements skills but should not replace them: `AGENTS.md` tells agents how to work in a repository; skills tell agents how to execute a repeatable workflow. Source: https://agents.md/

### Scalability and Performance Patterns

The key scalability constraint is context budget, not throughput. Large plugin ecosystems can flood the initial skill list if descriptions are verbose or overlapping. Codex explicitly caps the initial available-skill list at roughly 2 percent of model context or 8,000 characters when the context window is unknown, shortening descriptions first and potentially omitting some skills. The architectural response is to keep descriptions short, make trigger conditions precise, and move long examples or reference material into `references/` files loaded only after activation. Source: https://developers.openai.com/codex/skills and https://agentskills.io/specification

For many related workflows, prefer a small number of composable skills over dozens of near-duplicates. Split only when invocation intent, required tools, or validation flow differs materially. For recurring specialist work, use subagents rather than bloating a general skill. Codex describes narrow custom agents with clear jobs and tool surfaces; Claude Code describes subagents as independent-context assistants for side tasks that would otherwise flood the main conversation. Source: https://developers.openai.com/codex/subagents and https://code.claude.com/docs/en/sub-agents

For runtime scalability, isolate executable behavior in MCP servers or scripts with stable interfaces. Skills should orchestrate; MCP tools should execute. This keeps prompt content small and makes failures observable through tool errors, logs, and validation commands rather than through ambiguous instruction drift. Source: https://modelcontextprotocol.io/specification/2025-11-25 and https://modelcontextprotocol.io/specification/2025-11-25/server/index

### Integration and Communication Patterns

MCP should be the primary integration boundary for any capability that needs external state, APIs, file-backed resources, reusable prompts, or side-effecting tools. The current MCP specification defines hosts, clients, and servers over JSON-RPC 2.0, with server primitives for resources, prompts, and tools. The server overview maps prompts to user-controlled templates, resources to application-controlled context, and tools to model-controlled executable functions. That control hierarchy is a useful architectural rule: make commands and prompts user-visible, make resources read-oriented context, and make tools explicit side-effect boundaries. Source: https://modelcontextprotocol.io/specification/2025-11-25 and https://modelcontextprotocol.io/specification/2025-11-25/server/index

For commands, the portable design is command-as-entrypoint, skill-as-workflow. Claude Code now presents skills as the primary prompt-based extension model and keeps command-style plugin support, including flat Markdown `commands/`, for compatibility and shortcuts. Codex exposes skills in slash-command contexts and supports explicit `$skill` invocation. A dual-compatible package should therefore author the workflow once as a skill and add command shims only where a platform expects or benefits from command discovery. Source: https://code.claude.com/docs/en/slash-commands, https://code.claude.com/docs/en/plugins-reference, and https://developers.openai.com/codex/app/commands

For agents, treat subagents as adapter-specific projections of a shared role definition. Claude plugin agents are Markdown files in `agents/` with YAML frontmatter; project and user agents live in `.claude/agents/` or `~/.claude/agents/`. Codex custom agents are TOML files under `.codex/agents/` or `~/.codex/agents/`, with model, sandbox, tools, instructions, and optional MCP servers. A portable authoring source can define name, description, prompt, intended tools, and safety posture, then render Claude Markdown and Codex TOML variants. Source: https://code.claude.com/docs/en/sub-agents and https://developers.openai.com/codex/subagents

### Security Architecture Patterns

Use least privilege as a packaging rule. A skill that only explains or reviews should not depend on write-capable MCP tools. A subagent that only maps code should be read-only where the host supports sandboxing or tool restrictions. Codex custom-agent examples use narrow tool surfaces and read-only sandboxes for review and documentation roles. Claude Code supports subagent `tools` and `disallowedTools`, but plugin-shipped agents do not support `hooks`, `mcpServers`, or `permissionMode` for security reasons. Source: https://developers.openai.com/codex/subagents and https://code.claude.com/docs/en/sub-agents

Treat MCP servers, hooks, monitors, and scripts as high-risk extension points. The MCP specification emphasizes explicit user consent, data privacy, tool safety, and sampling controls. Its security guidance covers confused-deputy risks, token passthrough, SSRF, session hijacking, local server compromise, and scope minimization. OWASP identifies prompt injection as a top LLM application risk, which is directly relevant when agent instructions can trigger tools or process untrusted repository, web, or issue content. Source: https://modelcontextprotocol.io/specification/2025-11-25, https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices, and https://owasp.org/www-project-top-10-for-large-language-model-applications/

For Claude-specific hooks and monitors, default to opt-in and transparent behavior. Claude plugin hooks can run commands, HTTP calls, MCP tools, prompts, or agents on lifecycle events, and monitors can run unsandboxed background commands for interactive CLI sessions. Those are powerful operational features, but they are not portable to Codex plugin architecture and should be isolated in the Claude adapter with clear documentation and validation. Source: https://code.claude.com/docs/en/plugins-reference and https://code.claude.com/docs/en/hooks

### Data Architecture Patterns

Keep source-of-truth data declarative and version controlled. Recommended package layout:

```text
plugin-root/
  AGENTS.md
  skills/
    workflow-name/
      SKILL.md
      references/
      scripts/
      assets/
  mcp/
  .mcp.json
  .codex-plugin/plugin.json
  .claude-plugin/plugin.json
  .agents/plugins/marketplace.json
  .claude-plugin/marketplace.json
  agents-src/
  agents-codex/
  agents-claude/
```

Use manifest files as indexes into this shared tree, not as the source of behavior. Codex manifest fields point to `skills`, `mcpServers`, and `apps` relative to the plugin root. Claude manifest fields point to `skills`, `commands`, `agents`, `hooks`, `mcpServers`, `lspServers`, `monitors`, and related components. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugins-reference

For persistent or mutable data, prefer host-approved storage mechanisms or user-configured paths. Claude plugins provide variables such as `${CLAUDE_PLUGIN_ROOT}` and `${CLAUDE_PLUGIN_DATA}` for file resolution and persistent data. Codex plugins are installed into a cache path and loaded from the installed copy rather than directly from marketplace source, so plugin authors should not rely on editing the source checkout at runtime. Source: https://code.claude.com/docs/en/plugins-reference and https://developers.openai.com/codex/plugins/build

### Deployment and Operations Architecture

Use a two-track release process: validate the portable core, then validate each adapter. For the core, check `SKILL.md` frontmatter, concise descriptions, referenced files, scripts, and MCP server behavior. For Codex, verify `.codex-plugin/plugin.json`, repo or personal marketplace entries, relative path rules, and plugin install behavior. For Claude Code, verify `.claude-plugin/plugin.json`, plugin namespacing, `/plugin validate` or `claude plugin validate`, local `--plugin-dir` testing, and `/reload-plugins` behavior during development. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugins

Versioning should be explicit. Claude Code notes that if plugin `version` is omitted for git-distributed plugins, commit SHA behavior can make every commit a new version; Codex examples and manifest guidance also use stable `name`, `version`, and metadata for install surfaces. Cross-compatible plugins should use semantic versioning in both manifests and keep marketplace entries pinned or intentionally upgraded. Source: https://code.claude.com/docs/en/plugins and https://developers.openai.com/codex/plugins/build

Operationally, test installation in clean user-level and project-level scopes. Claude supports user, project, local, and managed plugin scopes. Codex supports official, repo, Claude-style repo, and personal marketplace sources, and stores plugin enablement in Codex configuration. This means compatibility testing must cover both discovery and runtime invocation, not just file layout. Source: https://code.claude.com/docs/en/plugins-reference and https://developers.openai.com/codex/plugins/build

## Implementation Approaches and Technology Adoption

### Technology Adoption Strategies

Adopt in three phases: local skill, shared plugin, then managed marketplace. Codex explicitly recommends starting with a local skill while iterating on one repo or personal workflow, then building a plugin when the workflow needs sharing, bundled app integrations, bundled MCP config, or stable distribution. Claude Code similarly recommends starting with standalone `.claude/` configuration for quick iteration and converting to a plugin when the extension should be versioned, shared, reused across projects, or distributed through a marketplace. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugins

The first production milestone should be a standards-compliant skill that works without plugin packaging. The Agent Skills specification defines the minimum portable unit: a directory with `SKILL.md`, YAML frontmatter, required `name` and `description`, and optional `scripts/`, `references/`, and `assets/`. That gives teams a low-friction adoption path before committing to Codex- and Claude-specific marketplace mechanics. Source: https://agentskills.io/specification

For migration from existing Claude commands or ad hoc prompts, convert repeated workflows into `skills/<name>/SKILL.md` first. Keep legacy `.claude/commands/*.md` files only as compatibility shims or shortcuts. For Codex, package the skill through `.codex-plugin/plugin.json` and a repo or personal marketplace when the workflow needs installation and discovery. Source: https://code.claude.com/docs/en/plugins and https://developers.openai.com/codex/plugins/build

### Development Workflows and Tooling

Use a source tree that keeps portable content and adapters visibly separate:

```text
plugin-root/
  skills/
    my-workflow/
      SKILL.md
      scripts/
      references/
      assets/
  .mcp.json
  .codex-plugin/plugin.json
  .claude-plugin/plugin.json
  .agents/plugins/marketplace.json
  .claude-plugin/marketplace.json
```

Codex plugin implementation requires `.codex-plugin/plugin.json` and recommends plugin-root-relative paths beginning with `./` for `skills`, `apps`, and `mcpServers`. Claude Code requires only `name` if a `.claude-plugin/plugin.json` manifest is present, but its full schema can reference skills, commands, agents, hooks, MCP servers, LSP servers, monitors, themes, dependencies, user config, and channels. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugins-reference

Development should run adapter-specific smoke tests. For Codex, add the plugin to a local marketplace, restart Codex, verify it appears, install it, and invoke the bundled skill. Codex notes that local plugin changes require updating the marketplace target directory and restarting Codex so the local install picks up the new files. For Claude Code, test with `claude --plugin-dir ./my-plugin`, try `/plugin-name:skill-name`, verify agents in `/agents`, verify hooks where present, and use `/reload-plugins` while iterating. Source: https://developers.openai.com/codex/plugins/build and https://code.claude.com/docs/en/plugins

### Testing and Quality Assurance

Testing should cover four layers:

1. Skill validation: verify `SKILL.md` frontmatter, name matching, description quality, relative file references, and progressive disclosure structure.
2. Manifest validation: verify `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json` independently.
3. Runtime discovery: verify skill appears and invokes correctly in both Codex and Claude Code.
4. Capability tests: verify MCP tools, scripts, hooks, agents, and marketplace install behavior in clean environments.

The Agent Skills spec recommends validating skills with the reference library using `skills-ref validate ./my-skill`. Claude Code documents `claude plugin validate` or `/plugin validate` for plugin JSON, skill/agent/command frontmatter, and hook syntax/schema errors. Codex documents manual marketplace installation and restart verification as the practical local test path. Source: https://agentskills.io/specification, https://code.claude.com/docs/en/plugins-reference, and https://developers.openai.com/codex/plugins/build

Quality gates should include fixture prompts that intentionally test implicit invocation boundaries. Codex recommends testing prompts against the skill description to confirm correct trigger behavior and keeping skills focused on one job. This matters for both tools because over-broad descriptions can cause unwanted invocation and context bloat. Source: https://developers.openai.com/codex/skills

### Deployment and Operations Practices

Use semantic versioning in both plugin manifests and keep release notes tied to changes in skills, MCP tools, hooks, agents, and command shims. Claude Code states that if `version` is omitted for git-distributed plugins, the git commit SHA can become the version behavior, which may cause every commit to be treated as an update. Codex plugin examples also use explicit `name`, `version`, and metadata. Source: https://code.claude.com/docs/en/plugins and https://developers.openai.com/codex/plugins/build

For Codex distribution, maintain `.agents/plugins/marketplace.json` for repo-scoped plugin lists or `~/.agents/plugins/marketplace.json` for personal plugin lists. Codex marketplace entries point at plugin folders with relative `./` paths from the marketplace root, and Codex can add marketplace sources through `codex plugin marketplace add`. Source: https://developers.openai.com/codex/plugins/build

For Claude Code distribution, use plugin marketplaces and test installation scopes: `user`, `project`, `local`, and `managed`. Project-scoped plugins are the right default for team-shared repository behavior; user scope is better for personal workflows; local scope is useful for project-specific private setup; managed scope is organization-controlled. Source: https://code.claude.com/docs/en/plugins-reference

### Team Organization and Skills

Treat plugin creation as a cross-functional ownership area, not only prompt authoring. Teams need:

- Skill authors who can write concise procedural instructions and examples.
- Integration engineers who can wrap external systems behind MCP servers or deterministic scripts.
- Security reviewers who understand tool permissions, token handling, prompt injection, and consent flows.
- Release owners who validate both Codex and Claude packaging.
- Documentation owners who maintain README, marketplace metadata, version notes, and usage examples.

The skill authoring role should understand Agent Skills progressive disclosure: metadata is loaded broadly, `SKILL.md` loads on activation, and references/assets load on demand. The integration role should understand MCP hosts, clients, servers, resources, prompts, and tools. Source: https://agentskills.io/specification and https://modelcontextprotocol.io/specification/2025-11-25

### Cost Optimization and Resource Management

The main cost control is context discipline. Keep descriptions precise and short, keep `SKILL.md` under the recommended size, and move long material to `references/`. Agent Skills recommends keeping main `SKILL.md` under 500 lines and loading supporting resources only when needed. Codex also warns that its initial skill list is context-budgeted, so overly verbose or numerous skills can reduce discoverability. Source: https://agentskills.io/specification and https://developers.openai.com/codex/skills

Use MCP tools for operations that require live data or repeatable execution instead of embedding long API instructions in skill prose. This reduces prompt size, centralizes retries and errors, and keeps secrets out of instructions. When a workflow can be deterministic, put that logic in `scripts/` with clear errors and documented dependencies. Source: https://agentskills.io/specification and https://modelcontextprotocol.io/specification/2025-11-25

Avoid duplicating the same skill through both standalone and plugin paths in a single environment. Claude Code plugin migration guidance notes that after migrating from `.claude/` configuration to a plugin, the original files can be removed to avoid duplicates and the plugin version takes precedence when loaded. Source: https://code.claude.com/docs/en/plugins

### Risk Assessment and Mitigation

The highest risks are security boundary mistakes, silent compatibility drift, and invocation ambiguity. MCP introduces powerful arbitrary data access and code execution paths; the MCP spec calls for explicit user consent, data privacy controls, tool safety, and sampling controls. Its security guidance covers confused deputy attacks, token passthrough, SSRF, session hijacking, local MCP server compromise, and scope minimization. Source: https://modelcontextprotocol.io/specification/2025-11-25 and https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices

Mitigations:

- Do not store secrets in skills, plugin manifests, marketplace JSON, examples, or reference files.
- Use least-privilege MCP scopes and tool surfaces.
- Keep mutating tools separate from read-only resources.
- Require explicit confirmation for destructive workflows.
- Review hooks and monitors as executable code, especially in Claude Code where they can run commands or background processes.
- Validate both plugin manifests in CI and run smoke tests in clean Codex and Claude environments.
- Track a compatibility matrix for every component: portable, Codex-only, Claude-only, or unsupported.

Prompt injection remains a core LLM application risk. OWASP lists prompt injection as a top LLM application security concern, and that risk is directly relevant when skills process untrusted repository content, issue text, web pages, logs, or documents before invoking tools. Source: https://owasp.org/www-project-top-10-for-large-language-model-applications/

## Technical Research Recommendations

### Implementation Roadmap

1. Define the portable workflow as `skills/<name>/SKILL.md` using the Agent Skills spec.
2. Add deterministic helper scripts only where instructions are insufficient.
3. Move large examples and reference material into `references/`.
4. Add MCP only for live data, external APIs, or side-effecting tools.
5. Add `.codex-plugin/plugin.json` and verify Codex local marketplace installation.
6. Add `.claude-plugin/plugin.json` and verify `claude --plugin-dir`, `/reload-plugins`, and plugin validation.
7. Add optional Claude commands, hooks, agents, LSP, or monitors only as adapter-specific enhancements.
8. Add optional Codex custom agents or app integrations only as Codex-specific enhancements.
9. Create CI checks for skill validation, manifest JSON validation, schema/path checks, secret scanning, and smoke-test prompts.
10. Release with semantic versioning and separate Codex/Claude installation instructions.

### Technology Stack Recommendations

Use Markdown plus YAML frontmatter for skills and Claude agents, JSON for plugin manifests and marketplaces, TOML for Codex custom agents, and MCP JSON-RPC for external capabilities. Use Python, Bash, or JavaScript/TypeScript for scripts based on the target environment. Prefer portable `SKILL.md` and MCP boundaries over vendor-specific hooks or command formats when functionality should work in both Codex and Claude Code.

### Skill Development Requirements

Skill authors should be able to write short trigger descriptions, precise step-by-step instructions, examples, edge cases, and references. They should understand progressive disclosure, relative file references, deterministic scripts, and when to split one large skill into several focused skills. Agent authors should understand both Codex TOML agents and Claude Markdown/YAML subagents, because there is no single documented cross-tool agent file format.

### Success Metrics and KPIs

Track:

- Skill activation precision: correct implicit activation and low false positives.
- Cross-tool install success: clean install in Codex and Claude Code.
- Runtime smoke-test pass rate: expected command/skill/agent/MCP behavior works in both tools.
- Context footprint: concise descriptions and bounded `SKILL.md` size.
- Security quality: no committed secrets, least-privilege MCP scopes, no unreviewed mutating tools.
- Release quality: semantic versioning, changelog coverage, validated marketplace metadata.
- Adoption: number of teams/projects using the plugin and number of support issues per release.

## Research Synthesis: Comprehensive Technical Research Document

# Shared Core, Thin Adapters: Comprehensive Technical Research on Codex and Claude Code Plugin, Skill, Agent, and Command Compatibility

## Executive Summary

The compatibility problem is not solved by trying to make one universal plugin manifest. Codex and Claude Code both support extensibility through skills and MCP, but they package, discover, invoke, and secure adjacent concepts differently. The durable architecture is therefore a layered source tree: use Agent Skills as the portable workflow layer, MCP as the portable integration boundary, and separate Codex and Claude Code adapter manifests for distribution, invocation, and runtime-specific capabilities.

The strongest shared standard is `SKILL.md`. Codex documents Agent Skills as task-specific packages of instructions, resources, and optional scripts, built on the open Agent Skills standard. Claude Code also states that its skills follow the Agent Skills standard, while extending it with Claude-specific invocation control, subagent execution, and dynamic context injection. The Agent Skills specification defines the lowest common denominator: a skill directory with a required `SKILL.md` file and optional `scripts/`, `references/`, and `assets/` directories.

The main implementation risk is treating powerful runtime extension points as simple documentation. MCP servers, hooks, monitors, executable scripts, and subagents can cross security boundaries. MCP should be used deliberately for live external systems and side effects, with least privilege, clear consent, and separated read-only versus mutating capabilities. OWASP LLM guidance identifies prompt injection and insecure output handling as top risks, which directly applies when skills or agents process untrusted repositories, issue text, web pages, logs, or documents before invoking tools.

**Key Technical Findings:**

- Agent Skills are the practical shared workflow unit for Codex and Claude Code.
- Codex plugins require `.codex-plugin/plugin.json`; Claude Code uses `.claude-plugin/plugin.json` and supports a wider set of plugin components.
- Codex custom agents are TOML files under `.codex/agents/` or `~/.codex/agents/`; Claude Code subagents are Markdown files with YAML frontmatter under `.claude/agents/`, `~/.claude/agents/`, or plugin `agents/`.
- Commands should be modeled as skill entrypoints for portability. Claude `commands/` remain useful as compatibility shims, but new portable command behavior should live in `SKILL.md`.
- MCP is the correct integration boundary for tools, resources, and prompt-like workflows that need live data or external systems.
- Security and quality gates must validate both the portable core and each platform adapter.

**Technical Recommendations:**

- Author workflows once in `skills/<name>/SKILL.md` using the Agent Skills specification.
- Keep `SKILL.md` concise; move long examples, schemas, and operating notes to `references/`.
- Package Codex and Claude Code separately with `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json`.
- Treat agents, hooks, monitors, app integrations, and legacy commands as adapter-specific generated or maintained artifacts.
- Add CI checks for skill structure, JSON manifests, MCP configuration, secret scanning, and clean-install smoke tests in both tools.

## Table of Contents

1. Technical Research Introduction and Methodology
2. Technical Landscape and Architecture Analysis
3. Implementation Approaches and Best Practices
4. Technology Stack Evolution and Current Trends
5. Integration and Interoperability Patterns
6. Performance and Scalability Analysis
7. Security and Compliance Considerations
8. Strategic Technical Recommendations
9. Implementation Roadmap and Risk Assessment
10. Future Technical Outlook and Innovation Opportunities
11. Technical Research Methodology and Source Verification
12. Technical Appendices and Reference Materials

## 1. Technical Research Introduction and Methodology

### Technical Research Significance

Codex and Claude Code are converging around file-based agent engineering, but not around a single complete extension format. Both systems can consume skills, both can use MCP, and both support specialized agents. Their packaging and invocation models remain different enough that a serious compatibility strategy needs architecture, not only documentation.

This matters because plugin and skill ecosystems are becoming the operational layer for repeatable agent work. Teams are no longer just writing prompts; they are shipping workflows, tool access, project rules, test procedures, and release automation into agent runtimes. A poorly designed extension can create context bloat, invocation ambiguity, tool-permission leakage, or unreviewed executable paths.

_Technical Importance:_ The shared standards layer is mature enough for practical reuse, while the adapter layer still requires explicit Codex and Claude Code handling.
_Business Impact:_ Teams can reduce duplicated prompt engineering and support cost by maintaining one portable workflow core, but only if platform-specific behavior is isolated and tested.
_Sources:_ https://developers.openai.com/codex/skills, https://code.claude.com/docs/en/slash-commands, https://agentskills.io/specification

### Technical Research Methodology

The research used a layered technical analysis:

- **Technical Scope:** skills, plugins, commands, subagents, MCP servers, manifests, validation, security, release operations, and source-tree layout.
- **Data Sources:** primary vendor documentation from OpenAI and Claude Code, the Agent Skills specification, the MCP specification, and OWASP LLM security guidance.
- **Analysis Framework:** identify portable primitives, identify adapter-only primitives, define source-of-truth ownership, then derive implementation, testing, and governance recommendations.
- **Time Period:** current documentation and web verification performed on 2026-04-26.
- **Technical Depth:** architecture, implementation workflow, validation, security, operations, and future compatibility outlook.

### Technical Research Goals and Objectives

**Original Technical Goals:** Identify latest best practices for creating plugins, skills, agents, and commands compatible with both Codex and Claude Code.

**Achieved Technical Objectives:**

- Established `SKILL.md` as the portable workflow core.
- Identified MCP as the portable integration boundary for tools, resources, and prompt templates.
- Mapped Codex plugin, skill, and custom-agent formats against Claude Code plugin, skill, command, and subagent formats.
- Produced a compatibility architecture that separates shared behavior from platform-specific adapters.
- Defined validation, security, and release controls for dual-runtime extension packages.

## 2. Technical Landscape and Architecture Analysis

### Current Technical Architecture Patterns

The most reliable architecture is **shared core, thin adapters**:

```text
plugin-root/
  AGENTS.md
  skills/
    workflow-name/
      SKILL.md
      references/
      scripts/
      assets/
  .mcp.json
  .codex-plugin/plugin.json
  .claude-plugin/plugin.json
  agents-src/
  agents-codex/
  agents-claude/
```

The shared core contains the reusable procedure and supporting files. Codex and Claude Code adapters reference that core through their own manifest and runtime conventions. This avoids pretending that Codex TOML agents and Claude Markdown/YAML subagents are the same artifact.

_Dominant Patterns:_ `SKILL.md` for workflow behavior, JSON manifests for packaging, MCP for tool access, and adapter-specific files for agents and commands.
_Architectural Evolution:_ Extension systems are moving from ad hoc prompts toward reviewable file-based packages.
_Architectural Trade-offs:_ One source tree improves maintainability, but adapter validation remains mandatory because runtime behavior differs.
_Sources:_ https://developers.openai.com/codex/plugins/build, https://code.claude.com/docs/en/plugins-reference

### System Design Principles and Best Practices

Use these principles when designing cross-compatible packages:

- **Portable by default:** put reusable instructions in Agent Skills first.
- **Adapter-specific by exception:** use Codex or Claude-only features only when they add clear value.
- **Least privilege:** keep read-only context, mutating tools, and executable hooks separate.
- **Progressive disclosure:** keep activation metadata short and load deeper references only when needed.
- **Deterministic execution where possible:** use scripts for repeatable transformations, validation, or generation.
- **Validation before release:** test both package managers and both runtime invocation paths.

_Sources:_ https://agentskills.io/specification, https://developers.openai.com/codex/skills, https://code.claude.com/docs/en/slash-commands

## 3. Implementation Approaches and Best Practices

### Current Implementation Methodologies

Start with a standalone skill before creating plugins. A skill is cheaper to iterate, easier to review, and naturally portable. Once the workflow needs distribution, bundled MCP configuration, marketplace installation, or runtime-specific enhancements, add plugin manifests.

Implementation should proceed in this order:

1. Write `skills/<name>/SKILL.md`.
2. Move long material to `references/`.
3. Add scripts only for deterministic operations.
4. Add MCP only for live data, external APIs, or side effects.
5. Add `.codex-plugin/plugin.json`.
6. Add `.claude-plugin/plugin.json`.
7. Add Claude command shims or subagents only when Claude-specific ergonomics justify them.
8. Add Codex custom agents or app mappings only when Codex-specific ergonomics justify them.

_Sources:_ https://developers.openai.com/codex/plugins/build, https://code.claude.com/docs/en/plugins-reference

### Implementation Framework and Tooling

Codex requires `.codex-plugin/plugin.json` as the plugin entry point. Its plugin root can include `skills/`, `.mcp.json`, `.app.json`, and `assets/`, while only `plugin.json` belongs inside `.codex-plugin/`.

Claude Code plugin components live at the plugin root, with `.claude-plugin/plugin.json` as plugin metadata. Claude Code can discover plugin skills and commands, and it supports agents, hooks, MCP servers, LSP servers, monitors, themes, and settings as plugin components.

_Development Frameworks:_ Agent Skills, MCP, Codex plugin manifests, Claude Code plugin manifests.
_Tool Ecosystem:_ Codex plugin marketplace/install flow, Claude `plugin validate`, local plugin-dir testing, repository-scoped artifacts.
_Build and Deployment Systems:_ CI should validate manifests, references, scripts, MCP config, and clean installs.
_Sources:_ https://developers.openai.com/codex/plugins/build, https://code.claude.com/docs/en/plugins-reference

## 4. Technology Stack Evolution and Current Trends

### Current Technology Stack Landscape

The compatibility stack is intentionally simple:

- Markdown plus YAML frontmatter for skills and Claude agents.
- JSON for plugin manifests, marketplace files, MCP config, hooks, and settings.
- TOML for Codex custom agents and configuration.
- Shell, Python, or JavaScript/TypeScript for deterministic helper scripts.
- MCP JSON-RPC for external tools, contextual resources, and reusable prompt workflows.

This stack is version-control-friendly and reviewable, which is important because agent extensions affect behavior, permissions, and execution paths.

_Sources:_ https://agentskills.io/specification, https://modelcontextprotocol.io/specification/2025-11-25

### Technology Adoption Patterns

Adoption is moving from isolated prompts to packaged agent capabilities. The practical migration path is:

- Convert repeated prompts into skills.
- Convert high-value skills into plugin packages.
- Move live integrations behind MCP.
- Add platform-specific adapters only where discovery, invocation, or security controls require them.
- Manage releases through semantic versioning and validated marketplace metadata.

The main trend is not a new programming language or framework. It is the emergence of file-based agent operating assets: `AGENTS.md`, `SKILL.md`, plugin manifests, MCP config, and runtime-specific agent definitions.

## 5. Integration and Interoperability Patterns

### Current Integration Approaches

Use a four-layer integration model:

1. **Repository instruction layer:** `AGENTS.md` for project-wide operating rules.
2. **Workflow layer:** Agent Skills for reusable procedures.
3. **Tool layer:** MCP for live data, APIs, and executable capabilities.
4. **Packaging layer:** Codex and Claude Code plugin manifests and marketplaces.

This model avoids overloading skills with API details and avoids hiding reusable procedures inside tool-specific hooks or commands.

_Sources:_ https://developers.openai.com/codex/skills, https://code.claude.com/docs/en/slash-commands, https://modelcontextprotocol.io/specification/2025-11-25

### Interoperability Standards and Protocols

MCP defines server-side features for resources, prompts, and tools. In a cross-compatible design, resources should provide context, prompts should expose user-controlled templates or workflows, and tools should be explicit executable boundaries. That separation is especially important for security review and permission modeling.

Commands should be treated as entrypoints into skills. Claude Code still supports command-style plugin components, but its own documentation recommends skills for new plugin behavior because skills support supporting files. Codex exposes skills directly through its own invocation surfaces rather than through Claude-style flat command files.

_Sources:_ https://modelcontextprotocol.io/specification/2025-11-25, https://code.claude.com/docs/en/plugins-reference, https://developers.openai.com/codex/app/commands

## 6. Performance and Scalability Analysis

### Performance Characteristics and Optimization

The primary performance constraint is context footprint, not CPU. Skills and plugin metadata can affect discovery and model context before any useful work starts. Large descriptions, overly broad skills, duplicated installed packages, or excessive tool schemas can reduce precision and increase context cost.

Optimization practices:

- Keep skill descriptions precise and short.
- Keep `SKILL.md` focused on activation-critical behavior.
- Move long reference content into `references/`.
- Use scripts for repeatable mechanical work.
- Avoid loading duplicate standalone and plugin versions of the same skill in one environment.
- Prefer narrow MCP tools over broad catch-all tools.

_Sources:_ https://agentskills.io/specification, https://developers.openai.com/codex/skills

### Scalability Patterns and Approaches

Scale through package governance, not larger prompts. As the number of skills and agents grows, teams need ownership, versioning, compatibility matrices, and smoke tests. The recommended scaling unit is a small, focused skill with optional references and scripts, not a large omnibus skill.

For MCP-heavy environments, avoid exposing every backend operation as a top-level model tool. Use coherent domain servers, least-privilege tool surfaces, and retrieval or routing patterns where appropriate.

## 7. Security and Compliance Considerations

### Security Best Practices and Frameworks

The security model must assume that extension files can influence tool use and that tools can affect external systems. Prompt injection is a top LLM application risk, and insecure output handling can lead to downstream execution or data exposure. This matters when agents consume untrusted repository content, web pages, issue comments, logs, or documents.

Security requirements:

- Do not store secrets in skills, manifests, examples, scripts, or marketplace metadata.
- Separate read-only resources from mutating tools.
- Require explicit confirmation for destructive operations.
- Minimize MCP scopes and tool permissions.
- Treat Claude hooks and monitors as executable code requiring review.
- Treat Codex custom agents with write-capable tools as privileged runtime configurations.
- Add secret scanning and static validation to CI.

_Sources:_ https://owasp.org/www-project-top-10-for-large-language-model-applications/, https://modelcontextprotocol.io/specification/2025-11-25

### Compliance and Governance Considerations

Cross-compatible plugin governance should define:

- Which artifacts are portable, Codex-only, Claude-only, or generated.
- Which MCP servers are approved for project, user, or managed installation scopes.
- Which scripts and hooks require security review.
- How semantic versions map across Codex and Claude manifests.
- How marketplace entries are pinned, upgraded, and audited.

This governance is not bureaucracy for its own sake. It prevents silent compatibility drift when one runtime adds features or changes discovery behavior.

## 8. Strategic Technical Recommendations

### Technical Strategy and Decision Framework

Use this decision framework:

- **Is the behavior a reusable procedure?** Put it in `SKILL.md`.
- **Does it need long examples or standards?** Put them in `references/`.
- **Does it need deterministic file transformation or validation?** Put it in `scripts/`.
- **Does it need external systems or live state?** Put it behind MCP.
- **Does it only improve Claude invocation or lifecycle behavior?** Put it in Claude adapters.
- **Does it only improve Codex agent behavior or app integration?** Put it in Codex adapters.
- **Does it need team or community distribution?** Package it as plugins for both runtimes.

### Competitive Technical Advantage

The competitive advantage comes from maintaining one high-quality workflow core while still respecting each runtime. Teams that build only Claude commands or only Codex-specific instructions will duplicate effort. Teams that force all behavior into one nominally portable file will lose runtime quality. The right tradeoff is portable semantics plus deliberate adapters.

## 9. Implementation Roadmap and Risk Assessment

### Technical Implementation Framework

Recommended phased roadmap:

1. Inventory existing prompts, commands, agents, hooks, and MCP configs.
2. Classify each item as portable workflow, tool integration, runtime adapter, or obsolete.
3. Convert repeated command-like workflows into Agent Skills.
4. Normalize skill layout and references.
5. Add plugin manifests for Codex and Claude Code.
6. Add generated or manually maintained agent projections where needed.
7. Add validation and clean-install smoke tests.
8. Publish to repo-scoped marketplaces first.
9. Promote stable packages to user, team, or managed distribution.

### Technical Risk Management

Major risks and mitigations:

- **Invocation ambiguity:** use narrow skill descriptions and fixture prompts.
- **Context bloat:** keep skills focused and references lazy-loaded.
- **Manifest drift:** validate both plugin manifests in CI.
- **Security boundary mistakes:** separate read-only and mutating tool surfaces.
- **Runtime divergence:** maintain a compatibility matrix and adapter-specific tests.
- **Duplicate installation:** document precedence and avoid installing the same skill through multiple paths.
- **Unreviewed executable behavior:** review scripts, hooks, monitors, and MCP servers as code.

## 10. Future Technical Outlook and Innovation Opportunities

### Emerging Technology Trends

Near term, the center of gravity will remain `SKILL.md` plus MCP. Skills provide procedural knowledge with progressive disclosure; MCP provides external capability boundaries. Plugin systems will continue to differ because they also encode product-specific distribution, UI, security, and lifecycle concerns.

Medium term, expect more adapter generation: one role definition rendered to Codex TOML and Claude Markdown/YAML, one skill source packaged into multiple plugin ecosystems, and CI tooling that validates both.

Long term, a richer cross-agent package standard may emerge, but current primary documentation does not justify betting on one manifest to replace both `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json`.

### Innovation and Research Opportunities

High-value opportunities:

- A generator that renders Codex and Claude agent adapters from one typed source.
- A validator that checks Agent Skills plus both plugin manifests.
- A compatibility matrix linter that labels components as portable, Codex-only, Claude-only, or unsupported.
- Smoke-test harnesses that replay skill invocation prompts across both runtimes.
- Security policy tooling for MCP scope, scripts, hooks, monitors, and destructive workflows.

## 11. Technical Research Methodology and Source Verification

### Comprehensive Technical Source Documentation

Primary sources used:

- OpenAI Codex plugin build documentation: https://developers.openai.com/codex/plugins/build
- OpenAI Codex skills documentation: https://developers.openai.com/codex/skills
- OpenAI Codex subagents documentation: https://developers.openai.com/codex/subagents
- OpenAI Codex commands documentation: https://developers.openai.com/codex/app/commands
- Claude Code plugin reference: https://code.claude.com/docs/en/plugins-reference
- Claude Code skills documentation: https://code.claude.com/docs/en/slash-commands
- Claude Code subagents documentation: https://code.claude.com/docs/en/sub-agents
- Agent Skills specification: https://agentskills.io/specification
- Model Context Protocol specification: https://modelcontextprotocol.io/specification/2025-11-25
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/

Representative search and verification queries:

- Codex plugins skills subagents commands documentation
- Claude Code plugins skills subagents commands documentation
- Agent Skills specification SKILL.md open standard
- Model Context Protocol specification resources prompts tools
- OWASP LLM Top 10 prompt injection insecure output handling

### Technical Research Quality Assurance

The final recommendations are based on source agreement across the documented systems. Claims about shared portability are limited to Agent Skills and MCP because those are the overlapping standards in primary documentation. Claims about agents, commands, hooks, monitors, app integrations, and plugin manifests are intentionally adapter-specific because the primary docs define different formats and runtime behavior.

Confidence levels:

- **High:** `SKILL.md` as shared workflow core; MCP as integration boundary; separate plugin manifests for Codex and Claude Code.
- **High:** Codex custom agents use TOML; Claude Code subagents use Markdown/YAML.
- **Medium:** Future convergence around richer shared plugin packaging. Current evidence supports layered interoperability, not a single universal manifest.
- **Medium:** Automated adapter generation. It is technically straightforward, but runtime validation remains necessary.

Limitations:

- The previously supplied Gemini shared document could not be fetched because Google returned a consent/sign-in interstitial.
- Runtime behavior should still be verified in clean Codex and Claude Code installations before release.
- Platform docs can change; package maintainers should re-run source verification before major releases.

## 12. Technical Appendices and Reference Materials

### Compatibility Matrix

| Component | Portable Core | Codex Adapter | Claude Code Adapter | Recommendation |
| --- | --- | --- | --- | --- |
| Repository guidance | `AGENTS.md` | Codex reads AGENTS.md | Claude commonly uses CLAUDE.md/project config patterns | Keep `AGENTS.md`; add runtime-specific guidance only if needed |
| Skill workflow | `skills/<name>/SKILL.md` | Supported through Codex skills/plugins | Supported through Claude skills/plugins | Use as source of truth |
| Plugin manifest | None | `.codex-plugin/plugin.json` | `.claude-plugin/plugin.json` | Maintain both |
| Command workflow | Skill entrypoint | Skill invocation / Codex command surfaces | `skills/` preferred; `commands/` still supported | Put behavior in skills; add shims where useful |
| Subagent | Shared role definition only | TOML under `.codex/agents/` | Markdown/YAML under `.claude/agents/` or plugin `agents/` | Generate or maintain separate projections |
| External tools | MCP server design | `.mcp.json` in plugin root | `.mcp.json` in plugin root | Use MCP with least privilege |
| Hooks/monitors | Not portable | Codex-specific hooks/config where supported | Claude plugin hooks/monitors | Treat as runtime-specific executable code |
| App integrations | Not portable | Codex `.app.json` | Not equivalent | Keep Codex-specific |

### Reference Package Layout

```text
plugin-root/
  README.md
  AGENTS.md
  skills/
    dual-compatible-workflow/
      SKILL.md
      references/
      scripts/
      assets/
  .mcp.json
  .codex-plugin/
    plugin.json
  .claude-plugin/
    plugin.json
  agents-src/
    reviewer.yaml
  agents-codex/
    reviewer.toml
  agents-claude/
    reviewer.md
  tests/
    skill-fixtures/
    install-smoke/
```

### Minimum Release Checklist

- Validate `SKILL.md` frontmatter and skill name.
- Validate all referenced files exist.
- Validate `.codex-plugin/plugin.json`.
- Validate `.claude-plugin/plugin.json`.
- Run `claude plugin validate` where Claude Code is available.
- Install from a local Codex marketplace and verify discovery.
- Install with `claude --plugin-dir` or marketplace flow and verify discovery.
- Invoke each skill with expected direct and implicit prompts.
- Run MCP server smoke tests.
- Run secret scanning.
- Confirm version numbers and release notes match across manifests.

---

## Technical Research Conclusion

The most defensible compatibility strategy is to standardize on skills for behavior and MCP for integrations, then isolate Codex and Claude Code differences in thin adapter files. This keeps the reusable knowledge portable while allowing each runtime to use its own packaging, invocation, agent, and security model.

The practical next step is to implement a reference dual-compatible package with one real skill, one optional MCP server, one Codex manifest, one Claude Code manifest, and a small validation harness. That reference package can become the template for future plugins, skills, agents, and command migrations.

**Technical Research Completion Date:** 2026-04-26
**Research Period:** current comprehensive technical analysis
**Source Verification:** Current web verification completed with primary sources
**Technical Confidence Level:** High for the shared-core/thin-adapter architecture; medium for future package-standard convergence
