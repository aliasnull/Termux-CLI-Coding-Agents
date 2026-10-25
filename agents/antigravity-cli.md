# Antigravity CLI on Termux (Native ARM64)

Run Google Antigravity CLI directly on **Android/Termux without PRoot**.

> **Important:** This setup uses Termux's `glibc` package for the
> `agy.va39` payload. It does **not** use PRoot or `proot-distro`.

## Requirements

-   Android device with ARM64 (`aarch64`)
-   Termux from the GitHub/F-Droid build
-   Internet connection
-   A Google account with Antigravity access
-   Sufficient free storage for the CLI payload

## What this setup uses

  Component         Version / Type
  ----------------- ------------------------------
  Termux            0.118.3 or newer recommended
  Architecture      ARM64 / `aarch64`
  Antigravity CLI   v1.2.16
  glibc             Termux `glibc` package
  PRoot             **Not required**

## 1. Update Termux packages

``` bash
pkg update -y
```

Optional, but recommended on a fresh Termux installation:

``` bash
pkg upgrade -y
```

## 2. Install the required glibc repository and package

``` bash
pkg install glibc-repo -y
```

Then install glibc:

``` bash
pkg install glibc -y
```

## 3. Download Antigravity CLI

Create a temporary installation directory:

``` bash
mkdir -p ~/antigravity-install
cd ~/antigravity-install
```

Download the Termux standalone release:

``` bash
curl -fL --retry 2 -o antigravity-termux-standalone.tar.gz https://github.com/wallentx/antigravity-cli-termux/releases/download/v1.2.16/antigravity-termux-standalone.tar.gz
```

## 4. Extract the CLI

``` bash
tar -xzf antigravity-termux-standalone.tar.gz
```

The release contains:

-   `agy` - Android/Termux launcher
-   `agy.va39` - Antigravity CLI payload

Both files are required.

## 5. Install the CLI

Install the launcher:

``` bash
install -m 0755 agy "$PREFIX/bin/agy"
```

Install the payload beside it:

``` bash
install -m 0755 agy.va39 "$PREFIX/bin/agy.va39"
```

After this, `agy` is available as a normal Termux command.

## 6. Start Antigravity

Run:

``` bash
agy
```

On the first launch, follow the Google authentication flow. A browser
may open for Google OAuth. Complete the login with the Google account
you want to use with Antigravity.

## 7. Updating Antigravity

The Termux port includes its own updater:

``` bash
agy update -y
```

If a newer standalone release is available, the updater downloads and
installs the matching `agy` and `agy.va39` files.

## 8. Choosing a model

Inside an Antigravity session, use:

``` text
/models
```

Select the model you want from the available model list.

Available models depend on the Google account, subscription/entitlement,
region, rollout status, and current Antigravity model availability.

## 9. Useful CLI commands

Start an interactive session:

``` bash
agy
```

Show CLI help:

``` bash
agy --help
```

Continue the most recent conversation:

``` bash
agy --continue
```

Run a one-shot prompt:

``` bash
agy --print "Explain this project structure"
```

Specify a model:

``` bash
agy --model "MODEL_NAME"
```

Add a workspace directory:

``` bash
agy --add-dir /path/to/project
```

## 10. Project usage

Move into your project directory and start Antigravity:

``` bash
cd ~/your-project
agy
```

For Android shared storage projects, for example:

``` bash
cd ~/storage/shared/Download/your-project
agy
```

If shared storage has not been configured yet:

``` bash
termux-setup-storage
```

## 11. Updating the Termux port

To update the Antigravity standalone build:

``` bash
agy update -y
```

The updater checks the latest `wallentx/antigravity-cli-termux` release
and keeps the launcher and payload synchronized.

## 12. Uninstall

Remove the installed Antigravity files:

``` bash
rm -f "$PREFIX/bin/agy" "$PREFIX/bin/agy.va39"
```

Remove the downloaded installer files:

``` bash
rm -rf ~/antigravity-install
```

If you installed `glibc` only for Antigravity and no other Termux
application depends on it, it can be removed separately:

``` bash
pkg uninstall glibc glibc-repo
```

> Do not remove `glibc` if you use other glibc-based programs in Termux.

## Notes

### No PRoot required

This setup runs from the native Termux environment. You do **not** need:

``` text
proot
proot-distro
Ubuntu/Debian inside PRoot
```

### Why glibc is required

The Termux standalone release contains an Android-native `agy` launcher,
while the main `agy.va39` payload is a GNU/Linux ARM64 binary that uses
the Termux glibc runtime.

Therefore:

``` text
Termux
  ↓
Android-native agy launcher
  ↓
Termux glibc
  ↓
agy.va39
  ↓
Antigravity CLI
```

### Model availability

Installing the CLI does not guarantee access to every model. The model
list is determined by the authenticated Google account and Antigravity's
current entitlement/availability.

For example, a Google AI Pro account may show Claude models, but the
exact Claude/Gemini model versions available can differ by account and
rollout.

## Credits

-   Antigravity CLI: Google
-   Termux standalone packaging/Android compatibility:
    `wallentx/antigravity-cli-termux`

Repository:

https://github.com/wallentx/antigravity-cli-termux

## License / Terms

Use Antigravity according to Google's current terms and policies. The
Termux packaging project is a community compatibility project and is not
an official Google Android/Termux distribution.
