---
slug: lab-data-manipulation-esql
id: fhj1mnuwzyso
type: challenge
title: 'Lab: Data manipulation with ES|QL'
tabs:
- id: 7gmknlwn2zmy
  title: Kibana
  type: service
  hostname: kubernetes-vm
  port: 30001
- id: kph6mk13aoea
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
enhanced_loading: null
---
**Lab: Data manipulation with ES|QL**
# In this lab...
You will:
- Use the **WHERE** command to filter for matches on data
- Use the **SORT** command to order query results
- Use the **KEEP** command to reduce fields in displayed results
- Use the **LIMIT** command to reduce displayed results


Manipulation data with ES|QL
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create ES|QL queries that do the following:
- Use the **WHERE** command to filter for specific field values
- Use the **KEEP** command to show only certain fields in results
- Use the **SORT** command to order results by a field value
- Use the **LIMIT** command to control the number of results returned

Using the above commands, answer the following:
## Q1: Create a query, using the `logs-*` index pattern, to find where `file.name` is `Trickbot.exe`. How many results return?
## Q2: Using the same query from question 1, add a command to show only the following fields: `file.name`, `user.name`, and `file.path`. Which user is opening `Trickbot.exe`?
## Q3: Create a query, using the `logs-*` index pattern, to find instances where `url.domain` has data populated in it. Then add a command to only show the `url.domain`, `source.bytes`, and `destination.bytes` fields. Finally, add a command to sort the results, with the largest values of `source.bytes` at the top. What is the highest value for `source.bytes`?
## Q4: Using the query from the previous question, remove the 3rd and 4th lines, then add a command to limit results to 100,000. How many results do you get?
## Q5: Why would we get only 10,000 documents if we set a limit of 100,000?

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- Use `==` for exact string matching in `WHERE` conditions
- Use `IS NOT NULL` to check if a field contains data
- The `SORT` command defaults to ascending order; use `DESC` for descending
- The `LIMIT` command accepts a number, but ES|QL enforces a maximum supported limit

Correct query
===

The following queries will provide you the answers:

**Q1:**
```copy
FROM logs-*
| WHERE file.name == "Trickbot.exe"
```

**Q2:**
```copy
FROM logs-*
| WHERE file.name == "Trickbot.exe"
| KEEP file.name, user.name, file.path
```

**Q3:**
```copy
FROM logs-*
| WHERE url.domain IS NOT NULL
| KEEP url.domain, source.bytes, destination.bytes
| SORT source.bytes DESC
```

**Q4:**
```copy
FROM logs-*
| WHERE url.domain IS NOT NULL
| LIMIT 100000
```

Solutions
===

A1: 4

A2: MSSQL_svc

A3: 2,698

A4: 10,000

A5: 10,000 is the highest limit that ES|QL supports.

Conclusion
===
In this lab, you used the **WHERE**, **SORT**, **KEEP**, and **LIMIT** ES|QL commands to manipulate your data.

For further information, refer to the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).