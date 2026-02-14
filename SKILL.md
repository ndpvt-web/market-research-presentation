---
name: market-research-presentation
description: "End-to-end market research to institutional-grade PPTX pipeline with AI-generated imagery, 5 adaptive themes, and 12 layout types. Live web research, consulting frameworks (TAM/SAM/SOM, Porter's, SWOT, PESTEL), native charts, tables, framework diagrams, AI cover images, hero stats, KPI dashboards, icon grids -- rivaling Gamma and NotebookLM but with editable PowerPoint output. Use when: (1) user asks to research a market/industry/company AND produce a presentation, (2) user asks for a market analysis deck or pitch book, (3) user asks for competitive analysis presentation, (4) user says 'market research presentation' or 'research deck' or 'pitch book' or 'industry analysis presentation' or 'consulting deck', (5) user wants to analyze and present findings on any market/industry/company topic"
---

# Market Research to Presentation

Automated pipeline: researches any topic using live web data, applies consulting frameworks, generates institutional-grade PPTX with native charts, AI-generated imagery, adaptive themes, and 12 layout types. Outputs editable PowerPoint (not PDF) -- a key advantage over NotebookLM.

## Pipeline Overview

```
USER REQUEST --> Intake & Scoping (+ Theme Selection) --> Parallel Research --> Analysis & Frameworks --> Content Planning --> AI Image Generation --> PPTX Assembly --> Quality Check --> Output
```

Follow each phase completely before proceeding to the next.

## Phase 1: Intake & Scoping

Parse the user request and classify:

| Type | Example Triggers | Frameworks to Apply |
|---|---|---|
| **Industry Analysis** | "EV market", "SaaS industry" | TAM/SAM/SOM, Porter's, PESTEL, Competitive Landscape |
| **Company Deep-Dive** | "Analyze Rivian", "deep dive on Stripe" | SWOT, Financial Benchmarking, Competitive Positioning |
| **Competitive Analysis** | "Compare X vs Y vs Z" | Positioning Matrix, Peer Benchmarking, Feature Comparison |
| **Market Entry** | "Should we enter X market?" | TAM/SAM/SOM, Porter's, SWOT, Risk Assessment |
| **Investment Thesis** | "Is X a good investment?" | Financial Analysis, Valuation, Risk/Catalyst, Scenarios |

Generate internally:
- Presentation title
- Geographic and time scope
- 4-6 key research questions
- Which frameworks to apply (from table above)
- Target slide count: 15-25
- **Design theme** (select from design-specs.md theme table based on topic sector)

### Theme Selection

Read [references/design-specs.md](references/design-specs.md) for the full theme system. Map topic to theme:

| Sector | Theme |
|---|---|
| Finance / Banking / Investment | `wall-street` |
| Technology / SaaS / AI | `tech-forward` |
| Healthcare / Pharma / Biotech | `clinical` |
| Consumer / Retail / CPG | `vibrant` |
| Energy / Industrial / Infrastructure | `industrial` |
| General / Multi-sector | `wall-street` (default) |

The selected theme drives ALL visual decisions: colors, chart palettes, cover style, and HTML slide styling.

## Phase 2: Parallel Research

Run 8-12 web searches using the web-search skill. Execute searches for multiple streams simultaneously using the Task tool for parallelism.

```bash
python ~/.claude/skills/web-search/scripts/web_search.py "QUERY" --count 10
```

### Research Streams

**Stream A -- Market Sizing & Growth:**
- "[topic] market size 2024 2025"
- "[topic] market growth rate CAGR forecast"
- "[topic] TAM total addressable market"

**Stream B -- Competitive Landscape:**
- "[topic] market share leading companies"
- "[topic] top players competitive landscape"
- "[topic] recent M&A acquisitions funding"

**Stream C -- Industry Dynamics:**
- "[topic] industry trends disruption 2024 2025"
- "[topic] regulatory environment policy changes"
- "[topic] technology innovation drivers"

