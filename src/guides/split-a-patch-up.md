# Split a patch up

When you first discover the importance of logically structured Git commits
(a.k.a. patches), which is by the way a core fundamental expectation & design
characteristic of Git and how it is intended to be used.

It naturally leads to the question, "How do I split a patch up into multiple
patches?" This is crucial because I know I am not going to get this right the
first try. Below I present a simple contrived example so that you can learn the
mechanics and process of doing this, as the mechanics & process don't change.

The other topic related to this which this guide **does not** cover is the
process of taking code and splitting it up into logical chunks. This generally
takes an understanding of the application architecture & the dependency
relationship between the various elements.

So lets get to it.

## TLDR

For those who just want a quick reminder reference here is the TLDR. For those
who need a bit more context and detail through the walk through read the
sections below.

- `gps rebase` - do an interactive rebase of the patch stack & mark the patch
  you want to split for `edit`, it will drop you out into the shell at that
  patch
- `git reset HEAD^` - soft reset the patch
- `gps add -p` - stage just the parts you want in the first patch
- `gps c` - create the first patch
- `gps add -p` - stage just the parts you want in the next patch
- `gps c` - create the next patch
- `gps rebase --continue` - continue the rebase to play the other commits on
  top of the new commits you created

## Initial State

For this example lets assume that we have a Patch Stack that has the following
patches. 

```
2           aaaba1 Add first paragraph to README.md
1           e2e6a6 Add README.md subtitle & description
0           5a3538 Add Readme Title
```

As we can see from the first patch `Add Readme Title`, it adds the title to the
README.

```
✔ gps show 0
commit 5a3538336bfa070648f4a221dd66f58d171e0372
tree ae384913fa2fd8ae03b4509817e78b8d6dc3677c
parent a34b62b61d1675073eb1df953f1cdfc01232cceb
author Drew De Ponte <cyphactor@gmail.com> 1656473449 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1656473449 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmK7x28ACgkQQcXSxuWv
 lEwG4gf/TAyEypLa/yzKcOO791jL1ew0jGS1ZzaJjQHRYkdNte3jZK/703wDrJX5
 HGR/csZnpR7LV7FByeaCWzoXPtIiIr/lQ89xEHk+E8BpGKkcgxVGgF27BFJ/vAHh
 LGEgbgbHNHDvesoyt85oN7trfvKbI7sm8p4NYL9rLb4rovy/2x5VMSOGvHjKL5Rn
 eTCWv9AuQxVjvphZgoAa2RwPP9fWzToy8CVO2uwbXBptlzEGm9s7Rsz4J4Q3WwHk
 FPAMDIlJlmpn6bPFlY7V5hnt/JT4Qkysk/nCisfQ8OwmQ7ZAhv0qwAKe4eor2OI6
 +0sR5IHqUXkXv0EEuCr8Td3W2I9G9A==
 =VlR/
 -----END PGP SIGNATURE-----

    Add Readme Title

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..db58698
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+# Readme Title
```

The second patch, `Add README.md subtitle & description`, adds the
subtitle and the description as seen in the diff below.

```
✔ gps show 1
commit e2e6a6f470651b6a32940f57eba74582ac26fcca
tree 527436c6fdac33c0944aeb659f127ad12523a768
parent 5a3538336bfa070648f4a221dd66f58d171e0372
author Drew De Ponte <cyphactor@gmail.com> 1656473483 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1656473483 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmK7x5YACgkQQcXSxuWv
 lExNrggA2NEk3q8PqdyLcLN8wYN1QDo95HEbncjI1JrGKp8R+jbU5z5XxXM5LPgY
 lyq0fsViJjEMswZajNWeM9BaQ0drNrs6GK5B/1cgjlcZkkC41zvNNcDQdYOYZr9c
 pbriYf9VVXeX8l9FzjeZEGmdzCIZi1wKu6WovKFNx1bNHtTND3RQkrD8zWkHajKp
 I6HsN62Wr7u4hZPa0tqXzyfqPQZ6cSXuikCzoZLbWha1Fq2q3aVyTsazW3zMsXm6
 8jpEc+vge3GDDKVt+m9Tn1XrW//u13q+pyBJ44UxGsb4q6to7LetN+ttbAyCYvbZ
 ws4Thz4bwjFvV9eONMbtR3VhnH4UDg==
 =nFgy
 -----END PGP SIGNATURE-----

    Add README.md subtitle & description

diff --git a/README.md b/README.md
index db58698..b8c4d95 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,5 @@
 # Readme Title
+
+## Subtitle
+
+Here is the description
```

