# The Concept

The idea of Git Patch Stack at a high level is simply that instead of managing
code changes as isolated feature branches managing them as a stack of logical
patches. This extends out through the peer review process as well.

Probably the biggest difference in thinking is that we have to stop thinking in
terms of branches or feature branches and start thinking in terms of a stack of
logical code changes, a.k.a. patches.

## No Branches

The primary goal of Git Patch Stack is to eliminate the management of branches
or even having to think about the concept of branches. This is the main goal
because there are a lot of downsides to branching. For example feature branches
make valuable peer review very difficult if not impossible due to the amount of
scope they include.

Feature branches also go against software maintainability & tooling best
practices due to being scoped at the product level rather than the software
architecture level. Beyond that they are designed strictly to isolate code
changes away from each other. We have known for a long time now that delaying
integration of software changes not only makes the integration process more
difficult later on, but also prevents knowledge sharing and the natural
evolution of an application architecture. For more details on this and where
the concept of Git Patch Stack came from please see, [Journey to Small Pull
Requests][].

This isn't to say that branches are a horrible thing that should never be used.
However, it is to say that we should be aware of the pros & cons of all our
tools and processes, so we can use the ideal tools & processes for the job at
hand. Git Patch Stack does **not** prevent the use of branches, it simply
facilitates using a more ideal tool and process for the 99.99% of time where
you don't need or want to have to use branches.

## Mental Model

In Git Patch Stack instead of branches & commits we focus on a **stack of
patches**.

A **patch** is simply a logical change to source code. From a technical
standpoint a patch is simply a Git commit that exists on your **patch stack**.
There are ideal characteristics that patches should strive to have. For
example, being small, logical, build-able, testable, releasable, and having a
good message. We will get into the specifics of these characteristics later and
tools and techniques to help facilitate them.

A **patch stack** is what it sounds like, a stack of patches. Think of it like
a stack of papers. You can add new patches to the top of the stack, reorder the
patches in the stack, squash patches together, split a patch apart into
multiple patches or even drop patches you no longer want. These operations are
extremely useful for evolving patches. From a technical standpoint a **patch
stack** is the Git commit history on a remote tracking branch between the local
branches *HEAD* and the tracked remote branches *HEAD*. You can have as many
patch stacks as you like, however generally you work off of `main` as your
primary patch stack.

Once you have iterated on a patch to the point where you feel it is ready for
review you naturally **request review** of that patch. Let's say you get some
good feedback as part of that review. Therefore you make some more changes to
the patch using the operations from above. Then you simply **request review**
of the updated patch.

After receiving a final review with an approval, you likely want to share that
patch with the rest of your team. This is done by **integrating** the patch.
From a technical standpoint **integrating** is pushing the particular patch up
to the upstream of your patch stack.

Git Patch Stack enables you to focus on creating, evolving, reviewing, and
integrating patches which aids with creating code changes that are easy to
review, aligned with the application architecture, scoped logically, facilitate
efficient development, and support long term software maintainability.

[Journey to Small Pull Requests]: https://engineering.uptechstudio.com/blog/journey-to-small-pull-requests/ 
