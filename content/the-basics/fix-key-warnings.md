---
title: "Fix key warnings"
date: 2022-07-28T15:14:39+10:00
kind: section
slug: fix-key-warnings
weight: 1
---

Keys are special props used to uniquely identify a component. React will warn when you render a list of components without providing keys. Check in your browser console for any warnings and fix them.

React's reconciliation algorithm relies on keys to identify list items that have not changed. Imagine you have a `NavBar` component:

```React
<NavBar>
  <NavItem path="/about" label="About"/>
  <NavItem path="/buy-my-book" label="Buy My Book!"/>
</NavBar>
```

Now you add functionality for a logged-in user to access their account:

```React
<NavBar>
  {user.loggedIn && (<NavItem path="/my-account" label="My Account"/>)}
  <NavItem path="/about" label="About"/>
  <NavItem path="/buy-my-book" label="Buy My Book!"/>
</NavBar>
```

When the user logs in and React adds the `NavItem`, it will compare the new first element with the previous first element and find they don't match. React will recreate every `NavItem`, even though two of them have not changed. We must provide stable, unique keys so that React can "see" that two `NavItem`s remain unchanged and it only needs to create a single new one.

It's surprisingly easy to introduce key warnings as you refactor code. The key must always be defined on the component that is directly rendered by the list. In the example above, if we wrapped the `NavItem`s in another component, we'd need to move the keys to the wrapping component for them to have any benefit.
