---
layout: default
title: Artifact 2
---

# Title

Machine Learning versus Deep Learning: Matching the Method to the Problem

---

# Introduction

Most explanations of machine learning and deep learning stay abstract. They define terms and move on, without ever pinning either approach to a decision a practitioner actually has to make. That leaves a gap: knowing the definitions doesn't tell you which one to reach for.

This artifact closes that gap with two concrete cases. One is a problem simple machine learning solves well. The other only works as deep learning. Comparing them side by side, and being explicit about why the other approach would fail each one, makes the difference between the two categories concrete instead of academic.

---

# Description

The report walks through two applications. The first is credit scoring for loan approval, handled with a simple model like logistic regression or a decision tree reading structured applicant data. The second is medical image diagnosis, handled with a convolutional neural network reading raw pixel data from X-rays, CT scans, and MRIs.

For each, the report explains what makes the chosen approach the right fit and what would break if you swapped in the other one. It closes with a general rule for matching method to problem: structured, explainable data points toward simple machine learning; raw, high-dimensional data where patterns resist manual description points toward deep learning.

---

# Objective

The assignment asked for one example of simple machine learning and one example of deep learning, with a justification for each. I set out to pick examples that would hold up under real scrutiny rather than textbook ones chosen for convenience.

The credit scoring example was chosen deliberately. I work in financial services under FINRA oversight, and lending is a domain where the model's interpretability isn't a nice-to-have — it's a legal requirement. Picking an example grounded in my own professional context meant I could defend the reasoning if someone pushed back on it, not just recite a textbook justification.

---

# Process

I started from the constraint that would matter most in each domain, rather than the algorithm. For lending, that constraint is explainability: fair lending law requires a specific reason behind every denial, so the example had to lead with a model that exposes its own reasoning. For medical imaging, the constraint is the shape of the data itself: a single scan is millions of unstructured pixel values, which rules out any method that depends on someone hand-defining features first.

Working from the constraint outward, rather than starting with "here's an ML example, here's a DL example" and backfilling justification, kept both write-ups grounded in why the method fits instead of just asserting that it does.

The last step was checking my own reasoning against the counterfactual for each case: why would the other approach fail here, specifically. Stating the failure mode of the wrong tool — not just the strength of the right one — is what makes the comparison useful instead of a generic pro/con list.

---

# Tools and Technologies Used

- Logistic regression and decision trees, as the reference models for the credit scoring example
- Convolutional neural networks, as the reference architecture for the medical imaging example
- Fair lending / adverse action requirements, as the real-world constraint anchoring the interpretability argument
- My own background in regulated financial software, as the source of the credit scoring framing

---

# Value Proposition

A practitioner deciding between simple machine learning and deep learning rarely has a clean rule to reach for. This report gives one: look at whether the data is structured and explainable, or raw and high-dimensional, and let that decide the method before cost or hype does.

The comparison also pushes back on a common default — reaching for deep learning because it sounds more advanced. In lending, a deep network is not just unnecessary, it's a liability, since its opacity conflicts directly with a legal requirement to explain decisions. That's a case where the "simpler" model is the objectively correct engineering choice, not a compromise.

---

# Unique Value

The credit scoring example isn't a generic pick. It comes directly from the regulated financial environment I work in, where "the model said so" is not an acceptable answer to a denied applicant. That background is what makes the interpretability argument concrete instead of theoretical — I'm describing a real constraint I've had to design around, not one I looked up for the assignment.

---

# Relevance

Teams shipping AI features increasingly default to the most powerful model available, without asking whether the problem actually needs it. This report argues for the opposite habit: start from what the data looks like and what the decision requires — explainability, data volume, dimensionality — and let that dictate the method.

That habit generalizes past these two examples. Any team choosing between a simple model and a deep one can ask the same two questions this report asks: is the data structured enough for a human to define the features, and does the decision need to be explained to the person it affects. Those two questions cut through most of the "which model should we use" debate.

---

## Evidence

[REPLACE: Attach the full report as a downloadable file in assets/ if your instructor wants one, e.g. `assets/ml-vs-dl-report.docx`, and link it here as done in Artifact 1.]

### Simple Machine Learning: Credit Scoring for Loan Approval

Banks and lenders predict loan default risk with machine learning. A logistic regression or decision tree model scores each applicant, reading structured features like income, credit history, debt-to-income ratio, and payment history, and labeling the applicant low risk or high risk.

A simple model is the right tool here for a few reasons. The data is tabular and well-defined, and domain experts — not the algorithm — chose the features, because they already know which financial signals predict default. That matters because lending decisions have to be explainable: fair lending law requires a specific reason behind every denial, and a logistic regression makes that easy. You can point to the weight on each feature and show your work. The datasets involved are usually modest in size too, so a lighter model trains fast and performs well without needing millions of rows.

A deep neural network would be the wrong choice here. Its reasoning is opaque, and that opacity becomes legal exposure the moment a lending decision gets challenged. It also needs far more data and compute than this task requires, and none of that extra machinery buys any accuracy — the signal in clean tabular data with a handful of strong features is already there for a simple model to find. Manual feature selection already covers what the network would spend its capacity learning on its own.

### Deep Learning: Medical Image Diagnosis

Hospitals increasingly rely on convolutional neural networks to read X-rays, CT scans, and MRIs, flagging tumors, fractures, and other findings. This isn't hypothetical — CNNs already assist radiologists in cancer screening and diabetic retinopathy detection.

Here the situation flips. The input is raw pixel data, and a single image carries millions of values with no structure a human could reasonably hand-code. A CNN doesn't need that structure handed to it — it builds its own, layer by layer, starting with edges, then shapes, then the more abstract patterns that define a tumor or a lesion. Nobody sits down and writes rules for what a malignant mass looks like across every size, angle, and lighting condition. The visual patterns are too subtle and too numerous to specify by hand.

A traditional model can't compete here because it depends on someone engineering the features first, and that's not realistic for this kind of visual complexity. Run logistic regression or a decision tree directly on pixel values and it falls flat — there's no way for a shallow model to capture the spatial hierarchy that image recognition actually requires.

### Matching the Method to the Problem

The choice isn't about which approach is more advanced — it's about what the data looks like and what the job demands. Structured data with clear, well-understood features and a real need for explainability points toward simple machine learning. Raw, high-dimensional data like images or audio, where the patterns can't be written down by hand, points toward deep learning. Get that match right, and both accuracy and trust follow.

---

[Back to artifacts](artifacts.md) | [Back to home](index.md)
