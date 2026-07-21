---
description: "Use when the learner is working on Lab 2 — DISSECT and GROK, or asks how to extract structured fields from raw, unformatted log data using ES|QL."
---

# Lab 2: DISSECT and GROK

## Beat 1 — Explain

In 3-4 sentences:

> DISSECT and GROK are both ES|QL functions for extracting structured data out of raw string fields — but they work differently. DISSECT uses a simple delimiter-based pattern (fast, no regex), while GROK uses named Logstash-style patterns that handle variable-length content and complex structures. In security, this matters when your raw logs contain OS names, file paths, or user agent strings that haven't been parsed at ingest.

Close with: _"We'll use DISSECT to pull OS info from user agent strings, then GROK to extract file names from raw Windows paths — ready?"_

## Beat 2 — DISSECT Challenge

Direct the learner to **Discover** or **Timelines** using the **BLISTER** time picker.

**Part 1 — DISSECT:** Build a query that:
- Searches `logs-*`
- Filters for `user_agent.original` starting with `Mozilla/4.0`
- Uses DISSECT to extract everything after the first space into a new field called `os`
- Counts results per `os`, sorted ascending

**DISSECT hints (reveal one at a time):**
1. You'll need `STARTS_WITH()` to filter by user agent prefix.
2. The DISSECT pattern captures the OS string inside the parentheses: `"Mozilla/4.0 (%{os})"`.

**DISSECT solution** (share after hints exhausted):

```esql
FROM logs*
| WHERE STARTS_WITH(user_agent.original, "Mozilla/4.0")
| DISSECT user_agent.original "Mozilla/4.0 (%{os})"
| STATS x = COUNT(*) BY os
| SORT x ASC
```

Questions 1 and 2 from this result:
- **Q1:** What is the operating system? → **Windows 7**
- **Q2:** Why may this be a concern? → **Older operating systems are unsupported and do not receive security updates, which could introduce a vulnerability.**

## Beat 3 — GROK Challenge

**Part 2 — GROK:** Build a query that:
- Searches `logs-*`
- Filters for rows where `file.path` is populated
- Uses GROK to match a Windows path pattern (`C:\dir\subdir\...\file.ext`) and extract file name and drive letter
- Keeps only file name and drive, then counts occurrences

**GROK hints (reveal one at a time):**
1. Use the **GROK Debugger** in Kibana Dev Tools to test your pattern before putting it in ES|QL.
2. The pattern needs to handle a variable number of subdirectories — look for a GROK pattern that matches any characters between backslashes.

**GROK solution** (share after hints exhausted):

```esql
FROM logs-*
| WHERE file.path IS NOT NULL
| GROK file.path """%{WORD:drive}:\\(?<directory_structure>.*)\\(?<fileName>[0-9A-z]*\..*)"""
| KEEP fileName, drive
| STATS count = COUNT(*) BY fileName
| SORT count DESC
```

Questions 3 and 4 from this result:
- **Q3:** What is the most commonly accessed file? → **store.db-journal**
- **Q4:** Why could we still see null results even after filtering `file.path IS NOT NULL`? → **Linux files would not populate because the GROK pattern was written for a Windows file path structure.**

## Hard rules

- Reveal hints one at a time; do not combine them in a single message.
- For GROK specifically, encourage the learner to use the GROK Debugger before writing the ES|QL query — reinforce the workflow, not just the answer.
- After the lab, ask: _"When would you choose GROK over DISSECT?"_ The answer: when the field has variable-length content or complex structure that delimiters alone can't handle.
