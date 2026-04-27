# AI-Enhanced Bioinformatics — MUNI Summer School 2026

A four-week online prep course followed by a one-week in-person course on agentic development for bioinformaticians and life-science students. The prep teaches enough about LLMs, agentic IDEs, bio foundation models, and the agent–tool ecosystem that students arrive in Brno ready to *build*, not to be lectured at.

Hosted by Masaryk University. Course page: <https://summeratmasaryk.cz/aienhanced-bioinformatics-educ>

---

## Schedule at a glance

| Week | Dates (Mon–Fri) | Theme | Live sessions (Europe/Prague) |
|------|-----------------|-------|--------------------------------|
| 1 | **25 May – 29 May 2026** | [What an LLM actually is](weeks/week1.md) | **Mon 25.5 · 09:00–10:00** kick-off · **Fri 29.5 · 12:00–12:30** Q&A |
| 2 | **1 Jun – 5 Jun 2026** | [Agentic IDEs on bioinformatics tasks](weeks/week2.md) | **Fri 5.6 · 12:00–12:30** Q&A |
| 3 | **8 Jun – 12 Jun 2026** | [Bio foundation models as tools](weeks/week3.md) | **Fri 12.6 · 12:00–12:30** Q&A |
| 4 | **15 Jun – 19 Jun 2026** | [How agents talk to the world: tools, commands, MCP](weeks/week4.md) | **Fri 19.6 · 12:00–12:30** Q&A |
| In person | **22 Jun – 26 Jun 2026** | Build week, Brno (Czechia) | Full days, on site |

All Q&A slots are **30 minutes**, drop-in, no prepared talk. Bring questions, broken pipelines, and confused agents.

---

## Course philosophy

Three principles run through every week:

1. **Build alongside the agent, not under it.** Every exercise produces a real artifact — a script, a notebook, a small repo — that ends up in version control. Nothing is throwaway.
2. **Biological invariants are the cheapest validator you have.** Agents will produce code that runs, prints plausible output, and is silently wrong. The single most transferable skill in the course is the habit of running domain-aware checks on agent-generated artifacts. This thread is explicit from week 2 onward.
3. **Lessons accumulate.** Across all four weeks, you keep a running [`lessons.md`](lessons.md) in your repo: one-line entries every time the agent surprises you, helps unexpectedly, or fails silently. By week 4 this file is the most valuable artifact of the prep period.

---

## The running project

Across the four weeks, you build one cohesive deliverable: an agentic pipeline that touches real bioinformatics tools and a real bio foundation model, lives in a Git repo, and is reproducible by someone else. Each week adds a layer:

- **Week 1.** First chatbot interactions, GitHub account, starter repo cloned, `lessons.md` started.
- **Week 2.** Agent-generated bioinformatics code in the repo, with versioned commits and real tests against the trap exercise.
- **Week 3.** Foundation-model evaluation added — a genomic LM benchmarked against the published baseline on Genomic Benchmarks.
- **Week 4.** Reproducibility scaffolding (`pyproject.toml`, `AGENTS.md`), MCP demo, tagged release. Ready to bring to the in-person week.

Day one of the in-person week, you'll bring this repo plus `lessons.md` plus a list of three things you want to build.

---

## Tools and platforms

- **IDE:** [Antigravity](https://antigravity.google) (free for individuals and students). [Cursor](https://cursor.com) and [Claude Code](https://www.anthropic.com/claude-code) mentioned as alternatives.
- **Notebooks:** [Google Colab](https://colab.research.google.com) for compute-heavy exercises (free tier sufficient for everything in the course).
- **Version control:** [GitHub](https://github.com). Everything ends up there.
- **Compute constraints assumed:** Colab free tier (T4 when available, CPU otherwise). All exercises sized to fit.

---

## How to engage with the videos and papers

Watching is not learning. For each video, stop every ~20 minutes and write one sentence in `lessons.md` answering: *"what's the one thing I'd want to test from what I just heard?"* For each paper, the same: one sentence answering *"what claim in this paper would I most want to verify on my own data?"*

A short ungraded Google Form quiz per week (~5 application-style questions, answers revealed afterward) gives a soft accountability signal without creating evaluation overhead. Quizzes are open-everything; questions are designed so an LLM alone can't answer them — they require integration with your own context.

---

## Assessment

Three things, that's it:

1. A working repo (this one, forked or cloned) with weekly commits.
2. `lessons.md` — your running log of what surprised, helped, or misled you.
3. A one-paragraph reflection at the end of week 4: *"What would I trust an agent to do in my research, and what would I not?"*

Anyone who can answer that reflection concretely after four weeks has gotten what the prep was meant to deliver.

---

## In-person week (22–26 June 2026, Brno)

The in-person week shapes the prep. **Day one in person: pair up, exchange repos, and try to reproduce each other's week 3 results.** That single arrangement raises the quality of every week 4 deliverable, so plan for it.

---

## Repository layout

```
.
├── README.md            # this file
├── weeks/
│   ├── week1.md         # What an LLM actually is
│   ├── week2.md         # Agentic IDEs on bioinformatics tasks
│   ├── week3.md         # Bio foundation models as tools
│   └── week4.md         # Tools, commands, MCP
├── lessons.md           # YOU create this in week 1 and grow it weekly
├── exercises/           # populated as you go (week 2 trap exercise, etc.)
└── LICENSE
```

The `exercises/` directory and starter assets (synthetic genome, GFF, etc.) will appear before each cohort starts. Until then, prompts and specifications live in the weekly markdowns.

---

## Contact

- **General contact:** Mai Hoa Magnus — `+420 775 853 057` — `magnus@czs.muni.cz`

Issues with the repo or course materials: open a GitHub issue against this repository.

---

## License

Course materials in this repository are released under the [Apache License 2.0](LICENSE). Use, fork, and adapt freely.
