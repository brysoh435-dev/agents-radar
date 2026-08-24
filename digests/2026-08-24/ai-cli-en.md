# AI CLI Tools Community Digest 2026-08-24

> Generated: 2026-08-24 03:32 UTC | Tools covered: 10

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
## Community Digest Analysis — 2026-08-24

---

## 1. Ecosystem Overview

The AI CLI tools landscape in late 2026 is characterized by **intense competition around agent reliability and cross-session persistence**, with all major tools grappling with production-grade stability challenges. Claude Code and OpenAI Codex lead in raw community scale, while newer entrants (Qwen Code, CodeWhale/DeepSeek TUI) differentiate through specialized workflows—code review infrastructure and subagent fleet governance respectively. A notable convergence is underway: every tool is investing in **MCP ecosystem support**, **Windows platform parity**, and **memory/context architecture**, suggesting these have become table stakes rather than differentiators. Meanwhile, **metering transparency and trust erosion** have emerged as unexpected competitive battlegrounds, with Kimi CLI and Claude Code both facing serious user backlash on billing practices.

---

## 2. Activity Comparison

| Tool | Issues Active (24h) | PRs Active (24h) | Releases | Release Notes |
|------|---------------------|------------------|----------|---------------|
| **Claude Code** | 10 highlighted + ongoing | 1 | None | Stabilization period; no shipping |
| **OpenAI Codex** | 10 highlighted + ongoing | 20+ internal | rust-v0.149.1 + alpha | Patch release; heavy internal annotation sprint |
| **Gemini CLI** | 10 highlighted (50 total) | 9 | v0.56.0-nightly | Automated nightly; no functional changes |
| **GitHub Copilot CLI** | 10 highlighted | 1 (spam/invalid) | v1.0.81-8 | xhigh reasoning for Grok 4.6; live plugin reload |
| **Kimi CLI** | 3 highlighted | 3 | None | No shipping; metering controversy |
| **OpenCode** | 10 highlighted | 10 | None | Critical surrogate sanitization fix merged |
| **Pi** | 10 highlighted | 10 | None | v0.84.2+ reliability patch (recent) |
| **Qwen Code** | 10 highlighted | 10+ stacked | v0.22.0-nightly | Web Shell CWD fixes; VS Code cutover in progress |
| **CodeWhale** (ex-DeepSeek TUI) | 10 highlighted (50 total) | 18 (4 new from M-Maciej) | v0.9.11 | Rebrand complete; v0.9.12 integration active |
| **Grok Build** | 0 | 0 | None | No activity |

---

## 3. Shared Feature Directions

