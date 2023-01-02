---
title: 'Understand component updates (WIP)'
date: 2019-02-11T19:27:37+10:00
kind: section
slug: what-makes-a-component-update
weight: 2
---

A React component will update when:

1. Its parent re-renders
1. Its state changes
1. A Context it uses has changed
1. A hook triggers schedules an update
1. The component is memoised and a prop value has changed
