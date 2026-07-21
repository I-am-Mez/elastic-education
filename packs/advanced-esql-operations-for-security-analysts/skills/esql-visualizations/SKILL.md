---
description: "Use when the learner is working on Lab 6 — ES|QL Visualizations, or asks how to build a Lens visualization panel using an ES|QL query in Kibana."
---

# Lab 6: ES|QL Visualizations

## Beat 1 — Explain

In 3-4 sentences:

> Kibana's Lens editor supports ES|QL as a query source, letting you build dashboards directly from ES|QL pipelines without needing to define index patterns or aggregations separately. You write the same query syntax you've been using all module, then choose a chart type and map columns to visual axes. This closes the loop between hunting and reporting: the same query that finds a threat can become the panel that monitors for it.

Close with: _"You'll build a visualization that surfaces high-priority hosts receiving external traffic — it builds on the same query from Lab 1. Ready?"_

## Beat 2 — Challenge

In Kibana, create a new **Dashboard** and add an **ES|QL Visualization** panel. Use the **BLISTER** time picker.

Build a query that:
- Searches `logs-*`
- Joins `asset-criticality-lookup` on `host.name`
- Filters for external source IPs
- Removes null `network.protocol` values
- Keeps only `host.name`, `network.protocol`, and `asset.criticality`
- Summarizes the count of logs per `host.name` and `network.protocol`

**Hint:** This query is very similar to Lab 1 — the key addition is a final `STATS COUNT(*) BY host.name, network.protocol`.

## Beat 3 — Solution

```esql
FROM logs-*
| LOOKUP JOIN asset-criticality-lookup ON host.name
| WHERE asset.criticality IN ("extreme_impact", "high_impact")
| WHERE NOT CIDR_MATCH(source.ip, "10.0.0.1/8", "192.168.0.1/16", "172.16.0.1/12", "127.0.0.1/8")
| WHERE network.protocol IS NOT NULL
| KEEP network.protocol, host.name, asset.criticality
| STATS COUNT(*) BY host.name, network.protocol
```

After they build the visualization, ask them to answer the lab questions by reading the chart:

- **Q1:** How many high impact or extreme impact devices were connected to by external network ranges? → **1**
- **Q2:** What is the name of that device? → **web02**
- **Q3:** How many http connections did the device see? → **5,946**

## Hard rules

- The learner must create an actual visualization panel on a dashboard — do not accept just running the query in Discover.
- Confirm they can read the answer from the chart itself, not just the raw query output.
- After the lab, ask: _"How would you use this visualization operationally — what would you monitor, and what threshold would trigger an investigation?"_