**Stream D -- Financial & Sentiment:**
- "[topic] revenue profitability financial performance"
- "[topic] investment outlook risks opportunities"

**Stream E (company-focused only):**
- "[company] revenue growth earnings"
- "[company] competitive advantages moat"
- "[company] SEC filing 10-K annual report"

For each finding, record: **Claim** (specific data point), **Source** (publication + date), **Confidence** (High/Medium/Low), **Data point** (number + units).

Save findings to `tmp/research_findings.md`.

### Optional: Reddit Sentiment (consumer-facing topics)

```bash
python3 ~/.claude/skills/reddit/scripts/search_posts.py "[topic]" --limit 20
```

## Phase 3: Analysis & Framework Application

Apply selected frameworks using collected data. Read [references/frameworks.md](references/frameworks.md) for detailed framework templates and scoring methodology.

### Critical Rules

1. **Real data only** -- populate frameworks with actual numbers from Phase 2
   - BAD: "Strength: Strong brand recognition"
   - GOOD: "Strength: #1 market share at 34% ($45B revenue), 2.5x nearest competitor"
2. **Cross-reference conflicts** -- when sources disagree, note both figures
3. **Flag gaps** -- write "Data not available" rather than fabricating numbers
4. **Calculate derived metrics** where data allows (CAGR, market share %, relative multiples)

Save analysis to `tmp/analysis.md`.

## Phase 4: Content Planning

Build slide-by-slide plan. Read [references/slide-structure.md](references/slide-structure.md) for the full presentation structure template.

### Action Title Rule (MANDATORY)

Every slide title MUST be a complete sentence stating a conclusion:

| BAD (Topic Title) | GOOD (Action Title) |
|---|---|
| Market Overview | Global EV market reached $500B in 2024, growing at 23% CAGR |
| Competitive Landscape | Top 3 players control 65% of market, but share is fragmenting |
| Risks | Supply chain concentration poses the highest near-term risk |

### Per-Slide Specification

For each slide, define:
- Action title (complete sentence)
- **Layout type** (choose from the 12 layouts in design-specs.md)
- **Visualization method** (choose from decision tree below)
- Data for charts/tables (actual numbers from research)
- Source citation
- Speaker notes (2-3 sentences)

### Layout Selection Guide

| Slide Purpose | Recommended Layout |
|---|---|
| Opening / cover | **Title Slide** (with AI hero image) |
| Key takeaway with one big number | **Hero Stat** |
| Market sizing bar/line chart | **Full Chart** or **Chart with Insight Panel** |
| Multiple KPIs at a glance | **KPI Dashboard** (3-4 cards) |
| Company comparison | **Comparison Table** |
| SWOT / Porter's / TAM circles | **Framework 2x2** or specialized framework |
| Section transition | **Section Divider** (with optional AI image) |
| Concept explanation with visual | **Split Image + Text** |
| Feature/driver enumeration | **Icon Grid / Feature Cards** |
| Final recommendations | **Verdict / Recommendation** |
| Data appendix | **Sources** (multi-column text) |

### Visualization Decision Tree

```
Is it a standard chart type (bar, line, pie, scatter)?
  YES --> Use PptxGenJS native chart (see references/charts.md)

Is it a framework diagram (SWOT, Porter's, TAM, positioning matrix)?
  YES --> Use PptxGenJS shapes + text (see references/framework-visuals.md)

Is it a data comparison table?
  YES --> Use PptxGenJS table with institutional formatting

Is it a single hero metric (TAM, market size, growth rate)?
  YES --> Use Hero Stat layout with oversized number

Is it 3-4 key metrics together?
  YES --> Use KPI Dashboard layout with card shapes

Is it a complex custom visual (geographic map, infographic)?
  YES --> Use AI image generation (generate-image skill)
```

**Prefer native PptxGenJS elements** (charts, shapes, tables) over images. Native elements are editable in PowerPoint.

Save to `tmp/slide_plan.md`.

## Phase 4.5: AI Image Generation

