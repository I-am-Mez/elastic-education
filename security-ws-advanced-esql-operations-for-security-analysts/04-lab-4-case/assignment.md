---
slug: lab-4-case
id: nsfqvkaaznzl
type: challenge
title: 'Lab 4: CASE'
tabs:
- id: 7vl3q0jx3zzb
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: fw10xqmfbytu
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
enhanced_loading: null
---
**Lab 4: CASE**
# In this lab...
You will:
- Use **CASE** functions in order to perform conditional queries based on security data to identify key network infrastructure information.

CASE
===
# Parameters:
- Utilize the **BLISTER** quick time picker
- Run your ES|QL query in either **Discover** or **Timelines**

# Challenge:
Create an ES|QL query that does the following:
- Searches the `logs-*` index pattern
- Uses a CASE function in order to create a new field called `OS` that uses the `file.path` field to determine if a device is a **Windows** or **Unix** device
- Calculates the amount of times each `OS` appears

Using the above query, answer questions in **Lab 4** in the [button label="Questions"](tab-1) tab.

> [!IMPORTANT]
> You **MUST** use the `file.path` field for this, using other fields will return different results.

> [!NOTE]
> You may need to use the [ES|QL documentation](https://www.elastic.co/docs/reference/query-languages/esql).


Hints
===

- All of our Windows devices appear to only be using a **C: drive**
- Unix directory structure always starts with a **/**

Solution
===

The following query will provide you the answers in **Lab 4**:

```copy
FROM logs-*
| EVAL OS = CASE(
  STARTS_WITH(file.path, "C"), "Windows",
  STARTS_WITH(file.path, "/"), "Unix"
)
| STATS X = COUNT(OS) by OS
```


Conclusion
===
In this lab, you used **CASE** functions in order to perform conditional queries based on security data to identify key network infrastructure information.

For further information, refer to the [CASE documentation](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/conditional-functions-and-expressions/case).