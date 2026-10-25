# OmniRush CLI on Termux

## OmniRush Invite

[Join OmniRush](https://omnirush.ai/console?ref=MN5ZE5NG)


This guide documents the **native Termux** setup for OmniRush CLI.

It does **not** use `proot-distro`, a Linux container, or a glibc
environment.

## What is required

  Requirement       Verified setup
  ----------------- --------------------------------------------
  Termux            Native Android Termux
  CPU               ARM64 (`aarch64`)
  Node.js           `22.19.0` or newer
  npm               Installed with Node.js
  OmniRush          `1.0.15` tested
  Internet          Required for installation and device login
  CA certificates   `ca-certificates` package
  `fd`              Recommended by OmniRush for file search

### Important: Node.js version

OmniRush requires **Node.js 22.19 or newer**.

Check it with:

``` bash
node --version
```

Check npm with:

``` bash
npm --version
```

The tested setup used:

``` text
Node.js v26.4.0
```

## 1. Install Node.js and npm

If Node.js and npm are not installed in Termux, install Node.js:

```bash
pkg install nodejs
```

Verify Node.js:

```bash
node --version
```

Verify npm:

```bash
npm --version
```

OmniRush requires **Node.js 22.19 or newer**. If the installed Node.js version is older, update Termux packages and install a newer Node.js package before continuing.

## 2. Check the Termux environment

Check Node.js, architecture, and platform:

``` bash
node -p "process.platform + ' ' + process.arch + ' ' + process.version"
```

A native Termux setup reports something similar to:

``` text
android arm64 v26.4.0
```

The important part is that Node.js is available and is version 22.19 or
newer.

## 3. Install CA certificates

Install Termux's CA certificate package:

``` bash
pkg install ca-certificates
```

This provides the CA certificate bundle used for HTTPS/TLS connections.

The certificate bundle is normally available at:

``` text
$PREFIX/etc/tls/cert.pem
```

You can check it with:

``` bash
ls -l "$PREFIX/etc/tls/cert.pem"
```

### If OmniRush reports a missing TLS certificate directory

If OmniRush prints:

``` text
Cannot open directory .../etc/tls/certs to load OpenSSL certificates.
```

and the `cert.pem` file already exists, create the missing directory:

``` bash
mkdir -p "$PREFIX/etc/tls/certs"
```

Then verify that the warning is gone:

``` bash
omnirush --version
```

A successful result looks like:

``` text
omnirush 1.0.15
```

## 4. Install `fd`

OmniRush uses `fd` for fast file searching. Without it, OmniRush can
still start, but file search functionality can be degraded.

Install it with:

``` bash
pkg install fd
```

Verify:

``` bash
fd --version
```

Example:

``` text
fd 10.5.0
```

## 5. Install OmniRush

Install the latest OmniRush CLI:

``` bash
npm i -g omnirush@latest
```

To install the tested version specifically:

``` bash
npm i -g omnirush@1.0.15
```

Verify the installation:

``` bash
omnirush --version
```

Expected:

``` text
omnirush 1.0.15
```

## 6. Sign in

Run:

``` bash
omnirush login
```

OmniRush displays a device-login code and opens the browser for
approval.

After approving the login, it reports the account that was signed in.

No API key is required for this login flow.

## 7. Start OmniRush in a project

OmniRush should be run inside a project/workspace.

For a new test directory:

``` bash
mkdir -p ~/omnirush-test && cd ~/omnirush-test
```

Then start OmniRush:

``` bash
omnirush
```

You should see the OmniRush CLI interface with the current workspace and
selected model.

## 8. Normal usage

Once inside a project:

``` bash
omnirush
```

Useful built-in commands shown by the CLI include:

``` text
/model
/resume
/status
/
```

The exact available commands can vary by OmniRush version.

## Termux-specific notes

### No proot-distro required

The tested setup runs directly in native Termux:

``` text
Android
  └── Termux
       └── Node.js
            └── OmniRush CLI
```

There is no need to install Ubuntu, Debian, or another distribution
through `proot-distro`.

### Why Linux ARM64 support is not required for the CLI core

OmniRush publishes native Linux ARM64 runtime packages, but native
Termux reports:

``` text
process.platform = android
process.arch = arm64
```

The OmniRush launcher does not select the Linux ARM64 runtime on
Android. Instead, when a bundled runtime is unavailable, it can run the
OmniRush core directly with Node.js when the Node.js version is new
enough.

Therefore, **Node.js 22.19+ is the important runtime requirement for
this native Termux setup.**

## Complete setup sequence

For a fresh native Termux environment where Node.js 22.19+ and npm are
already available:

``` bash
pkg install ca-certificates
pkg install fd
npm i -g omnirush@latest
omnirush --version
omnirush login
mkdir -p ~/omnirush-test && cd ~/omnirush-test
omnirush
```

If the TLS certificate directory warning appears even though `cert.pem`
exists:

``` bash
mkdir -p "$PREFIX/etc/tls/certs"
```

Then run:

``` bash
omnirush --version
```

## Quick requirements checklist

Before installing OmniRush:

-   [ ] Native Termux
-   [ ] ARM64 device
-   [ ] Node.js 22.19+
-   [ ] npm
-   [ ] Internet connection
-   [ ] `ca-certificates`
-   [ ] `fd`

After installation:

-   [ ] `omnirush --version` works
-   [ ] `omnirush login` completes
-   [ ] `omnirush` starts inside a project directory
