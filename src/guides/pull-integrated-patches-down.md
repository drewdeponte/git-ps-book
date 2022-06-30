# Pull integrated patches down

In this guide we're going to focus on how to pull down integrated patches from
upstream and have your stack replayed on top of the pulled patches.

## TL;DR

1. `gps pull`
2. if conflict, resolve it
3. if conflict, `gps add` resolved conflict files
4. if conflict, `gps rebase --continue`
5. repeat steps 2 to 4 for each conflicting patch

## WalkThrough

The TL;DR section makes this feel trivial. And generally it is pretty trivial as you
simply run `gps pull`, which effectively runs a `git fetch` to update your local
repositories knowledge of the upstream repository's Git tree followed by a
`git rebase --onto <upstream-branch-name> <upstream-branch-name> <head-branch-shortname>`
, e.g. `git rebase --onto origin/main origin/main main`.

Conflicts occurring during the rebase are what might catch you off guard the
first time. Here in your stack is exactly where you want to be confronted with
and resolve any conflicts though. It forces you to integrate the upstream
branch's changes with your stack more often and with smaller increments
which makes conflict resolution easier because the scope of changes is smaller.
Beyond that having integration be bound to the pull is also beneficial because
it forces you to integrate your stack when fetching any changes from upstream.

Given that this command performs a rebase it is beneficial to have a good
understanding of what rebasing is and what happens during a rebase.
To get a deeper understanding of rebase and gain some comfort with it you can
check out [ProGit - Git Branching - Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing).

### Merge Conflicts vs Rebase Conflicts

In addition to being comfortable with the basics of rebasing it is important to
understand that merge conflicts that you might be used to with `git merge` are
quite different than the conflicts you will run into with `git rebase`. When
presented with a conflict from a `git merge` operation you are effectively
resolving all the conflicts of all the commits involved as one singular
conflict.

The `git rebase` operation works in a completely different manner. It
effectively lifts up your stack of patches and plays them back one by one on
top of the upstream branch (e.g. `origin/main`). This is important because
conflicts are risen at the commit level. This means as it is going through
and replaying each commit on top of one another it is checking if there is a
conflict or not. If there isn't a conflict then it applies that commit cleanly
and moves on to playing the next commit in the stack. If there is a conflict it
pauses the rebase and leaves you in a working state with the conflict present.

You tactically resolve this the same way you would any conflict. However, you
need to understand that the scope of the conflict is specific to the commit that
was replayed and not all the commits combined together like a `git merge` would
be. This means when you resolve that conflict you should resolve it as if none
of the commits above it existed yet.

This works brilliantly with Git Patch Stack because you write patches as
logical changes in alignment with the application architecture. This means when
conflicts arise they are within the scope of some application architecture
concept or within the integration of an application architecture concept. This
makes understanding and resolving conflicts much easier than with `git merge`,
where the changes aren't scoped to a particular application architecture
concept.

Once you have resolved the conflict and staged the change with `git add` you
can continue the rebase process with `git rebase --continue` to have it pick
back up from where it was paused.

### Convenience Functions

Git Patch Stack provides the `gps add` and `gps rebase --continue` commands as
convenience mechanisms to help provide a complete abstraction so you don't have
to bounce between Git and Git Patch Stack if you don't want to.
