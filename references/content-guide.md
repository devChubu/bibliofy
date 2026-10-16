# Learning Guide — Content Components Reference

Every content component available in the learning guide template. When generating guide content, use these exact HTML patterns — the template CSS expects these class names and nesting structures.

All colors are theme-aware via CSS variables. Never hardcode hex colors in content HTML.

---

## Chapter Structure

Every chapter must follow this structure. The `id` on `h2` must match the sidebar nav link.

```html
<!-- ═══════════════════════════════════════════ -->
<!-- PART N: PART TITLE                          -->
<!-- ═══════════════════════════════════════════ -->

<h2 id="ch1"><span class="ch-num">Chapter 1</span> Chapter Title</h2>
<div class="chapter-summary">
  <div class="reading-time">~8 min read</div>
  <strong>Key question:</strong> What central question does this chapter answer?
</div>

<!-- Section content (h3, h4, paragraphs, callouts, diagrams, code, tables) -->

<div class="references">
  <h4>Further Reading</h4>
  <ul>
    <li><a href="https://example.com" target="_blank" rel="noopener">Resource Title</a> &mdash; Brief description of what it covers.</li>
    <li><a href="https://example.com" target="_blank" rel="noopener">Another Resource</a> &mdash; Why this is useful.</li>
  </ul>
</div>

<div class="chapter-nav">
  <button class="chapter-prev" data-target="ch0" disabled>&larr; Previous</button>
  <button class="chapter-next" data-target="ch2">Next: Next Chapter Title &rarr;</button>
</div>
```

**ID conventions:**
- Chapter heading: `id="chN"` (e.g., `ch1`, `ch12`)
- Section: `id="chN-slug"` (e.g., `ch7-rs485`, `ch3-memory-map`)
- Subsection: `id="chN-sub-slug"` (e.g., `ch4-link-script`)

**Reading time:** Calculate as `word_count / 200` rounded to nearest minute.

---

## Section Headings

```html
<h3 id="ch1-section-slug">Section Title</h3>
<h4 id="ch1-subsection-slug">Subsection Title</h4>
```

`h3` renders in accent color. `h4` renders in body text color with bold weight.

---

## Sidebar Navigation

Build this from the outline. Part labels are uppercase dividers. Chapter links get class `ch` for bold weight. Section links are plain `<a>` tags.

```html
<div class="part-label">Part I &mdash; Part Title</div>
<a href="#ch1" class="ch">1. First Chapter Title</a>
<a href="#ch1-section1">Section One Title</a>
<a href="#ch1-section2">Section Two Title</a>

<div class="part-label">Part II &mdash; Part Title</div>
<a href="#ch2" class="ch">2. Second Chapter Title</a>
<a href="#ch2-section1">Section One Title</a>
```

---

## Callout: Concept

For definitions, mental models, and key ideas the reader must understand.

```html
<div class="callout concept">
  <div class="callout-title">Key Concept: Name</div>
  <p>Explanation of the concept. Can include multiple paragraphs.</p>
</div>
```

---

## Callout: Warning

For pitfalls, common mistakes, gotchas, and things that can go wrong.

```html
<div class="callout warning">
  <div class="callout-title">Common Pitfall</div>
  <p>Description of what to watch out for and why it matters.</p>
</div>
```

---

## Callout: Tip

For practical advice, best practices, and shortcuts.

```html
<div class="callout tip">
  <div class="callout-title">Practical Tip</div>
  <p>Actionable advice the reader can apply immediately.</p>
</div>
```

---

## Callout: Key Takeaway

For end-of-section or end-of-chapter summaries. Use to reinforce the most important points.

```html
<div class="callout key-takeaway">
  <div class="callout-title">Key Takeaway</div>
  <p>The one thing the reader should remember from this section.</p>
</div>
```

---

## Callout: Deep Dive (Collapsible)

For optional advanced content. Hidden by default — reader clicks to expand. Use when content is valuable but would break the flow of the main narrative.

```html
<div class="callout deep-dive">
  <details>
    <summary class="callout-title">Deep Dive: Advanced Topic Title</summary>
    <div class="deep-dive-content">
      <p>Detailed explanation that goes deeper into the topic.</p>
      <p>Can include code, lists, even sub-diagrams.</p>
    </div>
  </details>
</div>
```

---

## Callout: Prerequisite

For stating assumed knowledge before a section. Link back to earlier chapters when possible.

