---
slug: lab-3-inline-stats
id: kvh3wmt4bm3o
type: challenge
title: 'Lab 3: INLINE STATS'
tabs:
- id: xpi1ngzurpwx
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: wxbcedwxucg0
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
enhanced_loading: null
---
**Lab 3: INLINE STATS**
# In this lab...
You will:
- Use **INLINE STATS** in order to aggregate data and perform mathematical operations while retaining data from the source command to identify potential infrastructure risks.

INLINE STATS
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create an ES|QL query that does the following:
- Searches the `logs-*` index pattern
- Searches for where `event.duration` and `network.protocol` are populated
- Finds the average `event.duration` of each `network.protocol`
- Maintains the other fields in each of the logs

Using the above query, answer the following:
## Q1: What is the `network.protocol` associated with the longest average connection duration?
## Q2: What is the `network.protocol` associated with the shortest average connection duration? (You will have to change your query)

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Correct query
===

The following query will provide you the answers:

```copy
FROM logs-*
| WHERE event.duration IS NOT NULL
| WHERE network.protocol IS NOT NULL
| INLINE STATS averageEventDuration = AVG(event.duration) by network.protocol
| SORT averageEventDuration DESC
```

Solutions
===

A1: http

A2: tls

Conclusion
===
In this lab, you used **INLINE STATS** in order to aggregate data and perform mathematical operations while retaining data from the source command to identify potential infrastructure risks.

For further information, refer to the [INLINE STATS documentation](https://www.elastic.co/docs/reference/query-languages/esql/commands/inlinestats-by).