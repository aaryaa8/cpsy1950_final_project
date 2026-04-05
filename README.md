# cpsy1950_final_project
Behavioral experiment testing hyperbolic discounting in LLMs (Llama, Mistral,  Claude, GPT-5.4) using the Psych-101 dataset. Includes zorbs contamination  test and NLL alignment scoring. CPSY 1950, Brown University Spring 2026.

# Do LLMs Discount the Future Like Humans?
### Behavioral Experiments on Large Language Models — Intertemporal Choice

**Course:** CPSY 1950 — Deep Learning in Brains, Minds & Machines  
**Institution:** Brown University, Spring 2026  
**Authors:** Aaryaa Kamdar, Josef Hatcher

---

## Overview

This repository contains the full experimental pipeline for a cognitive science 
study treating large language models as behavioral subjects. We administered a 
classical intertemporal choice task to four LLMs and compared their trial-by-trial 
choices to human data from a large cross-cultural study.

The central question: do LLMs reproduce the condition-level signatures of hyperbolic 
discounting — or do they match human aggregate behavior while failing to share the 
underlying cognitive process?

---

## The Experiment

**Task:** Ruggeri et al. (2022) intertemporal choice paradigm  
**Dataset:** Psych-101 (Binz et al., 2024) — `ruggeri2022globalizability/exp1.csv`  
**Human baseline:** 11,937 participants across 61 countries  
**Sample used:** 50 participants × 598 trials per model

Participants chose between a smaller-sooner (SS) and larger-later (LL) reward 
across trials varying three conditions:

- **Sign effect** — gain trials ("receiving $500") vs loss trials ("paying $500")
- **Magnitude effect** — small ($500) vs large ($5000) amounts  
- **Delay sensitivity** — 1-year vs 2-year delays; immediate vs both-future options

---

## Models Tested

| Model | Type | NLL available |
|---|---|---|
| Llama-3.3-70B-Instruct | Open-weight | ✅ Yes |
| Mistral-Large-3 | Open-weight | ❌ No |
| claude-sonnet-4-5 | Frontier | ❌ No |
| gpt-5.4 | Frontier | ❌ No |

All models accessed via Brown University CCV LiteLLM endpoint.

---

## Key Findings

**Finding 1 — Aggregate alignment:**  
Only Llama matches human patience rates (37.5% vs 37.3%). Mistral (63.7%), 
Claude (59.4%), and GPT-5.4 (67.4%) all dramatically overshoot. Frontier scale 
does not predict human-likeness.

**Finding 2 — Sign effect:**  
No model correctly reproduces the human sign effect (gain 40.4% vs loss 26.8%). 
Llama ignores the distinction (2.5pt gap). Mistral and GPT-5.4 invert it. 
Claude shows the correct direction in the money condition but this collapses 
in the zorbs condition — indicating contamination sensitivity.

**Finding 3 — Contamination test (zorbs):**  
Replacing dollar amounts with "zorbs" — a fictional currency absent from any 
training corpus — produced negligible behavioral change in Llama (+1.5%) and 
GPT-5.4 (+5.3%), moderate change in Mistral (-5.9%), and a large shift in 
Claude (+17.2%), concentrated in the sign effect. Original money-condition 
results are robust for three of four models.

**Finding 4 — NLL alignment (Llama only):**  
Mean NLL of 0.362 — 47.8% better than the random baseline of 0.693. Llama's 
internal probability distribution tracks human choices at the trial level 
even when its binary choices diverge.

---

## Repository Structure
CPSY-1950-Project-Demo/
│
├── exp_2_intertemporal_choice_nll.ipynb   # Main experimental notebook
│
├── results_llama_50p.csv                  # Llama — money condition
├── results_mistral_50p.csv                # Mistral — money condition
├── results_claude_money_50p.csv           # Claude — money condition
├── results_gpt_money_50p.csv              # GPT-5.4 — money condition
│
├── results_llama_zorbs_50p.csv            # Llama — zorbs condition
├── results_mistral_zorbs_50p.csv          # Mistral — zorbs condition
├── results_claude_zorbs_50p.csv           # Claude — zorbs condition
├── results_gpt_zorbs_50p.csv             # GPT-5.4 — zorbs condition
│
├── poster_figures.png                     # Main results figures
├── zorbs_comparison.png                   # Money vs zorbs comparison
├── zorbs_nll_comparison.png              # NLL money vs zorbs
├── all_models_comparison.png             # Full four-model comparison
│
├── .env                                   # API key — NOT tracked by git
└── .gitignore

---

## Setup and Replication

### Prerequisites
```bash
conda create -n cpsy1950 python=3.11
conda activate cpsy1950
pip install openai python-dotenv datasets tqdm pandas matplotlib
```

### API Access

This pipeline uses Brown University's CCV LiteLLM endpoint. Access requires:
- A Brown University account
- Connection to Brown VPN
- A valid API key stored in `.env` as `OPENAI_API_KEY`

To replicate with a different endpoint, update `base_url` in the API setup cell.

### Running the Notebook

1. Clone this repository
2. Add your API key to `.env`
3. Connect to Brown VPN
4. Run `jupyter lab` from the project folder
5. Open `exp_2_intertemporal_choice_nll.ipynb`
6. Run cells in order — **skip** the API execution cells if CSVs are already present

### On Kernel Restart

Run these cells in order, skipping all API execution cells:
1. Cell 3 — API setup
2. Cell 5 — Load dataset
3. Cell 7 — All function definitions
4. Cell 12 — Load all CSVs from disk
5. Analysis and plotting cells

---

## Theoretical Framework

This project applies the following course frameworks to LLM behavioral evaluation:

**Marr (1982):** Computational-level alignment (matching aggregate output) does 
not imply algorithmic-level alignment (sharing the underlying process). Our 
condition-level decomposition tests both levels simultaneously.

**Mahowald et al. (2024):** Formal vs functional competence. Models have formal 
knowledge of hyperbolic discounting from training text. This experiment tests 
whether they functionally exhibit it.

**Serre & Pavlick (2025):** Prediction vs understanding. Llama's NLL alignment 
demonstrates predictive alignment with human choices that coexists with 
algorithmic-level failure — high prediction does not imply shared mechanism.

**Binz et al. / Centaur (2024):** Aggregate fit ≠ process fidelity. Llama's 
near-perfect aggregate match masks complete failure to reproduce condition-level 
dynamics.

**Firestone (2020):** Species-fair comparison requires removing confounds. 
Letter randomization, system message constraints, and the zorbs manipulation 
address position bias, format bias, and training data contamination respectively.

---

## References

- Ruggeri et al. (2022). Globalizability of intertemporal choice. *Nature Human Behaviour.*
- Binz et al. (2024). Centaur: a foundation model of human cognition. *arXiv.*
- Mahowald et al. (2024). Dissociating language and thought in LLMs. *Trends in Cognitive Sciences.*
- Serre & Pavlick (2025). Prediction vs understanding in foundation models.
- Marr, D. (1982). *Vision.* W. H. Freeman.
- Firestone, C. (2020). Performance vs competence in human-machine comparisons. *PNAS.*
- Elazar et al. (2021). Measuring and improving consistency in pretrained language models. *TACL.*
- Kahneman, D. & Tversky, A. (1979). Prospect theory. *Econometrica.*

