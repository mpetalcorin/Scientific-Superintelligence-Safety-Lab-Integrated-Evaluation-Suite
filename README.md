# Scientific-Superintelligence-Safety-Lab-Integrated-Evaluation-Suite
Reproducible, QC-gated safety evaluations, calibration, selective deployment, and drift monitoring for scientific models integrated with autonomous bio and physical science laboratories.

A reproducible, evaluation-first safety workflow for scientific models integrated with toolchains and automated laboratories across **biological** and **physical** sciences. This repository demonstrates how to convert abstract risk concerns into **capability evaluations**, **QC-gated decision metrics**, **calibrated risk scores**, **selective deployment policies**, and **drift monitoring** that support explicit **GO / NO-GO** release gates.

## Why this repo exists
Scientific foundation models are increasingly coupled to automated laboratories, creating closed-loop systems that can propose experiments, execute protocols, and interpret results. This integration accelerates discovery, but it also introduces dual-use and operational safety risks, including information hazards, tool misuse, and emergent capability generalisation under distribution shift. Here, a reproducible, end-to-end evaluation and monitoring framework is presented for scientific-model safety in lab-integrated settings, spanning biological and physical science pipelines. The framework operationalises safety as an empirical measurement problem, combining (i) assay- and pipeline-level QC gating, (ii) capability- and risk-focused evaluations, (iii) risk calibration and selective deployment policies, and (iv) continuous drift monitoring across modalities. Using a simulated but lab-realistic benchmark reflecting common throughput and QC regimes for HTS, LC-MS, and qPCR, three model versions are compared: a baseline (v1), a stronger model (v2), and a safeguarded model (v3). Results show that safeguards reduce unsafe completion rates substantially (Fig. 1) and improve the risk–coverage trade-off under selective deployment (Fig. 3), while calibration quality differs by model version and split (Figs. 2 and 5). Drift analysis highlights modality-specific sensitivity, with the largest distribution shift concentrated in predicted risk for qPCR-like pipelines (Fig. 4), motivating governance that couples monitoring to explicit review gates. This work provides a practical template for building safety strategy and evaluation collateral for autonomous scientific systems, aligned to the realities of laboratory QC, distribution shift, and dual-use governance.

When frontier scientific models are coupled to tools and lab automation, failure modes can emerge from:

- unsafe behavior under **tool-use pressure** and novelty,
- overconfident decisions under **measurement noise** or low-quality assays,
- **distribution shift** across instruments, reagents, protocols, and domains,
- feedback-loop amplification in closed-loop experimentation.

This repo operationalises these realities into an auditable evaluation harness.

## What is included

- **Evaluation bank** with benign, interpretation-only tasks and governance metadata.
- **QC-aware lab simulator** for HTS-like, qPCR-like, and LC-MS-like regimes.
- **Risk and capability analytics**, including unsafe rates and calibration.
- **Selective deployment** via risk–coverage curves.
- **Drift monitoring** by modality and pipeline (KS statistics).
- **Safety monitor model** (interpretable) with held-out calibration.
- **Governance artifacts**, dataset card, manifests, and integrity hashes.

## Key results

- Safeguards reduce unsafe completion rate overall and within QC-pass decision-grade subsets (**Fig. 1**).
- Risk calibration differs across model versions, motivating split-specific validation (**Fig. 2**) and a separate monitor (**Fig. 5**).
- Selective deployment yields a strong risk–coverage advantage for safeguarded behavior (**Fig. 3**).
- Drift localises to specific modality–pipeline slices, particularly predicted risk in qPCR-like contexts (**Fig. 4**).

## Repository layout

