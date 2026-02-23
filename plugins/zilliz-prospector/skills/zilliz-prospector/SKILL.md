---
name: zilliz-prospector
description: "Generate scored prospecting lists for Zilliz (Milvus vector database) targeting a user-specified industry, domain, or region. Use this skill whenever the user asks to find prospects, build a prospecting list, identify target accounts, research companies for outreach, or generate leads for Zilliz. Also trigger when the user mentions 'prospecting', 'target accounts', 'lead list', 'ICP companies', or asks 'who should we sell to in [industry/region]'. The skill researches companies via web search, job portals, LinkedIn, and cloud case studies, then produces a scored .xlsx spreadsheet with tiered prospects, a scoring matrix tab, a summary tab, plus a companion outreach document with email templates and LinkedIn messages."
---

# Zilliz Prospector

You are building a high-quality prospecting list for Zilliz, whose core product is **Milvus** (open-source vector database) and **Zilliz Cloud** (fully managed version). Your job is to find companies in a user-specified industry/domain/region that have a strong fit for vector search, then rank and score them so a sales team can prioritize outreach.

## What Makes a Great Prospecting List

A great prospecting list isn't just company names — it's an intelligence brief. Each row should tell a sales rep *why* this company needs Zilliz, *what evidence* supports that, *who* to contact, and *how* to open the conversation. The difference between a mediocre list and a great one is the depth of research and specificity of the pitch angles. Generic entries like "they could use AI" are useless. Entries like "their job posting for a Senior Search Engineer explicitly requires 'vector embeddings at billion scale'" are what make a list actionable.

## Step 1: Understand the Request

Extract from the user's prompt:

- **Industry / Domain**: e.g., "legaltech in APAC", "e-commerce in Southeast Asia", "healthcare AI in ANZ"
- **Region** (if specified): geographic focus
- **Number of prospects to deliver**: how many the user wants in the final output (default 15, user can request any number)
- **Scoring overrides**: if the user wants to adjust the default scoring weights
- **Specific angles**: e.g., "companies using OpenSearch", "companies building RAG pipelines"

If the request is vague (e.g., "find me some prospects"), ask which industry/domain and region to focus on before proceeding.

