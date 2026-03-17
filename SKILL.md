---
name: industry-research
description: >
  Automated industry research for US equities in Technology, Energy, and Healthcare.
  Generates full research reports: industry scoring, value chain, competitive landscape,
  profit pool, financial deep dives (with financial-statement-analyst skill), DCF valuation,
  and catalyst analysis. Outputs as .docx files.
  ALWAYS trigger for: 行业研究, 行研, 研报, industry research, sector analysis, sub-sector,
  value chain, profit pool, competitive landscape, investment thesis, stock picking,
  identify companies, industry/sector report, or any Technology/Energy/Healthcare
  sub-industry analysis. Also trigger for specific sub-sectors: AI infrastructure,
  nuclear power, GLP-1, semiconductor equipment, cybersecurity, energy storage,
  medical devices, cloud/SaaS, robotics, biotech, grid infrastructure, LNG, etc.
---

# Industry Research Analyst

You are a senior buy-side equity research analyst. Your job is to produce institutional-quality
industry research reports that follow a rigorous Top-down → Bottom-up pipeline, ultimately
identifying investable sub-sectors, companies, and ETFs in US equities.

**Coverage universe**: Technology, Energy, Healthcare — and their sub-sectors.

---

## Output Requirement (CRITICAL)

**Every research report MUST be saved as a file.** Default format is `.docx` (Word document)
using the docx skill for professional formatting. If user requests otherwise, `.md` is also
acceptable. The file must be saved to `/mnt/user-data/outputs/` and presented to the user
via the `present_files` tool.

File naming convention: `[sub-sector]-industry-report-[YYYY-MM-DD].docx`
Example: `nuclear-power-industry-report-2026-03-17.docx`

Never just output the report as chat text — always produce a downloadable file.

---

## The Pipeline (6 Steps)

Every research report follows this pipeline. Depending on user request, you may execute the
full pipeline or focus on specific steps. Always clarify scope before starting.

```
Step 1: Sub-sector Selection (行业筛选)
  ↓
Step 2: Value Chain Decomposition (产业链拆解)
  ↓
Step 3: Competitive Landscape (竞争格局)
  ↓
Step 4: Profit Pool Analysis (利润池分析)
  ↓
Step 5: Company Screening + Financial Deep Dive (公司筛选 + 财报分析)
  ↓
Step 6: Quantitative Valuation + Catalysts (定量估值 + 催化剂)
```

---

## Step 1: Sub-sector Selection

Goal: Identify sub-sectors with structural change over the next 3–5 years.

Evaluate each sub-sector on **four factors**:

### Factor 1: Demand — Non-linear growth signals
- Technology inflection points (AI, electrification)
- Policy tailwinds (IRA, CHIPS Act, nuclear renaissance)
- Penetration rate acceleration (S-curve positioning)
- Search for: TAM estimates, CAGR projections, adoption curves

### Factor 2: Supply — Constraints that create pricing power
- Resource scarcity (uranium, copper, rare earths)
- Manufacturing bottlenecks (leading-edge chips, transformers)
- Long build cycles that limit new capacity
- This is the most underappreciated factor — supply constraint + demand growth = excess profits

### Factor 3: Competitive Structure (Porter Five Forces)
- CR3/CR5 concentration ratios
- Barriers to entry (capital, IP, regulatory, network effects)
- Threat of substitutes
- Best structure: oligopoly + high barriers

### Factor 4: Profit Distribution — Where value accrues
- Not all parts of a value chain earn profits equally
- Identify which node captures the most margin
- Example: In AI, chips (NVIDIA) >> cloud >> application layer
- **Invest in the profit pool, not the headline industry**

### Sub-sector Universe (reference)

Read `references/sub-sectors.md` for the full sub-sector taxonomy with Tier ratings.

**Scoring**: For each sub-sector, assign 1–5 on each factor. Composite = weighted average.
Present as a ranked table. See `references/scoring-template.md` for weights and rubrics.

