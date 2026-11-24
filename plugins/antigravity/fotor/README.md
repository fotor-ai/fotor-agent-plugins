# Fotor for Google Antigravity

<p align="center"><img src="assets/logo.svg" width="72" alt="Fotor logo" /></p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.13.0&amp;color=087f8c&amp;style=flat-square" height="20" alt="Version" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=production&amp;color=238636&amp;style=flat-square" height="20" alt="Release channel" />
  <img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" height="20" alt="Apache-2.0 license" />
</p>

Create and edit images, videos, and audio in an Antigravity conversation.
The [plugin](plugin.json) bundles six skills and connects to Fotor through
its [MCP configuration](mcp_config.json).

| Field | Value |
| --- | --- |
| Plugin | `fotor` |
| Version | `0.13.0` |
| Release channel | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Supported agent versions | Antigravity App `>=2.33.0` · Antigravity CLI `>=1.5.13` |

## Installation

Fotor **0.13.0** is distributed on the **production** package channel.
The App and CLI share one installation when they use the same operating-system account.

[App installation](#install-for-the-antigravity-app) · [CLI installation](#install-for-the-antigravity-cli)

### Install for the Antigravity App

1. On the computer running the App, open a terminal under the same user account
   and run the [Antigravity installation command](#antigravity-installation-command)
   below. If this version is already installed with `agy`, continue with step 2.
2. Open **Customizations → Installed → Fotor** in the App. Check that Fotor is
   enabled and its version is **0.13.0**.
3. If sign-in is required, open **Accounts** and select **Sign in** for
   `fotor` at `mcp.fotor.com`. Complete authorization in your browser and
   enter a code in the App if prompted.
4. Open **MCP** and look for a connected Fotor service with `list_models`.
   In a new project conversation, ask “Which image models are available in Fotor?”

### Install for the Antigravity CLI

1. Run the [Antigravity installation command](#antigravity-installation-command)
   below in a terminal. If you already installed Fotor for the App under this
   account, run `agy plugin list --json` to check its version and enabled state.
2. Start `agy` in your project. Open `/plugin`, select **Installed → Fotor**,
   and choose **Sign in** if prompted. Complete authorization in your browser
   and enter any requested code in the CLI.
3. Open `/mcp` to check that `fotor_fotor` is connected. Type `/fotor` and
   select `/fotor:fotor-connect` to use the connection skill.

### Antigravity installation command

You need Git and the [Antigravity CLI (`agy`)](https://antigravity.google/docs/getting-started/).

On [GitHub Releases](https://github.com/fotor-ai/fotor-agent-plugins/releases), choose a published collection whose version table lists Antigravity **0.13.0**. In your terminal, enter `release_tag='…'`, replacing the ellipsis with that release’s tag.

In your terminal, enter `public_sha='…'`, replacing the ellipsis with the full
40-character commit shown on GitHub. Run the block from a directory where
`fotor-antigravity` does not yet exist.

If `agy plugin list --json` already shows Fotor, check its source and files
before replacing it. Resolve any same-name workspace skill or MCP service first.

```bash
(
  set -eu
  test -n "$public_sha"
  git clone --single-branch --branch "$release_tag" \
    https://github.com/fotor-ai/fotor-agent-plugins.git fotor-antigravity
  cd fotor-antigravity
  test "$(git rev-parse HEAD)" = "$public_sha" || exit 1
  git checkout --detach "$public_sha"
  agy plugin validate --strict --json ./plugins/antigravity/fotor
  agy plugin install ./plugins/antigravity/fotor
  agy plugin list --json
)
```

If Fotor shows an account but MCP reports `Unauthorized`, use Fotor’s account
controls in the affected client to sign out and sign in again. [Antigravity authorization guide](skills/fotor-connect/references/authentication.md)

## Manage the plugin

In the App, open **Customizations → Installed** and use Fotor’s menu. From a
terminal, use `agy plugin disable fotor`, `agy plugin enable fotor`, or
`agy plugin uninstall fotor` for an installation you own. Before updating or
removing Fotor, check the installed source and preserve any local edits.

## Account and data

Antigravity manages sign-in and tool approvals. Fotor receives the prompts,
parameters, and reference media needed for the operations you request.
Generation and processing may use Fotor credits. The credits skill checks
only your own balance. [Service workflow](skills/fotor-connect/references/mcp-integration.md)

## License

[Apache License 2.0](LICENSE)
