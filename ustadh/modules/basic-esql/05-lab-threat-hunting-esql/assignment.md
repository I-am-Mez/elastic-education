---
slug: lab-threat-hunting-esql
id: 6aldbaezr0ml
type: challenge
title: 'Lab: Threat hunting with ES|QL'
tabs:
- id: suf1jvndhljo
  title: Kibana
  type: service
  hostname: kubernetes-vm
  port: 30001
- id: 5pdzxff5r0s2
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
enhanced_loading: null
---
**Lab: Threat hunting with ES|QL**
# In this lab...
You will:
- Use **ES|QL** to perform a threat hunting exercise.


Threat hunting with ES|QL
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create ES|QL queries that do the following:
- Use the **IN** operator to filter for multiple values at once
- Use the **KEEP** command to focus on relevant fields
- Use the **STATS** command to summarize activity per user and file

Using the above commands, answer the following:
## Q1: Create a query, using the `logs-*` index pattern, to find where `file.name` is either `Trickbot.exe`, `Ttickbot.exe`, or `Tsickbot.exe`. How many results do you get?
## Q2: Add a command to your query to show only `file.name` and `user.name`. Which user only shows up 1 time?
## Q3: Add a command to your query to find how many times each user opened each file. How many times did the `SYSTEM` user open `Trickbot.exe`?

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- The `IN` operator matches a field against a list of values: `WHERE field IN ("val1", "val2", "val3")`
- `STATS COUNT(*) BY field1, field2` groups results by multiple fields simultaneously
- Replace the `KEEP` command with `STATS` to aggregate instead of filtering displayed fields

Correct query
===

The following queries will provide you the answers:

**Q1:**
```copy
FROM logs-*
| WHERE file.name IN ("Trickbot.exe", "Ttickbot.exe", "Tsickbot.exe")
```

**Q2:**
```copy
FROM logs-*
| WHERE file.name IN ("Trickbot.exe", "Ttickbot.exe", "Tsickbot.exe")
| KEEP file.name, user.name
```

**Q3:**
```copy
FROM logs-*
| WHERE file.name IN ("Trickbot.exe", "Ttickbot.exe", "Tsickbot.exe")
| STATS count = COUNT(*) BY user.name, file.name
```

Solutions
===

A1: 41

A2: NETWORK SERVICE

A3: 0

Conclusion
===
In this lab, you used **ES|QL** to perform a threat hunting exercise.

For further information, refer to the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).