---
slug: intro-to-eql
id: vttv8fhgrhiy
type: challenge
title: 'Lesson: Intro to Event Query Language'
teaser: Basics of Event Query Language
notes:
- type: video
  url: https://play.vidyard.com/ZGSuemEhGErurRe6xxsoBu.html
tabs:
- id: 5xhuwrhlpoxr
  title: Learning interface overview
  type: website
  url: https://play.vidyard.com/StrPhsDhCHjnW38Dp5QfJr.html
- id: rzfkwwb3nott
  title: 'Lesson: Intro to Event Query Language'
  type: website
  url: https://play.vidyard.com/sk85VC5Jhtp9XXS5ZtWBGq.html
- id: xnb2ly08gizb
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
difficulty: ""
timelimit: 0
enhanced_loading: null
---
> [!NOTE]
> Watch the [button label="Learning interface overview"](tab-0)(optional).<br><br> To start learning, select [button label="Lesson: Intro to Event Query Language"](tab-1)

In this lesson...
===
You will
- Define Event Query Language (EQL) #add timestamp
- Learn EQL syntax #add timestamp
- Try EQL #add timestamp

> [!NOTE]
> To test out what you saw in the video lesson, select the [button label="Kibana tab"](tab-2)

EQL Syntax
===
**Event Query Language (EQL)** is a query language for event-based time series data (like logs and metrics).

EQL has some requirements to work, such as:
- `@timestamp` field being present
	- EQL is a time-series focused language, so time being present is needed
- `event.category` field must also be populated
	- This may restrict which logging capabilities may leverage EQL for searching
- EQL queries must be performed in the `Correlation` tab of a `Timeline`

The basic syntax of EQL is as follows:

```
  <event.category> where <field> == "<value>"
  ```

This simple approach to querying allows the language to be understood more easily. Let's take a look at a few examples:
<details>
<summary>Example 1: Process Events</summary>

```
  process where process.name == "regsvr32.exe"
  ```

This query will look for process events where the process name is `regsvr32.exe` (a command-line utility used to register DLLs; commonly abused by attackers).

</details>


<details>
<summary>Example 2: File Events</summary>

```
  file where file.name == "ttickbot.exe"
  ```

This query will look for file events where the file name is `ttickbot.exe` (a possible obfuscation of TrickBot, a trojan that was used to steal banking info).

</details>


<details>
	<summary>Example 3: Network Events</summary>

`network where source.bytes > 1000000`

This query will look for network events where over 1MB of data was sent by the source of the connection.

</details>

These are three very simple examples of EQL queries. Next, we will get into different boolean operators we can use with EQL.


EQL Boolean Operators
===

EQL uses similar boolean operators to many other query languages, they are as follows:

`==` - Equals
<br>
`!=` - Not equals
<br>
`>` - Greater than
<br>
`>=` - Greater than or equal
<br>
`<` - Less than
<br>
`<=` - Less than or equal
<br>
`:` - Equals, case-insensitive
<br>
`and` - Used to require 2 matches
<br>
`or` - Used to require 1 or the other to match

>[!NOTE]
> The `:` operator can also be used when running wildcard searches (Ex. `url.domain: "*.io"`)

Let's take a look at a couple of examples:
<br>
<details> <summary>Example 1</summary>

`file where file.name != "WINWORD.EXE"`
- Find file events where the file name is NOT "WINWORD.EXE".
</details>
<br>
<details> <summary>Example 2</summary>

`network where destination.bytes < 1000 and url.domain : "*.io"`
- Find network events where the destination IP sent less than 1KB of data and the url.domain field ends in `.io`.
</details>
<br>
<details><summary>Example 3</summary>

`process where process.name : "winword.exe" or event.action == "creation"`
- Find process events where the process name is "winword.exe" but ignore case OR find where processes are being created.
</details>
<br>


Conclusion
===

Nicely done! In this lesson you looked at the basic structure of EQL queries including boolean operators and syntax.
For more information, refer to the [Event Query Language documentation](https://www.elastic.co/docs/explore-analyze/query-filter/languages/eql).

Next we will take a try at creating specific network queries.
Select  `Next` to continue.
