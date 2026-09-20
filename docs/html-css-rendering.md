# HTML/CSS CV Rendering System

A reusable design and implementation protocol for turning a role-specific CV into an editable, browser-previewable, print-ready PDF without inheriting the limitations of Word-style document layout.

The goal is not to make a CV look like a website for its own sake. The goal is to use web-layout tools — semantic HTML, CSS grid/flexbox, design tokens, component grammar and browser rendering — to create a CV that is visually coherent, structurally clear, ATS-readable and easy to iterate.

## 1. Core principle

Treat the CV as a **small information interface**, not a formatted text document.

Use this pipeline:

```text
verified evidence
    ↓
role / vacancy analysis
    ↓
targeted content
    ↓
company / role presentation overlay
    ↓
semantic HTML
    ↓
CSS design system
    ↓
browser preview
    ↓
A4 PDF render
    ↓
visual + semantic quality gate
```

Content decides **what deserves to exist**.

Information architecture decides **what deserves attention first**.

HTML defines **meaning and reading order**.

CSS defines **visual hierarchy and composition**.

The browser is the layout engine.

The PDF is the submission artifact.

## 2. Separation of concerns

Keep these layers conceptually separate:

### Evidence layer
The factual source of truth. Employers, projects, dates, qualifications, responsibilities, tools, results and verified claims.

### Targeting layer
What this specific role cares about. Employer vocabulary, role criteria, strongest candidate evidence, gaps and hiring thesis.

### Content layer
The selected copy for this one application. It should already be truthful, concise and role-specific before layout begins.

### Presentation overlay
A small role/company-specific instruction set describing what should visually dominate, what can be quiet, and which semantic vocabulary should be visible.

Principle:

> **Semantic mirroring > brand mimicry.**

Use the employer's mental model and vocabulary where truthful. Do not imitate logos, colors or visual branding so closely that the CV looks like a fake internal document.

### Rendering layer
Semantic HTML + CSS + browser-to-PDF generation.

The rendering layer should not invent claims or silently rewrite the candidate's evidence.

## 3. Recommended file architecture

```text
cv/
├── content.md                  # human-readable targeted content
├── overlay.md                  # role/company presentation priorities
├── index.html                  # semantic structure / template
├── styles.css                  # design system and print rules
├── render.py                   # deterministic browser → PDF renderer
├── README.md                   # edit/render instructions
└── output.pdf                  # generated submission artifact
```

For a more automated system, eliminate duplicated copy by generating `index.html` from structured Markdown, YAML, JSON or another data source.

Ideal long-term architecture:

```text
content.md + overlay.md + template.html + styles.css
                    ↓
                 renderer
                    ↓
                  PDF
```

## 4. Semantic HTML first

The DOM should remain understandable even if all CSS is removed.

Recommended reading order:

```text
identity / role
contact information
primary fit signals
profile / hiring thesis
core expertise
primary evidence
projects / systems
supporting evidence
education / formal context
languages
```

Use real semantic elements where practical:

- `<main>` for the CV
- `<header>` for identity
- `<section>` for major conceptual blocks
- `<article>` for repeated evidence components
- `<h1>`, `<h2>`, `<h3>` for hierarchy
- `<p>` for prose
- `<dl>` for label/value information such as languages

Do not use layout order to change semantic reading order unless there is a very strong reason.

Key rule:

> **Visual grid ≠ parsing order.**

The DOM can remain linear and ATS-friendly while CSS Grid or Flexbox controls visual composition.

## 5. A4 page geometry

Use the browser as a fixed print canvas.

```css
@page {
  size: A4;
  margin: 0;
}

html,
body {
  width: 210mm;
  min-height: 297mm;
}

.page {
  width: 210mm;
  min-height: 297mm;
  padding: 12mm 14mm 10mm;
}
```

Use physical units (`mm`) for page geometry and print spacing when precision matters.

Use points (`pt`) for print typography where convenient.

The browser preview may add outer padding or a shadow, but print rules should remove presentation that is not part of the final CV.

## 6. Design tokens before scattered CSS

Define the system at the top of the stylesheet.

Example:

```css
:root {
  --ink: #111827;
  --muted: #5d6676;
  --soft: #f5f7fa;
  --line: #d9dee7;
  --accent: #3157d5;

  --page-x: 14mm;
  --page-top: 12mm;
  --page-bottom: 10mm;
  --section-gap: 4mm;
  --radius: 2mm;
}
```

Tokens make the design editable as a system rather than as dozens of unrelated values.

Useful token categories:

- page margins
- section spacing
- component gaps
- primary / muted text
- divider colors
- one restrained accent
- border radius
- type scale

