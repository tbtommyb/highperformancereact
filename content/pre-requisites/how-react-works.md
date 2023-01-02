---
title: 'Understand React'
date: 2019-02-11T19:27:37+10:00
kind: section
slug: how-react-works
weight: 1
---

React updates components in three steps:
 
1. Render: React calls your component and gets a React Element in return.
1. Reconciliation: React compares the new Elements from step 1 with those from the previous render.
1. Commit: If there are changes, React updates the DOM to reflect the new changes.

React is designed on the assumption that re-rendering components (step 1 above) is cheap but updating the DOM is expensive. While this is a good rule of thumb, bear in mind that a rendering component will render all of its children too. It's therefore possible to inadvertently perform a lot of unnecessary work.

Optimising React apps is largely about __speeding up slow renders and eliminating unnecessary re-renders__.

[WIP: maybe clearer to use "update" for React rendering to separate it from DOM changes?]
