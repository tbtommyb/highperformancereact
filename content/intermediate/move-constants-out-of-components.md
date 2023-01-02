---
title: 'Move constants out of components (WIP)'
date: 2019-02-11T19:27:37+10:00
kind: section
slug: move-constants-out-of-components
weight: 4
---

Computed values that do not change should be defined outside of the component. Style objects are a common example:

```React
const Component = props => {
  const styles = { fontSize: 24 };
  return (
    <Button {...props} styles={styles} />
  )
};
```

There are two issues here:

- redefining the `styles` object will fail shallow equality comparisons, causing `Button` to re-render unnecessarily
- the object is computed every time `Component` renders even though it never changes, wasting computation.

Though the amount of wasted work is tiny in this example, it is easy to accidentally waste significant time filtering arrays, reformatting data etc.

When the data depends partially on a prop or state value, use the `useMemo` hook to cache the data as much as possible.
