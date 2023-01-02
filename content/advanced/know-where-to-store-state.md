---
title: "Know where to store state without re-rendering (WIP)"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: know-where-to-store-state
weight: 1
---


Sometimes it is useful to track state without triggering a re-render whenever that state changes.

For example, you may want to track whether the cursor has hovered over a subcomponent and use that data in a click handler.

In a class component, you can simply write to an instance property on the component class. You are of course responsible for keeping that instance property in sync with prop and state changes.

In functional components, you can (mis)use the `useRef` hook to store data without triggering a re-render. I have used this technique to keep a click handler in sync with changes caused by mouseover events. Without `useRef`, the click handler will hold a reference to a callback with a stale closure:

[wip example code]
