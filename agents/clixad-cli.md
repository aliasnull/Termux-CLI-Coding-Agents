# Clixad on Termux

Run **Clixad**, a terminal-based AI coding agent, natively on Android using Termux.

## Requirements

- Android device with Termux
- Internet connection
- Node.js **20 or newer**
- npm
- GitHub account for Clixad login
- Enough free storage for Node.js, npm dependencies, and Clixad

> Tested successfully on Termux with Node.js `v26.4.0` and npm `12.2.0`.

## Installation

### 1. Update Termux packages

```bash
pkg update && pkg upgrade
```

### 2. Install Node.js and npm

```bash
pkg install nodejs
```

Verify:

```bash
node --version
npm --version
```

Make sure Node.js is version 20 or newer.

### 3. Install Clixad

```bash
npm install -g clixad
```

Verify the installation:

```bash
clixad --version
```

Check available commands:

```bash
clixad --help
```

## Login

Start the GitHub login flow:

```bash
clixad login
```

Clixad will provide a GitHub device-login URL and code. Open the URL in a browser, enter the code, and authorize the account.

Check the logged-in account:

```bash
clixad whoami
```

## Check available models

```bash
clixad models
```

Set a default model:

```bash
clixad model <model-id>
```

Example:

```bash
clixad model deepseek-v4-flash
```

## Start the coding agent

Go to your project directory:

```bash
cd ~/your-project
```

Start Clixad:

```bash
clixad
```

Or start it with an initial task:

```bash
clixad "Inspect this project and explain its structure"
```

## Read-only / headless mode

For a read-only agent run:

```bash
clixad -p "Inspect this project and report any issues. Do not modify any files."
```

## Resume sessions

Continue the last session:

```bash
clixad --continue
```

List saved sessions:

```bash
clixad --resume
```

Resume a specific session:

```bash
clixad --resume <session-id>
```

## Account and credits

Check account and balance:

```bash
clixad wallet
```

Open available earning offers:

```bash
clixad earn
```

Check your referral information:

```bash
clixad refer
```

Redeem a code:

```bash
clixad redeem <code>
```

## Logout

Remove the locally stored Clixad login:

```bash
clixad logout
```

## Uninstall

Remove Clixad:

```bash
npm uninstall -g clixad
```

## Termux Compatibility

Clixad was tested natively on Termux without:

- proot-distro
- Debian/Ubuntu
- a desktop Linux environment
- an Android compatibility layer

The npm package requires Node.js `>=20` and does not declare an OS or CPU restriction.

Tested successfully:

```text
Node.js: v26.4.0
npm:     12.2.0
Clixad:  0.0.1-beta.27
```

The following commands were successfully tested in Termux:

```bash
npm install -g clixad
clixad --version
clixad --help
clixad models
clixad login
```

## Notes

- Clixad is a cloud-based AI coding agent. It does not run the AI models locally on the Android device.
- A GitHub account is used for the login flow.
- Model availability and credit pricing can change over time.
- Do not expose GitHub device-login codes or account credentials publicly.