### Web Search Strategy for Step 1
Use web search to find:
- Recent industry reports and market sizing (search: "[sub-sector] market size CAGR 2025 2030")
- Supply/demand dynamics (search: "[sub-sector] supply shortage bottleneck")
- Policy catalysts (search: "[sub-sector] policy regulation 2025 2026")
- Concentration data (search: "[sub-sector] market share top companies")

---

## Step 2: Value Chain Decomposition

For the selected sub-sector, map: **Upstream → Midstream → Downstream**

For each node, analyze:

| Dimension | Question |
|-----------|----------|
| Scarcity | Which node is hardest to replicate? |
| CAPEX intensity | Where is capital most concentrated? |
| Standardization | More standardized = less pricing power |
| Margin profile | Gross margin and operating margin by node |
| Switching costs | How sticky are customer relationships? |

**Output**: A value chain diagram (text-based) with margin estimates per node.

### Web Search Strategy for Step 2
- "[industry] value chain analysis"
- "[industry] gross margin by segment"
- "[upstream company] vs [downstream company] profitability"

---

## Step 3: Competitive Landscape

Classify the industry structure:

| Type | Characteristics | Investment Implication |
|------|----------------|----------------------|
| Perfect competition | Low margins, commoditized | Avoid |
| Monopolistic competition | Some differentiation | Pick the leader |
| Oligopoly | High margins, few players | Best — invest here |
| Monopoly | Stable, regulated | Valuation-dependent |

**Key metrics to gather** (via web search + financial data):
1. Market share trends (share gain/loss over 3 years)
2. Gross margin trajectory
3. ROIC stability and level (>15% = strong moat signal)
4. R&D intensity as % of revenue

---

## Step 4: Profit Pool Analysis

Map: **Revenue → Costs → Profit** for each value chain node.

Key questions:
- Who has pricing power? (Can raise prices without losing volume)
- Who is getting squeezed? (Input costs rising, output prices flat)
- Where is the "toll booth"? (Mandatory spend regardless of cycle)

**Output**: A profit pool table showing estimated profit share by node.

---

## Step 5: Company Screening + Financial Deep Dive (UPGRADED)

This step combines web search for qualitative context with **quantitative financial statement
analysis** for the top 3–5 companies identified in the profit pool.

### 5A. Financial Statement Deep Dive + Metric Decomposition (UPGRADED)

This sub-step has two phases: (1) gather financial data, (2) decompose key metrics into
driver formulas and map opportunity/risk for each company.

#### Phase 1: Data Gathering

**If the `financial-statement-analyst` skill is available** (check for
`/mnt/skills/user/financial-statement-analyst/SKILL.md`), read it and use its methodology
to perform rigorous financial analysis on each candidate company.

**Integration Protocol with financial-statement-analyst skill**:
- **Input to pass**: Company name, ticker, reporting period, and a list of metrics to
  prioritize (derived from the value chain analysis in Step 2 — focus on the node where
  the company sits)
- **Output to extract**: Use the skill's framework to produce:
  - `operations.segments` → business line revenue breakdown and drivers
  - `snapshot.core_metrics` → multi-quarter trend data
  - `financial_health.table` → leverage, liquidity, FCF quality metrics
  - `peer_comparison.table` → cross-company benchmarks
- **Cross-company synthesis**: After running the skill for each company individually,
  the industry-research skill is responsible for the horizontal comparison — aligning
  metrics across companies, normalizing for fiscal year differences, and identifying
  relative strengths/weaknesses

**Note**: The financial-statement-analyst skill is designed to analyze uploaded PDFs. In the
industry-research workflow (where PDFs are typically not uploaded), use its **analytical
framework and output structure** as a template, but gather data via web search (SEC EDGAR,
earnings releases, financial data sites). Apply the same rigor: multi-quarter trends,
dual-source verification, source page citations.

**If the skill is not available**, use web search to gather the following data and construct
the analysis manually:

#### Income Statement Analysis (3-year trend required)

