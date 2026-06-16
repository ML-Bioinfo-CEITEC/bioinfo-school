# Week 4 — Tools, commands, MCP, and getting ready for Brno

**Dates.** Monday 15 June – Friday 19 June 2026  
**Live session (Europe/Prague).**

- **Friday 19.6 · 12:00–12:30** — drop-in Q&A and on-ramp to the in-person week

**Goal.** Make your prep-period repo easier to reproduce, teach the agent one or two of your conventions, and understand the difference between local agent skills and MCP-style tools.

**Time budget.** ~5-7 hours.

---

## Required materials

### Video

- Tim Berglund, *[Agent Skills or MCP in the era of Claude Code?](https://www.youtube.com/watch?v=pvxNcQTcIy4)* (~10 min). Watch for the distinction between local skills, scripts, and MCP servers.

### Reading

- Mitchener et al., *[BixBench: A Comprehensive Benchmark for LLM-based Agents in Computational Biology](https://arxiv.org/abs/2503.00096)*, arXiv 2025. Read for calibration: what current agents can and cannot do on real computational-biology tasks.

### Optional supplement

- Anthropic, *[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)*. Useful if you want a broader taxonomy of workflows, agents, evaluator-optimizer loops, and orchestrator-worker patterns.
- [Model Context Protocol documentation](https://modelcontextprotocol.io/). Skim the introduction if MCP is still a new term.

---

## Activities

### Exercise A — Reproducibility refresh (~1.5h)

Pick one artifact from weeks 2 or 3: a script, notebook, mini-project, or small repo. Make it easier for somebody else to rerun.

Minimum version:

1. Add or update a short `README.md`.
2. List the required inputs and outputs.
3. List the commands or notebook cells needed to rerun it.
4. Record any dependency that was implicit before.

If you are comfortable with Python packaging, also add a `pyproject.toml` and try a fresh environment with [`uv`](https://docs.astral.sh/uv/). If that takes too long, write down what broke and move on. The failure is useful evidence.

Do not spend the week fighting Docker. We will cover containers in person.

### Exercise B — Agent rules and reusable instructions (~1h)

Create a short `AGENTS.md` at the root of your repo. It should be specific to your work, not generic AI advice.

Include 6-10 lines such as:

- which Python version or package manager to use
- where data files live
- which genome build or identifier convention matters in your field
- what validation checks the agent should run before claiming success
- what files it should not edit or commit

Then start a new agent chat and ask it to summarize the repo. Check whether it noticed and followed the instructions.

The idea from the video is simple: if you keep telling the agent the same thing, turn it into a rule, command, or skill.

### Exercise C — Three ways agents use tools (~1.5h)

Take one small bioinformatics task and think through three possible implementations.

Example task: *"Given a list of UniProt IDs, return protein length and organism."*

For your chosen task, write one short note for each mode:

1. **Code mode.** The agent writes a reusable script or notebook.
2. **Command mode.** The agent runs shell commands or API calls directly and reports the answer.
3. **MCP/tool mode.** The agent calls a typed tool such as an MCP server or function wrapper.

For each mode, answer:

- What artifact do you get?
- What can silently go wrong?
- Would you use this mode for one-off exploration, a reusable analysis, or a production workflow?

You do not need to build all three. The goal is to learn when each mode is appropriate.

### Exercise D — Optional MCP exploration (~1h)

If you have time, try one small MCP-related exploration:

- read the first page of the MCP docs and summarize what a tool is
- inspect the [BioMCP](https://github.com/genomoncology/biomcp) README
- ask an agent what MCP servers would be useful for your subfield, then verify whether any of them actually exist

Write one paragraph under **Week 4 → From the materials** in [`lessons.md`](../lessons.md): *"What should be a skill, and what should be an MCP tool?"*

### Reflection and wrap-up (~1h)

Finish the prep period with two small artifacts:

1. Update [`lessons.md`](../lessons.md), especially **Week 4 → Surprises**.
2. Write a one-paragraph `reflection.md`: *"What would I trust an agent to do in my research, and what would I not?"*

If your repo is in good shape, tag it:

```bash
git tag v0.1
git push --tags
```

## Practical notes

- **Keep this week lighter than weeks 2 and 3.** The main job is to package what you already did and bring useful questions to Brno.
- **MCP setup friction is real.** Reading and reasoning about MCP is enough this week. We can debug actual server setup in person.
- **Rules are practical.** A short `AGENTS.md` that prevents one repeated mistake is more valuable than a long generic manifesto.

---

## What "done" looks like

- One previous artifact has clearer rerun instructions
- `AGENTS.md` added with 6-10 useful repo-specific instructions
- Notes on the three tool-use modes written under **Week 4 → From the materials** or in your repo
- [`lessons.md`](../lessons.md): **Week 4 → Surprises** updated
- `reflection.md` committed: *"what I would and would not trust an agent with"*
- Optional: repo tagged `v0.1`

---

## Friday Q&A — what to bring

- Your `reflection.md` paragraph, if you want a sanity check
- Anything that broke while making your repo reproducible
- Your `AGENTS.md` rules, especially if you want feedback
- Questions about skills vs MCP
- Three things you want to build next week in Brno

---

## On-ramp to the in-person week (22–26 June 2026, Brno)

- **Mon 22.6 — day one.** Pair up, exchange repos, try to reproduce each other's week 2 or week 3 result. *(Pre-announced so you have time to make your repo reproducible.)*
- **Tue 23.6 – Thu 25.6.** Docker, MCP server authoring, multi-agent workflows, capstone build days.
- **Fri 26.6.** Lightning talks, retros, what-next.

Detailed schedule and logistics (room, accommodation, meals) emailed separately. **See you in Brno.**
