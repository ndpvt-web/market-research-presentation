# Design Specifications

Adaptive design system with 6 themes that match topic tone. All themes maintain institutional credibility while offering visual variety.

## Theme System

### Theme Selection (Phase 1)

Map the topic classification to a theme:

| Topic Category | Theme | Rationale |
|---|---|---|
| **Finance / Banking / Investment** | `wall-street` | Navy, conservative, JP Morgan style |
| **Technology / SaaS / AI** | `tech-forward` | Dark mode, teal accents, modern feel |
| **Healthcare / Pharma / Biotech** | `clinical` | Clean whites, blue-green, professional calm |
| **Consumer / Retail / CPG** | `vibrant` | Warm palette, coral/amber, energetic |
| **Energy / Industrial / Infrastructure** | `industrial` | Slate/steel, amber highlights, grounded |
| **General / Multi-sector / Default** | `wall-street` | Safe default for any audience |

### Theme Definitions

Each theme defines: `primary`, `accent`, `tertiary`, `negative`, `darkBg`, `bodyText`, `secondaryText`, `tertiaryText`, `lightGray`, `bgGray`.

```javascript
const THEMES = {
  'wall-street': {
    primary: '002D72', accent: '00875A', tertiary: 'B3995D', negative: 'C8102E',
    darkBg: '001A44', bodyText: '333333', secondaryText: '666666',
    tertiaryText: '999999', lightGray: 'CCCCCC', bgGray: 'F5F5F5',
    chartColors: ['002D72', '00875A', 'B3995D', '5B9BD5', '7F7F7F', 'C8102E'],
    coverStyle: 'gradient', // navy gradient with gold accent line
    fontDisplay: 'Arial',
  },
  'tech-forward': {
    primary: '0F172A', accent: '06B6D4', tertiary: '8B5CF6', negative: 'EF4444',
    darkBg: '020617', bodyText: 'E2E8F0', secondaryText: '94A3B8',
    tertiaryText: '64748B', lightGray: '334155', bgGray: '1E293B',
    chartColors: ['06B6D4', '8B5CF6', '22D3EE', 'A78BFA', '94A3B8', 'EF4444'],
    coverStyle: 'dark', // dark background, bright accent text
    fontDisplay: 'Arial',
  },
  'clinical': {
    primary: '0369A1', accent: '059669', tertiary: '6366F1', negative: 'DC2626',
    darkBg: '0C4A6E', bodyText: '1E293B', secondaryText: '475569',
    tertiaryText: '94A3B8', lightGray: 'CBD5E1', bgGray: 'F0F9FF',
    chartColors: ['0369A1', '059669', '6366F1', '0EA5E9', '94A3B8', 'DC2626'],
    coverStyle: 'clean', // white with blue accent bar
    fontDisplay: 'Arial',
  },
  'vibrant': {
    primary: '9D174D', accent: 'D97706', tertiary: '059669', negative: 'DC2626',
    darkBg: '4C1D95', bodyText: '1F2937', secondaryText: '4B5563',
    tertiaryText: '9CA3AF', lightGray: 'D1D5DB', bgGray: 'FFF7ED',
    chartColors: ['9D174D', 'D97706', '059669', 'EC4899', '6B7280', 'DC2626'],
    coverStyle: 'bold', // warm gradient with large typography
    fontDisplay: 'Arial',
  },
  'industrial': {
    primary: '374151', accent: 'D97706', tertiary: '0369A1', negative: 'B91C1C',
    darkBg: '111827', bodyText: '1F2937', secondaryText: '6B7280',
    tertiaryText: '9CA3AF', lightGray: 'D1D5DB', bgGray: 'F3F4F6',
    chartColors: ['374151', 'D97706', '0369A1', '6B7280', '9CA3AF', 'B91C1C'],
    coverStyle: 'minimal', // clean lines, industrial palette
    fontDisplay: 'Arial',
  },
};
```

Apply theme by replacing hardcoded hex values throughout the JS file with theme variables. In HTML slides, use CSS variables mapped from theme.

## Typography

Use web-safe fonts only (must render in PowerPoint without embedding):

