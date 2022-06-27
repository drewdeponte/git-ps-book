# List patchs on your stack

How to list patches on your stack and their associated state information.

## TLDR

1. `gps list` or `gps ls` for short

## Walkthrough

The `list` command lists out your stack of patches in a format that exposes
the patch index on the far left followed by the state information, followed by
the short SHA of the git commit, and finally followed by the patch summary.

```
[index] [status]    [sha] [summary]
```

The patch index value is used with other commands, e.g. `gps rr <patch-index>`.

The state information is broken down into main states and modifiers. The main
states are as follows.

```
b    - local branch has been created with the patch
s    - the patch has been synced to the remote
rr   - you have requested review of the patch
int  - you have integrated the patch into upstream
```

Each of the states can have any of the following modifiers.

```
+    - the patch in your patch stack has changed since the operation
!    - the remote patch has changed since the operation
↓    - patch is behind, the patch stack base has been updated since the operation
```

To fully understand this lets look at an example. Let say you see the status
`rr+!` when you ran the `list` command. This is telling you that the current
patch is in a state of **requested review**, indicated by the `rr`. It is also
telling you that since you last requested review of that patch changes have
been made to it, in your patch stack. This is indicated by the `+` modifier.
In addition it is telling you that the patch on the remote has also changed
since you last requested review, indicated by the `!` modifier.

The `+` by itself is a pretty common modifier to see as it is there to simply
remind you that you have made changes to a patch and need to `sync` or
`request-review` for that patch again.

The `!` modifier is less common as it only happens when the patch on the
remote has changed since the last operation. This is generally because someone
either force pushed up a patch out of band of Git Patch Stack to replace that
commit or because someone added a commit to the branch that Git Patch Stack
created and associated to that patch. To resolve `!` modifiers you really need
to go look at the commits on the remote branch and see if there are any
changes there you want to keep. If so you should cherry-pick the ones you want
into your patch stack and squash/fix them up into the logical patch using the
`rebase` command. Then you should `sync` or `request-review` of that patch
again to get it back in sync.

The `↓` modifier indicates that the patch is conceptually behind. What this
means is that when the last rr/sync operation was performed the base of the
patch stack was at one point in the git tree but now it has progressed forward
as someone integrated changes into it. This can be addressed by doing a `gps
pull` to make sure that your local stack is up to date and integrates
everything from upstream and then doing a `gps sync` or `gps rr` to update the
remote with newly rebased patch.
