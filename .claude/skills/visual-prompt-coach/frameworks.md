# Frameworks Reference

Short reference for the five frameworks the skill applies. Each section has three parts:

- **Use when** — conditions that trigger this framework in the flow.
- **Key rules** — the minimum you need to apply it.
- **Decision lever** — the single choice in the generated output that this framework actually drives. (This is what you cite in the "Framework rationale" block.)

---

## Dan Roam — Visual Thinking (6×6 and SQVID)

**Use when** — the user's intake answer to *"what is being shown?"* is ambiguous, or the visual archetype is not obvious from the question alone.

**Key rules**

The 6×6 maps the **question being answered** to a **visual archetype**:

| Question word | What the visual shows | Archetype |
|---|---|---|
| Who / what | Identity, parts | Portrait / diagram of parts |
| How much | Quantity | Chart *(out of scope for this skill)* |
| Where | Location, topology | Map / architecture diagram |
| When | Timing, sequence | Timeline / sequence diagram |
| How | Causation, process | Flowchart |
| Why | Multi-variable interaction | Multi-variable plot / concept map |

SQVID is a second-pass filter that sharpens the *style* of the chosen archetype along five axes: **S**imple↔elaborate, **Q**uality↔quantity, **V**ision↔execution, **I**ndividual↔comparison, **D**elta↔status quo. For course material, default to simple, quality, vision, individual, delta — novices learn better from stripped-down visuals that emphasize change.

**Decision lever** — picks the visual archetype (flowchart vs sequence vs map vs portrait), which in turn picks the Mermaid template or Gemini scaffold.

---

## C4 Model

**Use when** — the archetype is an architecture diagram (software structure).

**Key rules**

Pick exactly one level; never mix:

1. **Context** — the system as a black box, showing users and external systems it talks to. Default for introductions.
2. **Container** — applications, data stores, services inside the system. Default for most technical courses.
3. **Component** — the modules inside one container. Use when teaching a specific subsystem.
4. **Code** — classes/functions inside one component. Rarely needed; UML class diagrams are usually better here.

A diagram that jumps levels confuses learners. If the user is teaching *"how OAuth works"*, that is Context. If they are teaching *"how our auth service is built"*, that is Container.

**Decision lever** — which C4 level the Mermaid diagram is drawn at. Cite the level explicitly in the rationale.

---

## Mayer's Multimedia Learning Principles

**Use when** — always. This is the instructional quality gate. Apply it to every output, Mermaid or Gemini.

**Key rules** (the five that do the most work here)

- **Coherence** — remove anything that doesn't teach. No decorative backgrounds, no unrelated icons, no ornamental borders. Cut until cutting more would remove meaning.
- **Signaling** — highlight the one or two elements that carry the lesson. Color, weight, or position the critical step; leave the rest neutral.
- **Spatial contiguity** — put labels next to what they label. Never use legends when inline labels fit.
- **Pre-training** — if the visual introduces new vocabulary, show the components labeled *before* showing them in action. For a complex diagram, this may mean splitting into two panels.
- **Modality** — for a course, the visual pairs with spoken or written narration. Do not duplicate the narration as in-image text. Keep on-image text to labels and one-line captions.

**Decision lever** — drives what gets *removed* from the prompt (coherence) and what gets *emphasized* (signaling). Cite both in the rationale.

---

## Cognitive Load Theory (Sweller)

**Use when** — always, but the audience prior-knowledge answer (intake Q3) sets the budget.

**Key rules**

Three kinds of load:

- **Intrinsic** — the inherent complexity of the material. Match to the audience: novices get fewer elements and more labels; experts can handle denser visuals.
- **Extraneous** — load caused by poor design (decoration, mismatched labels, split attention). Drive this to zero.
- **Germane** — load that builds mental models. Add structure (grouping, consistent shapes, repeated visual grammar across lessons) to raise this.

Practical caps for course visuals:
- Novice: ≤ 7 primary elements, ≤ 5 labeled relationships.
- Intermediate: ≤ 12 elements.
- Expert: no hard cap, but still signal the critical path.

**Decision lever** — sets element-count and label-density caps that the generated prompt must respect. Cite the cap in the rationale when it shaped the output.

---

## Gestalt + CRAP

**Use when** — the output is an infographic, a multi-panel layout, or any Gemini/Nano Banana composition where spatial arrangement carries meaning.

**Key rules**

Gestalt principles (grouping):

- **Proximity** — elements close together read as a group. Use space to separate concepts.
- **Similarity** — elements with the same color/shape read as related. Use consistent visual grammar across a course.
- **Continuity** — the eye follows lines and curves. Arrange flow left-to-right or top-to-bottom in the direction of reading.
- **Closure** — the mind completes incomplete shapes. You can use open containers to group without heavy borders.

CRAP (layout):

- **C**ontrast — differentiate things that differ; don't make things subtly different, make them obviously different.
- **R**epetition — repeat visual elements (color, shape, type) to unify.
- **A**lignment — every element has a visible relationship to another on the page. Nothing floats.
- **P**roximity — restate of Gestalt proximity; related items group.

**Decision lever** — shapes the *Composition* / *Spacing and grouping* sections of the Gemini infographic prompt. Cite proximity and alignment specifically when they drove the panel layout.

---

## Which frameworks apply to which output

| Output | Always | Also |
|---|---|---|
| Mermaid — flowchart / sequence / class | Mayer, CLT | Dan Roam (if archetype unclear) |
| Mermaid — C4 architecture | Mayer, CLT, C4 | Dan Roam (if level unclear) |
| Gemini — illustration | Mayer, CLT | Dan Roam (metaphor picking) |
| Gemini — infographic | Mayer, CLT, Gestalt+CRAP | Dan Roam (per-panel archetype) |