| Requirement | Tools Demanding | Specific Needs |
|-------------|---------------|--------------|
| **Persistent Memory / Cross-Session Identity** | Claude Code (#85557, #39195), Kimi CLI (#1283), Gemini CLI (#18836, #21000), Pi (#7724 cold restore) | Portable experience memory; shared project subsets; AI-managed + user-defined layers; account-level sync |
| **Windows Platform Parity** | Codex (10+ issues), Gemini CLI (#28926, #28832), Pi (#8512 PowerShell, #8544 CRLF), Qwen Code (#8625 IME), Copilot CLI (#4570 file locks) | Path normalization; terminal rendering; sandbox helper reliability; file locking; IME support; native shell alternatives |
| **MCP Ecosystem Robustness** | Codex (#30408 leaks, #38754 respawn), Gemini CLI (#28839 schema normalization, #24246 tool limits), Pi (#7740 /reload breaks renderers) | Process lifecycle management; schema validation; >128 tool handling; live config reloading |
| **TUI Transparency & Control** | Codex (#39903 command collapsing), Claude Code (#86110 banner UX), OpenCode (#30587 scroll buffer), Pi (#8032 mouse events) | Configurable verbosity; scroll persistence; command visibility; interactive components |
| **Rate-Limit / Metering Transparency** | Codex (#37445 silent drain, #39808 subagent overhead), Kimi CLI (#1994, #2604 disputed accounting) | Granular usage attribution; real-time budget visibility; fair billing for thinking chains; policy change announcements |
| **Subagent/Fleet Governance** | CodeWhale (#5575 role SSoT, #5596 silent cancellation), Gemini CLI (#22323 false success, #21409 hangs), Claude Code (Cowork proxy #76248) | Single source of truth for roles; accurate success/failure reporting; graceful interruption; approval scope durability |

---

## 4. Differentiation Analysis

| Tool | Core Focus | Target User | Technical Approach |
|------|-----------|-------------|------------------|
| **Claude Code** | Controllable, style-aware AI assistance | Professional developers, enterprise teams | Anthropic model integration; Cowork remote dev; skills system; memory architecture (evolving) |
| **OpenAI Codex** | Scalable agent infrastructure, annotation integrity | Platform builders, ChatGPT Work automation | Rust core; heavy internal taxonomy/annotation system; MongoDB persistence experiments; sandbox security |
| **Gemini CLI** | Agent reliability, Google ecosystem integration | GCP/Vertex users, multi-agent workflows | Subagent-heavy architecture; ACP protocol; AST-aware tooling roadmap; nightly release cadence |
| **GitHub Copilot CLI** | IDE-adjacent developer productivity, plugin ecosystem | VS Code users, GitHub-centric workflows | ACP protocol compliance; live plugin development; model auto-routing; xhigh reasoning features |
| **Kimi CLI** | Cost-efficient long-context coding | Price-sensitive developers, China market | K2.6 extended thinking; token-based metering; phone pairing (gbr/1); emerging plugin ecosystem |
| **OpenCode** | Provider-agnostic, open AI abstraction | Multi-model users, self-hosters | Bun/Node runtime; broad provider support (Bedrock, Cloudflare, Anthropic, Gemini); VS Code diff integration; ACP v2 early adoption |
| **Pi** | Extensible local-first agent platform | Extension authors, on-premise deployers | llama.cpp native integration; TUI component system; npm package ecosystem; settings atomicity focus |
| **Qwen Code** | Enterprise code review, deterministic workflows | Enterprise engineering teams, PR reviewers | Workflow engine (deterministic vs. model-driven); containerized execution; audit lenses; Web Shell architecture |
| **CodeWhale** | Subagent fleet orchestration, spend governance | High-scale automation, safety-critical deployments | Rust core; control sockets; lifecycle outbox; structured context contracts; OAuth 2.1 for MCP |

**Key Differentiators:**
- **Qwen Code** is alone in treating **code review as a first-class workflow** with containerized execution and audit lenses
- **CodeWhale** pushes furthest on **subagent governance architecture** (role SSoT, approval receipts, spend bounding)
- **OpenCode** maintains the **broadest provider neutrality** with active ACP v2 standard participation
- **Pi** emphasizes **local inference and extension ecosystem** over cloud dependency
- **Kimi CLI** is most exposed on **pricing model clarity**, creating vulnerability versus competitors

---

## 5. Community Momentum & Maturity

| Tier | Tools | Evidence |
|------|-------|----------|
| **Highest Activity / Rapid Iteration** | OpenAI Codex (20+ PRs), CodeWhale (50 issues, 18 PRs, rebrand), OpenCode (10 PRs, critical fixes), Qwen Code (stacked PRs for VS Code cutover) | Codex: internal annotation sprint; CodeWhale: v0.9.12 milestone with P0 tracker; OpenCode: daily critical reliability fixes; Qwen Code: multi-PR architectural cutover |
| **Steady State / Maintenance Mode** | Claude Code, Gemini CLI, Pi | Claude Code: 1 PR in 24h, quality-focused triage; Gemini CLI: automated nightlies, P1 bug backlog; Pi: v0.84.2 reliability patch, incremental improvements |
| **At Risk / Trust Erosion** | Kimi CLI, GitHub Copilot CLI | Kimi CLI: metering controversy with data-driven user reports, no official response; Copilot CLI: intermittent enterprise auth, cloud workflow breakdown, low PR velocity |
| **Stalled** | Grok Build | Zero activity; strategic uncertainty given xAI's Grok 4.6 shipping in Copilot CLI instead |

**Maturity Indicators:**
- **Most mature PR discipline**: OpenCode (regression tests on bot contributions, narrow forward-compatibility policies)
- **Most mature issue triage**: Qwen Code (clear P1/P2 labels, `autofix/takeover` workflow, roadmap-flagged epics)
- **Most concerning signal**: Claude Code's **380+ combined 👍 on quality regression issues** with legal threat; core product promise under stress

---

## 6. Trend Signals

| Trend | Evidence | Implication for Developers |
|-------|----------|---------------------------|
| **Trust becomes a feature, not assumption** | Kimi CLI metering backlash (#2604), Claude Code legal threat (#68780), Codex silent rate-limit drain (#37445) | Users now instrument and verify vendor claims; tools must build auditability into core architecture |
| **Subagent correctness > subagent scale** | CodeWhale (#5596 silent cancellation, #5575 role drift), Gemini CLI (#22323 false success), Codex (#30408 MCP leaks) | Fleet orchestration is entering "hard problems" phase; expect new patterns for SSoT, receipts, bounded execution |
| **Windows as make-or-break platform** | Every tool has critical Windows issues; Pi adding PowerShell; CodeWhale, Codex, Gemini all investing | Cross-platform is no longer "nice to have"; Linux-first development carries market penalty |
| **Structured context contracts replacing implicit behavior** | CodeWhale (#4394 compaction survival), Qwen Code (workflow engine migration), Codex (annotation taxonomy) | Long-session reliability requires explicit, testable guarantees; "it usually works" is insufficient |
| **Phone/device pairing as emerging interaction paradigm** | Kimi CLI (#2616), CodeWhale (#5574), Pi (#8538) — all `gbr/1` protocol | Remote supervision/veto model suggests caution about full agent autonomy; new UX patterns forming |
| **Deterministic workflow engines displacing model-driven orchestration** | Qwen Code (#8769 review steps 3–5), CodeWhale (goal continuation cadence) | Where correctness matters, hardcoded state machines replace LLM planning; hybrid architectures emerging |

---

*Report compiled from 2026-08-24 community digests across 10 AI CLI tools. Data reflects public GitHub activity only; private/enterprise channels may diverge.*

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report
**Data as of 2026-08-24 | Repository: [anthropics/skills](https://github.com/anthropics/skills)**

---

## 1. Top Skills Ranking (Most-Discussed PRs)

| Rank | Skill | PR | Status | Discussion Focus |
|:---|:---|:---|:---|:---|
| 1 | **skill-creator eval fix** | [#1298](https://github.com/anthropics/skills/pull/1298) | 🔵 Open | Critical fix for `run_eval.py` reporting 0% recall across all skill descriptions; includes Windows stream reading, trigger detection, and parallel worker fixes. Most technically complex PR in dataset. |
| 2 | **document-typography** | [#514](https://github.com/anthropics/skills/pull/514) | 🔵 Open | Quality control for AI-generated documents—prevents orphan words, widow paragraphs, numbering misalignment. Addresses universal pain point in document output. |
| 3 | **scnet-hpc** | [#1615](https://github.com/anthropics/skills/pull/1615) | 🔵 Open | SCNet HPC cluster operations via profile-based SSH and Slurm workflows. Targets scientific computing users with partition/memory/accelerator guidance. |
| 4 | **ODT skill** | [#486](https://github.com/anthropics/skills/pull/486) | 🔵 Open | OpenDocument creation, template filling, and ODT→HTML conversion. Fills gap in open-source document standards support. |
| 5 | **frontend-design clarity** | [#210](https://github.com/anthropics/skills/pull/210) | 🔵 Open | Revision of frontend-design skill for single-conversation actionability; focuses on token-efficient, executable instructions. |
| 6 | **skill-quality-analyzer + skill-security-analyzer** | [#83](https://github.com/anthropics/skills/pull/83) | 🔵 Open | Meta-skills for evaluating skill structure/documentation (20%) and security posture. First systematic quality tooling for the ecosystem. |
| 7 | **DOCX tracked changes fix** | [#541](https://github.com/anthropics/skills/pull/541) | 🔵 Open | Fixes document corruption from `w:id` collisions between tracked changes and existing bookmarks in OOXML. |
| 8 | **Hivemind multi-agent** | [#1628](https://github.com/anthropics/skills/pull/1628) | 🔵 Open | Zero-cost multi-agent orchestration using free opencode workers with Claude Code as sole planner/reviewer. Cost-optimization thesis: "context is the scarce resource, not intelligence." |

---

## 2. Community Demand Trends (From Issues)

| Trend | Evidence | Implication |
|:---|:---|:---|
| **Trust & security boundaries** | [#492](https://github.com/anthropics/skills/issues/492) (43 comments) — namespace impersonation vulnerability | Community skills under `anthropic/` prefix create attack surface; demand for cryptographic provenance or verified publisher model |
| **Org-wide skill distribution** | [#228](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) | Enterprise users need native sharing, not manual `.skill` file exchange |
| **Evaluation tooling reliability** | [#556](https://github.com/anthropics/skills/issues/556) (12 comments) + 3 related PRs | `run_eval.py` is a single point of failure for skill development; Windows compatibility repeatedly flagged |
| **Context window efficiency** | [#1487](https://github.com/anthropics/skills/issues/1487) (`claude-api` 156k token injection), [#1329](https://github.com/anthropics/skills/issues/1329) (compact-memory proposal) | Skills are becoming victims of their own verbosity; demand for compression, symbolic notation, and lazy-loading patterns |
| **MCP interoperability** | [#16](https://github.com/anthropics/skills/issues/16) (Expose Skills as MCPs) | Community wants Skills to be callable via standard protocol, not siloed in Claude Code |
| **Bedrock/platform portability** | [#29](https://github.com/anthropics/skills/issues/29) | AWS users locked out of Skills ecosystem |

---

## 3. High-Potential Pending Skills

These active PRs have substantial technical investment and address validated pain points, but remain unmerged:

| Skill | PR | Why It May Land Soon |
|:---|:---|:---|
| **testing-patterns** | [#723](https://github.com/anthropics/skills/pull/723) | Comprehensive Testing Trophy → unit → React → E2E coverage; fills major gap in code quality skills |
| **ServiceNow platform** | [#568](https://github.com/anthropics/skills/pull/568) | Broad enterprise ITSM/ITOM/SecOps coverage; only major enterprise platform skill in queue |
| **pyxel retro game dev** | [#525](https://github.com/anthropics/skills/pull/525) | MCP-native; from established Pyxel maintainer (@kitao); creative coding is underserved |
| **self-audit quality gate** | [#1367](https://github.com/anthropics/skills/pull/1367) | Mechanical verification + four-dimension reasoning audit; addresses trust gap in AI output |
| **SAP-RPT-1-OSS predictor** | [#181](https://github.com/anthropics/skills/pull/181) | First enterprise tabular foundation model integration; SAP ecosystem entry point |

---

## 4. Skills Ecosystem Insight

> **The community's most concentrated demand is for trustworthy, measurable skill quality—encompassing evaluation tooling that actually works, context-efficient skill design, and cryptographic provenance to prevent namespace abuse—while simultaneously expanding into enterprise platforms (ServiceNow, SAP, HPC) and cost-optimized multi-agent architectures.**

The tension between **skill richness** (comprehensive capabilities) and **context poverty** (token limits, window exhaustion) defines the current development frontier.

---

# Claude Code Community Digest — 2026-08-24

## Today's Highlights

The community is intensely focused on model quality concerns, with a 93-comment thread on repetitive rhetorical tics in Claude 4.7–5.0 and Fable gaining significant traction (351 👍). Meanwhile, infrastructure stability issues persist across platforms—Windows Desktop crashes, macOS CPU freezes, and Cowork git proxy regressions are all actively discussed. No new releases shipped in the last 24 hours.

---

## Releases

*No releases in the past 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#77136](https://github.com/anthropics/claude-code/issues/77136) | **Repetitive rhetorical tics in Claude 4.7–5.0 and Fable** — Models increasingly default to verbal patterns ("Let's dive in," "Here's the thing") and ignore explicit style instructions | Most-upvoted quality issue (351 👍); strikes at core value proposition of controllable AI assistance | 93 comments; users sharing prompt engineering workarounds, comparing degradation across model versions |
| [#85199](https://github.com/anthropics/claude-code/issues/85199) | **Claude Desktop crashes on Windows, requires Repair** | Blocks daily workflow for Windows users; "Advanced Options → Repair" is not a sustainable fix | 35 comments; frustration with Desktop app reliability vs. CLI stability |
| [#18532](https://github.com/anthropics/claude-code/issues/18532) | **v2.1.9 complete freeze: 100% CPU, infinite loop (macOS ARM64)** | Data-loss risk from unresponsive sessions; 2-hour hangs with 7GB RAM consumption | 30 comments; long-running thread since January, indicates persistent threading/regression issue |
| [#76248](https://github.com/anthropics/claude-code/issues/76248) | **Cowork git proxy blocks all pushes; PAT pass-through broken** | Breaks CI/CD and remote pair programming workflows; suspected `CCR_TEST_GITPROXY` rollout | 29 comments; enterprise users affected, seeking rollback or bypass |
| [#68780](https://github.com/anthropics/claude-code/issues/68780) | **Opus 4.8/5.0 reasoning degradation and speed regression** | EU customer threatening legal action for "deceptive business practices" | 29 comments, 35 👍; unusually adversarial tone signals trust erosion among power users |
| [#85557](https://github.com/anthropics/claude-code/issues/85557) | **Portable experience memory across sessions and agents** | Addresses fundamental limitation: Claude starts "from scratch" per session | 20 comments; strong demand for persistent identity in multi-agent workflows |
| [#39195](https://github.com/anthropics/claude-code/issues/39195) | **Shared memory across multiple projects** | Current binary choice (global vs. per-project) doesn't match monorepo/multi-service realities | 9 comments, 11 👍; clear gap in memory architecture |
| [#78523](https://github.com/anthropics/claude-code/issues/78523) | **`disable-model-invocation: true` breaks explicit `/skill-name` invocation** | Skills system has unexpected coupling between auto-invocation and manual invocation | 3 comments; plugin developers hitting footgun in skill configuration |
| [#86110](https://github.com/anthropics/claude-code/issues/86110) | **VSCode: Remote Control banner × disables instead of dismisses** | Violates UI convention; accidental deactivation of critical feature | 2 comments, 1 👍; small but representative of IDE integration polish issues |
| [#89097](https://github.com/anthropics/claude-code/issues/89097) | **`/stats` Ctrl+S copy fails on WSL2 — platform string mismatch** | Fix for Linux (#54695) didn't cover WSL; "wsl" ≠ "linux" in platform detection | 1 comment; typical platform abstraction gap, easily fixable |

---

## Key PR Progress

| # | PR | Description | Status |
|---|-----|-------------|--------|
| [#83374](https://github.com/anthropics/claude-code/pull/83374) | **docs(plugin-dev): document `MessageDisplay` streaming semantics** | Fills documentation gap for hook developers; adds `MessageDisplay` to skill trigger descriptions, event guidance, and quick-reference tables | Open since Aug 2; single PR active in last 24h |

*Only 1 PR updated in the past 24 hours. The plugin development documentation PR addresses a real gap but indicates low external contribution velocity.*

---

## Feature Request Trends

1. **Persistent Memory & Cross-Session Identity** — Three overlapping requests ([#85557](https://github.com/anthropics/claude-code/issues/85557), [#39195](https://github.com/anthropics/claude-code/issues/39195), [#87834](https://github.com/anthropics/claude-code/issues/87834)) converge on the same need: memory that travels with the user, not the project or session. Users want: portable experience memory, shared project subsets, and persistent identity across Claude instances.

2. **Account-Level Configuration Sync** ([#87027](https://github.com/anthropics/claude-code/issues/87027)) — Multi-machine developers expect settings and auto-memory to follow their login, not remain local-file-bound.

3. **Workflow Mode Automation** ([#76981](https://github.com/anthropics/claude-code/issues/76981)) — Plan mode should optionally auto-reactivate after execution completes, supporting iterative plan→approve→plan cycles without manual intervention.

4. **Skill System Scalability** ([#89123](https://github.com/anthropics/claude-code/issues/89123)) — As MCP servers and user skills proliferate, context budget overflow requires relevance-based filtering, not all-or-nothing fallback.

---

## Developer Pain Points

| Theme | Evidence | Severity |
|-------|----------|----------|
| **Model Quality Regression** | [#77136](https://github.com/anthropics/claude-code/issues/77136), [#68780](github.com/anthropics/claude-code/issues/68780) | 🔴 Critical — 380+ combined 👍, legal threat, core product promise at risk |
| **Memory System Effectiveness** | [#75334](https://github.com/anthropics/claude-code/issues/75334), [#83114](https://github.com/anthropics/claude-code/issues/83114), [#85595](https://github.com/anthropics/claude-code/issues/85595) | 🟠 High — memories not read, size budgets force curation tax, inconsistent project identity resolution |
| **Platform Stability (Windows/macOS)** | [#85199](https://github.com/anthropics/claude-code/issues/85199), [#18532](https://github.com/anthropics/claude-code/issues/18532), [#88430](github.com/anthropics/claude-code/issues/88430) | 🟠 High — Desktop crashes, infinite loops, IDE focus bugs across both major platforms |
| **Cowork/Remote Dev Reliability** | [#76248](https://github.com/anthropics/claude-code/issues/76248), [#78350](https://github.com/anthropics/claude-code/issues/78350), [#89120](github.com/anthropics/claude-code/issues/89120) | 🟠 High — git proxy auth, worktree reaping races, artifact tool disappearance |
| **Permissions & Safety Bypass Edge Cases** | [#89121](https://github.com/anthropics/claude-code/issues/89121), [#89116](github.com/anthropics/claude-code/issues/89116) | 🟡 Medium — `--dangerously-skip-permissions` produces unexpected tool substitution; destructive changes without validation |
| **Internationalization/Encoding** | [#84966](https://github.com/anthropics/claude-code/issues/84966), [#89097](github.com/anthropics/claude-code/issues/89097) | 🟡 Medium — Korean path handling, WSL platform string mismatches indicate incomplete globalization |

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-08-24

## Today's Highlights

The Codex team shipped **rust-v0.149.1** with a patch release, while a massive wave of 20+ internal PRs landed focused on **content annotation integrity** across compaction, forking, truncation, and model-switching paths. Meanwhile, Windows platform stability and MCP process management remain the most active community pain points, with several high-comment issues seeing renewed activity.

---

## Releases

| Version | Notes |
|--------|-------|
| [rust-v0.149.1](https://github.com/openai/codex/releases/tag/rust-v0.149.1) | Patch release; no detailed changelog provided. Full diff available [here](https://github.com/openai/codex/compare/rust-v0.149.0...rust-v0.149.1). |
| [rust-v0.149.0-alpha.4.3](https://github.com/openai/codex/releases/tag/rust-v0.149.0-alpha.4.3) | Alpha pre-release for the 0.149.0 line. |

---

## Hot Issues

| Issue | Why It Matters | Community Reaction |
|-------|-------------|-------------------|
| [#38350](https://github.com/openai/codex/issues/38350) — Recurring scheduled tasks self-disable after successful runs | Breaks automation reliability for ChatGPT Work users; "without user authorization" implies a permission/bug in the scheduler. | **36 comments**, no upvotes — likely internal tracking heavy |
| [#30408](https://github.com/openai/codex/issues/30408) — MCP server processes leak, 9+ GB RSS | Critical resource exhaustion; per-thread MCP spawning without cleanup is an architectural scaling bug affecting long-running sessions. | **31 comments**, 8 👍 — active debugging |
| [#17525](https://github.com/openai/codex/issues/17525) — Ubuntu sandbox regression: every edit requires skip | **CLOSED** — Sandbox UX degraded to unusable on Linux; regression indicates test gap for Linux sandbox paths. | 25 comments, resolved |
| [#20730](https://github.com/openai/codex/issues/20730) — Custom pets fail in WSL due to path normalization | Windows/WSL path handling remains fragile; affects personalization features in cross-platform environments. | **24 comments**, **28 👍** — highly upvoted |
| [#34227](https://github.com/openai/codex/issues/34227) — Windows pet overlay hit region desynchronizes | Visual/interaction bug degrading mascot UX on Windows; "over time" suggests state drift or timer issue. | 16 comments, 1 👍 |
| [#38754](https://github.com/openai/codex/issues/38754) — Windows: stdio MCP servers respawned, not reaped per-task | Related to #30408 but scoped to single-task lifecycle; compounds Windows MCP reliability concerns. | 15 comments, 3 👍 |
| [#29797](https://github.com/openai/codex/issues/29797) — Windows sandbox helper errors on git log/file reads | Sandbox tooling broken on Windows for common operations; blocks basic codebase interaction. | 15 comments, 2 👍 |
| [#39903](https://github.com/openai/codex/issues/39903) — Option to disable "Ran N commands" collapsing | TUI transparency request; power users need full command visibility for debugging/verification. | **14 comments**, **29 👍** — strong community demand |
| [#37445](https://github.com/openai/codex/issues/37445) — ChatGPT desktop app silently consumes Codex weekly limit | **6% fixed deduction** per background run; quantified, reproducible rate-limit bug with financial impact. | 14 comments, **11 👍** |
| [#39170](https://github.com/openai/codex/issues/39170) — Windows auth loss with Advanced Account Security | Security feature incompatibility causing rapid session expiry; CLI unaffected suggests desktop-specific token handling. | 14 comments, **15 👍** |

---

## Key PR Progress

| PR | Feature / Fix |
|----|-------------|
| [#40315](https://github.com/openai/codex/pull/40315) — Add task mentions to TUI composer | `@` mention popup now includes Codex tasks as bounded live references; enables task-aware composition workflows |
| [#40308](https://github.com/openai/codex/pull/40308) — Add TUI tools for managing Codex tasks | `codex_tui` namespace for CRUD operations on tasks; unblocks agent self-management via TUI |
| [#31175](https://github.com/openai/codex/pull/31175) — MongoDB thread store + session migration | Experimental persistent backend with streaming migration; signals production scaling intent |
| [#40302](https://github.com/openai/codex/pull/40302) — Harden bubblewrap synthetic mount registry isolation | Security hardening: prevents sandbox escape via writable bind mounts or symlink attacks |
| [#40297](https://github.com/openai/codex/pull/40297) — Preserve developer instruction annotations in subagent forks | Content-kind metadata survives fork operations; part of annotation integrity sprint |
| [#40296](https://github.com/openai/codex/pull/40296) — Annotate Responses Lite base instructions | Proper content classification for model base instructions; improves observability/debuggability |
| [#40295](https://github.com/openai/codex/pull/40295) — Classify permission instructions under `permissions` namespace | Content taxonomy refinement: `permissions.instructions` replaces generic classification |
| [#40294](https://github.com/openai/codex/pull/40294) — Classify internal model context by source | Source-specific kinds (`<source>.internal_context`) for better tracing |
| [#40292](https://github.com/openai/codex/pull/40292) — Smoke tests for assembled Codex packages | Cross-platform pytest validation of packaged CLI/app-server; catches distribution regressions |
| [#40280](https://github.com/openai/codex/pull/40280) — Budget retained images during remote compaction | Fixes image-heavy histories exceeding context budgets; `compaction_image_budget` feature flag |

---

## Feature Request Trends

1. **TUI Transparency & Control** — [#39903](https://github.com/openai/codex/issues/39903) (command collapsing) and [#24080](https://github.com/openai/codex/issues/24080) (rate-limit status tokens) show demand for richer CLI observability and user-configurable verbosity.

2. **PreToolUse Human-in-the-Loop** — [#28437](https://github.com/openai/codex/issues/28437) requests native `permissionDecision: "ask"` escalation; hooks need approval prompt capabilities beyond binary allow/deny.

3. **Rate-Limit Visibility & Fairness** — [#37445](https://github.com/openai/codex/issues/37445) (silent consumption) and [#39808](https://github.com/openai/codex/issues/39808) (subagent overhead) reveal billing anxiety; users want granular usage attribution and optimization guidance.

4. **Cross-Platform Pet/Personalization Polish** — [#20730](https://github.com/openai/codex/issues/20730), [#34227](https://github.com/openai/codex/issues/34227) indicate Windows/WSL pet system needs path and rendering robustness.

---

## Developer Pain Points

| Theme | Evidence | Severity |
|-------|----------|----------|
| **Windows as Second-Class Citizen** | 10+ open issues: auth (#39170, #40242, #39850), sandbox (#29797, #38898), pets (#20730, #34227), MCP (#38754), session hangs (#34026), browser automation (#40048), clipboard images (#32699), deleted conversation ghosts (#39989) | 🔴 Critical — platform parity gap |
| **MCP Process Lifecycle** | [#30408](https://github.com/openai/codex/issues/30408) (leak), [#38754](https://github.com/openai/codex/issues/38754) (respawn), [#20652](https://github.com/openai/codex/issues/20652) (tool name resolution) | 🔴 Critical — resource & correctness |
| **Rate-Limit Opacity & Drain** | [#37445](https://github.com/openai/codex/issues/37445) (background drain), [#39808](https://github.com/openai/codex/issues/39808) (subagent overhead), [#24080](https://github.com/openai/codex/issues/24080) (missing reset times) | 🟡 High — trust/UX impact |
| **Session/Resume Reliability** | [#38792](https://github.com/openai/codex/issues/38792) (cursor desync), [#25394](https://github.com/openai/codex/issues/25394) (context compression drops recency), [#34026](https://github.com/openai/codex/issues/34026) (stuck "thinking") | 🟡 High — workflow continuity |
| **macOS Computer-Use Stability** | [#38939](https://github.com/openai/codex/issues/38939), [#39151](https://github.com/openai/codex/issues/39151) — runaway `SkyComputerUseService` threads → V8 OOM/SIGABRT | 🟡 High — app crash |

---

*Digest compiled from github.com/openai/codex activity on 2026-08-24.*

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-24

## 1. Today's Highlights

The nightly release pipeline continues with **v0.56.0-nightly.20260824.g5411f113c**, while community activity centers on agent reliability and core stability. Three critical P1 issues remain active around subagent misreporting, generalist agent hangs, and shell execution deadlocks—suggesting the team is prioritizing robustness over new features this cycle.

---

## 2. Releases

| Version | Date | Notes |
|---------|------|-------|
| [v0.56.0-nightly.20260824.g5411f113c](https://github.com/google-gemini/gemini-cli/releases/tag/v0.56.0-nightly.20260824.g5411f113c) | 2026-08-24 | Automated nightly bump; no functional changes in changelog |

---

## 3. Hot Issues

| # | Issue | Priority | Why It Matters | Community Signal |
|---|-------|----------|--------------|----------------|
| [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) | Subagent recovery after MAX_TURNS reported as GOAL success | **P1** | Silent failure mode: subagents claim success when actually interrupted, corrupting task state and misleading users | 13 comments, 2 👍; active maintainer triage |
| [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) | Generalist agent hangs indefinitely | **P1** | Complete blocker for any workflow requiring generalist delegation; workaround (disabling subagents) defeats purpose | 8 comments, 8 👍; high user impact |
| [#25166](https://github.com/google-gemini/gemini-cli/issues/25166) | Shell command execution stuck at "Waiting input" | **P1** | Core execution loop broken for simple commands; false-positive interactive detection | 4 comments, 3 👍 |
| [#22186](https://github.com/google-gemini/gemini-cli/issues/22186) | get-shit-done output hook causes crash | **P1** | Crash during summary generation destroys session output at completion | 3 comments |
| [#21983](https://github.com/google-gemini/gemini-cli/issues/21983) | Browser subagent fails on Wayland | **P1** | Linux display protocol incompatibility blocks headless browser workflows | 4 comments, 1 👍 |
| [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) | Zero-Dependency OS Sandboxing & Post-Execution Intent Routing | **P2** | Large effort to align with Gemini 3's native bash affinity; security/UX tradeoff | 8 comments, 1 👍; architectural EPIC |
| [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) | AST-aware file reads, search, and mapping | **P2** | Token efficiency and precision: reduce misaligned reads and navigation noise | 7 comments, 1 👍 |
| [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) | Gemini does not use skills and sub-agents enough | **P2** | Core agent routing logic underutilizes user-defined capabilities | 6 comments |
| [#26522](https://github.com/google-gemini/gemini-cli/issues/26522) | Auto Memory retrying low-signal sessions indefinitely | **P2** | Background processing waste; infinite retry loop on unprocessable content | 5 comments |
| [#26525](https://github.com/google-gemini/gemini-cli/issues/26525) | Deterministic redaction and reduce Auto Memory logging | **P2** | Security: model-side redaction happens too late; secrets exposed in context | 4 comments |

---

## 4. Key PR Progress

| # | PR | Status | What It Does | Impact |
|---|-----|--------|--------------|--------|
| [#28926](https://github.com/google-gemini/gemini-cli/pull/28926) | Windows longpaths setup docs | **Open** | Documents `core.longpaths=true` git config; ~3,000 dirty files on clean Windows checkout resolved | Windows contributor onboarding |
| [#28832](https://github.com/google-gemini/gemini-cli/pull/28832) | Skip environment-dependent tests with reason | **Open** | Converts 13 Windows test failures to skips (privilege/PowerShell gaps) | CI reliability, cross-platform dev |
| [#28834](https://github.com/google-gemini/gemini-cli/pull/28834) | Suppress ENOENT for transient subdirs | **Open** | Eliminates spurious workspace scan warnings on lockfile races | UX polish, log noise reduction |
| [#28848](https://github.com/google-gemini/gemini-cli/pull/28848) | Graceful refreshAuth in non-interactive mode | **Open** | Clean error codes instead of raw stack traces on auth failure | `--prompt` script reliability |
| [#28840](https://github.com/google-gemini/gemini-cli/pull/28840) | Populate cached/thought tokens in ACP usage | **Open** | Fixes ~3x cost overestimation for cached sessions | ACP client accuracy |
| [#28839](https://github.com/google-gemini/gemini-cli/pull/28839) | Normalize MCP tool schemas | **Open** | Injects `type:object` at root for malformed MCP schemas; fixes strict-mode Vertex rejections | MCP ecosystem compatibility |
| [#28835](https://github.com/google-gemini/gemini-cli/pull/28835) | Skip user agents dir when workspace is home | **Open** | Eliminates duplicate agent warnings on `~` runs | Home-directory UX |
| [#28740](https://github.com/google-gemini/gemini-cli/pull/28740) | Prevent supply chain RCE in eval-pr workflows | **Closed** | Splits `pull_request_target` into secure build + trusted execution | Critical security hardening |
| [#28744](https://github.com/google-gemini/gemini-cli/pull/28744) | Don't start fresh chat before resuming | **Closed** | Removes session file poisoning on ACP resume | Session persistence fix |
| [#28743](https://github.com/google-gemini/gemini-cli/pull/28743) | Preserve resolved model config systemInstruction/tools | **Closed** | Fixes chat-level overwrite of model-specific config | Correct tool/instruction routing |

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **AST-aware tooling** | [#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746) | Strong: token efficiency + precision reads; tilth/glyph evaluation underway |
| **Persistent file-based task tracking** | [#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) | Moderate: replacing in-context WriteToDo to solve "context rot" |
| **Subagent observability** | [#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) | Growing: `/chat share` for trajectories, bugreport context inclusion |
| **Agent self-awareness** | [#21432](https://github.com/google-gemini/gemini-cli/issues/21432) | Niche but deep: accurate CLI flags, hotkeys, self-execution docs |
| **Tactful/surgical extraction** | [#19561](https://github.com/google-gemini/gemini-cli/issues/19561) | Token-frugal hierarchy: grep → structure → full read |

---

## 6. Developer Pain Points

| Pain Point | Frequency | Manifestations | Workaround Availability |
|------------|-----------|----------------|------------------------|
| **Agent hangs/deadlocks** | Very High | Generalist agent (#21409), shell "Waiting input" (#25166), browser Wayland (#21983), vite interactive prompts (#22465) | Partial: disable subagents, manual prompt engineering |
| **False success reporting** | High | MAX_TURNS → GOAL success (#22323), browser failures masked (#21983) | None; corrupts task state silently |
| **Memory system fragility** | Moderate | Infinite retry loops (#26522), invalid patch handling (#26523), security redaction gaps (#26525) | Manual inbox cleanup |
| **Tool/schema brittleness** | Moderate | >128 tools 400 error (#24246), MCP schema normalization needed (#28839), symlink agents ignored (#20079) | Limit tools, manual schema fixes |
| **Windows development friction** | Moderate | Longpaths (#28926), test failures (#28832), terminal resize perf (#21924) | Git config, skip tests |
| **Subagent underutilization** | Moderate | Skills ignored (#21968), custom agents not invoked without explicit instruction | Manual prompting |

---

*Digest compiled from 50 issues and 49 PRs updated 2026-08-24.*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-24

## Today's Highlights

Copilot CLI v1.0.81-8 dropped with xhigh reasoning effort support for Grok 4.6 and smoother plugin development workflows—local directory-sourced plugins now reload live without explicit updates. Meanwhile, the issue tracker shows active triage on GPT-5.6 Sol compaction bugs and Windows file-locking conflicts with VS Code, alongside a notable authentication regression affecting enterprise users intermittently.

---

## Releases

### [v1.0.81-8](https://github.com/github/copilot-cli/releases/tag/v1.0.81-8)
| Category | Change |
|----------|--------|
| **Added** | xhigh reasoning effort support for Grok 4.6 |
| **Improved** | Local directory-sourced plugins now load live from their real paths; edits take effect on `/restart` or new session without `/plugin update` |
| **Improved** | Skills and custom agents are now discoverable |

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#2306](https://github.com/github/copilot-cli/issues/2306) | **Enterprise auth flaps 2-3× weekly** — "You are not authorized" error resolves spontaneously | Breaks production workflows for enterprise users; intermittent nature makes debugging nearly impossible | 9 comments, 3 👍 — users sharing `/context` dumps, no root cause identified |
| [#4535](https://github.com/github/copilot-cli/issues/4535) | **`store_memory` fails in 1.0.81 prereleases**: `Instance id is required` | Blocks native memory persistence in GPT-5.6 Sol agent sessions; regression from runtime API change | 5 comments, reported by Femi's agent — active debugging in progress |
| [#4414](https://github.com/github/copilot-cli/issues/4414) | **BYOK custom providers return local 403 before reaching provider** | Kills bring-your-own-key setups for OpenAI/Anthropic-compatible endpoints; false auth error masks real networking issue | Closed today — fix likely shipped, 2 👍 showed demand |
| [#4572](https://github.com/github/copilot-cli/issues/4572) | **Background compaction loses parallel GPT tool results → HTTP 400** | Data loss in long-context GPT-5.6 Sol sessions; successful tool executions vanish post-compaction | Fresh report, 1 comment — critical for autopilot reliability |
| [#4570](https://github.com/github/copilot-cli/issues/4570) | **Windows: plugin install/update blocked by VS Code file lock** | Daily friction for Windows developers; requires killing IDE to manage CLI plugins | New, reproducible — suggests missing `FILE_SHARE_DELETE` flag |
| [#4566](https://github.com/github/copilot-cli/issues/4566) | **Agent acknowledges work without executing tools** | GPT-5.3 Codex "hallucinates" completion — dangerous for automated workflows | 1 👍, confirmed reproducible — trust-breaking UX |
| [#4571](https://github.com/github/copilot-cli/issues/4571) | **Compaction triggers at 50% with GPT-5.6 Luna Max** | Premature compaction disrupts small tasks; suggests context window math error for this model tier | Zero engagement yet — likely affects all Luna Max users |
| [#4560](https://github.com/github/copilot-cli/issues/4560) | **Model "auto" hardcodes `reasoningEffort: null`, rejects configuration** | Auto-router silently disables reasoning; contradicts new Grok 4.6 xhigh support in same release | No comments — configuration gap between features |
| [#4561](https://github.com/github/copilot-cli/issues/4561) | **ACP `session/cancel` returns `end_turn` not `cancelled`** | Breaks LSP-compliant clients expecting proper cancellation signaling; protocol compliance bug | Zero engagement — ACP adoption risk |
| [#4568](https://github.com/github/copilot-cli/issues/4568) | **`--cloud` owner picker hangs, reconnect crashes, 429 polling** | Complete cloud workflow breakdown: hangs without repo context, timeouts with it, rate-limits on recovery | Fresh, multi-symptom — suggests backend coordination failure |

---

## Key PR Progress

*Only 1 PR updated in the last 24 hours:*

| # | PR | Status | Notes |
|---|-----|--------|-------|
| [#4573](https://github.com/github/copilot-cli/pull/4573) | Rename README.md to README.mdmain | **OPEN** | Appears to be spam/invalid — no description, no linked issue; likely to be closed |

*No substantive PR activity today. Engineering focus appears to be on direct commits for v1.0.81 stabilization.*

---

## Feature Request Trends

| Trend | Evidence | Momentum |
|-------|----------|----------|
| **Inline plan annotations** | [#4563](https://github.com/github/copilot-cli/issues/4563) — attach comments to plan steps without restating context | New; addresses core UX verbosity in agent workflows |
| **Insecure OTLP endpoint opt-in** | [#4567](https://github.com/github/copilot-cli/issues/4567) — align with VS Code for local telemetry collectors | Parity request; enterprise observability need |
| **Live MCP config reloading** | [#4562](https://github.com/github/copilot-cli/issues/4562) — `.github/mcp.json` changes require session restart | Friction point as MCP adoption grows |
| **Reasoning effort configurability for auto-router** | [#4560](https://github.com/github/copilot-cli/issues/4560) — implicit in the bug report | Will become explicit request as xhigh features expand |

---

## Developer Pain Points

| Pain Point | Frequency | Severity | Tracking |
|------------|-----------|----------|----------|
| **Enterprise auth intermittency** | Recurring (2-3×/week reports) | 🔴 High — blocks all usage | [#2306](https://github.com/github/copilot-cli/issues/2306) |
| **Windows file locking (VS Code conflict)** | Reproducible | 🟡 Medium — workaround exists (close IDE) | [#4570](https://github.com/github/copilot-cli/issues/4570) |
| **GPT-5.6 Sol compaction/data loss** | Multiple reports (2 open) | 🔴 High — silent data corruption | [#4572](https://github.com/github/copilot-cli/issues/4572), [#4571](https://github.com/github/copilot-cli/issues/4571) |
| **BYOK/Custom provider auth misrouting** | Now fixed | Was 🔴 High | [#4414](https://github.com/github/copilot-cli/issues/4414) |
| **Agent "acknowledge without action"** | Confirmed reproducible | 🟡 Medium — breaks trust in automation | [#4566](https://github.com/github/copilot-cli/issues/4566) |
| **Cloud workflow instability** | New, multi-faceted | 🔴 High — complete feature broken | [#4568](https://github.com/github/copilot-cli/issues/4568) |
| **ACP protocol compliance gaps** | Underreported | 🟡 Medium — ecosystem integration risk | [#4561](https://github.com/github/copilot-cli/issues/4561) |

---

*Digest compiled from github.com/github/copilot-cli activity 2026-08-23/24.*

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-24

## Today's Highlights

Community attention is focused on **metering transparency concerns**, with two active issues questioning whether token consumption calculations or unannounced policy changes are reducing effective usage quotas by 3–5×. Meanwhile, a long-running **Memory System feature request** (#1283) continues to attract discussion as users seek persistent context across sessions.

---

## Releases

*No new releases in the last 24 hours.*

---

## Hot Issues

| # | Title | Why It Matters | Community Pulse |
|---|-------|--------------|-----------------|
| [#1283](https://github.com/MoonshotAI/kimi-cli/issues/1283) | Memory System — Persistent context across sessions | The most-discussed enhancement request (27 comments); addresses core UX gap vs. competitors like Claude Code's memory features. Proposes both AI-managed and user-defined memory layers. | Active long-term discussion; no maintainer commitment visible |
| [#1994](https://github.com/MoonshotAI/kimi-cli/issues/1994) | kimiCode usage calculation problem | User reports 2-hour subscription exhausted in 2 tasks due to K2.6's extended thinking chain consuming tokens. Discrepancy between "API request" marketing and actual token-based metering. | 7 upvotes; frustration with pricing-model clarity |
| [#2604](https://github.com/MoonshotAI/kimi-cli/issues/2604) | Effective weekly allowance reduced ~3–5× without announcement | Instrumented, data-driven report from paying Vivace-tier subscriber. Wire-level JSONL ledger shows raw token volumes vs. credited usage diverging significantly. Raises trust issue. | 3 comments; awaiting official response |

---

## Key PR Progress

| # | Title | Description | Status |
|---|-------|-------------|--------|
| [#2595](https://github.com/MoonshotAI/kimi-cli/pull/2595) | fix(StrReplaceFile): refuse to edit files that are not valid UTF-8 | **Critical data-integrity fix.** Current `errors="replace"` behavior silently corrupts non-UTF-8 bytes across entire files during edits. PR proposes refusing operation instead. | Open, awaiting review |
| [#2616](https://github.com/MoonshotAI/kimi-cli/pull/2616) | Add Build Remote Agent phone pairing (gbr/1) | **Ecosystem expansion.** Enables iOS/Android remote spectating/injection via MIT-licensed `gbr-agent`. Phone acts as spectator+veto, not orchestrator—interesting security model. | Open, very recent |
| [#2614](https://github.com/MoonshotAI/kimi-cli/pull/2614) | docs(plugins): document security and persistent data | **Trust infrastructure.** Clarifies plugin sandbox boundaries, `~/.kimi/plugins/` data persistence, and command-based tool risks. Needed as plugin ecosystem grows. | Open, docs-only |

---

## Feature Request Trends

1. **Persistent Memory / Context Retention** — #1283 represents broad demand for cross-session state, spanning automatic (AI-managed) and manual (user-defined) modes. Competitor parity pressure evident.

2. **Remote/Multi-Device Workflows** — #2616's phone-pairing PR signals community interest in extending CLI beyond desktop, though "spectator + veto" model suggests caution about full remote orchestration.

3. **Plugin Ecosystem Maturity** — Documentation PR #2614 reflects growing need for security guarantees as third-party plugin adoption increases.

---

## Developer Pain Points

| Theme | Evidence | Severity |
|-------|----------|----------|
| **Opaque/Disputed Metering** | #1994, #2604: Token accounting vs. time-based subscriptions poorly explained; possible silent quota reductions | 🔴 High — affects paying users' trust |
| **Data Integrity in File Operations** | #2595: Silent UTF-8 replacement corruption risk in core `StrReplaceFile` tool | 🔴 High — potential data loss |
| **Context Window Management** | #1994: K2.6's thinking chains consume disproportionate tokens, practical task count far below marketing estimates | 🟡 Medium — model/architecture tension |
| **Communication Gaps** | #2604: No changelog or announcement for apparent policy/terms changes | 🟡 Medium — operational transparency |

---

*Digest compiled from github.com/MoonshotAI/kimi-cli public activity. Links verified 2026-08-24.*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-24

## Today's Highlights

The OpenCode team merged a critical fix for unpaired surrogate sanitization across all chat protocols (PR #44585), preventing JSON serialization failures that have been breaking multiple providers. Desktop stability improved with fixes for late draft flushes and WSL PATH resolution, while the AI protocol layer saw heavy activity with four provider-related PRs landing in 24 hours.

## Releases

*No releases in the last 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Reaction |
|---|-------|--------------|----------|
| [#14612](https://github.com/anomalyco/opencode/issues/14612) | Edit tool silently doubles tab indentation | **Data integrity bug**: The diff UI hides the real damage, causing committed files to have wrong indentation. Trust-breaking for code review workflows. | 12 comments; user provided clear repro; closed with fix |
| [#11926](https://github.com/anomalyco/opencode/issues/11926) | Missing from OpenRouter top apps despite 1M users | **Marketing/visibility concern**: Questions whether OpenRouter integration telemetry is actually working or if user attribution is broken. | 9 comments; 3 👍; sparked investigation into API routing |
| [#29869](https://github.com/anomalyco/opencode/issues/29869) | Desktop merges separate directories of same Git repo | **Workspace management flaw**: Breaks monorepo and worktree workflows; users lose project isolation unexpectedly. | 8 comments; 5 👍; **still open** — active user pain |
| [#24817](https://github.com/anomalyco/opencode/issues/24817) | Ctrl+Z suspends process instead of undoing (Linux) | **Platform parity bug**: Violates universal text-editing conventions; SIGTSTP trap makes TUI dangerous for muscle memory. | 8 comments; 5 👍; closed with terminal mode fix |
| [#15172](https://github.com/anomalyco/opencode/issues/15172) | Unanswered questions vanish after restart | **Session state bug**: Breaks conversation continuity; users cannot resume interrupted Q&A flows. | 8 comments; closed with persistence layer patch |
| [#31607](https://github.com/anomalyco/opencode/issues/31607) | TUI crash on launch (Windows, Bun segfault) | **Runtime stability**: Segfault in standalone executable undermines confidence in Bun-based distribution. | 7 comments; closed with runtime upgrade |
| [#27822](https://github.com/anomalyco/opencode/issues/27822) | Auto-relink stale sessions after directory moves | **Developer ergonomics**: Common refactor (renaming project folders) breaks all session history. | 7 comments; 3 👍; closed with project_id matching logic |
| [#28257](https://github.com/anomalyco/opencode/issues/28257) | /Undo fails when message scrolls out of view | **TUI UX gap**: Scroll buffer truncation creates "ghost" history — commands depend on visible state, not actual state. | 6 comments; closed with history indexing fix |
| [#17232](https://github.com/anomalyco/opencode/issues/17232) | Support `opencode.local.json` for project-local config | **Team/onboarding need**: 11 👍 — highest voted issue; enables repo-committed settings without polluting user config. | 5 comments; 11 👍; closed with auto-merge implementation |
| [#30587](https://github.com/anomalyco/opencode/issues/30587) | TUI scroll buffer truncates old messages | **Workflow breakage**: Prevents forking from historical context in long sessions, forcing restarts. | 4 comments; 5 👍; closed with configurable buffer limit |

---

## Key PR Progress

| # | PR | Impact |
|---|-----|--------|
| [#44585](https://github.com/anomalyco/opencode/pull/44585) | Sanitize unpaired surrogates in all chat protocols | **Critical reliability fix**: Adds `ProviderShared.sanitizeSurrogates` to strip lone surrogates that crash JSON serialization across providers. Prevents hard failures on Bedrock, Anthropic, and others. |
| [#44582](https://github.com/anomalyco/opencode/pull/44582) | Make packed workerd SDK executable | **Distribution fix**: Restores Cloudflare Workers runtime support without forcing Node build target changes. Includes regression gate for 11-package SDK cohort. |
| [#44492](https://github.com/anomalyco/opencode/pull/44492) | `disabled_plugins` config and plugin management commands | **User control**: Closes long-standing gap (#7687); allows disabling plugins without uninstalling, with CLI commands for management. |
| [#44584](https://github.com/anomalyco/opencode/pull/44584) | Ignore late draft flushes (desktop) | **Crash prevention**: Eliminates SQLite shutdown race condition causing data corruption on Electron exit. Bot-contributed with regression tests. |
| [#44583](https://github.com/anomalyco/opencode/pull/44583) | Support `disable_parallel_tool_use` for Anthropic | **Provider feature parity**: Enables Anthropic's beta tool-choice mode; critical for deterministic single-tool execution workflows. |
| [#44570](https://github.com/anomalyco/opencode/pull/44570) | Drop unknown/malformed provider stream parts | **Forward compatibility**: Makes Gemini/Anthropic streams resilient to new provider variants without schema weakening. Narrow, safe policy. |
| [#44281](https://github.com/anomalyco/opencode/pull/44281) | Fix Anthropic slug through Cloudflare AI Gateway | **Integration fix**: All Anthropic models were failing via Cloudflare gateway due to slug format mismatch (`claude-haiku-4.5` vs expected format). |
| [#43535](https://github.com/anomalyco/opencode/pull/43535) | Preserve cross-instance tool schema validation | **Plugin ecosystem health**: Fixes subtle bug where plugins with their own `effect` dependency failed validation against core's bundled copy. |
| [#44533](https://github.com/anomalyco/opencode/pull/44533) | Open VS Code native diff for edit approvals | **IDE integration**: When running in VS Code terminal, file change approvals now show native diff editor — major UX upgrade for code review workflows. |
| [#44524](https://github.com/anomalyco/opencode/pull/44524) | ACP v2 draft support | **Protocol future-proofing**: WIP implementation of Agent Client Protocol v2; positions OpenCode for emerging agent interoperability standards. |

---

## Feature Request Trends

1. **Configuration locality and versioning** — `opencode.local.json` (#17232, merged) represents broader demand for project-committed, team-shareable settings that don't conflict with user preferences. Multiple issues reference CI/CD and onboarding friction from global-only config.

2. **TUI/terminal experience parity with GUI** — Scroll buffer persistence (#30587), word-deletion keybindings (#26289), and clipboard selection (#32370) show Linux/power-user terminal workflows lagging behind desktop polish.

3. **Provider protocol robustness** — Surrogate handling (#44585), empty-content guards (#23260), and stream forward-compatibility (#44570) indicate the AI abstraction layer is maturing under production load across diverse providers.

4. **Desktop session and workspace management** — Auto-relinking (#27822), directory isolation (#29869), and draft durability (#44584) reflect scale pain as users maintain long-lived sessions across project restructures.

---

## Developer Pain Points

| Pain Point | Frequency | Evidence |
|-----------|-----------|----------|
| **Silent data corruption / hidden state changes** | High | #14612 (indentation), #15172 (vanished questions), #30597 (silent sidecar death) — fixes often don't surface problems in UI until too late |
| **Cross-platform behavioral inconsistency** | High | #24817 (Linux Ctrl+Z), #26289 (Windows TUI keybindings), #32370 (Linux clipboard), #20965 (mobile Enter) — platform-native feel is uneven |
| **Provider-specific integration fragility** | High | #23260, #31971, #33634 (Bedrock thinking), #33726 (Cloudflare timeouts), #44281 — abstraction leaks require per-provider firefighting |
| **Session durability across interruptions** | Medium-High | #27822, #30587, #28257, #31607 — crashes, truncations, and moves break long coding flows |
| **Plugin system edge cases** | Medium | #43535 (cross-instance validation), #44492 (disable management) — ecosystem growing but tooling immature |
| **Desktop architecture stability** | Medium | #30597 (sidecar death), #33671 (renderer crash), #44584 (draft flush races) — Electron + native backend complexity showing |

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-24

## Today's Highlights

The Pi team shipped a major reliability patch (v0.84.2+) addressing five production defects including race conditions in settings persistence and provider failure visibility. Meanwhile, Windows developers saw significant investment with a new optional PowerShell tool and documentation for keybinding conflicts, while llama.cpp integration improvements finally expose unloaded models for on-demand loading.

---

## Releases

*No new releases in the last 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#7683](https://github.com/earendil-works/pi/issues/7683) | **pi-tui: let components receive mouse events on their own rows** | Enables interactive TUI components like clickable buttons or expandable panels—foundation for richer terminal UX. Closed with PR #8032 after 11 comments of design iteration. | Strong engagement; author refined hit-testing semantics through review |
| [#8167](https://github.com/earendil-works/pi/issues/8167) | **Cannot pick a model with built-in llama.cpp support** | Router-mode llama-server models were invisible in `/model`; blocked local inference workflows. Resolved by PRs #8479 and #8535. | 10 comments, multiple affected users confirming fix |
| [#3627](https://github.com/earendil-works/pi/issues/3627) | **Expose timeout and retry settings on openai-* providers** | 10-minute default timeout breaks local inference; long-standing pain point with 2 upvotes and linked duplicates (#3159, #3589). | Persistent community pressure; finally addressed |
| [#7885](https://github.com/earendil-works/pi/issues/7885) | **npm search not indexing newly published pi-packages** | Package discovery pipeline broken since Aug 4; affects ecosystem growth. Gallery shows stale 49/50 packages. | Reported with clear repro; no resolution yet |
| [#5932](https://github.com/earendil-works/pi/issues/5932) | **Exposing ctx.navigateTree() to agents** | Gap between `ExtensionCommandContext` and `ExtensionContext` blocks custom `/goal` implementations. Tagged `to-discuss`, `new-harness`. | 7 comments, 2 upvotes; active use case from extension author |
| [#7740](https://github.com/earendil-works/pi/issues/7740) | **TUI after /reload ignores custom tool renderers** | Session reload breaks MCP and event-registered tools' visual output—reliability issue for extension-heavy workflows. | Open, 4 comments; affects production extensions |
| [#7724](https://github.com/earendil-works/pi/issues/7724) | **Cold restore replays overflow assistant removed by live recovery** | Context compaction retry logic leaks truncated responses into restored sessions, corrupting agent state. | Open, 4 comments; subtle data integrity bug |
| [#8452](https://github.com/earendil-works/pi/issues/8452) | **Improve default compaction prompt for continuation-state fidelity** | Proposes structured checkpointing over prose summaries for coding sessions—could reduce context loss. | Closed as discussion; no-action due to scope |
| [#8544](https://github.com/earendil-works/pi/issues/8544) | **Edit tool rewrites line endings in mixed CRLF/LF files** | Silent byte-level corruption of untouched lines; breaks Windows cross-platform workflows. | Fresh report, 1 comment; needs triage |
| [#8537](https://github.com/earendil-works/pi/issues/8537) | **Kimi 400s on replayed tool history** | Strict OpenAI-compatible providers reject Pi's tool message ordering; breaks Kimi/k2/k3 compatibility. | Fixed same day by PR #8536 |

---

## Key PR Progress

| # | PR | Description | Status |
|---|-----|-------------|--------|
| [#8543](https://github.com/earendil-works/pi/pull/8543) | **fix(settings,retry,sdk): settings concurrency, quota-error retryability, provider-failure visibility, model warnings** | Five production fixes: atomic settings writes, stale-snapshot prevention, 429/503 retry classification, explicit provider failure messages, model deprecation warnings. | **Merged** |
| [#8512](https://github.com/earendil-works/pi/pull/8512) | **feat(coding-agent): add optional PowerShell tool** | New `powershell` tool for Windows; addresses Git Bash path-handling failures. Opt-in, coexists with bash. | **Open** — under review |
| [#8535](https://github.com/earendil-works/pi/pull/8535) | **feat(coding-agent): For llama.cpp, also show unloaded models in `/model`** | Surfaces router-exposed presets; enables on-demand loading without manual `/llama` invocation. | **Merged** |
| [#8536](https://github.com/earendil-works/pi/pull/8536) | **fix(ai): normalize tool-result history for strict OpenAI-compatible providers** | Reorders orphaned tool messages, deduplicates `tool_call_id`s for Kimi/Moonshot strict validation. | **Merged** |
| [#8032](https://github.com/earendil-works/pi/pull/8032) | **feat(tui): let components receive mouse events on their own rows** | `Component.onMouse(event)` hook with relative coordinates; enables interactive TUI extensions. | **Open** — awaiting final review |
| [#8532](https://github.com/earendil-works/pi/pull/8532) | **fix(coding-agent): cap grep and find child output** | Prevents `RangeError: Invalid string length` from unbounded readline buffers on ripgrep/fd output. | **Merged** |
| [#8513](https://github.com/earendil-works/pi/pull/8513) | **fix(coding-agent): repair raw control characters in stringified edit args** | Follow-up to #3370: handles unescaped newlines/tabs in model-generated JSON strings for edit tool. | **Merged** |
| [#8509](https://github.com/earendil-works/pi/pull/8509) | **fix(ai): surface stream errors and support toolless models** | Exposes `native_finish_reason: "network_error"` as actual errors; prevents silent session termination. | **Merged** |
| [#8505](https://github.com/earendil-works/pi/pull/8505) | **fix(coding-agent): cap agent retry backoff** | Adds `retry.maxAgentDelayMs` (default 30s) to prevent unbounded exponential backoff in agent loop. | **Merged** |
| [#8538](https://github.com/earendil-works/pi/pull/8538) | **Add Build Remote Agent phone pairing (gbr/1)** | External device integration; phone as spectator/veto for desktop agent. Protocol `gbr/1`. | **Merged** |

---

## Feature Request Trends

1. **TUI Interactivity & Customization** — Mouse event handling (#7683/#8032), per-tool output expansion (#8344), syntax highlighting fixes (#8534), and keybinding documentation (#8183, #8372) show sustained demand for richer terminal UX.

2. **Extension API Surface Expansion** — Requests for `navigateTree()` in `ExtensionContext` (#5932), skill visibility controls (#8533), mid-sentence skill invocation (#8457), and bash completion events (#8530) indicate extension authors hitting context boundaries.

3. **Local/Private Inference Polish** — llama.cpp model discovery (#8167, #8539, #8535), timeout configurability (#3627), and Windows-native tooling (#8512, #8372) reflect growing on-premise deployment needs.

4. **Workflow Robustness** — Idempotent todo operations (#8529), retry/cap configuration (#8505), and compaction prompt fidelity (#8452) target long-running agent reliability.

---

## Developer Pain Points

| Theme | Frequency | Details |
|-------|-----------|---------|
| **Windows Terminal Friction** | High | Keybinding conflicts (#8183, #8372), CRLF corruption (#8544), path handling in Git Bash → driving PowerShell tool (#8512) |
| **Provider Compatibility Gaps** | High | Strict OpenAI-compatible providers (Kimi, Nous) fail on message ordering (#8537), finish reason handling (#8509), empty `custom: {}` routing (#8504) |
| **Settings/State Reliability** | Medium | Race conditions in concurrent saves, non-atomic writes, silent failures—all addressed in #8543 but indicate systemic fragility |
| **Edit Tool Brittleness** | Medium | Timeout defaults too low (#3159, #3627), raw control character handling (#8521, #8513), line ending normalization (#8544) |
| **Extension Lifecycle Edge Cases** | Medium | `/reload` breaks renderers (#7740), factory failures leave stale state (#8424), cold restore replays bad history (#7724) |
| **Package Discovery Stagnation** | Low (critical) | npm search pipeline broken since Aug 4 (#7885); blocks ecosystem growth |

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-24

## 1. Today's Highlights

The v0.22.0 nightly build landed with Web Shell workspace CWD fixes, while the team aggressively pushed review infrastructure forward with containerized execution, deferred suggestion recovery, and two new audit lenses (prose-execution and counter-frame). Meanwhile, the VS Code companion cutover from legacy WebUI to Web Shell entered its final implementation phase across multiple stacked PRs.

---

## 2. Releases

**[v0.22.0-nightly.20260824.3a1f86d805](https://github.com/QwenLM/qwen-code/releases/tag/v0.22.0-nightly.20260824.3a1f86d805)**
- `fix(web-shell)`: Pass session workspace CWD when opening from overview panel — resolves path resolution failures in multi-root workspaces
- `fix(web-shell)`: Additional Web Shell stability fix (details truncated in release notes)

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Signal |
|---|-------|--------------|----------------|
| [#7585](https://github.com/QwenLM/qwen-code/issues/7585) | **Direct External Context Provider Profile** (closed) | Enables private monorepo integrations with mutually exclusive on-demand/Auto Recall profiles — critical for enterprise adoption. 13 comments show deep design discussion. | Closed after extensive iteration; architecture likely absorbed into roadmap |
| [#8625](https://github.com/QwenLM/qwen-code/issues/8625) | **Windows terminal Chinese input: pinyin illegible** | Active P2 UI bug affecting CJK users on Windows — IME rendering broken in terminal. 8 comments, no assignee. | `welcome-pr` label; needs Windows TUI contributor |
| [#9219](https://github.com/QwenLM/qwen-code/issues/9219) | **`/review` presubmit overlap: exact-line only, misses multi-line** | Core review reliability bug — duplicate comments slip through on multi-line ranges. Filed by maintainer @wenshao. | Self-reported; fix likely in flight |
| [#9827](https://github.com/QwenLM/qwen-code/issues/9827) | **`permissions.allow` doesn't restrict tool schemas sent to model** | Security/expectation gap: allowlist filters UI but full tool set still hits API. 4 comments. | Affects cost and safety; needs core architecture change |
| [#9016](https://github.com/QwenLM/qwen-code/issues/9016) | **Vertex AI ADC broken: any API key value disables ADC** | Auth regression breaking GCP enterprise users; closed but pattern suggests recurring provider auth fragility. | Closed; verify fix in nightly |
| [#8586](https://github.com/QwenLM/qwen-code/issues/8586) | **Track `activeWork` and background Agent recovery** | Daemon reliability cornerstone — background agents that outlive foreground prompts need resurrection path. | `roadmap/background-automation`; high structural importance |
| [#8769](https://github.com/QwenLM/qwen-code/issues/8769) | **Rebuild `/review` Steps 3–5 on workflow engine** | Moves model-driven orchestration to deterministic code — reduces hallucinated review flows. | `need-discussion`; significant refactor |
| [#9821](https://github.com/QwenLM/qwen-code/issues/9821) | **Native slash commands intermittently missing (async race)** | 50% failure rate on user-defined commands — flaky registration breaks skill extensibility. | Recent regression (0.21.8–0.22.0) |
| [#9832](https://github.com/QwenLM/qwen-code/issues/9832) | **deepseek-v4-flash-vision-exp lacks image capability** | Model capability gating too rigid — hostname-based checks miss valid vision models. | User diagnosed root cause; `welcome-pr` |
| [#9743](https://github.com/QwenLM/qwen-code/issues/9743) | **VS Code Companion: drag-drop file support** | Parity request vs. Copilot Chat — explorer/tab drag into chat. Low comment count but clear UX gap. | Feature request; likely post-cutover |

---

## 4. Key PR Progress

| # | PR | Description | Status |
|---|-----|-------------|--------|
| [#9812](https://github.com/QwenLM/qwen-code/pull/9812) | **Retire `@qwen-code/webui`** | Stacked draft tracking legacy package deletion; gated on VS Code cutover completion | `autofix/takeover` — implementation pending |
| [#9811](https://github.com/QwenLM/qwen-code/pull/9811) | **Complete WebShell UI cutover (VS Code)** | Web Shell takes full VS Code surface; architecture verified, implementation follows | Draft; critical path for #9812 |
| [#9837](https://github.com/QwenLM/qwen-code/pull/9837) | **Workflow approval dialog with scoped grants** | Replaces opaque "Confirm WorkflowTool" with structured preview: phases, args, file scope | **Merged** — major UX win |
| [#9835](https://github.com/QwenLM/qwen-code/pull/9835) | **Catalog previews: byte budget, compress on truncation** | Evidence catalog capped at 240 UTF-8 bytes with compression fallback instead of Goal abort | **Merged** — reliability fix |
| [#9723](https://github.com/QwenLM/qwen-code/pull/9723) | **Containerize reviewed-repo command execution** | Sandboxes `/review` when it runs foreign code; operator policy-driven, not install-dependent | Open; security-critical |
| [#9717](https://github.com/QwenLM/qwen-code/pull/9717) | **Add prose-execution + counter-frame audits** | Two new review lenses from #9655 post-mortem: detects instruction drift and adversarial framing | `autofix/takeover` — review depth |
| [#9805](https://github.com/QwenLM/qwen-code/pull/9805) | **Promote language-pitfall, wrapper/proxy checks to dedicated agents** | Splits folded Agent 1a checks into standalone 1d/1e roles at high effort | `autofix/takeover` — review quality |
| [#9761](https://github.com/QwenLM/qwen-code/pull/9761) | **Recoverable deferred suggestions off PR page** | Convergence-deferred suggestions persist for later tooling; closes review loop gap | `autofix/needs-human` — workflow completion |
| [#9729](https://github.com/QwenLM/qwen-code/pull/9729) | **Backfill session↔PR bindings, refresh merge state** | Extends #9543 to historical sessions + live merge-state polling | Open; session management |
| [#9814](https://github.com/QwenLM/qwen-code/pull/9814) | **Add Kimi (Moonshot AI) as built-in provider** | Quick-setup under `/auth` → Third-party; intl/China endpoint choice | **Merged** — provider expansion |

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **Enterprise auth & provider expansion** | Kimi (#9197/#9814), Vertex ADC (#9016), Moonshot preset | High — multiple providers merged recently |
| **Review infrastructure hardening** | Containerized exec (#9723), workflow engine migration (#8769), deferred recovery (#9761), audit lenses (#9717, #9805) | Very high — core investment area |
| **Background automation & daemon reliability** | `activeWork` tracking (#8586), session rotation (#8927), scheduled tasks with existing sessions (#8906) | Medium — roadmap-flagged |
| **VS Code companion parity** | Drag-drop (#9743), settings schema fixes (#8752), Web Shell cutover (#9811) | High — active cutover in progress |
| **Context provider extensibility** | Direct external profiles (#7585), MCP integration scope | Medium — enterprise blocker |

---

## 6. Developer Pain Points

| Pain Point | Frequency | Manifestations |
|------------|-----------|----------------|
| **Windows platform gaps** | Recurring | Terminal IME (#8625), file link URL-encoding (#8644), path handling inconsistencies |
| **Tool/permission system expectation mismatch** | Rising | `permissions.allow` doesn't filter API schemas (#9827), native commands race (#9821), agent launch failures misreported (#9509) |
| **Model capability detection rigidity** | Persistent | deepseek-v4 vision blocked (#9832), hostname-based gating too coarse |
| **Review system edge cases** | Active | Exact-line overlap (#9219), multi-line range blindness, deferred suggestion discoverability |
| **Auth provider fragility** | Sporadic | Vertex ADC (#9016), API key vs. credential priority bugs |
| **Session/catalog lifecycle bugs** | Moderate | Web Shell catalog refresh loops (#9562), branch title inheritance (#9764), conditional-close hold leaks (#9820) |

---

*Digest compiled from github.com/QwenLM/qwen-code activity 2026-08-23/24. For real-time updates: watch releases, subscribe to #8586 (daemon), #8769 (review workflow), and #9811/#9812 (VS Code cutover).*

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-08-24

## 1. Today's Highlights

The project has officially rebranded to **CodeWhale** (Shannon Labs' public product), with v0.9.11 released and the legacy `deepseek-tui` npm package deprecated. The v0.9.12 milestone is in active integration with a critical must-fix tracker ([#5573](https://github.com/Hmbown/CodeWhale/issues/5573)) addressing safety, spend bounding, and workflow reliability. Community activity is intense with 50 issues updated in 24 hours and 18 PRs in flight, including four new PRs from M-Maciej advancing control sockets, self-relaunch, lifecycle outbox, and goal continuation cadence.

---

## 2. Releases

**v0.9.11** — Latest release (2026-08-23/24)
- Rebrands to **CodeWhale** as the public product from Shannon Labs
- All command, npm package, and release-asset names remain lowercase (`codewhale`)
- Legacy npm package `deepseek-tui` is **deprecated** — no further releases
- Users on v0.8.x legacy `deepseek`/`deepseek-tui` must migrate

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#1004](https://github.com/Hmbown/CodeWhale/issues/1004) | `/dryrun` — preview chat completion without sending | **CLOSED** — Saves real money for DeepSeek V4 Pro users with long contexts; addresses concrete, recurring API costs from cached repo files and multi-step thinking. | 10 comments; recognized as high-value cost-control feature |
| [#4959](https://github.com/Hmbown/CodeWhale/issues/4959) | `/stop` command + runtime STOP-word intercept | **CLOSED** — Critical safety issue: YOLO mode ignores "stop" text, causing runaway autonomous execution. Mechanical tool-call blocking needs emergency brake. | 9 comments; safety-focused, urgent tone |
| [#4326](https://github.com/Hmbown/CodeWhale/issues/4326) | Perf: RSS not settling after 32-worker storm cancellation | **OPEN** — High fan-out PTY benchmark shows memory retention post-cancellation; risks production memory leaks at scale. Needs allocator vs. leak distinction. | 6 comments; performance engineering deep-dive |
| [#5583](https://github.com/Hmbown/CodeWhale/issues/5583) | `responseSchema` failures need bounded repair + raw receipts | **OPEN** — Workflow robustness: schema failures currently discard malformed output instead of attempting repair, wasting tokens and breaking pipelines. | 3 comments; v0.9.12 blocker |
| [#5582](https://github.com/Hmbown/CodeWhale/issues/5582) | Workflow owner snapshots collapse Degraded into Completed | **OPEN** — Observability bug: degraded workflows misreport as completed, masking partial failures from operators. | 3 comments; state-machine correctness issue |
| [#5573](https://github.com/Hmbown/CodeWhale/issues/5573) | **v0.9.12 milestone tracker** | **OPEN** — Central coordination for release; P0 items include spend bounding (R1), SSE error frames (R3), approval-scope fix (R2), and cache control (C4). | 2 comments; actively managed by Hmbown |
| [#5596](https://github.com/Hmbown/CodeWhale/issues/5596) | Turn end silently cancels turn-owned subagents | **OPEN** — **New today** — Destroys resumable work without warning; 347k-token reviewer child produced zero findings due to this class of bug. Data loss severity. | 1 comment; critical reliability gap |
| [#5595](https://github.com/Hmbown/CodeWhale/issues/5595) | Read-only inspection children reject `git -C` at execute time | **OPEN** — **New today** — Gap between policy (allowed) and execution (rejected) wastes massive token spend; role posture gate drift suspected. | 1 comment; security/efficiency intersection |
| [#5575](https://github.com/Hmbown/CodeWhale/issues/5575) | Fleet/subagent role posture has no single source of truth | **OPEN** — Root cause of #5562's "verifier contradicts its own description"; role definitions drifted across 5+ locations. Architecture debt. | 1 comment; systemic design issue |
| [#4394](https://github.com/Hmbown/CodeWhale/issues/4394) | Compaction: publish structured survival contract | **OPEN** — Context management lacks explicit guarantees; cache-aligned summaries, retries, and pruning exist but without documented contract. Trust issue for long sessions. | 3 comments; documentation/architecture gap |

---

## 4. Key PR Progress

| # | PR | Feature / Fix | Status |
|---|-----|-------------|--------|
| [#5576](https://github.com/Hmbown/CodeWhale/pull/5576) | v0.9.12 integration: must-fix + UX fixes | 24-commit WIP branch consolidating R2 approval-scope, R3 SSE error frames, R4 cache control, and more. **Not ready to merge** — central integration point. | OPEN |
| [#5594](https://github.com/Hmbown/CodeWhale/pull/5594) | Control socket — part d (final) | Unix-only, opt-in JSON-RPC control surface per session for supervised operation. Enables external tooling integration and orchestration. | OPEN |
| [#5593](https://github.com/Hmbown/CodeWhale/pull/5593) | `/relaunch` command — part c | Self-relaunch after `/update` install; completes the OTA loop without manual restart. Handles persistence, terminal restore, telemetry flush. | OPEN |
| [#5592](https://github.com/Hmbown/CodeWhale/pull/5592) | Lifecycle outbox — part b | JSONL event stream for TUI and headless runs; replaces per-hook shell commands with structured observability. | OPEN |
| [#5591](https://github.com/Hmbown/CodeWhale/pull/5591) | Goal continuation cadence fix — part a | Fixes missing `continuation_delay_seconds` on within-turn dispatch path; prevents tight-loop goal continuation. | OPEN |
| [#5590](https://github.com/Hmbown/CodeWhale/pull/5590) | CI: run Linux workspace tests on PRs | **CLOSED** — Fixes #5547's placeholder Ubuntu job; heavy PRs now run full test matrix regardless of branch prefix. | MERGED |
| [#5584](https://github.com/Hmbown/CodeWhale/pull/5584) | Persist child approval receipts | Fixes #5543: durable `Asked` + terminal evidence for child tool approvals; closes in-memory-only security gap. | OPEN |
| [#5574](https://github.com/Hmbown/CodeWhale/pull/5574) | Build Remote Agent phone pairing (gbr/1) | Phone spectate capability for desktop agent; QR + 8-char code pairing. Optional, protocol-stable. | OPEN |
| [#5563](https://github.com/Hmbown/CodeWhale/pull/5563) | Show all providers on first run, not local-only | **CLOSED** — Fixes onboarding hiding hosted APIs behind keypress; DeepSeek was invisible to new users. Critical UX fix. | MERGED |
| [#5561](https://github.com/Hmbown/CodeWhale/pull/5561) | Auto-retry reasoning-only clean-stop | **CLOSED** — Eliminates "incomplete response" dead-end for reasoning models; auto-retries instead of forcing manual resubmit. | MERGED |

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **OAuth 2.1 for MCP** | [#1409](https://github.com/Hmbown/CodeWhale/issues/1409) — MCP servers like Tinyfish require OAuth 2.1, not just API keys | Moderate; security-critical for MCP ecosystem |
| **Debugger protocol surface** | [#3981](https://github.com/Hmbown/CodeWhale/issues/3981) — Breakpoints, stack frames, variables for agent self-debugging | Long-term; enables agentic development loops |
| **Provider-neutral architecture** | [#5103](https://github.com/Hmbown/CodeWhale/issues/5103), [#5094](https://github.com/Hmbown/CodeWhale/issues/5094), [#5093](https://github.com/Hmbown/CodeWhale/issues/5093), [#5092](https://github.com/Hmbown/CodeWhale/issues/5092) — Remove `DeepSeekClient` legacy naming; profiled Responses dialect instead of hard-coded branches | High — multi-provider is strategic |
| **Structured context contracts** | [#4394](https://github.com/Hmbown/CodeWhale/issues/4394) compaction survival, [#4069](https://github.com/Hmbown/CodeWhale/issues/4069) `.codewhaleignore` privacy controls | Growing — trust and predictability for enterprise |
| **Subagent/fleet governance** | [#5575](https://github.com/Hmbown/CodeWhale/issues/5575) role posture SSoT, [#5263](https://github.com/Hmbown/CodeWhale/issues/5263) prompt assembly in `crates/core`, [#4167](https://github.com/Hmbown/CodeWhale/issues/4167) JobManager/TaskManager merge | Active — distributed execution is core |

---

## 6. Developer Pain Points

| Pain Point | Frequency | Manifestations |
|------------|-----------|---------------|
| **Runaway spend / unbounded execution** | **Critical** — 3+ issues, P0 in v0.9.12 | `u32::MAX` default turns, inert tool-call budget, per-step stream cap re-arm ([#5566](https://github.com/Hmbown/CodeWhale/issues/5566)); silent subagent destruction wastes tokens ([#5596](https://github.com/Hmbown/CodeWhale/issues/5596)) |
| **Role/policy drift causing execution gaps** | High | Verifier contradicts description ([#5575](https://github.com/Hmbown/CodeWhale/issues/5575)); `git -C` allowed by policy, rejected at runtime ([#5595](https://github.com/Hmbown/CodeWhale/issues/5595)) |
| **Memory/performance at scale** | Moderate-High | 32-worker RSS not settling ([#4326](https://github.com/Hmbown/CodeWhale/issues/4326)); compaction lacks guarantees ([#4394](https://github.com/Hmbown/CodeWhale/issues/4394)) |
| **CI/test reliability** | Moderate | Linux workspace tests skipped by branch prefix ([#5547](https://github.com/Hmbown/CodeWhale/issues/5547)); stack overflow in test setup ([#5585](https://github.com/Hmbown/CodeWhale/issues/5585)) |
| **Rebranding/migration friction** | Ongoing | Legacy `deepseek-tui` deprecated; `DeepSeekClient` internals still need renaming ([#5103](https://github.com/Hmbown/CodeWhale/issues/5103)) |
| **Workflow observability** | Moderate | Degraded→Completed misclassification ([#5582](https://github.com/Hmbown/CodeWhale/issues/5582)); schema failures without repair paths ([#5583](https://github.com/Hmbown/CodeWhale/issues/5583)) |

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*