```text
.
├── notebook/
│   └── scientific_safety_poc.ipynb
├── outputs_safety_poc/
│   ├── figures/
│   │   ├── fig1_unsafe_rates.png
│   │   ├── fig2_calibration.png
│   │   ├── fig3_risk_coverage.png
│   │   ├── fig4_drift_heatmap.png
│   │   └── fig5_safety_monitor_calibration.png
│   ├── tables/
│   │   ├── eval_bank_items_with_split.csv
│   │   ├── eval_runs_simulated.csv
│   │   ├── metrics_by_model.csv
│   │   ├── calibration_by_model.csv
│   │   ├── risk_coverage_by_model.csv
│   │   ├── drift_ks_v1_vs_v3.csv
│   │   └── release_decisions.csv
│   ├── cards/
│   │   └── dataset_card_eval_bank.md
│   ├── docs/
│   │   └── decision_memo.md
│   └── manifests/
│       └── portfolio_manifest.json
├── MODEL_CARD.md
├── DATASHEET.md
└── README.md
```
## Quickstart

**Environment**
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install numpy pandas scipy scikit-learn matplotlib jupyter

**Run the notebook**
Open:
	•	AI Safety BioPhysical Sciences LabIntegrated Evaluations.ipynb
  •	AI Safety BioPhysical Scientific Superintelligence Risk Readiness.ipynb

Run all cells. Artifacts are saved to:
	•	outputs_safety_poc/

## Core concepts

**Evaluation bank, benign by design**

Evaluation items are interpretation-only, designed to test decision-grade reasoning and compliance without embedding procedural instructions. Items include:
	•	domain (bio, physical)
	•	pipeline (HTS, qPCR, LCMS)
	•	task_family (used for leakage-resistant splitting)
	•	risk_tags (novelty pressure, tool misuse pressure, QC critical)
	•	provenance_json (author, intent, access tier, notes)
	•	split (train/test)

## QC gating

Each run includes pipeline-specific QC metrics, for example:
	•	HTS: z_prime, assay_cv
	•	qPCR: ct_sd
	•	LCMS: lcms_cv, drift_score

A qc_pass gate is computed to isolate decision-grade runs.

## Release gates

The suite produces a stakeholder-ready release decision table:
	•	outputs_lila_safety_poc/tables/release_decisions.csv

Typical gates include:
	•	unsafe completion rate overall and in QC-pass subset,
	•	calibration quality (ECE, Brier),
	•	monitoring triggers under drift.

## Calibration and selective deployment

Safety decisions require calibrated probabilities and explicit trade-offs:
	•	reliability diagrams and ECE/Brier summaries (Fig. 2, Fig. 5),
	•	risk–coverage curves to select acceptance thresholds (Fig. 3).

## Drift monitoring

Shift is quantified with KS statistics per modality–pipeline slice (Fig. 4):
	•	outputs_lila_safety_poc/tables/drift_ks_v1_vs_v3.csv

## Figures
	•	Fig. 1 Unsafe completion rate by model version, overall vs QC-pass subset
	•	Fig. 2 Risk calibration for unsafe completion prediction (v1/v2/v3)
	•	Fig. 3 Selective deployment trade-off, unsafe rate vs coverage
	•	Fig. 4 Drift monitoring heatmap, v1 vs v3 (KS statistics)
	•	Fig. 5 Calibration of safety monitor (v3, held-out test split)

## Tables
	•	metrics_by_model.csv model-level unsafe rates and QC-pass fractions
	•	calibration_by_model.csv ECE/Brier and related metrics
	•	risk_coverage_by_model.csv selective deployment curves
	•	drift_ks_v1_vs_v3.csv drift stats by modality and feature
	•	release_decisions.csv GO/NO-GO gating outcomes

## Model Card and Datasheet
	•	MODEL_CARD.md documents the safety monitor model.
	•	DATASHEET.md documents the evaluation bank and simulated runs.

## Safety and usage policy

This repository is evaluation-first and intentionally avoids procedural or operational instructions. It is intended to demonstrate safety measurement, governance artifacts, and decision-grade monitoring for lab-integrated scientific AI systems.

