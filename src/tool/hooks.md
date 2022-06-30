# Hooks

Git Patch Stack takes the stance that it shouldn't be bound to a specific
source control management platform (GitHub, Bitbucket, GitLab, etc.) or a
particular request review process. Even across projects.

To give our users this flexibility we have created a **hooks** system for a
number of the commands, e.g. the `request-review` and `isolate` commands. This
allows the users to configure & customize what these commands do.

A hook is simply an executable file (script, binary, etc.) that is named
according to the particular hook name and located in one of the three general
locations for hooks.

- `.git-ps/hooks/` - communal repository specific hooks
- `.git/git-ps/hooks/` - personal repository specific hooks
- `~/.config/git-ps/hooks/` - personal user global hooks

Communal repository specific hooks are searched first, if not found then it
is followed by searching the personal repository specific hooks, and if not
found then it searches in the user's global hooks. This allows a team to
standardize certain hooks across the repository using the communal repository
specific hooks as well configure personalized hooks in their personal
repository specific hooks or general sane default hooks in their user global
hooks.

The following is a list of currently supported hooks and their expected
filenames.

- `request_review_post_sync` - hook executed by `request-review` command after
	successfully syncing the patch to remote - generally used to create a pull
	request / patch email & send it - **Note: This hook is required to be able
	to use the `request-review` command.**
- `isolate_post_checkout` - hook executed by `isolate` command after
	successfully creating the temporary branch, cherry-picking the patch to it,
	and checking the branch out

You can find examples of hooks that you can straight up use or just use as a
starting point in [example_hooks](https://github.com/uptech/git-ps-rs/tree/main/example_hooks).

#### Get Started with Hooks

To get started with hooks lets set up the `request_review_post_sync` hook using
the [super basic GitHub CLI
implementation](https://github.com/uptech/git-ps-rs/tree/main/example_hooks/request_review_post_sync-github-cli-example)
that handles creating the pull request for you as part of the `request-review`
command. 

To start we need to make sure that the Global Hook Directory is created with
the following:

```
mkdir -p ~/.config/git-ps/hooks
```

Then we need to copy the example hook of your choice to the Global Hooks
Directory and give it execute permissions. This can be done with the following.

```
curl -fsSL https://raw.githubusercontent.com/uptech/git-ps-rs/main/example_hooks/request_review_post_sync-github-cli-example --output ~/.config/git-ps/hooks/request_review_post_sync
chmod u+x ~/.config/git-ps/hooks/request_review_post_sync
```

This hook uses the GitHub CLI to interface with GitHub and create the pull
requests. In order for this to work we need to make sure that we have the
GitHub CLI installed and makes sure that we have authenticated it. This can be
done as follows no macOS.

```
brew install gh
gh auth login
```

Once you have setup the hook as described above and installed the GitHub CLI
and authenticated with it to GitHub. You should be all set. When you run the
`request-review` it should use the hook to create a pull request of the patch.
