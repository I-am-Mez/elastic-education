---
slug: lab-5-date-diff
id: hrpkzy1ox1bd
type: challenge
title: 'Lab 5: DATE_DIFF'
tabs:
- id: adkixyvacbmn
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: 1pxoccfdainp
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
enhanced_loading: null
---
**Lab 5: DATE_DIFF**
# In this lab...
You will:
- Use the **DATE_DIFF** function in order to identify the duration between security events to determine attack timelines.

DATE_DIFF
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create an ES|QL query that does the following:
- Searches the `logs-*` index pattern
- Finds where `file.name` ends with `ickbot.exe`
- Creates new fields for the first AND last time each individual `file.name` was seen
- Finds the difference, in hours, between the first and last instance of each `file.name` being seen
- Displays only the file name, first and last seen times, and the difference between the two

Using the above query, answer the following:
## Q1: How many hours went by between the first and last execution of `Ttickbot.exe`?
## Q2: How many minutes went by between the first and last execution of `Tsickbot.exe`? (You will have to change your query)
## Q3: How many seconds went by between the first and last execution of `Trickbot.exe`? (You will have to change your query)

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- Like you used in a previous section with `STARTS_WITH()`, the inverse would be `ENDS_WITH()`
- A minimum value of `@timestamp` would be the first time something is seen
- You may need to change the **unit** of time to find each question

Correct query
===

The following query will provide you the answers:

```copy
FROM logs-*
| WHERE ENDS_WITH(file.name, "ickbot.exe")
| STATS first_seen = MIN(@timestamp), last_seen = MAX(@timestamp) BY file.name
| EVAL difference = DATE_DIFF("hours", first_seen, last_seen)
| KEEP file.name, first_seen, last_seen, difference
```

Solutions
===

A1: 23

A2: 1

A3: 12

Conclusion
===
In this lab, you used the **DATE_DIFF** function in order to identify the duration between security events to determine attack timelines.

For further information, refer to the [DATE_DIFF documentation](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/date-time-functions/date_diff).