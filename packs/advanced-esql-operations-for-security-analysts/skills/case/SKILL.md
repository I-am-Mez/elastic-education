---
description: "Use when the learner is working on Lab 4 — CASE, or asks how to write conditional logic inside an ES|QL query to classify or categorize events."
---

# Lab 4: CASE

## Beat 1 — Explain

In 3-4 sentences:

> The CASE function in ES|QL works like an if-else chain: it evaluates conditions in order and returns the value for the first condition that matches. You use it inside an EVAL command to create a new computed field based on the values of existing fields. In security, this is useful for categorizing raw events — for example, tagging events as "Windows" or "Unix" based on file path patterns so you can count events by OS without relying on a dedicated OS field.

Close with: _"Ready to build a query that tags each event with its OS type using the file path, then counts them?"_

## Beat 2 — Challenge

Direct the learner to **Discover** or **Timelines** using the **BLISTER** time picker.

Build a query that:
- Searches `logs-*`
- Uses a CASE function to create a new field called `OS` based on `file.path`
- Tags events as `Windows` if the path starts with a C drive, `Unix` if it starts with `/`
- Counts how many times each `OS` value appears

> **Important:** Use `file.path` specifically — other fields will return different results.

**Hints (reveal one at a time):**
1. All Windows devices in this dataset use a C: drive — `STARTS_WITH(file.path, "C")` identifies them.
2. Unix directory structure always starts with `/` — `STARTS_WITH(file.path, "/")` identifies those.

## Beat 3 — Solution

If the learner asks or has exhausted attempts:

```esql
FROM logs-*
| EVAL OS = CASE(
    STARTS_WITH(file.path, "C"), "Windows",
    STARTS_WITH(file.path, "/"), "Unix"
  )
| STATS X = COUNT(OS) BY OS
```

After they run it, ask them to answer the lab questions:

- **Q1:** How many events are associated with Windows devices? → **4,232**
- **Q2:** How many events are associated with Unix devices? → **193**

## Hard rules

- Emphasize that CASE evaluates conditions in order and stops at the first match — ordering matters.
- Do not let the learner use a field other than `file.path`; the lab question answers are calibrated to it.
- After the lab, ask: _"What would happen to events where `file.path` doesn't match either condition?"_ (They'd get a null `OS` value.)
