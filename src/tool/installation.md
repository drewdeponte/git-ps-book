# Installation

As Git Patch Stack is written in [Rust][] it can be compiled and installed on
many different platforms. Currently, we provide package management via
[Homebrew][] on macOS and [Cargo][] on all platforms. If you don't like either
of those package managers you will have to follow the **Build from Source**
instructions below.

### Install on macOS via Homebrew

To install on macOS we provide a [Homebrew][] tap which provides the
`git-ps-rs` formula. To use it first you need to add the tap as follows.

	brew tap "uptech/homebrew-oss"

This basically registers our tap as another source for packages for your
[Homebrew][]. Enabling you to do things like install the Git Patch Stack
command line tool as follows.

	brew install uptech/oss/git-ps-rs

Because you have registered the tap you can also do useful things like upgrade
your version of the Git Patch Stack command line tool as follows.

	brew update
	brew upgrade git-ps-rs

#### zsh & bash Completions

Our [Homebrew][] formula installs the `zsh` & `bash` completion scripts into the
standard [Homebrew][] shell completions location. So you just need to make sure
that path is configured in your shell configuration. For `zsh` it is generally
something like the following:

	# add the Homebrew zsh completion scripts folder so it will be searched
	fpath=(/opt/homebrew/share/zsh/site-functions/ $fpath)
	# enable completion in zsh
	autoload -Uz compinit
	compinit

### Install on all platforms via Cargo

To install the Git Patch Stack command line tool via [Cargo][] simply run the
following command.

	cargo add gps

### Build from Source

If you don't like either package manager or you just want to build from source
don't fret. You will just have to make sure you have the following build
dependencies installed.

- [Rust][] (macOS: `brew install rust`)

Once you have the build dependencies installed all you should need to do is
run the following command to build the release version of the command line
tool.

	cargo build --release

Once you have built it successfully you can use the `mv` command to move the
`target/release/gps` file into `/usr/local/bin/` or some other location in your
`PATH` environment variable.

#### zsh & bash completions

The `zsh` and `bash` completion scripts are generated as part of the build
process by [Cargo][]'s custom build script, `build.rs` at the root of the
project.

The scripts are output to the [Cargo -
OUT_DIR](https://doc.rust-lang.org/cargo/reference/environment-variables.html)
location, generally `target/release/build/gps-*/out` where the `*` is a hash
value. The files are named as follows.

- `gps.bash` - bash completion script
- `_gps` - zsh completion script

Simply move the files to whatever location on your system you are sourcing for
completion scripts.

[Rust]: https://www.rust-lang.org
[Homebrew]: https://brew.sh
[Cargo]: https://doc.rust-lang.org/cargo/
