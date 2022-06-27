# Diff Stacking

## What is it in general?

## Why not stack branches instead of patches?

Well there are two primary which I think are very important reasons to choose stacking patches over stacking branches.

- stacking patches removes a layer of abstraction simplifying how you think about things as well as how you do them

This simplification and constraint is actually more valuable than what you
might think. The stacking of patches and having to deal with the concepts of
patch dependence/independence is an impatice driving the developer to truely
understand their application architecture and write patches in relation to it.
This is why I would argue that stacking patches has a more natural alignment
with the application architecture than stacking branches which are generally
associated with features and not the application architecture.
