# Institutional Design Specifications

Professional design standards for JP Morgan / McKinsey-quality presentations.

## Color Palette

### Primary Palette

| Role | Hex | RGB | Usage |
|---|---|---|---|
| **Navy** (Primary) | #002D72 | 0, 45, 114 | Slide titles, header bars, chart primary color |
| **Dark Navy** | #001A44 | 0, 26, 68 | Section divider backgrounds |
| **Green** (Accent) | #00875A | 0, 135, 90 | Positive metrics, secondary charts, highlights |
| **Gold** (Tertiary) | #B3995D | 179, 153, 93 | Accent lines, decorative elements, emphasis |
| **Red** (Negative) | #C8102E | 200, 16, 46 | Negative metrics, risk indicators, declines |

### Neutral Palette

| Role | Hex | Usage |
|---|---|---|
| **Body Text** | #333333 | All body copy, bullet text |
| **Secondary Text** | #666666 | Chart labels, footnotes |
| **Tertiary Text** | #999999 | Source citations, disclaimers |
| **Light Gray** | #CCCCCC | Gridlines, borders, dividers |
| **Background Gray** | #F5F5F5 | Subtle background fills, alternating rows |
| **White** | #FFFFFF | Primary slide background |

### Chart Color Sequence

When multiple data series needed, use in this order:
1. #002D72 (Navy)
2. #00875A (Green)
3. #B3995D (Gold)
4. #5B9BD5 (Light Blue)
5. #7F7F7F (Gray)
6. #C8102E (Red -- use sparingly, only for negative/warning)

## Typography

### Font Stack

Use web-safe fonts only (must render in PowerPoint without embedding):

| Element | Font | Size | Weight | Color |
|---|---|---|---|---|
| **Slide Title** | Arial | 24pt | Bold | #002D72 |
| **Subtitle** | Arial | 18pt | Regular | #333333 |
| **Body Text** | Arial | 12pt | Regular | #333333 |
| **Bullet Text** | Arial | 11-12pt | Regular | #333333 |
| **Chart Title** | Arial | 14pt | Bold | #333333 |
| **Chart Labels** | Arial | 10pt | Regular | #666666 |
| **Source Line** | Arial | 8pt | Italic | #999999 |
| **Table Header** | Arial | 11pt | Bold | #FFFFFF (on navy bg) |
| **Table Body** | Arial | 10pt | Regular | #333333 |
| **Section Number** | Arial | 72pt | Bold | #FFFFFF (on navy bg) |

### Typography Rules

- Left-align all text (except table numbers which right-align)
- Title case for slide titles
- No underlines (use bold for emphasis instead)
- Line spacing: 1.2x for body text, 1.0x for bullets
- Max 6 lines of bullets per slide
- Max 8 words per bullet point

## Slide Dimensions

- Standard: 13.33" x 7.5" (Widescreen 16:9)
- HTML equivalent: 960px x 540px or 720pt x 405pt

## Layout Specifications

### Title Slide
```
+------------------------------------------+
|                                          |
|                                          |
|        [TITLE - 36pt Bold Navy]          |
|        [Subtitle - 18pt Gray]            |
|                                          |
|        [Date | Author]                   |
|                                          |
|  ____________________________________    |
|  [Gold accent line 2pt]                  |
|  [Disclaimer - 8pt Gray]                 |
+------------------------------------------+
```

### Executive Summary
```
+------------------------------------------+
| [Action Title - 24pt Navy]               |
|------------------------------------------|
|  [Bullet 1 with key finding]    | KEY    |
|  [Bullet 2 with key finding]    | METRICS|
|  [Bullet 3 with key finding]    | TABLE  |
|  [Bullet 4 with key finding]    | (right)|
|  [Bullet 5 with key finding]    |        |
|                                          |
| Source: [citation - 8pt italic gray]     |
+------------------------------------------+
```

