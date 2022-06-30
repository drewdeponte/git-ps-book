# Reorder patches

One of the core concepts of Git Patch Stack is this idea of starting out with
patches in an non-reviewable state and then iterating on them to get them to a
state where they are ready for review. As we iterate on various patches it is
crucial to be able to reorder the patches on your stack so that the
dependencies can be moved to the bottom of the stack.

## TL;DR

1. `gps rebase`
2. reorder the commit lines presented, in your configured editor, into the order you want them
3. save and quit the editor

## WalkThrough

The `gps rebase` command is a convenience function that really runs an
interactive rebase of the stack on top of it's associated upstream, e.g.
`git rebase -i --onto origin/main origin/main main`.

So understanding how to reorder patches with this command is really simply
learning how to reorder commits using git's interactive rebase.

Lets start with the following patch stack (`gps ls`).

```
2           0317f6 Add function C
1           5b5f29 Add function B
0           32e6fd Add function A
```

Lets say that function B needs to become a dependency of function A. In order
to put our stack into a state where we can actually iterate on function A
adding B as a dependency we need to first reorder the patches so that "Add
function B" is at the bottom of the stack.

To do this we start by running `gps rebase` to kick off the interactive rebase.
It presents the following in our configured editor.

```
pick 32e6fd7 Add function A
pick 5b5f290 Add function B
pick 0317f6a Add function C

# Rebase a34b62b..0317f6a onto a34b62b (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified); use -c <commit> to reword the commit message
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

To reorder the commits we simply reorder the lines of text in this buffer so
that they are in the order we want. *Note:* The stack is inverted when
presented in an interactive rebase. So the bottom most commit on the stack is
actually the top most commit. To accomplish our goals lets swap "Add function
A" and "Add function B" as follows.

```
pick 5b5f290 Add function B
pick 32e6fd7 Add function A
pick 0317f6a Add function C

# Rebase a34b62b..0317f6a onto a34b62b (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified); use -c <commit> to reword the commit message
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

We then save and quit the editor and it reorders the commits as part of the
rebase. This leaves our patch stack as follows (`gps ls`).

```
2           04ea78 Add function C
1           643bb7 Add function A
0           1d9185 Add function B
```

Exactly the state we wanted it to be in so that we can iterate on the "Add
function A" patch to make it integrate with function B.