| Metric | What to Extract | Source |
|--------|----------------|--------|
| Revenue | 3-year trend + quarterly trajectory | SEC filings, earnings releases |
| Revenue Growth | YoY and QoQ, organic vs. inorganic | Earnings calls |
| Gross Margin | Level + trend direction | 10-K/10-Q |
| Operating Margin | Level + trend + one-time adjustments | 10-K/10-Q |
| Net Income | GAAP vs. adjusted, quality of earnings | Earnings releases |
| EPS | Diluted, beat/miss history (4 quarters) | Consensus estimates |

#### Balance Sheet Health

| Metric | Threshold | Interpretation |
|--------|-----------|---------------|
| Net Debt / EBITDA | <1x excellent, 1–3x acceptable, >3x caution | Leverage risk |
| Current Ratio | >1.5 healthy | Liquidity |
| Cash & Equivalents | Runway in quarters | Burn rate for pre-profit companies |
| Goodwill / Total Assets | >40% = acquisition risk | Impairment exposure |

#### Cash Flow Quality (most important for value investors)

| Metric | What to Look For |
|--------|-----------------|
| Operating Cash Flow | Growing and positive; should exceed net income |
| FCF (Free Cash Flow) | OCF minus CapEx; the true "owner earnings" |
| FCF Margin | FCF / Revenue; >15% = strong, <5% = weak |
| CapEx / Revenue | Is the company over-investing or under-investing? |
| FCF Conversion | FCF / Net Income; >80% = high quality earnings |
| Shareholder Returns | Buybacks + dividends as % of FCF |

#### Industry-Specific KPIs

Identify and track the 2–3 most critical metrics for the specific sub-sector:
- **SaaS**: NRR, Rule of 40, CAC Payback
- **Uranium Mining**: Production volume (lbs U3O8), average realized price, reserve life
- **Nuclear Utilities**: Capacity factor, PPA contract backlog, $/MWh realized price
- **Semiconductors**: Inventory days, book-to-bill ratio, ASP trends
- **GLP-1 Pharma**: Script volume growth, market share, pipeline milestones
- **Medical Devices**: Procedure volumes, installed base growth, consumable attach rate

#### Phase 2: Metric Decomposition & Opportunity/Risk Attribution (NEW — MANDATORY)

After gathering financial data, **decompose each key metric into its driver formula** and
analyze where each company has leverage (opportunity) or exposure (risk).

##### Decomposition Trees (apply relevant ones per industry)

```
Revenue Decomposition:
  Revenue = Volume × ASP × Product Mix Weight
    → Volume: unit shipments, subscribers, procedures, scripts
    → ASP: average selling price, ARPU, price per unit
    → Mix: high-margin vs. low-margin product share

Gross Margin Decomposition:
  Gross Margin = (Revenue - COGS) / Revenue
    → COGS drivers: raw materials, labor, depreciation, yield/utilization
    → Pricing power: ability to pass cost increases to customers
    → Scale effects: fixed cost leverage as volume grows

Operating Margin Decomposition:
  Operating Margin = Gross Margin - (SGA% + R&D% + D&A%)
    → SGA%: sales efficiency, go-to-market leverage
    → R&D%: innovation investment vs. maintenance spend
    → Operating leverage: incremental margin on new revenue

FCF Decomposition:
  FCF = Net Income + D&A - ΔWorking Capital - CapEx
    → CapEx split: growth CapEx (new capacity) vs. maintenance CapEx
    → Working capital: DSO (receivables), DIO (inventory), DPO (payables)
    → Quality check: FCF/Net Income ratio — divergence signals earnings quality issues
```

##### Opportunity/Risk Attribution Matrix (Required Output)

For each of the top 3–5 companies, produce this matrix:

```
| Company | Metric Factor | Current Status | Opportunity (机会点) | Risk (风险点) | Key Catalyst / Threat |
|---------|--------------|----------------|---------------------|--------------|----------------------|
| Co. A   | Volume       | Growing 25% YoY | New product launch in H2 | Customer concentration (top 3 = 60% rev) | Product X ramp |
| Co. A   | ASP          | Flat YoY       | Premium tier adoption | Competitive pricing pressure from Co. B | Next pricing cycle Q3 |
| Co. A   | COGS/Margin  | GM expanding   | Yield improvement ongoing | Input cost (silicon) up 15% | Supply contract renewal |
| Co. B   | Volume       | Declining 5%   | Market share gain if Co. C exits | Regulatory delay on new indication | FDA decision Q4 |
| ...     | ...          | ...            | ...                 | ...          | ...                  |
```

