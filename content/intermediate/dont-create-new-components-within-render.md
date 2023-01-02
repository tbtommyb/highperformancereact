---
title: "Don't create new components within render (WIP)"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: dont-create-new-components-within-render
weight: 3
---

Do not create new component definitions within the render path of another:

```React
const Component = props => {
  const Wrapper = wrapperProps => (
    <div onClick={props.onClick>{wrapperProps.children}</div>
  );
  
  return (
    <Wrapper {...props} />
      // content
    </Wrapper>
  )
};
```

React will always think that each instance of `Wrapper` is an entirely new component and will always re-render the entire component tree [WIP: source link here].

[WIP: what happens if we add a key to Wrapper?]

You might think that the example above is an obvious code smell. It is possible to hit this problem in a less obvious way using higher-order components:

```React
import { someHoC } from "./some-hoc";

const Component = props => {
  const WrappedComponent = someHoC(OtherComponent);
  
  return (
    <WrappedComponent {...props} />
      // content
    </WrappedComponent/>
  )
};
```

Always wrap components in higher-order components at the file level so that only one component definition is created:

```React
import { someHoC } from "./some-hoc";

const WrappedComponent = someHoC(OtherComponent);

const Component = props => {
  return (
    <WrappedComponent {...props} />
      // content
    </WrappedComponent/>
  )
};
```