```html
<div class="callout prerequisite">
  <div class="callout-title">Prerequisite</div>
  <p>This section assumes you understand topic X. See <a href="#ch3">Chapter 3</a> for background.</p>
</div>
```

---

## Code Block (with language label)

The copy button is injected automatically by JavaScript. Always wrap in `.code-block` div.

```html
<div class="code-block">
  <div class="code-header"><span class="code-lang">python</span></div>
  <pre><code>def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)</code></pre>
</div>
```

Common language labels: `python`, `c`, `javascript`, `bash`, `json`, `yaml`, `html`, `css`, `sql`, `rust`, `go`, `text`

Optional caption below:

```html
<div class="code-caption">A recursive implementation — simple but exponential time complexity.</div>
```

---

## Table (with scroll wrapper)

Always wrap tables in `.table-scroll` for mobile horizontal scrolling.

```html
<div class="table-scroll">
  <table>
    <thead>
      <tr>
        <th>Parameter</th>
        <th>Value</th>
        <th>Unit</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>V<sub>TH</sub></td>
        <td>2.93</td>
        <td>V</td>
        <td>Threshold voltage</td>
      </tr>
      <tr>
        <td>t<sub>RST</sub></td>
        <td>140</td>
        <td>ms</td>
        <td>Reset timeout period</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## Block Diagram (CSS Flexbox)

For architecture overviews, data flow, pipelines, and signal chains. Uses styled divs, no images needed.

### Single-row linear flow:

```html
<div class="figure">
  <div class="diagram-row">
    <div class="d-block d-gray">Input<small>Source data</small></div>
    <div class="d-arrow">&rarr;</div>
    <div class="d-block d-blue">Process<small>Transform</small></div>
    <div class="d-arrow">&rarr;</div>
    <div class="d-block d-blue">Validate<small>Check rules</small></div>
    <div class="d-arrow">&rarr;</div>
    <div class="d-block d-green">Output<small>Result</small></div>
  </div>
  <div class="figure-caption">Figure 1.1 &mdash; Data processing pipeline.</div>
</div>
```

### Multi-row (stacked layers):

```html
<div class="figure">
  <div class="diagram-row">
    <div class="d-block d-blue">Application Layer</div>
    <div class="d-block d-blue">Business Logic</div>
    <div class="d-block d-blue">API Gateway</div>
  </div>
  <div class="diagram-row">
    <div class="d-block d-navy">Database</div>
    <div class="d-block d-navy">Cache</div>
    <div class="d-block d-navy">Message Queue</div>
  </div>
  <div class="diagram-row">
    <div class="d-block d-gray">Infrastructure</div>
  </div>
  <div class="figure-caption">Figure 2.3 &mdash; Three-tier architecture.</div>
</div>
```

**Available block colors:**

| Class | Semantic use |
|-------|-------------|
| `.d-blue` | Primary elements, main components |
| `.d-navy` | Secondary/infrastructure elements |
| `.d-green` | Output, success, result |
| `.d-orange` | Warning, attention, critical path |
| `.d-purple` | Special, highlight, external |
| `.d-gray` | Input, neutral, background |

---

## Inline SVG Diagram

For flowcharts, state machines, and complex diagrams that need arbitrary layout. SVGs must use CSS variables for theme compatibility.

```html
<div class="figure">
  <svg viewBox="0 0 500 200" xmlns="http://www.w3.org/2000/svg">
    <!-- Nodes -->
    <rect x="10" y="70" width="120" height="50" rx="8"
          fill="var(--accent-light)" stroke="var(--accent)" stroke-width="2"/>
    <text x="70" y="100" text-anchor="middle"
          fill="var(--text)" font-size="13">Start</text>

    <rect x="190" y="70" width="120" height="50" rx="8"
          fill="var(--accent-light)" stroke="var(--accent)" stroke-width="2"/>
    <text x="250" y="100" text-anchor="middle"
          fill="var(--text)" font-size="13">Process</text>

    <rect x="370" y="70" width="120" height="50" rx="8"
          fill="var(--callout-tip-bg)" stroke="var(--callout-tip-border)" stroke-width="2"/>
    <text x="430" y="100" text-anchor="middle"
          fill="var(--text)" font-size="13">Done</text>

    <!-- Arrows -->
    <line x1="130" y1="95" x2="190" y2="95"
          stroke="var(--accent)" stroke-width="2" marker-end="url(#arrowhead)"/>
    <line x1="310" y1="95" x2="370" y2="95"
          stroke="var(--accent)" stroke-width="2" marker-end="url(#arrowhead)"/>

    <!-- Arrowhead marker -->
    <defs>
      <marker id="arrowhead" markerWidth="10" markerHeight="7"
              refX="10" refY="3.5" orient="auto">
        <polygon points="0 0, 10 3.5, 0 7" fill="var(--accent)"/>
      </marker>
    </defs>
  </svg>
  <div class="figure-caption">Figure 3.1 &mdash; Simple state transition.</div>
