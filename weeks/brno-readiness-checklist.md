# Brno readiness checklist

Use this checklist before travelling to Brno. You do not need to have completed everything perfectly. The goal is to arrive with the shared vocabulary, basic tools, and enough hands-on experience that we can work together productively during the in-person week.

## Week 1: LLM basics and scientific validation

You should know or at least recognize:

- Karpathy's basic framing: next-token prediction, pretraining, post-training, RLHF, context windows, and hallucination.
- Tokenization and why it matters for biological sequences, identifiers, coordinates, and counting.
- GeneGPT and the idea of LLMs using domain tools.
- Domain expertise as the internal validator of AI output.
- Clever Hans effects, data leakage, shortcut learning, and validation by biological or scientific constraints.

You should have:

- A GitHub account.
- Slack access.
- This course repository forked or cloned.
- A `lessons.md` file started and pushed somewhere.

## Week 2: Agentic IDEs and bioinformatics code

You should know or at least recognize:

- Antigravity, Cursor, Claude Code, or a similar agentic IDE.
- Editor mode versus agent mode.
- Autonomy sliders: when to ask for one edit, and when to let an agent inspect files, write code, run commands, and debug.
- Git basics: `clone`, `add`, `commit`, `push`, branches, and `.gitignore`.
- ReAct: agents interleaving reasoning and acting/tool use.
- The coordinate trap: GFF3 is 1-based inclusive, while Python slicing is 0-based with an exclusive end.
- Biological invariants as cheap validators: CDS length divisible by 3, protein starts with M, stop codons, strand handling, and genome build conventions.

You should have:

- `git` working in a terminal.
- `python3` working in a terminal.
- One agentic IDE installed and signed in. Antigravity is preferred, but Cursor, Claude Code, or another serious coding agent is also OK.
- At least one small agent-generated or agent-assisted script or notebook you can show or discuss.
- Some commits in your repo so we can see your work history.

## Week 3: Bio foundation models as tools

You should know or at least recognize:

- AlphaFold, ColabFold, OmegaFold, ESMFold, or similar structure-prediction tools.
- pLDDT and PAE as confidence or uncertainty signals, not truth.
- Protein embeddings from models such as ESM, ProtT5, ESM-C, or similar.
- Cosine similarity, PCA, and UMAP as ways to inspect embeddings.
- CARBON and the idea of foundation models for biological sequences.
- Genomic Benchmarks, frozen embeddings versus fine-tuning, and honest evaluation against baselines.
- The risk of saying "the model predicted X, therefore X is true."

You should have:

- Access to Google Colab or another environment where you can run small notebooks.
- At least attempted one structure-prediction or embedding exercise.
- Notes on what you would and would not trust from a notebook or model output alone.

## Week 4: Reproducibility, skills, MCP, and agent instructions

You should know or at least recognize:

- `README.md` as rerun instructions for humans.
- `AGENTS.md` as rerun and instruction context for agents.
- The difference between code mode, command mode, and MCP/tool mode.
- Skills as local reusable instructions or workflows.
- MCP as a structured way for agents to call external tools.
- BixBench and the current limits of computational-biology agents.
- The idea that a repo should be reproducible enough for another person, or another agent, to continue from it.

You should have:

- A short `README.md` or equivalent rerun instructions for at least one artifact.
- Ideally a short `AGENTS.md` with 6-10 concrete instructions for your repo.
- Dependencies written down somewhere, even if the environment is not perfect.
- Final `lessons.md` pushed and submitted.

## For the in-person week

Think about what you might want to build during the in-person week. Ideally, choose something related to your own work.

Small examples:

- A script that summarizes your planned experiments and sends reminders.
- A helper that checks FASTA, GFF, or VCF files for common mistakes.
- A small literature or UniProt/PubMed query assistant.
- A reproducible notebook template for one analysis you often repeat.

Larger examples:

- Bioinformatics pipeline automation.
- An MCP server for a database or tool you use.
- A multi-agent workflow for checking, running, and documenting analyses.
- A reproducible mini-project around your real research data.

It is fine if your idea is vague. Bring the vague version, and we will shape it together.
