# GitLab Duo CLI on Termux (Android)

This README documents the working setup for running GitLab Duo CLI on Android/Termux using the official Linux ARM64 binary through `glibc-runner`.

> Scope: GitLab CLI / GitLab Duo CLI only.

## 1. Environment

Working setup:
- Android + Termux
- ARM64 / aarch64
- `glab`
- `glibc-runner`
- GitLab Duo CLI 9.24.0

GitLab's automatic Duo installer rejects Android, so the official Linux ARM64 binary is installed manually.

## 2. Install GitLab CLI

```bash
pkg install glab-cli
```

Verify:

```bash
glab --version
```

If Termux's mirror is unavailable:

```bash
termux-change-repo
```

Select a working mirror, then retry the install.

## 3. Authenticate GitLab CLI

```bash
glab auth login
```

Use GitLab.com and device authentication if offered.

Verify:

```bash
glab auth status
```

On Android, the Termux keyring may be unavailable, so `glab` can store credentials in:

```text
~/.config/glab-cli/config.yml
```

Keep this file private.

## 4. Why manual Duo installation is needed

This normally fails on Android:

```bash
glab duo cli --install
```

with:

```text
Unsupported platform: operating system android
```

GitLab nevertheless publishes a Linux ARM64 Duo CLI binary. `glibc-runner` lets that GNU/Linux binary run under Termux.

## 5. Check glibc-runner

```bash
glibc-runner --help
```

The working setup used `glibc-runner v2.0`.

## 6. Download the official Linux ARM64 Duo binary

GitLab's Duo CLI package is published by the GitLab `gitlab-lsp` project.

The package API can be queried with:

```bash
curl -fsSL "https://gitlab.com/api/v4/projects/46519181/packages?package_name=duo-cli&package_type=generic&order_by=created_at&sort=desc&per_page=1"
```

For this setup the package was:
- Version: `9.24.0`
- Package ID: `70397337`
- Binary: `duo-linux-arm64`

The verified package-file URL used was:

```text
https://gitlab.com/gitlab-org/editor-extensions/gitlab-lsp/-/package_files/356530926/download
```

Create the bin directory:

```bash
mkdir -p ~/bin
```

Download:

```bash
curl -fL --progress-bar -o ~/bin/duo-bin "https://gitlab.com/gitlab-org/editor-extensions/gitlab-lsp/-/package_files/356530926/download"
```

Package versions and file IDs can change. For a future reinstall, query the latest package first rather than assuming these values remain current.

## 7. Verify the binary

Expected SHA-256 for the exact 9.24.0 binary used here:

```text
f50247099e11e28fc457405d38d93974df8c61358b6327949ce690eccffd2a44
```

Verify:

```bash
sha256sum ~/bin/duo-bin
```

Also verify architecture:

```bash
file ~/bin/duo-bin
```

It should report an ELF 64-bit ARM aarch64 GNU/Linux executable.

Make executable:

```bash
chmod +x ~/bin/duo-bin
```

## 8. Test through glibc-runner

```bash
glibc-runner ~/bin/duo-bin --version
```

Expected:

```text
9.24.0
```

## 9. Create the convenient `duo` command

Create the wrapper:

```bash
cat > ~/bin/duo <<'EOF'
#!/data/data/com.termux/files/usr/bin/bash
exec glibc-runner "$HOME/bin/duo-bin" "$@"
EOF
```

Make it executable:

```bash
chmod +x ~/bin/duo
```

Make sure `~/bin` is in PATH:

```bash
echo "$PATH" | tr ':' '\n' | grep -Fx "$HOME/bin"
```

Then:

```bash
duo --version
```

Expected:

```text
9.24.0
```

## 10. Start interactive Duo CLI

Go to a directory:

```bash
cd /storage/emulated/0/Documents/duotest
```

Start Duo:

```bash
duo
```

A successful startup should look similar to:

```text
GitLab Duo CLI v9.24.0
User: @YOUR_USERNAME (glab OAuth)
GitLab Duo access: ✓ Available
```

You can then type normal prompts directly.

## 11. Default Duo namespace

If startup reports:

```text
GitLab Duo access: ✗ Unavailable
```

and:

```text
No default namespace found
```

configure a default GitLab Duo group in GitLab web settings:

```text
GitLab → Preferences → Behavior → Default GitLab Duo group
```

Select an eligible namespace/group and save.

Restart:

```bash
duo
```

The successful setup showed:

```text
GitLab Duo access: ✓ Available
```

## 12. Empty/non-Git directories

A directory such as:

```text
/storage/emulated/0/Documents/duotest
```

does not have to be a GitLab repository for basic interactive Duo access.

You may see:

```text
Could not find GitLab remote info in project
```

This is a warning that GitLab project-specific context is unavailable. It is not a binary/setup failure.

For project-specific features, use a GitLab repository:

```bash
git clone <your-gitlab-repository>
cd <repository>
duo
```

## 13. Headless mode

Check syntax:

```bash
duo run --help
```

The goal option is:

```text
-g, --goal <goal>
```

Correct:

```bash
duo run --goal "Explain this project"
```

Do not use a positional prompt such as:

```bash
duo run "Explain this project"
```

That produces a `too many arguments` error in this version.

## 14. Useful commands

Version:

```bash
duo --version
```

Help:

```bash
duo --help
```

Interactive mode:

```bash
duo
```

Headless help:

```bash
duo run --help
```

GitLab authentication:

```bash
glab auth status
```

GitLab repositories:

```bash
glab repo list
```

## 15. Troubleshooting

### `Unsupported platform: operating system android`

The automatic installer does not recognize Android.

Use the official Linux ARM64 binary manually with `glibc-runner`.

### `incorrect flag or binary to run`

Check:

```bash
glibc-runner --help
```

Then test the actual binary:

```bash
glibc-runner ~/bin/duo-bin --version
```

Keep `duo-bin` (real binary) separate from `duo` (wrapper).

### `No default namespace found`

Configure the default GitLab Duo group in:

```text
GitLab → Preferences → Behavior → Default GitLab Duo group
```

### `Could not find GitLab remote info in project`

The current directory is not detected as a GitLab repository. Basic conversational use can still work.

## 16. Reinstall checklist

1. Install/authenticate `glab`.
2. Ensure `glibc-runner` works.
3. Query the latest Duo CLI package.
4. Download the official `duo-linux-arm64` binary.
5. Verify its SHA-256.
6. Save it as `~/bin/duo-bin`.
7. Create `~/bin/duo` wrapper using `glibc-runner`.
8. Run:

```bash
duo --version
```

9. Start:

```bash
duo
```

10. If Duo access is unavailable because no default namespace exists, configure the default GitLab Duo group.

## 17. Working architecture

```text
Android
  |
  └── Termux
       |
       ├── glab
       |    └── GitLab OAuth authentication
       |
       ├── ~/bin/duo
       |    └── wrapper
       |
       ├── ~/bin/duo-bin
       |    └── official GitLab Duo CLI
       |        Linux ARM64 binary
       |
       └── glibc-runner
            └── runs the Linux binary
```

## Notes

- Keep GitLab credentials, tokens, and `~/.config/glab-cli/config.yml` private.
- The exact Duo CLI version and package-file ID change over time.
- The reusable workaround is: official `duo-linux-arm64` binary + `glibc-runner` + `~/bin/duo` wrapper.
