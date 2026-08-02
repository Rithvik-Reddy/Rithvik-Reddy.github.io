---
layout: default
title: Artifact 4
---

# Title

Diagnosing Data Quality: Missing Values, Class Imbalance, and Model Drift

---

# Introduction

Most data-quality problems come with a default fix already attached: drop the missing rows, resample the minority class, retrain on fresh data. Each of those defaults is defensible in isolation and wrong often enough to matter. This artifact is a conversation with an AI tutor working through three data-quality scenarios — missing values, severe class imbalance, and post-deployment model drift — where the point wasn't applying the standard technique, but deciding whether the standard technique was the right one to apply at all.

---

# Description

The conversation covers three separate scenarios, each built around a decision that a generic textbook answer would get wrong.

The first scenario is a customer churn dataset with `last_login_date` missing on 15% of records. The discussion centers on diagnosing *why* the field is missing — comparing customers with and without a login date across account age, subscription type, activity level, signup source, and churn rate — before choosing a fix. A missing value could mean the customer never logged in, or it could mean a tracking system failed to record the date, and those two causes call for different treatment. The resolution converts the raw date into a "days since last login" feature, treats "never logged in" as its own case rather than folding it into a single imputed number, and adds a missing-value indicator flag, since median imputation alone would make inactive or unknown customers look average.

The second scenario is fraud detection on a dataset where only 0.5% of transactions are fraudulent. A model that predicts "legitimate" for everything scores 99.5% accuracy while catching zero fraud, which exposes accuracy as the wrong metric under this kind of imbalance. The discussion moves to precision, recall, F1, the confusion matrix, and precision-recall AUC, then to the actual fix: class weighting, oversampling the fraud class, undersampling the legitimate class, or moving the classification threshold. The threshold choice itself is reframed as a business question rather than a modeling one — what missed fraud costs in dollars, how much customer friction the business will tolerate from false positives, and how many alerts a review team can actually work through in a day.

The third scenario is a customer lifetime value model that performed well at launch and then started consistently overestimating value six months later, coinciding with a shift toward smaller and more frequent purchases and a new competitor entering the market. The conversation separates two distinct failure modes that get lumped together in casual use: data drift, where the input feature distributions shifted, and concept drift, where the relationship between customer behavior and lifetime value itself changed. The two failures call for different retraining strategies — data drift tolerates blending historical and recent data with recency weighting, while concept drift means older examples may actively mislead the model and the training window should shrink instead.

---

# Objective

The assignment asked for a conversation demonstrating how to reason through data-quality problems in machine learning, rather than simply naming the standard technique for each one.

I wanted each scenario to end with a decision that depended on investigation, not a lookup. A missing-value problem answered with "impute the median" or an imbalance problem answered with "use SMOTE" would have been correct-sounding and incomplete. The goal was to show the diagnostic step that has to happen before any of those techniques get chosen, and to make the business context — cost of an error, operational capacity, cause of a missing field — part of the technical decision instead of an afterthought.

---

# Process

Each of the three scenarios followed the same underlying discipline: diagnose the mechanism before picking the fix.

**Missing values.** Rather than treating the 15% missing `last_login_date` rate as a single imputation problem, the conversation started by asking what distinguishes the customers missing the field from those who aren't — account age, subscription type, activity level, signup source, and churn rate — because that comparison reveals whether missingness is informative (never logged in) or incidental (a system failed to record the date). Those two causes point to different fixes, so the resolution didn't stop at "impute a value." It engineered the field into "days since last login," carved out "never logged in" as its own explicit case instead of erasing it into a population average, and added a missing-indicator flag so the model can tell an imputed value from an observed one.

**Class imbalance.** The 99.5%-accuracy, zero-fraud-caught model is the standard illustration of why accuracy fails under imbalance, but the conversation pushed past naming better metrics (precision, recall, F1, confusion matrix, PR-AUC) and into the actual decision a team has to make: where to set the threshold. That decision was reframed entirely around business inputs — the dollar cost of a missed fraud case, the cost of customer friction from a false positive, and the review team's actual throughput — rather than treated as a modeling parameter to be tuned in isolation.

**Model drift.** The lifetime value model's overestimation was traced to two coincident causes — a behavior shift toward smaller, more frequent purchases, and a new competitor changing market pricing — and the conversation deliberately separated these into data drift versus concept drift, since conflating the two leads to the wrong retraining response. Diagnosis meant comparing current and training data distributions, checking prediction error by customer segment, and lining up the timing of the performance drop against the competitor's market entry. The retraining strategy that followed — how much historical data to keep, how heavily to weight recent data, whether to shrink the training window — depended entirely on which type of drift was confirmed.

---

# Tools and Technologies Used

- An AI tutoring bot, working through applied data-quality scenarios in supervised learning
- Missing-data diagnosis techniques: subgroup comparison, missing-indicator features, and cause-aware imputation as an alternative to blind median-fill
- Imbalanced-classification metrics: precision, recall, F1-score, confusion matrix, and precision-recall AUC, alongside class weighting, oversampling, undersampling, and threshold tuning
- Data drift and concept drift diagnosis, including distribution comparison and segment-level error analysis, to distinguish input-distribution shift from a changed input-output relationship
- Business-cost framing (cost of false negatives vs. false positives, reviewer throughput) as an input to threshold and retraining decisions rather than a post-hoc justification

---

