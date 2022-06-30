# Combine multiple patches

Sometimes you decide that two separate patches should really be collapsed into
one to make a complete logical change.

## TL;DR

For those who just need a quick reference as a reminder you can do the
following.

- `gps rebase` - do an interactive rebase of the patch stack
- mark the patch(es) you want to combine up into another patch with `squash` or
  `fixup` in the interactive rebase buffer
- save the buffer and quit the editor

## WalkThrough

The `gps rebase` command is a convenience function that really runs an
interactive rebase of the stack on top of it's associated upstream, e.g.
`git rebase -i --onto origin/main origin/main main`.

So to understanding how to combine patches with this command is really simply
learning how to combine commits using git's interactive rebase.

### Initial State

Lets start with the following patch stack (`gps ls`).

```
2           1d16f9 Add function C
1           6c8104 Add function B
0           c6f715 Add function A
```

### Mark Patches

Lets say that we need to combine the "Add function A" and "Add function B" patches together to make a logical patch.

To do this we start by running `gps rebase` to kick off the interactive rebase.
It presents the following in our configured editor.

```
pick c6f7155 Add function A
pick 6c81046 Add function B
pick 1d16f98 Add function C

# Rebase a34b62b..1d16f98 onto a34b62b (3 commands)
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

To combine a patch with the patch above it we simply mark it for `squash` or
`fixup` instead of it's default `pick`. If you forget the marking you can always
reference the comment Git includes in the interactive rebase buffer. So in this
case we mark the "Add function B" patch with `squash` as follows.

*Note:* The stack is inverted when presented in an interactive rebase. So the
bottom most commit on the stack is actually the top most commit. This can be
confusing until you get used to it.

```
pick c6f7155 Add function A
squash 6c81046 Add function B
pick 1d16f98 Add function C

# Rebase a34b62b..1d16f98 onto a34b62b (3 commands)
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

### Squash vs. Fixup

Git's interactive rebase provides two mechanisms of combining commits, `squash`
and `fixup`. **Squash** combines two commits and presents you with the combined
commit messages in the rebase. **Fixup** on the other hand combines two commits
while throwing away the commit message of the commit marked withe `fixup`.

**Warning:** You need to be careful when using `fixup` to make sure that you
aren't accidentally throwing away a `ps-id` from the commit message that you
want to keep around. 

### Choose a ps-id

When you are combining patches you want to be aware that depending on what
state your patches are in on, or the other, or both might have `ps-id`'s in
their commit message. The `ps-id` in the commit message is how Git Patch Stack
uniquely identifies patches and tracks and associates state to them.

If you are in a situation where only one of the patches involved has a `ps-id`
you will likely want to keep that `ps-id` in the resulting combined patch's
commit message.

If you are in a situation where multiple patches involved have `ps-id`'s you
will have to select one `ps-id` to keep and use as the `ps-id` for the new
combined patch.

### Confirm Rebase

We then save and quit the editor and it combines the patches marked with
`squash` or `fixup` as part of the rebase. This leaves our patch stack as
follows (`gps ls`).

```
1           8a6145 Add function C
0           ba2f4d Add function A & function B
```

Exactly the state we wanted it to be in.
