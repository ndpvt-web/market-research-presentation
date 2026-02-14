---
name: market-research-presentation
description: "End-to-end market research to institutional-grade presentation pipeline. Conducts live web research, applies consulting frameworks (TAM/SAM/SOM, Porter's Five Forces, SWOT, PESTEL, competitive positioning), synthesizes findings, and generates professional PPTX presentations with native charts, tables, framework diagrams, and data visualizations in JP Morgan / McKinsey style. Use when: (1) user asks to research a market, industry, or company AND produce a presentation, (2) user asks for a market analysis deck or pitch book, (3) user asks for competitive analysis presentation, (4) user says 'market research presentation' or 'research deck' or 'pitch book' or 'industry analysis presentation' or 'consulting deck', (5) user wants to analyze and present findings on any market/industry/company topic"
---

# Market Research to Presentation

Automated pipeline that researches any market, industry, or company topic using live web data and produces institutional-grade PPTX presentations with native charts, framework diagrams, and professional tables -- comparable to JP Morgan equity research or McKinsey consulting deliverables.

## Pipeline Overview

```
USER REQUEST --> Intake & Scoping --> Parallel Research --> Analysis & Frameworks --> Content Planning --> PPTX Assembly (Charts + Frameworks + Tables) --> Quality Check --> Output
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

### Optional: Browser Scraping (SEC filings, government data)

```bash
browser navigate [URL]
browser act "Click on the most recent filing"
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
- **Visualization method** (choose from decision tree below)
- Data for charts/tables (actual numbers from research)
- Source citation
- Speaker notes (2-3 sentences)

### Visualization Decision Tree

For each data slide, select the right rendering method:

```
Is it a standard chart type (bar, line, pie, scatter)?
  YES --> Use PptxGenJS native chart (see references/charts.md)
         These are real, editable PowerPoint charts.

Is it a framework diagram (SWOT, Porter's, TAM, positioning matrix)?
  YES --> Use PptxGenJS shapes + text (see references/framework-visuals.md)
         These are built from native PowerPoint shapes.

Is it a data comparison table?
  YES --> Use PptxGenJS table with institutional formatting
         (see references/charts.md "Tables" section)

Is it a complex custom visual (geographic heat map, detailed infographic)?
  YES --> Use AI image generation (generate-image skill) or
         SVG-to-PNG via Sharp, then insert as image.
         Only use this as a last resort.
```

**Prefer native PptxGenJS elements** (charts, shapes, tables) over images. Native elements are editable in PowerPoint and look sharper at any zoom level.

Save to `tmp/slide_plan.md`.

## Phase 5: Presentation Assembly

Use the `pptx` skill (html2pptx workflow) to generate the PPTX.

### Assembly Workflow

1. **Read prerequisites**: Read the pptx skill's `html2pptx.md` fully. Read [references/design-specs.md](references/design-specs.md) for the institutional design system. Read [references/charts.md](references/charts.md) for chart code patterns. Read [references/framework-visuals.md](references/framework-visuals.md) for framework diagram code.

2. **Create HTML slides**: For each slide, create an HTML file at 720pt x 405pt (16:9). Use `class="placeholder"` divs to reserve space for charts/frameworks that will be added via PptxGenJS.

3. **Write the master JS file**: A single JavaScript file that:
   - Creates the pptx instance with `LAYOUT_16x9`
   - Calls `html2pptx()` for each HTML slide
   - Adds charts to placeholder areas using PptxGenJS
   - Adds framework diagrams using shapes + text
   - Adds formatted tables
   - Saves the final .pptx

4. **Run and validate**: Execute the JS file, generate thumbnail grid, inspect visually, fix issues.

### Key Design Rules

- **Palette**: Navy `002D72` primary, Green `00875A` accent, Gold `B3995D` tertiary
- **Fonts**: Arial, 24pt titles, 18pt subtitles, 12pt body
- **Charts**: Data labels on, horizontal gridlines only (dashed CCCCCC), source bottom-left
- **Tables**: Navy header with white text, alternating white/F5F5F5 rows, right-aligned numbers
- **Content limits**: Max 6 bullets, max 8 words per bullet, one key message per slide
- **Colors in PptxGenJS**: NEVER use `#` prefix -- use `002D72` not `#002D72`

### Chart Integration Pattern

Every chart slide follows this pattern:

```html
<!-- slide-chart.html -->
<body style="width: 720pt; height: 405pt; margin: 0; display: flex;">
  <div style="margin: 20pt 30pt 0 30pt;">
    <h2 style="color: #002D72; font-size: 20pt;">Action title goes here</h2>
  </div>
  <div id="main-chart" class="placeholder" style="position: absolute; left: 30pt; top: 60pt; width: 660pt; height: 300pt;"></div>
  <p style="position: absolute; bottom: 10pt; left: 30pt; font-size: 8pt; color: #999999; font-style: italic;">Source: Bloomberg, 2024</p>
</body>
```

```javascript
// In master JS file:
const { slide, placeholders } = await html2pptx('slide-chart.html', pptx);
const chartArea = placeholders.find(p => p.id === 'main-chart');
slide.addChart(pptx.charts.BAR, chartData, {
  ...chartArea,
  chartColors: ['002D72', '00875A', 'B3995D'],
  showValue: true,
  valGridLine: { style: 'dash', color: 'CCCCCC' },
});
```

### Framework Diagram Integration

Framework slides use PptxGenJS shapes directly (no placeholder needed). After creating the HTML slide with title and source line, add shapes programmatically. See [references/framework-visuals.md](references/framework-visuals.md) for complete code for each framework type.

### AI Image Generation (last resort)

For visuals that cannot be built with PptxGenJS (geographic maps, complex infographics):

```bash
python3 ~/.claude/skills/generate-image/scripts/generate_image.py \
  "Professional infographic showing [description], clean white background, corporate style, no text" \
  --output tmp/custom_visual.png
```

Then insert into slide:
```javascript
slide.addImage({ path: 'tmp/custom_visual.png', x: 1, y: 1.5, w: 8, h: 4.5 });
```

Use sparingly -- native charts and shapes are preferred.

## Phase 6: Quality & Output

### Visual Validation

```bash
python ~/.claude/skills/pptx/scripts/thumbnail.py OUTPUT.pptx workspace/thumbnails --cols 4
```

Read the thumbnail image and check:
1. All charts render correctly with readable labels
2. Framework diagrams are properly aligned
3. Tables have consistent formatting
4. No overlapping elements or text cutoff
5. Color palette is consistent across all slides
6. Source citations present on every data slide

### Narrative Validation

Read all slide titles in sequence. They must tell a coherent story:
```
Slide 2: "AI infrastructure is a $150B market growing at 35% CAGR"
Slide 4: "Training compute demand doubling annually, but inference will dominate by 2026"
Slide 6: "Three hyperscalers control 70% of the market..."
...must flow logically
```

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
