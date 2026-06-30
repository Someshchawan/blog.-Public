---
layout: post
title: "What I Learned Building an Evaluation Layer for AI-Generated Content"
subtitle: "The model said 'your account shows two transactions.' It was lying. Here is how I built the system that catches that."
date: 2026-06-25
tags: [AI, Evaluation, Quality, Python]
description: "A practical walkthrough of building a response evaluation system that catches hallucinations, error leaks, and low-quality outputs in AI-generated content before they reach users."
---

The response looked perfect. A customer had asked "Why was I charged twice?" and the AI responded: "I can see that your account shows two transactions on June 15. According to our records, the first charge was processed correctly and the second appears to be a duplicate."

Professional. Specific. Confidently delivered. And completely fabricated.

The AI had no access to any customer's account. It could not see transactions. It did not know what happened on June 15. It invented every detail because that is what language models do when asked about specific data they do not have. They fill in the blanks with plausible fiction and present it as fact.

That response is what motivated me to build an evaluation system. Not because I think AI-generated content is inherently bad, but because I think trusting it without verification is genuinely dangerous in any context where a user might believe what they read.

## Starting simple, failing instructively

My first attempt at evaluation was embarrassingly basic. Two checks: is the response shorter than 10 characters (probably empty), and does it contain the word "Error" anywhere (probably a raw error message). That was the entire quality gate between the AI and the user.

It caught almost nothing. A hallucinated response about fake account transactions would sail through both checks because it was long enough and did not contain the literal word "Error." An off-topic response about company history would pass. A response starting with "Great question!" followed by vague nothingness would pass.

I kept that first version for about a week before a particularly confident hallucination made me realize I was building a safety system that provided no actual safety.

## What the evaluation system does now

The current version runs 8 checks on every response before it reaches a user. Each check returns a score between 0.0 and 1.0, a pass/fail status, and a human-readable explanation of what it found.

The **hallucination check** is the one I find most important. It looks for phrases where the model claims access to data it cannot possibly have: "according to our records," "your account shows," "I can see that," "our database indicates." These phrases are the fingerprint of a model fabricating specificity to sound helpful. When detected, the response fails regardless of how high the other scores are. Hallucination is a hard gate, not a soft score.

The **error leak check** catches raw stack traces and Python exceptions appearing in user-facing responses. This sounds unlikely until you build a system where the model occasionally echoes back parts of its own error handling in the response text. A user should never see `Traceback (most recent call last)` in a support answer.

The **relevance check** extracts significant keywords from the user's query and verifies at least some of them appear in the response. It is a heuristic, not semantic similarity, but it catches the case where a user asks about password resets and the AI responds with a paragraph about the company's founding year.

The **uncertainty check** detects hedging language. When a response contains multiple phrases like "I am not sure," "I do not have access," and "it depends" in the same answer, the model is telling you it does not have a confident answer. The check flags this so a human can decide whether to show it or not.

The **filler check** catches responses that open with "Great question!" or "Sure, I would be happy to help!" instead of getting to the answer. These phrases add no value and waste the reader's time, which matters in a support context where someone is actively trying to solve a problem.

## Hard gates versus soft scores

The most important design decision was making certain checks act as hard gates. A hallucinated response fails the entire evaluation even if every other check scores perfectly. A leaked stack trace fails the entire evaluation. An empty response fails the entire evaluation.

I made this decision after seeing a response score 0.75 overall while containing fabricated account data. The average of all checks was above the threshold because the response was well-structured, relevant to the query, had no filler, and was an appropriate length. It just happened to also be lying about having access to user records.

Averaging quality scores hides critical failures. Hard gates prevent that.

## What the tests prove

I wrote 17 automated tests that each demonstrate a specific failure being caught. This was deliberate. The tests are not just verification. They are documentation. A reviewer who opens the test file sees concrete examples of what the evaluation system catches: a fabricated account access claim, a leaked stack trace, a completely off-topic response, a response full of hedging language, a filler opening.

Each test feeds a bad response into the evaluator and asserts that the right check caught it with the right score. This makes the evaluation system auditable and demonstrable, not just claimed.

## What this taught me about AI in documentation

Building this system changed how I think about using AI in professional documentation work. The question is not "can AI generate content?" It obviously can. The question is "how do you know the content is trustworthy before a user reads it?"

Most documentation teams using AI right now are operating without that second question. They generate drafts, do a quick human review, and publish. The human review catches the obvious errors but misses the subtle ones, which are the hallucinations that sound correct, the fabricated specifics that appear authoritative, and the confident answers that are confidently wrong.

An evaluation layer does not replace human review. It makes human review more effective by surfacing the responses most likely to contain problems before a human spends time on them. It is triage, not replacement.

The broader point is that AI-assisted documentation is not a choice between "use AI" and "do not use AI." It is a question of infrastructure. If you use AI without evaluation, you are publishing unverified content. If you use AI with evaluation, you are publishing content that has passed through a quality gate designed to catch the specific ways models fail. Both approaches use AI. Only one of them is responsible.

---

*The evaluation system described in this post is open source: [github.com/Someshchawan/ai-support-copilot](https://github.com/Someshchawan/ai-support-copilot). The full evaluation reference documentation is available at the [documentation site](https://someshchawan.github.io/ai-support-copilot-docs/docs/reference/evaluation-checks).*
