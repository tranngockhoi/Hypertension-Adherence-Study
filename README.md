# Antihypertensive Medication Patterns and Treatment Adherence Evaluation: A Cross-Sectional Clinical Study

A cross-sectional clinical study evaluating antihypertensive drug utilization and patient compliance patterns using the 8-item Morisky Medication Adherence Scale (MMAS-8) within a primary healthcare setting.

## Live Demonstration
The fully formatted, interactive clinical report is deployed and readable directly at: https://tranngockhoi.github.io/Hypertension-Adherence-Study/

## Abstract and Key Clinical Findings
* Background: Hypertension remains a primary driver of cardiovascular morbidity. Reaching target blood pressure depends heavily on long-term therapeutic compliance, yet non-adherence remains a paramount barrier at the primary care level.
* Objective: To investigate prescribing patterns and evaluate adherence behaviors among hypertensive outpatients.
* Key Findings: 
  * Monotherapy and dual therapy represented the predominant prescribing strategies, with CCBs and ARBs being the most frequently utilized drug classes.
  * Based on the MMAS-8 framework, a significant proportion of the cohort exhibited medium-to-low adherence. 
  * Forgetfulness (cognitive barrier) and missing doses when feeling well (behavioral barrier) were identified as the primary drivers of sub-optimal adherence, rather than regimen complexity or multi-drug therapy (p > 0.05).

## Dataset Notice and Data Synthesis Methodology
To comply with medical ethics, HIPAA, and patient data privacy regulations, the original primary clinical registry cannot be shared publicly. 

To maintain the scientific value and mathematical integrity of the study, a customized probabilistic data synthesis pipeline was developed using R. The generation script operates on a multi-stage resampling and distribution-matching framework:

### 1. Sociodemographic and Clinical Baseline Features
* Categorical Variables: Variables such as Occupation, Ethnicity, Residencies, Education, and Comorbidities were synthesized by calculating the exact marginal empirical probability distributions from the primary registry. Independent weighted sampling with replacement ensures the representation of each cohort remains statistically consistent.
* Gender Re-encoding: Labels were programmatically mapped from the original raw clinical notation ("Nam" / "Nữ") into standardized academic terminology ("Male" / "Female") during the frequency scanning phase to prevent localized encoding discrepancies.
* Continuous Physiological Parameters: Patient birth years, Systolic Blood Pressure (SBP), and Diastolic Blood Pressure (DBP) were modeled using Normal distributions derived from the real-world mean and standard deviation metrics. Values were subsequently rounded to integers to maintain biological plausibility.

### 2. Algorithmic Multi-Drug Therapy Assignment (Top-Down Constraint Matching)
To reflect realistic prescribing combinations without exposing individual medical charts, a top-down constraint loop was applied to the antihypertensive classes (ACEI, ARB, CCB, Diuretic, BB):
1. Regimen Complexity Vector: The exact number of concurrent medications per patient was calculated from the real database. The algorithm sample-assigns this count to the synthetic cohort first.
2. Weighted Probability Sampling: Individual drug classes were selected based on their overall clinical prevalence as weights, enforcing a strict non-replacement constraint (replace = FALSE) within each patient profile to prevent a single agent from being assigned multiple times.
3. Specific Medication Mapping: Once a class was assigned, a specific compound name was selected uniformly at random from the active formulary entries of that class.
4. Deterministic Synchronization: The final therapeutic Regimen classifier (Monotherapy, Dual therapy, Triple+ therapy, or No medication) was structurally synchronized downstream based on the actual synthetic drug count to achieve perfect mathematical alignment.

### 3. Psychometric Instrument Simulation (MMAS-8)
* The 8-item Morisky Medication Adherence Scale (Mas1 to Mas8) was simulated independently for each item by extracting empirical response vectors.
* Raw affirmative and negative localized metrics ("Có" / "Không") were translated to binary academic standard notation ("Yes" / "No") prior to weighted resampling. This method perfectly replicates the non-adherence patterns and cognitive behavior thresholds described in current literature.

All generated records are structured into the production-ready dataset data/hypertension_mmas8_raw.csv, serving as a verifiable and fully reproducible proxy for the clinical analysis pipeline.

## Reproducibility and Environment
To execute the source code and render the document locally in RStudio, ensure you have quarto installed along with the following R packages:
* tidyverse (for data manipulation and data synthesis)
* gtsummary (for clinical table generation)
* gt (for advanced table formatting)
* scales (for graphical percentage formatting)

### Repository Structure
```text
├── index.qmd                       # Core Quarto source file containing clinical code and narrative
├── index.html                      # Formatted production report (GitHub Pages deployment target)
├── styles.css                      # Global CSS override for layout justification and caption alignment
├── references.bib                  # BibTeX database containing validated medical literature citations
├── jama.csl                        # Journal of the American Medical Association citation style sheet
├── .gitignore                      # R and Quarto build artifact exclusion rules
└── data/
    └── hypertension_mmas8_raw.csv  # The fully de-identified synthetic clinical dataset