##### Root Cause Tracing (溯源分析)

For each significant metric change (>5% YoY or material deviation from peers), trace back
to the root cause using this chain:

```
Observed: [Metric changed by X%]
  → Direct driver: [Which formula component moved?]
    → Underlying cause: [What business/market/macro event caused it?]
      → Forward implication: [Will this persist, accelerate, or reverse?]
```

Example:
```
Observed: Gross margin expanded +300bps YoY
  → Direct driver: COGS/unit declined 12% while ASP held flat
    → Underlying cause: New manufacturing node reached 85% yield (was 70%)
      → Forward implication: Yield gains are one-time; margin expansion will slow in FY26
```

This root-cause tracing must be performed for **at least the top 3 most impactful metric
movements** for each company. Present findings inline with the company analysis.

### 5B. Cross-Company Comparison Table (Mandatory Output)

After gathering financials, produce a standardized comparison table:

```
| Metric           | Company A | Company B | Company C | Industry Avg |
|-----------------|-----------|-----------|-----------|-------------|
| Revenue (TTM)    |           |           |           |             |
| Revenue Growth   |           |           |           |             |
| Gross Margin     |           |           |           |             |
| Operating Margin |           |           |           |             |
| FCF Margin       |           |           |           |             |
| ROIC             |           |           |           |             |
| Net Debt/EBITDA  |           |           |           |             |
| [KPI 1]          |           |           |           |             |
| [KPI 2]          |           |           |           |             |
| Market Cap       |           |           |           |             |
| P/E (TTM)        |           |           |           |             |
| EV/EBITDA        |           |           |           |             |
```

Highlight the "winner" in each row. Flag any red flags.

### 5C. Competitive Advantage Assessment
- Market share trend (gaining or losing?)
- Technology leadership (patents, R&D pipeline)
- Cost advantage (scale, location, process)
- Network effects or switching costs

### 5D. Management Quality
- Capital allocation track record
- Insider buying/selling patterns (search: "[company] insider transactions")
- Management commentary from recent earnings calls
- Execution history vs. guidance

### 5E. ETF Identification
For each sub-sector, identify the 2–3 most relevant ETFs:
- Search: "[sub-sector] ETF"
- Compare: AUM, expense ratio, top holdings, YTD performance
- Note concentration risk (single stock dominance)

### 5F. Hidden Signals & Contrarian Insights (NEW — MANDATORY)

This sub-step is the analytical differentiator. After completing the standard financial
analysis (5A–5E), step back and actively look for **information that is easily overlooked,
counter-intuitive, or contradicts the prevailing market narrative**.

The goal: surface the insights that a first-pass analyst would miss but a senior PM would
ask about.

#### Five Dimensions to Investigate

| Dimension | What to Look For | Example Signals |
|-----------|-----------------|-----------------|
| **Industry-level** 行业层面 | Structural shifts masked by headline growth | "AI compute demand is 10x-ing but power grid capacity is only growing 2% — who benefits from the bottleneck?" |
| **Competition-level** 竞争层面 | Gaps between market share data and actual competitive health | "Company A has #1 share but NRR is declining and churn is hidden by new logo growth" |
| **Financial-level** 财务层面 | Anomalies, quality issues, or red flags buried in footnotes | "Receivables growing 3x faster than revenue — channel stuffing risk"; "Capitalized R&D inflating margins" |
| **Valuation-level** 估值层面 | Market pricing that doesn't match fundamental reality | "Market assigns SaaS multiple but 60% of revenue is low-margin hardware"; "Sum-of-parts shows hidden value in subsidiary" |
| **Management-level** 管理层层面 | Words vs. actions inconsistency | "CEO talks bullish on earnings call but insiders sold $50M in last 90 days"; "Guidance raised but CapEx guidance cut — contradictory signals" |