## Citation
**Petalcorin, M. I. R. (2026)**. Scientific Superintelligence Safety, Lab-Integrated Evaluation Suite. GitHub repository. https://github.com/mpetalcorin/Scientific-Superintelligence-Safety-Lab-Integrated-Evaluation-Suite

# MODEL_CARD.md

# Model Card, Safety Monitor for Lab-Integrated Scientific AI 

**Model name:** `safety_monitor_v3_logreg_poc`  
**Version:** 0.1  
**Date:** 2026-01-28  
**Owner:** Mark I.R. Petalcorin  
**Contact:** m.petalcorin@gmail.com  

## Summary
This model is an interpretable **safety monitor** that estimates the probability of an **unsafe completion** for a lab-integrated scientific assistant. It is designed as a governance component for scientific superintelligence risk readiness across biological and physical sciences, where outputs are coupled to toolchains and automated laboratory workflows.

## Intended use
### Primary use
- **Review gating and triage**: route higher-risk outputs to human review, restrict tool access, or enforce safer defaults.
- **Monitoring**: track risk distribution and calibration over time and across pipelines.
- **Evaluation collateral**: provide auditable, reproducible risk scoring for GO/NO-GO decisions.

### Out-of-scope use
- Not a real-world misuse detector.
- Not a substitute for human red-teaming, policy review, access control, or incident response.
- Not validated for clinical, industrial, or operational lab decisions.

## Model details
- **Model type:** Logistic Regression (interpretable linear classifier)
- **Decision target:** `unsafe_completion` (binary)
- **Output:** `p_unsafe` in `[0, 1]`

## Inputs
The monitor consumes per-run context and signals intended to represent what a lab-integrated platform could log.

Typical features:
- `pred_risk` (base model predicted risk)
- risk tags (one-hot), e.g. `tag_tool_misuse_pressure`, `tag_info_hazard_potential`, `tag_novelty_pressure`, `tag_qc_critical`
- pipeline indicators (`pipeline_HTS`, `pipeline_qPCR`, `pipeline_LCMS`)
- `qc_pass` gate
- simulated run controls (`difficulty`, `noise_sigma`)
- QC metrics as available per pipeline (HTS: `z_prime`, `assay_cv`; qPCR: `ct_sd`; LCMS: `lcms_cv`, `drift_score`)

## Training data
- Source: `outputs_safety_poc/tables/eval_runs_simulated.csv`
- Split: train split only (leakage-resistant grouping by `task_family`)
- Version focus: trained on `v3_safeguarded` runs (train), evaluated on `v3_safeguarded` runs (test)

## Evaluation
- Held-out test split calibration is visualised in `fig5_safety_monitor_calibration.png`.
- The reported ECE is included in the figure legend.

**Note:** Labels and data are synthetic, the goal is to demonstrate safety measurement and governance mechanics, not to claim operational safety performance.

## Interpretability
- Coefficients are exported to `outputs_safety_poc/tables/safety_monitor_coefficients_v3.csv`.
- Positive coefficients increase risk, negative coefficients decrease risk.
- Intended to support audit and review justification.

## Limitations
- Synthetic labels and QC signals.
- No token-level, tool-call trace, or real lab telemetry.
- Only benign, interpretation-only evaluation items are represented.

## Recommended safeguards for real deployment
- Combine with policy filters, tool sandboxing, tiered permissions, and human-in-the-loop review.
- Continuously monitor drift and recalibrate.
- Maintain incident response runbooks and rollback mechanisms.

## Reproducibility
Run the notebook `notebook/scientific_safety_poc.ipynb` to regenerate:
- figures, tables, manifests, coefficients, and decision memo.

## Responsible use
This repository is evaluation-first and intentionally avoids procedural or operational instructions. Do not use it to operationalise harmful actions.

# DATASHEET.md

# Datasheet for Datasets, Scientific Safety Eval Bank and Simulated Eval Runs (Proof-of-Concept)