When a design feels wrong, adjust tokens and component rules before making one-off patches.

## 7. Build a component grammar

A coherent CV should not feel like every section was invented separately.

Define a small set of repeated visual components, for example:

- hero / identity block
- signal strip
- profile panel
- section heading
- expertise item
- evidence case
- project card
- supporting evidence item
- footer / formal context block

Repeated components should share:

- alignment
- padding logic
- heading hierarchy
- border treatment
- metadata treatment
- spacing rhythm

This is what makes the page feel architecturally sound rather than "formatted."

## 8. Use grids deliberately

CSS Grid is useful for information that belongs to the same conceptual level.

Good examples:

```css
.expertise-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.case-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2.4mm;
}

.project-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2.4mm;
}
```

Do not force symmetry when the content hierarchy is genuinely asymmetric.

But when three items have equal conceptual weight, their visual geometry should usually communicate that equality.

Avoid accidental asymmetry caused only by inconsistent margins, arbitrary widths, mismatched padding or unrelated component rules.

## 9. Hierarchy before density

A one-page CV is not successful merely because everything fits.

The page should answer these questions quickly:

1. Who is this person?
2. What role are they relevant to?
3. Why should I keep reading?
4. What is the strongest evidence?
5. What supporting context makes the claim credible?

Use visual weight accordingly.

Primary evidence may use:

- more whitespace
- larger component area
- stronger heading contrast
- a subtle background
- a restrained accent border

Secondary evidence should become quieter rather than competing for equal attention.

## 10. Density rule: do not shrink first

If the page overflows or feels cognitively heavy, use this order:

```text
1. remove duplicated concepts
2. shorten low-value wording
3. merge redundant bullets
4. demote secondary information
5. rebalance component widths
6. reduce unnecessary spacing
7. only then consider slightly smaller type
```

Do not solve every overflow problem by making the font tiny.

A technically one-page CV that hurts to read is a failed one-page CV.

## 11. Typography and line length

Use a restrained type system.

A practical hierarchy might include:

- name / hero
- role / eyebrow
- section heading
- component heading
- body copy
- metadata / labels

Do not create a different font size for every individual element.

Keep body text comfortably readable in the actual PDF, not just at 150% browser zoom.

Control line length with grids or max-width rather than allowing every paragraph to span the full page.

Shorter line lengths improve scanning and make dense information feel calmer.

## 12. Color and visual restraint

Use color as hierarchy, not decoration.

A robust baseline:

- near-black primary text
- gray secondary text
- light neutral surfaces
- one accent color
- thin neutral separators

No critical information should exist only through color.

Avoid decorative complexity that competes with the candidate's evidence.

## 13. Role/company alignment

The CV should feel purpose-built for the role without becoming cosplay.

Use the employer's language when the candidate genuinely matches it.

Examples of useful semantic alignment:

- their terminology for evaluation criteria
- their wording for quality dimensions
- their distinction between research, QA, implementation or support
- their preferred nouns for outputs and responsibilities

Then reflect those categories in:

- headings
- expertise labels
- evidence ordering
- project descriptions
- metadata

Do not copy marketing slogans or mimic proprietary visual branding.

The desired reaction is:

> "This person already thinks about the work in categories we recognize."

## 14. ATS and machine readability

A visually designed CV can still remain machine-readable.

Preserve:

- real text, never a flattened image
- logical DOM order
- semantic headings
- standard contact text
- selectable PDF text
- ordinary Unicode punctuation where possible
- no essential content inside decorative pseudo-elements
- no canvas-rendered body copy

Avoid relying on:

- icons without text labels
- absolute positioning for the entire document
- visually reordered fragments that produce nonsensical extraction order
- embedded images of text
- complex overlapping layers

After rendering, verify that text can be selected and copied in a sensible order.

## 15. Browser preview and print should be related, not identical

Screen mode can make editing pleasant:

```css
@media screen {
  body {
    padding: 14mm 0;
    background: #e9edf3;
  }

  .page {
    box-shadow: 0 20px 60px rgba(17, 24, 39, 0.14);
  }
}
```

Print mode should remove editor-preview effects:

```css
@media print {
  html,
  body {
    width: 210mm;
    height: 297mm;
    background: #fff;
  }

  .page {
    box-shadow: none;
  }

  * {
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }
}
```

The browser becomes the live visual editor; the print stylesheet makes the same source deterministic for PDF.

## 16. Deterministic browser-to-PDF rendering

A simple Playwright renderer is enough:

