# Commit Signing

Git Patch Stack supports Git Commit Signing via the following mechanisms.

- SSH signing
- GPG signing

It does **not** currently support signing via certificates.

## Configuration

Git Patch Stack uses your official Git configuration to determine your Git
Commit Signing configuration and follows suite with that.

Generally this involves having the following configs set in your `~/.gitconfig`.

- `user.signingkey` - for SSH signing this is the path to your SSH signing key, for GPG this is the GPG signing key identifier
- `commit.gpgsign` - `true` to enable either SSH or GPG signing when creating commits
- `tag.gpgsign` - `true` to enable either SSH or GPG signing when creating tags
- `gpg.format` - `ssh` for SSH signing, and `openpgp` for GPG signing
- `gpg."ssh".allowedSignersFile` - path to the allowed signers file, only needed for SSH signing

For further details please reference the Git documentation. The above is not
supposed to provide all the possible details. It is just to provide you
enough information to quickly be able to get started with configuring signing
and be able to quickly look up documentation to refine your configuration.

## Credential Management

If you are using GPG signing, the credential management is handled strictly by
the GPG program that you are using. See the documentation from it for details.

However, if you are using SSH signing. Git Patch Stack securely manages your
credentials for your signing keys so that you don't have to enter the password
for your signing keys every time. It uses platform specific secure credential
management services. See the breakdown by platform below.

- **macOS** - keychain
- **Linux** - secret-service and kernel keyutils
- **FreeBSD & OpenBSD** - secret-service
- **Windows** - credential manager
