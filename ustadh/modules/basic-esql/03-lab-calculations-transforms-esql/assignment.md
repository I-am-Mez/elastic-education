---
slug: lab-calculations-transforms-esql
id: hnazaw4531gh
type: challenge
title: 'Lab: Calculations and transforms with ES|QL'
tabs:
- id: tzzx4ytny5qo
  title: Kibana
  type: service
  hostname: kubernetes-vm
  port: 30001
- id: cxpwk0phcxzy
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
enhanced_loading: null
---
**Lab: Calculations and transforms with ES|QL**
# In this lab...
You will:
- Use the **STATS** command to perform mathematical operations on data.
- Use the **EVAL** command to transform data into more meaningful results.


Math and transformations with ES|QL
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create ES|QL queries that do the following:
- Use the **STATS** command along with aggregation functions to perform mathematical operations on data
- Use the **EVAL** command with date functions to transform timestamp data into more meaningful values

Using the above commands, answer the following:
## Q1: Create a query, using the `logs-*` index pattern, to find the total number of `source.bytes`.
## Q2: Create a query, using the `logs-*` index pattern, to pull the day of the week out of each `@timestamp` field. Then, get a count of how many days of the week we have data. How many different days of the week are there data?

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- The `SUM()` function within `STATS` will total all values of a numeric field
- `DATE_EXTRACT()` can pull specific parts from a timestamp (e.g. `"day_of_week"`)
- `STATS COUNT(*) BY field` groups and counts by a specific field, with one row per unique value

Correct query
===

The following queries will provide you the answers:

**Q1:**
```copy
FROM logs-*
| STATS SUM(source.bytes)
```

**Q2:**
```copy
FROM logs-*
| EVAL dayOfWeek = DATE_EXTRACT("day_of_week", @timestamp)
| STATS count = COUNT(*) BY dayOfWeek
```

Solutions
===

A1: 193,659,918,957,470

A2: 6

Conclusion
===
In this lab, you used the **STATS** and **EVAL** commands to perform calculations on and transform your data.

For further information, refer to the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).