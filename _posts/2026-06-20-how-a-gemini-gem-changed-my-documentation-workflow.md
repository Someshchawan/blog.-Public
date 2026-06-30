---
layout: post
title: "How a Custom Gemini Gem Changed My Documentation Workflow"
subtitle: "What happened when I stopped treating AI as an experiment and started building it into my daily process."
date: 2026-06-20
tags: [AI, Documentation, Gemini, Workflow]
description: "A practical account of building a custom Gemini Gem to automate documentation structuring, error detection, and content refinement, and what it actually changed about how I work."
---

There is a specific moment when an AI tool stops being something you try occasionally and starts being something you rely on. For me, that moment was a Tuesday afternoon when I realized I had finished structuring a table of contents for a new product area in four hours instead of two days.

I did not plan to build a custom Gemini Gem. I planned to keep doing what I had been doing for years: reading through product specs, interviewing engineers, sketching out a content hierarchy on paper, reorganizing it three times, and eventually arriving at a structure I was confident about. That process worked. It also took two full days for every new product area, and I was responsible for two being built simultaneously.

## The problem was not writing. It was structuring.

Most conversations about AI in documentation focus on generating draft text. That was never my bottleneck. I can write a configuration guide or an API reference quickly once I know what the structure should be. The slow part was always the work that happens before writing: figuring out what content needs to exist, how it should be organized, what the user's journey through it looks like, and where the gaps are.

That structuring work is cognitively expensive because it requires holding the entire product in your head while simultaneously thinking about three different audience types (developers, administrators, and end users) who each need different things from the same underlying information.

## What the Gem actually does

I built the Gemini Gem around three specific tasks that were consuming most of my pre-writing time.

**Content structuring.** I feed in a product spec, release notes, or a set of Jira tickets describing new features, and the Gem generates a proposed table of contents with section headings, suggested page types (conceptual, procedural, reference), and a recommended reading order. I do not use the output as-is. I use it as a starting point that I then reshape based on what I know about our users. But starting from a structured proposal instead of a blank page cuts the initial planning phase dramatically.

**Error detection.** I paste existing documentation into the Gem and ask it to identify inconsistencies, outdated references, broken logical flows, and places where terminology usage is not consistent. This catches things that are genuinely hard to spot when you have been staring at the same content for weeks. Last month, it flagged a configuration parameter that had been renamed in the latest release but was still referenced by its old name in four different guides. I had read those guides multiple times and missed it every time.

**Content refinement.** After I write a first draft, I run it through the Gem with instructions to check for clarity, conciseness, and whether the content would make sense to someone encountering the product for the first time. This is not a grammar check. It is closer to having a second pair of eyes do a structural review, which is something I used to rely on colleagues for and could not always get on the timeline I needed.

## What actually changed

The table of contents planning time dropped from roughly two days to half a day. That is a 75% reduction, and I have been tracking it across six product areas over the past year to make sure the number holds up. It does.

But the number that matters more is what happened downstream. When I started at HCL, there was a documentation backlog that had been accumulating for three to four years. Features had shipped without documentation, existing guides had drifted out of sync with the product, and the support team was absorbing the cost of that gap through ticket volume. With the planning time freed up by the Gem, I cleared that entire backlog within a single quarter. Support tickets on the features I documented dropped to zero. Not reduced. Zero.

## What the Gem does not do

It does not replace editorial judgment. The structuring suggestions are a starting point, not a final answer. About 30% of the time, the Gem proposes a content hierarchy that makes logical sense from a product perspective but does not match how users actually approach the product. I catch that because I have spent years watching how users navigate documentation. The Gem has not.

It also does not write final copy. Every output needs human review, verification against the actual product behaviour, and editing for voice and tone consistency. I treat everything the Gem produces as an unverified first draft, the same way I would treat a draft from a junior writer. Useful. Not publishable.

## What I learned

The most valuable thing about building this tool was not the time savings, though those are real. It was the clarity it forced about which parts of my job are genuinely high-value human work and which parts are pattern recognition that a model can do faster.

Structuring content from a product spec is mostly pattern recognition. Deciding whether that structure serves the user is judgment. Detecting inconsistent terminology is pattern matching. Knowing which inconsistency matters and which one is intentional is experience. Writing clear sentences is a learnable skill. Knowing which sentence the reader needs at this exact point in their journey is empathy.

The Gem handles the first half of each of those pairs. I handle the second half. That division of labor is what made it work, not as a novelty, but as core infrastructure for how I do my job every day.

---

*The Gemini Gem is a custom tool I built for my documentation workflow at HCL Software. The evaluation system I reference is part of my [AI Support Copilot](https://github.com/Someshchawan/ai-support-copilot) project.*
