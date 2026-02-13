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
- **Number of prospects**: default 10-15, user can request up to 50
- **Scoring overrides**: if the user wants to adjust the default scoring weights
- **Specific angles**: e.g., "companies using OpenSearch", "companies building RAG pipelines"

If the request is vague (e.g., "find me some prospects"), ask which industry/domain and region to focus on before proceeding.

## Step 2: Research

This is the heart of the skill. Use web search extensively — expect to run many searches per prospect.

### 2a. Identify Candidate Companies

Start broad, then narrow. Search for:

1. **Industry landscape**: "top [industry] companies in [region]", "[industry] startups [region] funding"
2. **Tech stack signals**: "[industry] companies using OpenSearch/Elasticsearch/vector search"
3. **AI adoption signals**: "[company] AI features", "[company] machine learning", "[industry] GenAI [region]"
4. **Cloud case studies**: Search AWS, GCP, Azure case study pages filtered by region/industry — these often reveal exact infrastructure
5. **Funding/growth signals**: Recent funding rounds, IPOs, rapid hiring indicate budget and ambition

Aim to identify 2-3x more candidates than the final list, then filter to strongest fits.

### 2b. Deep-Dive Per Company

For each strong candidate, research:

- **Company website**: Products, scale (users, transactions, data volume)
- **Tech blog / engineering blog**: Architecture decisions, tech stack choices
- **Job postings**: Search "[company] software engineer" on LinkedIn, Indeed, or careers page. Job descriptions reveal specific technologies (Elasticsearch, OpenSearch, pgvector, Pinecone, FAISS), scale requirements, and AI/ML initiatives
- **Cloud provider case studies**: "[company] AWS case study", "[company] GCP case study" — published with company consent, contain confirmed infra details
- **Conference talks**: "[company] engineer [technology] talk" — engineers present architecture
- **News / press releases**: AI launches, product announcements, partnerships
- **Crunchbase / funding data**: Funding stage, valuation, investors

### 2c. Find Key Contacts with LinkedIn Profile Links

For each prospect, identify 3-4 ideal contacts:

- **Infrastructure decision-makers**: CTO, VP Engineering, Head of Platform
- **AI/ML leaders**: Head of AI, ML Engineering Manager
- **Publicly vocal engineers**: Quoted in case studies, tech blog authors, conference speakers
- **Recent joiners from Milvus/Zilliz users**: Warm connections

Search: `site:linkedin.com/in "[company name]" [role]`

Provide full LinkedIn profile URLs when findable. If a specific URL can't be found, provide name + title + company for easy manual lookup.

## Step 3: Score Each Prospect

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

### Tier Assignment

- **Tier 1**: Score 7-10 AND at least one confirmed evidence source
- **Tier 2**: Score 5-8 OR strong inferred fit with medium evidence
- **Tier 3**: Score 3-6 with mostly inferred evidence but clear alignment

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

- **Depth over breadth**: 10 deeply-researched prospects beat 30 shallow ones
- **Evidence labeling is critical**: Always mark confirmed vs. inferred. "HIGH - AWS published case study" is immediately credible
- **Pitch angles must be specific**: Reference something concrete about the company
- **LinkedIn profiles save sales time**: Search thoroughly for actual URLs
- **Drop weak candidates**: If research doesn't yield strong signals, don't pad the list