| Element | Font | Size | Weight | Color |
|---|---|---|---|---|
| **Slide Title** | Arial | 24pt | Bold | theme.primary |
| **Subtitle** | Arial | 18pt | Regular | theme.bodyText |
| **Body Text** | Arial | 12pt | Regular | theme.bodyText |
| **Bullet Text** | Arial | 11-12pt | Regular | theme.bodyText |
| **Chart Title** | Arial | 14pt | Bold | theme.bodyText |
| **Chart Labels** | Arial | 10pt | Regular | theme.secondaryText |
| **Source Line** | Arial | 8pt | Italic | theme.tertiaryText |
| **Table Header** | Arial | 11pt | Bold | FFFFFF (on primary bg) |
| **Table Body** | Arial | 10pt | Regular | theme.bodyText |
| **Section Number** | Arial | 72pt | Bold | FFFFFF (on dark bg) |
| **Hero Stat** | Arial | 48-64pt | Bold | theme.primary |

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

## Layout Library

### 1. Title Slide (with AI hero image)
```
+------------------------------------------+
| [AI-generated hero image - full bleed    |
|  or half-bleed with overlay gradient]    |
|                                          |
|     [TITLE - 36pt Bold White/Primary]    |
|     [Subtitle - 18pt]                    |
|     [Date | Author]                      |
|     [Accent line 2pt]                    |
|     [Disclaimer - 8pt]                   |
+------------------------------------------+
```
Generate cover image: `python3 ~/.claude/skills/generate-image/scripts/generate_image.py "Professional abstract [topic] themed background, corporate presentation, dark gradient, geometric patterns, no text" --output tmp/cover_image.png`

### 2. Executive Summary (text + callout metrics)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
|  [Bullet 1 with key finding]    | KEY    |
|  [Bullet 2 with key finding]    | METRICS|
|  [Bullet 3 with key finding]    | TABLE  |
|  [Bullet 4 with key finding]    | (right)|
|                                          |
| Source: [citation - 8pt italic]          |
+------------------------------------------+
```

### 3. Hero Stat Layout (NEW)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
|                                          |
|     [$XXX.XB]  48-64pt bold primary      |
|     [Label - 14pt secondary text]        |
|                                          |
|  [Context bullet 1]    [Context bullet 2]|
|  [Context bullet 3]    [Context bullet 4]|
|                                          |
| Source: [citation]                       |
+------------------------------------------+
```
Use for: TAM figures, key market sizes, growth rates, dominant statistics.
Build with PptxGenJS `slide.addText()` using oversized font for the hero number.

### 4. Full Chart
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
|                                          |
|          [CHART - 80% of slide]          |
|          [Data labels visible]           |
|          [Gridlines: horiz only, dashed] |
|                                          |
| Source: [citation]                       |
+------------------------------------------+
```

### 5. Chart with Insight Panel
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
|                        | [INSIGHT BOX]   |
|   [CHART - 60%]       | Primary bg 10%  |
|                        | * Key takeaway 1|
|                        | * Key takeaway 2|
|                        | * Key takeaway 3|
| Source: [citation]                       |
+------------------------------------------+
```

### 6. KPI Dashboard (NEW - 3 or 4 cards)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
| +--------+ +--------+ +--------+ +------+|
| | $135B  | | 19.4%  | | 3.2x   | | #1   ||
| | Market | | CAGR   | | YoY    | | Rank ||
| | Size   | | 24-30  | | Growth | |      ||
| | [trend]| | [trend]| | [trend]| |[icon]||
| +--------+ +--------+ +--------+ +------+|
|                                          |
| Source: [citation]                       |
+------------------------------------------+
```
Build with PptxGenJS shapes: rounded rectangle bg with text overlays. Use theme colors for card headers.

### 7. Comparison Table
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
| Company  | Rev($B) | Growth | Margin | PE |
|----------|---------|--------|--------|-----|
| [Primary-color header row, white text]   |
| [Alt row 1 - white bg]                  |
| [Alt row 2 - bgGray bg]                 |
|                                          |
| Source: [citation]                       |
+------------------------------------------+
```

### 8. Framework 2x2 (SWOT, etc.)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
|                  |                        |
|   [Quadrant 1]   |   [Quadrant 2]        |
|   accent bg 10%  |   primary bg 10%      |
|                  |                        |
|  ----------------|--------------------    |
|                  |                        |
|   [Quadrant 3]   |   [Quadrant 4]        |
|   tertiary bg 10%|   negative bg 10%     |
|                  |                        |
| Source: [citation]                       |
+------------------------------------------+
```

### 9. Section Divider (with AI image)
```
+------------------------------------------+
| [AI image or darkBg gradient full bleed] |
|                                          |
|     [Section Number - 72pt White]        |
|     [Section Title - 36pt White]         |
|     [Accent line 2pt]                    |
|                                          |
+------------------------------------------+
```
Optional: Generate section-specific AI image for background.

### 10. Split Image + Text (NEW)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
|                        |                 |
|  [AI-generated image]  | * Key point 1  |
|  [or chart]            | * Key point 2  |
|  [50% width]           | * Key point 3  |
|                        | * Key point 4  |
| Source: [citation]                       |
+------------------------------------------+
```

