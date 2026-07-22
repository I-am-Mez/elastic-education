---
slug: lab-2-host-hunting
id: jr2mduvseyhd
type: challenge
title: 'Lab 2: Host Hunting with EQL'
teaser: Craft host-focused EQL queries to identify anomalous host-based events.
tabs:
- id: yd85xnhle0un
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
  custom_request_headers:
  - key: Content-Security-Policy
    value: 'script-src ''self''; worker-src blob: ''self''; style-src ''unsafe-inline''
      ''self'''
- id: y21cmztjrkvu
  title: Questions
  type: service
  hostname: kubernetes-vm
  port: 8008
difficulty: ""
timelimit: 0
lab_config:
  custom_layout: '{"root":{"children":[{"leaf":{"tabs":["assignment"],"activeTabId":"assignment","size":23}},{"leaf":{"tabs":["yd85xnhle0un"],"activeTabId":"yd85xnhle0un","size":53}},{"leaf":{"tabs":["y21cmztjrkvu"],"activeTabId":"y21cmztjrkvu","size":21}}],"orientation":"Horizontal"}}'
enhanced_loading: null
---
# Objectives
In this section, you will:
- Create EQL queries in order to hunt for host-specific events
- Utilize our EQL queries to answer questions in the CTFd instance

# Lab Parameters
- Navigate to `Timelines` and select `Untitled timeline` at the bottom of the screen.
![Open timeline.png](../assets/Open%20timeline.png)
- Select the `Correlation` tab and set your time picker to `BLISTER`.
![Blister.png](../assets/Blister.png)

Query 1: Seize the means
===

Oftentimes attackers will rely on you downloading a file in order to get into your system. From there, they will likely want to establish persistence in some fashion, normally by creating new files; that way, even if you delete the original file that infected you, you can still be accessed by the attackers. Make an EQL query to look for that.

**Task:** Create an EQL query to find files being created by an executable. Use this query to answer "Seize the means" in the [button label="Questions"](Questions) tab.

<details><summary>Hints</summary>

- You are looking for files being created, so we need to look for a file event.
- You need to find which field shows that a file is being created.
- You need to define that an executable ('.exe') is creating the files.
- You may need to add a column to show who created the file and the file name.

</details>


Query 2: Links of a chain
===

Attackers can oftentimes automate their malware to create malicious DLLs in order to cause regular programs to do irregular actions. Try to find that activity next.

**Task:** Create an EQL query to find executables creating DLL files. Use this query to answer "Links of a chain" in the [button label="Questions"](Questions) tab.

> [!IMPORTANT]
> Do not add anything to the query after completing the above step. Use the results of the query as it is in order to find the answer.

<details><summary>Hints</summary>

- You can use the same query from before, we just need to add a statement to find files that end in `.dll`.

</details>


Query 3: What's up doc?
===

Attackers will also like to create seemingly innocuous files on your system that can be accessed later to re-establish a connection or cause further actions to take place. Microsoft Office files are notoriously used for this purpose. Try to find something like that now.

**Task:** Create an EQL query to find executables creating Microsoft Word files. Use this query to answer "What's up doc?" in the [button label="Questions"](Questions) tab.

<details><summary>Hints</summary>

- You can use the same query as before, we just need to change one field and add an `or` statement.
- You want to find files that end in either `.doc` or `.docx`.

</details>



Conclusion
===

Nicely done! In this lesson, you created EQL queries to find specific host-based events.

Next you will take a try at creating EQL queries that look for both network and host events happening in a particular order.
Select `Check` to continue once you have completed the assignments in the [button label="Questions"](Questions) tab.
