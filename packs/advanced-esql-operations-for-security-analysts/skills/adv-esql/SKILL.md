---
name: advanced-esql-operations-for-security-analysts
description: Teach and drill advanced ES|QL commands for security analysts — LOOKUP JOINs, DISSECT/GROK, INLINE STATS, CASE, DATE_DIFF, and Lens visualizations.
---

# Advanced ES|QL Operations for Security Analysts

## Overview

This pack covers advanced ES|QL techniques applied to security use cases. Learners should already be comfortable with basic ES|QL syntax before starting.

## Key Topics

- **LOOKUP JOIN** — Enrich queries by joining data from multiple indices to identify threats on priority devices.
- **DISSECT / GROK** — Extract structured fields from raw, unformatted log data.
- **INLINE STATS** — Perform aggregate math while retaining source-level rows to surface infrastructure risks.
- **CASE** — Write conditional queries that classify security events.
- **DATE_DIFF** — Calculate time between events to reconstruct attack timelines.
- **Lens Visualizations** — Build ES|QL-powered panels to present security findings.

## Terminal Learning Objective

Utilize advanced features of the Elasticsearch Query Language (ES|QL) to conduct security threat investigations.

## Teaching Approach

1. Start by confirming the learner can write a basic `FROM ... | WHERE ... | STATS` pipeline.
2. Introduce each command with a real-world security scenario (e.g., "You need to know which endpoints are high priority — use LOOKUP JOIN against an asset inventory index").
3. Have the learner write a query before showing the solution.
4. After each lab, ask the learner to explain what the command did and why it matters for security.
5. For visualization exercises, verify the learner can interpret the chart output, not just produce it.

## Grading Criteria

- Query runs without errors
- Output contains the expected fields and row count
- Learner can explain the security relevance of the result

## Lab Questions and Answers

### Lab 1: LOOKUP JOINs

**Q1: Which `host.name` is being connected to?**
A: web02

**Q2: What is this host's criticality level?**
A: high_impact

**Q3: Which protocol appears to be used less by this device?**
A: dhcpv4

---

### Lab 2: DISSECT / GROK

**Q1: What is the operating system?**
A: Windows 7

**Q2: Why is this something we may need to worry about?**
A: Older browsers are un-supported and as such do not get security updates; this could introduce a vulnerability.

**Q3: What is the most commonly accessed file according to our query?**
A: store.db-journal

**Q4: Why could we still see null results for this query even after searching for where `file.path` is not null?**
A: Linux files would not populate here since we used a GROK pattern made for windows file structure.

---

### Lab 3: INLINE STATS

**Q1: What is the `network.protocol` associated with the longest average connection duration?**
A: http

**Q2: What is the `network.protocol` associated with the shortest average connection duration? (You will have to change your query)**
A: tls

---

### Lab 4: CASE

**Q1: How many events are associated with Windows devices?**
A: 4,232

**Q2: How many events are associated with Unix devices?**
A: 193

---

### Lab 5: DATE_DIFF

**Q1: How many hours went by between the first and last execution of `Ttickbot.exe`?**
A: 23

**Q2: How many minutes went by between the first and last execution of `Tsickbot.exe`? (You will have to change your query)**
A: 1

**Q3: How many seconds went by between the first and last execution of `Trickbot.exe`? (You will have to change your query)**
A: 12

---

### Lab 6: ES|QL Visualizations

**Q1: How many high impact or extreme impact devices were connected to by external network ranges?**
A: 1

**Q2: What is the name of that device?**
A: web02

**Q3: How many http connections did the device see?**
A: 5,946
