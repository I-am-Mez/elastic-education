---
slug: lab-6-esql-visualizations
id: lxv08ia8ttuc
type: challenge
title: 'Lab 6: ES|QL Visualizations'
tabs:
- id: mspvywjepwdf
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: bmqfmtuhumbz
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
enhanced_loading: null
---
**Lab 6: ES|QL Visualizations**
# In this lab...
You will:
- Create a visualization using ES|QL to highlight key security data.

ES|QL Visualization
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Create a new **ES|QL Visualization** by adding a panel to a new dashboard

# Challenge:
Create an ES|QL visualization that does the following:
- Searches the `logs-*` index pattern
- Joins data from the `asset-criticality-lookup` index, using `host.name` as the match field
- Finds where external IP ranges are the `source.ip`
- Removes **null** values in `network.protocol`
- Shows only `host.name`, `network.protocol`, and `asset.criticality` in the final results
- Summarizes the amount of logs per `host.name` and `network.protocol`

Using the above visualization, answer questions in **Lab 6** in the [button label="Questions"](tab-1) tab.

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- This query will be *very* similar to the query used in **Lab 1**

Solution
===

The following query will provide you the answers in **Lab 6**:

```copy
from logs-*
| LOOKUP JOIN asset-criticality-lookup ON host.name
| WHERE asset.criticality IN ("extreme_impact", "high_impact")
| WHERE NOT CIDR_MATCH(source.ip, "10.0.0.1/8", "192.168.0.1/16", "172.16.0.1/12", "127.0.0.1/8")
| WHERE network.protocol IS NOT NULL
| KEEP network.protocol, host.name, asset.criticality
| STATS COUNT(*) by host.name, network.protocol
```


Conclusion
===
In this lab, you created a visualization using ES|QL to highlight key security data.

For further information, refer to the [ES|QL Visualization documentation](https://www.elastic.co/docs/explore-analyze/visualize/esorql).