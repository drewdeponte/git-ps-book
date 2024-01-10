# Absorb Changes

## TL;DR

1. discover changes that were pushed up to your review branch
2. `git cherry-pick <pushed-up-commit-sha>` - cherry-pick the changes down into your patch stack
3. `gps rebase` w/ `fixup/squash` - absorb the change
4. `gps rr` - re-request review of updated patch(es)

## WalkThrough

In this example let's assume that we have a simple patch stack consisting of a
single patch. If we run `gps ls` it looks as follows because we have already
requested review of our patch.

```
✔ gps ls
0 rr         0a301d Add foo() function
```

### Discover Changes

We soon discover that one of the reviewers of our pull request has pushed
changes up to review branch. This often happens when the reviewer is trying to
give you an example or when they are trying to help you out with some changes.

We don't want just ignore their changes and do a `gps rr` again as we would be
throwing their changes away. We also don't want to manually implement what they
have already done as that is more work.

### Cherry-Pick Changes

So we `git cherry-pick 5bca34` to cherry-pick the change down into our patch
stack. Now if we run `gps ls` our patch stack looks as follows.

```
✔ gps ls
1           8ce58e Add bar() function
0 rr        0a301d Add foo() function
```

### Absorb Changes

Now we simply need to absorb the change in some way. It could be that it
logically fits with our patch and therefore we `fixup/squash` it with our patch
using `gps rebase`.

Or, it could be that it makes sense as a separate patch.

### Re-request Review

If we did the `fixup/squash` approach we simply need to re-request review of
our now updated patch with `gps rr`.

We could also have decided to keep a separate request review for it
individually with `gps rr` and `gps rr` our originally patch, adding a comment
on the PR to explicitly communicate the decision to keep them separate.

The last option would be that we chose to keep them separate but as part of a
patch series. In which case we would use the `gps rr 0-1` command to re-request
review.

That is it! We just absorbed the change that was pushed up.
