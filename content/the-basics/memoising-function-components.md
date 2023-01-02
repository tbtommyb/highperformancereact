---
title: "Memoising function components (WIP)"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: memoising-function-components
weight: 2
---

React executes your entire functional component every time it re-renders.

## Don't recompute data or callbacks

`useMemo` for expensive computation.

`useCallback` so that callback props have a persistent reference value.

## Use `React.memo` to avoid re-renders

React will normally avoid re-rendering a component wrapped in `React.memo` while its props do not change.
