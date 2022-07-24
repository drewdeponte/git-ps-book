# Configuration

Git Patch Stack supports various settings via three layers of configuration
files.

- **personal global settings** - `~/.config/git-ps/config.toml` - intended to allow you to define default personal settings for when a repository doesn't specify a setting
- **personal repository settings** - `repo_root/.git/git-ps/config.toml` - intended to allow you to define personal settings constrained to a repository. *Note:* Settings defined in here **override** any values defined in the **personal global settings**.
- **communal repository settings** - `repo_root/.git-ps/config.toml` intended to allow a team to enforce settings for everyone working on a repository. *Note:* Settings defined in here **override** any values defined in the **personal repository settings** or in the **personal global settings**.

The following is an example of a config defining all of the settings. All sections and settings are optional so you don't need to specify them all in each config.

```
[pull]
show_list_post_pull = false

[request_review]
verify_isolation = true

[integrate]
verify_isolation = true
prompt_for_reassurance = true
pull_after_integrate = false

[fetch]
show_upstream_patches_after_fetch = true

[list]
extra_patch_info_length = 10
```

The following is a breakdown of the supported settings.

- `pull.show_list_post_pull` - (**true**/**false** default: **false**) - controls whether the `pull` command will show the patch list after successfully pulling
- `request_review.verify_isolation` - (**true**/**false** default: **true**) - if **true** the `request-review` command will run the `isolate` command & it's hooks to verify the patch is isolated prior to requesting review. If the isolation verification fails it errors preventing you from requesting review.
- `integrate.verify_isolation` - (**true**/**false** default: **true**) - if **true** the `integrate` command will run the `isolate` command & it's hooks to verify the patch is isolated prior to integrating it. If the isolation verification fails it errors preventing you from integrating the patch.
- `integrate.prompt_for_reassurance` - (**true**/**false** default: **true**) - if **true** the `integrate` command will present the user with the patch details and prompt the user asking them if they are sure they want to integrate the patch. If they say yes, then it moves on the with integration. If not it aborts the integration.
- `integrate.pull_after_integrate` - (**true**/**false** default: **false**) - if **true** the `integrate` command will `pull` after a successful integration.
- `fetch.show_upstream_patches_after_fetch` - (**true**/**false** default: **true**) - if **true** the `fetch` command will show the upstream patches that were fetched.
- `list.extra_patch_info_length` - (**integer** default: **10**) - the width of the additional information column in the output of `gps list`. If the output is longer it will get truncated. See [Hooks](./tool/hooks.md) for more details.

