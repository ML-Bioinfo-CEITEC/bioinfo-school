# Week 1 — What an LLM actually is

**Dates.** Monday 25 May – Friday 29 May 2026  
**Live sessions (Europe/Prague).**

- **Monday 25.5 · 09:00–10:00** — kick-off lesson (mandatory if you can make it; recording shared otherwise)
- **Tuesday 26.5 · 11:00–14:00** — Stefan — *Domain Expertise: The Internal Validator of AI Quality*
- **Friday 29.5 · 12:00–12:30** — drop-in Q&A

**Goal.** Build a working mental model of what LLMs are and aren't, so the agent's behavior in weeks 2–4 stops being magic. Get GitHub and Slack set up; start [`lessons.md`](../lessons.md) (**From the materials** + **Surprises**). The week also introduces the idea that AI can generate candidates, but domain expertise defines the checks that decide what survives.

**Time budget.** ~7 hours.

---

## Activities

### Karpathy deep dive (~5h)

*[Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI)* (Karpathy, 3h31m, Feb 2025) is the spine of the week. Pretraining vs post-training, RLHF, tokenization, hallucination — all of it pays off later. Bioinformaticians who internalize *"it's literally next-token prediction over a learned distribution"* make qualitatively better decisions about when to trust an agent.

Budget another 30–60 min for pausing, taking notes, and rewatching the bits that don't land. **Tokenization in particular deserves attention** — it explains an enormous amount of weird LLM behavior (miscounting amino acids, mangling long identifiers, failing at reverse-complement).

### Domain Expertise — The Internal Validator of AI Quality

This session connects the technical mental model of LLMs with a scientific question: if AI systems can generate fluent explanations, working code, convincing figures, and complete analysis pipelines, what keeps scientific work connected to reality?

The lecture focuses on three ideas:

1. **Truth, falsehood, and the scientific method in the age of generative AI.**  
   LLMs, diffusion models, and other generative systems can make the boundary between true, false, synthetic, and merely plausible harder to see. In science, this is not only a communication problem. It threatens the basic function of the scientific method: separating claims that survive contact with reality from claims that only sound convincing.

2. **Domain expertise as our contact with reality.**  
   Domain expertise is not only knowing facts. It is knowing what matters in a specific problem: expected orders of magnitude, hidden assumptions, typical errors, physical and biological constraints, limit cases, and the validation checks that an answer must survive. Our critical thinking is strongest in the fields where we have the deepest experience.

3. **Practical traps when AI replaces domain judgment.**  
   We discuss several ways AI-assisted workflows can look successful while being scientifically wrong: polished hallucinations, fake references, working code that implements the wrong assumption, broken validation, data leakage, Clever Hans effects, shortcut learning, misleading metrics, and copy-paste data science.

The lecture includes a practical Clever Hans-style example: a synthetic biomedical classification pipeline where a naive AI-style workflow obtains high performance because the model learns a hidden site/batch shortcut rather than disease biology. The correction is not just to check the metric, but to inspect metadata, identify possible confounders, test independence, and redesign validation around the scientific question.

Write at least one note under **Week 1 → From the materials** answering:

> What is one example from my own domain where expertise is needed to instruct an LLM, design a model, or validate an AI-generated result?

### Reflection exercise (~1h)

Pick three things you've seen LLMs do badly (or three things you'd want them to do for you). For each, explain — using the mental model from the videos and lecture — *why* that task is hard or easy for an LLM, and what kind of domain check would be needed before trusting the result. Some real examples to get you started:

