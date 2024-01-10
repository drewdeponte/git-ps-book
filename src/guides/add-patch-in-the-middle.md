# Add a patch in the middle

Another common action you will want to perform when working with a stack of
patches is being able to add a patch at a specific point in the middle of your
stack.

This is beneficial over adding a patch on top of the stack and reordering it
into its correct position because it makes it so that when you are creating
your patch it is based on the correct dependent code and not code that is only
introduced higher up in the stack. It also has the benefit of forcing you to
properly integrate changes higher up in the stack with your newly introduced
patch.

This operation is really just a specific use case of a Git interactive rebase.
So as with most of these operations being comfortable with Git's interactive
rebase is key.

## TL;DR

For those who just want a quick reminder reference here is the TL;DR. For those
who need a bit more context and detail through the walk through read the
sections below.

- `gps rebase` - do an interactive rebase of the patch stack & mark the patch
  you want to add a new patch after with `edit`, it will drop you out into the
  shell at that patch
- make your changes to the code
- `git add` - stage changes you want in the patch
- `git commit` - create the patch
- `git rebase --continue` - continue the rebase to play the other commits on
  top of the new commits you created

## Initial State

For this example lets assume that we have a Patch Stack that has the following
patches. 

```
✔ gps ls
2           3d490f Add car() function
1           3e483a Add bar() function
0           20d08a Add foo() function
```

As we can see from the first patch `Add foo() function`, it adds the `foo()`
function.

```
✔ gps show 0                                                                           0m main fcff7bc
commit 20d08af5aecde6126398208dc5ea16fe87eeb7e6
tree c7f299cc61fab8bcd89c7a003322779b63246b07
parent a34b62b61d1675073eb1df953f1cdfc01232cceb
author Drew De Ponte <cyphactor@gmail.com> 1657069825 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1657069825 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmLE4QkACgkQQcXSxuWv
 lEzj3Af/aeJgWK3j2RuYwRU+W5lF/oiKYukpIDGrtFkyLMPXglWD/TFqfh35Cf+q
 z+GSwC4WrHcOdPXf6geJ6iXDMfu8i0WI1A16xu9d6299uPgpTRUOdC8g3IW6MO/V
 Iv7CBITdGkdPrPznSN1xDjvarmYpjYJsSXravn6uvK26TKJDP0hDSREWO4LM/prI
 d12TEaRm4OtWDDhCMh7HvuYyr0lJXdAsO/HpPjNadqTT1RvJ5O52RypSx79tIe7Q
 cPY+N/CPLa1YaQVkbO0pl8YaIhz+bh+m51xNizgbutb/pvnMtyIALYrffVQzDPlW
 e1BZRP+9ux+aw8w6CN5+0OU8BFax5w==
 =3wZ5
 -----END PGP SIGNATURE-----

    Add foo() function

diff --git a/src/main.rs b/src/main.rs
index e7a11a9..b5a8cf4 100644
--- a/src/main.rs
+++ b/src/main.rs
@@ -1,3 +1,7 @@
 fn main() {
     println!("Hello, world!");
 }
+
+fn foo() {
+  println!("Foo");
+}
```

The second patch, `Add bar() function`, adds the `bar()` function as seen in
the diff below.

```
✔ gps show 1
commit 3e483a19203cffae28cb8a99f989c0c1d250eda8
tree 443074d262fc9d86048becf7d6f436ef799b9ced
parent 20d08af5aecde6126398208dc5ea16fe87eeb7e6
author Drew De Ponte <cyphactor@gmail.com> 1657069869 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1657070294 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmLE4tgACgkQQcXSxuWv
 lEzdWAf9G6XI/DJmIL+Ws4mtyLP07MaGqybt0DrOdgPusjTKMrkZbj6sFD4CuSMk
 LiZEsL4oAQRo3XtxBd5ggh8S4DQwusn/KZf4oJq9GvhJJylFJbg5Qe5b4/HNKMGJ
 NA04+uDI1V+ZSsMgTPSBtpFq0vw/xPc7sG8uas+ESUNJ+91aAtPSBuVXFpnPYkJ1
 +j7UB0I9JMyhBQtNyZ9BI+0biE5yDmepUzg/1InKQJvQOLPjHN4IbfGraAcw8Dym
 6C9AU79/e8VlEHJd5OLfgqjMISh+jPEDqBsIgRuppcFTpm0F1OzVv4877RR42llL
 uA6XFe5iR8ciy9mVihROZGh05X069Q==
 =Weyt
 -----END PGP SIGNATURE-----

    Add bar() function

diff --git a/src/main.rs b/src/main.rs
index b5a8cf4..84be80e 100644
--- a/src/main.rs
+++ b/src/main.rs
@@ -5,3 +5,7 @@ fn main() {
 fn foo() {
   println!("Foo");
 }
+
+fn bar() {
+  println!("Bar");
+}
```

