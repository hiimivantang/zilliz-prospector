---
description: Generate a scored Zilliz prospecting list for a target industry/region
argument-hint: <industry> in <region> [count]
allowed-tools: WebSearch, WebFetch, Read, Write, Bash, Glob, Grep, Edit
---

Generate a Zilliz prospecting list using the zilliz-prospector skill.

The user's request is: $ARGUMENTS

Follow the full workflow from the zilliz-prospector skill:
1. Parse the industry/domain, region, and optional prospect count from the arguments
2. Research companies via web search, job portals, LinkedIn, and cloud case studies
3. Score each prospect using the Lead Warmth Scoring Matrix
4. Build the formatted .xlsx spreadsheet with Prospect List, Scoring Matrix, and Summary tabs
5. Generate the outreach companion document with email templates and LinkedIn messages
6. Save both files and present to the user
