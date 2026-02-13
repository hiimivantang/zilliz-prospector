# Lead Warmth Scoring Matrix

## Overview

Each prospect is scored across 5 dimensions on a 1-10 scale, then combined using weighted averages to produce a final Lead Warmth Score. The score determines prioritization and tier assignment for sales outreach.

## Formula

```
Lead Warmth Score = (Evidence Strength × 0.30)
                  + (Vector Search / GenAI Fit × 0.25)
                  + (Scale & Pain Probability × 0.20)
                  + (Reachability & Timing × 0.15)
                  + (Strategic Value × 0.10)
```

Round the result to the nearest integer (1-10).

---

## Dimension 1: Evidence Strength (Weight: 30%)

**What it measures**: How well-documented and confirmed is the prospect's current tech stack, search infrastructure, and pain points? Higher evidence = less discovery needed, faster sales cycle.

| Score Range | Anchor Description |
|-------------|-------------------|
| **1-3 (Low)** | No public information about tech stack. Inference based only on industry patterns or company size. No job postings, case studies, or tech blog mentions. Example: "They're a large company in [industry], so they probably use Elasticsearch." |
| **4-6 (Medium)** | Some indirect signals: job postings mention relevant technologies, company is known to use a major cloud provider, or industry peers have published architecture details. Reasonable inference but not confirmed. Example: "Job posting for ML Engineer mentions 'search relevance' and 'embedding models'." |
| **7-10 (High)** | Direct, confirmed evidence: published cloud provider case study naming specific technologies, company engineer presented architecture at conference, tech blog post detailing search infrastructure, or job posting explicitly mentioning vector search / Elasticsearch / OpenSearch scaling challenges. Example: "AWS published case study confirms they use OpenSearch Service for 35M searches/day." |

---

## Dimension 2: Vector Search / GenAI Fit (Weight: 25%)

**What it measures**: How naturally does the prospect's core product or business benefit from vector search capabilities? Is vector search a "nice to have" or a "core competitive advantage"?

| Score Range | Anchor Description |
|-------------|-------------------|
| **1-3 (Low)** | Vector search is tangential to their core product. Primary use case is structured data, transactions, or workflows. AI features are marketing-level, not product-core. Example: "Accounting SaaS — could use embeddings for document search but it's not central to the product." |
| **4-6 (Medium)** | Clear use cases for vector search exist but aren't the primary product driver. They have AI features that would benefit from embeddings, or they're building new AI capabilities where vector search fits. Example: "HR platform building an AI assistant for employee policy questions — RAG is a natural fit." |
| **7-10 (High)** | Vector search is central to their core product value. Search, matching, recommendation, or similarity are fundamental to what they sell. Moving from keyword to semantic search is a competitive imperative. Example: "Job matching platform moving from keyword to semantic matching — vector search IS the product upgrade." |

---

## Dimension 3: Scale & Pain Probability (Weight: 20%)

**What it measures**: Is the prospect operating at a scale where current search infrastructure (Elasticsearch, OpenSearch, pgvector) hits known limitations? Larger scale = more pain with bolted-on vector search.

| Score Range | Anchor Description |
|-------------|-------------------|
| **1-3 (Low)** | Small to medium data volumes. Current infrastructure likely sufficient. Under 1M documents, low query volume, or early-stage product. The cost and performance benefits of purpose-built vector search are marginal at this scale. |
| **4-6 (Medium)** | Meaningful scale: millions of documents, thousands of queries per second, or growing data volumes that will stress current infra within 12-18 months. Pain is emerging but may not be acute yet. |
| **7-10 (High)** | Large scale: hundreds of millions to billions of documents/vectors, high QPS, real-time update requirements. At this scale, OpenSearch/ES kNN hits known limitations: memory-heavy HNSW, 3-replica HA costs, segment merge latency during reindexing. The cost and performance case for Zilliz is strong. |

---

## Dimension 4: Reachability & Timing (Weight: 15%)

**What it measures**: Can we actually reach the decision-makers? Is the timing right for a conversation? Recent changes (funding, AI initiative, new CTO, infrastructure migration) create openings.

| Score Range | Anchor Description |
|-------------|-------------------|
| **1-3 (Low)** | No identifiable decision-makers. Large bureaucratic organization with no visible AI initiative. No recent changes or triggers. Cold outreach to generic "info@" or careers page. No warm connections. |
| **4-6 (Medium)** | Decision-makers identifiable (CTO, VP Eng on LinkedIn) but no warm connection or timing trigger. Company is stable — no recent funding, migration, or AI launch. Outreach is possible but may take multiple touches. |
| **7-10 (High)** | Clear decision-makers with public profiles. Recent timing trigger: new funding round, AI product launch, infrastructure migration, new CTO/VP Eng hire, published RFP, or engineer quoted in case study (they're already talking publicly). Warm connections possible via mutual contacts, events, or open-source community. |

---

## Dimension 5: Strategic Value (Weight: 10%)

**What it measures**: Beyond revenue potential, how valuable is this prospect as a logo, reference customer, or market signal? Some customers are worth pursuing even at lower warmth because of their signaling value.

| Score Range | Anchor Description |
|-------------|-------------------|
| **1-3 (Low)** | Small or unknown company. Limited market signaling value. Win wouldn't be referenceable for other prospects. Niche industry without broader applicability. |
| **4-6 (Medium)** | Recognized company in their industry. Win would be useful for vertical-specific sales motions. Moderate brand recognition. Could become a case study for similar companies. |
| **7-10 (High)** | Marquee logo that every other prospect in the region/industry would recognize. Win would unlock doors — "If [Company] uses Zilliz, we should look at it too." Public company, industry leader, or household name. Strong case study potential for marketing. |

---

## Tier Assignment Rules

Tiers combine the Lead Warmth Score with evidence quality for a more nuanced prioritization than score alone:

| Tier | Criteria | Sales Action |
|------|----------|--------------|
| **Tier 1** | Score 7-10 AND at least one confirmed evidence source (published case study, confirmed job posting, tech blog, conference talk) | Immediate outreach. Personalized email referencing specific evidence. Offer POC or architecture review. |
| **Tier 2** | Score 5-8 OR strong inferred fit with medium evidence | Consultative outreach. Lead with industry insights, offer value-first content. Discovery call to validate fit. |
| **Tier 3** | Score 3-6 with mostly inferred evidence but clear product-market alignment | Nurture track. Educational content, webinar invites, newsletter. Revisit when new signals emerge. |

## Scoring Override

Users can optionally adjust the default weights. If custom weights are provided, they must sum to 100%. The dimension definitions and anchor descriptions remain the same regardless of weight adjustments.
