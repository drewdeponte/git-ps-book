# Request Review of a patch

## TLDR

1. `gps ls` - list out patches in patch stack to the patch index you want to request review of
2. `gps rr <patch-index>` - request review of the patch identified by the given patch-index

## Walkthrough

### Get Patch Index

Before we can request review for a patch. We have to identify which patch we
want to request review of and get its associated index. The best way to do this
is to simply run `gps ls` to list out the patches in the stack with their
associated indices and statuses. An example of this looks as follows.

```
✔ gps ls
1           8a6145 Add function C
0           ba2f4d Add function A & function B
```

### Request Review

In the example above lets say we wanted to request review of teh "Add function
A & function B" patch. We look and it's associated index is `0`. Therefore to
request review of this patch we simply run the following.

```
gps rr 0
```

The above kicks off the request review process for the "Add function A &
function B" patch.

#### Isolation Verification

The fist step in the request review process is to run the
**isolation verification** if the configuration for it is enabled. For
specifics on the configuration checkout the
[Tool - Configuration chapter](../tool/configuration.md).

Isolation Verification is a process where a temporary branch is created that is
based on the upstream base and the patch is cherry-picked into this branch.
This verifies that at least from a Git perspective the patch is independent
enough to successfully be cherry-picked on top of the upstream base.

This however does not verify the patch is truly indendent because it doesn't
address code dependencies. To address this the Isolation Verification process
supports the `isolate_post_checkout` hook. This is a hook that if present gets
executed after cherry-picking the patch into the temporary branch and checking
that branch out. It allows you to provide an `isolate_post_checkout` hook
script that can run linting, test suite, build process, etc. which can help
verify that your patch is actually independent. Details on the hook can be
found in the [Tool - Hooks chapter](../tool/hooks.md).

#### Request Review Branch Creation & Sync

Assuming that the isolaction verification is successful it then moves onto
creating the request review branch (e.g. `ps/rr/some-patch-summary`) based on
the upstream base, cherry-picking the patch into it, and then syncing that
branch with the remote.

If the isolation verification fails the command exits aborting the request
review process.

#### Request Review Post Sync Hook

After syncing the request review branch if the `request_review_post_sync` hook
is configured it will be executed. To find details about this hook and others
check out the [Tool - Hooks chapter](../tool/hooks.md).

This hook is commonly use to automate the creation of pull requests or create a
patch email and send it to a mailing list so that all you have to do is a `gps
rr <patch-index>` and it will take care of the entirety of requesting review of
that patch.

### Update a previosly Requested Review

Generally when you request review of a patch you end up getting some feedback
and need to modify the patch. This is done using all the techniques described
in the other guides. Once the patch has been updated you want to update the
request review with the new version of the patch. This is done by just
requesting review of the patch again as follows.

```
gps rr <patch-index>
```

Thats it!