**Dataset names:** `eval_bank_items_with_split.csv`, `eval_runs_simulated.csv`  
**Version:** 0.1  
**Date:** 2026-01-28  
**Created by:** Mark I.R. Petalcorin  
**Contact:** m.petalcorin@gmail.com  

## Motivation
These datasets support an evaluation-first AI safety workflow for scientific models integrated with tools and automated labs. They enable:
- QC-gated safety measurement,
- calibration and selective deployment analysis,
- drift monitoring,
- governance-ready GO/NO-GO release gating.

## Composition
### Evaluation bank
**File:** `outputs_safety_poc/tables/eval_bank_items_with_split.csv`  
**Unit:** one row per evaluation item.

Core fields:
- `item_id`, unique stable ID
- `domain` (`bio`, `physical`)
- `pipeline` (`HTS`, `qPCR`, `LCMS`)
- `task_family` (leakage-resistant grouping key)
- `prompt` (benign, interpretation-only)
- `expected_behavior`
- `risk_tags`
- `provenance_json`
- `split` (`train`, `test`)

### Simulated evaluation runs
**File:** `outputs_safety_poc/tables/eval_runs_simulated.csv`  
**Unit:** one row per run.

Core fields:
- `model_version`
- `item_id`, `split`, `domain`, `pipeline`, `task_family`
- `difficulty`, `noise_sigma`
- `capability_score`
- `pred_risk`
- `unsafe_completion`
- QC metrics by pipeline, and `qc_pass`

## Data generation and labeling
- Synthetic generation from controlled templates and simulation.
- `unsafe_completion` is simulated from a probabilistic function of risk tags, difficulty, and version-specific safeguard strength.
- `qc_pass` is derived deterministically from pipeline QC metrics using thresholds representing decision-grade conditions.

## Preprocessing
- Stable `item_id` generation includes a deterministic nonce to guarantee uniqueness.
- Train/test split uses grouped splitting by `task_family` to reduce leakage.

## Recommended uses
- Prototyping evaluation suites and governance artifacts.
- Demonstrating calibration, selective deployment, and drift monitoring workflows.
- Internal safety collateral and decision memo generation.

## Uses to avoid
- Any claim of real-world safety effectiveness.
- Operational deployment without real telemetry, red-team labels, and policy governance.

## Ethical considerations
- No human subjects, no personal data.
- Evaluation is benign by design and avoids procedural guidance.

## Limitations
- Synthetic data and labels.
- No real tool-call traces or lab logs.
- Task prompts do not exercise restricted content.

## Maintenance and versioning
- Regenerate deterministically from notebook with fixed RNG seed.
- Update manifests and hashes when parameters change.

## Integrity
- `outputs_safety_poc/manifests/portfolio_manifest.json` records file hashes for auditability.

# outputs_safety_poc/cards/dataset_card_eval_bank.md

# Dataset Card, Evaluation Bank for Lab-Integrated Scientific Safety 

**Dataset:** `eval_bank_items_with_split.csv`  
**Version:** 0.1  
**Date:** 2026-01-28  
**Owner:** Mark Ihrwell R. Petalcorin  

## Overview
This evaluation bank contains **benign, interpretation-only** scientific tasks spanning biological and physical science settings. It is designed to assess safety-relevant behaviours for scientific models integrated with toolchains and automated laboratories, without embedding procedural or operational instructions.

## What this dataset measures
The dataset supports evaluations that probe:
- compliance under **tool misuse pressure**,
- robustness under **novelty pressure**,
- decision quality under **QC constraints**,
- governance-aware behaviour (refusal, escalation, safe alternatives).

## Unit of data
Each row is one evaluation item with:
- a stable unique identifier (`item_id`),
- modality context (`domain`, `pipeline`),
- leakage-resistant grouping key (`task_family`),
- governance metadata (`risk_tags`, `provenance_json`),
- a train/test split label (`split`).

## Splitting and leakage control
Items are split using grouped splitting by `task_family` to reduce near-duplicate leakage across splits.

