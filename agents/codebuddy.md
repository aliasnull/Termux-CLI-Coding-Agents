# CodeBuddy CLI on Termux (Android)

> **Status: Tested and working.** This setup was tested in Termux on an
> Android ARM64 device. Installation and login succeeded, and CodeBuddy
> completed a coding task that generated a landing page.
>
> **Important:** Available plans, models, free usage, quotas, and
> account eligibility are controlled by Tencent/CodeBuddy and may
> change. A model being visible in the CLI does not guarantee that it is
> available to every account or free to use.

## Requirements

-   Android device with [Termux](https://termux.dev/) installed.
-   ARM64 (`aarch64`) Android environment was used for this test.
-   Node.js and npm installed in Termux.
-   Working internet connection.
-   A CodeBuddy account and access to the appropriate login site.

### Tested environment

-   Android kernel/device architecture: `aarch64`
-   Node.js: `v26.4.0`
-   npm: `12.2.0`
-   CodeBuddy CLI: `2.176.0`

These are the versions used during testing, not necessarily mandatory
minimum versions. Check the package's current requirements if a future
release behaves differently.

## Installation

### 1. Check Node.js and npm

``` bash
node --version
npm --version
```

If Node.js/npm are not installed in Termux, install them with:

``` bash
pkg update
pkg install nodejs
```

### 2. Install CodeBuddy CLI

``` bash
npm install -g @tencent-ai/codebuddy-code
```

### 3. Verify the installation

``` bash
codebuddy --version
```

To view the available CLI commands and options:

``` bash
codebuddy --help
```

## Login and first run

Start CodeBuddy:

``` bash
codebuddy
```

When the login options appear, choose **Log in via International Site**
if that is the correct site for your account, then complete the login
flow shown by the CLI.

### Login troubleshooting note

During testing, the login URL initially did not appear while AdGuard DNS
was configured. After disabling the custom DNS and waiting briefly,
choosing **Log in via International Site** produced the login URL and
login succeeded.

If a similar problem occurs: 1. Check Android's **Private DNS** setting
and any DNS filtering app you use. 2. Temporarily switch a custom
Private DNS setting to **Automatic**, if appropriate. 3. Wait briefly
and try the login flow again.

This is an observation from one test, not a guarantee that DNS is the
cause of every login failure. Do not share access tokens, authorization
codes, or other login secrets.

## Model and plan availability

The CLI may list multiple models, but actual access depends on the
account, plan, service rules, quota, and current availability. DeepSeek
was available for free during this test, but free access may change or
be limited at any time.

Check the official CodeBuddy/WorkBuddy documentation and your account's
own interface for current terms. Do not assume that a model or quota
will remain free.

## Uninstall

Remove the globally installed CLI package:

``` bash
npm uninstall -g @tencent-ai/codebuddy-code
```

Verify that the command is no longer available:

``` bash
command -v codebuddy
```

If no path is printed, the shell cannot find the `codebuddy` executable.

## Remove remaining CodeBuddy user data

CodeBuddy may leave user data in `~/.codebuddy` after the npm package is
uninstalled. This directory can contain history, sessions, settings,
logs, project metadata, and other local data.

**Warning:** The following command permanently deletes that CodeBuddy
user-data directory, including its history and local settings. Back up
anything you need before running it.

``` bash
rm -rf ~/.codebuddy
```

Verify cleanup:

``` bash
[ ! -e ~/.codebuddy ] && echo "CodeBuddy user data removed" || echo "CodeBuddy user data still exists"
```

Optional final checks:

``` bash
command -v codebuddy || echo "CodeBuddy CLI removed"
npm ls -g @tencent-ai/codebuddy-code --depth=0
```

## Official resources

-   CodeBuddy CLI documentation:
    https://www.codebuddy.ai/docs/cli/quickstart
-   CodeBuddy CLI troubleshooting:
    https://www.codebuddy.ai/docs/cli/troubleshooting
-   WorkBuddy documentation: https://www.workbuddy.ai/docs/cli/

------------------------------------------------------------------------

**Testing note:** Successful installation, login, and a landing-page
coding task were confirmed in Termux during this test. This does not
guarantee that every model, feature, plan, or future CLI release will
work identically.
