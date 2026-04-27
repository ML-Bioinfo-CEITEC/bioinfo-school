# Week 2 — Agentic IDEs on bioinformatics tasks

**Dates.** Monday 1 June – Friday 5 June 2026
**Live session.** **Friday 5.6 · 12:00–12:30** — drop-in Q&A (Europe/Prague)

**Goal.** Move from talking to an LLM to working alongside an agent in a real IDE on real bio tasks. Develop the rhythm of when to let the agent run and when to intervene. Internalize the single most important lesson of the course:

> **Agent-generated bioinformatics code can run cleanly, print plausible output, and be silently wrong.**

**Time budget.** ~7 hours.

---

## Activities

### Setup and Git foundations (~1.5h)

1. Install [Antigravity](https://antigravity.google). Sign in.
2. Clone the starter repo into the IDE.
3. Then ruthlessly practical Git: `clone`, `add`, `commit`, `push`, branches, basic merge conflicts, `.gitignore`.

The framing matters: **you're about to have an agent generate hundreds of lines of code per session; without Git, you can't tell what changed, can't undo, and can't share.**

If Git already feels native to you, skim and move on. If it doesn't, this is the most important hour of the week. We are about to put a fast, confident, occasionally wrong intern at the keyboard. Git is the safety net.

### Guided exercises (~3h)

Four small tasks, escalating in agent autonomy.

1. **Tab completion and inline edits.** Open the broken Python script in the starter repo (`exercises/week2/broken_script.py`). Use Antigravity inline edits (or Cursor/Claude Code equivalents) to fix small issues one at a time. Feel the tightest loop.
2. **Chat with codebase context.** Ask the agent to explain what the script does, find where a function is used, suggest a refactor. Understand `@`-references and how context windows work.
3. **Agent mode on a real task.** Give it a genuine bioinformatics task — *"write a script that takes this VCF and outputs allele frequencies per chromosome"* or *"parse this GFF and report the longest CDS per gene."* Watch what it does. Commit the result.
4. **The trap exercise** (next section). The most important exercise of the week. Do not skip.

### The trap exercise — *"It looks right but it isn't"* (~1h within the 3h above)

The trap is a coordinate-system mismatch between **GFF3 (1-based, inclusive)** and **Python slicing (0-based, exclusive end)**. Agents fail it constantly. The failure is silently visible only to someone who knows what a CDS is supposed to look like.

**Setup files** (provided in the starter repo, under `exercises/week2/trap/`):

- `genome.fa` — tiny synthetic genome, ~160 nt, two hand-placed ORFs
- `annotations.gff3` — two CDS entries, designed so that *correct* extraction (`seq[start-1:end]`) produces ORFs starting with **ATG** and ending in stop codons
- `README.md` for the exercise

Both CDS entries are designed so that, if extracted using `seq[start:end]` (treating GFF coordinates as 0-based), the frame shifts by one and the translation produces garbage with no methionine and no clean stop.

**Prompt to give the agent (use these exact words):**

> *Write a Python script that reads `genome.fa` and `annotations.gff3`, extracts the nucleotide sequence of each CDS, translates it to protein using the standard genetic code, and prints `gene_name<TAB>nt_sequence<TAB>protein_sequence` for each CDS. Use Biopython if you want.*

Note what's *not* in the prompt: any mention of coordinate systems. Just like a real Tuesday afternoon.

**Validation step (the moment the lesson lands).** Without re-prompting the agent, answer three questions about the output:

1. Does every protein start with **M**?
2. Does every protein end with `*` (stop)?
3. Is the nucleotide length divisible by 3?

If the agent fell into the trap, the answers are no, no, and probably no. Now ask: *based on these three checks alone, is the output correct?*

This is the lesson. Not *"the agent made a mistake"* — every tool makes mistakes. The lesson is that **biological invariants are the cheapest validator you have**, and you must build the habit of running them on every agent-generated artifact.

**Reveal and fix.** Now go back to the agent:

> *The proteins don't start with M and aren't divisible by 3. I think there may be a coordinate system mismatch — GFF is 1-based inclusive, but Python slicing is 0-based with an exclusive end. Please review and fix.*

The agent will almost certainly fix it cleanly once told. Re-run, validate, confirm M-starts. Discuss with yourself in `lessons.md`: *the agent had all the information to get this right the first time. Why didn't it? What does that tell you about how to prompt next time?*

**Discussion questions for `lessons.md`:**

- What other *"looks right but isn't"* failures might hide in agent-generated bioinformatics code? (Strand handling, GRCh37/38 confusion, BED vs GFF, 0-based vs 1-based VCF positions, samtools mpileup off-by-one, BAM flag bitfield misreads, phred encoding…)
- For your own subfield, what are three biological invariants you could routinely use to validate agent output?
- If you had ten thousand CDS features and couldn't eyeball them all, how would you scale this validation?

**Optional extension.** Add `regions.bed` (0-based half-open) and ask the agent to filter CDS overlapping BED regions. Now there are two coordinate conventions in the same script. Watch it mix them up.

### Self-directed mini-project (~2h)

Pick one (or propose your own — get a thumbs-up at the Friday Q&A):

- Small CLI tool that takes a list of UniProt IDs and produces a summary table (length, organism, domain annotations) via the [UniProt REST API](https://www.uniprot.org/help/api).
- Script that reads a FASTQ file and produces basic QC stats with a one-page HTML report.
- Reproducible analysis notebook that takes a small expression matrix and produces a PCA plot with sensible defaults.

End-to-end with the agent. Hit moments where it goes off the rails. Recover. Commit.

### Reflection (~30 min)

Add to `lessons.md`: where did the agent help most, where did it mislead you, where did you have to step in. Be specific — *"agent confused 0-based and 1-based again on the BED filter"* is more useful than *"agent made mistakes."*

---

## Required materials

### Video

- Andrej Karpathy, *[Software Is Changing (Again)](https://www.youtube.com/watch?v=LCEmiRjPEtQ)* — YC AI Startup School 2025 (~40 min). The "Software 3.0" framing, partial autonomy, autonomy sliders, "decade of agents." Exactly the right conceptual frame for opening a week of agent-IDE work.

### Reading

- Yao et al., *[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)*, ICLR 2023. The foundational paper on agents that interleave reasoning and tool use. Short, easy to read, and explains the loop the IDE agent is running underneath.

### Optional supplement

- Anthropic, *[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)*. The cleanest practical taxonomy of agentic patterns — workflows vs. agents, orchestrator-worker, evaluator-optimizer.

---

## Practical notes

- **Be explicit about agent-mode quotas.** You get ~20 agent invocations per day on the free tier. Don't burn them on *"can you indent this function."* Use them when you actually want the agent to plan, read multiple files, or write something non-trivial.
- **Pre-warn yourself about the *"agent runs `rm -rf`"* failure mode.** It happens. Always work in a dedicated repo, commit often, and never run agent mode against your actual research directory until you've built up trust.
- **The trap exercise is non-negotiable.** If anything gets cut for time, don't cut that one.

---

## What "done" looks like

- Antigravity (or Cursor / Claude Code) installed and signed in
- Git basics comfortable: `clone`, `add`, `commit`, `push`, branches, `.gitignore`
- All four guided exercises complete; trap exercise included with both pre-fix and post-fix outputs committed
- One mini-project committed end-to-end
- `lessons.md` updated with at least one trap-exercise-style observation

---

## Friday Q&A — what to bring

- Live demos of agents going off the rails (we collect these — they make the best teaching examples)
- Coordinate-system horror stories from your own data
- Any mini-project that stalled — what worked, what didn't
- Questions about Antigravity quotas, Cursor pricing, or Claude Code mode mixing