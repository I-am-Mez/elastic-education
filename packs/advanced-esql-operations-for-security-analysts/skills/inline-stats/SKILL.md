---
description: "Use when the learner is working on Lab 3 — INLINE STATS, or asks how to compute per-group aggregates while keeping individual source rows in ES|QL results."
---

# Lab 3: INLINE STATS

## Beat 1 — Explain

In 3-4 sentences:

> INLINE STATS is like `STATS` but non-destructive — it computes aggregates per group and appends them as new columns to each matching row, rather than collapsing the results into one row per group. This lets you compare individual events against their group average in the same result set, which is useful for anomaly detection. In this lab you'll use it to find the average connection duration per network protocol, helping you spot protocols with unusually long or short sessions.

Close with: _"Want to build the query that computes average connection duration per protocol while keeping all the original log rows?"_

## Beat 2 — Challenge

Direct the learner to **Discover** or **Timelines** using the **BLISTER** time picker.

Build a query that:
- Searches `logs-*`
- Filters for rows where both `event.duration` and `network.protocol` are populated
- Uses INLINE STATS to compute the average `event.duration` per `network.protocol`
- Retains all other fields from each log row
- Sorts by average duration descending

## Beat 3 — Solution

If the learner asks or has exhausted attempts:

```esql
FROM logs-*
| WHERE event.duration IS NOT NULL
| WHERE network.protocol IS NOT NULL
| INLINE STATS averageEventDuration = AVG(event.duration) BY network.protocol
| SORT averageEventDuration DESC
```

After they run it, ask them to answer the lab questions:

- **Q1:** What `network.protocol` has the longest average connection duration? → **http**
- **Q2:** What `network.protocol` has the shortest average connection duration? (requires changing `DESC` to `ASC`) → **tls**

## Hard rules

- Make sure the learner understands the key difference from `STATS`: INLINE STATS keeps all source rows, `STATS` collapses them.
- Don't skip the conceptual explanation — understanding when to use each is more important than the syntax.
- After the lab, ask: _"How would the output differ if you used `STATS` instead of `INLINE STATS` here?"_
