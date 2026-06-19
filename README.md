## Synthetic Data Generation Methodology

To ensure absolute patient confidentiality while preserving the scientific value and mathematical integrity of the original study, a customized probabilistic data synthesis pipeline was developed using R. 

The generation script operates on a multi-stage resampling and distribution-matching framework:

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

All generated records are structured into the production-ready dataset `data/hypertension_mmas8_raw.csv`, serving as a verifiable and fully reproducible proxy for the clinical analysis pipeline.