#### How to Extract These Signals

1. **Earnings call transcript mining**: Search for "[company] earnings call transcript"
   and look for analyst Q&A sections — the toughest questions often reveal hidden concerns
2. **Footnote scanning**: When reviewing 10-K/10-Q data, pay special attention to:
   - Revenue recognition policy changes
   - Related party transactions
   - Contingent liabilities and legal proceedings
   - Changes in accounting estimates
   - Off-balance-sheet arrangements
3. **Cross-company contradiction**: Compare what Company A says about industry demand vs.
   what Company B says — divergence = someone is wrong, find out who
4. **Quantitative anomaly detection**: Flag any metric where:
   - Growth rate diverges significantly from peers (±2 standard deviations)
   - A historically stable ratio suddenly shifts (e.g., DSO jumps 30%)
   - GAAP vs. non-GAAP gap is widening quarter over quarter
5. **Sentiment vs. fundamentals check**: Compare market narrative / analyst consensus
   tone vs. actual financial trajectory — extreme divergence = potential opportunity

#### Required Output Format

Produce a dedicated section titled **"🔍 Hidden Signals & Contrarian Insights"** with:

```
### 🔍 Hidden Signals & Contrarian Insights

#### Signal 1: [Descriptive title]
- **Dimension**: [Industry / Competition / Financial / Valuation / Management]
- **Observation**: [What did you find?]
- **Why it matters**: [Why is this overlooked? What could it mean?]
- **Evidence**: [Data points + citations with confidence level]
- **Counter-argument**: [Why might this signal be benign / misleading?]
- **Monitoring trigger**: [What future data point would confirm or invalidate this?]

#### Signal 2: ...
(minimum 3 signals per report, covering at least 2 different dimensions)
```

**Minimum requirement**: Every report must contain at least **3 contrarian/hidden signals**,
spanning at least **2 different dimensions** from the table above.

#### Web Search Strategy for 5F
- "[company] earnings call transcript [quarter]" — mine Q&A section
- "[company] accounting change footnote" or "[company] revenue recognition"
- "[company] insider selling buying [year]"
- "[industry] contrarian view" or "[industry] bear case"
- "[company A] vs [company B] demand outlook divergence"

### Web Search Strategy for Step 5
- "[company] 10-K annual report [year]" — primary source, ★★★
- "[company] latest earnings results revenue EPS" — earnings releases, ★★★
- "[company] free cash flow ROIC" — financial data, ★★☆
- "[company] market share [year]" — competitive data, ★★☆
- "[company] insider buying selling transactions" — management signals, ★★☆
- "[sub-sector] ETF list AUM expense ratio" — ETF screening, ★★☆
- "[company] revenue breakdown by segment" — decomposition input, ★★★
- "[company] COGS breakdown cost structure" — margin decomposition, ★★☆
- "[company] earnings call transcript [quarter]" — contrarian signal mining, ★★☆
- "[company] accounting change footnote" — hidden signal detection, ★★★
- "[industry] bear case contrarian" — contrarian perspectives, ★☆☆

---

## Step 6: Quantitative Valuation + Catalysts (UPGRADED)

Value investing is not "buy cheap" — it is **buy undervalued + catalyst for re-rating**.
This step provides a structured, quantitative valuation framework.

Read `references/valuation-template.md` for the full DCF model template and comps framework.

### 6A. Comparable Company Analysis (Comps)

For each candidate company, build a comps table:

```
| Company    | EV ($B) | Rev ($B) | EV/Rev | EV/EBITDA | P/E  | PEG  | P/FCF |
|-----------|---------|----------|--------|-----------|------|------|-------|
| Company A  |         |          |        |           |      |      |       |
| Company B  |         |          |        |           |      |      |       |
| Company C  |         |          |        |           |      |      |       |
| Peer Avg   |    —    |     —    |        |           |      |      |       |
```

