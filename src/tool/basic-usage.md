# Basic Usage

The basic usage pattern for Git Patch Stack is pretty straight forward and is
outlined as follows.

1. add one or more WIP patch(es) to your stack 
2. iterate and evolve a patch to a state where it is ready to be reviewed
3. request review of the patch
4. rinse and repeat steps 2 & 3 until that patch is approved
5. integrate the patch

## Add one or more WIP patch(es) to you stack

The first step is to add a WIP, Work In Progress, patch to your stack. This is
effectively a stack that is incomplete in it's nature with a summary prefixed
with "WIP:".

This is done simply by making some local changes, staging them with `gps add`,
and then create a patch with `gps c` or `gps create-patch`. This will take the
staged changes and create a patch on top of your stack from them.

## Iterate and evolve a patch to a state where it is ready to be reviewed

The second step is to iterate the WIP patch using various commands like `gps a`
or `gps amend-patch`, `gps rebase`, etc. to get it to a place where it is ready
to review. At this point you remove the "WIP:" prefix from the patches summary.

## Request review of the patch

Then you request review of the patch with `gps rr` or `gps request-review`.
This requests review of the specified patch using the provided hook. This most
likely is creating a pull request on GitHub, Bitbucket, or GitLab.

## Rinse and Repeat until approved

Once you have requested review of the patch you wait for feedback and make any
necessary changes using the same commands as when iterating on it before. Once
you feel it is once again ready to be reviewed you simply request review for
the patch again using `gps rr` or `gps request-review`. You continue this
process until you have addressed all the issues and the patch has been
approved.

## Integrate the patch

Once the patch has been approved you are ready to integrate it upstream so that
the rest of the team can continue building on it. This is done with the `gps
int` or `gps integrate` command.

Once you have integrated your patch in. You do it all over again with another
patch.
