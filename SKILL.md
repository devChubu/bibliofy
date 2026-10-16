---
name: bibliofy
description: >
  Generate a self-contained HTML learning guide with interactive features
  (theme toggle, scroll spy, reading progress, bookmarks, responsive drawer nav).
  Triggers: "create a learning guide", "generate a study guide", "build a tutorial",
  "make a textbook", "write educational content", "learning guide",
  "bibliofy", "learning-guide", or when the user wants a comprehensive, single-file
  HTML document that teaches a topic with chapters, diagrams, and code examples.
version: 1.0.0
---

# Bibliofy — Learning Guide Generator

Generates a self-contained HTML learning guide document. Two modes: **Project Guide** (documents a specific project) or **Textbook** (teaches the underlying concepts a project uses, from first principles). Single file, zero external dependencies, with three reading themes (Light, Dark, Book), interactive navigation, and rich content components.

## Output Format

- Single `.html` file, all CSS and JS inlined
- Zero external dependencies — no CDN, no build step, works offline
- Three theme modes: Light (modern docs), Dark (night reading), Book (warm paper, serif fonts)
- File naming: `{topic_slug}_learning_guide.html`
- Target size: 50-200 KB depending on content density

## Workflow

### Step 1: Analyze Context

Determine the mode, topic, and audience. Ask the user these questions:

1. **Mode** — Which type of guide?
   - **Project Guide** — Documents this specific project: its architecture, setup, workflows, and how things connect. The reader learns how *this codebase* works.
   - **Textbook** — Teaches the underlying concepts and technologies the project uses. The project serves as real-world examples, but the goal is general knowledge the reader can apply anywhere. Think university textbook, not project README.

2. **Topic** — If invoked from a project directory, read `CLAUDE.md`, scan project files (source code, schematics, documentation, specs, design files), and identify the domain. If the user provides a topic string, use that directly. Works for any domain: software, hardware, mixed-signal, systems engineering, or non-technical topics.

3. **Audience & prior knowledge** — Who is this for, and what level should the guide assume?

#### Mode differences in content generation:

**Project Guide mode:**
- Chapters organized around the project's architecture and components
- Content explains how and why things are built the way they are
- Code examples are from the actual project source
- Diagrams show the project's architecture, data flow, module relationships
- References point to project-specific dependencies, APIs, and tools

**Textbook mode:**
- Chapters organized around concepts and knowledge domains (e.g., "Interrupts", "RTOS", "Buck Converters")
- Content teaches from first principles — what, why, how, with theory and intuition
- Code examples illustrate concepts, using the project as one concrete example among general patterns
- Diagrams explain concepts generically (how an ADC works, how a state machine operates)
- References point to textbooks, standards, authoritative tutorials, and documentation
- Use callout boxes like "In Our Project" (concept callout) to bridge theory back to the specific codebase

### Step 2: Research

Gather authoritative information for accuracy and references:

- Use web search to verify facts, find authoritative sources, and identify best practices
- Collect 3-5 high-quality references per planned chapter (official docs, tutorials, papers, respected guides)
- Note any areas where information is sparse — flag these for the user

### Step 3: Generate Outline

Create a structured outline using the Part > Chapter > Section hierarchy:

```
Part I — Part Title
  Chapter 1: Chapter Title
    Key question: What does this chapter answer?
    Sections: Section A, Section B, Section C
    Est. reading time: ~8 min

  Chapter 2: Chapter Title
    ...
```

Guidelines:
- 2-4 parts, 3-5 chapters per part, 2-5 sections per chapter
- Each chapter should be ~1500-3000 words (8-15 min read)
- Each chapter has a "Key question" that it answers
- Total guide: 8-16 chapters (60-120 min total reading time)

**Present the outline to the user for approval. Do not proceed until approved or modified.**

### Step 4: Read Reference Files

Read both reference files from this skill's directory:

1. `references/template.html` — The complete HTML skeleton with all CSS, JS, and placeholder tokens
2. `references/content-guide.md` — Component catalog with HTML snippets for every content type

### Step 5: Generate HTML

**Sizing decision:** Estimate the guide size from the approved outline:
- **Small guide** (1-7 chapters): Generate in one pass — write the entire HTML file at once.
- **Large guide** (8+ chapters): Generate in **phases** to avoid context limits. Write the full HTML scaffold (all CSS, JS, cover banner, and complete sidebar nav for ALL chapters) with the first 3-4 chapters of content. Save the file. Then in subsequent phases, read the file, generate the next 3-4 chapters, and insert them before the bibliography section using Edit. Repeat until all chapters are written. The file must be valid, working HTML at every phase. Tell the user at the start of each phase: "Writing chapters N-M of T (Phase X)..."

Build the complete HTML file by filling the template:

1. **Replace placeholders** in the template:
   - `{{TITLE}}` — Guide title (appears in sidebar, cover banner, and `<title>`)
   - `{{SUBTITLE}}` — Short descriptor (appears in sidebar under title)
   - `{{DESCRIPTION}}` — One-to-three sentence description (cover banner)
   - `{{SIDEBAR_NAV}}` — Built from the approved outline using the sidebar nav pattern
   - `{{CONTENT}}` — All chapters, sequentially
   - `{{BIBLIOGRAPHY}}` — Master bibliography list items
   - `{{FOOTER}}` — Generation date and source info

