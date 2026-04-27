# Week 3 — Bio foundation models as tools

**Dates.** Monday 8 June – Friday 12 June 2026
**Live session.** **Friday 12.6 · 12:00–12:30** — drop-in Q&A (Europe/Prague)

**Goal.** Treat ESM, AlphaFold/Boltz, and genomic LMs as tools the agent calls — not as objects of study. Build evaluation literacy: what does it mean to trust a foundation-model output, and how do you check?

**Time budget.** ~7 hours.

---

## Activities

### Conceptual framing (~1h)

A short asynchronous lecture (linked at cohort kick-off) covering:

- **What changed when biology got foundation models.** Pre-2018: task-specific models, hand-crafted features. Post-ESM/AlphaFold: pretrained representations beat task-specific approaches on dozens of downstream problems. The paradigm has won; the job is to use it well.
- **The model zoo, briefly.** A whirlwind tour at *"what each model does, what it costs, where to access it"* level: ESM2 / ESM-C / ESM3, AlphaFold2/3, ColabFold, Boltz, RFdiffusion, Evo / Nucleotide Transformer / DNABERT-2 / HyenaDNA, scGPT / Geneformer. A one-page cheat sheet ships in the starter repo (`weeks/cheatsheets/bio-fm.md`); refreshed each cohort because this space moves monthly.
- **The agent's role.** Re-frame: these are tools an agent orchestrates. The interesting capability is composition: *sequence → embedding → structure → score → decision.*

### Exercise A — Protein embeddings with ESM (~1.5h)

In Colab, with the agent in the IDE:

1. Load ~50 protein sequences spanning a few families (kinases, GPCRs, immunoglobulins). The starter repo provides a `proteins.fasta`.
2. Have the agent embed them with ESM-C (or ESM2 small variant if compute is tight).
3. Compute pairwise cosine similarity. Visualize as heatmap or UMAP.
4. **Validation:** do sequences from the same family cluster? They should. If not, debug.

The lesson from week 2 lands again in a new context: the agent will produce an embedding pipeline that runs but uses the wrong layer (final vs penultimate vs averaged-over-residues vs CLS-token), and cluster quality will suffer. Find the choice in the code; try one alternative; compare.

### Exercise B — Structure prediction (~2h)

Two paths depending on compute:

