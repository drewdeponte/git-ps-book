# Introduction

## What is it?

Git Patch Stack is a command line tool that facilitates a Patch Stack based
workflow with [Git][].

It focuses on being **platform-agnostic**, **hosting provider-agnostic**, as
well as supporting customization for a **personal development experience**.

I like to think of it as the [Neovim][] of Patch Stack tools for [Git][]. It
provides a solid baseline set of functionality, around the concepts necessary
for a Patch Stack workflow, while still facilitating the necessary
customization to truly create a personal development experience.

## Why use it?

Git Patch Stack helps you think in terms of a stack of patches instead of a
series of isolated branches. This affords you the following benefits. First,
you don't have to think about branches. Second, it prevents you from being
blocked by waiting on peer reviews.

This alone is a good start, and yes, it is probably better than what you were
doing before. But to really make huge differences at an application
architecture level and a software maintainability level you really need to
adopt the **methodology** which will unlock benefits like the following.

- develop & ship software faster
- aligns with best practices for producing more maintainable software
- less mental overhead & complexity
- smaller, logical code changes, scoped to the application architecture
- more valuable peer reviews
- makes conflict resolution easier
- facilitates continuous integration while still support pre-commit code review

[Git]: https://git-scm.com
[Neovim]: https://neovim.io
