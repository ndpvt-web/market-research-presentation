# Analytical Frameworks Reference

Detailed instructions for applying each framework with real research data.

## Table of Contents
1. [TAM/SAM/SOM](#tamsam-som)
2. [Porter's Five Forces](#porters-five-forces)
3. [SWOT Analysis](#swot-analysis)
4. [PESTEL Analysis](#pestel-analysis)
5. [Competitive Positioning Matrix](#competitive-positioning-matrix)
6. [Financial Benchmarking](#financial-benchmarking)
7. [Risk Assessment Matrix](#risk-assessment-matrix)
8. [Bear/Base/Bull Scenarios](#bearbull-scenarios)

---

## TAM/SAM/SOM

### Purpose
Size the market opportunity using both top-down and bottom-up approaches.

### Data Required
- Total global/regional market value (from industry reports, analyst estimates)
- Segment breakdowns (by geography, product type, customer segment)
- Growth rates (historical CAGR and forecast)
- Company-specific revenue and market share (for SOM)

### Methodology

**Top-Down:**
```
TAM = Total market value from industry reports
SAM = TAM x (% addressable by geography) x (% addressable by segment)
SOM = SAM x (realistic market share based on competitive position)
```

**Bottom-Up:**
```
TAM = (Total potential customers) x (Average revenue per customer)
SAM = (Reachable customers with current product/channel) x (ARPC)
SOM = (Customers you can realistically win in 3-5 years) x (ARPC)
```

### Output Format
- Nested circles diagram (TAM outermost, SOM innermost)
- Methodology table showing both approaches
- Growth projection table (current year + 3-5 year forecast)

### Example Output
```
TAM: $450B (Global AI infrastructure, all segments)
SAM: $85B  (Cloud AI services, North America + Europe)
SOM: $12B  (Mid-market enterprise segment, year 3 target)

CAGR: TAM 35%, SAM 28%, SOM 45% (faster growth from market share gains)
```

---

## Porter's Five Forces

### Purpose
Assess industry attractiveness and competitive intensity.

### Scoring
Rate each force 1-5 (1 = very weak/favorable, 5 = very strong/unfavorable):

| Force | Score 1-2 (Favorable) | Score 3 (Neutral) | Score 4-5 (Unfavorable) |
|---|---|---|---|
| Supplier Power | Many suppliers, commodity inputs | Moderate concentration | Few suppliers, proprietary inputs |
| Buyer Power | Fragmented buyers, high switching costs | Moderate | Concentrated buyers, low switching costs |
| Substitutes | No close substitutes | Some alternatives | Many substitutes, low switching cost |
| New Entrants | High barriers (capital, regulation, IP) | Moderate barriers | Low barriers, easy entry |
| Rivalry | Few competitors, growing market | Moderate competition | Many competitors, slow growth, commodity |

### Data Required (per force)
- Supplier Power: # of key suppliers, input cost as % of COGS, switching costs
- Buyer Power: customer concentration, price sensitivity, switching costs
- Substitutes: alternative products, price-performance ratio, switching barriers
- New Entrants: capital requirements, regulatory barriers, IP/patent landscape
- Rivalry: # of competitors, market growth rate, differentiation level

### Output Format
- Radar/pentagon chart with 5 axes (1-5 scale)
- Assessment table: Force | Score | Key Drivers (2-3 data-backed points) | Trend (arrow)

---

## SWOT Analysis

### Purpose
Map internal strengths/weaknesses against external opportunities/threats.

### Structure

| | Positive | Negative |
|---|---|---|
| **Internal** | **Strengths** (controllable advantages) | **Weaknesses** (controllable disadvantages) |
| **External** | **Opportunities** (market/macro tailwinds) | **Threats** (market/macro headwinds) |

### Rules
- Each quadrant: 3-5 items maximum
- Each item must include a specific data point or metric
- Prioritize by impact (highest impact first)
- Cross-reference: each Strength should map to an Opportunity it can exploit; each Weakness should map to a Threat it amplifies

### Example Items
```
Strengths:
- #1 market share at 34% ($45B revenue), 2.5x nearest competitor
- Gross margin of 65%, 15pp above peer average
- 12,000+ patents in core technology domain

Weaknesses:
- Revenue concentrated in single geography (78% North America)
- Customer acquisition cost increased 35% YoY
- Management turnover: 3 C-suite departures in 18 months

Opportunities:
- European market growing 40% CAGR, currently only 8% penetrated
- Regulatory mandate in 2026 will require adoption by 50% of enterprises
- Adjacent market ($25B) shares same customer base

Threats:
- Two well-funded startups raised $500M+ each in last 12 months
- Input costs (rare earth minerals) up 45% due to supply constraints
- Pending antitrust investigation in EU
```

### Output Format
- 2x2 quadrant diagram with colored backgrounds
- Each cell contains 3-5 bullet points with data

---

## PESTEL Analysis

### Purpose
Map macro-environmental factors affecting the industry.

### Categories

| Factor | What to Research | Example Data Points |
|---|---|---|
| **Political** | Government policy, trade, stability | Tariff rates, trade agreements, political risk index |
| **Economic** | GDP, inflation, interest rates, FX | GDP growth %, inflation rate, consumer confidence index |
| **Social** | Demographics, culture, consumer behavior | Population trends, adoption rates, consumer surveys |
| **Technological** | Innovation, R&D, digital transformation | Patent filings, R&D spend as % revenue, tech adoption curves |
| **Environmental** | Climate, sustainability, ESG | Carbon regulations, ESG scores, sustainability mandates |
| **Legal** | Regulations, compliance, IP law | Regulatory changes, compliance costs, patent disputes |

### Scoring
Rate each factor: High Impact / Medium Impact / Low Impact, and Trend: Favorable / Neutral / Unfavorable

### Output Format
- Matrix table: Factor | Key Finding (with data) | Impact | Trend
- Color-code: Green (favorable), Yellow (neutral), Red (unfavorable)

---

## Competitive Positioning Matrix

### Purpose
Visualize where competitors sit on key strategic dimensions.

### Steps

1. **Select axes** based on industry dynamics:
   - Market Share vs Growth Rate (BCG-style)
   - Price vs Quality/Features
   - Scale vs Innovation
   - Breadth vs Depth of offering
   - B2B vs B2C focus

2. **Plot companies** as bubbles:
   - X position = axis 1 metric
   - Y position = axis 2 metric
   - Bubble size = revenue or market cap

3. **Identify strategic groups** -- clusters of companies with similar positioning

4. **Find white space** -- underserved positions on the matrix

### Data Required
- Revenue/market cap for each competitor (bubble size)
- Metrics for both axes (from financial data, product analysis)
- 5-12 competitors for meaningful visualization

### Output Format
- Scatter/bubble chart with labeled competitors
- Quadrant labels (e.g., "Premium Leaders", "Value Challengers", "Niche Players", "Commodity")
- Peer comparison table alongside the chart

---

## Financial Benchmarking

### Purpose
Compare financial performance across peer set.

### Standard Metrics

| Category | Metrics | Format |
|---|---|---|
| **Scale** | Revenue, Market Cap | $M or $B |
| **Growth** | Revenue Growth YoY, 3yr CAGR | x.x% |
| **Profitability** | Gross Margin, EBITDA Margin, Net Margin | x.x% |
| **Efficiency** | Revenue per Employee, CAC, LTV/CAC | $K or ratio |
| **Valuation** | P/E, EV/Revenue, EV/EBITDA | x.x |

### Output Format
- Comparison table with companies as columns, metrics as rows
- Highlight best-in-class (green) and laggard (red) for each metric
- Bar charts for 2-3 most important metrics

---

## Risk Assessment Matrix

### Purpose
Map key risks by probability and impact.

### Structure
- X-axis: Probability (Low / Medium / High)
- Y-axis: Impact (Low / Medium / High)
- Each risk plotted as a labeled point
- Color zones: Green (low-low), Yellow (medium), Red (high-high)

### Standard Risk Categories
- Market risk (demand, competition, pricing)
- Operational risk (supply chain, execution, talent)
- Regulatory risk (policy changes, compliance)
- Technology risk (disruption, obsolescence)
- Financial risk (liquidity, currency, interest rates)
- Geopolitical risk (trade, sanctions, instability)

### Output Format
- 3x3 matrix with risks plotted
- Risk mitigation table: Risk | Probability | Impact | Mitigation Strategy

---

## Bear/Base/Bull Scenarios

### Purpose
Provide a range of outcomes for key metrics.

### Structure

| Metric | Bear Case | Base Case | Bull Case |
|---|---|---|---|
| Market Size (Year X) | Conservative estimate | Consensus estimate | Optimistic estimate |
| Revenue Growth | Below-trend growth rate | Trend growth rate | Above-trend rate |
| Market Share | Share loss scenario | Stable share | Share gain scenario |
| Margin | Compression scenario | Stable margins | Expansion scenario |

### Key Assumptions for Each Scenario
- **Bear**: Macro headwinds, competitive pressure, regulatory burden, execution missteps
- **Base**: Current trends continue, moderate competition, normal execution
- **Bull**: Market acceleration, competitive wins, favorable regulation, strong execution

### Probability Assignment
- Bear: 20-25% probability
- Base: 50-60% probability
- Bull: 20-25% probability

### Output Format
- Table with scenarios as columns, key metrics as rows
- Implied valuation range for each scenario
- Football field chart (horizontal bar showing value range)
