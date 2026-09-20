---
name: beautiful-html
description: Turn any document, proposal, report, or outline into a stunning single-file HTML presentation using 34 pre-built professional templates. Use when the user wants to convert a markdown file, proposal, or written content into a beautiful visual HTML document ready to share or convert to PDF.
---

# Beautiful HTML

Convert any written content into a polished, single-file HTML presentation using templates from the [beautiful-html-templates](https://github.com/zarazhangrui/beautiful-html-templates) library.

## Template Library

34 templates available. Key ones by use case:

| Use Case | Template | Style | Raw URL |
|---|---|---|---|
| B2B proposals, consulting, investor updates | `blue-professional` | Light / Cobalt Blue | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/blue-professional/template.html |
| Investor decks, board presentations | `signal` | Mixed / Navy & Gold | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/signal/template.html |
| White papers, research, policy briefs | `monochrome` | Light / Black & White | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/monochrome/template.html |
| Investment theses, research reports | `cartesian` | Light / Neutral Warm | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/cartesian/template.html |
| Leadership presentations, strategy | `emerald-editorial` | Mixed / Emerald-Navy | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/emerald-editorial/template.html |
| Research findings, white papers | `vellum` | Dark / Navy & Gold | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/vellum/template.html |
| Design reports, studio annuals | `cobalt-grid` | Light / Cobalt Blue | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/cobalt-grid/template.html |
| Startup pitches, founder presentations | `raw-grid` | Light / Multi | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/raw-grid/template.html |
| Quarterly reviews, studio updates | `editorial-forest` | Mixed / Forest Green | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/editorial-forest/template.html |
| Design studios, creative agencies | `studio` | Dark / Electric Yellow | https://raw.githubusercontent.com/zarazhangrui/beautiful-html-templates/main/templates/studio/template.html |

Full list of all 34 templates at: https://github.com/zarazhangrui/beautiful-html-templates

---

## Workflow

### Phase 1 — Gather Content

Ask the user:
1. "What's the content? (paste text, share a .md file path, or describe what to build)"
2. "What's the purpose? (proposal, report, pitch, research, internal update)"
3. "Who's the audience?"

If a file path is provided, read it. If it's a markdown proposal, parse it into sections.

### Phase 2 — Template Selection

Based on the purpose and audience, recommend 3 templates from the library above with a one-line reason for each. Show them as options (A, B, C).

Wait for the user to pick one — or auto-select if they say "you choose."

### Phase 3 — Fetch Template

Download the raw template with `curl -sL <raw url> -o template.html`, then Read it. Note the slide layouts available, the CSS variable names, and the placeholder text patterns.

### Phase 4 — Populate Content

Map the content onto the template's slide layouts:

- Cover slide → title, subtitle, date
- Agenda slide → top-level sections
- Content slides → one section per slide; headline = the key insight, not the topic label
- Cards slide → parallel items (capabilities, terms, questions)
- Two-column slide → comparisons, before/after
- Timeline slide → phases, roadmaps, "what happens next"
- Closing slide → thank-you and contact

Keep bullets to 4–5 per slide. Never dump raw sections; rewrite as slide-friendly statements.

Replace only the placeholder text. Preserve all CSS, JS, navigation, and structure.

### Phase 5 — Save

Save the populated HTML with a descriptive filename. Offer to convert to PDF with Playwright or deploy for a shareable link.

---

## Debrief defaults (unattended mode)

When running inside the Debrief pipeline there is no user to ask. Skip Phases 1–3 and use these fixed choices:

- **Template:** `cartesian.html` in this folder (already downloaded). Read it, do not fetch.
- **Input:** the markdown file you were given. **Output:** the same path with `.html`.
- **Cover slide:** document title, "Module NN", course name from `CLAUDE.md`.
- **session-report.md** → agenda slide lists topics in order; one content, cards or two-column slide per topic; a timeline slide for announcements; closing slide.
- **quiz.md** → one two-column slide per question: question left, answer and timestamp right. Eight slides plus cover and closing.
- **flashcards.md** → cards slides, three terms per slide, term as card title and definition as body. Four slides plus cover and closing.
- Remove template slides you did not use (charts, team). Do not leave placeholder text anywhere.
- Do not offer PDF or deployment.

---

## Source

Skill from [hamzafarooq/claude-code-starter](https://github.com/hamzafarooq/claude-code-starter). Templates from [beautiful-html-templates](https://github.com/zarazhangrui/beautiful-html-templates) by Zara Zhang Rui, MIT licensed.