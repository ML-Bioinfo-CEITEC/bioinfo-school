# Week 3 — Bio foundation models as tools

**Dates.** Monday 8 June – Friday 12 June 2026  
**Live sessions (Europe/Prague).**

- **Tuesday 9.6 · 11:00–14:00** — Stefan — *Interpretability: Translating AI Outputs Back into Human Knowledge* (2h lecture + 1h Q&A)
- **Friday 12.6 · 12:00–12:30** — drop-in Q&A

**Goal.** Use bio foundation models in Colab as practical tools, then interpret their outputs carefully. Build evaluation literacy: what does it mean to trust a model output, and how do you check?

**Time budget.** ~7 hours.

---

## Required materials

### Video

- John Jumper, *[Nobel Lecture in Chemistry 2024](https://www.nobelprize.org/prizes/chemistry/2024/jumper/lecture/)* (~45 min, December 2024). Watch for how confidence, uncertainty, and experimental feedback shape the AlphaFold story.

### Reading

- Hugging Face, *[CARBON: Decoding language of life](https://github.com/huggingface/carbon/blob/main/tech-report.pdf)*, 2026. Read it as users of a bio foundation model: what tasks does it target, what evidence is offered, what limitations are visible, and what would you test before using it in your own work?

### Optional supplement

- Hayes et al., *[Simulating 500 million years of evolution with a language model](https://www.science.org/doi/10.1126/science.ads0018)*, Science 2025. Useful if you want more depth on protein language models.
- Grešová, K., Martinek, V., Čechák, D., Šimeček, P., & Alexiou, P., *[Genomic benchmarks: a collection of datasets for genomic sequence classification](https://link.springer.com/article/10.1186/s12863-023-01123-8)*, BMC Genomic Data 2023. Useful background for Exercise C and for understanding what the published CNN baseline means.

---

## Activities


### Lecture: Interpretability — Translating AI Outputs Back into Human Knowledge

This session asks what comes after prediction. Bio foundation models can produce scores, structures, embeddings, rankings, attention maps, attribution values, clusters, and generated biological objects. But these outputs are not automatically scientific knowledge.

The lecture focuses on five ideas:

1. **From prediction to explanation.**  
   A model can produce a correct prediction without producing a scientific explanation. A model explanation is also not automatically a biological explanation. We separate three objects: model output, model explanation, and scientific explanation.

2. **Bio foundation models as representation machines.**  
   Foundation models do not only produce final predictions. They transform biological objects into learned numerical representations: protein embeddings, genomic language-model representations, latent chemical vectors, geometric representations, or transcriptomic embeddings. These representations can be useful, but they are not automatically human-understandable.

3. **Embeddings, attention, attribution, and latent spaces.**  
   Embeddings allow similarity search, clustering, classification, visualization, and generation, but they operate in model space. Attention and attribution can suggest where to look, but high attention or strong attribution does not prove biological mechanism or causality.

4. **Scientific traps in interpretability.**  
   Interpretability can also mislead. Beautiful heatmaps, clean clusters, named latent dimensions, post-hoc stories, and selective examples can create a false sense of explanation. Visual clarity is not biological validity.

5. **From model output to biological hypothesis.**  
   The goal is not to make a model sound explainable. The goal is to translate model behaviour into hypotheses that can be checked using motifs, conservation, structure, perturbations, controls, experiments, or independent evidence.

A central rule for this week:

> Interpretability is useful only when it separates what the model output shows, what biological hypothesis it suggests, what evidence would be needed, and what alternative explanations could exist.

Write at least one note under **Week 3 → From the materials** answering:

> What kind of model output do I encounter most often in my own work, and what would I need to check before calling it biologically meaningful?


### Exercise A — Structure prediction (~2h)

Use ColabFold in Colab. There is no course-specific Colab notebook for this week; use the public notebooks linked from [`exercises/week3/`](../exercises/week3/).

1. Pick 1-3 short proteins or domains, ideally under 200 amino acids.
2. Run structure prediction with ColabFold, OmegaFold, or ESMFold.
3. You might want to visualize the result with `py3Dmol` or other protein structure viewer.
4. Inspect pLDDT and, if relevant, Predicted Aligned Error (PAE).

The key exercise is **interpretation**:

- Identify high vs low pLDDT regions. What does that map to biologically? (Disorder, flexible loops, signal peptides...)
- Predict the same sequence twice with different seeds and observe variation.

Then ask yourself: *based only on the model output, would I stake a claim on this structure?*

### Exercise B — Protein embeddings (~1.5h)

Use any protein embedding model that runs comfortably in Colab: ESM2 small, ESM-C, ProtT5, or another documented model.

1. Use [`exercises/week3/`](../exercises/week3/) as the starter folder.
2. Build the starter FASTA: `python3 exercises/week3/fetch_proteins.py`. This creates `exercises/week3/proteins.fasta` from labeled UniProt accessions.
3. Compute one embedding per protein.
4. Compute pairwise cosine similarity.
5. Visualize the embeddings with PCA or UMAP.
6. Check whether sequences from the same coarse family cluster together.

You do not need to train a classifier this week. The important idea is that these embeddings can become features for a classifier later: embedding first, then another simple model on top (logistic regression, random forest, or small neural network). 

### Exercise C — Optional: Genomic LM on Genomic Benchmarks (~2h)

If you have time and want a bigger challenge, take a pre-trained genomic language model, run it on one [Genomic Benchmarks](https://github.com/ML-Bioinfo-CEITEC/genomic_benchmarks) task, and compare against the published CNN baseline. Report numbers honestly. Discuss what they mean.

**Dataset.** Default: `human_nontata_promoters` (~27,000 sequences, balanced binary classification). Alternative for fast finishers: `demo_coding_vs_intergenomic_seqs`.

**Model.** Default: `[InstaDeepAI/nucleotide-transformer-v2-50m-multi-species](https://huggingface.co/InstaDeepAI/nucleotide-transformer-v2-50m-multi-species)` (cleanest HuggingFace API, fits Colab). Alternatives: DNABERT-2, HyenaDNA-small.

**Prompt to adapt in Colab:**

> *Using the `genomic-benchmarks` package, load the `human_nontata_promoters` dataset. Use `InstaDeepAI/nucleotide-transformer-v2-50m-multi-species` from HuggingFace to extract per-sequence embeddings (mean-pool over tokens). Train a logistic regression classifier on the training embeddings, evaluate on the test set. Report accuracy, F1, and a confusion matrix. Make it reproducible — set seeds.*

**Stretch (optional, ~1h evening).** Fine-tune instead of frozen embeddings. 

Write the final numbers and short interpretation in [`exercises/week3/results.md`](../exercises/week3/results.md).

### Reflection (~30 min)

Under **Week 3 → Surprises** in [`lessons.md`](../lessons.md): which model outputs were easy to interpret, which biological checks you used, and what you would not trust from a notebook run alone.

## Practical notes

- **This week is mostly Colab work.** The agent can help explain code or debug small errors, but it will not control the remote notebook as smoothly as it controls files in the IDE.
- **Compute is the constraint.** Plan for Colab free-tier limits. Keep proteins small (<200 aa), batches small, and notebook runs reproducible.
- **Don't try to teach yourself how these models work mechanistically.** That's a different course. Week 3 is about *using* them. Mechanistic understanding can come later.
- **Anticipate the *"AlphaFold told me X, therefore X is true"* failure.** Spend 15 min on disordered regions, conformational ensembles, post-translational state. Foundation models predict a single most-likely structure under their training distribution. That's not always what the cell does.

---

## What "done" looks like
- Stefan's interpretability lecture attended or recording watched
- At least one structure predicted (Exercise A); pLDDT and PAE, if available, eyeballed and noted
- Protein embeddings computed (Exercise B); cosine similarity plus PCA or UMAP plot saved
- Short notes written in `exercises/week3/results.md`
- Optional: Genomic Benchmarks pipeline (Exercise C) attempted and compared against the published CNN baseline
- [`lessons.md`](../lessons.md): **Week 3 → Surprises** updated with at least one model-output surprise and one validation hook
- At least one example written from your own field where an AI-generated interpretation would need independent evidence before being trusted

---

## Friday Q&A — what to bring

- Any structure where you genuinely couldn't tell whether to trust it
- Embedding-clustering plots that surprised you
- Optional: your Genomic Benchmarks numbers, especially if they're worse than the CNN
- Compute pain points — Colab disconnects, OOM, model-too-big-for-T4, etc.
