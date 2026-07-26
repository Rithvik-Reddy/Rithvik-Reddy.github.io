---
layout: default
title: Artifact 3
---

# Title

Supervised Learning Under Questioning: A Tutoring Conversation on Spam Filters and Loan Approval

---

# Introduction

Explaining a concept out loud exposes gaps that reading about it never does. This artifact is a transcript of a conversation with an AI tutor bot built for Workshop 3, designed to walk through the three-stage supervised learning workflow — Data Input, Model Training, Prediction — not by reciting definitions, but by defending them against two concrete scenarios: a spam filter and a bank loan approval system.

I came in already fluent in the mechanics: labeled data, loss functions, backpropagation, gradient descent, epochs, overfitting. The tutor didn't dwell there. It moved straight to asking me to map that theory onto a specific pipeline, then pushed the scenario from a low-stakes classification problem to a regulated, high-cost one, to see whether the reasoning held up when the consequences of an error stopped being trivial.

---

# Description

The conversation runs in two full passes through the three-stage workflow.

The first pass uses a spam filter: turning email text into word counts or embeddings, adding metadata features like sender history and link count, splitting the data by time instead of randomly so the model never trains on future emails, then training a model that outputs a junk-probability score and tuning the decision threshold around the asymmetric cost of a false positive (a real invoice in the junk folder) versus a false negative (spam in the inbox).

The second pass uses a loan approval system, which raises the stakes. Data input has to handle a defaults rate around three percent, which makes accuracy a useless metric and pulls in precision, recall, and expected dollar loss instead. It also has to keep protected and proxy attributes — race, gender, zip code — out of the feature set, since zip code alone reintroduces racial bias through the back door. Model training shifts from "get a usable ranking score" to calibration: a stated 12 percent default probability has to actually correspond to a 12 percent default rate, because the number sets the applicant's interest rate rather than just a spam/not-spam cutoff. Prediction adds a human-in-the-loop review for borderline cases, a legally required reason code attached to every denial, and long-latency label collection, since a loan's true outcome isn't known for years.

The conversation closes on a reflection tying the exercise back to my actual job: the production reality of data quality problems and drift between offline and online pipelines, specifically how duplicated preprocessing logic and inconsistent null-handling let a model's inputs silently diverge between training and serving.

---

# Objective

The workshop assignment asked for a recorded conversation with an AI tutor demonstrating understanding of the supervised learning workflow — data input, training, prediction — well enough to apply it to a new scenario, not just describe it in the abstract.

I wanted the conversation to do more than confirm I know the vocabulary. I pushed past the version of the answer a course would accept and toward the version a production engineer has to defend: what changes when the classes are imbalanced, what changes when errors have a dollar cost attached, and what changes when a regulator can ask you to justify a decision. That's the version of "understanding supervised learning" that matters in my actual work.

---

# Process

The conversation followed the tutor's structure, but the substance came from grounding each stage in a scenario I could reason about concretely rather than answering in the abstract.

**Established the baseline.** The tutor asked what I already knew. I gave the standard mechanics — labeled examples, forward pass, loss, backpropagation, gradient descent, epochs, the training/validation gap that signals overfitting — so the conversation could skip re-teaching fundamentals and go straight to application.

**Applied the framework to a low-stakes case.** For the spam filter, I walked through all three stages: featurizing text, time-based splitting to prevent leakage, threshold tuning around the cost asymmetry between the two error types, and the feedback loop where a user's spam report becomes a fresh label.

**Applied the framework to a high-stakes case.** The tutor then raised the difficulty by swapping in loan approval. This forced three additions the spam example didn't need: handling severe class imbalance without letting the model collapse to "approve everyone," building an explicit cost ratio into the loss instead of optimizing plain accuracy, and calibrating the output probability, since it directly sets a financial term rather than just ranking candidates.

**Connected the exercise to compliance and fairness.** I raised the legal requirement to exclude race and gender, and specifically flagged zip code as a laundered proxy for both — a constraint that doesn't show up in a textbook description of "features" but is non-negotiable in a regulated setting.

**Reflected on the gap between the exercise and daily work.** The tutor asked where this workflow shows up in my actual job, which is mostly RAG and LLM pipelines rather than classic training loops. I named two recurring failure points: data quality work outweighing everything else, and offline/online pipeline drift — specifically duplicated preprocessing logic and mismatched null-handling that shifts a model's scores with no visible error anywhere.

---

# Tools and Technologies Used

- An AI tutoring bot, structured around the three-stage supervised learning workflow (Data Input, Model Training, Prediction)
- Logistic-regression-style scoring, as the reference model for both the spam filter and loan approval examples
- Cost-sensitive learning and class-rebalancing concepts, applied to the loan default imbalance problem
- Probability calibration, as the standard distinguishing a ranking score from a number that sets a financial term
- Fair lending and adverse-action requirements, as the compliance constraint shaping the loan example's feature set and denial process
- My background in production RAG pipelines and AWS data work, as the source of the closing reflection on pipeline parity

---

# Value Proposition

Most explanations of supervised learning stop at the training loop. This conversation demonstrates the parts that determine whether a model survives contact with a real decision: what happens when the classes are imbalanced, what happens when the two error types cost different amounts, and what happens when a human has the legal right to ask why they were denied.

The spam-to-lending progression is deliberate. It shows the same three-stage skeleton holding up across a low-stakes and a high-stakes domain, while making explicit exactly what has to change at each stage when the cost of being wrong goes up — imbalance handling and cost-weighted loss in training, calibration and reason codes in prediction.

---

# Unique Value

The loan approval scenario isn't a generic textbook example — it's the same regulated-lending framing I used in [Artifact 2](artifact-2.md), where interpretability is a legal requirement rather than a design preference. Bringing that same domain into a live tutoring conversation let me stress-test the reasoning under direct questioning instead of writing it once and moving on.

The closing exchange is grounded the same way. Rather than a generic answer about "MLOps challenges," I named the two specific failure modes I actually see: preprocessing logic written once in a notebook and reimplemented in a service, and null-handling defaults that silently diverge between the two. Those are not textbook answers — they're the two things that have cost me debugging time.

---

# Relevance

Teams building ML or LLM systems tend to treat the training loop as the hard part and the surrounding pipeline as plumbing. This conversation argues the opposite emphasis: the training mechanics are the easy, well-documented part. The parts that actually determine whether a system is safe to ship are the ones that don't show up in a textbook diagram — class imbalance, cost-weighted loss, calibration, reason codes for denials, and keeping the offline and serving pipelines from quietly drifting apart.

That last point generalizes past classic supervised learning and into the LLM pipeline work I do day to day. A RAG system has the same offline/online parity risk as a loan model: whatever preprocessing or retrieval logic ran during evaluation has to be the exact logic running in production, or the system degrades with no error raised anywhere.

---
