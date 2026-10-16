# Bibliofy — Learning Guide Skill for Claude Code

A Claude Code skill that generates interactive, self-contained HTML learning guides with three reading themes and rich content components.

## Features

- **Three reading themes** — Light (modern docs), Dark (night reading), Book (warm cream, serif fonts — like a real textbook)
- **Responsive design** — works on desktop, tablet, and phone
- **Sidebar navigation** with scroll spy
- **Reading progress bar**
- **Font size controls** (A-/A+)
- **Continue-reading bookmark** — picks up where you left off
- **6 callout types** — concept, warning, tip, key-takeaway, deep-dive (collapsible), prerequisite
- **CSS block diagrams** and inline SVG support
- **Code blocks** with copy-to-clipboard
- **Tables** with mobile horizontal scroll
- **Per-chapter references** + master bibliography
- **Keyboard navigation** (j/k keys)
- **Print-friendly layout**
- **Zero external dependencies** — single HTML file, works offline

## Preview

Three example guides are included in the `examples/` directory:

| Example | Audience |
|---------|----------|
| `embedded_firmware_interrupts_learning_guide.html` | Embedded Software |
| `buck_converter_fundamentals_learning_guide.html` | Power Electronics |
| `how_the_internet_works_learning_guide.html` | General Audience |

Open any example HTML file in your browser to see it in action.

## Installation

1. **Clone or download** this repo.

2. **Copy the skill folder** to your Claude Code skills directory:

   ```bash
   # macOS / Linux
   cp -r learning-guide ~/.claude/skills/learning-guide

   # Windows (PowerShell)
   Copy-Item -Recurse learning-guide "$env:USERPROFILE\.claude\skills\learning-guide"
   ```

3. **Register the skill** in your Claude Code settings. Add the path to `additionalDirectories` in `~/.claude/settings.json`:

   ```json
   {
     "permissions": {
       "additionalDirectories": [
         "/path/to/your/.claude/skills/learning-guide"
       ]
     }
   }
   ```

   Replace the path with your actual skill directory path.

4. **Restart Claude Code.** The skill will be available as `/bibliofy`.

## Usage

Invoke with `/bibliofy` from any project.

- **With a topic:** "create a learning guide about Docker containers"
- **From project context:** Claude reads CLAUDE.md and source files to generate a guide about the project

The skill generates a detailed outline first and waits for approval before writing content.

## File Structure

```
learning-guide/
├── SKILL.md                          # Skill definition & workflow
├── README.md
├── LICENSE
├── references/
│   ├── template.html                 # HTML/CSS/JS template skeleton
│   └── content-guide.md              # Component catalog & HTML snippets
└── examples/
    ├── embedded_firmware_interrupts_learning_guide.html
    ├── buck_converter_fundamentals_learning_guide.html
    └── how_the_internet_works_learning_guide.html
```

## Customization

- **Theme colors:** modify CSS variables in the three `[data-theme]` blocks in `references/template.html`
- **New callout types:** add CSS variables + `.callout.{name}` rule
- All JS is vanilla — no build step needed

## License

MIT License — see [LICENSE](LICENSE) file.
