# Framework Diagram Code Patterns

PptxGenJS shapes + text code for building consulting framework visuals as native, editable PowerPoint elements. Each framework is built directly on the slide after html2pptx creates the base layout.

**CRITICAL**: Never use `#` prefix with hex colors in PptxGenJS -- causes file corruption.

All examples use `theme.*` variables. Define at top of JS file:
```javascript
const theme = THEMES['wall-street']; // or 'tech-forward', 'clinical', 'vibrant', 'industrial'
```

## Table of Contents
1. [SWOT Analysis 2x2](#swot-analysis-2x2)
2. [Porter's Five Forces](#porters-five-forces)
3. [TAM/SAM/SOM Nested Circles](#tamsam-som-nested-circles)
4. [Competitive Positioning Matrix](#competitive-positioning-matrix)
5. [Risk Assessment Matrix](#risk-assessment-matrix)
6. [Timeline / Catalyst Chart](#timeline--catalyst-chart)
7. [Key Metrics Dashboard](#key-metrics-dashboard)
8. [Three-Column Layout](#three-column-layout)

---

## SWOT Analysis 2x2

Builds a professional 4-quadrant SWOT diagram with color-coded sections.

```javascript
// After html2pptx creates slide with title + source line
const ox = 0.8, oy = 1.3;  // origin x, y
const qw = 4.0, qh = 2.5;  // quadrant width, height
const gap = 0.08;

// Quadrant backgrounds
const quadrants = [
  { label: 'STRENGTHS', color: theme.primary, x: ox, y: oy },
  { label: 'WEAKNESSES', color: theme.chartColors[3], x: ox + qw + gap, y: oy },
  { label: 'OPPORTUNITIES', color: theme.accent, x: ox, y: oy + qh + gap },
  { label: 'THREATS', color: theme.negative, x: ox + qw + gap, y: oy + qh + gap },
];

quadrants.forEach(q => {
  // Background rectangle
  slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
    x: q.x, y: q.y, w: qw, h: qh,
    fill: { color: q.color, transparency: 90 }, // 90% transparent = very light
    line: { color: q.color, width: 1.5 },
    rectRadius: 0.1,
  });

  // Quadrant label (top-left of quadrant)
  slide.addText(q.label, {
    x: q.x + 0.15, y: q.y + 0.1, w: qw - 0.3, h: 0.35,
    fontSize: 11, bold: true, color: q.color,
  });
});

// Add data items to each quadrant
const swotData = {
  strengths: [
    '#1 market share at 34% ($45B)',
    'Gross margin 65%, 15pp above peers',
    '12,000+ patents in core tech',
  ],
  weaknesses: [
    '78% revenue from North America',
    'CAC increased 35% YoY',
    '3 C-suite departures in 18 months',
  ],
  opportunities: [
    'EU market growing 40% CAGR',
    '2026 regulatory mandate',
    'Adjacent $25B market opportunity',
  ],
  threats: [
    '2 startups raised $500M+ each',
    'Input costs up 45%',
    'Pending EU antitrust investigation',
  ],
};

function addBullets(items, x, y, w, color) {
  items.forEach((item, i) => {
    slide.addText([
      { text: '\u2022 ', options: { fontSize: 10, color: color, bold: true } },
      { text: item, options: { fontSize: 9, color: theme.bodyText } }
    ], {
      x: x + 0.15, y: y + 0.5 + i * 0.55, w: w - 0.3, h: 0.5,
      valign: 'top',
    });
  });
}

addBullets(swotData.strengths, ox, oy, qw, theme.primary);
addBullets(swotData.weaknesses, ox + qw + gap, oy, qw, theme.chartColors[3]);
addBullets(swotData.opportunities, ox, oy + qh + gap, qw, theme.accent);
addBullets(swotData.threats, ox + qw + gap, oy + qh + gap, qw, theme.negative);

// Row/column labels
slide.addText('INTERNAL', {
  x: 0.1, y: oy + qh * 0.5 - 0.15, w: 0.6, h: 0.3,
  fontSize: 8, color: theme.tertiaryText, rotate: 270, align: 'center',
});
slide.addText('EXTERNAL', {
  x: 0.1, y: oy + qh + gap + qh * 0.5 - 0.15, w: 0.6, h: 0.3,
  fontSize: 8, color: theme.tertiaryText, rotate: 270, align: 'center',
});
```

---

## Porter's Five Forces

Builds a pentagon-style diagram with the 5 forces arranged around a central "Rivalry" element.

```javascript
const cx = 5.0, cy = 3.2;  // center of diagram
const r = 1.8;              // radius
const boxW = 2.2, boxH = 0.9;

// Central box: Competitive Rivalry
slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
  x: cx - boxW/2, y: cy - boxH/2, w: boxW, h: boxH,
  fill: { color: theme.primary }, rectRadius: 0.1,
});
slide.addText([
  { text: 'Competitive Rivalry\n', options: { fontSize: 10, bold: true, color: 'FFFFFF' } },
  { text: 'HIGH (4/5)', options: { fontSize: 9, color: theme.tertiary } }
], { x: cx - boxW/2, y: cy - boxH/2, w: boxW, h: boxH, align: 'center', valign: 'middle' });

// Five forces positioned around center
const forces = [
  { name: 'Supplier Power', score: 'HIGH (4/5)', angle: 270, detail: 'NVIDIA 80%+ GPU share' },
  { name: 'Buyer Power', score: 'MEDIUM (3/5)', angle: 90, detail: 'Enterprise switching costs high' },
  { name: 'New Entrants', score: 'LOW (2/5)', angle: 0, detail: '$10B+ capex barrier' },
  { name: 'Substitutes', score: 'LOW (2/5)', angle: 180, detail: 'No viable alternatives at scale' },
  { name: 'Rivalry', score: 'HIGH (4/5)', angle: -1, detail: 'Hyperscaler + startup competition' }, // center
];

forces.filter(f => f.angle >= 0).forEach(f => {
  const rad = (f.angle * Math.PI) / 180;
  const fx = cx + r * Math.cos(rad) - boxW/2;
  const fy = cy - r * Math.sin(rad) - boxH/2;

  // Force box
  slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
    x: fx, y: fy, w: boxW, h: boxH,
    fill: { color: theme.bgGray },
    line: { color: theme.primary, width: 1 },
    rectRadius: 0.1,
  });

  slide.addText([
    { text: f.name + '\n', options: { fontSize: 9, bold: true, color: theme.primary } },
    { text: f.score + '\n', options: { fontSize: 8, bold: true, color: f.score.includes('HIGH') ? theme.negative : f.score.includes('MEDIUM') ? theme.tertiary : theme.accent } },
    { text: f.detail, options: { fontSize: 7, color: theme.secondaryText } }
  ], { x: fx, y: fy, w: boxW, h: boxH, align: 'center', valign: 'middle' });

  // Arrow from force to center
  const arrowStartX = fx + boxW/2;
  const arrowStartY = fy + boxH/2;
  slide.addShape(pptx.shapes.LINE, {
    x: Math.min(arrowStartX, cx), y: Math.min(arrowStartY, cy),
    w: Math.abs(arrowStartX - cx) || 0.01, h: Math.abs(arrowStartY - cy) || 0.01,
    line: { color: theme.lightGray, width: 1, dashType: 'dash' },
    flipH: arrowStartX > cx, flipV: arrowStartY > cy,
  });
});
```

---

## TAM/SAM/SOM Nested Circles

Builds concentric circles showing market sizing layers.

```javascript
const cx = 4.5, cy = 3.0;

// TAM (outermost)
slide.addShape(pptx.shapes.OVAL, {
  x: cx - 3.0, y: cy - 2.2, w: 6.0, h: 4.4,
  fill: { color: theme.primary, transparency: 85 },
  line: { color: theme.primary, width: 2 },
});
slide.addText([
  { text: 'TAM\n', options: { fontSize: 14, bold: true, color: theme.primary } },
  { text: '$450B', options: { fontSize: 20, bold: true, color: theme.primary } }
], { x: cx + 1.5, y: cy - 2.0, w: 1.8, h: 0.8, align: 'left' });

// SAM (middle)
slide.addShape(pptx.shapes.OVAL, {
  x: cx - 2.0, y: cy - 1.5, w: 4.0, h: 3.0,
  fill: { color: theme.accent, transparency: 80 },
  line: { color: theme.accent, width: 2 },
});
slide.addText([
  { text: 'SAM\n', options: { fontSize: 12, bold: true, color: theme.accent } },
  { text: '$85B', options: { fontSize: 16, bold: true, color: theme.accent } }
], { x: cx + 0.8, y: cy - 1.3, w: 1.5, h: 0.7, align: 'left' });

// SOM (innermost)
slide.addShape(pptx.shapes.OVAL, {
  x: cx - 1.0, y: cy - 0.75, w: 2.0, h: 1.5,
  fill: { color: theme.tertiary, transparency: 70 },
  line: { color: theme.tertiary, width: 2 },
});
slide.addText([
  { text: 'SOM\n', options: { fontSize: 10, bold: true, color: theme.tertiary } },
  { text: '$12B', options: { fontSize: 14, bold: true, color: theme.tertiary } }
], { x: cx - 0.8, y: cy - 0.5, w: 1.6, h: 0.7, align: 'center', valign: 'middle' });

// Methodology callout box (right side)
slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
  x: 7.0, y: 1.5, w: 2.5, h: 4.2,
  fill: { color: theme.bgGray },
  line: { color: theme.lightGray, width: 0.5 },
  rectRadius: 0.1,
});
slide.addText([
  { text: 'Methodology\n\n', options: { fontSize: 10, bold: true, color: theme.primary } },
  { text: 'TAM: ', options: { fontSize: 8, bold: true, color: theme.bodyText } },
  { text: 'Global market, all segments\n\n', options: { fontSize: 8, color: theme.secondaryText } },
  { text: 'SAM: ', options: { fontSize: 8, bold: true, color: theme.bodyText } },
  { text: 'Cloud AI, NA + Europe\n\n', options: { fontSize: 8, color: theme.secondaryText } },
  { text: 'SOM: ', options: { fontSize: 8, bold: true, color: theme.bodyText } },
  { text: 'Mid-market enterprise, Year 3\n\n', options: { fontSize: 8, color: theme.secondaryText } },
  { text: 'CAGR: 35% / 28% / 45%', options: { fontSize: 8, bold: true, color: theme.accent } },
], { x: 7.1, y: 1.6, w: 2.3, h: 4.0, valign: 'top' });
```

---

## Competitive Positioning Matrix

Builds a 2x2 strategic positioning grid with company bubbles.

```javascript
const gx = 1.0, gy = 1.3, gw = 7.5, gh = 4.5;

// Background quadrants
const quadColors = [
  { x: gx, y: gy, w: gw/2, h: gh/2, color: theme.primary, label: 'Premium Leaders' },
  { x: gx + gw/2, y: gy, w: gw/2, h: gh/2, color: theme.accent, label: 'Innovators' },
  { x: gx, y: gy + gh/2, w: gw/2, h: gh/2, color: theme.tertiary, label: 'Value Players' },
  { x: gx + gw/2, y: gy + gh/2, w: gw/2, h: gh/2, color: theme.chartColors[4], label: 'Niche Specialists' },
];

quadColors.forEach(q => {
  slide.addShape(pptx.shapes.RECTANGLE, {
    x: q.x, y: q.y, w: q.w, h: q.h,
    fill: { color: q.color, transparency: 92 },
    line: { color: theme.lightGray, width: 0.5 },
  });
  slide.addText(q.label, {
    x: q.x + 0.1, y: q.y + 0.05, w: q.w - 0.2, h: 0.3,
    fontSize: 8, italic: true, color: q.color,
  });
});

// Axis labels
slide.addText('Scale / Market Share  \u2192', {
  x: gx, y: gy + gh + 0.1, w: gw, h: 0.25,
  fontSize: 9, color: theme.secondaryText, align: 'center',
});
slide.addText('Innovation / Growth  \u2192', {
  x: gx - 0.7, y: gy + gh/2, w: gh, h: 0.25,
  fontSize: 9, color: theme.secondaryText, align: 'center', rotate: 270,
});

// Company bubbles (x%, y% position within grid, size = relative revenue)
const companies = [
  { name: 'NVIDIA', xPct: 0.85, yPct: 0.15, size: 0.8, color: theme.primary },
  { name: 'AMD', xPct: 0.35, yPct: 0.30, size: 0.5, color: theme.accent },
  { name: 'Intel', xPct: 0.55, yPct: 0.65, size: 0.6, color: theme.tertiary },
  { name: 'Startup A', xPct: 0.20, yPct: 0.20, size: 0.3, color: theme.negative },
];

companies.forEach(c => {
  const bx = gx + c.xPct * gw - c.size/2;
  const by = gy + c.yPct * gh - c.size/2;
  slide.addShape(pptx.shapes.OVAL, {
    x: bx, y: by, w: c.size, h: c.size,
    fill: { color: c.color, transparency: 30 },
    line: { color: c.color, width: 1.5 },
  });
  slide.addText(c.name, {
    x: bx, y: by, w: c.size, h: c.size,
    fontSize: 8, bold: true, color: c.color, align: 'center', valign: 'middle',
  });
});
```

---

## Risk Assessment Matrix

Builds a 3x3 grid with color-coded risk zones.

```javascript
const mx = 1.5, my = 1.3, mw = 6.0, mh = 4.5;
const cellW = mw / 3, cellH = mh / 3;

// Color zones (green bottom-left, red top-right)
const zoneColors = [
  // Row 0 (High impact) - top
  [theme.tertiary, theme.negative, theme.negative],
  // Row 1 (Medium impact) - middle
  [theme.accent, theme.tertiary, theme.negative],
  // Row 2 (Low impact) - bottom
  [theme.accent, theme.accent, theme.tertiary],
];

for (let row = 0; row < 3; row++) {
  for (let col = 0; col < 3; col++) {
    slide.addShape(pptx.shapes.RECTANGLE, {
      x: mx + col * cellW, y: my + row * cellH, w: cellW, h: cellH,
      fill: { color: zoneColors[row][col], transparency: 85 },
      line: { color: theme.lightGray, width: 0.5 },
    });
  }
}

// Axis labels
const impactLabels = ['HIGH', 'MEDIUM', 'LOW'];
const probLabels = ['LOW', 'MEDIUM', 'HIGH'];
impactLabels.forEach((l, i) => {
  slide.addText(l, { x: mx - 0.8, y: my + i * cellH, w: 0.7, h: cellH, fontSize: 8, color: theme.secondaryText, align: 'right', valign: 'middle' });
});
probLabels.forEach((l, i) => {
  slide.addText(l, { x: mx + i * cellW, y: my + mh + 0.05, w: cellW, h: 0.25, fontSize: 8, color: theme.secondaryText, align: 'center' });
});
slide.addText('IMPACT  \u2192', { x: mx - 1.2, y: my + mh/2 - 0.15, w: mh, h: 0.3, fontSize: 9, bold: true, color: theme.bodyText, rotate: 270, align: 'center' });
slide.addText('PROBABILITY  \u2192', { x: mx, y: my + mh + 0.3, w: mw, h: 0.25, fontSize: 9, bold: true, color: theme.bodyText, align: 'center' });

// Plot risks as labeled circles
const risks = [
  { name: 'Supply chain\nconcentration', prob: 2, impact: 0, color: theme.negative },
  { name: 'Regulatory\nchange', prob: 1, impact: 0, color: theme.tertiary },
  { name: 'Talent\nshortage', prob: 2, impact: 1, color: theme.negative },
  { name: 'FX\nexposure', prob: 0, impact: 2, color: theme.accent },
  { name: 'Tech\ndisruption', prob: 1, impact: 1, color: theme.tertiary },
];

risks.forEach(r => {
  const rx = mx + r.prob * cellW + cellW/2 - 0.4;
  const ry = my + r.impact * cellH + cellH/2 - 0.25;
  slide.addShape(pptx.shapes.OVAL, {
    x: rx, y: ry, w: 0.8, h: 0.5,
    fill: { color: 'FFFFFF' },
    line: { color: r.color, width: 2 },
  });
  slide.addText(r.name, {
    x: rx, y: ry, w: 0.8, h: 0.5,
    fontSize: 6, color: theme.bodyText, align: 'center', valign: 'middle',
  });
});
```

---

## Timeline / Catalyst Chart

Builds a horizontal timeline with dated events.

```javascript
const tlx = 0.8, tly = 3.0, tlw = 8.4;  // timeline position

// Main timeline line
slide.addShape(pptx.shapes.LINE, {
  x: tlx, y: tly, w: tlw, h: 0,
  line: { color: theme.primary, width: 3 },
});

const events = [
  { date: 'Q1 2025', label: 'NVIDIA Blackwell\nfull ramp', above: true },
  { date: 'Q2 2025', label: 'EU AI Act\nenforcement begins', above: false },
  { date: 'H2 2025', label: 'Sovereign AI\nmandates expand', above: true },
  { date: '2026', label: 'Inference cost\ncrossover point', above: false },
  { date: '2027', label: 'Custom silicon\n>20% share', above: true },
];

events.forEach((e, i) => {
  const ex = tlx + (i / (events.length - 1)) * tlw;

  // Vertical tick mark
  slide.addShape(pptx.shapes.LINE, {
    x: ex, y: tly - 0.15, w: 0, h: 0.3,
    line: { color: theme.primary, width: 2 },
  });

  // Dot on timeline
  slide.addShape(pptx.shapes.OVAL, {
    x: ex - 0.08, y: tly - 0.08, w: 0.16, h: 0.16,
    fill: { color: theme.primary },
  });

  // Date label
  slide.addText(e.date, {
    x: ex - 0.6, y: e.above ? tly - 0.9 : tly + 0.4, w: 1.2, h: 0.25,
    fontSize: 9, bold: true, color: theme.primary, align: 'center',
  });

  // Event description
  slide.addText(e.label, {
    x: ex - 0.7, y: e.above ? tly - 1.7 : tly + 0.65, w: 1.4, h: 0.7,
    fontSize: 8, color: theme.bodyText, align: 'center', valign: 'top',
  });
});
```

---

## Key Metrics Dashboard

Builds a row of large-number KPI cards for executive summary slides.

```javascript
const metrics = [
  { label: 'Market Size', value: '$150B', change: '+23% YoY', positive: true },
  { label: 'Top Player Share', value: '80%', change: 'NVIDIA dominates', positive: null },
  { label: 'Growth (CAGR)', value: '35%', change: '2024-2028E', positive: true },
  { label: 'Key Risk', value: 'Supply', change: 'Concentration', positive: false },
];

const cardW = 2.1, cardH = 1.6, startX = 0.6, startY = 1.5, gap = 0.2;
const accentColors = [theme.primary, theme.accent, theme.tertiary, theme.negative];

metrics.forEach((m, i) => {
  const cx = startX + i * (cardW + gap);

  // Card background
  slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
    x: cx, y: startY, w: cardW, h: cardH,
    fill: { color: theme.bgGray },
    line: { color: theme.lightGray, width: 0.5 },
    rectRadius: 0.1,
  });

  // Top accent line
  slide.addShape(pptx.shapes.RECTANGLE, {
    x: cx, y: startY, w: cardW, h: 0.06,
    fill: { color: accentColors[i] },
  });

  // Label
  slide.addText(m.label, {
    x: cx + 0.15, y: startY + 0.2, w: cardW - 0.3, h: 0.3,
    fontSize: 9, color: theme.secondaryText,
  });

  // Big number
  slide.addText(m.value, {
    x: cx + 0.15, y: startY + 0.5, w: cardW - 0.3, h: 0.6,
    fontSize: 28, bold: true, color: theme.primary,
  });

  // Change indicator
  slide.addText(m.change, {
    x: cx + 0.15, y: startY + 1.1, w: cardW - 0.3, h: 0.3,
    fontSize: 9, bold: true,
    color: m.positive === true ? theme.accent : m.positive === false ? theme.negative : theme.secondaryText,
  });
});
```

---

## Three-Column Layout

For "three key points" or "three trends" slides.

```javascript
const cols = [
  { number: '01', title: 'AI-First Infrastructure', body: 'Cloud providers investing $200B+ in AI-optimized data centers through 2027' },
  { number: '02', title: 'Inference Cost Decline', body: 'Cost per inference token dropping 90% every 18 months, enabling new use cases' },
  { number: '03', title: 'Sovereign AI Mandates', body: '15+ countries mandating domestic AI compute capacity by 2026' },
];

const colW = 2.6, colH = 3.5, startX = 0.8, startY = 1.5, gap = 0.35;
const colAccentColors = [theme.primary, theme.accent, theme.tertiary];

cols.forEach((c, i) => {
  const cx = startX + i * (colW + gap);

  // Large number
  slide.addText(c.number, {
    x: cx, y: startY, w: colW, h: 0.7,
    fontSize: 36, bold: true, color: colAccentColors[i],
  });

  // Accent line
  slide.addShape(pptx.shapes.RECTANGLE, {
    x: cx, y: startY + 0.75, w: 1.0, h: 0.04,
    fill: { color: colAccentColors[i] },
  });

  // Title
  slide.addText(c.title, {
    x: cx, y: startY + 0.95, w: colW, h: 0.5,
    fontSize: 13, bold: true, color: theme.bodyText,
  });

  // Body text
  slide.addText(c.body, {
    x: cx, y: startY + 1.5, w: colW, h: 2.0,
    fontSize: 10, color: theme.secondaryText, lineSpacing: 14,
  });
});
```
