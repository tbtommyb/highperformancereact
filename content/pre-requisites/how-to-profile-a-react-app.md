---
title: 'Profile your app'
date: 2019-02-11T19:27:37+10:00
kind: section
slug: how-to-profile
weight: 3
---

It is essential to understand your browser's JS profiler and the profiler included in [React Developer Tools](https://beta.reactjs.org/learn/react-developer-tools).

Your browser's renderer has detailed information but can only observe plain JS execution. The React profiler can tell you which components are rendering and why. The React profiler is pretty easy to use once you know how.

## How to record a profile

1. Install React Developer Tools and then open the Profile tab in your browser's dev tools
1. Click the gear button, go to 'Profiler' tab and enable 'Record why each component rendered while profiling'
1. Click the red 'record' button in the top left, interact with your app and then stop recording.

## How to read a profile (WIP)

At the top right we have the list of commits React performed during the profile. The higher the bar, the longer that commit took.

The main pane shows which components updated during the selected commit. At the top we have the root component and leaves at the bottom. Take some time to work out how the profiler output corresponds to your app's component structure.

The width of each bar indicates how long that component and its children took to render. 'Hotter' colours, such as yellow and red, indicate slower components. Grey means that the component didn't update at all in this commit.

As you hover over each component, the tooltip will tell you why the component updated. This is super useful to identify which props are triggering updates. Sadly, the explanation is often inaccurate. You'll almost certainly find cases where the profiler says that a component updated because its parent re-rendered while also claiming that the parent didn't re-render. This is because the profiler uses `The parent component rendered.` as [its default explanation](https://github.com/facebook/react/blob/9cdf8a99edcfd94d7420835ea663edca04237527/packages/react-devtools-shared/src/devtools/views/Profiler/WhatChanged.js#L154).

When the profiler tries to tell you nonsense, stand your ground! Look carefully at the troublesome component and see what else could have triggered an update. I usually find it involves a hook.
