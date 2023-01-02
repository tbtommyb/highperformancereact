---
title: "Understand useState"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: usestate-tips
weight: 2
---

`useState` is one of the most common hooks. Understand how it works to avoid unnecessary anti-patterns.

## Don't check for changes before updating

The `useState` implementation [wip: code link] already checks whether the value has changed before doing anything. Implementing your own check is unnecessary and does not improve performance (though it has no additional cost either).


## Access current state in the update handler

This example code sets the visibility of a modal:

```React
const ModalWrapper = props => {
  const [isVisible, setIsVisible] = useState(false);
  const handleClick = useCallback(
    () => setIsVisible(!isVisible),
    [isVisible]
  );

  return (
    <ModalContent onClick={handleClick} show={isVisible} />
  );
};

```

It attempts to cache the click handler to aid `React.memo`'s memoisation and avoid re-renders.

However, the callback depends on `isVisible` and so it will be recreated every time `isVisible` changes value! This is more or less as bad as not memoising at all.

Update handlers provide the current state as the first parameter, so use that instead of recreating the callback:

```React
const ModalWrapper = props => {
  const [isVisible, setIsVisible] = useState(false);
  const handleClick = useCallback(
    () => setIsVisible(v => !v), // change here
    [] // and here
  ); 

  return (
    <ModalContent onClick={handleClick} show={isVisible} />
  );
};

```

