---
slug: lab-1-lookup-joins
id: kcthvqoyh0uo
type: challenge
title: 'Lab 1: LOOKUP JOINs'
tabs:
- id: qcptkvyez5jz
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: 99z9nkdsnqo9
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
enhanced_loading: null
---
**Lab 1: LOOKUP JOINs**
# In this lab...
You will:
- Use **LOOKUP JOIN** to aggregate and query on data from multiple indices


LOOKUP JOIN
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create an ES|QL query that does the following:
- Searches the `logs-*` index pattern
- Joins data from the `asset-criticality-lookup` index, using `host.name` as the match field
- Finds only hosts that are considered `high_impact` or `extreme_impact`
- Finds where external IP ranges are the `source.ip`
- Removes **null** values in `network.protocol`
- Shows only `host.name`, `network.protocol`, and `asset.criticality` in the final results

Using the above query, answer the following:
## Q1: Which `host.name` is being connected to?
## Q2: What is this host's **criticality level**
## Q3: Which protocol appears to be used less by this device?


> [!NOTE]
> The term "external IP ranges" in this case refers to "any ranges that are **not** used as private IP space nor loopback addresses."

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- The field for **host impact** is `asset.criticality`.

- Finding external IP ranges can be done with the following: `WHERE NOT CIDR_MATCH(source.ip, "10.0.0.1/8", "192.168.0.1/16", "172.16.0.1/12", "127.0.0.1/8")`


Correct query
===

The following query will provide you the answers:

```copy
from logs-*
| LOOKUP JOIN asset-criticality-lookup ON host.name
| WHERE asset.criticality IN ("extreme_impact", "high_impact")
| WHERE NOT CIDR_MATCH(source.ip, "10.0.0.1/8", "192.168.0.1/16", "172.16.0.1/12", "127.0.0.1/8")
| WHERE network.protocol IS NOT NULL
| KEEP network.protocol, host.name, asset.criticality
```

Solutions
===

A1: web02

A2: high_impact

A3: dhcpv4

Conclusion
===
In this lab, you used **LOOKUP JOIN** to aggregate and query on data from multiple indices.

For further information, refer to the [LOOKUP JOIN documentation](https://www.elastic.co/docs/reference/query-languages/esql/esql-lookup-join).