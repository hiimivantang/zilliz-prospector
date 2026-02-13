# Zilliz Prospector Plugin

Generate scored prospecting lists for Zilliz (Milvus vector database) targeting any industry, domain, or region.

## What It Does

Given a target industry and region, this plugin researches companies via web search, job portals, LinkedIn, and cloud provider case studies, then produces:

- **Scored spreadsheet** (.xlsx) with ranked, tiered prospects, a scoring matrix tab, and a summary tab
- **Outreach document** (.md) with email templates and LinkedIn messages customized by tier

Each prospect includes: company overview, funding, why Zilliz fits, current search infrastructure (with evidence), key contacts with LinkedIn URLs, specific pitch angles, and a Lead Warmth Score (1-10).

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Skill | `zilliz-prospector` | Core prospecting workflow — research, score, build spreadsheet, generate outreach |
| Command | `/prospect` | Quick invocation: `/prospect legaltech in APAC 15` |

## Usage

### Via Skill (automatic trigger)

Say things like:
- "Find me 15 legaltech prospects in APAC"
- "Build a prospecting list for fintech companies in Southeast Asia"
- "Who should we sell to in the healthcare AI space in ANZ?"

### Via Command

```
/prospect legaltech in APAC
/prospect e-commerce in Southeast Asia 20
/prospect healthcare AI in ANZ 10
```

## Scoring Matrix

Prospects are scored across 5 weighted dimensions:

| Dimension | Weight |
|-----------|--------|
| Evidence Strength | 30% |
| Vector Search / GenAI Fit | 25% |
| Scale & Pain Probability | 20% |
| Reachability & Timing | 15% |
| Strategic Value | 10% |

Weights can be overridden per run by telling Claude (e.g., "use 40% for evidence strength").

## Output Files

- `[industry]_[region]_prospects.xlsx` — the prospecting spreadsheet
- `[industry]_[region]_outreach.md` — email templates and LinkedIn messages
