---
slug: lab-esql-basics
id: ntg4lsgzuj9h
type: challenge
title: 'Lab: ES|QL basics'
tabs:
- id: 2dunc1bw6vpc
  title: Kibana
  type: service
  hostname: kubernetes-vm
  port: 30001
- id: exeaqfioybjh
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
enhanced_loading: null
---
**Lab: ES|QL basics**
# In this lab...
You will:
- Use the **FROM** command to pull logs from the correct indices


Getting data with ES|QL
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create ES|QL queries that do the following:
- Use the **FROM** command to retrieve data from the `logs-*` index pattern
- Use the **FROM** command to retrieve data from the `filebeat-*` index pattern

Using the above queries, answer the following:
## Q1: Create a query to pull in data from the `logs-*` index pattern. How many documents are present?
## Q2: Create a query to pull in data from the `filebeat-*` index pattern. How many documents are present?
## Q3: Why are we only seeing 1000 results with the above queries?

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).

Hints
===

- The `FROM` command is used to specify which index to query in ES|QL
- Running a query without a `LIMIT` command still applies a default result cap

Correct query
===

The following queries will provide you the answers:

**Q1:**
```copy
FROM logs-*
```

**Q2:**
```copy
FROM filebeat-*
```

Solutions
===

A1: 1,000

A2: 1,000

A3: The default limit for ES|QL queries is 1,000.

Conclusion
===
In this lab, you used the **FROM** command to pull logs from the correct indices

For further information, refer to the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).