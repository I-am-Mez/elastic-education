---
slug: lab-3-correlation-hunting
id: sgwgeeshmxuk
type: challenge
title: 'Lab 3: Correlation Hunting with EQL'
teaser: Craft host and network-focused EQL queries to identify anomalous events.
tabs:
- id: pnzcrolf7zy2
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: uh8xrgzrku9j
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
lab_config:
  custom_layout: '{"root":{"children":[{"leaf":{"tabs":["assignment"],"activeTabId":"assignment","size":25}},{"leaf":{"tabs":["pnzcrolf7zy2"],"activeTabId":"pnzcrolf7zy2","size":52}},{"leaf":{"tabs":["uh8xrgzrku9j"],"activeTabId":"uh8xrgzrku9j","size":20}}],"orientation":"Horizontal"}}'
enhanced_loading: null
---
# Objectives
In this section, you will:
- Create EQL queries in order to hunt for network and host events
- Utilize our EQL queries to answer questions in the CTFd instance

# Lab Parameters
- Navigate to `Timelines` and select `Untitled timeline` at the bottom of the screen.
![Open timeline.png](../assets/Open%20timeline.png)
- Select the `Correlation` tab and set your time picker to `BLISTER`.
![Blister.png](../assets/Blister.png)

Query 1: Processes making documents then network connections
===

Now you can start to combine principles of the queries you've already made; let's find a Word document being created then a connection to a domain occurring.

**Task:** Create an EQL query to identify an executable creating a Microsoft Word document, then, within 30 seconds, a connection being made with the url.domain field populated. Use this query to answer "Well that ain't right", "Where you going?", and "Win some, lose some" in the [button label="Questions"](Questions) tab.

<details><summary>Hints</summary>

- You need to define the max timespan of our events first.
- Remember the query you used at the end of the `Host Hunting` section for line 2.
- The search for a field being populated is formatted as: `field.name: *`.
- You may need to add columns or expand the log to find the field you want.

</details>


Query 2: Processes making Word documents then sending large amounts of network data
===

**Task:** Create an EQL query to identify an executable creating a Word document, then, within 30 seconds, a network connection where the `source.ip` sent more than 1 MB. Finally, you want to see if a Word document may be responsible for some data exfiltration. Use this query to answer "Here we go again", "How much though?", and "Where are you?" in the [button label="Questions"](Questions) tab.

<details><summary>Hints</summary>

- You need to define the max timespan of our events first.
- Remember the query you used at the end of the `Host Hunting` section for line 2.
- Remember the query you used in the beginning of `Network Hunting` for line 3.
- You may need to add columns or expand the log to find the field we want.

</details>


Conclusion
===

Nicely done! In this lesson, you created sequence EQL queries to find two different types of events happening.

Next you will turn your final query into a rule in order to automate our process.
Select `Check` to continue once you have completed the assignments in the [button label="Questions"](Questions) tab.
