# Week 1 — What an LLM actually is

**Dates.** Monday 25 May – Friday 29 May 2026
**Live sessions (Europe/Prague).**

- **Monday 25.5 · 09:00–10:00** — kick-off lesson (mandatory if you can make it; recording shared otherwise)
- **Friday 29.5 · 12:00–12:30** — drop-in Q&A

**Goal.** Build a working mental model of what LLMs are and aren't, so the agent's behavior in weeks 2–4 stops being magic. Get GitHub set up.

**Time budget.** ~7 hours.

---

## Activities

### Karpathy deep dive (~5h)

*[Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI)* (Karpathy, 3h31m, Feb 2025) is the spine of the week. Pretraining vs post-training, RLHF, tokenization, hallucination — all of it pays off later. Bioinformaticians who internalize *"it's literally next-token prediction over a learned distribution"* make qualitatively better decisions about when to trust an agent.

Budget another 30–60 min for pausing, taking notes, and rewatching the bits that don't land. **Tokenization in particular deserves attention** — it explains an enormous amount of weird LLM behavior (miscounting amino acids, mangling long identifiers, failing at reverse-complement).

### Reflection exercise (~1h)

Pick three things you've seen LLMs do badly (or three things you'd want them to do for you). For each, explain — using the mental model from the videos — *why* that task is hard or easy for an LLM. Some real examples to get you started:

- Hallucinated PMIDs and DOIs
- Made-up gene symbols (`SLC25A4`-ish things that don't exist)
- Miscounted residues in a sequence
- Confused coordinate systems (1-based vs 0-based; we'll come back to this in week 2)
- Confident-but-wrong taxonomy assignments

Write it up; commit it as the first entry in your `lessons.md`.

### Light hands-on (~1h)

One small chatbot exercise. Take a real bio task — *"explain what this stretch of GTF means,"* or *"write Python to parse this FASTA header format."* Do it once with a vanilla chatbot. Do it again with a chatbot that has code execution (ChatGPT with the code-interpreter tool, Claude with analysis tool, Gemini with code execution — pick whichever you have access to). Note the gap.

That gap is what week 2 is built on.

### Setup (~30 min)

1. Create a [GitHub](https://github.com) account if you don't have one.
2. Fork or clone the course starter repo.
3. Add a `lessons.md` at the repo root. Drop in your reflection from above as the first entry.
4. `git add . && git commit -m "week 1: lessons.md, first reflection" && git push`.

If any step here is unfamiliar, that's fine — week 2 has Git fundamentals built in. For now, getting the account and the first commit is the goal.

---

## Required materials

### Video

- Andrej Karpathy, *[Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI)* (3h31m, Feb 2025).

### Reading

- Jin et al., *[GeneGPT: Augmenting Large Language Models with Domain Tools for Improved Access to Biomedical Information](https://academic.oup.com/bioinformatics/article/40/2/btae075/7606338)*, Bioinformatics 2024. Short, accessible, demonstrates LLM tool use on real biomedical APIs and primes the agentic concepts for week 2.

### Optional supplement

- Karpathy, *[Let's build the GPT Tokenizer](https://www.youtube.com/watch?v=zduSFxRajkE)* (2h). The single best follow-up for understanding *why* LLMs make the mistakes they make on biological sequences.

---

## What "done" looks like

- Karpathy's *Deep Dive* watched (with pauses and notes)
- GeneGPT paper read
- GitHub account created, starter repo cloned
- `lessons.md` started, with your three-task reflection committed and pushed
- Your one-sentence "what I'd want to test" notes accumulated while watching

---

## Friday Q&A — what to bring

- Anything from the Karpathy video that didn't make sense
- Anything from GeneGPT you want to push back on
- Your three reflection items, if you want feedback on whether the explanation holds up
- A working GitHub repo URL (so we can sanity-check setup before week 2 hits)