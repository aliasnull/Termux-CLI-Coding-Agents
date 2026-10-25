# Qoder CLI on Termux (Android)

A record of installing and using Qoder CLI directly on Android with
Termux, without needing a PC.

> **Status: Successfully tested.** Qoder CLI was installed, login
> completed successfully, and a small task was completed successfully in
> Termux. The CLI worked well in this test environment.

## Compatibility notes

-   Environment tested: Android / Termux
-   Architecture: `aarch64` (ARM64)
-   Node.js: `v26.4.0`
-   npm: `12.2.0`
-   Qoder CLI package: `@qoder-ai/qodercli@1.1.78`
-   `ripgrep` (`rg`) was available and reported version `15.2.0`.
-   Qoder's npm package declares support for `darwin`, `linux`, and
    `win32`, but not `android`.
-   **Termux/Android is not listed as an officially supported
    platform.** Using `--force` bypasses npm's platform restriction; it
    does not make the package officially supported. However,
    installation, login, and a small agent task were successfully tested
    in this environment.

## Requirements

Install Node.js and ripgrep in Termux if they are not already available:

``` bash
pkg update
pkg install nodejs ripgrep
```

Check the tools:

``` bash
uname -m
uname -o
node -v
npm -v
rg --version
```

The tested environment returned `aarch64` for the architecture and
`Android` for the operating system.

## Installation

The regular installation command is rejected by npm on Android because
the package's supported operating systems exclude Android:

``` bash
npm install -g @qoder-ai/qodercli
```

For the successful Termux test, the platform check was bypassed with:

``` bash
npm install -g --force @qoder-ai/qodercli
```

The package installed, but npm warned that install scripts for
`@qoder-ai/qodercli` and `sharp` were blocked by the `allowScripts`
setting. Despite those warnings, the CLI was able to log in and complete
a small task during testing.

## Verify the installation

Check the installed version:

``` bash
qoder --version
```

Check available commands and options:

``` bash
qoder --help
```

In the test, these returned version `1.1.78` and the CLI help text.

Check that ripgrep is available:

``` bash
rg --version
```

The tested version was `15.2.0`.

## Sign in

Start the login flow:

``` bash
qoder login
```

Follow the URL or browser-based instructions shown by the CLI and
complete sign-in to your Qoder account.

**Security note:** Login URLs can contain temporary authentication
parameters. Do not publish them in a README, issue, screenshot, or
public chat. Complete sign-in only in a browser and account flow you
trust.

## Start the agent

Launch the interactive CLI:

``` bash
qoder
```

To work in a specific project, change into that directory first:

``` bash
cd /path/to/your/project
qoder
```

Replace `/path/to/your/project` with the actual project path.

You can also pass an initial prompt:

``` bash
qoder "Explain the files in this project"
```

**Test result:** Login completed successfully and a small task was
completed successfully in Termux. Qoder CLI worked well in the tested
environment. As with any unofficial platform setup, behavior may vary
across devices, package versions, or tasks.

## Uninstall

Remove the global npm package:

``` bash
npm uninstall -g @qoder-ai/qodercli
```

Check whether the command is still on your PATH:

``` bash
command -v qoder
```

No output means the shell could not find a `qoder` command on its
current PATH.

### Remove leftover Qoder data (optional)

The package uninstall may leave user-level data under `~/.qoder`.
Inspect it first:

``` bash
ls -la ~/.qoder
```

If you no longer need Qoder's local authentication data, logs, temporary
files, or configuration, remove that directory:

``` bash
rm -rf ~/.qoder
```

**Warning:** This permanently deletes the contents of `~/.qoder`,
including local authentication data and any Qoder configuration stored
there. Run it only if you intend to remove those files.

## Official resources

-   Qoder website: https://qoder.com/
-   Qoder download page: https://qoder.com/en/download
-   Qoder CLI installation documentation:
    https://docs.qoder.com/cli/installation

## Test summary

  -----------------------------------------------------------------------
  Check                               Result
  ----------------------------------- -----------------------------------
  Standard npm install                Failed: Android is not an allowed
                                      platform

  Install with `--force`              Package installed; npm install
                                      scripts were blocked

  `qoder --version`                   Passed: `1.1.78`

  `qoder --help`                      Passed

  `rg --version`                      Passed: `15.2.0`

  `qoder login`                       Passed: sign-in completed
                                      successfully

  Actual AI-agent task                Passed: a small task completed
                                      successfully in Termux

  npm uninstall                       Passed

  `command -v qoder` after uninstall  No output
  -----------------------------------------------------------------------