2. **Write each chapter** following the chapter structure pattern from content-guide.md:
   - Open with `h2` + `chapter-summary` (with key question and reading time)
   - Write sections with `h3` headings, each with a clear ID
   - Use callout boxes where they fit naturally:
     - **Concept** for definitions and mental models
     - **Warning** for pitfalls and common mistakes
     - **Tip** for practical advice and shortcuts
     - **Key Takeaway** for chapter/section summaries (max one per section)
     - **Deep Dive** for optional advanced material (collapsible)
     - **Prerequisite** for assumed knowledge (link back to earlier chapters)
   - Include diagrams where visual explanation adds clarity
   - Add code examples with language labels where relevant
   - Close with per-chapter references and prev/next navigation buttons

3. **Visual content requirements:**
   - At least one visual element per 2-3 paragraphs of prose
   - Use CSS block diagrams (`.diagram-row`) for architecture, flow, and pipelines
   - Use inline SVG for flowcharts and state machines (must use CSS variables for theme compatibility)
   - Use tables (with `.table-scroll` wrapper) for comparisons and specifications
   - Every diagram and table must have a caption

4. **References:**
   - 3-5 further-reading links per chapter with brief descriptions
   - Master bibliography at the end with all sources cited

### Step 6: Self-Review

Before saving, verify:

- [ ] All sidebar nav links have matching `id` attributes in the content
- [ ] All chapters have a `chapter-summary` box with key question and reading time
- [ ] Callout types are varied (not all one kind) — use at least 3 different types
- [ ] Code blocks are wrapped in `.code-block` divs with `.code-header` language labels
- [ ] Tables are wrapped in `.table-scroll` containers
- [ ] Diagrams use CSS variables (not hardcoded colors) for theme compatibility
- [ ] Prev/next chapter navigation `data-target` attributes point to correct chapter IDs
- [ ] First chapter prev button is `disabled`, last chapter next button is `disabled`
- [ ] The HTML is well-formed (all tags closed, proper nesting)
- [ ] Reading times are calculated from actual word counts (words / 200, rounded)

### Step 7: Save and Report

Write the file to the current project directory.

Report to the user:
- File path
- Total word count
- Number of chapters
- Estimated total reading time
- Number of diagrams, tables, code blocks, and callout boxes used

## Theme Quick Reference

The template includes three complete theme palettes. All content uses CSS variables — never hardcode colors.

| Token | Light | Dark | Book |
|-------|-------|------|------|
| `--bg` | `#ffffff` | `#1a1b1e` | `#f5f0e8` |
| `--text` | `#2d2d2d` | `#d4d4d8` | `#3d3229` |
| `--accent` | `#2563eb` | `#60a5fa` | `#8b5e3c` |
| `--sidebar-bg` | `#1e293b` | `#111318` | `#2c1e12` |
| Body font | System sans-serif | System sans-serif | Georgia, serif |

Book mode switches to serif fonts for a printed-textbook feel.

## Content Components Quick Reference

| Component | CSS Class | When to Use |
|-----------|-----------|-------------|
| Concept callout | `.callout.concept` | Definitions, mental models |
| Warning callout | `.callout.warning` | Pitfalls, common mistakes |
| Tip callout | `.callout.tip` | Practical advice, shortcuts |
| Key Takeaway | `.callout.key-takeaway` | End-of-section summaries |
| Deep Dive | `.callout.deep-dive` | Collapsible advanced content |
| Prerequisite | `.callout.prerequisite` | Required prior knowledge |
| Block diagram | `.diagram-row` + `.d-block` | Architecture, data flow |
| Inline SVG | `<svg>` in `.figure` | Flowcharts, state machines |
| Table | `.table-scroll` wrapper | Comparisons, specifications |
| Code block | `.code-block` + `.code-header` | Implementation examples |
| Chapter summary | `.chapter-summary` | After every chapter heading |
| References | `.references` | Per-chapter further reading |
| Bibliography | `.bibliography-list` | Master source list at end |

See `references/content-guide.md` for full HTML snippets of each component.

## Reference Files

- **`references/template.html`** — Complete HTML skeleton. Read before generating any content. Contains all CSS (three theme palettes via `[data-theme]` attributes), all JavaScript (theme toggle, scroll spy, progress bar, font size controls, code copy, bookmarks, drawer nav, keyboard nav, back-to-top, chapter navigation, diagram lightbox), and placeholder content markers. Replace `{{PLACEHOLDERS}}` with generated content.

- **`references/content-guide.md`** — Component catalog with copy-paste HTML snippets for every content type. Organized by component. Read before writing chapter content.

## Customization

- To add a new callout type: define CSS variables in all three `[data-theme]` blocks, add the `.callout.{name}` CSS rule
- To change theme colors: modify CSS custom property values in the three `[data-theme]` attribute blocks
- All JS is vanilla — no framework, no transpilation needed
- Features degrade gracefully if JS is disabled (content remains fully readable)
