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
- At each lead time L (6 to 2 days), only CDMs available at least L days before TCA are used. 13,154 events (162,634 CDMs) in the full training set; 11,942 events   (323 high-risk, 2.7%) at the 2-day lead time.
- Events available: 8,426 (6 days), 9,484 (5), 10,481 (4), 11,273 (3), 11,942 (2).
- High-risk definition: final collision probability ≥ 10⁻⁶ (log₁₀ risk ≥ −6)

Each conjunction event consists of a sequence of CDMs. Instead of treating every CDM independently, the analysis considers the evolution of information over time.

## Methodology

Three approaches were compared at five lead times (6, 5, 4, 3, and 2 days before TCA):

1. Persistence baseline: the latest available risk value is the score (for ROC-AUC and PR-AUC), and an event is flagged high-risk if that value is ≥ −6 (for recall, precision, F₂).
2. Single-CDM model: Random Forest (100 trees, max depth 5, class-weighted) using only the latest CDM’s risk and miss distance.
3. Temporal model: same Random Forest using the latest CDM plus features from all earlier CDMs (mean, min, and max risk, risk change, number of CDMs, mean miss distance).

### Evaluation

Models were evaluated using Repeated Stratified 5-Fold Cross-Validation (3 repeats).  
Metrics reported: 
- Recall, Precision, F₂, ROC-AUC, and PR-AUC.
- Each row is one conjunction event, so cross-validation splits are at event level and CDMs from the same event never appear in both train and test.

Because high-risk events are rare, particular attention was given to **Recall** and **PR-AUC**.

## Key Findings

- The latest available risk value is already a strong baseline (persistence ROC-AUC 0.89 at 6 days, 0.975 at 2 days).
- The Random Forest models rank high-risk events better than persistence (higher ROC-AUC and PR-AUC at every lead time), but at the fixed risk threshold they do  not improve recall or F₂ over persistence.
- Temporal features improve ROC-AUC and PR-AUC over the single-CDM model, with the largest PR-AUC gains at 3 to 4 days (about +0.16 to +0.17) and +0.12 at 2 days. At 6 days the PR-AUC gain is negligible (0.233 vs 0.237).
- Temporal features do not improve recall, and precision is roughly unchanged.
- Performance generally improves as the prediction point approaches TCA.
- Overall, the evolution of CDM information improves the ranking of high-risk events early, though identifying more of them without more false alarms remains challenging.

##PR-AUC, mean over repeated CV:

Lead time	 Persistence	    Single-CDM	Temporal
6 days	     0.161	             0.233	0.237
5 days	     0.197	             0.281	0.372
4 days	     0.252	             0.363	0.526
3 days	     0.323	             0.442	0.619
2 days	     0.459            	 0.594	0.710

## Why This Matters

Early identification of potentially high-risk conjunctions can give satellite operators more time to investigate an event.

The purpose of this project was to understand the problem, work with real-world satellite conjunction data, formulate a clear research question, and examine whether temporal information from multiple CDMs provides additional predictive value.

## Limitations

This is an experimental study. Repeated cross-validation gives more stable estimates than a single split, but all models use one fixed high-risk threshold (risk ≥ −6), and tuning it could change the recall and precision comparison. The number of events also varies by lead time. Further validation on independent data would be required before any operational use.

## Data Access

The dataset is available through the ESA Kelvins Collision Avoidance Challenge. It is not included in this repository. To reproduce the analysis, download the dataset and update the file path in the notebook.

## Files

- `Early_Collision_Risk_Prediction(3).ipynb` — Complete notebook containing data exploration, feature construction, modelling, evaluation, and results.

## Author

**Swathi Pasupuleti**  
B.Tech – Computer Science and Engineering (AI & Data Science)


**Note:** This project is an exploratory study of whether temporal CDM information can improve early high-risk prediction. The results should be interpreted as methodological findings rather than operational recommendations.
