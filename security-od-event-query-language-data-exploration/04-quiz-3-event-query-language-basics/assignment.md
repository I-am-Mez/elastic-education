---
slug: quiz-3-event-query-language-basics
id: dqluisriw7sy
type: quiz
title: 'Quiz 3: Intro to Event Query Language'
answers:
- 'sequence with maxspan=30m [file where process.args : "*.exe" and event.action ==
  "creation"] [network where url.domain : "*"]'
- 'sequence with maxspan=30s [file where process.args : "*.exe" and event.action ==
  "creation"] [network where url.domain: "*"]'
- sequence with maxspan=30s [file where process.args == "*.exe" and event.action ==
  "creation"] [network where url.domain == "*"]
- '[file where process.args : "*.exe" and event.action == "creation"] [network where
  url.domain: "*"]'
solution:
- 1
difficulty: ""
enhanced_loading: null
---
Which query will search for all of the following:
- A file event where an executable creates a new file
- A network event is created after with the field `url.domain` existing in the log
- A max timespan of 30s between the events
