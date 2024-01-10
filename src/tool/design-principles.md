# Design Principles

As we started to define Git Patch Stack as a **tool** we used the following
design principles as guiding lights to help constrain and shape it.

- streamline working within the [Patch Stack Workflow](../patch-stack-workflow.md)
  and working with Git in general
- work with existing de facto peer review tooling
- support stepping outside of Git Patch Stack and back into Git
- facilitate the Continuous Integration Methodology while supporting pre-commit
  peer review
- platform-agnostic
- hosting provider-agnostic
- customizable for a personal development experience

These design principles are the key to Git Patch Stack being such an amazing
tool.

## Streamline working within the conceptual model

Other tools exist that, to some degree, facilitate working within the mental
model of a stack of patches. However, using them generally feels like more work
than what you normally experience with Git itself. To prevent this we have
chosen this design principle to make sure that working in the [Patch Stack
Workflow](../patch-stack-workflow.md) would not feel like overhead, but also
feel even more streamlined than a normal Git workflow.

## Work with existing de facto peer review tools

One of the biggest driving forces for Git Patch Stack was that we wanted a way
to work within the [Patch Stack Workflow](../patch-stack-workflow.md) without
having to switch wholesale to a separate code hosting and peer review provider,
e.g. Phabricator. Therefore, we applied the constraint that it must support de
facto peer review tools that people are used to working with, e.g. GitHub,
Bitbucket, GitLab, Email, etc. Having this requirement also makes it possible
for Git Patch Stack to be used even in environments where a portion of the team
is using Feature Branches.

## Support stepping outside of Git Patch Stack

Another crucial design principle we follow is that we never want Git Patch
Stack to lock you in such that you can only use it. We want you to easily be
able to work inside the [Patch Stack Workflow](../patch-stack-workflow.md) but
if you need for some reason to step out of that mental model and use Git
itself, Git Patch Stack should make that trivial.

## Facilitate the Continuous Integration Methodology

We have also chosen the requirement that we must support the Continuous
Integration Methodology as closely as possible while still supporting a
pre-commit peer review, as that is what the majority of developers are used to.
The thinking is that this requirement will help us bring developers as close to
100% pure Continuous Integration but supporting the peer review process that
everyone is used to.

*Note:* It is important to recognize that we are talking strictly about the
Methodology of Continuous Integration and not simply having a continuous
integration server setup running automated tests. Many people don't even
realize that Continuous Integration is and was a Methodology and that the
running of automated tests was only a very small portion of it.

## Platform Agnostic

Another requirement we have chosen to focus on is being platform-agnostic. This
will allow developers in all different situations to be able to use Git Patch
Stack. It shouldn't matter if you are on macOS, a Linux distribution, or
Windows.

## Hosting Provider Agnostic

Beyond that we have also decided to focus on being hosting provider-agnostic.
This means we will design Git Patch Stack so that it can work with any hosting
provider even for its support for the peer review process. It is of course
possible some providers might not facilitate any integration. If that is the
case there isn't much we can do. But this design choice will make it so that if
it is possible to integrate with the provider it will be possible to use Git
Patch Stack with them.

## Customizable for a Personal Development Experience

Last but not least we are big believers in the concept of Personal Development
Environments and have a personalized development experience customized just to
your liking. Therefore, we have chosen to design Git Patch Stack around this
concept so that you can easily make it part of your personal development
experience.
