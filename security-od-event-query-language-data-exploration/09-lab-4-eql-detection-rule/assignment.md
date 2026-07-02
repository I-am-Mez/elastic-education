---
slug: lab-4-eql-detection-rule
id: 3dhfl2ng49kw
type: challenge
title: 'Lab: EQL Detection Rules'
teaser: Create an EQL detection rule using the most recent query we built in the last
  lab.
notes:
- type: video
  url: https://play.vidyard.com/6QzRARBZ3uK1o6L9Bgb9jp.html
tabs:
- id: x8p7iiz6zlxi
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
# Objective
In this section, you will:
- Create an EQL rule to automate detections.


Task
===

Create an EQL detection rule using the following query:

```
sequence with maxspan=30s
[process where process.name : "*.exe" and event.action == "creation" and file.name: "*.doc" or file.name: "*.docx"]
[network where source.bytes > 10000000]
```

## Requirements
Choose whichever settings you want for the rule, leave them default if you like. Completion of this lab just requires you use the query above to create an Event Correlation detection rule.


Conclusion
===

Nicely done! In this lesson you created an EQL rule to automate your detections. This will allow you to be notified any time your query matches in the form of an Alert, rather than needing to manually search each time you want to check for activity.

Select `Check` to finish the course once you have created your detection rule.