</div>
```

**Rules for SVGs:**
- Use `var(--accent)`, `var(--text)`, `var(--bg)`, `var(--accent-light)` for fills and strokes
- Use `var(--callout-*-border)` and `var(--callout-*-bg)` for semantic coloring
- Set `font-family` via `var(--font-heading)` or leave unset (CSS rule handles it)
- Keep `viewBox` reasonable (400-800px wide). The CSS makes SVGs responsive.
- For decision diamonds, use `<polygon>` or rotated `<rect>`

---

## References (Per-Chapter)

Place at the end of each chapter, before the chapter-nav buttons.

```html
<div class="references">
  <h4>Further Reading</h4>
  <ul>
    <li><a href="https://example.com/docs" target="_blank" rel="noopener">Official Documentation</a> &mdash; Complete API reference and tutorials.</li>
    <li><a href="https://example.com/paper" target="_blank" rel="noopener">Research Paper Title (Author, Year)</a> &mdash; Foundational theory behind this approach.</li>
    <li><a href="https://example.com/tutorial" target="_blank" rel="noopener">Hands-On Tutorial</a> &mdash; Step-by-step walkthrough with examples.</li>
  </ul>
</div>
```

Aim for 3-5 references per chapter. Prioritize: official docs, authoritative tutorials, foundational papers, and practical guides.

---

## Master Bibliography

Place at the end of the document after all chapters.

```html
<h2 id="bibliography">Bibliography</h2>
<ol class="bibliography-list">
  <li id="ref-1"><a href="URL" target="_blank" rel="noopener">Author (Year). <em>Title</em>. Publisher.</a> Brief description of relevance.</li>
  <li id="ref-2"><a href="URL" target="_blank" rel="noopener">Author (Year). <em>Title</em>.</a> Brief description.</li>
</ol>
```

---

## Cover Banner

The opening section of the guide. One per document.

```html
<div class="cover-banner">
  <h1>Guide Title</h1>
  <div class="subtitle">A one-to-three sentence description of what this guide covers and who it's for.</div>
</div>
```

---

## Content Writing Guidelines

**Prose style:**
- Write in clear, direct language. Explain concepts as if teaching a colleague.
- Start each chapter with the "why" before the "how."
- Use analogies to bridge unfamiliar concepts to familiar ones.
- Keep paragraphs short (3-5 sentences max).

**Visual density:**
- Aim for at least one visual element (diagram, table, code block, or callout) per 2-3 paragraphs of prose.
- Use block diagrams for architecture/flow, tables for comparisons/specs, code blocks for implementations.
- Every diagram and table needs a caption.

**Callout usage guidelines:**
- **Concept**: Use when introducing a new idea that has a specific name or definition.
- **Warning**: Use when something could silently go wrong or is commonly misunderstood.
- **Tip**: Use for practical shortcuts, tools, or techniques the reader can use immediately.
- **Key Takeaway**: Use at most once per section. Summarize the single most important point.
- **Deep Dive**: Use for advanced material that beginners can skip. Should be self-contained.
- **Prerequisite**: Use at the start of a section, not mid-flow. Always link back.

**Hierarchy:**
- Parts group related chapters (3-5 chapters per part)
- Chapters are self-contained topics (~1500-3000 words each)
- Sections (h3) break a chapter into logical steps or sub-topics
- Subsections (h4) are optional, for fine-grained breakdown within a section

**Lists:**
- Use unordered lists for sets of items with no inherent order
- Use ordered lists for sequences, steps, or ranked items
- Nest at most 2 levels deep
- Keep list items concise (1-2 sentences)

**Code examples:**
- Always include a language label in the `.code-header`
- Keep examples minimal — show the concept, not production code
- Use the `.code-caption` for brief explanation when the code isn't self-evident
