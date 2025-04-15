---
date: <%tp.date.now("YYYY-MM-DD")%>T<%tp.date.now("HH:mm")%>
tags:
  - Daily
cssclasses:
  - daily
  - <% tp.date.now("dddd").toLowerCase() %>
---
# DAILY NOTE
## <%tp.date.now("dddd, MMMM Do, YYYY")%>
***
### Tasks
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

***
<< [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD').subtract(1, 'd').format('YYYY-MM-DD') %>|Yesterday]] | [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD').add(1, 'd').format('YYYY-MM-DD') %>|Tomorrow]] >>