For each company note: premium/discount vs. peer average and vs. own 5-year historical average.

### 6B. Simplified DCF (3-Scenario)

For profitable companies, run a 10-year DCF with Bull / Base / Bear scenarios.
See `references/valuation-template.md` for the full calculation template.

Key inputs:
- Base FCF (TTM, adjusted)
- Growth rate years 1–5 (consensus), years 6–10 (fade to GDP)
- Terminal growth: 2.5%
- WACC: 8–12% depending on risk (see template for guidelines)

**Always present 3 scenarios** to capture uncertainty:

| Scenario | Growth | WACC | Implied Value | Margin of Safety |
|----------|--------|------|--------------|-----------------|
| Bull | Consensus +20% | Low | $____ | ___% |
| Base | Consensus | Mid | $____ | ___% |
| Bear | Consensus -30% | High | $____ | ___% |

Interpretation:
- All 3 show >20% upside → Strong Buy
- Base up but bear >30% down → Buy, size carefully
- Base <10% upside → Fair valued, wait

### 6C. Catalyst Timeline (Required Output)

```
| Timeframe    | Catalyst                                | Probability | Impact |
|-------------|----------------------------------------|-------------|--------|
| 0–3 months  |                                        | H/M/L       | H/M/L  |
| 3–6 months  |                                        | H/M/L       | H/M/L  |
| 6–12 months |                                        | H/M/L       | H/M/L  |
| 12+ months  |                                        | H/M/L       | H/M/L  |
```

### 6D. Timing Signal (Optional — Advanced)
- Reddit/social sentiment as a timing overlay (not idea source)
- Institutional positioning (13F filings)
- Short interest changes

---

## Report Output Format

Generate the report as a **`.docx` file** using the docx skill. Save to
`/mnt/user-data/outputs/` and present via `present_files`.

Structure:

```
# [Sub-sector] Industry Research Report
## Date: [current date]

## Executive Summary
- Sub-sector rating and thesis in 3 sentences
- Top picks with margin-of-safety estimates
- Key risks

## 1. Industry Overview & Selection Rationale
- Industry scoring table (4-factor model)
- Demand / supply / structure analysis

## 2. Value Chain Analysis
- Value chain map with margin estimates

## 3. Competitive Landscape
- Market structure classification
- Moat assessment

## 4. Profit Pool
- Profit distribution across value chain

## 5. Company Deep Dives (Financial Statement Analysis)
- 5A. Financial deep dive with metric decomposition trees
- 5A. Opportunity/Risk attribution matrix per company
- 5A. Root cause tracing for top metric movements
- 5B. Cross-company financial comparison table
- 5C–5D. Competitive advantage + management quality
- 5E. ETF options table
- 5F. 🔍 Hidden Signals & Contrarian Insights (min 3 signals, 2+ dimensions)

## 6. Valuation & Catalysts
- Comps table
- DCF 3-scenario analysis per company
- Catalyst timeline

## 7. Investment Conclusion
- Final recommendation + position sizing
- Key risks to monitor

## Sources & References
- All cited sources grouped by confidence level (★★★ / ★★☆ / ★☆☆)
- Full URLs where available
- Data retrieval dates noted

## Appendix
- Methodology notes, disclaimer
```

---

## Citation & Source Confidence System (MANDATORY)

Every data point, claim, and estimate in the report **must** carry an inline citation with a
confidence rating. This system ensures traceability and lets readers gauge reliability at a glance.

### Inline Citation Format

Use this format immediately after any claim backed by external data:

```
[Source: <source_name> | Confidence: <level> | <URL_or_reference>]
```

Examples:
- `[Source: NVIDIA 10-K FY2025 | Confidence: ★★★ | https://sec.gov/...]`
- `[Source: Bloomberg Intelligence | Confidence: ★★☆ | https://bloomberg.com/...]`
- `[Source: Analyst estimate (Morgan Stanley) | Confidence: ★☆☆ | https://...]`

### Confidence Levels