The third patch, `Add car() function` adds the `car()` function in the diff
below.

```
✔ gps show 2
commit 3d490f84e505c2da67d52624d84357de6ad1a746
tree 2206a333eaa4329e8762976bc9e462a29fbe48fa
parent 3e483a19203cffae28cb8a99f989c0c1d250eda8
author Drew De Ponte <cyphactor@gmail.com> 1657069940 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1657070300 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmLE4twACgkQQcXSxuWv
 lEztJwgAztuVEGj1/9pguOiV6VhPrbtEIznWIlPvik4aL4/0U2DEW7sK8ZzTnl7d
 lj/JEMoKEq32CR1oAxqk8iUwiShtggyhjic+HjgKg+CYOQQ3LycEhD1c85vm8+Pk
 MEWWuzzzf/vgEZqb9mIAO9IvpVqcnj7sMyrKMP/WvIhWDpw+lpyNjJeXpg2dY8Oz
 dUB3m4FnUlycaxsEOxK5GlDu9AwTbtKALTkVA4eioqM3rervqcgOJnap07E4zWHH
 vnDrCjfaAn4ckzHJwgNnAVLXWshKzfuq48WvNN0Bvo7wb4YwVEX+NkUE9d74BVLR
 p5Y1CrJmYLF0xjsOIF3bb0Yj95NoCw==
 =MnJj
 -----END PGP SIGNATURE-----

    Add car() function

diff --git a/src/main.rs b/src/main.rs
index 84be80e..2282948 100644
--- a/src/main.rs
+++ b/src/main.rs
@@ -9,3 +9,7 @@ fn foo() {
 fn bar() {
   println!("Bar");
 }
+
+fn car() {
+  println!("Car");
+}
```

## Add the foobar() function

Let us say for sake of discussion we want to add a new function, `foobar()`
that is composed of the `foo()` and `bar()` functions respectively. So we want
to write something like the following.

```
fn foobar() {
	foo();
	bar();
}
```

### Edit Mode

To accomplish this we need to utilize an interactive rebase to enter "edit"
mode in the correct place in the Patch Stack. In this particular case we
want to rebase our Patch Stack.

```
gps rebase
```

This will bring up the following in your editor.

```
pick 20d08af Add foo() function
pick 3e483a1 Add bar() function
pick 3d490f8 Add car() function

# Rebase a34b62b..3d490f8 onto a34b62b (3 commands)
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

In the interactive rebase buffer we want to change the action for the `Add
bar() function` patch to `edit`, so it is as follows.

```
pick 20d08af Add foo() function
edit 3e483a1 Add bar() function
pick 3d490f8 Add car() function

# Rebase a34b62b..3d490f8 onto a34b62b (3 commands)
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

When you save & quit the editor it will run the specified interactive rebase
commands. In this case pick (meaning keep) the first patch and then stop on the
second patch allowing for editing because we specified, `edit`. When it does
this it will drop you back to the console with a message similar to the
following:

```
Stopped at 3e483a1...  Add bar() function
You can amend the commit now, with

  git commit --amend '-S'

Once you are satisfied with your changes, run

  git rebase --continue

```

**Note:** This drops you right after the patch (a.k.a. commit) that was marked
for `edit` in the interactive rebase. We can see this if we look at the Git
tree and look for `HEAD` as it shows which commit we are currently checked out
on.

