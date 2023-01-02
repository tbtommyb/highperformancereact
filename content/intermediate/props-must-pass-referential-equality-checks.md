---
title: 'Props must pass referential equality checks (WIP)'
date: 2019-02-11T19:27:37+10:00
kind: section
slug: props-must-pass-referential-equality-checks
weight: 1
---

Your component will re-render if a prop value changes. Even with `PureComponent` or `React.memo`, changing function and object references can cause unexpected re-renders:

```React
const MemoisedButton = React.memo(Button);

const Component = props => (
  <MemoisedButton onClick={event => props.onClick(event)}
)
```

You might think that memoising the `Button` will prevent it rendering when `Component` renders. In fact, the definition of `onClick` creates a new lambda every time. Each lambda will have a different reference and `React.memo` will think that the prop value has changed and will re-render.

In this case, the lambda provides no benefit and removing it suffices to fix the problem.

When you need to pass extra parameters into the callback, consider creating an intermediate component that receives the extra parameters as props and can construct a memoised callback:

```React
const MemoisedButton = React.memo(Button);

// Each button has a different value to pass to the callback
const ButtonsUnoptimised = props => (
  <>
    <MemoisedButton onClick={event => props.onClick(event, "first")} />
    <MemoisedButton onClick={event => props.onClick(event, "second"}} />
    <MemoisedButton onClick={event => props.onClick(event, "third")} />
  </>
)

const NamedButton = props => {
  const handleClick = React.useCallback(
    event => props.onClick(event, props.name),
    [props.name]
  );
  
  return (
    <MemoisedButton onClick={handleClick} />
  );
};

const MemoisedNamedButton = React.memo(NamedButton);

const ButtonsUnoptimised = props => (
  <>
    <MemoisedNamedButton onClick={props.onClick} name="first" />
    <MemoisedNamedButton onClick={props.onClick} name="second" />
    <MemoisedNamedButton onClick={props.onClick} name="third" />
  </>
);
```

Don't bother trying to use `Function.prototype.bind` to construct partially-applied callbacks. They will still fail shallow equality checks.

Object literals passed in as props will be recreated on each render:

```React
const MemoisedButton = React.memo(Button);

const Component = props => (
  <MemoisedButton onClick={props.onClick} styles={{ background: "red" }} />
);
```

Each `styles` prop value will have a different object reference, and so failing `React.memo`'s shallow equality checks, even though the content does not change.

## Be careful when mapping over arrays (WIP)
