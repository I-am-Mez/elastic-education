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

Using the above query, answer questions **1 and 2** in **Lab 2** in the [button label="Questions"](tab-1) tab.

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


DISSECT hints
===

- The `STARTS_WITH()` function will be needed
- This lab will require you to know a bit about user agent fields and what they mean. Or you could search for what they mean...

DISSECT solution
===

The following query will provide you the answers in **Lab 2 - Questions 1 and 2**:

```copy
from logs*
| WHERE STARTS_WITH(user_agent.original, "Mozilla/4.0")
| DISSECT user_agent.original "Mozilla/4.0 (%{os})"
| STATS x = COUNT(*) by os
| SORT x ASC
```

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

Using the above query, answer questions **3 and 4** in **Lab 2** in the [button label="Questions"](tab-1) tab.

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


GROK hints
===

- You may want to use the **GROK Debugger** found in **Dev tools** to test your GROK pattern
- There are many GROK pattern resources to be found on the internet if you are needing a reference

GROK solution
===

The following query will provide you the answers in **Lab 2 - Questions 3 and 4**:

```copy
FROM logs-*
| WHERE file.path IS NOT NULL
| GROK file.path """%{WORD:drive}:\\(?<directory_structure>.*)\\(?<fileName>[0-9A-z]*\..*)"""
| KEEP fileName, drive
| STATS count = COUNT(*) by fileName
| SORT count DESC
```

Conclusion
===
In this lab, you used **DISSECT** and **GROK** to extract data from formatted fields in order to highlight key security information.

For further information, refer to the [DISSECT documentation](https://www.elastic.co/docs/reference/query-languages/esql/commands/dissect) and [GROK documentation](https://www.elastic.co/docs/reference/query-languages/esql/commands/grok).