Generate AI images for cover slide, section dividers, and optional concept slides. Run in parallel using the Task tool.

```bash
python3 ~/.claude/skills/generate-image/scripts/generate_image.py \
  "Professional abstract [TOPIC] themed [STYLE KEYWORDS] background for corporate presentation slide, [COLOR MOOD] color palette, geometric patterns, clean composition, no text, no logos, 16:9 aspect ratio" \
  --output tmp/cover_image.png
```

See [references/design-specs.md](references/design-specs.md) "AI Image Generation Guidelines" section for theme-specific style keywords and integration patterns.

### Minimum Images to Generate
- **1 cover image** (always)
- **1-3 section divider images** (recommended for decks with 15+ slides)
- **0-2 concept images** (optional, for Split Image + Text layouts)

## Phase 5: Presentation Assembly

Use the `pptx` skill (html2pptx workflow) to generate the PPTX.

### Assembly Workflow

1. **Read prerequisites**: Read the pptx skill's `html2pptx.md` fully. Read [references/design-specs.md](references/design-specs.md) for the theme system and layout library. Read [references/charts.md](references/charts.md) for chart code patterns. Read [references/framework-visuals.md](references/framework-visuals.md) for framework diagram code.

2. **Define theme at top of JS file**: Copy the selected theme object from design-specs.md and reference `theme.primary`, `theme.accent`, etc. throughout.

3. **Create HTML slides**: For each slide, create an HTML file at 720pt x 405pt (16:9). Use the theme colors in CSS. Use `class="placeholder"` divs to reserve space for charts/frameworks that will be added via PptxGenJS.

4. **Write the master JS file**: A single JavaScript file that:
   - Defines the theme object
   - Creates the pptx instance with `LAYOUT_16x9`
   - Adds cover slide with AI image + dark overlay + white text
   - Calls `html2pptx()` for each content slide
   - Adds charts to placeholder areas using theme.chartColors
   - Adds framework diagrams using shapes + text with theme colors
   - Adds Hero Stat slides with oversized text
   - Adds KPI Dashboard cards with shapes
   - Adds section dividers with optional AI background images
   - Saves the final .pptx

5. **Run and validate**: Execute the JS file, generate thumbnail grid, inspect visually, fix issues.

### Key Design Rules

- **Theme-driven colors**: All colors come from the theme object -- NO hardcoded hex values
- **Fonts**: Arial, 24pt titles, 18pt subtitles, 12pt body
- **Charts**: Data labels on, horizontal gridlines only (dashed theme.lightGray), source bottom-left
- **Tables**: Theme primary header with white text, alternating white/theme.bgGray rows
- **Content limits**: Max 6 bullets, max 8 words per bullet, one key message per slide
- **Colors in PptxGenJS**: NEVER use `#` prefix -- use `002D72` not `#002D72`
- **Cover slide**: ALWAYS has AI-generated image with dark overlay for text readability

### Chart Integration Pattern

```javascript
// Theme object at top of file
const theme = THEMES['tech-forward']; // or whichever was selected

// Chart slide
const { slide, placeholders } = await html2pptx('slide-chart.html', pptx);
const chartArea = placeholders.find(p => p.id === 'main-chart');
slide.addChart(pptx.charts.BAR, chartData, {
  ...chartArea,
  chartColors: theme.chartColors,
  showValue: true,
  valGridLine: { style: 'dash', color: theme.lightGray },
});
```

### Cover Slide with AI Image

```javascript
const coverSlide = pptx.addSlide();
coverSlide.addImage({ path: 'tmp/cover_image.png', x: 0, y: 0, w: 13.33, h: 7.5 });
coverSlide.addShape(pptx.shapes.RECT, {
  x: 0, y: 0, w: 13.33, h: 7.5,
  fill: { color: theme.darkBg, transparency: 40 },
});
coverSlide.addText('Presentation Title', {
  x: 1, y: 2.2, w: 11, fontSize: 36, color: 'FFFFFF', bold: true, fontFace: 'Arial',
});
```

