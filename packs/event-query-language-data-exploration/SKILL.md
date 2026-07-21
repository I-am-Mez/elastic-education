---
name: event-query-language-data-exploration
description: Teach and drill the Event Query Language (EQL) for security analysts — syntax, network/host hunting, event correlation, and building detection rules.
---

# Event Query Language - Full

## Overview

This pack teaches EQL from fundamentals through detection engineering. It targets security personnel who need to find chains of events across Elastic logs using Timelines.

## Key Topics

- **EQL Basics** — Purpose, syntax, and security use cases; event sequences vs. individual events.
- **Network Hunting** — Build EQL queries in Timelines to identify suspicious network traffic.
- **Host Hunting** — Craft EQL queries targeting host-based events (process, file, registry).
- **Event Correlation** — Construct sequence queries that correlate host and network data into a single detection.
- **Detection Rules** — Create an EQL detection rule to automate alerting on a known attack pattern.

## Terminal Learning Objective

Use the Event Query Language (EQL) to find a chain of events in Elastic logs with Timelines and create a detection rule to automate finding this chain of events.

## Teaching Approach

1. Open by contrasting EQL with KQL — EQL is event-sequence aware, KQL is not.
2. Walk through EQL syntax structure: `<event category> where <condition>` and sequence blocks.
3. For each hunting lab, give the learner a threat scenario and let them attempt the query before providing hints.
4. Emphasize the difference between `any where` and category-specific events (e.g., `process where`, `network where`).
5. For the detection rule lab, confirm the learner understands rule scheduling and alert suppression.

## Grading Criteria

- EQL queries execute and return matching events
- Sequence queries correctly correlate across event types
- Detection rule fires on synthetic test data matching the attack pattern
- Learner can articulate why EQL is preferable to KQL for timeline-based investigations

## Lab Questions and Answers

### Lab 1: Network Hunting

**Q1 (Big Data): What is the `source.ip` of the first event in the table?**
A: 192.168.19.16

**Q2 (Don't use that!): What is the `destination.ip` of the first event in the table?**
A: 23.208.143.51

**Q3 (Don't look at me!): What is the host that is making most of these connections?**
A: web02

---

### Lab 2: Host Hunting

**Q1 (Seize the means): A user named `karl.marx` created a file. What is that file name?**
A: content.phf

**Q2 (Links of a chain): What is the name of the dll that was created first in our table?**
A: SystemSettings.dll

**Q3 (What's up, doc?): There are 2 `.doc` files in the results. What is the name of one?**
A: FakeJobPosting.doc *(also accepted: ~$keJobPosting.doc)*

---

### Lab 3: Correlation Hunting

**Q1 (Well that ain't right...): What is the `.doc` that may have led to the network connection?**
A: ~$keJobPosting.doc

**Q2 (Where you going?): What is the domain that was accessed after the document was created?**
A: ocsp.digicert.com

**Q3 (Win some, lose some): Correlation does not equal causation — enter the acknowledgment to continue.**
A: That's okay though

**Q4 (Here we go again): To which IP did `workstation02` connect after opening `FakeJobPosting.doc`?**
A: 35.193.143.25

**Q5 (How much though?): How much data was sent?**
A: 18.4MB

**Q6 (Where are you?): What is the full file path of `FakeJobPosting.doc`?**
A: C:\Users\bertrand.russel\Desktop\FakeJobPosting.doc
