---
title: "Know when updates are batched (WIP)"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: know-when-updates-are-batched
weight: 3
---

React by default batches state updates so that multiple state updates are applied together and only cause a single re-render.

This does not apply in all cases. Most importantly, state updates in asynchronous calls are not batched.

[WIP code example]