```
✔ git la
* 3d490f8 - G - (main) Add car() function (20 minutes ago) <Drew De Ponte>
* 3e483a1 - G - (HEAD) Add bar() function (22 minutes ago) <Drew De Ponte>
* 20d08af - G - Add foo() function (22 minutes ago) <Drew De Ponte>
* a34b62b - G - (origin/main) Initial skeleton (10 months ago) <Drew De Ponte>
```

### Add `foobar()` function patch

Now that we know that we are in the middle of a rebase, and we know where we
are located in terms of the patches. We are ready to simply create a new patch
right where we are.

When we open the `src/main.rs` file to add the new `foobar()` function we see
the following.

```
fn main() {
    println!("Hello, world!");
}

fn foo() {
  println!("Foo");
}

fn bar() {
  println!("Bar");
}
```

**Note:** We do **NOT** see the `car()` function. That is because that patch is
above our current location in the stack. Which is exactly what we want.

So we add the `foobar()` function so our code is as follows.

```
fn main() {
    println!("Hello, world!");
}

fn foo() {
  println!("Foo");
}

fn bar() {
  println!("Bar");
}

fn foobar() {
  foo();
  bar();
}
```

Then we stage the change with `git add` and create the patch with `git commit`
as we normally would. After creating the patch if we look at the Git tree we
will see the following.

```
✔ git la
* 60e2c40 - G - (HEAD) Add foobar() function (11 seconds ago) <Drew De Ponte>
| * 3d490f8 - G - (main) Add car() function (30 minutes ago) <Drew De Ponte>
|/
* 3e483a1 - G - Add bar() function (31 minutes ago) <Drew De Ponte>
* 20d08af - G - Add foo() function (32 minutes ago) <Drew De Ponte>
* a34b62b - G - (origin/main) Initial skeleton (10 months ago) <Drew De Ponte>
```

Here we can see the new `Add foobar() function` patch but we can also see that
the `Add car() function` patch isn't stacked on top of it yet. This is because
we are still in the middle of the rebase.

### Finish the Rebase

To replay the rest of the patches on top of the new patch(es) we just created
we simply run the following.

```
gps rebase --continue
```

### Potential Conflicts

Depending on the changes you made you may run into conflicts that you created
with the patches above. This is actually exactly what you want because if you
made the change in the correct location in your stack then you want it to force
you to integrate the above patches with the new change.

In the case of our example we get the following output telling us there is a
conflict.

```
Auto-merging src/main.rs
CONFLICT (content): Merge conflict in src/main.rs
error: could not apply 3d490f8... Add car() function
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 3d490f8... Add car() function
Error: RebaseFailed(ExitStatus(1))
```

This happened because the `foobar()` function definition that we introduced
into the `src/main.rs` file was added in the same location that the `car()`
function definition was created before.

If we look at `src/main.rs` right now it looks like the following.

```
fn main() {
    println!("Hello, world!");
}

fn foo() {
  println!("Foo");
}

fn bar() {
  println!("Bar");
}

<<<<<<< HEAD
fn foobar() {
  foo();
  bar();
=======
fn car() {
  println!("Car");
>>>>>>> 3d490f8 (Add car() function)
}
```

We can resolve this by simply moving the `car()` definition down below
`foobar()` and remove the conflict markers like so.

```
fn main() {
    println!("Hello, world!");
}

fn foo() {
  println!("Foo");
}

fn bar() {
  println!("Bar");
}

fn foobar() {
  foo();
  bar();
}

fn car() {
  println!("Car");
}
```

Then we simply stage the conflict resolution state with `git add` and request
that it continue the rebase with `git rebase --continue`.

Since we have resolved all the conflicts we see the following out.

```
✔ gps rebase --continue
[detached HEAD 87903c9] Add car() function
 1 file changed, 4 insertions(+)
Successfully rebased and updated refs/heads/main.
```

If we list our Patch Stack it will now look as follows.

```
✔ gps ls
3           87903c Add car() function
2           60e2c4 Add foobar() function
1           3e483a Add bar() function
0           20d08a Add foo() function
```

And we have successfully added a patch into the middle of our stack!
