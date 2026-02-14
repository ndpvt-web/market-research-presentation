# Chart & Table Code Patterns

Production-ready PptxGenJS code for every chart type used in institutional research presentations. All examples use **theme variables** from the selected theme object (see design-specs.md for theme definitions).

**CRITICAL**: Never use `#` prefix with hex colors in PptxGenJS -- causes file corruption.

## Theme Variable Usage

All chart code references `theme.*` variables. Define the theme object at the top of your JS file:

```javascript
const theme = THEMES['wall-street']; // or 'tech-forward', 'clinical', 'vibrant', 'industrial'
```

Key mappings: `theme.primary`, `theme.accent`, `theme.tertiary`, `theme.negative`, `theme.chartColors` (array of 6), `theme.bodyText`, `theme.secondaryText`, `theme.lightGray`, `theme.bgGray`.

## Table of Contents
1. [Bar Chart - Market Size](#bar-chart---market-size)
2. [Grouped Bar - Peer Comparison](#grouped-bar---peer-comparison)
3. [Horizontal Bar - Market Share](#horizontal-bar---market-share)
4. [Line Chart - Growth Trends](#line-chart---growth-trends)
5. [Multi-Line - Revenue Trajectories](#multi-line---revenue-trajectories)
6. [Pie Chart - Segment Breakdown](#pie-chart---segment-breakdown)
7. [Stacked Bar - Revenue Mix](#stacked-bar---revenue-mix)
8. [Scatter/Bubble - Competitive Positioning](#scatterbubble---competitive-positioning)
9. [Combo Chart - Size + Growth](#combo-chart---size--growth)
10. [Institutional Tables](#institutional-tables)
11. [Waterfall via Stacked Bar](#waterfall-via-stacked-bar)

---

## Bar Chart - Market Size

Use for: Market size by year, revenue comparison, single-metric comparison.

```javascript
slide.addChart(pptx.charts.BAR, [
  {
    name: 'Market Size ($B)',
    labels: ['2020', '2021', '2022', '2023', '2024', '2025E', '2026E', '2027E'],
    values: [65, 82, 105, 128, 150, 185, 225, 280],
  }
], {
  ...chartArea,           // from placeholder
  barDir: 'col',          // vertical bars
  barGrouping: 'clustered',
  chartColors: [theme.primary],
  showValue: true,
  dataLabelPosition: 'outEnd',
  dataLabelColor: theme.bodyText,
  dataLabelFontSize: 9,
  showLegend: false,
  valGridLine: { style: 'dash', color: theme.lightGray, size: 0.5 },
  catAxisLabelColor: theme.secondaryText,
  catAxisLabelFontSize: 9,
  valAxisLabelColor: theme.secondaryText,
  valAxisLabelFontSize: 9,
  valAxisMinVal: 0,
  valAxisMajorUnit: 50,
  showCatAxisTitle: false,
  showValAxisTitle: true,
  valAxisTitle: 'Market Size ($B)',
  valAxisTitleColor: theme.secondaryText,
  valAxisTitleFontSize: 9,
});
```

**Forecast bars pattern**: To distinguish historical vs forecast, use two series:
```javascript
slide.addChart(pptx.charts.BAR, [
  {
    name: 'Actual',
    labels: ['2020', '2021', '2022', '2023', '2024', '2025E', '2026E'],
    values: [65, 82, 105, 128, 150, null, null],
  },
  {
    name: 'Forecast',
    labels: ['2020', '2021', '2022', '2023', '2024', '2025E', '2026E'],
    values: [null, null, null, null, null, 185, 225],
  }
], {
  ...chartArea,
  barDir: 'col',
  barGrouping: 'stacked',
  chartColors: [theme.primary, theme.chartColors[3]], // Solid primary for actual, lighter for forecast
  showValue: true,
  dataLabelPosition: 'outEnd',
  showLegend: true,
  legendPos: 'b',
  valGridLine: { style: 'dash', color: theme.lightGray },
});
```

---

## Grouped Bar - Peer Comparison

Use for: Comparing 2-3 metrics across companies or time periods.

```javascript
slide.addChart(pptx.charts.BAR, [
  {
    name: 'Gross Margin',
    labels: ['NVIDIA', 'AMD', 'Intel', 'Broadcom', 'Qualcomm'],
    values: [65.2, 48.5, 42.1, 68.3, 55.8],
  },
  {
    name: 'EBITDA Margin',
    labels: ['NVIDIA', 'AMD', 'Intel', 'Broadcom', 'Qualcomm'],
    values: [58.1, 22.4, 18.7, 52.6, 32.1],
  }
], {
  ...chartArea,
  barDir: 'col',
  barGrouping: 'clustered',
  chartColors: [theme.primary, theme.accent],
  showValue: true,
  dataLabelPosition: 'outEnd',
  dataLabelFontSize: 8,
  dataLabelColor: theme.bodyText,
  showLegend: true,
  legendPos: 'b',
  legendFontSize: 9,
  valGridLine: { style: 'dash', color: theme.lightGray },
  catAxisLabelFontSize: 9,
  valAxisLabelFontSize: 9,
  showValAxisTitle: true,
  valAxisTitle: 'Margin (%)',
  valAxisMinVal: 0,
  valAxisMaxVal: 80,
  valAxisMajorUnit: 20,
});
```

---

## Horizontal Bar - Market Share

Use for: Market share ranking, sorted comparisons.

```javascript
slide.addChart(pptx.charts.BAR, [
  {
    name: 'Market Share (%)',
    labels: ['NVIDIA', 'AMD', 'Intel', 'Google (TPU)', 'AWS (Trainium)', 'Other'],
    values: [80, 8, 5, 3, 2, 2],
  }
], {
  ...chartArea,
  barDir: 'bar',           // horizontal
  chartColors: [theme.primary],
  showValue: true,
  dataLabelPosition: 'outEnd',
  dataLabelColor: theme.bodyText,
  dataLabelFontSize: 9,
  showLegend: false,
  valGridLine: { style: 'none' },
  catAxisLabelFontSize: 10,
  catAxisOrientation: 'maxMin',  // largest at top
  valAxisMinVal: 0,
  valAxisMaxVal: 100,
});
```

---

## Line Chart - Growth Trends

Use for: Time series, growth trajectories, trend lines.

```javascript
slide.addChart(pptx.charts.LINE, [
  {
    name: 'Revenue ($B)',
    labels: ['2019', '2020', '2021', '2022', '2023', '2024'],
    values: [10.9, 16.7, 26.9, 27.0, 60.9, 130.5],
  }
], {
  ...chartArea,
  lineSize: 3,
  lineSmooth: false,
  lineDataSymbol: 'circle',
  lineDataSymbolSize: 8,
  chartColors: [theme.primary],
  showValue: true,
  dataLabelPosition: 't',
  dataLabelColor: theme.primary,
  dataLabelFontSize: 9,
  showLegend: false,
  valGridLine: { style: 'dash', color: theme.lightGray },
  catAxisLabelColor: theme.secondaryText,
  valAxisLabelColor: theme.secondaryText,
  valAxisMinVal: 0,
  showValAxisTitle: true,
  valAxisTitle: 'Revenue ($B)',
});
```

---

## Multi-Line - Revenue Trajectories

Use for: Comparing growth of multiple companies over time.

```javascript
slide.addChart(pptx.charts.LINE, [
  {
    name: 'Company A',
    labels: ['2020', '2021', '2022', '2023', '2024'],
    values: [15, 22, 35, 48, 65],
  },
  {
    name: 'Company B',
    labels: ['2020', '2021', '2022', '2023', '2024'],
    values: [12, 18, 24, 32, 42],
  },
  {
    name: 'Company C',
    labels: ['2020', '2021', '2022', '2023', '2024'],
    values: [8, 10, 14, 18, 25],
  }
], {
  ...chartArea,
  lineSize: 2.5,
  lineSmooth: false,
  lineDataSymbol: 'circle',
  lineDataSymbolSize: 6,
  chartColors: [theme.primary, theme.accent, theme.tertiary],
  showLegend: true,
  legendPos: 'b',
  legendFontSize: 9,
  valGridLine: { style: 'dash', color: theme.lightGray },
  showValAxisTitle: true,
  valAxisTitle: 'Revenue ($B)',
  valAxisMinVal: 0,
});
```

---

## Pie Chart - Segment Breakdown

Use for: Market share, revenue mix, segment breakdown.

```javascript
slide.addChart(pptx.charts.PIE, [
  {
    name: 'Revenue Mix',
    labels: ['Cloud Services', 'Enterprise SW', 'Consulting', 'Hardware', 'Other'],
    values: [42, 28, 15, 10, 5],
  }
], {
  ...chartArea,
  showPercent: true,
  showLegend: true,
  legendPos: 'r',
  legendFontSize: 10,
  chartColors: theme.chartColors,
  dataLabelColor: 'FFFFFF',
  dataLabelFontSize: 10,
});
```

---

## Stacked Bar - Revenue Mix

Use for: Composition over time, segment evolution.

```javascript
slide.addChart(pptx.charts.BAR, [
  {
    name: 'Data Center',
    labels: ['2021', '2022', '2023', '2024'],
    values: [10.6, 15.0, 47.5, 87.5],
  },
  {
    name: 'Gaming',
    labels: ['2021', '2022', '2023', '2024'],
    values: [12.5, 9.1, 9.0, 10.3],
  },
  {
    name: 'Auto & Other',
    labels: ['2021', '2022', '2023', '2024'],
    values: [3.8, 2.9, 4.4, 5.7],
  }
], {
  ...chartArea,
  barDir: 'col',
  barGrouping: 'stacked',
  chartColors: [theme.primary, theme.accent, theme.tertiary],
  showValue: true,
  dataLabelFontSize: 8,
  dataLabelColor: 'FFFFFF',
  showLegend: true,
  legendPos: 'b',
  valGridLine: { style: 'dash', color: theme.lightGray },
  showValAxisTitle: true,
  valAxisTitle: 'Revenue ($B)',
});
```

---

## Scatter/Bubble - Competitive Positioning

Use for: Competitive positioning matrix, price vs quality mapping.

```javascript
// Scatter data format: first series = X values, subsequent = Y values
const companies = [
  { name: 'NVIDIA', x: 80, y: 65 },   // market share, gross margin
  { name: 'AMD', x: 8, y: 48 },
  { name: 'Intel', x: 5, y: 42 },
  { name: 'Broadcom', x: 3, y: 68 },
];

slide.addChart(pptx.charts.SCATTER, [
  { name: 'X-Axis', values: companies.map(c => c.x) },
  { name: 'Companies', values: companies.map(c => c.y) },
], {
  ...chartArea,
  lineSize: 0,
  lineDataSymbol: 'circle',
  lineDataSymbolSize: 12,
  chartColors: [theme.primary],
  showCatAxisTitle: true,
  catAxisTitle: 'Market Share (%)',
  showValAxisTitle: true,
  valAxisTitle: 'Gross Margin (%)',
  catAxisLabelColor: theme.secondaryText,
  valAxisLabelColor: theme.secondaryText,
  valGridLine: { style: 'dash', color: theme.lightGray },
  catGridLine: { style: 'dash', color: theme.lightGray },
});

// Add company labels manually
companies.forEach((c, i) => {
  const xPct = (c.x - 0) / (100 - 0);
  const yPct = 1 - (c.y - 30) / (80 - 30);
  slide.addText(c.name, {
    x: chartArea.x + xPct * chartArea.w + 0.1,
    y: chartArea.y + yPct * chartArea.h - 0.15,
    w: 1.2, h: 0.3,
    fontSize: 8, color: theme.bodyText, bold: true,
  });
});
```

---

## Combo Chart - Size + Growth

Use for: Market size bars with growth rate line overlay.

```javascript
// PptxGenJS supports combo charts with multiple chart types
slide.addChart(
  [pptx.charts.BAR, pptx.charts.LINE],
  [
    // Bar series (primary axis)
    {
      name: 'Market Size ($B)',
      labels: ['2021', '2022', '2023', '2024', '2025E'],
      values: [82, 105, 128, 150, 185],
    },
    // Line series (secondary axis)
    {
      name: 'YoY Growth (%)',
      labels: ['2021', '2022', '2023', '2024', '2025E'],
      values: [26, 28, 22, 17, 23],
    }
  ],
  {
    ...chartArea,
    barDir: 'col',
    chartColors: [theme.primary, theme.negative],
    showValue: true,
    dataLabelFontSize: 8,
    showLegend: true,
    legendPos: 'b',
    catAxisLabelFontSize: 9,
    valGridLine: { style: 'dash', color: theme.lightGray },
    secondaryValAxis: true,
    showValAxisTitle: true,
    valAxisTitle: 'Market Size ($B)',
    secondaryValAxisTitle: 'YoY Growth (%)',
    lineSize: 3,
    lineDataSymbol: 'circle',
    lineDataSymbolSize: 8,
  }
);
```

---

## Institutional Tables

### Peer Benchmarking Table

```javascript
const headerStyle = { fill: { color: theme.primary }, color: 'FFFFFF', bold: true, fontSize: 10, align: 'center', valign: 'middle' };
const cellStyle = { fontSize: 10, color: theme.bodyText, valign: 'middle' };
const numStyle = { ...cellStyle, align: 'right' };
const altRow = { fill: { color: theme.bgGray } };

const tableData = [
  // Header row
  [
    { text: 'Company', options: { ...headerStyle, align: 'left' } },
    { text: 'Revenue ($B)', options: headerStyle },
    { text: 'Growth (%)', options: headerStyle },
    { text: 'Gross Margin', options: headerStyle },
    { text: 'EV/Revenue', options: headerStyle },
    { text: 'P/E', options: headerStyle },
  ],
  // Data rows (alternate shading)
  [
    { text: 'NVIDIA', options: { ...cellStyle, bold: true } },
    { text: '$130.5', options: numStyle },
    { text: '114.2%', options: { ...numStyle, color: theme.accent } },
    { text: '65.2%', options: numStyle },
    { text: '25.4x', options: numStyle },
    { text: '58.2x', options: numStyle },
  ],
  [
    { text: 'AMD', options: { ...cellStyle, bold: true, ...altRow } },
    { text: '$23.3', options: { ...numStyle, ...altRow } },
    { text: '10.2%', options: { ...numStyle, ...altRow } },
    { text: '48.5%', options: { ...numStyle, ...altRow } },
    { text: '8.2x', options: { ...numStyle, ...altRow } },
    { text: '42.1x', options: { ...numStyle, ...altRow } },
  ],
  // ... more rows
];

slide.addTable(tableData, {
  x: 0.5, y: 1.2, w: 9, h: 0.1,  // h auto-adjusts
  colW: [1.8, 1.4, 1.2, 1.4, 1.2, 1.0],
  border: { pt: 0.5, color: theme.lightGray },
  autoPage: false,
});
```

### Conditional Formatting Rules

- **Positive values** (growth, margins above average): Color `theme.accent` (green)
- **Negative values** (declines, below average): Color `theme.negative` (red)
- **Best-in-class**: Bold + accent color
- **Worst-in-class**: Negative color text
- **Numbers**: Always right-aligned
- **Company names**: Left-aligned, bold

---

## Waterfall via Stacked Bar

PptxGenJS does not have a native waterfall chart. Simulate using invisible base + visible increment stacked bars.

```javascript
// Revenue bridge: $150B total = $65B chips + $40B cloud + $25B networking + $20B services
const labels = ['Chips', 'Cloud Infra', 'Networking', 'Services', 'Total'];
const base =   [0, 65, 105, 130, 0];        // invisible base
const values = [65, 40, 25, 20, 150];        // visible increment

slide.addChart(pptx.charts.BAR, [
  {
    name: 'Base',
    labels: labels,
    values: base,
  },
  {
    name: 'Value',
    labels: labels,
    values: values,
  }
], {
  ...chartArea,
  barDir: 'col',
  barGrouping: 'stacked',
  chartColors: ['FFFFFF', theme.primary],  // Base invisible (white), value primary
  showValue: true,
  dataLabelFontSize: 9,
  dataLabelColor: 'FFFFFF',
  showLegend: false,
  valGridLine: { style: 'none' },
  catAxisLabelFontSize: 10,
  valAxisHidden: true,
});
// Note: Base series uses white color to appear invisible against white bg
// For dark themes (tech-forward), use theme.darkBg instead of 'FFFFFF' for base
```
