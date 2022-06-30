# Design Principles

As we started to define Git Patch Stack as a **tool** we used the following
design principles as guiding lights to help constrain and shape it.

- streamline working within the [conceptual model](../conceptual-model.md)
  and working with Git in general
- work with existing defacto peer review tooling
- support stepping outside of Git Patch Stack and back into Git
- facilitate the Continuous Integration Methodology while supporting pre-commit
  peer review

These design principles are the key to Git Patch Stack being such an amazing
tool.

## Streamline working within the conceptual model

Other tools exist that to some degree facilitate working within the mental
model of a stack of patches. However using them generally feels like more work
than what you normally experience with Git itself. To prevent
this we have chosen this design principle to make sure that working in the [conceptual
model](../conceptual-model.md) would not only, not feel like overhead, but feel
even more streamlined than a normal Git workflow.

## Work with existing defacto peer review tools

One of the biggest driving forces for Git Patch Stack was that we wanted a way
to work within the [conceptual model](../conceptual-model.md) without having to
switch whole sale to separate code hosting and peer review provider, e.g.
Phabricator. Therefore, we applied the constraint that it must support defacto
peer review tools that people are used to working with, e.g. GitHub, Bitbucket,
GitLab, Email, etc. Having this requirement also makes it possible for Git
Patch Stack to be used even in environments where a portion of the team is
using Feature Branches.

## Support stepping outside of Git Patch Stack

Another crucial design principle we follow is that we never want Git Patch
Stack to lock you in such that you can only use it. We want you to easily be
able to work inside the [conceptual model](../conceptual-model.md) but if you
need for some reason to step out of that mental model and use Git itself,
Git Patch Stack should make that trivial.

## Facilitate the Continuous Integration Methodology

Last but not least we have chosen the requirement that we must support the
Continuous Integration Methodology as closely as possible while still
supporting a pre-commit peer review as that is what the majority of developers
are used to. The thinking is that this requirement will help us bring
developers as close to 100% pure Continuous Integration but supporting the peer
review process that everyone is used to.

*Note:* It is important to recognize that we are talking strictly about the
Methodology of Continuous Integration and not simply having a continuous
integration server setup running automated tests. Many people don't even
realize that Continuos Integration is and was a Methodology and that the
running of automated tests was only a very small portion of it.
