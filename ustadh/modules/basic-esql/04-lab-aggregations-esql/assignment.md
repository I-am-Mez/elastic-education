---
slug: lab-aggregations-esql
id: hrxpyjvf8kto
type: challenge
title: 'Lab: Aggregations with ES|QL'
tabs:
- id: wwov7u9nto4h
  title: Kibana
  type: service
  hostname: kubernetes-vm
  port: 30001
- id: zdshqnnpmtli
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
enhanced_loading: null
---
**Lab: Aggregations with ES|QL**
# In this lab...
You will:
- Use the various functions of the **STATS** command to aggregate and query on data.


Aggregating with ES|QL
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create ES|QL queries that do the following:
- Use the various aggregation functions of the **STATS** command to aggregate and analyze data
- Combine **EVAL** with **STATS** to group and count by computed fields

Using the above commands, answer the following:
## Q1: Create a query, using the `logs-*` index pattern, to pull the day of the week out of each `@timestamp` field. Then, get a count of how many logs were created on each day of the week. Which day of the week number had the most logs generated?
## Q2: Create a query, using the `logs-*` index pattern, to find the highest `source.bytes` connection. How many bytes were sent in this connection?
## Q3: Create a query, using the `logs-*` index pattern, to find the average amount of `source.bytes` across our logs. How many bytes are sent, on average?
## Q4: Create a query, using the `logs-*` index pattern, to find where the `source.bytes` field is populated, then find the average amount of `source.bytes` across our logs. How many bytes are sent, on average?
## Q5: Why are the two numbers the same across the previous two questions?

> [!NOTE]
> You may need to use the [ES|QL aggregations documentation](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/aggregation-functions).


Hints
===

- `DATE_EXTRACT("day_of_week", @timestamp)` extracts the day of the week as a number (1 = Monday)
- `MAX()` returns the highest value of a field across all documents
- `AVG()` returns the mean value of a field across all documents
- Adding `| SORT count DESC` after a `STATS BY` will surface the highest count first

Correct query
===

The following queries will provide you the answers:

**Q1:**
```copy
FROM logs-*
| EVAL dayOfWeek = DATE_EXTRACT("day_of_week", @timestamp)
| STATS count = COUNT(*) BY dayOfWeek
| SORT count DESC
```

**Q2:**
```copy
FROM logs-*
| STATS MAX(source.bytes)
```

**Q3:**
```copy
FROM logs-*
| STATS AVG(source.bytes)
```

**Q4:**
```copy
FROM logs-*
| WHERE source.bytes IS NOT NULL
| STATS AVG(source.bytes)
```

Solutions
===

A1: 1

A2: 15,990,169,270

A3: 44,520,687.762

A4: 44,520,687.762

A5: The STATS command will only look at logs where `source.bytes` is populated, so the `IS NOT NULL` command is redundant.

Conclusion
===
In this lab, you used different functions from the **STATS** command to aggregate and query on data.

For further information, refer to the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).