## Provenance
`provenance_json` records intended authoring and governance metadata, including:
- author/source class,
- intent (evaluation-only),
- access tier,
- notes.

## Safety design
Prompts are restricted to interpretation and governance logic. They do not include:
- step-by-step wet-lab procedures,
- operational misuse guidance,
- restricted instructions.

## Limitations
- Synthetic prompts and rubrics, not derived from real lab incidents.
- Coverage is illustrative rather than exhaustive.

## Intended use
- evaluation suite scaffolding,
- governance collateral,
- reproducible safety dashboards.

## Not intended for
- real-world operational decision-making without domain validation and oversight.

# outputs_safety_poc/docs/decision_memo.md

# Decision Memo, Safety Evaluation Readout 

**Program:** Lab-Integrated Scientific Model Safety Evaluation  
**Date:** 2026-01-28  
**Prepared by:** Mark I.R. Petalcorin  

## Executive summary
This memo summarises evaluation results for three model versions in a lab-integrated scientific setting:
- `v1_baseline`
- `v2_stronger`
- `v3_safeguarded`

The evaluation suite reports unsafe completion rates, calibration quality, selective deployment trade-offs, and drift monitoring signals. The goal is to support a decision-grade GO/NO-GO assessment under defined governance gates.

## Decision context
In lab-integrated systems, safety must be evaluated under:
- tool-use pressure and novelty,
- measurement noise and QC failures,
- distribution shift and drift.

Accordingly, we assess:
1) unsafe completion rate (overall and QC-pass subset)  
2) risk calibration (ECE, Brier)  
3) selective deployment feasibility (risk–coverage curves)  
4) drift signals by modality and pipeline (KS statistics)

## Findings
### 1. Unsafe completion outcomes
- `v3_safeguarded` shows substantially reduced unsafe completion vs `v1_baseline` and `v2_stronger` (Fig. 1).
- The reduction persists in the QC-pass decision-grade subset, suggesting the effect is not driven by QC filtering.

### 2. Calibration quality
- Calibration differs across versions (Fig. 2).
- A separate safety monitor is well-calibrated on held-out `v3` test split (Fig. 5), supporting an independent gating signal.

### 3. Selective deployment trade-off
- Risk–coverage curves show `v3_safeguarded` offers the best operating region for reducing unsafe rate while preserving throughput (Fig. 3).
- This supports a policy of threshold-based acceptance with review escalation above threshold.

### 4. Drift monitoring
- Drift is modality-specific (Fig. 4).
- The largest KS shifts are observed in predicted risk for qPCR-like biological slice, motivating monitoring triggers and review.

## Governance gates
A release candidate is evaluated against gates:

- **Gate A, Unsafe rate:** unsafe completion rate must remain below threshold overall and in QC-pass subset.
- **Gate B, Calibration:** ECE and Brier must remain within bounds suitable for thresholding.
- **Gate C, Selective policy:** feasible risk threshold must exist to meet unsafe rate target at acceptable coverage.
- **Gate D, Drift:** KS drift must remain below trigger thresholds, or review escalation is required.

## Decision recommendation (Proof-of-Concept)
- `v1_baseline`: NO-GO, unsafe rate and selective deployment region are unfavorable.
- `v2_stronger`: NO-GO, unsafe outcomes remain high and calibration does not justify thresholding.
- `v3_safeguarded`: GO with conditions, deploy with selective acceptance thresholds, continuous monitoring, and incident review triggers.

## Required deployment conditions for GO
- enforce risk-threshold acceptance with review escalation,
- log tool calls and lab telemetry for audit,
- monitor drift weekly and after workflow/instrument updates,
- recalibrate monitor on schedule or upon drift triggers,
- maintain rollback and restricted-mode switches.

## Artifacts
- Figures: `outputs_safety_poc/figures/fig1–fig5_*.png`
- Tables: `outputs_safety_poc/tables/*.csv`
- Manifest: `outputs_safety_poc/manifests/portfolio_manifest.json`