- Hallucinated PMIDs and DOIs
- Made-up gene symbols (`SLC25A4`-ish things that don't exist)
- Miscounted residues in a sequence
- Confused coordinate systems (1-based vs 0-based; we'll come back to this in week 2)
- Confident-but-wrong taxonomy assignments
- A classifier learning hospital/site/batch instead of biology
- A model with a high metric that fails in the scientifically important regime

Write it up under **Week 1 → From the materials** in [`lessons.md`](../lessons.md). Use the mental model from Karpathy and the domain-validation ideas from the invited lecture; personal chatbot examples are welcome but not required.

### Light hands-on (~1h)

One small chatbot exercise. Take a real bio task — *"explain what this stretch of GTF means,"* or *"write Python to parse this FASTA header format."* Do it once with a vanilla chatbot. Do it again with a chatbot that has code execution (ChatGPT with the code-interpreter tool, Claude with analysis tool, Gemini with code execution — pick whichever you have access to). Note the gap.

Then add one extra check: ask yourself what would make the answer scientifically valid. For example:

- What assumption did the chatbot make?
- What biological invariant should not be violated?
- What metadata or control would you inspect?
- What would be a simple negative case?
- Could the answer be correct for the wrong reason?

Log anything that surprised you under **Week 1 → Surprises** in [`lessons.md`](../lessons.md) — model name, your prompt, what it got wrong or oddly right.

That gap is what week 2 is built on.

### Setup (~30 min)

1. Create a [GitHub](https://github.com) account if you don't have one.
2. Create a [Slack](https://slack.com/get-started) account if you don't have one, then join the cohort workspace (invite link in the kick-off email).
3. Fork or clone this repo.
4. Open [`lessons.md`](../lessons.md) and fill in **Week 1** (both subsections). See the template for what goes where.
5. `git add . && git commit -m "week 1: lessons.md" && git push`.

If any Git step here is unfamiliar, that's fine — week 2 has Git fundamentals built in. For now: both accounts, Slack joined, first commit pushed.

---

## Required materials

### Video

- Andrej Karpathy, *[Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI)* (3h31m, Feb 2025).

### Reading

- Jin et al., *[GeneGPT: Augmenting Large Language Models with Domain Tools for Improved Access to Biomedical Information](https://academic.oup.com/bioinformatics/article/40/2/btae075/7606338)*, Bioinformatics 2024. Short, accessible, demonstrates LLM tool use on real biomedical APIs and primes the agentic concepts for week 2.

- Dash et al., *[A review of some techniques for inclusion of domain-knowledge into deep neural networks](https://www.nature.com/articles/s41598-021-04590-0)*, Scientific Reports 2022. Recommended after Stefan's lecture: it surveys ways to incorporate domain knowledge into deep learning through inputs, loss functions, architectures, and constraints.

- *[Explainable AI reveals Clever Hans effects in unsupervised learning models](https://www.nature.com/articles/s42256-025-01000-2)*, Nature Machine Intelligence 2025. Recommended after Stefan's lecture: it connects explainable AI with the detection of Clever Hans effects, where models appear successful while relying on unintended signals.

### Domain Expertise lecture material

- Stefan Milenković, *Domain Expertise: The Internal Validator of AI Quality*. Slides and demo notebook will be shared through the course repository or Slack after the session.

### Optional supplement

- Karpathy, *[Let's build the GPT Tokenizer](https://www.youtube.com/watch?v=zduSFxRajkE)* (2h). The single best follow-up for understanding *why* LLMs make the mistakes they make on biological sequences.

---

## What "done" looks like

- Karpathy's *Deep Dive* watched (with pauses; notes under **From the materials**)
- GeneGPT paper read (at least one note under **From the materials**)
- Stefan's lecture attended or recording watched
- At least one note written on how domain expertise changes what you would check before trusting an AI-generated result
- At least one example from your own field where domain expertise is important for LLM instruction, model building, or validation
- GitHub account created; Slack account created and cohort workspace joined
- This repo forked or cloned
- `lessons.md` — **Week 1 → From the materials** (reflection + any video/paper/lecture notes) and **Surprises** (chatbot moments) — committed and pushed
- Your one-sentence *"what I'd want to test"* notes from watching/reading/listening (under **From the materials**)

---

## Friday Q&A — what to bring

- Anything from the Karpathy video that didn't make sense
- Anything from GeneGPT you want to push back on
- Questions from Stefan's lecture, especially about validation, Clever Hans effects, data leakage, shortcut learning, and domain expertise
- One example from your own field where domain expertise is important for:
  - instructing an LLM,
  - designing useful model inputs or constraints,
  - choosing a validation strategy,
  - or deciding whether an AI-generated result is scientifically meaningful
- Your three reflection items, if you want feedback on whether the explanation holds up
- A working GitHub repo URL (so we can sanity-check setup before week 2 hits)
- You're on Slack and can post in the cohort channel if something breaks