### Full Chart
```
+------------------------------------------+
| [Action Title - 24pt Navy]               |
|------------------------------------------|
|                                          |
|          [CHART - 80% of slide]          |
|          [Data labels visible]           |
|          [Gridlines: horiz only, dashed] |
|                                          |
| Source: [citation - 8pt italic gray]     |
+------------------------------------------+
```

### Chart with Text
```
+------------------------------------------+
| [Action Title - 24pt Navy]               |
|------------------------------------------|
|                        |                 |
|   [CHART - 60%]       | * Key point 1   |
|                        | * Key point 2   |
|                        | * Key point 3   |
|                        |                 |
| Source: [citation]                       |
+------------------------------------------+
```

### Comparison Table
```
+------------------------------------------+
| [Action Title - 24pt Navy]               |
|------------------------------------------|
| Company  | Rev($B) | Growth | Margin | PE |
|----------|---------|--------|--------|-----|
| [Navy header row with white text]        |
| [Alt row 1 - white bg]                   |
| [Alt row 2 - #F5F5F5 bg]                |
| [Alt row 3 - white bg]                   |
|                                          |
| Source: [citation]                       |
+------------------------------------------+
```

### Framework 2x2
```
+------------------------------------------+
| [Action Title - 24pt Navy]               |
|------------------------------------------|
|                  |                        |
|   [Quadrant 1]   |   [Quadrant 2]        |
|   Green bg 10%   |   Navy bg 10%         |
|                  |                        |
|  ----------------|--------------------    |
|                  |                        |
|   [Quadrant 3]   |   [Quadrant 4]        |
|   Gold bg 10%    |   Red bg 10%          |
|                  |                        |
| Source: [citation]                       |
+------------------------------------------+
```

### Section Divider
```
+------------------------------------------+
|                                          |
|  [Dark Navy Background]                  |
|                                          |
|     [Section Number - 72pt White]        |
|     [Section Title - 36pt White]         |
|     [Gold accent line]                   |
|                                          |
+------------------------------------------+
```

## Chart Formatting Standards

### Bar Charts
- Primary color for main series
- Horizontal gridlines only (dashed, #CCCCCC)
- Data labels: above bars, 10pt, #333333
- Axis labels: #666666
- No chart border
- Bar gap: 50-80% of bar width

### Line Charts
- 2pt line weight
- Markers: circle, size 6
- Primary + secondary colors for 2 series
- Smooth lines (no jagged)
- Data labels on endpoints or inflection points

### Pie Charts
- Max 6 segments (group small ones as "Other")
- Data labels: value + percentage
- Start at 12 o'clock position
- Slight explosion (2-3%) for key segment
- No 3D effects

### Tables
- Header row: Navy background (#002D72), white text, bold
- Body: Alternating white / #F5F5F5 rows
- Numbers: right-aligned, consistent decimal places
- Currency: $X.XB or $X,XXXM format
- Percentages: X.X% format
- Negative numbers: parentheses, red text
- Column width: proportional to content
- Cell padding: 4-6pt

### Waterfall Charts
- Positive increments: Green (#00875A)
- Negative increments: Red (#C8102E)
- Totals: Navy (#002D72)
- Connectors: thin gray dashed lines between bars

## Source Citation Format

Bottom-left of every data slide:
```
Source: [Publisher], [Year/Date]. [Additional context if needed]
```

Examples:
```
Source: Grand View Research, 2024. Global EV Market Report
Source: Company 10-K filings, SEC EDGAR, FY2024
Source: Bloomberg, Statista, Industry interviews, 2024
Source: McKinsey Global Institute, December 2024
```

## Anti-Patterns (Things to Avoid)

- No 3D charts or effects
- No gradients on data elements (flat colors only)
- No clip art or stock photos unless specifically relevant
- No more than 2 chart types per slide
- No red/green combinations (colorblind issue) -- use navy/red instead
- No font sizes below 8pt
- No more than 4 colors in a single chart
- No borders around the slide
- No logos unless user provides them
- No "Questions?" slide (end with recommendations)