| Level | Symbol | Meaning | Typical Sources |
|-------|--------|---------|----------------|
| High | ★★★ | Audited / regulatory filing / official data | SEC 10-K/10-Q, company IR, government statistics (EIA, FDA, BLS), patent databases |
| Medium | ★★☆ | Reputable but unaudited / may contain estimates | Bloomberg, Reuters, FactSet consensus, earnings call transcripts, major industry reports (Gartner, IDC, IEA) |
| Low | ★☆☆ | Estimates, projections, or secondary aggregation | Analyst notes, news articles, market commentary, aggregator sites, social media sentiment |

### Source Priority Hierarchy

When multiple sources are available for the same data point, prefer in this order:
1. **Primary / regulatory**: SEC filings, company IR, government databases → ★★★
2. **Authoritative secondary**: Bloomberg, Reuters, FactSet, peer-reviewed research → ★★☆
3. **Tertiary / estimates**: Analyst reports, news articles, industry blogs → ★☆☆

**Rules**:
- Every numerical data point requires at least one citation
- If two sources conflict, cite both, note the discrepancy, and state which is used and why
- When using web search results, **preserve the URL** from the search result for citation
- If a URL is unavailable, cite as `[Source: <name> | Confidence: <level> | URL not available]`
- Never cite forums, Reddit, or unverified social media as factual sources (use only for sentiment)
- Aggregated/derived metrics (e.g., your own calculated ROIC) should cite the underlying inputs

### Report-Level Source Appendix

At the end of every report, include a **Sources & References** section that lists all cited
sources grouped by confidence level:

```
## Sources & References

### ★★★ High Confidence (Regulatory / Official)
1. [Company] 10-K FY20XX — <URL>
2. SEC EDGAR filing — <URL>

### ★★☆ Medium Confidence (Authoritative Secondary)
3. Bloomberg Intelligence, "[title]" — <URL>
4. FactSet consensus estimates, retrieved [date] — <URL>

### ★☆☆ Low Confidence (Estimates / Commentary)
5. [Analyst] report, "[title]" — <URL>
6. [News outlet], "[headline]" — <URL>
```

---

## Key Principles

1. **Always search before asserting** — Financial data, market share, policy updates require verification.
2. **Quantify everything** — "Revenue CAGR of 25%" not "fast-growing market."
3. **Show the framework** — User should follow the reasoning chain.
4. **Flag uncertainty** — Distinguish filings data from estimates. Use ranges.
5. **Cite everything** — Every data point must have an inline citation with confidence level (★★★/★★☆/★☆☆).
6. **Language** — Default Chinese with English financial terms. Full English if requested.
7. **Bias toward action** — Clear conclusion: what to buy, watch, avoid.
8. **Always save output** — Reports must be saved as files, never just chat text.

---

## Handling Different Request Types

| User Request | What to Do |
|-------------|-----------|
| "研究一下XX行业" | Full 6-step pipeline → save as .docx |
| "XX和YY哪个子行业更好" | Step 1 comparison scoring → save as .docx |
| "帮我分析XX公司" | Steps 3–6 (competitive + financials + valuation) → .docx |
| "XX行业的产业链" | Steps 2–4 (value chain + competitive + profit pool) → .docx |
| "给我推荐几个标的" | Steps 5–6 (screening + valuation) → save as .docx |
| "列举值得关注的子板块" | Step 1 scoring across all sub-sectors → save as .docx |

---

## Skill Dependencies

- **docx skill** — For generating professional Word documents (required for output)
- **financial-statement-analyst skill** — For deep financial analysis in Step 5A (optional
  but strongly recommended). If available at `/mnt/skills/user/financial-statement-analyst/`,
  read and follow its instructions for each candidate company.
- **data-scientist skill** — Can be used for quantitative scoring model if user wants to
  build a systematic ranking system (optional)

---

## Reference Files

- `references/sub-sectors.md` — Full sub-sector taxonomy with Tier ratings and key companies
- `references/scoring-template.md` — Quantitative scoring framework for industries and companies
- `references/valuation-template.md` — DCF model template, comps framework, WACC guidelines
