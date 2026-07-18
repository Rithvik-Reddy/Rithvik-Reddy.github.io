---
layout: default
title: Artifact 1
---

# Title

Behavioral Interview Coach: Designing, Building, and Testing a Coaching Bot

---

# Introduction

I struggled with behavioral interviews. The problem was never my experience. The problem was how I framed the story.

Most candidates rehearse alone. They already know the ending, so they fill the gaps mentally and never hear what is missing. An interviewer has none of that context. Friends give encouragement instead of diagnosis. Paid coaching costs money and takes scheduling.

This artifact documents a chatbot I designed to help with that gap. A Behavioral Interview Coach, planned with Design Thinking, built on Chatbase, and tested against twelve scenarios.

[Try the bot](https://www.chatbase.co/FQRNw2ulYhVtTy2hCGCV7/help)

---

# Description

The bot asks a behavioral question, waits, then scores the answer against the STAR method (Situation, Task, Action, Result). Feedback follows a fixed shape: one strength, one specific gap, and a rewrite of the single weakest component.

The STAR framework document defines each component with paired strong and weak examples, plus seven common failure patterns. The question bank holds forty behavioral questions across conflict, failure, leadership, and teamwork, each tagged with what the interviewer tests and what a red flag answer sounds like. The scoring rubric sets three tiers per STAR component. The sample answers file carries four annotated examples, including one humble brag the bot has to catch. The boundary responses file scripts ten refusals covering scope, privacy, and instruction override.

A test matrix accompanies the build. Twelve scenarios, one per behavioral rule, each with a named input and an expected behavior. All twelve passed on the live bot.

---

# Objective

I built the bot to work on a problem I lived through.

The course objective was to plan a bot with Design Thinking and build it on a no-code platform.

I also wanted to test an idea for myself. A well-scoped system prompt plus a curated knowledge base seemed like they would matter more than the base model. The claim comes up often in AI work, and I wanted to see the difference firsthand rather than take someone's word for it.

The design goal followed from the diagnosis. Candidates lack structure and lack feedback. They do not lack stories. So the bot coaches. The bot never writes the answer, because a candidate who reads a polished script learns little and struggles to deliver it under pressure.

---

# Process

**Plan before touching the tool.**

1. Ran Design Thinking. Empathy: the user is anxious, time-constrained, and cannot self-diagnose. Define: the gap is structure and feedback. Ideate: I weighed a resume reviewer, a mock interviewer, and a coach. Prototype: I chose the coach model because it leaves the work with the user. Test: twelve scenarios.
2. Answered ten planning questions in writing before opening Chatbase. Objective, knowledge sources, prohibitions, tone, media, opening message, conversation steps, ending condition, success criteria, and tests.
3. Wrote the prohibition list early. No writing answers for the user. No technical interview prep. No resume review. No hiring predictions. No collecting or echoing personal identifying information. One question at a time. No job market speculation.

**Build.**

4. Wrote and uploaded five knowledge documents to Chatbase, then wired the system prompt.
5. Set a fixed greeting as the initial message, ending in a constrained choice of four categories plus random.
6. Turned off image handling at the tool level rather than relying on the prompt to enforce a text-only rule.
7. Finished the build in under an hour. Most planning answers transferred into the system prompt with little editing.

**Test.**

8. Wrote twelve scenarios, one per rule, each mapping a rule to a test input and an expected behavior.
9. Ran every scenario against the live bot and logged the result. Three targeted robustness rather than helpfulness: instruction override resistance, scripted opening adherence, and catching the humble brag.

**Iterate.**

Two failures reshaped the build.

The bot answered its own questions. Running Llama as the base model, the bot asked a conflict question and then produced a sample answer in the same turn, which undercuts the point of a coach. My original stop instruction was too soft. I rewrote it as a hard boundary. After asking a question, your turn ENDS. Output the question and nothing else. Do not provide a sample answer. Do not add tips. Wait for the user. The behavior stopped. Other base models never showed the failure, which suggests a weak prompt stays hidden until a weaker model exposes it.

Responses ran past 400 words. The bot reviewed all four STAR components at once, and a user cannot fix four things at the same time. Asking for brevity did nothing. A hard 150-word cap worked, but only after I added a cut-priority rule: when the response exceeds the cap, cut the rewrite example, never the diagnosis. A limit without a priority rule leaves the model to guess what matters.

---

# Tools and Technologies Used

- Chatbase, for the build, knowledge base retrieval, base model selection, and live testing
- Design Thinking framework, for the planning phase
- STAR method, as the backbone of the knowledge base
- Claude Opus, to draft the knowledge documents and generate the test scenarios, which I then verified against published career center guidance
- Published guidance from MIT, University of Washington, Harvard Medical School, University of Florida, University of Arkansas, and Wichita State, as the source material behind the question bank

---

# Value Proposition

The bot gives a job seeker unlimited practice reps with structured feedback. Feedback names a STAR component rather than offering encouragement, which is the part friends and family cannot provide.

The build also produced a finding I did not expect. Base model choice barely moved results. The knowledge base and the system prompt moved almost everything. Stripped of both, the bot returned generic advice a search engine already gives. A smaller model handled the task without a drop in quality, which matters for anyone weighing cost against capability.

The wider takeaway is about method. Writing the rules down and turning each into a test caught problems I would have missed by trying the bot a few times and calling it finished.

---

# Unique Value

I built this from the user side. I sat in behavioral interviews and framed my stories badly, so the design started from a problem I understood rather than a feature list.

Two of the seven prohibitions exist for ethical reasons rather than scope reasons. Interview stories carry real names of managers and coworkers, and users volunteer them without thinking. So the bot coaches around the detail without repeating a name, an employer, or an email. The bot also refuses to predict hiring outcomes. Guessing at someone's odds is not a coach's role, and a confident wrong guess changes how a candidate walks into a room.

The engineering habits came from my day job. I build backend services in regulated financial environments, where a system has to hold up under review. Writing down what a system refuses to do, and testing each refusal, is a normal expectation there. Applying the same habit to a small chatbot felt natural, and I think the bot is better for it.

---

# Relevance

Many organizations now have an LLM feature shipped or on the roadmap, and evaluation practice tends to lag behind. A common question follows: how do you know the system does what you claim.

This project offered me a small answer, and one I plan to keep using.

Write down what the system must refuse before writing what it does. Turn each rule into a test with a named input and an expected behavior. Try the tests on a weaker model, because a weaker model exposes prompt gaps a stronger one covers up. When a constraint fails, add structure rather than adjectives.

The scale here is twelve rows in a table. The habit carries.

---

## Evidence

- [Try the Behavioral Interview Coach live](https://www.chatbase.co/FQRNw2ulYhVtTy2hCGCV7/help)
- [Download the full lab documentation, including the complete test matrix and prompt log (Word)](assets/ai-lab-documentation.docx)

[REPLACE: Add your two screenshots to the assets folder.]

![The bot loads with the scripted greeting, four category options, and two suggested prompts](assets/figure-1-opening.png)

*Figure 1. The scripted opening. The greeting rendered exactly as written in the system prompt, ending in a constrained choice.*

![A live feedback exchange showing the component-level STAR score](assets/figure-2-feedback.png)

*Figure 2. A live exchange. The bot asked one conflict question and stopped without answering itself. After the user answer, the bot produced a component-level score, credited the Action, and flagged the missing Task.*

---

[Back to artifacts](artifacts.md) | [Back to home](index.md)