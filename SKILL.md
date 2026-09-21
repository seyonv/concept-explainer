---
name: concept-explainer
description: Use when the user asks for a visual explainer, "notes card", concept card, or screenshot-ready visual of a concept, term, or paper passage (e.g. "make a visual explainer of top-p", "explain quantization visually", "several beautiful visuals of LLM latency"). Produces flat, minimal, self-contained HTML cards — title + aliases, plain definition, worked example with real computed numbers, a 2x2 "why it beats the alternatives" grid, and an honest clarification box — plus optional overview cards that tie several concepts together.
---

# Concept explainer

Produce one screenshot-ready card per concept. Everything the reader needs lives inside the card, not in chat. The house style is fixed; copy `template.html` (same folder as this file) and fill it in. Do not restyle.

## Inputs

The context can be a paper passage, a screenshot, a concept name, or an open question ("how to think about LLM latency"). If the facts aren't in the context and the card needs real numbers (benchmarks, measured speeds, prices), research them first if you have web access, and cite the source in the card footer. Never invent a measured number. Illustrative numbers are fine only when labelled "illustrative".

## Card structure (in this order)

1. **Title + subtitle.** `h1` is the proper name. The subtitle gives common aliases, then one line on exactly how *this* source uses it (settings, values, section). With no source, say where it shows up in practice.
2. **In one breath.** A plain-language definition for a smart beginner, in 1–2 sentences.
3. **Worked example.** Use a chart, diagram, or table that shows the mechanism with specific, realistic numbers, fill in the formulas, and add a short caption. Pick one simple concrete example (tied to the source if there is one) and reuse it throughout the card. **Actually compute every number**; check the arithmetic with a quick script if there are more than a few. Show the computation (a running-total column, a `.formula` block, or annotated bars). Label anything the source doesn't give as illustrative.
   - Distributions, rankings, and cutoffs: use the horizontal bar pattern in `template.html` (label | bar | value | running total, with a dashed red cutoff line and greyed rows below it).
   - Sequences (tokens, steps, requests): use the `.chips` row pattern from `examples/speculative-decoding.html`.
   - Pipelines and flows: use inline SVG boxes and arrows, with every colour taken from `var(--…)`.
   - Comparisons of numbers: use a table with `.num` right-aligned tabular figures.
4. **2x2 grid.** Title it "Why it beats the alternatives": three alternatives, each with a red ✗, what it does and its specific failure mode, then this concept in the green `.win` cell with a ✓ and why it wins.
   - If there are no true alternatives, title it "How it differs from related ideas".
   - If the passage is mostly jargon, replace it with a glossary grid: one short cell per term, saying what it means and what the source's specific value does.
5. **Clarification box.** A `.note` that honestly answers the most likely misconception or the "is this always used / always better?" question, including when it's *not* the best choice and what people use instead.
6. **Footer (optional).** Related cards and sources for any researched numbers.

## Style rules (non-negotiable)

- One self-contained HTML file per card: a single `<style>` block, no `<script>`, no external fonts or images. Inline SVG is encouraged.
- Colours only via the CSS variables in the template, with light and dark modes included. That gives 2–3 colours: neutral text, green accent for the winner/positive, red for ✗ and cutoffs.
- Flat and minimal: no gradients, shadows, or emoji. Use sentence case everywhere.
- Type: title 26px, subtitle 15px muted, body 15px, captions 13px muted. Grid cells hold 1–3 sentences.
- Keep the card compact enough to screenshot in one shot: `max-width: 820px`, and aim for under one tall screen.
- Accuracy: if the source's claim is narrower than general practice, or a detail isn't specified, say so on the card.

## Multiple concepts and "tie it together" requests

When asked for several explainers, or for a topic rather than a single term:

1. Pick the concept list (one card each). Share it in one line and keep going; don't block on approval.
2. Write each concept card as above.
3. Add 1–2 **overview cards** (`_overview.html`, plus more if needed). Use the same shell, but swap the structure for:
   - **The mental model.** An SVG map of how the concepts relate: a pipeline, a tree, or a "knobs → metrics" diagram.
   - **What matters most.** A ranked table of factors with rough effect size (real numbers, sourced) and whether the reader controls it.
   - **Where to look it up.** Real tools, papers, or dashboards, with links.
   - A clarification box that sums up the main takeaway.
4. Write an `index.html` that links every card (a plain list in the same style).

## Output

- Default location: `./explainers/<topic-slug>/<concept-slug>.html` in the current working directory, or a path the user names.
- After writing, offer to open the files in a browser (`open` on macOS, `xdg-open` on Linux, `start` on Windows).
- In chat, just list the files and flag anything uncertain. Don't repeat the card content.

## Before you say done

- [ ] Every number on the card is computed, sourced, or labelled illustrative.
- [ ] The HTML makes no external requests and reads in both light and dark mode (no hard-coded hex colours outside `:root`).
- [ ] The grid has exactly 3 ✗ plus 1 ✓ (or it's the "related ideas" or glossary variant).
- [ ] The clarification box names at least one case where the concept is *not* the right choice.

See `examples/` for two finished cards in this style.
