# Week 4 — How agents talk to the world: tools, commands, MCP

**Dates.** Monday 15 June – Friday 19 June 2026
**Live session.** **Friday 19.6 · 12:00–12:30** — drop-in Q&A and on-ramp to in-person week (Europe/Prague)

**Goal.** Understand the three modes by which an agent uses tools (writing code, running shell commands, calling structured MCP servers). Add reproducibility scaffolding. Set up the in-person week.

**Time budget.** ~7 hours.

---

## Activities

### Light reproducibility refresh (~1h)

Go back to the Genomic Benchmarks repo from week 3. Add a proper `pyproject.toml` (we recommend [`uv`](https://docs.astral.sh/uv/)). Run on a fresh `uv venv`. Fix what breaks.

The *"fix what breaks"* **is** the lesson — agents leave hidden dependencies (an inline `pip install`, an implicit Colab-installed library), and reading the diff between *"what I wrote"* and *"what actually runs"* is the skill.

Mention Docker exists; we'll cover it in person. One paragraph of mental-model framing — *"container = frozen filesystem + a process, runs identically everywhere"* — and move on. **Do not try to install Docker Desktop on Windows this week.** That's an in-person debugging session.

### The three modes of agent–tool interaction (~1.5h)

This is the conceptual spine of the week. Walk through one bioinformatics example — *"count reads aligned to chr21 in this BAM"* — three ways:

| Mode | What happens | Output | Cost |
|---|---|---|---|
| **1. Agent writes code that calls the tool.** | What weeks 2–3 did. Agent generates Python that calls `subprocess.run(["samtools", ...])` or hits a REST API. | Versioned, reusable code. | Silent flag mistakes; throwaway for one-off questions. |
| **2. Agent executes commands directly.** | Antigravity agent mode, Claude Code, Cursor terminal. Agent literally runs `samtools view -c file.bam chr21` and reads stdout. | An answer (and a session log if you save it). | Side effects, *"rm -rf my data,"* no shareable artifact. |
| **3. Agent calls structured tools (MCP / function calling).** | Tools wrapped as named, typed interfaces invoked via protocol. BioMCP, NCBI MCP, custom servers. | Structured, auditable, composable. | Somebody builds and maintains the wrapper. |

The criterion for choosing:

> *One-off exploration → mode 2; reusable analysis → mode 1; production or oft-repeated workflow → mode 3.*

That heuristic is what you should walk away with. You will spontaneously ask *"is this a mode 1 or mode 3 task?"* for years afterward, and that distinction will guide better tool choices.

### Skills, commands, agent rules (~1h)

The mechanism by which preferences and context persist across agent sessions.

- **`AGENTS.md` / `CLAUDE.md` / `.cursorrules`.** Markdown at repo root that the agent reads on every session. Tells it your conventions: GRCh38 not GRCh37, Python 3.11 not 3.9, pytest not unittest, never commit to `main`. **Add a 10-line `AGENTS.md` to your genomic-benchmarks repo capturing one or two real conventions from `lessons.md`.** This is the highest-leverage single thing you can do to make next month's work smoother.
- **Slash commands / custom commands.** Saved prompts wrapping repeatable workflows. `/explain this CDS`, `/check coordinates`. Each agent IDE has its own flavor; the concept is universal.
- **Skills.** Newer pattern — instead of one big rules file, a directory of focused capability bundles the agent loads when relevant. Each is a `SKILL.md` plus optional helpers. For a bioinformatics group: a *"VCF handling"* skill, a *"GFF/BED coordinate conversions"* skill, an *"ESMFold-via-API"* skill. The pattern is emergent; bring ideas to the in-person week and we'll build them together.

The framing: **every *"I keep telling the agent the same thing"* is a candidate for a skill or rule.**

### BioTerm-Bench exercise (~2h)

Hands-on for Mode 2: point an agent at a benchmark of bioinformatics CLI tasks and watch it succeed or fail.

Pick 3–5 tasks from BioTerm-Bench (link in starter repo). Run with Antigravity in agent/terminal mode. For each task:

1. **Read the spec.** Predict yourself how an expert bioinformatician would solve it (which tools, which flags). Write the prediction down before invoking the agent.
2. **Let the agent attempt.** Observe what it does.
3. **Compare.** Same tools? Correct flags? Right output? Confidently wrong output?
4. **Log failure modes** in `lessons.md`.

Same epistemology as the week 2 trap exercise, applied to a published benchmark with known answers. You get to see, with numbers, where current agents are reliable and where they aren't.

### MCP and where this is going (~1h)

Forward-looking, but grounded.

Open with a quick MCP demo — connect to a [BioMCP](https://github.com/genomoncology/biomcp) server (PubMed search, ClinVar lookup, NCBI fetch — pick one with low setup friction; setup notes in `weeks/cheatsheets/mcp-setup.md`) and have the agent answer a real question through it. The contrast with Mode 2 makes the value tangible: structured args, structured responses, no parsing of raw command output.

Then three threads, brief and somewhat skeptical:

- **MCP ecosystem in bio.** What exists today (BioMCP, NCBI servers, ChEMBL wrappers); what doesn't (most niche bio tools — your samtools server doesn't exist, you'd build it). Why this matters: agent capability scales with the tool ecosystem, not just the model.
- **Agentic pipelines and *"AI scientist"* systems.** BixBench, Aviary, Sakana — early but real. Frontier agents at ~17% accuracy on real bioinformatics tasks (BixBench). They're useful, calibrated tools, not general bioinformaticians.
- **Bio FM frontier.** Where ESM3, AlphaFold3, Evo2, scFoundation are heading; what's still unsolved (dynamics, ensembles, post-translational state). The cheat sheet from week 3 will need updating in 6 months.

Close with the on-ramp: **day one of the in-person week (Mon 22.6), bring your repo + `lessons.md` + a list of three things you want to build.**

### Wrap-up and capstone (~30 min)

- **Tag a release** in your repo: `git tag v0.1 && git push --tags`.
- **Write a one-paragraph reflection:** *"What would I trust an agent to do in my research, and what would I not?"* Commit it as `reflection.md`.
- **Submit `lessons.md`** as your portfolio artifact (link your repo on the cohort form).

That reflection paragraph is the assessment. Anyone who can answer it concretely after four weeks has gotten what the prep was meant to deliver.

---

## Required materials

### Video

- A current introduction to the Model Context Protocol — Anthropic's intro on [`modelcontextprotocol.io`](https://modelcontextprotocol.io), or a recent good walk-through (~30–45 min). Pick the most current well-produced video at cohort launch — link refreshed in the kick-off email. The conceptual leap from *"agent runs shell commands"* to *"agent calls structured tools"* needs a concrete demo.

### Reading

- Mitchener et al., [*BixBench: A Comprehensive Benchmark for LLM-based Agents in Computational Biology*](https://arxiv.org/abs/2503.00096), arXiv 2025. Quantifies what agents can actually do on real bioinformatics analyses. The honest numbers are the calibration the whole course has been building toward.

---

## Practical notes

- **The "three modes" framing pays compounding dividends.** Once you have it, you'll spontaneously ask *"is this a mode 1 or mode 3 task?"* That distinction guides better tool choices for years.
- **MCP setup friction is real.** Stick to the one MCP server we've tested in Antigravity for this week; save free exploration for the in-person week.
- **The `AGENTS.md` exercise is high-leverage and underrated.** Most people learn agent-rule files only after months of frustration. Doing it explicitly here means you'll be productive with your next agent setup *immediately*.

---

## What "done" looks like

- [ ] `pyproject.toml` (uv-managed) added to your week 3 repo; project runs in a clean venv
- [ ] 10-line `AGENTS.md` committed at repo root
- [ ] BioTerm-Bench: 3–5 tasks attempted, predictions vs agent results logged
- [ ] One MCP demo tried; observation about it written into `lessons.md`
- [ ] Repo tagged `v0.1`
- [ ] `reflection.md` committed: *"what I would and wouldn't trust an agent with."*
- [ ] Repo URL submitted to the cohort form before the in-person week

---

## Friday Q&A — what to bring

- Your `reflection.md` paragraph — read it out if you want a sanity check
- Anywhere `pyproject.toml` / `uv` broke
- Whichever MCP server you tried, and whether the value-vs-friction trade-off felt worth it
- Three things you want to build next week in Brno

---

## On-ramp to the in-person week (22–26 June 2026, Brno)

- **Mon 22.6 — day one.** Pair up, exchange repos, try to reproduce each other's week 3 results. *(Pre-announced so you have time to make your repo reproducible.)*
- **Tue 23.6 – Thu 25.6.** Docker, MCP server authoring, multi-agent workflows, capstone build days.
- **Fri 26.6.** Lightning talks, retros, what-next.

Detailed schedule and logistics (room, accommodation, meals) emailed separately. **See you in Brno.**
