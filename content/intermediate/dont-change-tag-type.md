---
title: "Don't change tag type (WIP)"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: dont-change-tag-type
weight: 2
---

React will always re-render the entire component if you change its root tag, even if everything else is the same [WIP: source link].

Avoid components that regularly change their root component based on state or props.
