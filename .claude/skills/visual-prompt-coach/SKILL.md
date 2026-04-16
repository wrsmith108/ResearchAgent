---
name: visual-prompt-coach
description: Design visuals for technical course materials. Applies Dan Roam, Mayer's multimedia principles, C4, cognitive load theory, and Gestalt/CRAP to pick the right visual type and tool (Mermaid or Gemini/Nano Banana), then returns a ready-to-paste prompt plus framework rationale. Use when the user says any of design a visual, course diagram, illustration prompt, infographic for lesson, visualize this concept, what diagram should I use, help me prompt an image, visual for my course, diagram for a lesson.
author: williamsmith
version: 1.0.0
category: productivity
tags:
  - visual
  - course
  - frameworks
  - mermaid
  - gemini
  - instructional-design
license: MIT
created: 2026-04-16
---

# Visual Prompt Coach

## What This Skill Does

Turns a vague "I need a visual for this lesson" into a **tool-appropriate, instructionally-sound prompt**. The skill runs a short intake, classifies what the visual is actually supposed to teach, applies established visual-thinking and multimedia-learning frameworks, and returns three things: the framework rationale, the recommended tool (Mermaid/PlantUML for structural diagrams, Gemini/Nano Banana for illustrations and infographics), and a ready-to-paste prompt.

Scope: **technical/software course materials**. In scope: diagrams, illustrations, infographic panels. Out of scope: data charts.

## Quick Start

Trigger by asking for a visual in natural language. Example:

> "Help me design a diagram for a lesson on OAuth 2.0 authorization code flow."

The skill will ask up to four intake questions, then return framework rationale + tool pick + a ready-to-paste prompt.

## When to Use

**Use when the user says any of:**
- "design a visual / diagram / illustration"
- "course diagram", "diagram for a lesson"
- "illustration prompt", "infographic for a lesson"
- "visualize this concept", "what diagram should I use"
- "help me prompt an image for..."

**Do not use for:**
- Data charts (bar/line/pie/scatter) — out of scope for v1.
- Visuals unrelated to instruction or course material.
- When the user already has a fully-specified prompt and just wants it run.

## How It Works

Four phases. Always proceed in order; never skip intake.

1. **Intake** — ask the four questions below via `AskUserQuestion`. Stop early only if an earlier answer makes a later question moot (e.g., the user names a specific diagram type, collapsing Q2).
2. **Classify** — map the answers to a visual archetype using the classification table below. This decides whether the output is structural (Mermaid) or pictorial (Gemini/Nano Banana).
3. **Framework check** — apply the relevant frameworks from `frameworks.md`. Always apply Mayer + Cognitive Load Theory as a quality gate. Add Dan Roam for archetype selection, C4 for architecture diagrams, and Gestalt/CRAP for infographic layout.
4. **Generate** — fill the matching template from `prompt-templates.md`. Return the three-block output contract below.

## Intake Questions

Ask these with `AskUserQuestion` in a single call (short intake, max four questions).

1. **Learning objective** — what should the learner be able to do after seeing this visual?
   - Remember (recall terminology, labels)
   - Understand (explain a concept in their own words)
   - Apply (use the concept in a new context)
   - Analyze (break down, compare, debug)

2. **What is being shown** — pick the dominant relationship.
   - Structure / components (what the system is made of)
   - Process / sequence (what happens in what order)
   - Relationship / hierarchy (how things relate or classify)
   - Concrete scene / metaphor (a tangible picture that anchors an abstract idea)
   - Multi-panel comparison (side-by-side teaching unit)

3. **Audience prior knowledge** — drives intrinsic-load budget.
   - Novice (first exposure to this topic)
   - Intermediate (familiar with surrounding concepts)
   - Expert (reference material, not introduction)

4. **Hard constraints** — aspect ratio, brand/palette, must-include elements, text-in-image yes/no, style preference (diagram, flat illustration, isometric, hand-drawn).

## Visual-Type Classification

| Answer to Q2 | Archetype | Tool | Template |
|---|---|---|---|
| Structure / components | Architecture diagram (C4) | Mermaid | `C4Context` / `C4Container` |
| Process / sequence — actors interacting | Sequence diagram | Mermaid | `sequenceDiagram` |
| Process / sequence — single flow | Flowchart | Mermaid | `flowchart TD` |
| Relationship / hierarchy | Class diagram or mind map | Mermaid | `classDiagram` |
| Concrete scene / metaphor | Illustration | Gemini / Nano Banana | Illustration scaffold |
| Multi-panel comparison | Infographic | Gemini / Nano Banana | Infographic scaffold |

If Q2 is ambiguous, fall back to Dan Roam's 6×6 (see `frameworks.md`) to pick the archetype from the question the visual is answering ("what / how much / where / when / how / why").

## Output Contract

Always return exactly these three blocks, in this order, clearly labeled:

**1. Framework rationale** — one short paragraph. Name every framework that shaped the output and the single decision each one drove. Example: *"Dan Roam's 6×6 picked sequence over flowchart because the question is 'how' across multiple actors. C4 set the level at container (not context, not component). Mayer's signaling principle drove the highlight on the redirect step. Cognitive load theory capped the diagram at 7 labeled arrows for a novice audience."*

**2. Tool recommendation** — one line. Name the tool, and the single reason it won.

**3. Ready-to-paste prompt** — the actual artifact the user will use.
- For Mermaid: a fenced ` ```mermaid ` block with a complete, renderable diagram.
- For Gemini / Nano Banana: a structured text prompt with sections for Subject / Style / Composition / Lighting / Constraints (illustrations) or Panel layout / Panel content / Typography / Spacing / Constraints (infographics).

Never return the prompt without blocks 1 and 2. The rationale is the point — the user asked for it explicitly.

## References

Load these two files only when the relevant branch of the flow fires (progressive disclosure):

- **`frameworks.md`** — load during the Framework check phase. Contains Dan Roam 6×6/SQVID, C4, Mayer's multimedia principles, Cognitive Load Theory, and Gestalt/CRAP, each with "Use when / Key rules / Decision lever."
- **`prompt-templates.md`** — load during the Generate phase. Contains Mermaid skeletons (flowchart, sequence, class, C4) and Gemini/Nano Banana scaffolds (illustration, infographic) with fillable slots.

## Out of Scope (v1)

- Data charts (bar/line/pie/scatter).
- Stateful memory of course context across runs — the skill is stateless.
- Hybrid outputs (Mermaid embedded in a Gemini infographic) — pick one tool per run.
- Publishing externally — this is a project-level skill.
