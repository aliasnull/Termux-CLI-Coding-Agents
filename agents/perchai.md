# Perch AI CLI on Termux

A practical guide to installing, logging in, testing, uninstalling, and cleaning up Perch AI CLI in Termux.

## Requirements

- Termux
- Node.js and npm

Check that Node.js and npm are available:

```bash
node --version
npm --version
```

If either command is missing, install Node.js in Termux:

```bash
pkg update
pkg install nodejs
```

## Install

Install the Perch CLI globally with npm:

```bash
npm install -g perchai-cli
```

Check that the command is available:

```bash
perch status
```

The CLI may not support a `--version` flag. If `perch --version` returns an unknown-flag message, use `perch status` to check the installed CLI version and sign-in state.

## Login

Start the sign-in flow:

```bash
perch login
```

Follow the prompts to sign in with Google or GitHub. If asked to choose a plan, select the plan you want. The Starter plan includes hosted credits, and its sign-up screen explains the Starter Data Program and privacy options. Read those terms before accepting.

Verify the saved sign-in status:

```bash
perch status
```

## Start Perch

Open the interactive terminal chat:

```bash
perch
```

Exit the interactive session using the app's displayed exit instructions.

## Run a one-shot task

You can also run a task directly:

```bash
perch run "Explain what files are in the current directory. Do not modify anything."
```

For help, run:

```bash
perch
```

The CLI prints its usage and available commands when appropriate.

## Uninstall

Remove the globally installed npm package:

```bash
npm uninstall -g perchai-cli
```

Verify that the command was removed:

```bash
perch status
```

If uninstallation succeeded, the shell should report that `perch` is not found.

## Clean up local Perch data

Perch may keep local settings, history, cache, threads, browser session data, and other state under `~/.perch`.

Inspect the directory before deleting it:

```bash
ls -la ~/.perch
```

If you want to remove all local Perch data, delete the directory:

```bash
rm -rf ~/.perch
```

**Warning:** This permanently deletes the local Perch settings, history, cache, threads, and other data stored in that directory. Make sure you do not need anything in it first.

Verify cleanup:

```bash
ls -la ~/.perch
```

Expected result after cleanup:

```text
No such file or directory
```

## Optional: check for a leftover global package

If you want to verify that npm no longer lists the package globally:

```bash
npm list -g --depth=0
```

Look for `perchai-cli` in the output. It should not appear after a successful uninstall.

## Notes

- Installing and running the CLI successfully on one Termux device does not guarantee compatibility with every Android device or Termux setup.
- Removing `~/.perch` cleans local state, but it does not delete your Perch account or cancel a paid subscription. Manage account privacy, plan, and subscription settings through the official Perch website.
- Do not share login tokens, API keys, or private configuration files publicly.
