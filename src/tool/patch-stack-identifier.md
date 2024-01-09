# Patch Stack Identifier

## What is it?

The Patch Stack ID or ps-id is simply a Globally Unique Identifier (GUID) that
is added to the commit message of a commit when a commit is cherry picked into
a patch stack managed branch. Most commonly this occurs with the `gps branch`,
`gps request-review`, and `gps integrate` commands.

## What is it used for?

When you cherry-pick or rebase you end up with a commit that has a different
sha. So we can't use the sha to represent a patch because it changes on us. So
instead we use this ps-id which stays the same across cherry-picks and rebases.
This allows us to identify that two or more commits represent the same
conceptual patch even though the commit SHAs are different. This in turn
empowers Git Patch Stack to be able to tell that changes have been made to a
patch in one place but not another, etc.

## Picking a ps-id when squashing

When you are rebasing and squashing a patch into another patch you may notice
in the commit message you will have two ps-id values if you are squashing two
patches that have already been associated with a branch. A patch should
conceptually only have one identifier. So which one should you pick? Well it
depends. You need to decide which patch most accurately represents the concept
of the patch going forward and to use that one and get rid of the other one. If
it isn't clear in a particular case. You can simply pick one and the branch
that was already tied to that patch will be the branch that is used.

## What happens to ps-id when fixing up?

When you fixup a commit into another commit the commit being fixed up will lose
its commit message and along with it any ps-id it may have had. This is ok
because when you are fixing up the commit that is be fixed up into is
effectively absorbing the other commit. So you will be left with its ps-id at
the end which is conceptually what you would want anyway.

## Can we hide it so that out commit messages are clean?

No not really. There have been discussions on using git notes to track them but
I believe they are tied to the sha. Also I believe I heard they are on the way
out. If we didn't have the ps-id we would lose the ability to identify that
various commits are all just different states of the same patch. We would also
lose the ability to know the branches associated with a patch. So no we can't
hide it from the commit message. Also it has become a pseudo standard to add
additional metadata like this to the commit message. So we are following inline
with that pattern. Currently we have HTML comments around the ps-id which hides
it in PR descriptions in GitHub while still keeping it in the commit message.
If you are on a team that can't recognize the value it brings and refuses to
have it in the commit messages. Then you are fundamentally working on a team
that refuses to allow Git Patch Stack. Because without it Git Patch Stack can't
really exist. The best you can hope for is convincing them. But I imagine if
they are that strongly against having a ps-id in the commit message they
probably don't hold that opinion solely on logic.

## Other benefits of ps-id.

There is also the benefit of driving exposure via use by having the ps-id be
visible. Maybe it would even be worth having a tag line or url of
[https://git-ps.sh](https://git-ps.sh) in the line.
