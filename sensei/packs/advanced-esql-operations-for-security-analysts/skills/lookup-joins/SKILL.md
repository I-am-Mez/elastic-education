---
description: "Use when the learner is working on Lab 1 — LOOKUP JOINs, or asks how to enrich ES|QL results by joining data across indices."
---

# Lab 1: LOOKUP JOINs

## Beat 1 — Explain

In 3-4 sentences:

> LOOKUP JOIN lets you enrich a query result by pulling in fields from a secondary index, similar to a left join in SQL. You specify a match field that exists in both the source index and the lookup index — here, `host.name` links network logs to an asset criticality table. This is powerful for threat hunting because you can instantly tag raw events with business context (like criticality) without pre-enriching the data at ingest time.

Close with: _"Ready to build a query that finds high-priority hosts being contacted from external IPs?"_

## Beat 2 — Challenge

Direct the learner to **Discover** or **Timelines** in Kibana, using the **BLISTER** time picker.

The goal is a query that:
- Searches `logs-*`
- Joins `asset-criticality-lookup` on `host.name`
- Filters for `high_impact` or `extreme_impact` hosts
- Filters for external source IPs (not RFC 1918 or loopback)
- Removes null `network.protocol` values
- Keeps only `host.name`, `network.protocol`, and `asset.criticality`

**Hints to offer if the learner is stuck (reveal one at a time):**
1. The field for host impact is `asset.criticality`.
2. External IPs: `WHERE NOT CIDR_MATCH(source.ip, "10.0.0.1/8", "192.168.0.1/16", "172.16.0.1/12", "127.0.0.1/8")`

## Beat 3 — Solution

If the learner asks or has exhausted all hints:

```esql
FROM logs-*
| LOOKUP JOIN asset-criticality-lookup ON host.name
| WHERE asset.criticality IN ("extreme_impact", "high_impact")
| WHERE NOT CIDR_MATCH(source.ip, "10.0.0.1/8", "192.168.0.1/16", "172.16.0.1/12", "127.0.0.1/8")
| WHERE network.protocol IS NOT NULL
| KEEP network.protocol, host.name, asset.criticality
```

After they run it, ask them to answer the lab questions:

- **Q1:** Which `host.name` is being connected to? → **web02**
- **Q2:** What is this host's criticality level? → **high_impact**
- **Q3:** Which protocol appears to be used less by this device? → **dhcpv4**

## Hard rules

- Do not give the full solution immediately — work through hints first.
- Do not explain what CIDR_MATCH does before the learner has tried to figure out external IPs themselves.
- After the lab, ask: _"Why would LOOKUP JOIN be more useful than pre-enriching data at ingest?"_
