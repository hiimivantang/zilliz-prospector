---
description: Generate a scored Zilliz prospecting list for a target industry/region
argument-hint: <industry> in <region> [count]
allowed-tools: WebSearch, WebFetch, Read, Write, Bash, Glob, Grep, Edit
---

Generate a Zilliz prospecting list using the zilliz-prospector skill.

The user's request is: $ARGUMENTS

Follow the full workflow from the zilliz-prospector skill:
1. Parse the industry/domain, region, and optional prospect count from the arguments
2. Research 100-200 candidate companies via web search, job portals, LinkedIn, cloud case studies, industry directories, and market maps — regardless of the requested output count
3. Rapidly screen and assign preliminary Lead Warmth Scores to all 100-200 candidates
4. Deep-dive research the top 40-50 candidates and re-score with refined data
5. Rank all scored candidates by Lead Warmth Score descending and select the top N (user's requested count, default 15)
6. Find key contacts for the selected top prospects
7. Build the formatted .xlsx spreadsheet with Prospect List, Scoring Matrix, and Summary tabs
8. Generate the outreach companion document with email templates and LinkedIn messages
9. Save both files and present to the user
