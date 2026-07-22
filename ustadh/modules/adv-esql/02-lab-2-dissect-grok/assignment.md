---
slug: lab-2-dissect-grok
id: gt5jsljufodb
type: challenge
title: 'Lab 2: DISSECT and GROK'
tabs:
- id: a6of69wv9mcm
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: qdxonqpnomzs
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
enhanced_loading: null
---
**Lab 2: DISSECT and GROK**
# In this lab...
You will:
- Use **DISSECT** and **GROK** to extract data from formatted fields in order to highlight key security information

DISSECT
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create an ES|QL query that does the following:
- Searches the `logs-*` index pattern
- Uses the **DISSECT** function to search for where `user_agent.original` begins with `Mozilla/4.0`. Anything after the **FIRST SPACE** group into a new field called `os`
- Finds a count of results per each `os`
- Sorts the results in ascending order

Using the above query, answer the following:
## Q1: What is the operating system?
## Q2: Why is this something we *may* need to worry about?

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


DISSECT Hints
===

- The `STARTS_WITH()` function will be needed
- This lab will require you to know a bit about user agent fields and what they mean. Or you could search for what they mean...

DISSECT Correct query
===

The following query will provide you the answers:

```copy
from logs*
| WHERE STARTS_WITH(user_agent.original, "Mozilla/4.0")
| DISSECT user_agent.original "Mozilla/4.0 (%{os})"
| STATS x = COUNT(*) by os
| SORT x ASC
```

DISSECT Solutions
===

A1: Windows 7

A2: Older browsers are un-supported and as such do not get security updates; this could introduce a vulnerability.

GROK
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create an ES|QL query that does the following:
- Searches the `logs-*` index pattern
- Searches for where the `file.path` is populated with data
- Uses `GROK` to find a pattern in `file.path` that matches the following, while handling for variable amounts of sub-directories:
```copy
<drive_letter>:\<directory>\<sub-directory1>\<sub-directory#>\fileName.ext
```
- Shows only the file name and drive
- Summarizes the amount of each file name

Using the above query, answer the following:
## Q3: What is the most commonly accessed file according to our query?
## Q4: Why could we still see null results for this query even after searching for where `file.path` is not null?

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


GROK Hints
===

- You may want to use the **GROK Debugger** found in **Dev tools** to test your GROK pattern
- There are many GROK pattern resources to be found on the internet if you are needing a reference

GROK Correct query
===

The following query will provide you the answers:

```copy
FROM logs-*
| WHERE file.path IS NOT NULL
| GROK file.path """%{WORD:drive}:\\(?<directory_structure>.*)\\(?<fileName>[0-9A-z]*\..*)"""
| KEEP fileName, drive
| STATS count = COUNT(*) by fileName
| SORT count DESC
```

GROK Solutions
===

A3: store.db-journal

A4: Linux files would not populate here since we used a GROK pattern made for windows file structure

Conclusion
===
In this lab, you used **DISSECT** and **GROK** to extract data from formatted fields in order to highlight key security information.

For further information, refer to the [DISSECT documentation](https://www.elastic.co/docs/reference/query-languages/esql/commands/dissect) and [GROK documentation](https://www.elastic.co/docs/reference/query-languages/esql/commands/grok).