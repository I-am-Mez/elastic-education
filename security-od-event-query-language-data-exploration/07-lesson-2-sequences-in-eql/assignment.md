---
slug: lesson-2-sequences-in-eql
id: resil7skf6lu
type: challenge
title: 'Lesson 2: Sequences in EQL'
teaser: Sequences in EQL for correlative hunting
tabs:
- id: am0j9bbiqkpc
  title: Video
  type: website
  url: https://play.vidyard.com/FNd5pSJYGZJZKE9vukWxZi.html
- id: uhjfimv3zpgw
  title: Kibana
  type: service
  hostname: kubernetes-vm
  path: /app/security
  port: 30001
difficulty: ""
enhanced_loading: null
---
In this lesson...
===
You will
- Learn how sequences can enhance queries
- Construct EQL sequence queries

## Lab Parameters
- Open a timeline
![Open timeline.png](../assets/Open%20timeline.png)
- Select the `Correlation` tab and set your time picker to `BLISTER`
![Blister.png](../assets/Blister.png)

 EQL Sequences
===

The main power of EQL is the utilization of sequences; they allow us to chain queries together and define how much time should pass for the sequence. Let's look at the syntax then a couple examples.

The syntax to run a sequence query is fairly straightforward:  `sequence with maxspan=`

Then we define the time range the sequence should take place in. Here are a few examples: `20s` for 20 seconds, `30m` for 30 minutes. This pattern can be repeated with `ms`(milliseconds), `h`(hours), and `d` (days).

Once we define our time range we can start putting our queries in, all we need to do is enclose the query in `[ ]` brackets.
>[!NOTE]
> An EQL sequence query requires two queries to work.

Let's take a look at a couple of examples:

<details><summary>Example 1</summary>

Creating an EQL query to find a file event on dc01 and an executable being ran (also on dc01) happen within 20 seconds of each other:

Line 1: Define the sequence query and maxspan of 20 seconds.

Line 2: Query for a file event on dc01.

Line 3: Query for a process with a name ending in .exe starting on dc01.

<details><summary>Answer</summary>

```
sequence with maxspan=20s
[file where host.name : "dc01"]
[process where process.name : "*.exe" and event.action: "start" and host.name: "dc01"]
```

</details>
</details>

<details><summary>Example 2</summary>

Creating an EQL query to find a network event where the `url.domain` is `malware.com` and a `.pdf` file being created within five seconds of each other on `workstation02`.

Line 1: Define the sequence query and maxspan of 5 seconds.

Line 2: Query for `workstation02` going to `malware.com`.

Line 3: Query for a `.pdf` file being created on `workstation02`.

<details><summary>Answer</summary>

```
sequence with maxspan=5s
[network where url.domain: "malware.com" and host.name: "workstation02"]
[file where file.name: "*.pdf" and event.action: "creation" and host.name: "workstation02"]
```

</details>

>[!NOTE]
>You will not get any results on this query

</details>

Conclusion
===

Nicely done! In this lesson you looked at how sequences can enhance queries and created two sequence queries.
For more information, refer to the [EQL Sequencing documentation](https://www.elastic.co/docs/reference/query-languages/eql/eql-syntax#eql-sequences).

Next you will demonstrate your knowledge about sequencing with EQL in a lab.
Select  `Next` to continue.