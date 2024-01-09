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

[branch]
verify_isolation = true
push_to_remote = false

[list]
add_extra_patch_info = false
extra_patch_info_length = 10
reverse_order = false
alternate_patch_series_colors = true
# patch_background.color # default No Color so have commented out
patch_background.alternate_color.RGB = [58, 58, 58]
patch_foreground.color.RGB = [248, 153, 95]
#patch_foreground.alternate_color # default No Color so have commented out
patch_index.color.RGB = [237, 199, 99]
# patch_index.alternate_color # default No Color so have commented out
patch_sha.color.RGB = [157, 208, 108]
# patch_sha.alternate_color # default No Color so have commented out
# patch_summary.color # default No Color so have commented out
# patch_summary.alternate_color # default No Color so have commented out
patch_extra_info.color.RGB = [109, 202, 231]
# patch_extra_info.alternate_color # default No Color so have commented out
```

The following is a breakdown of the supported settings.

- `pull.show_list_post_pull` - (**true**/**false** default: **false**) - controls whether the `pull` command will show the patch list after successfully pulling
- `request_review.verify_isolation` - (**true**/**false** default: **true**) - if **true** the `request-review` command will run the `isolate` command & it's hooks to verify the patch is isolated prior to requesting review. If the isolation verification fails it errors preventing you from requesting review.
- `integrate.verify_isolation` - (**true**/**false** default: **true**) - if **true** the `integrate` command will run the `isolate` command & it's hooks to verify the patch is isolated prior to integrating it. If the isolation verification fails it errors preventing you from integrating the patch.
- `integrate.prompt_for_reassurance` - (**true**/**false** default: **true**) - if **true** the `integrate` command will present the user with the patch details and prompt the user asking them if they are sure they want to integrate the patch. If they say yes, then it moves on the with integration. If not it aborts the integration.
- `integrate.pull_after_integrate` - (**true**/**false** default: **false**) - if **true** the `integrate` command will `pull` after a successful integration.
- `fetch.show_upstream_patches_after_fetch` - (**true**/**false** default: **true**) - if **true** the `fetch` command will show the upstream patches that were fetched.
- `branch.verify_isolation` - (**true**/**false** default: **true**) - if **true** the `branch` command will run the `isolate` command & it's hooks to verify the patch(es) are isolated prior to creating and possibly pushing the branch. If the isolation verification fails it errors preventing you from creating the branch.
- `branch.push_to_remote` - (**true**/**false** default: **false**) - if **true** the `branch` command will push the branch to the remote with the same name automatically. If **false** it will only create the local branch.
- `list.add_extra_patch_info` - (**true**/**false** default: **false**) - if **true** the `list` command will include extra patch information from the `list_additional_info` hook. See [Hooks](hooks.md) for more details.
- `list.extra_patch_info_length` - (**integer** default: **10**) - the width of the additional information column in the output of `gps list`. If the output is longer it will get truncated. See [Hooks](hooks.md) for more details.
- `list.reverse_order` - (**true**/**false** default: **false**) - if set to **true** it will reverse the order in which `gps list` presents the patches in the stack. Some people use this option to make the patch order match the order patches are presented within interactive rebases.
- `list.alternate_patch_series_colors` - (**true**/**false** default: **true**) - if set to **true** it will alternate the colors of the patches using the configured alternate colors when listing out the patches with `gps list`.
- `list.patch_background.color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches background in the `gps list` output. To have **No Color** simply don't specify the config in your configuration file.
- `list.patch_background.alternate_color.RGB` - (default: `[58, 58, 58]`) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches alternate background in the `gps list` output. This is used when the `list.alternate_patch_series_colors` option is enabled.
- `list.patch_foreground.color.RGB` - (default: `[248, 153, 95]`) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches foreground in the `gps list` output.
- `list.patch_foreground.alternate_color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches alternate foreground color in the `gps list` output. This is used when the `list.alternate_patch_series_colors` option is enabled. To have **No Color** simply don't specify the config in your configuration file.
- `list.patch_index.color.RGB` - (default: `[237, 199, 99]`) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches index in the `gps list` output.
- `list.patch_index.alternate_color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches alternate patch index color in the `gps list` output. This is used when the `list.alternate_patch_series_colors` option is enabled. To have **No Color** simply don't specify the config in your configuration file.
- `list.patch_sha.color.RGB` - (default: `[157, 208, 108]`) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches sha in the `gps list` output.
- `list.patch_sha.alternate_color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches alternate sha color in the `gps list` output. This is used when the `list.alternate_patch_series_colors` option is enabled. To have **No Color** simply don't specify the config in your configuration file.
- `list.patch_sha.color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches summary in the `gps list` output. To have **No Color** simply don't specify the config in your configuration file.
- `list.patch_sha.alternate_color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches alternate summary color in the `gps list` output. This is used when the `list.alternate_patch_series_colors` option is enabled. To have **No Color** simply don't specify the config in your configuration file.
- `list.patch_extra_info.color.RGB` - (default: `[109, 202, 231]`) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches extra info in the `gps list` output.
- `list.patch_extra_info.alternate_color.RGB` - (default: **No Color**) an RGB value (e.g. `[58, 58, 58]`) specifying the color to use for a patches alternate extra info color in the `gps list` output. This is used when the `list.alternate_patch_series_colors` option is enabled. To have **No Color** simply don't specify the config in your configuration file.

## Colors

All `color` and `alternate_color` config options can be set either with
RGB values as follows.

```
patch_index.color.RGB = [255, 232, 18]
patch_index.alternate_color.RGB = [15, 30, 50]
```

Or with color names.

```
patch_index.color = "White"
patch_index.alternate_color = "Blue"
```
