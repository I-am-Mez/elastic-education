---
description: "Use when the learner is working on Lab 5 — DATE_DIFF, or asks how to calculate the duration between two timestamps in an ES|QL query."
---

# Lab 5: DATE_DIFF

## Beat 1 — Explain

In 3-4 sentences:

> DATE_DIFF computes the difference between two datetime values in a unit you specify — hours, minutes, seconds, days, etc. You combine it with MIN and MAX timestamps (via STATS) to find the first and last time an event was seen, then pass those into DATE_DIFF to get the span. In threat hunting, this reconstructs attack timelines: how long did a piece of malware run? How much time passed between infection and exfiltration?

Close with: _"Let's build a query that finds how long Trickbot variants were executing in the environment — want to start?"_

## Beat 2 — Challenge

Direct the learner to **Discover** or **Timelines** using the **BLISTER** time picker.

Build a query that:
- Searches `logs-*`
- Filters for `file.name` values ending with `ickbot.exe` (catches Trickbot, Ttickbot, Tsickbot variants)
- Creates fields for the first and last time each `file.name` was seen (`MIN` and `MAX` of `@timestamp`)
- Calculates the difference **in hours** between those two times
- Keeps only `file.name`, `first_seen`, `last_seen`, and `difference`

**Hints (reveal one at a time):**
1. Just like `STARTS_WITH()`, the inverse is `ENDS_WITH()`.
2. `MIN(@timestamp)` gives you the first time something was seen; `MAX(@timestamp)` gives the last.
3. To answer questions in different units, change the first argument of `DATE_DIFF` — e.g., `"minutes"` or `"seconds"`.

## Beat 3 — Solution

```esql
FROM logs-*
| WHERE ENDS_WITH(file.name, "ickbot.exe")
| STATS first_seen = MIN(@timestamp), last_seen = MAX(@timestamp) BY file.name
| EVAL difference = DATE_DIFF("hours", first_seen, last_seen)
| KEEP file.name, first_seen, last_seen, difference
```

After they run it, ask them to answer the lab questions (each requires changing the time unit in `DATE_DIFF`):

- **Q1:** How many **hours** between first and last execution of `Ttickbot.exe`? → **23**
- **Q2:** How many **minutes** between first and last execution of `Tsickbot.exe`? → **1**
- **Q3:** How many **seconds** between first and last execution of `Trickbot.exe`? → **12**

## Hard rules

- Make sure the learner understands they need to change the `DATE_DIFF` unit themselves for Q2 and Q3 — do not change it for them.
- Emphasize that each question targets a *different* filename variant, not the same one with different units.
- After the lab, ask: _"Why would an attacker run a tool for only 12 seconds?"_ — encourage threat-hunting instincts, not just query mechanics.
