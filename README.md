# Early Satellite Collision-Risk Prediction Using Temporal Conjunction Data

## Project Overview

Satellite conjunctions occur when two objects in orbit may come close enough to pose a collision risk. Detecting high-risk events early is important because operators need sufficient time to assess the situation and decide whether an avoidance action may be necessary.

This project is an exploratory research study using the **ESA Kelvins Collision Avoidance Challenge** dataset. The goal is to investigate whether information from multiple Conjunction Data Messages (CDMs) can improve early identification of high-risk events compared with using only the latest available CDM.

**Research Question:**  
Does the temporal evolution of CDM information improve early high-risk prediction compared with using only the latest available CDM, and how does this effect change across lead times from 6 to 2 days before the Time of Closest Approach (TCA)?

This work was carried out for research and learning purposes. It is **not** an operational collision-avoidance system.

## Dataset

- Source: ESA Kelvins Collision Avoidance Challenge
- Contains real Conjunction Data Messages (CDMs) for potential satellite conjunction events
- Analysis focused on CDMs available between 2 and 6 days before TCA
- High-risk definition: final collision probability ≥ 10⁻⁶ (log₁₀ risk ≥ −6)

Each conjunction event consists of a sequence of CDMs. Instead of treating every CDM independently, the analysis considers the evolution of information over time.

## Methodology

Three approaches were compared at five lead times (6, 5, 4, 3, and 2 days before TCA):

1. **Persistence Baseline** – uses the latest available risk value
2. **Single-CDM Model** – machine learning using only the most recent CDM
3. **Temporal Model** – machine learning using the latest CDM together with features derived from previous CDMs to describe how risk and related quantities change over time

### Evaluation

Models were evaluated using Repeated Stratified 5-Fold Cross-Validation (3 repeats).  
Metrics reported: Recall, Precision, F₂, ROC-AUC, and PR-AUC.

Because high-risk events are rare, particular attention was given to **Recall** and **PR-AUC**.

## Key Findings

- The latest available risk value is already a strong baseline for identifying high-risk events, even several days before TCA.
- Machine learning models achieve better ranking performance than the persistence baseline based on ROC-AUC and PR-AUC.
- Adding temporal information consistently improves ROC-AUC and PR-AUC compared with using only the latest CDM.
- The improvement in Recall from temporal features is limited.
- Prediction performance generally improves as the lead time approaches TCA.
- Overall, the evolution of CDM information contains useful predictive information for early high-risk detection, although a trade-off between recall and false alarms remains.

## Why This Matters

Early identification of potentially high-risk conjunctions can give satellite operators more time to investigate an event.

The purpose of this project was to understand the problem, work with real-world satellite conjunction data, formulate a clear research question, and examine whether temporal information from multiple CDMs provides additional predictive value.

## Limitations

This is an experimental study. Although repeated cross-validation provides more stable estimates than a single train-test split, further validation on independent data would be required before any operational use.

## Data Access

The dataset is available through the ESA Kelvins Collision Avoidance Challenge. It is not included in this repository. To reproduce the analysis, download the dataset and update the file path in the notebook.

## Files

- `Early_Collision_Risk_Prediction.ipynb` — Complete notebook containing data exploration, feature construction, modelling, evaluation, and results.

## Author

**Swathi Pasupuleti**  
B.Tech – Computer Science and Engineering (AI & Data Science)


**Note:** This project is an exploratory study of whether temporal CDM information can improve early high-risk prediction. The results should be interpreted as methodological findings rather than operational recommendations.