The third patch, `Add first paragraph to README.md` adds the first paragraph in
the diff below.

```
✔ gps show 2
commit aaaba12cb36e3239b90703a6777c9ee66a1ced14
tree f5ba2770179a826bb0fb63cf33dd2c6952812adf
parent e2e6a6f470651b6a32940f57eba74582ac26fcca
author Drew De Ponte <cyphactor@gmail.com> 1656473563 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1656473563 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmK7x+MACgkQQcXSxuWv
 lEwVBQgAnQl7Gzl6X4J0rz1eOVpLweO1tWCp7ScrzHiWStvNUuSNpJ8sayJAcD5E
 n97CZ/olOUcC0qo+yWWOv5qSqt+zxPIgh3bCiN7FtMdGyNcr8wS6Ph4H6ZYKV0Re
 J2jFvm7//8h6KxwUVvW7OPz+CFnVrSSNjF4etvmwDReyFo63TJZepczUCSsGUKvy
 0PtXU2ixyvbLctVfj6XhSe/3pkAKv04H88/4lX8f+GYN7Sh/Ml4wQ6gXYSyv7S5X
 vmmu7neYuwBQXF5HlCGJn72xJIEZJBKGoDkT3LdtKyiSVhdgl27Fb+9NbzkIs5e9
 TN0sCffFdMiPRrVPwaeKLfAU7zbQEQ==
 =kFns
 -----END PGP SIGNATURE-----

    Add first paragraph to README.md

diff --git a/README.md b/README.md
index b8c4d95..2ebe963 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,7 @@
 ## Subtitle

 Here is the description
+
+## First Paragraph
+
+Here is the first paragraph.
```

## Non-Logically Structured Patch

Looking at the patch summaries & the diffs themselves we can see that the
second commit, `Add README.md subtitle & description` is actually
doing two logical things. First it is adding the subtitle. Secondly it is
adding the description to the README.

```
✔ gps show 1
commit e2e6a6f470651b6a32940f57eba74582ac26fcca
tree 527436c6fdac33c0944aeb659f127ad12523a768
parent 5a3538336bfa070648f4a221dd66f58d171e0372
author Drew De Ponte <cyphactor@gmail.com> 1656473483 -0400
committer Drew De Ponte <cyphactor@gmail.com> 1656473483 -0400
gpgsig -----BEGIN PGP SIGNATURE-----

 iQEzBAABCAAdFiEEZ4qmf6cBz6fWZrzoQcXSxuWvlEwFAmK7x5YACgkQQcXSxuWv
 lExNrggA2NEk3q8PqdyLcLN8wYN1QDo95HEbncjI1JrGKp8R+jbU5z5XxXM5LPgY
 lyq0fsViJjEMswZajNWeM9BaQ0drNrs6GK5B/1cgjlcZkkC41zvNNcDQdYOYZr9c
 pbriYf9VVXeX8l9FzjeZEGmdzCIZi1wKu6WovKFNx1bNHtTND3RQkrD8zWkHajKp
 I6HsN62Wr7u4hZPa0tqXzyfqPQZ6cSXuikCzoZLbWha1Fq2q3aVyTsazW3zMsXm6
 8jpEc+vge3GDDKVt+m9Tn1XrW//u13q+pyBJ44UxGsb4q6to7LetN+ttbAyCYvbZ
 ws4Thz4bwjFvV9eONMbtR3VhnH4UDg==
 =nFgy
 -----END PGP SIGNATURE-----

    Add README.md subtitle & description

diff --git a/README.md b/README.md
index db58698..b8c4d95 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,5 @@
 # Readme Title
+
+## Subtitle
+
+Here is the description
```

Instead of the above what we really wanted to have was one patch that adds
the subtitle and a separate patch that adds the description as two isolated
logical chunks.

