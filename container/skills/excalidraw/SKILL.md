---
name: excalidraw
description: Create Excalidraw diagrams that make visual arguments — workflows, architectures, concepts. Renders to PNG and sends the image to the user. Use when the user wants to visualize or diagram something.
allowed-tools: Bash, Read, Write, mcp__nanoclaw__send_file, mcp__nanoclaw__send_message
---

# Excalidraw Diagram Creator

Generate `.excalidraw` JSON files that **argue visually**, not just display information. Then render them to PNG and send the image directly to the user.

## Quick Workflow

1. Plan the diagram (see Design Process below)
2. Write the `.excalidraw` JSON to `/workspace/group/diagrams/<name>.excalidraw`
3. Render to PNG: `python3 ~/.claude/skills/excalidraw/references/render_excalidraw.py /workspace/group/diagrams/<name>.excalidraw`
4. Read the PNG to inspect it visually
5. Fix any issues and re-render (repeat until it looks right)
6. Send to user: use `mcp__nanoclaw__send_file` with `filePath: "/workspace/group/diagrams/<name>.png"`

## Customization

**All colors live in one file:** `~/.claude/skills/excalidraw/references/color-palette.md`. Read it before generating any diagram and use it as the single source of truth for all color choices.

---

## Core Philosophy

**Diagrams should ARGUE, not DISPLAY.**

A diagram isn't formatted text. It's a visual argument that shows relationships, causality, and flow that words alone can't express. The shape should BE the meaning.

**The Isomorphism Test**: If you removed all text, would the structure alone communicate the concept? If not, redesign.

**The Education Test**: Could someone learn something concrete from this diagram, or does it just label boxes? A good diagram teaches—it shows actual formats, real event names, concrete examples.

---

## Depth Assessment (Do This First)

Before designing, determine what level of detail this diagram needs:

### Simple/Conceptual Diagrams
Use abstract shapes when:
- Explaining a mental model or philosophy
- The audience doesn't need technical specifics
- The concept IS the abstraction (e.g., "separation of concerns")

### Comprehensive/Technical Diagrams
Use concrete examples when:
- Diagramming a real system, protocol, or architecture
- The diagram will be used to teach or explain
- The audience needs to understand what things actually look like
- You're showing how multiple technologies integrate

**For technical diagrams, you MUST include evidence artifacts** (see below).

---

## Research Mandate (For Technical Diagrams)

**Before drawing anything technical, research the actual specifications.**

If you're diagramming a protocol, API, or framework:
1. Look up the actual JSON/data formats
2. Find the real event names, method names, or API endpoints
3. Understand how the pieces actually connect
4. Use real terminology, not generic placeholders

---

## Evidence Artifacts

Evidence artifacts are concrete examples that prove your diagram is accurate and help viewers learn. Include them in technical diagrams.

| Artifact Type | When to Use | How to Render |
|---------------|-------------|---------------|
| **Code snippets** | APIs, integrations, implementation details | Dark rectangle + syntax-colored text |
| **Data/JSON examples** | Data formats, schemas, payloads | Dark rectangle + colored text |
| **Event/step sequences** | Protocols, workflows, lifecycles | Timeline pattern |
| **UI mockups** | Showing actual output/results | Nested rectangles |
| **API/method names** | Real function calls, endpoints | Use actual names from docs |

---

## Multi-Zoom Architecture

Comprehensive diagrams operate at multiple zoom levels simultaneously:

- **Level 1: Summary Flow** — Overview of the full pipeline at a glance
- **Level 2: Section Boundaries** — Labeled regions grouping related components
- **Level 3: Detail Inside Sections** — Evidence artifacts, code snippets, concrete examples

**For comprehensive diagrams, aim to include all three levels.**

---

## Container vs. Free-Floating Text

**Not every piece of text needs a shape around it.** Default to free-floating text. Add containers only when they serve a purpose:

| Use a Container When... | Use Free-Floating Text When... |
|------------------------|-------------------------------|
| It's the focal point of a section | It's a label or description |
| Arrows need to connect to it | It describes something nearby |
| The shape carries meaning | It's a section title or annotation |

**Aim for <30% of text elements inside containers.**

---

## Design Process (Do This BEFORE Generating JSON)

### Step 0: Assess Depth Required
Simple/Conceptual or Comprehensive/Technical?

### Step 1: Understand Deeply
- What does each concept DO?
- What relationships exist?
- What's the core transformation or flow?

### Step 2: Map Concepts to Patterns

| If the concept... | Use this pattern |
|-------------------|------------------|
| Spawns multiple outputs | **Fan-out** (radial arrows from center) |
| Combines inputs into one | **Convergence** (funnel) |
| Has hierarchy/nesting | **Tree** (lines + free-floating text) |
| Is a sequence of steps | **Timeline** (line + dots + labels) |
| Loops or improves | **Spiral/Cycle** (arrow returning to start) |
| Is abstract state/context | **Cloud** (overlapping ellipses) |
| Transforms input to output | **Assembly line** (before → process → after) |
| Compares two things | **Side-by-side** (parallel with contrast) |
| Separates into phases | **Gap/Break** (visual separation) |