- **Light:** [ColabFold](https://github.com/sokrypton/ColabFold) via the agent. Take a FASTA, run ColabFold, parse output PDB + pLDDT + PAE. Predict 3–5 small proteins (~150 aa). Visualize with `py3Dmol`.
- **Heavier:** [Boltz-1](https://github.com/jwohlwend/boltz) or [Chai-1](https://github.com/chaidiscovery/chai-lab) if a GPU is available. Lets you do protein–ligand or protein–DNA complexes.

Either path, the key exercise is **interpretation**:

- Identify high vs low pLDDT regions. What does that map to biologically? (Disorder, flexible loops, signal peptides…)
- Read the PAE matrix for multi-chain predictions. Where does the model think it knows the relative orientation?
- Predict the same sequence twice with different seeds and observe variation.

Then ask yourself: *based only on the agent's output, would I stake a claim on this structure?* This is an evaluation-literacy exercise dressed as a structure-prediction one.

### Exercise C — Genomic LM on Genomic Benchmarks (~2h) — flagship exercise

Take a pre-trained genomic language model, run it on one [Genomic Benchmarks](https://github.com/ML-Bioinfo-CEITEC/genomic_benchmarks) task, compare against the published CNN baseline. Build the entire pipeline with the agent. Report numbers honestly. Discuss what they mean.

**Dataset.** Default: `human_nontata_promoters` (~27,000 sequences, balanced binary classification). Alternative for fast finishers: `demo_coding_vs_intergenomic_seqs`.

**Model.** Default: `[InstaDeepAI/nucleotide-transformer-v2-50m-multi-species](https://huggingface.co/InstaDeepAI/nucleotide-transformer-v2-50m-multi-species)` (cleanest HuggingFace API, fits Colab). Alternatives: DNABERT-2, HyenaDNA-small.

**Prompt for the agent:**

> *Using the `genomic-benchmarks` package, load the `human_nontata_promoters` dataset. Use `InstaDeepAI/nucleotide-transformer-v2-50m-multi-species` from HuggingFace to extract per-sequence embeddings (mean-pool over tokens). Train a logistic regression classifier on the training embeddings, evaluate on the test set. Report accuracy, F1, and a confusion matrix. Make it reproducible — set seeds.*

**Validation hooks (failure modes to look for):**

- **Wrong embedding pooling.** Agent will pick *something* — CLS, mean of all tokens, mean of non-padding tokens, last layer vs second-to-last. Find the choice in the code; try one alternative; compare.
- **Forgetting to truncate or pad.** Sequence lengths vary; model context is fixed.
- **Train/test contamination.** Less likely with this benchmark structure but worth a sanity check.
- **Class imbalance handled invisibly.** Even on balanced datasets, enforce F1/confusion-matrix discipline.
- **Mean-pool over padding tokens.** The silent killer — embeddings get diluted in proportion to padding, and sequence length leaks into the representation. Test it: sort by length, plot mean embedding norm vs length. If there's a relationship, masking is broken.

**Validation outcome (compare to published CNN baseline from the Genomic Benchmarks paper):**

- *LM beats baseline.* Expected. Discuss parameter ratios.
- *LM ties baseline.* Surprising. What's the CNN getting? Is the task saturated?
- *LM loses to baseline.* Most pedagogically valuable — almost certainly a pipeline bug. Suspects above. Debug.

**Stretch (optional, ~1h evening).** Fine-tune instead of frozen embeddings. Or compare a second model. Or both.

**Contribution back.** The Genomic Benchmarks repo invites results that beat the current best model. If your stretch produces a clean, reproducible result, you can submit it and it appears in a public benchmark.

### Reflection (~30 min)

Update `lessons.md` with which tasks the agent handled confidently, which biological invariants you checked, what you wouldn't trust the agent to do unsupervised.

---

## Required materials

### Video

- John Jumper, *[Nobel Lecture in Chemistry 2024](https://www.nobelprize.org/prizes/chemistry/2024/jumper/lecture/)* (~45 min, December 2024). The AlphaFold story from the source. Authoritative, recent, ties to the structure exercise.

### Reading

- Abramson et al., *[Accurate structure prediction of biomolecular interactions with AlphaFold 3](https://www.nature.com/articles/s41586-024-07487-w)*, Nature 2024. Read it as users — what does it predict, how is confidence reported, what failure modes do the authors call out.

### Optional supplement

- Hayes et al., *[Simulating 500 million years of evolution with a language model](https://www.science.org/doi/10.1126/science.ads0018)*, Science 2025 (the ESM3 paper). Optional unless your cohort works specifically on proteins.

---

## Practical notes

- **Compute is the constraint.** Plan for Colab free-tier limits. Keep proteins small (<200 aa) and batches small. Don't run AlphaFold3 on a 600-residue dimer at 02:00 the night before Friday Q&A.
- **The model-zoo cheat sheet is the most-used artifact from this week.** Skim it once a week even after the course ends; refresh it for the next cohort.
- **Don't try to teach yourself how these models work mechanistically.** That's a different course. Week 3 is about *using* them. Mechanistic understanding can come later.
- **Anticipate the *"AlphaFold told me X, therefore X is true"* failure.** Spend 15 min on disordered regions, conformational ensembles, post-translational state. Foundation models predict a single most-likely structure under their training distribution. That's not always what the cell does.

---

## What "done" looks like

- Embedding pipeline (Exercise A) committed; family clustering plot in repo
- At least one structure predicted (Exercise B); pLDDT and PAE eyeballed and noted
- Genomic Benchmarks pipeline (Exercise C) committed and runnable end-to-end
- Honest comparison numbers vs published CNN baseline written up in the repo (`exercises/week3/results.md`)
- `lessons.md` updated with at least one new failure mode and one new validation hook

---

## Friday Q&A — what to bring

- Your Genomic Benchmarks numbers (especially if they're *worse* than the CNN — those are the interesting ones)
- Embedding-clustering plots that surprised you
- Any structure where you genuinely couldn't tell whether to trust it
- Compute pain points — Colab disconnects, OOM, model-too-big-for-T4, etc.