```python
from pathlib import Path
from playwright.sync_api import sync_playwright

ROOT = Path(__file__).resolve().parent
HTML = ROOT / "index.html"
CSS = ROOT / "styles.css"
PDF = ROOT / "output.pdf"

html = HTML.read_text(encoding="utf-8")
css = CSS.read_text(encoding="utf-8")
html = html.replace(
    '<link rel="stylesheet" href="styles.css" />',
    f'<style>{css}</style>'
)

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    page = browser.new_page(viewport={"width": 1240, "height": 1754})
    page.set_content(html, wait_until="load")
    page.emulate_media(media="print")
    page.pdf(
        path=str(PDF),
        format="A4",
        print_background=True,
        margin={"top":"0mm", "right":"0mm", "bottom":"0mm", "left":"0mm"},
        prefer_css_page_size=True,
    )
    browser.close()
```

Use a fixed browser/runtime in automated workflows when reproducibility matters.

## 17. Visual quality gate

Always inspect the actual rendered PDF or page image.

Do not trust HTML source alone.

Check:

### First-glance hierarchy
- Is the professional identity obvious?
- Does the strongest evidence dominate?
- Is there one clear visual path through the page?

### Geometry
- Are repeated components aligned?
- Are columns intentionally balanced?
- Are gaps and margins consistent?
- Is any asymmetry intentional rather than accidental?

### Cognitive load
- Are there keyword walls?
- Are several sections competing with the same visual weight?
- Are line lengths comfortable?
- Does the eye get clear resting points?

### Print integrity
- exactly one page when one page is intended
- no clipping
- no orphaned headings
- no unexpected browser margins
- colors and dividers render correctly

### Text integrity
- selectable text
- sensible copy/paste order
- no missing characters
- no hidden overflow

## 18. Content-design feedback loop

Layout is allowed to reveal content problems.

If a section is visually painful, ask whether the cause is:

- too many concepts
- repeated claims
- weak grouping
- poor hierarchy
- excessive labels
- overly long evidence

Do not assume the solution is purely CSS.

Likewise, do not rewrite good content merely because the current component is badly designed.

The workflow is iterative:

```text
content → render → inspect → diagnose → adjust content or design → render again
```

## 19. Editing model

A user should be able to inspect and modify the system without specialized software.

Minimum editability:

- open `index.html` in a browser
- edit visible copy in a text editor
- edit global design values in `:root`
- refresh browser
- render PDF when satisfied

For non-technical users, a future layer can generate the HTML from Markdown automatically so copy changes never require editing markup.

## 20. Automation protocol

For future CV generation, an AI or script should follow this sequence:

```text
1. load verified evidence
2. analyze vacancy
3. build criterion → evidence mapping
4. select and write targeted content
5. load company/role presentation overlay
6. assign each content block a semantic component
7. generate semantic HTML in reading order
8. apply shared CSS design system
9. render browser preview + PDF
10. inspect visual result
11. test text extraction / reading order
12. fix hierarchy or density problems
13. obtain human approval
14. submit
```

The renderer should be downstream of the evidence system, not a replacement for it.

## 21. Anti-patterns

Avoid:

- designing in Word first and converting the result to HTML
- shrinking everything until it fits
- decorative skill bars
- progress circles for subjective proficiency
- multiple accent colors with no semantic purpose
- brand imitation
- random cards around every paragraph
- inconsistent component padding
- arbitrary asymmetry
- giant keyword clouds
- content duplicated only to fill visual space
- image-based CV output
- hiding important information in headers/footers that parsers may ignore
- allowing the visual layer to invent stronger claims than the evidence supports

## 22. Definition of done

A strong HTML/CSS CV should satisfy all of these simultaneously:

### Human
It feels calm, deliberate and easy to scan.

### Employer
The most relevant evidence appears early and speaks the role's language.

### Candidate
Every claim is defensible and the page feels like an accurate representation of the person.

### Machine
Text remains semantic, selectable and logically ordered.

### Designer
Spacing, alignment, hierarchy and component relationships appear intentional.

### Engineer
The output can be reproduced from source and changed without rebuilding the document manually.

---

## Compact instruction for an AI

> Build the CV as a semantic A4 web page, not as a Word-style document. Preserve a linear ATS-readable DOM, then use CSS Grid/Flexbox, design tokens and a small repeatable component system to create strong hierarchy and visual balance. Primary role evidence should dominate; supporting evidence should become progressively quieter. Use employer vocabulary semantically where truthful, but do not mimic branding. Render through Chromium to a selectable one-page PDF, inspect the rendered page visually, and fix duplicated content or hierarchy problems before reducing font size. Keep content/evidence, company overlay, HTML structure and CSS presentation logically separate so the system can be reused for future applications.
