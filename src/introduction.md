# Introduction

## What is it?

Git Patch Stack is really two things; a **conceptual model** and a **tool**.
The **conceptual model** is the high level mental model used to think in terms
of managing your code changes as a stack of patches rather than a series of
isolated feature branches. The **tool** is a command line interface to aid in
streamlining the process of working within this mental model.

Git Patch Stack is no different than any other tool built around a conceptual
model. It provides natural benefits as side effects of their innate
characteristics. However to truly unlock it's full potential it needs to be
paired with a **methodology** that is aligned not only with it's innate
characteristics but also the foundational principles.

## Why use it?

The conceptual model helps you think in terms of the stack of patches instead
of a series of isolated branches. The tool makes it easy for you to operate
within this model. This affords you the following benefits.

- don't think about branches anymore
- never blocked by waiting on peer reviews

This might be a good start and yes it is probably better than what you were
doing before. But, to really make huge differences at an application
architecture level and a software maintainability level you really need to
adopt the **methodology** which will unlock even more benefits like the
following.

- develop & ship software faster
- aligns with best practices for producing more maintainable software
- less mental overhead & complexity
- smaller, logical code changes, scoped to the application architecture
- more valuable peer reviews
- makes conflict resolution easier
- facilitates continuous integration while still support pre-commit code review