> **Critical: Wide-Funnel Approach** — Regardless of how many prospects the user requests in the final deliverable, you MUST always research and identify **100 to 200 candidate companies** during the research phase (Step 2). All candidates are then scored (Step 3), ranked by Lead Warmth Score descending, and only the **top N** (the user's requested count) are included in the final spreadsheet and outreach document. This ensures the delivered prospects are the highest-quality matches from a large candidate pool, not just the first ones found.

## Step 2: Research

This is the heart of the skill. Use web search extensively — expect to run many searches per prospect.

### 2a. Identify 100-200 Candidate Companies

Cast a wide net. Your goal is to build an initial list of **100 to 200 candidate companies** before any scoring or filtering. Search aggressively across multiple angles:

1. **Industry landscape**: "top [industry] companies in [region]", "[industry] startups [region] funding", "[industry] market leaders [region]", "[industry] unicorns [region]"
2. **Tech stack signals**: "[industry] companies using OpenSearch/Elasticsearch/vector search", "[industry] companies using embeddings", "[industry] AI search [region]"
3. **AI adoption signals**: "[company] AI features", "[company] machine learning", "[industry] GenAI [region]", "[industry] companies building AI products [region]"
4. **Cloud case studies**: Search AWS, GCP, Azure case study pages filtered by region/industry — these often reveal exact infrastructure
5. **Funding/growth signals**: Recent funding rounds, IPOs, rapid hiring indicate budget and ambition
6. **Industry directories and rankings**: Crunchbase lists, G2/Gartner quadrants, industry association member lists, "top 100 [industry] companies" lists
7. **Accelerator/incubator portfolios**: YC, Sequoia, a16z portfolios filtered by industry/region
8. **Conference attendee/sponsor lists**: Major industry conferences often publish sponsor and speaker company lists

Run **many parallel searches** across these categories. Do not stop at the first page of results — dig through multiple result pages and follow secondary links (e.g., "similar companies" lists, competitor roundups, market maps). Keep a running tally and continue until you have at least 100 candidate company names.

> **Do NOT skip this step or reduce the candidate pool.** Even if the user only wants 10 final prospects, you must still identify 100-200 candidates. The quality of the final list depends entirely on the breadth of the initial funnel.

### 2b. Rapid Screening (All 100-200 Candidates)

For every candidate in your pool, do a quick screening pass to gather enough information for initial scoring:

- **Company website**: What they do, approximate scale, whether AI/search is relevant
- **Quick tech signals**: A few targeted searches for "[company] vector search", "[company] AI", "[company] tech stack"
- **News / press releases**: Recent AI launches, product announcements
- **Crunchbase / funding data**: Funding stage, growth signals

This rapid pass should give you enough to assign a preliminary Lead Warmth Score to each of the 100-200 candidates.

### 2c. Deep-Dive Research (Top Candidates Only)

After preliminary scoring (Step 3a), take the **top 40-50 highest-scoring candidates** and perform deep-dive research:

- **Company website**: Products, scale (users, transactions, data volume)
- **Tech blog / engineering blog**: Architecture decisions, tech stack choices
- **Job postings**: Search "[company] software engineer" on LinkedIn, Indeed, or careers page. Job descriptions reveal specific technologies (Elasticsearch, OpenSearch, pgvector, Pinecone, FAISS), scale requirements, and AI/ML initiatives
- **Cloud provider case studies**: "[company] AWS case study", "[company] GCP case study" — published with company consent, contain confirmed infra details
- **Conference talks**: "[company] engineer [technology] talk" — engineers present architecture
- **News / press releases**: AI launches, product announcements, partnerships
- **Crunchbase / funding data**: Funding stage, valuation, investors

After deep-dive research, re-score these candidates with refined data.

### 2d. Find Key Contacts with LinkedIn Profile Links

For each prospect that will make the final deliverable (top N after final scoring), identify 3-4 ideal contacts:

- **Infrastructure decision-makers**: CTO, VP Engineering, Head of Platform
- **AI/ML leaders**: Head of AI, ML Engineering Manager
- **Publicly vocal engineers**: Quoted in case studies, tech blog authors, conference speakers
- **Recent joiners from Milvus/Zilliz users**: Warm connections

Search: `site:linkedin.com/in "[company name]" [role]`

Provide full LinkedIn profile URLs when findable. If a specific URL can't be found, provide name + title + company for easy manual lookup.

## Step 3: Score, Rank, and Select Top Prospects

Use the **Lead Warmth Scoring Matrix** to assign a 1-10 score. Read the full matrix with anchor descriptions from: `references/scoring-matrix.md`

### Dimensions Summary

| Dimension | Weight | Measures |
|-----------|--------|----------|
| Evidence Strength | 30% | How confirmed is their tech stack/pain? |
| Vector Search / GenAI Fit | 25% | How naturally does their product benefit from vector search? |
| Scale & Pain Probability | 20% | Are they at scale where current infra hits limits? |
| Reachability & Timing | 15% | Can we reach decision-makers? Is timing right? |
| Strategic Value | 10% | Logo value, reference-ability, market signaling |

### Scoring Process

1. Rate each dimension 1-10 using anchors from `references/scoring-matrix.md`
2. Compute: `(E × 0.30) + (V × 0.25) + (S × 0.20) + (R × 0.15) + (SV × 0.10)`
3. Round to nearest integer

### 3a. Preliminary Scoring (All 100-200 Candidates)

After the rapid screening pass (Step 2b), assign a preliminary Lead Warmth Score to every candidate based on the information gathered. This score may be rough — that's fine. The goal is to separate strong candidates from weak ones so deep-dive research is focused on the most promising companies.

### 3b. Final Scoring (After Deep-Dive Research)

After deep-dive research (Step 2c) on the top 40-50 candidates, re-score them with the refined data. These final scores will be more accurate and evidence-backed.

### 3c. Rank and Select Top N

1. **Rank all deep-dived candidates** by final Lead Warmth Score, descending
2. **Select the top N** prospects, where N is the user's requested count (default 15)
3. Break ties by preferring candidates with higher Evidence Strength scores (confirmed > inferred)

### Tier Assignment

- **Tier 1**: Score 7-10 AND at least one confirmed evidence source → Immediate outreach
- **Tier 2**: Score 5-8 OR strong inferred fit with medium evidence → Consultative outreach
- **Tier 3**: Score 3-6 with mostly inferred evidence but clear alignment → Nurture track

> **Note**: Because you are selecting the top N from a pool of 100-200 candidates, the final list should be heavily weighted toward Tier 1 and Tier 2 prospects. If the final list contains mostly Tier 3 prospects, the initial candidate pool was likely too narrow — go back to Step 2a and broaden your searches.

## Step 4: Build the Spreadsheet

Generate the `.xlsx` using the script at `scripts/build_spreadsheet.py`, or using openpyxl directly following the xlsx skill best practices. The spreadsheet has 3 tabs.

### Tab 1: Prospect List

| Column | Header | Content |
|--------|--------|---------|
| A | Rank | Sequential, ordered by score descending |
| B | Tier | "Tier 1" / "Tier 2" / "Tier 3" |
| C | Company | Company name (include parent/subsidiary if relevant) |
| D | HQ | City, Country |
| E | Funding | Public (ticker, market cap) or Private (raised, valuation) |
| F | Product / Focus | 2-3 sentences: what they do, scale, relevant AI features |
| G | Why Zilliz is a Good Fit | Evidence-based argument referencing their actual products/use cases |
| H | Likely Vector DB / Search Infra | Current infra with evidence: "(confirmed via [source])" or "(inferred from [reasoning])" |
| I | Evidence Strength | "HIGH - [source]" / "MEDIUM - [reasoning]" / "LOW - [reasoning]" |
| J | Key Contacts to Reach Out | Names, titles, LinkedIn URLs. 3-4 per company, newline-separated |
| K | Entry Point / Pitch Angle | Specific, actionable pitch referencing their pain points or initiatives |
| L | Lead Warmth Score (1-10) | Weighted score from scoring matrix |

### Formatting

- **Headers**: Bold, dark navy fill (hex: 1F4E79), white text, auto-filter on
- **Tier 1 rows**: Light green fill (hex: E2EFDA)
- **Tier 2 rows**: Light yellow fill (hex: FFF2CC)
- **Tier 3 rows**: Light gray fill (hex: F2F2F2)
- **Column widths**: A=6, B=8, C=25, D=20, E=30, F=50, G=55, H=45, I=30, J=45, K=55, L=12
- **Text wrap**: Columns F, G, H, J, K
- **Freeze panes**: Row 1
- **Row heights**: Minimum 60px for data rows

### Tab 2: Scoring Matrix

Display the 5 dimensions with weights, descriptions, and anchor descriptions for Low (1-3), Medium (4-6), and High (7-10). Pull content from `references/scoring-matrix.md`. Format as a clean reference table.

### Tab 3: Summary

- Total prospects, count per tier
- Average Lead Warmth Score
- Count by evidence level (HIGH / MEDIUM / LOW)
- Key Positioning: Primary Pain Point, Zilliz Differentiator, Region Focus, Social Proof

## Step 5: Generate Outreach Document

Create a markdown document with email templates and LinkedIn messages.

### Email Templates by Tier

**Tier 1** — Direct, evidence-based:
- Subject line referencing their known tech stack or case study
- Body demonstrating homework (their blog post, case study, job posting)
- CTA: technical POC or architecture review

**Tier 2** — Consultative, value-first:
- Subject line referencing industry trend or recent AI initiative
- Body offering insight about vector search in their space
- CTA: discovery call or whitepaper

**Tier 3** — Educational, long-game:
- Subject line about industry trend
- Body sharing a relevant customer story
- CTA: webinar invite or content share

### LinkedIn Messages

Per tier, provide:
- **Connection request** (300 char max): Reference something specific about the person's role or company. Offer value, not a pitch.
- **Follow-up message**: For after connection is accepted. Slightly more detailed, still value-first.

Read `references/outreach-templates.md` for template structures and examples.

## Step 6: Save and Deliver

1. Save spreadsheet: `[industry]_[region]_prospects.xlsx`
2. Save outreach doc: `[industry]_[region]_outreach.md`
3. Present both to user with a brief summary of top findings

## Zilliz Value Props

When writing fit arguments and pitch angles, draw from:

- **Purpose-built vector DB** vs. bolted-on kNN in Elasticsearch/OpenSearch
- **DiskANN** for cost-efficient billion-scale search (vs. memory-only HNSW)
- **Separated storage/compute** for elastic scaling
- **Real-time upserts** without full reindexing (vs. ES/OpenSearch segment merges)
- **Built-in hybrid search** (dense + sparse in one query)
- **Managed Zilliz Cloud** on AWS, GCP, Azure — less ops burden
- **Multi-tenancy** with partition keys for SaaS
- **GPU-accelerated indexing** for faster model iteration
- **SOC 2 Type II, HIPAA** for regulated industries
- **Open source Milvus** (30K+ GitHub stars) reduces lock-in fear

## Reference Customers

Cite when relevant as social proof:
- **Filevine** (legaltech): 3B+ vectors, 20M+ pages daily
- **Salesforce**: Enterprise AI search
- **PayPal**: Fraud detection at scale
- **Roblox**: Gaming recommendations

## Quality Standards

- **Wide funnel, narrow output**: Always research 100-200 candidates, then deliver only the top-ranked prospects. The user gets the cream of the crop, not the first companies you found
- **Depth on finalists**: The top candidates selected for the final list must have deep-dive research. Shallow entries in the delivered list are unacceptable
- **Evidence labeling is critical**: Always mark confirmed vs. inferred. "HIGH - AWS published case study" is immediately credible
- **Pitch angles must be specific**: Reference something concrete about the company
- **LinkedIn profiles save sales time**: Search thoroughly for actual URLs
- **Ranking integrity**: The final list must be sorted by Lead Warmth Score descending. Every prospect in the list should be stronger than every prospect that was cut