# Value Proposition

Each of these three scenarios has a fast, generic answer that a team under deadline pressure will reach for first: drop the nulls, rebalance the classes, retrain on new data. This conversation shows why each of those defaults needs a diagnostic step in front of it — why the field is missing, what an error actually costs the business, and which kind of drift is degrading the model — before the fix gets chosen. Skipping that step doesn't just risk a worse model; in the missing-data and fraud cases specifically, it risks a model that looks fine on the metric you happened to check while quietly failing the customers or transactions that matter most.

---

# Unique Value

The fraud detection scenario is the clearest example of the conversation's actual thesis: precision and recall aren't values to optimize against each other in a vacuum, they're proxies for a cost the business has to state out loud. The conversation insists on that business-cost framing as a required input, not an optional refinement — you cannot pick a threshold correctly without first knowing what a miss and a false alarm each cost.

The drift scenario connects directly back to production concerns I raised in [Artifact 3](artifact-3.md), where I named data quality and offline/online pipeline drift as the two hardest parts of my actual day-to-day work. This conversation supplied the missing vocabulary for that intuition — separating "the inputs changed" (data drift) from "the relationship changed" (concept drift) gives me a concrete diagnostic split to reach for instead of a vague sense that "the model got worse."

---

# Relevance

Teams that treat data-quality problems as solved by a standard technique tend to get burned by the cases where the standard technique is actively wrong for the situation — median-imputing a field that's missing for a meaningful reason, or optimizing accuracy on a 0.5% positive class. The habit this conversation reinforces generalizes past these three scenarios: before applying a technique, ask what mechanism produced the problem, and let the answer to that question, plus the actual business cost of getting it wrong, decide which technique applies.

That habit carries directly into the RAG and LLM pipeline work I do now. A retrieval system that starts returning worse results doesn't announce whether the underlying document distribution shifted or whether user query patterns changed — the same data-drift-versus-concept-drift split applies, and the same discipline of diagnosing before fixing is what keeps a production system reliable instead of patched.

---

## Evidence

Summary of the tutoring conversation, covering three data-quality scenarios.

**Scenario 1 — Missing values in churn data.** The task was handling missing values in the `last_login_date` field of a customer churn dataset, affecting 15% of records. Deleting all incomplete rows risked removing useful information and introducing bias. The recommended approach was to investigate the cause of the missingness first — comparing customers with and without login dates across account age, subscription type, activity level, signup source, and churn rate — and to determine whether a missing value meant the customer had never logged in versus a tracking or system error. The date was then converted into a "days since last login" feature. Rather than relying only on median imputation, which could make inactive or unknown customers appear average, the solution treated "never logged in" as a separate case, used an appropriate imputed value, and added a missing-value indicator. Different approaches were to be compared using validation results before selecting the most reliable method.

**Scenario 2 — Fraud detection under class imbalance.** The dataset was highly imbalanced, with only 0.5% of transactions fraudulent. A classifier predicting every transaction as legitimate reached 99.5% accuracy but caught almost no fraud, demonstrating that accuracy is misleading when one class dominates. Better evaluation metrics included precision, recall, F1-score, the confusion matrix, and precision-recall AUC. Methods for handling the imbalance included class weighting, oversampling fraudulent transactions, undersampling legitimate transactions, and adjusting the classification threshold. The discussion centered on the precision-recall trade-off and framed the threshold decision around business questions: whether missing fraud or incorrectly flagging legitimate customers is more costly, the average financial loss from fraud, how much customer inconvenience is acceptable, and how many alerts the investigation team can review. If missed fraud is more costly, the model should favor recall by lowering the threshold; if false positives create excessive friction or overwhelm reviewers, the threshold should be raised to favor precision. The final threshold should be based on business costs and operational capacity rather than accuracy alone.

**Scenario 3 — Customer lifetime value model drift.** A customer lifetime value model that initially performed well began consistently overestimating value six months after deployment. Customer behavior had shifted toward smaller, more frequent purchases, and a new competitor had affected market pricing — indicating both data drift (the input feature distribution changed) and concept drift (the relationship between customer behavior and lifetime value changed). Diagnosis involved comparing current and training data, examining prediction errors across customer groups, and checking whether the performance decline aligned with the competitor's market entry. The model needed retraining on more recent data, potentially with greater weight on recent observations, and new features to capture changing pricing and market conditions.

The distinction between data drift and concept drift determined how historical data should be used in retraining. With pure data drift, older examples could still hold valid relationships, so retraining could blend historical and recent data, possibly weighting newer records more heavily. With concept drift, older relationships could actively mislead the model, so the training window should shorten, recent data should be emphasized, and the model's features or structure might need reconsideration. Historical data should not be automatically discarded; different time windows and weighting strategies should be tested instead. Going forward, both feature distributions and prediction errors should be monitored to identify whether inputs changed, the target relationship changed, or both.

**Overall takeaway.** Data-quality problems should not be handled with automatic fixes. Missing data requires understanding the cause of the missingness, imbalanced classification requires metrics and thresholds aligned with business consequences, and model degradation requires distinguishing between changes in input distributions and changes in the underlying relationship. Across all three scenarios, the strongest approach was to investigate the problem, compare multiple solutions using validation data, incorporate business context into model decisions, and continuously monitor performance after deployment.

---

[Back to artifacts](artifacts.md) | [Back to home](index.md)
