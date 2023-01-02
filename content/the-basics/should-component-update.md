---
title: "Implement `shouldComponentUpdate` on class components"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: should-component-update
weight: 3
---

[WIP: create pre-requisite section on deep vs shallow equality checks?]

React will by default re-render your component when its parent re-renders. The [lifecycle method](https://reactjs.org/docs/react-component.html) `shouldComponentUpdate` is used to prevent re-renders when you know that the component has not changed.

React calls `shouldComponentUpdate` before rendering whenever new state or props have been received. React's default behaviour [WIP: add link] is to always re-render. 

You can provide a custom implementation of the `shouldComponentUpdate` method receiving the new and previous values of both props and state. You can compare them and return `false` when a re-render is unnecessary.

How can you tell when a re-render is unnecessary? When the component's rendered output is a pure function of its props and state, a re-render is only required if a prop or state value has changed.

[WIP: example]

When using an immutable data layer (e.g. Redux), you can perform a fast shallow equality check using `===` to quickly check whether object references have changed (see `PureComponent` below).

Checking nested objects for changing values is known as _deep equality checking_ and has its own cost. When you have complex, mutable data, I recommend against trying to write some fancy deep equality check unless you've profiled the app and determined that rendering time saved outweighs the cost of the deep equality check.

Consider adding to data a unique identifier which is updated every time the data is mutated. Then in `shouldComponentUpdate` you only need to compare the identifiers.

## `PureComponent`

`PureComponent` is a variant of the standard React `Component` class that behaves as if it had a `shouldComponentUpdate` implementing a shallow equality check of props and state.

Looking at the [`PureComponent implementation`](https://github.com/facebook/react/blob/f101c2d0d3a6cb5a788a3d91faef48462e45f515/packages/react-reconciler/src/ReactFiberClassComponent.js#L346), we can see that it doesn't actually work by providing a `shouldComponentUpdate` implementation, but it achieves the same effect.

`PureComponent` will stop your component re-rendering just because the parent re-rendered. Shallow equality checking will not prevent re-renders when object references changes, even if the new object holds the same values as the old one (see [here](#props-must-pass-referential-equality-checks)). Using mutable data will cause `PureComponent` to not re-render when it should, since the object references will not have changed.

Prefer to use `PureComponent` when your component's rendered output is a pure function of its state and props. 

