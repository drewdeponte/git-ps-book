# Stuck on Feature Branches

Sometimes you are in a situation where you are stuck with Feature Branches.
This could be because your team hasn't made the glorious switch over to Git
Patch Stack yet. Or it could be you are dropping in as a consultant to help a
team out with something. Or maybe you are working on a team that has a bunch of
automation setup already around the use of Feature Branches.

Whatever your situation, don't fret! Git Patch Stack is designed such that
you can use it locally and still get a lot of the benefits even if you are
stuck in the world of Feature Branches.

## Build up Patch Series

The big difference between the normal workflow and this one is simply that you
don't request review of the individual patches as soon as you finalize them.
Instead, you build up a series of finalized patches in the correct order within
in your patch stack. Then you effectively request review of the series of patches.

This sadly means you don't get the benefits of near continuous integration.
However, all is not lost. If you use Git Patch Stack locally and follow the best
practices and methodology around creating small logical patches. You will end
up with a well-defined series of patches that will stand as a logical "proof of
work" for your feature. Beyond that those logical patches will be invaluable
later on as part of the Git history.

## Request Review of Series

Once you have built up your patch series of finalized patches. We need to
create a pull request of that patch series. The best process at the moment to
do this with Git Patch Stack is the following.

1. build up your series of finalized patches in your stack
2. create and push branch containing copy of your patch series - e.g. `gps branch -p 1 3`
3. open a Pull Request in whatever source control management tool you are using

*Note:* The `gps branch` command allows you to pass the `-n your-branch-name`
switch to it if you want to control the name of the branch created. Also, there
is a configuration option within the `branch` command section called
`push_to_remote` that can be set, so you don't have to pass the `-p` switch on
the command line to have it push to remote. See
[Configuration](configuration.md) for details.

## Deal with Feedback

Often times you will get feedback on your Pull Request when it is reviewed. To
address this process is generally as follows.

1. update the patches in your patch stack to address the feedback
2. overwrite local branch with updated patch series - e.g. `gps branch -p 1 3`
3. comment on the Pull Request letting the reviewer know what changes you have addressed and that they have been pushed up

## The Future

Discussions are currently being had around how best to include the concept of a
series of patches into the `request-review` command. Doing so would provide the
same functionality but with the benefit of state tracking.
