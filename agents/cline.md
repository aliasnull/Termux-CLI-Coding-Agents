# Cline CLI on Termux: Installation, Testing, Uninstallation & Cleanup

This guide is for **Android Termux on `aarch64`** using the unofficial
Cline Termux port. It is not the same as installing the standard npm
package on desktop Linux.

> **Important:** The Termux port is unofficial and is not maintained as
> an official Cline release. Review the repository and installer before
> running them. The commands below follow the `IChouChiang/cline-termux`
> project documentation. Check its latest README/release notes before
> recording a video, because installer details can change.

Project: https://github.com/IChouChiang/cline-termux\
Official Cline project: https://github.com/cline/cline

## 1. Check the environment

Run each command:

``` bash
node -v
npm -v
uname -m
echo "$PREFIX"
```

Expected architecture: `aarch64`.

## 2. Update Termux packages

``` bash
pkg update
pkg upgrade
```

Install the basic tools required by the Termux port:

``` bash
pkg install curl nodejs-lts ripgrep tar coreutils
```

If `nodejs-lts` conflicts with an existing Node.js package, read the
package manager's prompt before accepting any removals.

## 3. Install Cline Termux

The project's documented latest-release installer is:

``` bash
curl -fsSL https://github.com/IChouChiang/cline-termux/releases/latest/download/install-cline-termux.sh -o install-cline-termux.sh
```

Inspect the downloaded script before running it:

``` bash
less install-cline-termux.sh
```

If you have reviewed and trust the script, run:

``` bash
bash install-cline-termux.sh
```

Remove the downloaded installer after successful installation:

``` bash
rm -f install-cline-termux.sh
```

Alternative one-line installation, as documented by the project (this
downloads and runs the script directly, so reviewing the script first is
safer):

``` bash
curl -fsSL https://github.com/IChouChiang/cline-termux/releases/latest/download/install-cline-termux.sh | bash
```

## 4. Verify installation

``` bash
command -v cline
cline --version
cline --help
```

The port also documents this check for its private FFI runtime:

``` bash
bun-ffi --version
```

If `cline` is not found immediately after installation, restart the
Termux session or refresh your shell's command lookup cache.

**For Zsh:**

``` zsh
rehash
command -v cline
```

**For Bash (the usual default shell in Termux):**

``` bash
hash -r
command -v cline
```

If you're unsure which shell you're using, check it with:

``` bash
echo "$SHELL"
ps -p $$ -o comm=
```

Then use the matching command above. You can also close and reopen the
Termux session before checking `command -v cline` again.

## 5. Launch Cline

``` bash
cline
```

Or explicitly open the terminal UI:

``` bash
cline --tui
```

Follow Cline's prompts to configure an AI provider/model and
authentication. An API key or supported provider is required; installing
Cline does not automatically mean model usage is free.

## 6. Free models and billing

Free model availability, quotas, and provider options can change. Check
the current Cline UI and official documentation before claiming a
particular model is free or unlimited. Do not assume that all models
shown by a provider are free.

Official Cline site: https://cline.bot/\
Cline CLI documentation: https://github.com/cline/cline

## 7. Uninstall Cline Termux

First exit Cline. Then remove the launcher and the port's private
runtime.

``` bash
rm -f "$PREFIX/bin/cline" "$PREFIX/bin/bun-ffi"
rm -rf "$PREFIX/opt/cline-termux" "$PREFIX/opt/bun-android-ffi"
```

**About Bun:** The `bun-android-ffi` folder is the private FFI runtime
used by this Cline Termux port. It is separate from a normal
`$PREFIX/bin/bun` installation. The commands above do not remove
`$PREFIX/bin/bun`. If you installed `bun-android-ffi` separately for
another purpose, do not remove its folder until you confirm it is not
needed.

## 8. Remove Cline user data and credentials

Cline stores local user data under `~/.cline`, which can include
settings, task/session history, logs, databases, and provider
credentials. This deletion is irreversible unless you have a backup.

Only if you want a completely fresh setup and no longer need that data:

``` bash
rm -rf "$HOME/.cline"
```

The project directory `~/Documents/Cline` is not part of the runtime
install by default. Inspect it before deleting it. If you know it
contains only empty folders created for Cline and want to remove those
empty folders:

``` bash
rmdir "$HOME/Documents/Cline/Hooks" "$HOME/Documents/Cline/Workflows" "$HOME/Documents/Cline"
```

`rmdir` removes only empty directories.

## 9. Optional: inspect leftovers

``` bash
find "$HOME" -maxdepth 4 \( -iname '*cline*' -o -iname '*cline-termux*' \) -print 2>/dev/null
```

Check the main install paths:

``` bash
ls -ld "$PREFIX/bin/cline" "$PREFIX/bin/bun-ffi" \
  "$PREFIX/opt/cline-termux" "$PREFIX/opt/bun-android-ffi" "$HOME/.cline" \
  2>/dev/null
```

A missing-path message from `ls` is expected after removal.

## 10. Important troubleshooting notes

-   The normal desktop command `npm install -g cline` is **not** the
    recommended installation route for this Termux port. The upstream
    CLI publishes compiled binaries for supported desktop platforms;
    Android Termux support is provided by an unofficial port.

-   If `npm uninstall -g cline` says `up to date` but `cline` still
    exists, inspect the executable before deleting anything:

    ``` bash
    command -v cline
    ls -l "$(command -v cline)"
    head -n 30 "$(command -v cline)"
    ```

-   If Termux prints
    `The program true is not installed. Install it by executing: pkg install coreutils`,
    check the package before reinstalling it:

    ``` bash
    dpkg -s coreutils
    type true
    whence -va true
    ```

-   Do not remove `$PREFIX/opt/bun-android-ffi` blindly if you installed
    or use that runtime separately. The project describes it as private
    to the Cline port, but your own setup may differ.

-   This is a community port. Compatibility and available features may
    change between releases.
