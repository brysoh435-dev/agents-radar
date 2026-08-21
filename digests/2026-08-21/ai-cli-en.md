# AI CLI Tools Community Digest 2026-08-21

> Generated: 2026-08-21 03:30 UTC | Tools covered: 10

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [Kimi Code CLI](https://github.com/MoonshotAI/kimi-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/badlogic/pi-mono)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [DeepSeek TUI](https://github.com/Hmbown/DeepSeek-TUI)
- [Grok Build](https://github.com/xai-org/grok-build)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## Cross-Tool Comparison

# AI CLI Tools Ecosystem Cross-Comparison Report
**Date: 2026-08-21**

---

## 1. Ecosystem Overview

The AI CLI developer tools landscape has matured into a multi-polar ecosystem with eight actively tracked projects, though activity levels vary dramatically. The space is consolidating around three core battlegrounds: **agent orchestration reliability** (subagent state management, hang detection, false success reporting), **cross-tool standardization** (AGENTS.md, MCP protocol adoption), and **enterprise readiness** (sandboxing, OAuth lifecycle, cost controls). Windows platform parity remains universally problematic, with every major tool showing disproportionate bug concentration on that platform. Release cadence has bifurcated between aggressive daily shipping (OpenAI Codex: 4 builds in 24h, Qwen Code: nightly validation discipline) and conservative maintenance mode (Kimi CLI: no releases, minimal issue volume).

---

## 2. Activity Comparison

| Tool | Issues (24h) | PRs (24h) | Releases (24h) | Release Status |
|------|:-----------:|:---------:|:------------:|----------------|
| **Claude Code** | 10 tracked | 0 | 1 stable (v2.1.238) | Shipped with immediate regressions |
| **OpenAI Codex** | 10 tracked | 10 | 4 (1 stable, 3 alpha) | Aggressive cadence; alpha pipeline active |
| **Gemini CLI** | 10 tracked | 9 | 1 nightly (v0.56.0) | Nightly discipline; reliability focus |
| **GitHub Copilot CLI** | 10 tracked | 1 | 1 (v1.0.81-6) | Issue-resolution sprint; low PR velocity |
| **Kimi CLI** | 2 | 1 | 0 | Dormant; critical bug underreported |
| **OpenCode** | 10 tracked | 10 | 1 stable (v1.18.19) | Community-contribution friendly |
| **Pi** | 10 tracked | 10 | 0 | UX papercut closure wave |
| **Qwen Code** | 10 tracked | 10 | 3 (1 stable, 2 nightly) | Validated: 4/4 SWE-bench smoke tests passed |
| **DeepSeek TUI / CodeWhale** | 7 | 5 | 1 rebrand (v0.9.10) | Rebrand migration in progress |
| **Grok Build** | 0 | 0 | 0 | **No activity** |

*Note: "10 tracked" indicates digest coverage scope, not necessarily total repository activity. Kimi CLI and Grok Build show significantly lower community engagement.*

---

## 3. Shared Feature Directions

| Requirement | Tools | Specific Needs |
|-------------|-------|--------------|
| **Cross-tool standardization** | Claude Code, OpenCode, Pi | AGENTS.md convention (Claude #6235 closed after 374 comments); `~/.agents/skills` auto-discovery (OpenCode #43747); `/exit`, `/config` alias parity (Pi #4537, #8399) |
| **MCP protocol maturity** | Claude Code, OpenAI Codex, Gemini CLI, Qwen Code, Kimi CLI | OAuth auto-refresh (Codex #17265), auto-reconnect (Codex #11489), server-side deployment compatibility (Claude #88370), MCP 2026 client (Qwen #8992), workspace-scoped memory via MCP (Kimi #2613) |
| **Subagent/orchestration reliability** | Gemini CLI, OpenCode, Claude Code, Qwen Code | Recovery from MAX_TURNS as false success (Gemini #22323), `model=undefined` crash (OpenCode #33043), fork cache forfeiture (Claude #88412), review loop convergence (Qwen #9278, #9526) |
| **Windows platform parity** | Claude Code, OpenAI Codex, Gemini CLI, GitHub Copilot CLI, Pi, OpenCode | Path normalization (`\\?\` prefix: Codex #39209), relaunch deadlocks (Claude #42776), sidecar timeouts (OpenCode #43762), terminal input corruption (Pi #6300), sandbox path validation (Copilot #4524) |
| **Cost/token management** | OpenAI Codex, Claude Code, Qwen Code, Pi | Bedrock cache controls (Codex #37674), context reprocessing regression (Codex #34971), prompt cache forfeiture (Claude #88412), per-model compaction (Pi #8133), compression correctness (Qwen #9309) |
| **Session persistence/portability** | GitHub Copilot CLI, Claude Code, Qwen Code, Pi | Remote-SSH/WSL split sessions (Copilot #4539, #4543), cross-session messaging deadlocks (Claude #86012), resumed session false "missing tool result" (Qwen #9573), fork cache continuity (Pi) |

---

## 4. Differentiation Analysis

| Tool | Primary Focus | Target User | Technical Approach |
|------|-------------|-------------|------------------|
| **Claude Code** | Enterprise IDE integration; cooperative ecosystem participation | Professional developers in org environments | Desktop+CLI dual mode; aggressive sandboxing; thinking-block introspection |
| **OpenAI Codex** | Rapid iteration; remote/enterprise scaling | Teams, remote developers, AWS Bedrock enterprises | Rust-based performance; aggressive release cadence; multi-agent V1/V2 architecture |
| **Gemini CLI** | Multi-agent orchestration at scale | Google Cloud ecosystem users, distributed agent builders | Deep A2A protocol integration; subagent-heavy architecture; Seatbelt sandboxing |
| **GitHub Copilot CLI** | IDE-adjacent workflow; Microsoft ecosystem lock-in | Existing Copilot subscribers; VS Code users | ACP protocol; tight VS Code/Remote-SSH integration; policy-driven model access |
| **OpenCode** | Community-driven multi-provider flexibility | Cost-conscious developers; self-hosters | Cloudflare AI Gateway native; skill system; broad provider support (DeepSeek, GLM, Kimi) |
| **Pi** | TUI fidelity; terminal-native experience | Terminal-first developers; multi-provider users | Elaborate TUI theming; per-model configuration; extension platform |
| **Qwen Code** | Autonomous code review; enterprise SCM integration | Alibaba Cloud/Aone Code enterprises; review automation | `/review` skill hardening; Web Shell; SWE-bench validated releases |
| **CodeWhale** (ex-DeepSeek) | Simplified onboarding; Chinese-market localization | Chinese-speaking developers; first-time AI CLI users | Progressive disclosure UX; crate modularity refactor; dictionary-based i18n |
| **Kimi CLI** | Workspace-scoped memory plugins | Moonshot AI ecosystem builders | MCP-native memory; plugin security documentation |
| **Grok Build** | — | — | **No discernible activity** |

**Key Technical Divergences:**
- **Sandbox philosophy**: Claude Code and Gemini CLI prioritize aggressive isolation (worktree blocking, Seatbelt); OpenCode and Pi emphasize escape hatches and user control
- **Agent architecture**: Gemini CLI and Claude Code default subagent-heavy; OpenCode and Qwen Code make subagent optional with fallback paths
- **Release model**: OpenAI Codex (continuous alpha), Qwen Code (nightly+validated), Claude Code (stable point releases with regression risk)

---

## 5. Community Momentum & Maturity

| Tier | Tools | Indicators |
|------|-------|------------|
| **High momentum, maturing** | OpenAI Codex, OpenCode, Qwen Code, Pi | 10+ PRs/24h, active issue closure, validated release pipelines, community contributions |
| **High momentum, stressed** | Claude Code, Gemini CLI | High issue volume with regressions on release; internal development on private branches (Claude: 0 PRs); P1 agent hangs (Gemini) |
| **Maintenance mode** | GitHub Copilot CLI | Issue-resolution focused; low PR velocity (1/24h); documentation migration signaling governance shift |
| **Early/fragile** | CodeWhale, Kimi CLI | Rebrand migration debt (CodeWhale); critical underreported bugs (Kimi #2615); low issue volume may indicate low adoption or poor visibility |
| **Inactive** | Grok Build | Zero activity |

**Community Health Signals:**
- **Most responsive to user feedback**: Pi (long-standing `/exit` alias resolved), CodeWhale (#5345 multi-line mode closed quickly)
- **Highest technical debate depth**: Claude Code (#6235 AGENTS.md: 374 comments)
- **Strongest validation discipline**: Qwen Code (4 consecutive SWE-bench smoke tests)
- **Greatest enterprise contribution**: OpenCode (Cloudflare passthroughs, community rate-limit matching)

---

## 6. Trend Signals

| Trend | Evidence | Implication for Developers |
|-------|----------|---------------------------|
| **Agent orchestration is the new concurrency: hard, and everyone is failing at it** | Gemini #22323 (false success), Claude #88412 (cache forfeiture), OpenCode #33043 (undefined model), Qwen #9278 (review loops) | Expect 12-18 months of reliability iteration before subagent defaults are trustworthy; design for supervisor/human-in-the-loop fallbacks |
| **MCP is strategic but operationally immature** | Server-side breaking changes (Claude #88370), OAuth refresh gaps (Codex #17265), process leaks (Codex #38754), A2A state corruption (Gemini #28940) | Abstract MCP behind internal adapters; pin server versions; budget for auth lifecycle engineering |
| **Windows is a tax on every AI CLI team** | Disproportionate issue concentration across all tools; no tool has solved it | Prioritize WSL2 or remote Linux for production AI CLI workflows; native Windows remains high-friction |
| **Cross-tool standardization pressure is winning** | AGENTS.md closure (Claude), `~/.agents/skills` (OpenCode), alias parity (Pi) | Design skills/prompts for portability; avoid tool-specific lock-in |
| **Cost awareness is shifting from "nice to have" to "architecture driver"** | Cache forfeiture bugs, per-model compaction, token-budget sessions (Codex #39827), compression audits | Instrument token spend per subagent; design for prompt cache affinity; model-router for cost optimization |
| **Progressive disclosure as competitive differentiator** | CodeWhale #5522 (first-run psychological cost), Pi TUI refinement | First 60 seconds of CLI experience increasingly matter for adoption; configuration front-loading churns users |
| **Chinese-market localization as growth vector** | CodeWhale dictionary spine (#5520), OpenCode Chinese-language issue surge, Kimi CLI's native context | Tools ignoring CJK input and documentation i18n risk losing fastest-growing developer segment |

---

*Report compiled from community digest data. For methodological questions or corrections, refer to individual tool digests.*

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report
**Data as of 2026-08-21 | Source: github.com/anthropics/skills**

---

## 1. Top Skills Ranking (Most-Discussed PRs)

| Rank | Skill | PR | Status | Description | Discussion Highlights |
|:---|:---|:---|:---|:---|:---|
| 1 | **Skill-Creator Eval Fix** | [#1298](https://github.com/anthropics/skills/pull/1298) | **OPEN** | Comprehensive repair of `run_eval.py` — installs eval artifacts as real skills, fixes Windows stream reading, trigger detection, and parallel workers | Addresses critical bug where recall=0% breaks the entire description-optimization loop; consolidates fixes from #556, #1099, #1050 |
| 2 | **Document Typography** | [#514](https://github.com/anthropics/skills/pull/514) | **OPEN** | Typographic quality control for AI-generated documents — prevents orphan words, widow paragraphs, and numbering misalignment | Universal pain point; affects every document Claude generates; users rarely ask for good typography but always suffer without it |
| 3 | **ODT Skill** | [#486](https://github.com/anthropics/skills/pull/486) | **OPEN** | OpenDocument text creation, template filling, and ODT→HTML conversion | Fills gap in open-source/ISO standard document workflows; triggers on LibreOffice and ODF mentions |
| 4 | **Frontend-Design Clarity** | [#210](https://github.com/anthropics/skills/pull/210) | **OPEN** | Revised frontend-design skill with actionable, conversation-scoped instructions | Focus on token efficiency and behavioral specificity — every instruction must be executable in a single turn |
| 5 | **Skill Quality & Security Analyzers** | [#83](https://github.com/anthropics/skills/pull/83) | **OPEN** | Two meta-skills: five-dimension quality analysis (structure, triggers, examples, safety, performance) and security vulnerability scanning | Meta-level tooling for the Skills ecosystem itself; positions Claude as a Skills reviewer |
| 6 | **Self-Audit Skill** | [#1367](https://github.com/anthropics/skills/pull/1367) | **OPEN** | Mechanical file verification + four-dimension reasoning quality gate (damage-severity priority) | Universal applicability; "Step 0" mechanical verification catches hallucinated file claims before reasoning audit |
| 7 | **Testing-Patterns** | [#723](https://github.com/anthropics/skills/pull/723) | **OPEN** | Full testing stack: Testing Trophy philosophy, AAA pattern, React component testing, E2E patterns | Addresses persistent community demand for test-generation guidance |
| 8 | **ServiceNow Platform** | [#568](https://github.com/anthropics/skills/pull/568) | **OPEN** | Broad enterprise platform coverage: ITSM, ITOM, ITAM/SAM, FSM, HRSD, CSM, SPM/PPM, SecOps, IntegrationHub | Largest domain scope of any pending skill; actively maintained (last updated 2026-08-12) |

---

## 2. Community Demand Trends (From Issues)

| Trend | Evidence | Implication |
|:---|:---|:---|
| **Trust & Security Architecture** | [#492](https://github.com/anthropics/skills/issues/492) (43 comments, #1 issue) — namespace impersonation vulnerability; [#412](https://github.com/anthropics/skills/issues/412) — agent governance patterns | Community skills distributed under `anthropic/` namespace create trust boundary abuse; demand for verifiable skill provenance and governance frameworks |
| **Enterprise Collaboration** | [#228](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) — org-wide skill sharing | Skills as organizational knowledge capital; need for shared libraries, version control, and access management |
| **Context Window Efficiency** | [#1487](https://github.com/anthropics/skills/issues/1487) — `claude-api` skill injects ~156k tokens; [#1329](https://github.com/anthropics/skills/issues/1329) — compact-memory symbolic notation | Skills themselves are becoming context hazards; demand for compression, lazy loading, and symbolic state representation |
| **Cross-Platform/Cloud Portability** | [#29](https://github.com/anthropics/skills/issues/29) — AWS Bedrock compatibility; [#16](https://github.com/anthropics/skills/issues/16) — Expose Skills as MCPs | Skills locked to Claude Code CLI limit deployment flexibility; MCP standardization would unlock broader ecosystem |
| **Build Toolchain Resilience** | [#1362](https://github.com/anthropics/skills/issues/1362) — pnpm ≥10.1 breaks web-artifacts-builder; [#12](https://github.com/anthropics/skills/issues/12) — whitespace corruption in DOCX | Skills with external dependencies decay as toolchains evolve; need for hermetic builds and CI-verified skill health |

---

## 3. High-Potential Pending Skills

These active PRs have significant discussion but remain unmerged — likely to land with revision:

| Skill | PR | Blocker/Opportunity |
|:---|:---|:---|
| **Skill-Creator Eval System** | [#1298](https://github.com/anthropics/skills/pull/1298) | Critical infrastructure fix; consolidates 3+ partial fixes (#1099 Windows pipes, #1050 subprocess encoding); blocked on review complexity |
| **Document Typography** | [#514](https://github.com/anthropics/skills/pull/514) | Universal applicability; low implementation risk; may need scope narrowing to merge |
| **Self-Audit** | [#1367](https://github.com/anthropics/skills/pull/1367) | Author has related proposal [#1385](https://github.com/anthropics/skills/issues/1385) for three-gate pipeline; potential to merge as v1.3.0 or expand |
| **Pyxel Retro Game Dev** | [#525](https://github.com/anthropics/skills/pull/525) | MCP server integration pattern; niche but complete implementation; author is Pyxel maintainer |
| **SAP-RPT-1-OSS Predictor** | [#181](https://github.com/anthropics/skills/pull/181) | Enterprise tabular ML; Apache 2.0 model; narrow but high-value SAP ecosystem use case |

---

## 4. Skills Ecosystem Insight

> **The community's most concentrated demand is for *meta-level reliability infrastructure* — skills that verify skills, context-efficient skills that manage their own bloat, and trust mechanisms that prevent namespace impersonation — reflecting a maturation from "more skills" to "skills I can safely depend on at scale."**

---

---

# Claude Code Community Digest — 2026-08-21

---

## 1. Today's Highlights

Anthropic shipped **v2.1.238** with a new `keybindingFlavor` setting for Bash-style Ctrl+W behavior and plugin marketplace improvements, while the community closed the long-running **AGENTS.md standardization debate** (#6235) after 374 comments. Meanwhile, fresh regressions in the latest build—particularly around thinking-block persistence and MCP widget rendering—are drawing urgent attention from maintainers.

---

## 2. Releases

### [v2.1.238](https://github.com/anthropics/claude-code/releases/tag/v2.1.238)
- **`keybindingFlavor` setting**: New `"readline"` option makes Ctrl+W delete back to previous whitespace (matching Bash behavior); default `"classic"` behavior unchanged
- **Plugin marketplace enhancements**: `headersHelper` support for URL marketplaces and catalog entries (incomplete in changelog—appears truncated)

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#6235](https://github.com/anthropics/claude-code/issues/6235) | **CLOSED: Support AGENTS.md standard** | Resolves year-long tension between Claude's `CLAUDE.md` convention and emerging cross-tool standard (Codex, Cursor, Amp). 374-comment thread shaped ecosystem interoperability. | **4,929 👍**, widely celebrated closure; positions Claude Code as cooperative participant in agent standards |
| [#42776](https://github.com/anthropics/claude-code/issues/42776) | **Windows Desktop relaunch blocked by orphaned process file lock** | Critical Windows workflow breaker—users cannot restart app without manual process termination. 125 comments suggest persistent, unresolved system-level issue. | Frustrated; workaround-heavy; marked `invalid` but clearly affecting real users |
| [#77136](https://github.com/anthropics/claude-code/issues/77136) | **Model degradation: repetitive rhetorical tics in Claude 4.7–5.0/Fable** | Quality regression spanning multiple model generations undermines trust in Anthropic's flagship capability. Explicit style instructions fail. | **319 👍**, deeply concerned; users documenting specific failure patterns across model versions |
| [#18567](https://github.com/anthropics/claude-code/issues/18567) | **Bun v1.3.5 crash on Windows blocks installation** | Complete Windows install failure via runtime dependency. "Integer does not fit in destination type" suggests native compilation issue. | 15 👍, blocked users; `has repro` and `oncall` labels indicate active internal tracking |
| [#86012](https://github.com/anthropics/claude-code/issues/86012) | **Cross-session messages freeze recipient until 15–20 min idle timeout** | Severe reliability issue in Desktop's async messaging (MCP/agent-view). `hadFirstResponse=false` with `no_response` indicates protocol-level deadlock. | 6 👍, urgent; affects production integrations; regression across Windows/macOS |
| [#88370](https://github.com/anthropics/claude-code/issues/88370) | **MCP Apps widgets stopped rendering after server-side rollout** | **Same-day regression** (Aug 20): widgets with `_meta.ui.resourceUri` broke without client changes. Points to staged server deployment lacking backward compatibility. | 0 👍 but 6 rapid comments; developer identifies exact server-side change; demands rollback capability |
| [#88383](https://github.com/anthropics/claude-code/issues/88383) | **v2.1.238 regression: thinking blocks persisted as empty husks** | New release corrupts session state: `thinking: ""` with signature-only objects. Breaks debugging, logging, and potentially downstream tools consuming thinking output. | 1 👍, immediate post-release report; references related SDK-cli issue #87947 |
| [#18467](https://github.com/anthropics/claude-code/issues/18467) | **Personal GitHub repos invisible in Claude web; org repos work** | Asymmetry in GitHub App permissions breaks individual developer workflows. Suggests scope/visibility bug in Claude web's repository enumeration. | 75 👍, widespread; `has repro` with clear steps |
| [#25286](https://github.com/anthropics/claude-code/issues/25286) | **Terminal freeze: 100% write ratio, no input accepted** | Hard UI hang requiring external `kill`. Memory/perf label suggests renderer I/O loop. 5+ occurrences for reporter indicates systemic, not sporadic. | 18 👍, detailed diagnostics; macOS-specific terminal renderer concern |
| [#66153](https://github.com/anthropics/claude-code/issues/66153) | **Tool-use markup corrupted as "court" instead of "antml:invoke"** | Model outputs garbled XML tags, breaking tool execution entirely. "court" suggests tokenization/alignment failure in model weights or inference path. | 14 👍, bizarre and critical; VS Code + Windows; marked duplicate but active |

---

## 4. Key PR Progress

**No Pull Requests updated in the last 24 hours.** *(Total: 0 items)*

This absence is notable given the volume of issues—suggests either:
- Internal development happening on private branches
- Release cadence focused on direct commits rather than community PRs
- Potential bottleneck in external contribution pipeline

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **Cross-tool standardization** | #6235 (AGENTS.md), #16345 (`.github/skills/` directory) | High — ecosystem pressure for interoperability; AGENTS.md resolved, skills directory pending |
| **Desktop parity with CLI** | #48564 (Opusplan in Desktop, closed), #62449 (detached window zoom), #87712 (session groups inheritance) | Moderate — Desktop consistently lags CLI feature set |
| **Bash/shell ergonomics** | #87959 (worktree isolation over-blocking compound commands), #88379 (git path classification bugs) | Rising — sandbox security model creating friction for legitimate workflows |
| **Agent/subagent reliability** | #88412 (fork cache forfeiture), #83463 (post-end_turn looping), #87216 (sandbox bypass in subagents) | High — agent orchestration is active development area with rough edges |
| **Internationalization** | #80415 (Korean text garbling), #70955 (IME overlap) | Steady — CJK input issues persist across platforms |

---

## 6. Developer Pain Points

### **Release Stability & Regressions**
v2.1.238 shipped with immediate reported regressions (#88383 thinking husks, #88370 MCP widgets). Pattern of point-release breakage erodes upgrade confidence. Users increasingly pin versions or defer updates.

### **Windows as Second-Class Platform**
Disproportionate bug concentration: install failures (#18567 Bun, #88417 protocol mismatch), relaunch deadlocks (#42776), IME issues (#70955), TUI quirks (#59408 Ctrl+C). MSIX packaging (#86012) appears to complicate autoupdate.

### **Model Quality at Scale**
#77136 documents degradation across 4.7→5.0/Fable—repetitive tics, incoherent prose despite explicit instructions. Combined with #87491 (Opus 5 "negotiation" behavior) and #67246 (overactive safety classifier), suggests training or alignment changes are affecting production reliability.

### **Sandbox Security vs. Usability Tension**
Worktree isolation (#87959, #88379) and file-read rules (#87216) are technically sound but aggressively block legitimate compound commands and path variations. Developers need escape hatches or smarter parsing, not blanket denials.

### **MCP Ecosystem Fragility**
Server-side deployments breaking client widgets (#88370) without version negotiation grace periods. Cross-session message deadlocks (#86012, #36477). MCP is strategic but operational maturity lags adoption.

### **Session State & Caching Economics**
#88412 reveals forked subagents forfeit prompt cache on wake—direct cost impact. #88383 corrupts thinking block persistence. Session JSONL as debug surface is becoming unreliable.

---

*Digest compiled from github.com/anthropics/claude-code public activity. For corrections or additions, open an issue or discussion.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-08-21

---

## 1. Today's Highlights

OpenAI shipped **Codex 0.149.0** stable with a new interactive `codex agents` dashboard and TUI directory management commands, while pushing ahead to **0.150.0-alpha.1**. The release cadence remains aggressive with four builds in 24 hours, but Windows Desktop continues to dominate the bug tracker with path-handling and RPC stability issues drawing heavy community engagement.

---

## 2. Releases

| Version | Type | Key Changes |
|---------|------|-------------|
| **[rust-v0.149.0](https://github.com/openai/codex/releases/tag/rust-v0.149.0)** | Stable | **Interactive `codex agents` dashboard** — search, start, open, rename, and stop tasks with configurable shortcuts ([#39094](https://github.com/openai/codex/issues/39094), [#39112](https://github.com/openai/codex/issues/39112), [#39114](https://github.com/openai/codex/issues/39114), [#39142](https://github.com/openai/codex/issues/39142)); **`/cd`, `/pwd`, `/cwd` TUI commands** for working directory management ([#38894](https://github.com/openai/codex/issues/38894)) |
| **[rust-v0.150.0-alpha.1](https://github.com/openai/codex/releases/tag/rust-v0.150.0-alpha.1)** | Alpha | Early next-cycle build; no detailed notes |
| **[rust-v0.149.0-alpha.7](https://github.com/openai/codex/releases/tag/rust-v0.149.0-alpha.7)** | Alpha | Pre-release stabilization |
| **[rust-v0.149.0-alpha.4.1](https://github.com/openai/codex/releases/tag/rust-v0.149.0-alpha.4.1)** | Alpha | Hotfix on alpha.4 |

---

## 3. Hot Issues

| # | Title | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| **[#17265](https://github.com/openai/codex/issues/17265)** | MCP OAuth tokens don't auto-refresh despite stored refresh tokens | Breaks all routed MCP tool calls after token expiry — core auth reliability gap | **57 👍, 32 comments**; long-running, highly upvoted; users expect seamless token lifecycle |
| **[#39130](https://github.com/openai/codex/issues/39130)** | Windows: Local thread archiving fails with os error 2 | Data loss risk — completed threads can't be archived, cluttering workspace | 17 comments, active triage; appears related to [#39209](https://github.com/openai/codex/issues/39209) and [#39378](https://github.com/openai/codex/issues/39378) |
| **[#39209](https://github.com/openai/codex/issues/39209)** | Windows `\\?\` extended-length path prefix breaks archiving | Root-cause analysis of Windows path normalization failure; affects long paths | 14 comments; community identifying external path normalization as ineffective |
| **[#20930](https://github.com/openai/codex/issues/20930)** | App notifications broken with remote connections | Remote Linux + macOS desktop workflow is common; silent completion hurts productivity | **18 👍**, 12 comments; cross-platform remote dev friction |
| **[#38754](https://github.com/openai/codex/issues/38754)** | Windows: stdio MCP servers repeatedly spawned, not reaped | Resource leak + performance degradation; process exhaustion in long tasks | 11 comments; Windows-specific MCP stability concern |
| **[#37674](https://github.com/openai/codex/issues/37674)** | Bedrock GPT-5.6 Sol lacks explicit cache controls, high spend | **Cost regression** — enterprise AWS users hit with excessive cache-write charges | **7 👍**, 8 comments; production usage evidence cited; linked to [#35300](https://github.com/openai/codex/issues/35300) |
| **[#38939](https://github.com/openai/codex/issues/38939)** | **CRITICAL**: macOS runaway computer-use threads → V8 OOM crash | App-unusable severity; thread exhaustion crashes entire Codex process | 5 comments; needs immediate attention despite lower comment count |
| **[#34971](https://github.com/openai/codex/issues/34971)** | Regression: massive cached context reprocessed, timeouts, credit burn | **Performance/cost emergency** — long sessions become unusable, JSONL bloat | 6 comments; regression tag signals recent breakage |
| **[#11489](https://github.com/openai/codex/issues/11489)** | MCP client lacks auto-reconnect after disconnect | Reliability gap vs. SSE streams; manual restart required | **7 👍**, 5 comments; parity request with existing retry infrastructure |
| **[#39815](https://github.com/openai/codex/issues/39815)** | Windows-Android Remote pairing succeeds but conversations fail (503) | Cross-device workflow broken; `/wham/tasks/list` endpoint failure | 6 comments; mobile-remote integration regression |

---

## 4. Key PR Progress

| # | Title | Feature / Fix | Significance |
|---|-------|-------------|------------|
| **[#39837](https://github.com/openai/codex/pull/39837)** | Ignore project instructions for untrusted projects | **Security**: Skips `AGENTS.md` discovery for untrusted projects; cache key includes trust level | Prevents instruction injection from untrusted codebases |
| **[#39827](https://github.com/openai/codex/pull/39827)** | Add history and notes tools for token-budget sessions | **Feature**: Context recovery across window transitions; direct-model history tools | Addresses long-session context loss; pairs with cost-control efforts |
| **[#39825](https://github.com/openai/codex/pull/39825)** | Use Responses compaction for Amazon Bedrock | **Fix**: Replaces legacy compaction protocol with `/v1/responses` trigger | Directly related to [#37674](https://github.com/openai/codex/issues/37674) cost issues; Bedrock modernization |
| **[#39822](https://github.com/openai/codex/pull/39822)** | Preserve uncapped Guardian classifier instructions | **Fix**: Removes implicit token limit on classifier policy rendering | Safety policy completeness; regression fix for Guardian v2 |
| **[#39804](https://github.com/openai/codex/pull/39804)** | Use multi-agent V1 for Amazon Bedrock models | **Fix**: Bedrock compatibility — advertises V1 since V2 response items unsupported | Unblocks multi-agent on Bedrock; provider capability normalization |
| **[#28407](https://github.com/openai/codex/pull/28407)** | Avoid blocking on optional MCP startup | **Performance**: Concurrent tool listing, non-blocking optional servers | MCP responsiveness; related to auth/connectivity stability |
| **[#39811](https://github.com/openai/codex/pull/39811)** | Restrict macOS preference reads to full-disk policies | **Security**: Seatbelt/cfprefsd grants only when filesystem policy requires | Sandboxing hardening; privacy leak prevention |
| **[#39809](https://github.com/openai/codex/pull/39809)** | Preserve WINDIR in core Windows shell environments | **Fix**: Adds `WINDIR` to allowlist; case-variant handling | Windows shell reliability; environment consistency |
| **[#39798](https://github.com/openai/codex/pull/39798)** | Update rmcp to 3.1.3 | **Maintenance**: MCP library upgrade; preserves auth/retry classifications | MCP ecosystem currency; legacy fallback robustness |
| **[#39795](https://github.com/openai/codex/pull/39795)** | Add hostname to configurable TUI status line | **UX**: Status-line customization; non-DNS hostname resolution | Multi-host workflow clarity; remote session awareness |

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **Remote/SSH workflow expansion** | [#34946](https://github.com/openai/codex/issues/34946) (scheduled tasks for SSH hosts), [#20930](https://github.com/openai/codex/issues/20930) (remote notifications), [#39815](https://github.com/openai/codex/issues/39815) (Android Remote) | Strong — enterprise/team use cases pushing beyond single-device |
| **Cost control & caching** | [#37674](https://github.com/openai/codex/issues/37674) (Bedrock cache), [#39808](https://github.com/openai/codex/issues/39808) (subagent overhead), [#34971](https://github.com/openai/codex/issues/34971) (context reprocessing) | Critical — production users tracking spend per-token |
| **MCP reliability & lifecycle** | [#17265](https://github.com/openai/codex/issues/17265) (OAuth refresh), [#11489](https://github.com/openai/codex/issues/11489) (auto-reconnect), [#38754](https://github.com/openai/codex/issues/38754) (process management) | High — MCP is strategic but rough edges persist |
| **Windows path/IO robustness** | Multiple `\\?\` prefix issues, archiving failures, sandbox path validation | Very high — platform-specific debt accumulating |
| **Subagent/orchestration efficiency** | [#39808](https://github.com/openai/codex/issues/39808) (fan-out cost), [#38533](https://github.com/openai/codex/issues/38533) (UI state sync) | Growing — multi-agent V2 adoption surfacing scaling concerns |

---

## 6. Developer Pain Points

| Pain Point | Frequency | Impact | Tracking |
|------------|-----------|--------|----------|
| **Windows as second-class citizen** | 12+ issues in top 30 | Severe — archiving, paths, RPC, sandboxing, browser plugins all broken | [#39130](https://github.com/openai/codex/issues/39130), [#39209](https://github.com/openai/codex/issues/39209), [#39378](https://github.com/openai/codex/issues/39378), [#39399](https://github.com/openai/codex/issues/39399), [#38425](https://github.com/openai/codex/issues/38425), [#39705](https://github.com/openai/codex/issues/39705) |
| **Context/cost runaway in long sessions** | 3+ issues | High — timeouts, credit burn, JSONL bloat | [#34971](https://github.com/openai/codex/issues/34971), [#39808](https://github.com/openai/codex/issues/39808), [#37674](https://github.com/openai/codex/issues/37674) |
| **MCP auth & connection fragility** | 4+ issues | High — manual restarts, token expiry, process leaks | [#17265](https://github.com/openai/codex/issues/17265), [#11489](https://github.com/openai/codex/issues/11489), [#38754](https://github.com/openai/codex/issues/38754), [#28407](https://github.com/openai/codex/pull/28407) |
| **Cross-platform remote sync gaps** | 3+ issues | Moderate — notifications, pairing, conversation loading fail | [#20930](https://github.com/openai/codex/issues/20930), [#39815](https://github.com/openai/codex/issues/39815), [#34946](https://github.com/openai/codex/issues/34946) |
| **UI/settings regressions on update** | 2+ issues | Moderate — font resets, archived chat deletion missing | [#39781](https://github.com/openai/codex/issues/39781), [#39839](https://github.com/openai/codex/issues/39839) |

---

*Digest compiled from github.com/openai/codex activity on 2026-08-21.*

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-21

## Today's Highlights

The v0.56.0 nightly release landed with symlink handling fixes and shell execution service cleanups, while a significant wave of PRs landed targeting core reliability: git environment sanitization, interrupted response handling, and A2A server state corruption. The community continues to surface critical agent orchestration bugs—particularly around subagent recovery, generalist agent hangs, and shell execution stalls—that appear to be the dominant friction points in production use.

---

## Releases

**v0.56.0-nightly.20260821.g30573d2e4** ([Release](https://github.com/google-gemini/gemini-cli/releases/tag/v0.56.0-nightly.20260821.g30573d2e4))
- Fixes symlink evaluation consistency in ignore path handling ([#28915](https://github.com/google-gemini/gemini-cli/pull/28915))
- Refactors `shellExecutionService` to remove `eslint-disable` suppressions and unsafe type assertions ([#28862](https://github.com/google-gemini/gemini-cli/pull/28862))

---

## Hot Issues

| # | Issue | Why It Matters | Community Signal |
|---|-------|--------------|----------------|
| [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) | Subagent recovery after MAX_TURNS reported as GOAL success | Critical reliability flaw: subagents silently claim success when actually interrupted, corrupting task state and misleading users | 12 comments, 🔒 maintainer-only; marked need-retesting |
| [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) | Generalist agent hangs indefinitely | Blocks basic operations like folder creation; workaround (disable subagents) defeats the purpose of the architecture | 8 comments, 8 👍; high user impact |
| [#25166](https://github.com/google-gemini/gemini-cli/issues/25166) | Shell execution stuck at "Waiting input" after command completes | Core execution loop bug; simple commands hang with false input prompt, breaking automation workflows | 4 comments, 3 👍; P1 priority |
| [#21983](https://github.com/google-gemini/gemini-cli/issues/21983) | Browser subagent fails on Wayland | Linux compatibility gap for headless browser automation; affects CI and remote workflows | 4 comments, P1 |
| [#22186](https://github.com/google-gemini/gemini-cli/issues/22186) | `get-shit-done` output hook crashes CLI | Regression in output formatting; crashes near completion of task summaries | 3 comments, P1 |
| [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) | Bug reports omit subagent context | Observability gap; `/bug` command useless for diagnosing subagent failures, which are increasingly central | 2 comments, P1 |
| [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) | Gemini ignores custom skills and sub-agents | Discovery/prompting failure: model won't autonomously use configured skills even for relevant tasks | 6 comments |
| [#26522](https://github.com/google-gemini/gemini-cli/issues/26522) | Auto Memory retries low-signal sessions indefinitely | Background extraction wastes resources; unbounded retry loop on skipped sessions | 5 comments |
| [#26525](https://github.com/google-gemini/gemini-cli/issues/26525) | Auto Memory lacks deterministic redaction, over-logs | Security concern: secrets hit model context before prompt-level redaction; excessive logging | 4 comments, security-labeled |
| [#24246](https://github.com/google-gemini/gemini-cli/issues/24246) | 400 error with >128 tools | API limit hit by tool proliferation; needs smarter tool scoping | 3 comments |

---

## Key PR Progress

| # | PR | Description | Status |
|---|-----|-------------|--------|
| [#28934](https://github.com/google-gemini/gemini-cli/pull/28934) | History rollback and retry nudge optimizations | Eliminates synthetic cancellation messages, reduces context window bloat, maximizes prefix cache efficiency on retries | Open |
| [#28938](https://github.com/google-gemini/gemini-cli/pull/28938) | Keep `GIT_CONFIG_*` environment triplets consistent | Fixes git invocation failures when `sanitizeEnvironment()` emits malformed directives; critical for sandboxed git operations | Open, P1 |
| [#28939](https://github.com/google-gemini/gemini-cli/pull/28939) | Avoid persisting interrupted response placeholder | Prevents corrupted history from `[The previous response was interrupted...]` being treated as model output on retry | Open |
| [#28940](https://github.com/google-gemini/gemini-cli/pull/28940) | Clear stale cancellation error on new A2A message turns | Fixes "Execution aborted" state corruption in Google Cloud Assistant after cancellations | Open |
| [#28935](https://github.com/google-gemini/gemini-cli/pull/28935) | Isolate Docker/container runtime in macOS Seatbelt | Hardens sandbox against escape via VirtioFS mounts; denies socket/binary access | Open |
| [#28804](https://github.com/google-gemini/gemini-cli/pull/28804) | Evals tools expansion: `read_many_files`, internal docs, MCP resources | Adds behavioral eval coverage for batch operations and MCP protocol features | Open |
| [#28930](https://github.com/google-gemini/gemini-cli/pull/28930) | Drop unsafe `diff.external` git override | Fixes git breakage from empty override; reverts prior sandbox hardening that backfired | Open, P1 |
| [#28718](https://github.com/google-gemini/gemini-cli/pull/28718) | Record usage already received when stream aborted | Prevents telemetry data loss on cancellation paths | **Merged** |
| [#28910](https://github.com/google-gemini/gemini-cli/pull/28910) | Add Gemini 3.7 Flash, 3.6 Flash, 3.5 Flash-Lite configs | Model expansion across core and CLI packages | **Merged** |
| [#28863](https://github.com/google-gemini/gemini-cli/pull/28863) | Prompt for consent on MCP environment changes | Security fix: prevents unauthorized env injection into MCP server processes | Open |

---

## Feature Request Trends

1. **AST-aware tooling** — Multiple issues ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)) track replacing regex-based code discovery with precise method-boundary reads and structural navigation, targeting reduced token burn and fewer misaligned reads.

2. **Subagent observability and control** — Requests for trajectory sharing ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598)), browser session takeover ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232)), and configuration respect ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) indicate the subagent system is maturing but lacks operational transparency.

3. **Tactful/surgical extraction** ([#19561](https://github.com/google-gemini/gemini-cli/issues/19561)) — Formalizing a hierarchy from `grep_search` → `read_file` to combat the 36.6k token baseline bloat from firehose reads.

4. **Zero-dependency OS sandboxing** ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)) — Leveraging Gemini 3's native bash affinity without sacrificing security, proposing post-execution intent routing.

---

## Developer Pain Points

| Theme | Evidence | Severity |
|-------|----------|----------|
| **Agent hangs and false success states** | #21409, #22323, #25166, #22465 | Critical — core orchestration unreliable |
| **Shell/PTY lifecycle bugs** | #25166, #23571 (tmp scripts scattered), #28938 (git env corruption) | High — breaks automation and git workflows |
| **Subagent system opacity** | #21763, #22598, #21968 | High — hard to debug, harder to trust |
| **Memory system quality/security** | #26522, #26523, #26525 | Medium-High — background jobs leak or retry improperly |
| **Browser agent fragility** | #21983, #22232, #22267 | Medium — Linux/Wayland gaps, config ignored |
| **Terminal rendering performance** | #21924 | Medium — resize flicker, batch update needed |

The dominant pattern: Gemini CLI's multi-agent architecture is powerful but the control plane—state recovery, hang detection, honest status reporting, and cross-agent observability—lags behind the execution surface. Users are working around it by disabling subagents entirely, which suggests the default orchestration strategy needs fundamental hardening before feature expansion.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-21

## 1. Today's Highlights

GitHub Copilot CLI v1.0.81-6 shipped with session startup controls and headless auth improvements, while the team closed 19 issues in 24 hours including long-standing UX and MCP integration bugs. The community is actively reporting sandbox restrictions and WSL/Remote-SSH environment fragmentation as top pain points.

---

## 2. Releases

### [v1.0.81-6](https://github.com/github/copilot-cli/releases/tag/v1.0.81-6) — Session Controls & Auth Enhancements

| Change | Impact |
|--------|--------|
| `defaultMode` / `defaultPermissionMode` settings | Configure startup mode and approval behavior for new interactive sessions without per-session prompts |
| `--with-token` flag for `copilot login` | Enables CI/CD and headless environments to authenticate via stdin instead of interactive browser flow |
| ACP client improvements: subagent IDs, raw event subscriptions, live title/mod | Better observability and real-time control for programmatic consumers of the Copilot CLI SDK |

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#1481](https://github.com/github/copilot-cli/issues/1481) | **SHIFT+ENTER executes instead of line break** | Fixed a universal UX expectation violation; `CTRL+ENTER` for newlines was non-standard | 28 comments, 17 👍 — highest engagement in dataset; users celebrated closure after 6 months |
| [#4390](https://github.com/github/copilot-cli/issues/4390) | **Org-enabled models (Claude Sonnet 5/Opus 5, Kimi K3) missing from catalogue** | Enterprise users pay for models they couldn't access; exposed policy/catalouge sync gap | 15 comments, 7 👍; resolved quickly suggesting priority escalation |
| [#3162](https://github.com/github/copilot-cli/issues/3162) | **Registry-listed MCP servers falsely flagged as policy-blocked** | Broke legitimate MCP workflows for security-conscious orgs using custom servers | 7 comments; fix validates registry trust model |
| [#4096](https://github.com/github/copilot-cli/issues/4096) | **OAuth MCP "Connected" but tools missing in CLI** | Critical auth token bridging failure between app UI and CLI sessions | 6 comments, 2 👍; affects Atlassian and similar third-party integrations |
| [#4503](https://github.com/github/copilot-cli/issues/4503) | **SDK server ready without auth, Slack session fails** | SDK consumers (Slack DMs, integrations) hit generic failures from missing `COPILOT_SDK_AUTH_TOKEN` | 5 comments; indicates SDK startup race condition |
| [#4439](https://github.com/github/copilot-cli/issues/4439) | **GitLab MCP OAuth rejected for RFC 8414 issuer mismatch** | Blocked GitLab Self-Managed adoption; strict OIDC validation incompatible with common deployments | 5 comments, 3 👍; enterprise GitLab users affected |
| [#4422](https://github.com/github/copilot-cli/issues/4422) | **All Claude models disabled under CLI model selection** | Personal Enterprise account holders lost access to previously working models | 4 comments, 3 👍; persisted across rollbacks suggesting server-side policy issue |
| [#4206](https://github.com/github/copilot-cli/issues/4206) | **Environment footer stuck "Loading:" forever** | UI deadlock under org MCP policy; `/env` worked but footer never updated | 4 comments, 3 👍; visual bug with functional impact on trust |
| [#4038](https://github.com/github/copilot-cli/issues/4038) | **Non-interactive mode: empty user message injection** | Model answered empty turn instead of prompt; system prompt leakage observed | 3 comments; MCP tool count threshold (≥7) suggests scaling edge case |
| [#4524](https://github.com/github/copilot-cli/issues/4524) | **Sandbox blocks git access despite directory enablement** | New enforced sandbox over-restricts core developer workflow | 3 comments; immediate friction for security feature adoption |

---

## 4. Key PR Progress

Only **1 PR** updated in the tracking period:

| # | PR | Status | Significance |
|---|-----|--------|------------|
| [#4510](https://github.com/github/copilot-cli/pull/4510) | **Remove GitHub Copilot CLI documentation from README** | OPEN | Removes installation/usage guidance from README; may signal documentation migration to official docs site or restructuring of repo purpose. No community support (0 👍). Worth monitoring for project governance implications. |

*Note: Low PR velocity suggests team focus on issue resolution over new feature development.*

---

## 5. Feature Request Trends

| Theme | Evidence | Momentum |
|-------|----------|----------|
| **Session persistence & portability** | [#4539](https://github.com/github/copilot-cli/issues/4539) (Ctrl+Z loses sessions), [#4529](https://github.com/github/copilot-cli/issues/4529) (Remote-SSH empty transcripts), [#4543](https://github.com/github/copilot-cli/issues/4543) (WSL/Windows session split) | High — environment-agnostic session handling is repeatedly requested |
| **Enhanced `/ask` and multi-turn workflows** | [#4538](https://github.com/github/copilot-cli/issues/4538) (multi-turn `/ask`), [#4541](https://github.com/github/copilot-cli/issues/4541) (queue editor add/pause), [#4544](https://github.com/github/copilot-cli/issues/4544) (paste images in questions) | Medium — users want richer interactive modalities without polluting main history |
| **Configuration persistence** | [#4530](https://github.com/github/copilot-cli/issues/4530) (Reasoning Effort persistence) | Low but specific — settings granularity expanding beyond model selection |
| **Sandbox flexibility** | [#4524](https://github.com/github/copilot-cli/issues/4524) (git access), [#4546](https://github.com/github/copilot-cli/issues/4546) (WSL VS Code remote) | High — security feature adoption blocked by workflow breakage |

---

## 6. Developer Pain Points

### 🔴 Critical: Environment Fragmentation
WSL, Remote-SSH, and Windows host environments create **split session stores**, **path resolution failures** ([#4540](https://github.com/github/copilot-cli/issues/4540): `wta.exe` quote mishandling in "Program Files"), and **tool discovery gaps**. The CLI's platform abstraction leaks at the filesystem and process boundaries.

### 🟡 High: Sandbox Over-Restriction
New enforced sandbox mode breaks:
- Cross-session state sharing (drove users to enable `~/.copilot`)
- Git operations even in explicitly allowed directories ([#4524](https://github.com/github/copilot-cli/issues/4524))
- VS Code remote launch from WSL ([#4546](https://github.com/github/copilot-cli/issues/4546))

Security without workflow compatibility is causing rollback pressure.

### 🟡 High: MCP Integration Fragility
OAuth token bridging ([#4096](https://github.com/github/copilot-cli/issues/4096)), registry validation ([#3162](https://github.com/github/copilot-cli/issues/3162)), and OIDC issuer strictness ([#4439](https://github.com/github/copilot-cli/issues/4439)) create a **"connected but unusable"** failure mode. Enterprise MCP adoption is gated on reliability.

### 🟢 Medium: Memory/Context System Defects
`store_memory` failing without instance ID ([#4535](https://github.com/github/copilot-cli/issues/4535)) and personal skills never discovered ([#4545](https://github.com/github/copilot-cli/issues/4545)) suggest the context/memory layer is **incomplete in prereleases** — risky for v1.0.81 stability.

---

*Digest compiled from github.com/github/copilot-cli activity 2026-08-20/21.*

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-21

## 1. Today's Highlights

No new releases today, but community activity centers on two critical threads: a **severe background task lifecycle bug** that allows orphaned subagents to consume LLM quota invisibly, and continued momentum around **workspace-scoped memory plugins** with a new security-focused documentation PR. The memory plugin ecosystem is maturing from experimental to production-ready with explicit MCP server compatibility.

---

## 2. Releases

*No releases in the last 24 hours.*

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Signal |
|---|-------|--------------|----------------|
| [#2615](https://github.com/MoonshotAI/kimi-cli/issues/2615) | **[Bug] Background subagent keeps making LLM calls after TaskStop/timeout marks it terminal** | **Critical resource leak.** Orphaned subagents bypass task tracking, making quota consumption undetectable and unstoppable. Affects cost control and reliability for any multi-agent workflows. | 🔴 **New, unconfirmed** — zero engagement suggests this may be underreported or recently introduced. Requires immediate maintainer triage. |
| [#2613](https://github.com/MoonshotAI/kimi-cli/issues/2613) | **[enhancement] 提案：Kimi Memory Plus — 工作区范围的长期记忆插件** | **Ecosystem expansion.** Proposes standardized workspace-scoped long-term memory via MCP server, with explicit compatibility note that current CLI can register tools but not experimental `kimi:` protocol extensions. Bridges gap between experimental and stable APIs. | 🟡 **Author-updated** — active refinement; compatibility caveat suggests close tracking of upstream MCP support. |

*Only 2 issues updated in last 24h; both included.*

---

## 4. Key PR Progress

| # | PR | Description | Status |
|---|-----|-------------|--------|
| [#2614](https://github.com/MoonshotAI/kimi-cli/pull/2614) | **docs(plugins): document security and persistent data** | Hardens plugin trust model: documents subprocess sandboxing boundaries (user-level file/network access), credential injection risks, directory replacement on reinstall, and recommends isolated data directories. Essential for enterprise adoption of plugin ecosystem. | 🟢 **Open** — foundational security documentation; likely fast-track candidate |

*Only 1 PR updated in last 24h.*

---

## 5. Feature Request Trends

**Memory & State Persistence** — Dominant theme. The "Kimi Memory Plus" proposal (#2613) reflects demand for:
- **Workspace-scoped** (not just session-scoped) context retention
- **MCP-native** integration rather than custom protocols
- **Explicit memory tools** as first-class plugins, not hidden internals

**Security Transparency** — PR #2614 signals community push for documented guarantees around plugin isolation and credential handling, suggesting production deployments are beginning.

---

## 6. Developer Pain Points

| Pain Point | Evidence | Severity |
|-----------|----------|----------|
| **Invisible resource consumption** | #2615 — orphaned subagents continue LLM calls untracked | 🔴 **Critical** — breaks cost predictability and termination guarantees |
| **Memory fragmentation across sessions** | #2613 — users building ad-hoc workarounds for lack of native long-term memory | 🟡 **Moderate-High** — productivity friction, workaround complexity |
| **Plugin security ambiguity** | #2614 — documentation PR needed because runtime behavior was underspecified | 🟡 **Moderate** — adoption blocker for security-conscious teams |
| **Protocol compatibility gaps** | #2613 compatibility note — experimental `kimi:` prefix not recognized by stable CLI | 🟡 **Moderate** — fragments plugin ecosystem between bleeding-edge and stable |

---

*Digest compiled from github.com/MoonshotAI/kimi-cli. For real-time updates: [Issues](https://github.com/MoonshotAI/kimi-cli/issues) | [PRs](https://github.com/MoonshotAI/kimi-cli/pulls) | [Releases](https://github.com/MoonshotAI/kimi-cli/releases)*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-21

## Today's Highlights

OpenCode shipped **v1.18.19** with native Cloudflare AI Gateway passthroughs and tightened Codex rate-limit parity. The community closed 28 issues in 24 hours, with heavy focus on subagent reliability, model provider edge cases, and TUI/desktop polish. A notable surge in Chinese-language issues around DeepSeek model compatibility and desktop sidecar failures signals growing regional adoption friction.

---

## Releases

### [v1.18.19](https://github.com/anomalyco/opencode/releases/tag/v1.18.19)

| Change | Impact |
|--------|--------|
| Native OpenAI/Anthropic passthroughs for Cloudflare AI Gateway | Simplifies enterprise proxy setups; removes manual configuration workarounds |
| Codex rate limits matched to ChatGPT subscription tiers | Reduces throttling for paid users; community contribution by @GameOn223 |
| Removed built-in Qwen sampling defaults | Fixes provider errors from unsupported parameters being sent |
| Bugfix for incomplete property handling | (Note: release notes truncated in source) |

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#33140](https://github.com/anomalyco/opencode/issues/33140) | Skip session title generation | Local model users face 2x latency from unnecessary LLM calls; now configurable | 6 👍, 5 comments; merged as feature |
| [#33006](https://github.com/anomalyco/opencode/issues/33006) | OpenAI Responses 500 doom-loop with too many tools | Unbounded retry on 500s bricks sessions; affects MCP-heavy workflows | 4 comments; fixed retry policy |
| [#33228](https://github.com/anomalyco/opencode/issues/33228) | Secret files copied to world-readable dirs | Security-critical: `.env`, private keys leaked by broad copy operations | 3 comments; closed with fix |
| [#32829](https://github.com/anomalyco/opencode/issues/32829) | DeepSeek MCP `$ref`/`$defs` schema crash | Breaks Asana, Notion MCP servers; JSON schema incompatibility | 4 comments; resolved |
| [#33043](https://github.com/anomalyco/opencode/issues/33043) | Subagent `model=undefined` crash | All built-in subagents fail (`general`, `explore`, `code-reviewer`); core task tool broken | 3 comments, 1 👍; fixed |
| [#33055](https://github.com/anomalyco/opencode/issues/33055) | ACP session hangs on API error | 100% CPU infinite retry; process kill required | 3 comments; closed |
| [#43756](https://github.com/anomalyco/opencode/issues/43756) | `TextNodeRenderable` type crash | Fatal TUI rendering error blocking startup | 3 comments; **OPEN** — active |
| [#43751](https://github.com/anomalyco/opencode/issues/43751) | DeepSeek Flash `max_tokens` mismatch | 384K vs 131K token limit discrepancy in DeepSeek harness | 2 comments; **OPEN** |
| [#31804](https://github.com/anomalyco/opencode/issues/31804) | File tree cache persists after deletion | Stale toasts for deleted folders; desktop UX polish | 4 comments, 1 👍; fixed |
| [#43762](https://github.com/anomalyco/opencode/issues/43762) | Sidecar timeout on Windows desktop | 60s startup failure; blocks all desktop usage for affected users | 1 comment; **OPEN**, `needs:compliance` |

---

## Key PR Progress

| # | PR | What It Does | Status |
|---|-----|-------------|--------|
| [#43761](https://github.com/anomalyco/opencode/pull/43761) | Accept nullable Anthropic input usage | Fixes crash from `message_delta.usage.input_tokens=null` per official SDK spec | **Merged** |
| [#43763](https://github.com/anomalyco/opencode/pull/43763) | Restore shell tool fallback | Brings back V1's safe shell fallback for agents; keeps terminal-only shells for direct use | Open |
| [#43702](https://github.com/anomalyco/opencode/pull/43702) | Use small model for titles | Auto-selects lightweight model for session titles; cuts cost/latency | Open |
| [#41864](https://github.com/anomalyco/opencode/pull/41864) | Desktop voice input | Local Whisper transcription + optional cloud fallback; major UX feature | Open (10 days) |
| [#43757](https://github.com/anomalyco/opencode/pull/43757) | Continue interrupted model streams | Treats transport failures as resumable; fixes subagent death on partial reasoning | Open |
| [#43747](https://github.com/anomalyco/opencode/pull/43747) | Load global compatibility skill sources | Auto-discovers `~/.claude/skills`, `~/.agents/skills`; watches for late creation | Open |
| [#43754](https://github.com/anomalyco/opencode/pull/43754) | Desktop prompt images + patch action labels | Renders TUI images in desktop timeline; corrects "Created"/"Removed"/"Patch" labels | Open |
| [#43724](https://github.com/anomalyco/opencode/pull/43724) | Steer manual compaction by default | `/compact` now runs at step boundary instead of waiting full turn queue | **Merged** |
| [#42980](https://github.com/anomalyco/opencode/pull/42980) | Reduce Windows server CPU under parallel sessions | 88% throughput gain, 48% CPU reduction via process spawn optimization | **Merged** |
| [#43738](https://github.com/anomalyco/opencode/pull/43738) | Speed up cold home navigation | Cuts desktop Home navigation from ~618ms to ~86ms via query cache fix | **Merged** |

---

## Feature Request Trends

1. **Session management UX** — Skip title generation (#33140), omit long skill prompts (#25926), checkpoints/undo (#33286), and command audit trails (#33295) all point to users wrestling with context overhead in long sessions.

2. **Skill system visibility** — Loaded skills in TUI sidebar (#33221), keyboard shortcuts for skill selector (#33296), and built-in compiled skills (#26342) show demand for making the skill system more discoverable and ergonomic.

3. **AXI/MCP ecosystem unification** — Surface AXI CLI tools alongside MCP servers (#32993) and global skill source compatibility (#43747) indicate push toward consolidating fragmented tool/plugin formats.

4. **Voice and multimodal input** — Voice input PR (#41864) is the flagship, but underlying theme is reducing friction in input modalities beyond typing.

---

## Developer Pain Points

| Theme | Evidence | Severity |
|-------|----------|----------|
| **Model provider fragility** | DeepSeek token limits (#43751), GLM-5.2 regressions (#33280), Anthropic null handling (#43761), Copilot prefill errors (#31807), Bedrock credential issues (#43681) | 🔴 High — constant whack-a-mole with provider quirks |
| **Subagent/task tool reliability** | `model=undefined` (#33043), hangs on API error (#33055), interrupted streams (#43757), resumable errors (#43657) | 🔴 High — core agentic feature is brittle |
| **Desktop/sidecar stability** | Windows sidecar timeout (#43762), white screen (#33278), session loss on restart (#33277), Feishu orphaned CPU loops (#33050) | 🟡 Medium-High — platform-specific, but blocking |
| **Security footguns** | Secret file copying (#33228), world-readable directories | 🔴 High — single misconfiguration = data exposure |
| **Observability gaps** | OTLP protobuf ignored (#33101), missing protocol respect | 🟡 Medium — breaks enterprise telemetry stacks |
| **TUI rendering edge cases** | `TextNodeRenderable` crash (#43756), block tool error visuals (#43749) | 🟡 Medium — polish, but fatal when hit |

---

*Digest compiled from github.com/anomalyco/opencode activity on 2026-08-21.*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-21

## Today's Highlights
The Pi community closed a wave of long-standing UX papercuts, with `/exit` and `/config` aliases finally landing after months of scattered requests. Windows terminal rendering remains the top unresolved pain point, with input line corruption drawing significant discussion. Meanwhile, a major TUI theming refactor by Armin Ronacher (mitsuhiko) signals deeper UI customization ahead.

---

## Releases
*No releases in the last 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#7547](https://github.com/earendil-works/pi/issues/7547) | **Windows usage survey** — Consolidating Windows deployment patterns | Windows is the largest untapped developer market; fragmentation across WSL, native, and containerized installs blocks focused investment | 36 comments, active maintainer engagement seeking signal from noise |
| [#5023](https://github.com/earendil-works/pi/issues/5023) | **Terminal scroll-to-beginning bug** | Random viewport jumps destroy context during long model outputs; now closed but was highly disruptive | 17 comments, fix validated by reporters |
| [#3442](https://github.com/earendil-works/pi/issues/3442) | **WebSocket transport for openai-responses** | Enables real-time bidirectional streaming, critical for latency-sensitive applications; closes gap with OpenAI's native capabilities | 9 comments, merged after provider-side unblock |
| [#6300](https://github.com/earendil-works/pi/issues/6300) | **Windows: Input line redrawn per keystroke** | Makes Pi unusable on stock Windows terminals; cmd.exe and Windows Terminal both affected | 8 comments, repro confirmed, awaiting root-cause analysis |
| [#8157](https://github.com/earendil-works/pi/issues/8157) | **Migrate grok-mermaid → lovely-mermaid** | grok-mermaid inherited upstream bugs; lovely-mermaid offers spec-compliant parsing with human curation | 7 comments, author of lovely-mermaid driving migration |
| [#6996](https://github.com/earendil-works/pi/issues/6996) | **Gemini 3.x tool-use failure: missing `thought_signature`** | Blocks Google's latest model family from reliable agentic use; signature handling diverged from Gemini API contract | 5 comments, reproduction confirmed, needs provider fix |
| [#8133](https://github.com/earendil-works/pi/issues/8133) | **Per-model compaction settings** | Different models have wildly different context economics; one-size-fits-all compaction wastes tokens or truncates prematurely | 3 comments, 3 upvotes, well-specified proposal |
| [#8409](https://github.com/earendil-works/pi/issues/8409) | **Regression: aborted turns report `stopReason: "error"`** | Corrupts session telemetry and downstream automation that depends on accurate lifecycle states | 3 comments, timing-dependent race condition |
| [#8344](https://github.com/earendil-works/pi/issues/8344) | **Per-tool output expansion in fullscreen TUI** | Long sessions accumulate dozens of tool outputs; global expand/collapse is too coarse | 4 comments, closed as `no-action` — design divergence from maintainer vision |
| [#8417](https://github.com/earendil-works/pi/issues/8417) | **SSH passphrase prompt overlays TUI** | Background git package update check blocks on SSH auth, corrupting terminal state | 2 comments, security-sensitive fix needed |

---

## Key PR Progress

| # | PR | Feature / Fix | Status |
|---|-----|-------------|--------|
| [#8398](https://github.com/earendil-works/pi/pull/8398) | **Color values and theme styling** (mitsuhiko) | Exposes raw color values to TUI themes; enables agent-driven dynamic styling and future non-terminal UIs. Backward-compatible API layering. | Open — significant architectural change |
| [#8302](https://github.com/earendil-works/pi/pull/8302) | **Amazon Bedrock Mantle support** (cristinaponcela) | Adds GPT models via Amazon's new Mantle API surface; existing Converse routing fails for these model IDs. WIP pending e2e testing. | Open |
| [#8118](https://github.com/earendil-works/pi/pull/8118) | **`requiresNonNullAssistantContent` compat flag** (gaoyk19) | Fixes OpenAI-compatible gateways that reject null-content assistant messages in tool-call-only turns. Precise, surgical fix. | Open |
| [#8416](https://github.com/earendil-works/pi/pull/8416) | **Hold `triggerTurn: false` custom messages until tool batch ends** (BetterAndBetterII) | Prevents custom messages from interleaving between `toolCall` and `toolResult`, which strict providers reject. Race condition fix. | Closed |
| [#8405](https://github.com/earendil-works/pi/pull/8405) | **Normalize kimi-coding thinking signatures to base64url** (ytspar) | Fixes recurring `invalid base64url encoding` errors on multi-turn reasoning with Moonshot's Kimi models. | Closed |
| [#8407](https://github.com/earendil-works/pi/pull/8407) | **Preserve logical lines when copying soft-wrapped text** (smrnjeet222) | Copies original content lines instead of viewport-broken visual rows. Fixes paragraph/URL corruption in clipboard. | Closed |
| [#8399](https://github.com/earendil-works/pi/pull/8399) | **Searchable "default" label in `/model` and `/thinking`** (cristinaponcela) | Surfaces which model/thinking config is persisted as default; `ctrl+S` workflow now discoverable. | Closed |
| [#8395](https://github.com/earendil-works/pi/pull/8395) | **Prevent TUI crash on large diffs** (Battleplus) | Replaces spread-operator push with loop to avoid V8 stack overflow on ~14.5MB diffs. Defensive rendering fix. | Closed |
| [#8363](https://github.com/earendil-works/pi/pull/8363) | **Fix wrapped table link color leaks** (rwachtler) | Resets ANSI link colors before table padding/borders; fixes visual corruption in markdown tables. | Closed |
| [#4537](https://github.com/earendil-works/pi/pull/4537) | **`/exit` alias for `/quit`** (AttAditya) | Closes months of fragmented requests. Minimal implementation matching `/quit` behavior exactly. | Closed |

---

## Feature Request Trends

1. **Command alias convergence with Claude Code / Codex** — `/exit`, `/bye`, `/config` as aliases for `/quit`, `/settings`. Muscle memory from competing tools is the dominant driver; now largely resolved.
2. **Granular TUI interaction model** — Per-tool expansion, configurable scroll rates, mouse-driven behaviors. Users want Emacs/VS Code-level UI control inside terminal constraints.
3. **Provider ecosystem expansion** — Umans AI, OpenAI Daybreak, Amazon Mantle, Kimi-coding fixes. Multi-provider robustness is strategic priority.
4. **Model-specific configuration** — Compaction, pricing, capability flags keyed by model ID rather than global defaults.
5. **Extension lifecycle and safety** — Settled-safe session control, theme change events, tool-name conflict resolution. Extension platform maturation.

---

## Developer Pain Points

| Theme | Frequency | Details |
|-------|-----------|---------|
| **Windows terminal fidelity** | Very High | Input redraw (#6300), scroll jumps (#5023), general "how to run" confusion (#7547). Windows is second-class despite large addressable market. |
| **Silent failure on unknown slash commands** | High | `/exit` sent to model as chat message, burning tokens and polluting context. Partially fixed; warning behavior still debated. |
| **Session/cache continuity across forks** | Medium | Forked sessions lose prompt cache hits due to new session IDs; wastes latency and cost on long conversations. |
| **Tool output ergonomics at scale** | Medium | Global expand/collapse insufficient for long sessions; per-tool control requested and rejected as `no-action`. |
| **Provider compatibility papercuts** | Medium | Gemini signatures, Anthropic scoped keys, OpenAI gateway null-content rules. Each requires bespoke compat flag or normalization. |
| **Background operation interference** | Low but sharp | Git update checks prompting for SSH passphrases over TUI; NTP sync inflating bash elapsed times. Async hygiene gaps. |

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-21

## 1. Today's Highlights

Qwen Code shipped **v0.21.15** stable with major Web Shell enhancements including file attachment support via composer and improved streaming performance. The release pipeline shows strong validation discipline with four consecutive SWE-bench Verified smoke tests passing across v0.21.14 and v0.21.15. Meanwhile, the `/review` skill is undergoing intensive hardening with 7+ active PRs addressing convergence loops, Aone Code integration gaps, and security audit coverage.

---

## 2. Releases

| Version | Type | Key Changes |
|---------|------|-------------|
| **[v0.21.15](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.15)** | Stable | Web Shell file attachments via composer/@ selection; improved streaming performance; immediate sidebar synchronization ([#9405](https://github.com/QwenLM/qwen-code/pull/9405), [#9477](https://github.com/QwenLM/qwen-code/pull/9477)) |
| **[v0.21.14-nightly.20260821.9f2342d323](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.14-nightly.20260821.9f2342d323)** | Nightly | Review loop settlement diagnostics ([#9461](https://github.com/QwenLM/qwen-code/pull/9461)); CI fallback fix |
| **[v0.21.11-nightly.20260820.b414f135fa](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.11-nightly.20260820.b414f135fa)** | Nightly | Web Shell approval/ask-user dialogs as in-flow sheets; background-agent false failure fix |

**Validation:** Four DSW EAS smoke tests (r1–r3 for v0.21.14, r1 for v0.21.15) all **SUCCEEDED** on SWE-bench Verified and Terminal-Bench 2.0.

---

## 3. Hot Issues

| Issue | Why It Matters | Community Signal |
|-------|--------------|----------------|
| **[#9278](https://github.com/QwenLM/qwen-code/issues/9278)** Design: `/review` publish-time convergence advisory | Documents the core "runaway review loop" problem where fix rounds amplify diffs rather than converge. Critical for autonomous code review reliability. | 8 comments, active design iteration by `wenshao` |
| **[#8382](https://github.com/QwenLM/qwen-code/issues/8382)** Duplicate provider tool call id | Recurring production error breaking tool execution; affects multiple providers. | 7 comments, `need-retesting` — persistent but hard to reproduce |
| **[#8724](https://github.com/QwenLM/qwen-code/issues/8724)** Cross-session messaging | Enables multi-agent orchestration on single machine — foundational for distributed workflows. | 7 comments, well-scoped design with fail-closed security gate |
| **[#9309](https://github.com/QwenLM/qwen-code/issues/9309)** Compression logic appears incorrect | User-reported data integrity issue: `/compress-fast` then `/compress` yields suspicious token counts. | 6 comments, needs reproduction validation |
| **[#2128](https://github.com/QwenLM/qwen-code/issues/2128)** Unbounded memory growth in long sessions | P1 bug open since March; root cause identified (unbounded UI History array) but fix pending. | 5 comments, affects production long-running sessions |
| **[#9556](https://github.com/QwenLM/qwen-code/issues/9556)** Review pipeline code execution privilege | Security design question: whether review should retain invoking-user execution rights. | 5 comments, `need-discussion` — governance-critical |
| **[#9573](https://github.com/QwenLM/qwen-code/issues/9573)** Resumed sessions show false "Tool result missing" | Data loss on session resume corrupts user trust in persistence. | 3 comments, P1 priority |
| **[#9485](https://github.com/QwenLM/qwen-code/issues/9485)** Web Shell clipboard fails over HTTP/non-localhost | Common remote dev setup broken; fixed but pattern indicates Web Shell hardening gaps. | 5 comments, **closed** |
| **[#9586](https://github.com/QwenLM/qwen-code/issues/9586)** ACP duplicate tool-call breaker leaves orphaned calls | Daemon reliability: circuit breaker can corrupt persisted state. | 4 comments, **closed** |
| **[#9620](https://github.com/QwenLM/qwen-code/issues/9620)** Aone Code branch-based MRs break write path | Blocks enterprise adoption: assumes AGit-Flow single-commit CRs only. | 2 comments, just opened |

---

## 4. Key PR Progress

| PR | Feature / Fix | Significance |
|----|-------------|------------|
| **[#9631](https://github.com/QwenLM/qwen-code/pull/9631)** fix(webui): keep observer pane loading across silent mid-turn gaps | Fixes UI state desync for hosted/scheduled runs and page refreshes — reliability for multi-pane workflows |
| **[#9626](https://github.com/QwenLM/qwen-code/pull/9626)** fix(serve): unify persisted session storage lifecycle | Completes boundary from #9513: canonical logical IDs vs. filesystem spellings — prevents identity bugs |
| **[#9632](https://github.com/QwenLM/qwen-code/pull/9632)** feat(web-shell): keep turn expanded while background shell runs | UX polish: visual continuity for backgrounded operations |
| **[#9633](https://github.com/QwenLM/qwen-code/pull/9633)** fix(review): audit Aone targets in cleanup bypass tripwire | Security parity: extends GitHub-only bypass detection to Aone Code |
| **[#9526](https://github.com/QwenLM/qwen-code/pull/9526)** feat(review): persistently-critical convergence advisory | **Core reliability**: detects stuck review loops and surfaces "land-with-residual-risk" advisory |
| **[#9584](https://github.com/QwenLM/qwen-code/pull/9584)** chore(deps): Clear high-severity CVE baseline | Hardens security gate from reporting-only to blocking; upgrades OTel stack |
| **[#9625](https://github.com/QwenLM/qwen-code/pull/9625)** feat(review): disclose Aone posts join discussion gate only | Transparency: Aone comments lack AI flag, only block via general discussion gate |
| **[#9596](https://github.com/QwenLM/qwen-code/pull/9596)** feat(review): ask each fix for its test, rule on non-convergence | **Loop reduction**: findings carry acceptance criteria; tests required for fixes |
| **[#9591](https://github.com/QwenLM/qwen-code/pull/9591)** feat(models): support dual-role image generation models | Flexibility: one route serves both chat and image generation |
| **[#8992](https://github.com/QwenLM/qwen-code/pull/8992)** feat(mcp): add MCP 2026 core and WebShell Apps host | Protocol modernization: MCP 2026 client + Apps host for extensibility |

---

## 5. Feature Request Trends

| Trend | Evidence | Maturity |
|-------|----------|----------|
| **Aone Code enterprise integration** | 7 open issues/PRs (#9613–#9618, #9620, #9625, #9633) covering comment dedup, self-PR detection, incremental cache, inline anchoring, AI-comment flags, branch-based MRs, bypass audits | Active development — filling parity gaps vs. GitHub path |
| **Review loop convergence control** | #9278 (design), #9526, #9596, #9262 — telemetry, advisories, test requirements, growth-budget auditing | **Top priority** — reliability of autonomous review |
| **Cross-session / multi-agent orchestration** | #8724 (messaging), #7802 (agent view CLI), #8583 (workflow cockpit), #8927 (session rotation) | Foundation-building for distributed agent systems |
| **Provider-aware reasoning controls** | #9590 (DeepSeek V4, GLM 5.2, Kimi) + #9591 (dual-role image models) | Keeping pace with model ecosystem diversity |
| **MCP protocol adoption** | #8992 — MCP 2026 client and Apps host | Strategic standardization bet |

---

## 6. Developer Pain Points

| Pain Point | Frequency | Indicators | Mitigation Status |
|------------|-----------|------------|-----------------|
| **Session persistence / resume bugs** | High | #9573, #9586, #8382, #2128 — "missing tool result", duplicate IDs, unbounded memory | Partial: fixes landing in v0.21.14–0.21.15, but #2128 (March) still open |
| **Web Shell focus/UX race conditions** | High | #9571, #9611, #9487, #9562 — confirmation box focus grabs, loading indicator drops, catalog refresh loops | Active: #9609, #9631, #9632 in flight |
| **Review loop non-convergence** | High | #9278, #9526, #9596, #9461 — "runaway" rounds, Criticals that won't clear | In progress: telemetry + advisory mechanisms |
| **Compression / token management correctness** | Medium | #9309 — suspicious token counts; #7306 (closed) tool-output budgeting | Needs validation: #9309 open with 6 comments |
| **Enterprise SCM integration gaps** | Medium | Aone Code cluster — branch MRs, comment dedup, caching all missing vs. GitHub | Sprint: 7 items opened 2026-08-20 alone |
| **Security hardening in CI/CD** | Medium | #9480 (wipe guard), #9584 (CVE gate), #9577 (install scripts) | Improving: gate moved from report to block |

---

*Digest compiled from github.com/QwenLM/qwen-code public activity on 2026-08-21.*

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-08-21

> **Note on naming:** The project has rebranded to **CodeWhale** (Shannon Labs). The legacy `deepseek-tui` npm package is deprecated; all new development occurs under the `codewhale` identifier.

---

## 1. Today's Highlights

The v0.9.10 release marks the official CodeWhale rebrand with deprecated legacy assets. Active development is accelerating on three fronts: crate decomposition for modularity (EPIC-005), on-demand LSP diagnostics tooling, and aggressive UX simplification to reduce first-run psychological friction for non-English users.

---

## 2. Releases

| Version | Details |
|---------|---------|
| **[v0.9.10](https://github.com/Hmbown/CodeWhale/releases/tag/v0.9.10)** | Official CodeWhale rebrand release. The `codewhale` command, npm package, and release assets are now the canonical lowercase identifiers. Legacy `deepseek-tui` npm package receives no further releases. Users on v0.8.x `deepseek`/`deepseek-tui` must migrate. |

---

## 3. Hot Issues

| # | Issue | Status | Why It Matters | Community Signal |
|---|-------|--------|--------------|----------------|
| [#5316](https://github.com/Hmbown/CodeWhale/issues/5316) | EPIC-005: CodeWhale TUI Crate Decomposition (Umbrella) | 🔵 OPEN | Foundational architectural refactor tracking 10+ sub-issues. Defines how the monolithic TUI crate splits into manageable, testable units. Critical for long-term contributor onboarding and build times. | 10 comments, active coordination by `aboimpinto` |
| [#4070](https://github.com/Hmbown/CodeWhale/issues/4070) | feat: standalone `read_lints` tool for on-demand diagnostics | 🔵 OPEN | Closes a gap where agents couldn't request linter/type errors for files they hadn't just edited—unlike Cursor/Copilot workflows. Enables proactive code quality checks without edit-side-effects. | 2 comments, approved scope with PR in flight |
| [#5345](https://github.com/Hmbown/CodeWhale/issues/5345) | [FR] 增加多行模式/自定义发送快捷键 | 🟢 CLOSED | Chinese user request for multi-line input mode (Enter=newline, Shift+Enter=send) matching Grok Build/Codex conventions. Signals localization UX expectations from core user base. | 2 comments, resolved |
| [#5526](https://github.com/Hmbown/CodeWhale/issues/5526) | Deprecated shell completion | 🔵 OPEN | PowerShell completion scripts still reference `codewhale-tui` instead of `codewhale`. Rebrand migration debt affecting daily CLI UX. No documented fix path. | 1 comment, user blocked |
| [#5482](https://github.com/Hmbown/CodeWhale/issues/5482) | EPIC(docs): localize documentation to Chinese | 🔵 OPEN | Recognition that English-only docs + stale content + MT errors create real barriers for Chinese-speaking majority user base. Partial restructure proposed. | 1 comment, substantial scope |
| [#5527](https://github.com/Hmbown/CodeWhale/issues/5527) | feat: NPC-style mention-driven agent on issues/PRs | 🔵 OPEN | Proposes @-mentionable bot personas (inspired by CNB Cloud Native Build) for automated issue/PR assistance. Could transform community support scalability. | New, no comments yet |
| [#5522](https://github.com/Hmbown/CodeWhale/issues/5522) | v0.9.10: make first run progressive instead of front-loading configuration | 🔵 OPEN | Direct user feedback: telemetry disclosure → settings wall → key hints before useful work creates "too much psychological cost." Release acceptance criteria defined. | New, no comments yet, high priority |

---

## 4. Key PR Progress

| # | PR | Status | What It Does | Technical Note |
|---|-----|--------|-----------|--------------|
| [#5524](https://github.com/Hmbown/CodeWhale/pull/5524) | feat(tui): add multi-file `read_lints` operation | 🔵 OPEN | Implements #4070 scope: model-visible `lsp` tool gains `read_lints` for multiple workspace files via existing `LspManager` transport pool—no new server lifecycles | Reuses session infrastructure; avoids resource leak pattern |
| [#5525](https://github.com/Hmbown/CodeWhale/pull/5525) | refactor(tui): adopt command shapes in utility group (FEAT-018) | 🔵 OPEN | Converts 7 utility commands to external command shapes from FEAT-014/015. Execution boundary change without physical file moves. | Part of EPIC-005 decomposition; `/a...` registration pattern |
| [#5523](https://github.com/Hmbown/CodeWhale/pull/5523) | refactor(tui): extract tool call stages from turn loop | 🔵 OPEN | Pure refactor: splits monolithic turn loop into `plan_tool_calls` → `execute_planned_tools` → `process_tool_results`. Preserves cancellation, mutable state flow, indexed outcomes. | Critical for readability and future async evolution |
| [#5520](https://github.com/Hmbown/CodeWhale/pull/5520) | feat(web): move docs/sandbox and docs/web onto dictionary spine | 🟢 CLOSED | Eliminates 29 `isZh` branches across two doc sections via dictionary-per-page pattern with `types.ts`/`index.ts` wiring. Adds to `check-locales.mjs` validation. | Continues #5337 localization infrastructure |
| [#5521](https://github.com/Hmbown/CodeWhale/pull/5521) | chore(tui): drop single-argument `concat!` | 🟢 CLOSED | Fixes `clippy::useless-concat` lint failure on `main` at 3b221c7a6. Trivial but unblocks CI. | Hygiene PR; clippy-driven maintenance |

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **Input modality parity with mainstream tools** | #5345 (multi-line/Codex-style keybindings) | High—closed quickly, suggests responsive maintainer priority |
| **Proactive/agentic diagnostics** | #4070 → #5524 (`read_lints`) | Medium—approved, in implementation |
| **GitHub-native automation (NPC agents)** | #5527 | Early—proposes competitive differentiation vs. CNB |
| **Progressive disclosure / reduced cognitive load** | #5522 (first-run), #5482 (docs restructuring) | High—explicitly tied to v0.9.10 release acceptance |
| **Architectural decomposition** | EPIC-005 (#5316), FEAT-018 (#5525), #5523 | Sustained—multi-month effort with active PR velocity |

---

## 6. Developer Pain Points

| Pain Point | Manifestations | Severity |
|-----------|---------------|----------|
| **Rebrand migration friction** | #5526 (stale shell completions referencing `codewhale-tui`), deprecated npm package confusion | 🔴 High—breaks CLI UX for existing users |
| **First-run abandonment risk** | #5522: "too much psychological cost" from sequential disclosures/settings before value | 🔴 High—explicitly flagged as release blocker |
| **Documentation i18n debt** | #5482: stale English docs, MT errors, barrier for Chinese-speaking majority | 🟡 Medium—large scope, partially addressed by dictionary spine work (#5520) |
| **Monolithic crate maintainability** | EPIC-005: build times, contributor onboarding, testability all suffer | 🟡 Medium—active mitigation in progress |
| **LSP integration gaps** | #4070: inability to query diagnostics without edit side-effects | 🟢 Resolving—PR #5524 in flight |

---

*Digest compiled from github.com/Hmbown/CodeWhale activity through 2026-08-21.*

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*