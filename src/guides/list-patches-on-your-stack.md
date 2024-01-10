# List patches on your stack

How to list patches on your stack and their associated state information.

## TL;DR

1. `gps list` or `gps ls` for short

## WalkThrough

The `list` command lists out your stack of patches in a format that exposes the
patch index on the far left followed by the short SHA of the git commit,
followed by the patch summary, and finally followed by the status information.

```
[index] [sha] [summary (50 chars)         ]  ( [status] )
```

The patch index value is used with other commands, e.g. `gps show
<patch-index>`.

State information exists between a patch in the patch stack and a branch.
As you use Git Patch Stack your patches will be associated with one or
more local branches and each of those branches will likely have a remote
tracking branch associated to them.

So we represent state with two main prefixes, `l` & `r`.

- `l` - indicating that the following state indicators are between the local
        branch & patch in the patch stack
- `r` - indicating that the following state indicators are between the
        remote branch & patch in the patch stack

The presence of these prefixes also communicates the existence of a local or
remote branch in association with the patch. So if you saw a state of `( )` it
would indicate that the patch has no local branches & has no remote branches.

Each of these prefixes are paired with zero or more of the following state
indicators.

- `*` - the patch in the respective branch has a different diff than
        the patch in the patch stack
- `!` - the respective branch has one or more non-patch commits in it

The following are some simple examples of state indications, so you can start
to understand.

- `( )` - patch has no local & no remote branches associated
- `( l )` - patch has a local branch associated & the diff match
- `( lr )` - patch has a local branch & remote branch associated & the
            	diffs match
- `( l*r* )` - patch has a local branch & remote branch associated, but the
            	diffs don't match
- `( l*r*! )` - patch has a local branch & remote branch associated, but the
            	diffs don't match & the remote has a non-patch commit in it

In the most common case you will have a single branch pairing (local & remote)
associated with your patches, and you will see the patch state simply
represented as above.

However, Git Patch Stack supports having multiple branch pairings associated
with a patch and it also supports custom naming of branches if you don't want
to use a generated branch name. This is especially useful when creating a
patch series.

If a patch falls into either of these scenarios the state will be presented
in long form where the branch name is provided in front of each state
indication. So each branch will have its branch name appear followed by its
associated state indication.

	[branch-name] [state indications]

These pairings of branch name and state indications are simply chained together
with spaces. So for example, if we assume we have a patch that is associated
with two branches, `foo` & `bar`. The patch state information might look
something like the following.

	( foo lr bar l*r* )

In the above state information example we can see that there are 4 branches
that exist with the patch in them. Specifically, there is a local branch
named `foo` and it has a remote tracking branch that also has the patch in it.
We can see that because there is no `*` or `!` characters after the `l` or `r`,
associated with the `foo` branch, we know that the patch diffs all match.

We can also see that the patch exists in another local branch named `bar`, as
well as the remote tracking branch of `bar`. The `*` characters here indicate
that both the copy of the patch in both the local `bar` branch and the remote
tracking branch of `bar` have different diffs than the patch in the patch
stack.