### Step 3: Ensure Variety
Each major concept must use a different visual pattern. No uniform cards or grids.

### Step 4: Sketch the Flow
Trace how the eye moves through the diagram.

### Step 5: Generate JSON (Section by Section)
For large diagrams, build one section at a time — do NOT generate the entire diagram in one pass.

### Step 6: Render & Validate (MANDATORY)
After generating JSON, render it, view it, and fix issues — in a loop until it's right.

---

## Large Diagram Strategy

**For comprehensive/technical diagrams, build the JSON one section at a time.** Do NOT attempt to generate the entire file in a single pass.

1. Create the base file with the JSON wrapper and first section
2. Add one section per edit
3. Use descriptive string IDs (e.g., `"trigger_rect"`, `"arrow_fan_left"`)
4. Namespace seeds by section (section 1 uses 100xxx, section 2 uses 200xxx)
5. Update cross-section bindings as you go
6. After all sections, review the whole for binding correctness
7. Then render & validate

---

## Visual Pattern Library

### Fan-Out (One-to-Many)
Central element with arrows radiating to multiple targets.

### Convergence (Many-to-One)
Multiple inputs merging through arrows to single output.

### Tree (Hierarchy)
Use `line` elements for trunk and branches, free-floating text for labels. No boxes needed.

### Timeline
Vertical/horizontal line + small dots (10-20px ellipses) at intervals + free-floating labels.

### Spiral/Cycle
Elements in sequence with arrow returning to start.

---

## Shape Meaning

| Concept Type | Shape |
|--------------|-------|
| Labels, descriptions, details | **none** (free-floating text) |
| Section titles, annotations | **none** (free-floating text) |
| Markers on a timeline | small `ellipse` (10-20px) |
| Start, trigger, input | `ellipse` |
| End, output, result | `ellipse` |
| Decision, condition | `diamond` |
| Process, action, step | `rectangle` |

---

## Modern Aesthetics

- **Roughness**: `0` for clean/modern, `1` for hand-drawn
- **Stroke Width**: `1` thin, `2` standard, `3` emphasis
- **Opacity**: Always `100` — use color and size for hierarchy, not transparency
- **Font**: `fontFamily: 3` (monospace) always

---

## Layout Principles

| Size | Use For |
|------|---------|
| 300×150 | Hero / most important |
| 180×90 | Primary |
| 120×60 | Secondary |
| 60×40 | Small |

Whitespace = importance. Most important elements have the most empty space (200px+).

---

## JSON Structure

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": 20
  },
  "files": {}
}
```

See `~/.claude/skills/excalidraw/references/element-templates.md` for copy-paste element templates.
See `~/.claude/skills/excalidraw/references/color-palette.md` for colors.
See `~/.claude/skills/excalidraw/references/json-schema.md` for full schema.

---

## Render & Validate (MANDATORY)

```bash
python3 ~/.claude/skills/excalidraw/references/render_excalidraw.py /workspace/group/diagrams/<name>.excalidraw
```

This outputs a PNG at the same path with `.png` extension. Then use the **Read tool** on the PNG to view it.

### The Loop

1. **Render & View** — Run the render script, then Read the PNG
2. **Audit** — Does the visual structure match your plan? Does each section use the right pattern?
3. **Check for defects** — Text clipping, overlapping elements, misaligned arrows, uneven spacing
4. **Fix** — Edit the JSON, re-render, re-view
5. **Repeat** — Until the diagram is right (typically 2-4 iterations)

### When to Stop

- Rendered diagram matches the conceptual design
- No text is clipped, overlapping, or unreadable
- Arrows route cleanly
- Spacing is consistent and balanced

---

## Sending the Image

After the diagram passes validation, send it to the user:

```
mcp__nanoclaw__send_file
  filePath: "/workspace/group/diagrams/<name>.png"
  caption: "Here's your diagram, Doctor."
```

Always save diagrams to `/workspace/group/diagrams/` so they persist in the workspace.

---

## Quality Checklist

### Depth & Evidence
1. Research done (for technical diagrams)?
2. Evidence artifacts included?
3. Multi-zoom architecture (summary + sections + detail)?
4. Concrete over abstract?

### Conceptual
5. Isomorphism test passed?
6. Variety — different pattern per major concept?
7. No uniform containers/card grids?

### Container Discipline
8. Minimal containers (<30% of text elements inside shapes)?
9. Lines for tree/timeline structures?
10. Typography creating hierarchy?

### Structural
11. Every relationship has an arrow or line?
12. Clear visual flow path?
13. Important elements are larger/more isolated?

### Technical
14. `text` contains only readable words?
15. `fontFamily: 3`?
16. `roughness: 0`?
17. `opacity: 100` for all elements?

### Visual Validation
18. Rendered to PNG and inspected?
19. No text overflow?
20. No overlapping elements?
21. Even spacing?
22. Arrows land correctly?
23. Readable at export size?
24. Balanced composition?