### Edit Mode

To accomplish this we need to utilize an interactive rebase to enter "edit"
mode in the correct place in the Patch Stack. In this particular case we
want to rebase our Patch Stack.

```
gps rebase
```

This will bring up the following in your editor.

```
pick 5a35383 Add Readme Title
pick e2e6a6f Add README.md subtitle & description
pick aaaba12 Add first paragraph to README.md

# Rebase a34b62b..aaaba12 onto a34b62b (3 commands)
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

In the interactive rebase buffer we can change the action for the middle patch
to `edit` so it as follows.

```
pick 5a35383 Add Readme Title
edit e2e6a6f Add README.md subtitle & description
pick aaaba12 Add first paragraph to README.md

# Rebase a34b62b..aaaba12 onto a34b62b (3 commands)
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
commands. In this case pick (meaning keep) the first patch and then stop on
the second patch allowing for editing because we specified, `edit`. When it
does this will drop you back to the console with a message similar to the
following:

```
Stopped at e2e6a6f...  Add README.md subtitle & description
You can amend the commit now, with

  git commit --amend '-S'

Once you are satisfied with your changes, run

  git rebase --continue
```

### Split the Patch

We want to split the changes currently held in this patch into multiple
patches. To do this we need to reset the patch that we are currently on and
then partially stage the changes back & create a patch, then stage the other part and
create another patch, and then continue the rebase.

So first we have to reset just the patch that we are checked out on and we
want to do a soft reset. So we do the following:

```
git reset HEAD^
```

Now if we run `git diff` to see the now local ustagged changes we see the
following.

```
✔ git di
diff --git a/README.md b/README.md
index db58698..b8c4d95 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,5 @@
 # Readme Title
+
+## Subtitle
+
+Here is the description
```

As we can see we now have the changes that add both the subtitle & the
description locally.

So we first want to stage a patch with just the `## Subtitle` portion. To do
this we need to use `gps add -p README.md` to do a partial stage of the README
files changes to just stage the `## Subtitle` portion. See this post, [git add
patch won't split](https://drewdeponte.com/blog/git-add-patch-wont-split/) for
details on how to accomplish this. This should leave us with the following.

```
+
+Description of the README
```

If you run `gps s` to check on things at this point it should look like this.

```
✔ gps s
interactive rebase in progress; onto a34b62b
Last commands done (2 commands done):
   pick 5a35383 Add Readme Title
   edit e2e6a6f Add README.md subtitle & description
Next command to do (1 remaining command):
   pick aaaba12 Add first paragraph to README.md
  (use "git rebase --edit-todo" to view and edit)
You are currently splitting a commit while rebasing branch 'main' on 'a34b62b'.
  (Once your working directory is clean, run "git rebase --continue")

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
```

If we specifically check out the staged changes with `git diff --staged` we get
the following.

```
✔ git diff --staged
diff --git a/README.md b/README.md
index db58698..b612c4b 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,3 @@
 # Readme Title
+
+## Subtitle
```

This is exactly what we wanted. But lets make sure the unstaged changes also
represent what we want by running `git diff`.

```
✔ git diff
diff --git a/README.md b/README.md
index b612c4b..b8c4d95 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,5 @@
 # Readme Title

 ## Subtitle
+
+Here is the description
```

Yep looks like they do. So now we just need create the first of the two patches
that will replace the patch we marked for edit. This can be done as follows.

```
gps c
```

When it opens the editor for the message we can give it a summary of
`Add subtitle to README`.

From there we can stage the rest of the changes with `gps add README.md` and
create the second patch with the following.

```
gps c
```

When it opens the editor for the message we can give it a summary of
`Add description to README`.

### Finish the Rebase

Now that the changes have been split up into separate patches like we wanted we
now need to instruct it to finish the interactive rebase that we started with
the edit. This is done as follows.

```
gps rebase --continue
```

Once it is complete if we checkout our Patch Stack it will look as follows.

```
✔ gps ls
3           023326 Add first paragraph to README.md
2           8a7fdc Add description to README
1           5aaf4c Add subtitle to README
0           5a3538 Add Readme Title
```

Exactly what we wanted!