### Hero Stat Layout

```javascript
slide.addText('$394B', {
  x: 1.5, y: 1.8, w: 10, h: 2, fontSize: 64, color: theme.primary,
  bold: true, fontFace: 'Arial', align: 'center',
});
slide.addText('Projected AI Infrastructure Market by 2030', {
  x: 2, y: 3.8, w: 9, h: 0.6, fontSize: 14, color: theme.secondaryText,
  fontFace: 'Arial', align: 'center',
});
```

### KPI Dashboard Cards

```javascript
const cards = [
  { value: '$135.8B', label: 'Market Size 2024', trend: '+19.4% CAGR' },
  { value: '86%', label: 'NVIDIA Share', trend: 'Dominant' },
  { value: '$320B+', label: 'Hyperscaler Capex', trend: '+44% YoY' },
  { value: '3.2x', label: 'Demand/Supply', trend: 'Constrained' },
];
cards.forEach((card, i) => {
  const x = 0.5 + (i * 3.15);
  slide.addShape(pptx.shapes.ROUNDED_RECT, {
    x, y: 1.5, w: 2.9, h: 3.5,
    fill: { color: theme.bgGray }, rectRadius: 0.1,
  });
  slide.addText(card.value, {
    x, y: 1.8, w: 2.9, fontSize: 28, color: theme.primary,
    bold: true, align: 'center', fontFace: 'Arial',
  });
  slide.addText(card.label, {
    x, y: 3.0, w: 2.9, fontSize: 11, color: theme.bodyText,
    align: 'center', fontFace: 'Arial',
  });
  slide.addText(card.trend, {
    x, y: 3.8, w: 2.9, fontSize: 10, color: theme.accent,
    bold: true, align: 'center', fontFace: 'Arial',
  });
});
```

### Framework Diagram Integration

Framework slides use PptxGenJS shapes directly (no placeholder needed). After creating the HTML slide with title and source line, add shapes programmatically. See [references/framework-visuals.md](references/framework-visuals.md) for complete code -- replace hardcoded colors with theme variables.

## Phase 6: Quality & Output

### Visual Validation

```bash
python ~/.claude/skills/pptx/scripts/thumbnail.py OUTPUT.pptx workspace/thumbnails --cols 4
```

Read the thumbnail image and check:
1. Cover slide has visible AI image with readable white text overlay
2. All charts render correctly with readable labels
3. Theme colors are consistent across all slides
4. KPI cards and hero stats are properly sized and centered
5. Framework diagrams are properly aligned
6. Tables have consistent formatting
7. No overlapping elements or text cutoff
8. Source citations present on every data slide

### Narrative Validation

Read all slide titles in sequence. They must tell a coherent story.

### Output Bundle

**Always generate:**
```
outputs/[Topic]_Market_Research_Presentation.pptx
outputs/[Topic]_Research_Sources.md
```

**For 15+ slide decks, also generate:**
- `outputs/[Topic]_Data_Pack.xlsx` -- market data, competitive tables, financials (use xlsx skill)
- `outputs/[Topic]_Executive_Brief.pdf` -- 1-2 page summary (use pdf or latex-document skill)

## Quality Mandates

Non-negotiable for institutional-grade output:

1. Every data point has a source. No unattributed statistics.
2. Precise language: "Grew 23% YoY to $45B" not "grew significantly."
3. Wall Street chart conventions: $M/$B currency, 1-decimal percentages, years on x-axis.
4. Every slide passes the "So What?" test.
5. Frameworks use real data, not generic observations.
6. Data gaps flagged honestly, never filled with fiction.
7. Consistent units across all slides.
8. All charts are native PptxGenJS (editable in PowerPoint), not images.
9. Framework diagrams built from shapes (editable), not screenshots.
10. Cover slide has AI-generated hero image.
11. Theme colors are consistent throughout -- no hardcoded hex values.
12. At least 3 different layout types used across the deck for visual variety.