### 11. Icon Grid / Feature Cards (NEW)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
| [Icon1]  [Label1]  | [Icon2]  [Label2]  |
| [Desc line 1]      | [Desc line 1]      |
| [Desc line 2]      | [Desc line 2]      |
|---------------------|---------------------|
| [Icon3]  [Label3]  | [Icon4]  [Label4]  |
| [Desc line 1]      | [Desc line 1]      |
| [Desc line 2]      | [Desc line 2]      |
| Source: [citation]                       |
+------------------------------------------+
```
Use PptxGenJS shapes: colored circle with text overlay for icon area.

### 12. Verdict / Recommendation (NEW)
```
+------------------------------------------+
| [Action Title - 24pt]                    |
|------------------------------------------|
| [Accent-color left bar 4pt]             |
|                                          |
|  [Verdict headline - 18pt bold]          |
|                                          |
|  * Recommendation 1 with supporting data |
|  * Recommendation 2 with supporting data |
|  * Recommendation 3 with supporting data |
|  * Recommendation 4 with supporting data |
|                                          |
| Source: [citation]                       |
+------------------------------------------+
```

## AI Image Generation Guidelines

### When to Generate Images
- **Always**: Cover slide (1 image)
- **Recommended**: Section dividers (1 per section, 2-4 images)
- **Optional**: Split image+text slides for key concepts

### Image Prompt Template
```
Professional abstract [TOPIC] themed [STYLE] background for corporate presentation slide. [COLOR MOOD] color palette, geometric patterns, clean composition, no text, no logos, 16:9 aspect ratio
```

### Style Keywords by Theme
| Theme | Prompt Style Keywords |
|---|---|
| wall-street | "navy blue gradient, financial charts abstract, sophisticated, minimal" |
| tech-forward | "dark futuristic, circuit board patterns, neon cyan accents, digital" |
| clinical | "clean white medical, molecular structures, blue tones, scientific" |
| vibrant | "warm sunset gradients, dynamic flowing shapes, coral amber, energetic" |
| industrial | "steel textures, amber industrial lighting, geometric engineering, bold" |

### Image Integration in PptxGenJS
```javascript
// Cover slide with full-bleed image + overlay
slide.addImage({ path: 'tmp/cover_image.png', x: 0, y: 0, w: 13.33, h: 7.5 });
// Add dark overlay for text readability
slide.addShape(pptx.shapes.RECT, {
  x: 0, y: 0, w: 13.33, h: 7.5,
  fill: { color: theme.darkBg, transparency: 40 },
});
// Then add text on top
slide.addText('Title', { x: 1, y: 2.5, w: 11, fontSize: 36, color: 'FFFFFF', bold: true });
```

## Chart Formatting Standards

### Bar Charts
- Primary color for main series
- Horizontal gridlines only (dashed, theme.lightGray)
- Data labels: above bars, 10pt, theme.bodyText
- Axis labels: theme.secondaryText
- No chart border
- Bar gap: 50-80% of bar width

### Line Charts
- 2pt line weight
- Markers: circle, size 6
- Primary + accent colors for 2 series
- Smooth lines
- Data labels on endpoints or inflection points

### Pie Charts
- Max 6 segments (group small as "Other")
- Data labels: value + percentage
- Start at 12 o'clock position
- Slight explosion (2-3%) for key segment
- No 3D effects

### Tables
- Header row: primary background, white text, bold
- Body: Alternating white / bgGray rows
- Numbers: right-aligned, consistent decimal places
- Currency: $X.XB or $X,XXXM format
- Percentages: X.X% format
- Negative numbers: parentheses, negative color text
- Column width: proportional to content
- Cell padding: 4-6pt

### Waterfall Charts
- Positive increments: accent color
- Negative increments: negative color
- Totals: primary color
- Connectors: thin lightGray dashed lines

## Source Citation Format

Bottom-left of every data slide:
```
Source: [Publisher], [Year/Date]. [Additional context if needed]
```

## Anti-Patterns

- No 3D charts or effects
- No gradients on data elements (flat colors only)
- No clip art or stock photos (use AI-generated imagery instead)
- No more than 2 chart types per slide
- No red/green combinations (colorblind issue)
- No font sizes below 8pt
- No more than 4 colors in a single chart
- No borders around the slide
- No logos unless user provides them
- No "Questions?" slide (end with recommendations)
