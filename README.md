<p align="center">
  <img src="plugins/codex/fotor/assets/logo.svg" width="96" alt="Fotor logo" />
</p>

<h1 align="center">Fotor Plugin for Agents</h1>

<p align="center">Create images and videos with Fotor, directly from your AI agent.</p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.2.0&amp;color=087f8c&amp;style=flat-square" alt="Version 0.2.0" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=production&amp;color=238636&amp;style=flat-square" alt="production channel" />
  <img src="https://img.shields.io/static/v1?label=agent&amp;message=Codex+and+Claude+Code&amp;color=24292f&amp;style=flat-square" alt="Included: Codex and Claude Code" />
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" alt="Apache-2.0 license" /></a>
</p>

<p align="center">
  <a href="#supported-agents">Agents</a> ·
  <a href="#features">Features</a> ·
  <a href="#codex">Codex setup</a> · <a href="#claude-code">Claude Code setup</a> ·
  <a href="#documentation">Documentation</a>
</p>

## Supported Agents

| Agent | Status | Getting started |
| --- | --- | --- |
| **Codex** | Included | [Install and connect](#codex) |
| **Claude Code** | Included | [Install and connect](#claude-code) |

This snapshot includes Codex and Claude Code. Installation and live-service acceptance are separate checks; see the verification scope below.

Both release channels connect to Fotor's production service and website. The test channel previews plugin changes; it uses your production account and credits.

## Features

| Capability | What you can do |
| --- | --- |
| **Images** | Generate images or edit them with prompts and references. |
| **Videos** | Generate from text, frames, or supported media references. |
| **Media uploads** | Upload local images, videos, and audio for reuse. |
| **Image processing** | Upscale images and remove backgrounds. |
| **Website connection** | Open Fotor with your authorized MCP identity. |
| **Credits** | Check your balance and get a pricing-page link when needed. |
| **Task results** | Check submitted tasks and retrieve available results. |

Available models, parameters, and operations come from the connected Fotor service.

## Codex

### Installation

Use a Codex version with plugin support and Git access to this public repository. GitHub sign-in is not required to install the public package. If `fotor-codex` is already registered from another source, follow [Switch release channel](#switch-release-channel).

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-codex
```

### Connect

1. Check the installed plugin and enabled state:

   ```bash
   codex plugin list --marketplace fotor-codex --json
   ```

2. Confirm version **0.2.0**, release channel **production**, and the **production** MCP endpoint `https://mcp.fotor.com/mcp` in the installed package. Check any standalone Fotor MCP override before authorization; it must use the same production endpoint.
3. Open a new Codex task and ask:

   ```text
   Connect to Fotor and introduce its main features.
   ```

Valid credentials are reused. When authorization is needed, complete Codex's sign-in flow in an external browser, then choose "Signed in and authorized; open Fotor in the in-app browser" in the localized confirmation prompt. You can also choose to keep waiting or cancel. After Codex confirms authorization, the plugin opens the website sign-in link in a visible in-app browser. See the [connection workflow](plugins/codex/fotor/references/mcp-integration.md#connect-to-the-website) for verification and manual fallback.

For a read-only check without opening the website, ask: **Check only the Fotor MCP connection.**

### Updates

Refresh and reinstall from the currently selected channel:

```bash
codex plugin marketplace upgrade fotor-codex
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

Open a new task and verify the version, enabled state, and endpoint. An update keeps the selected branch; use the next section to change channels.

### Switch release channel

Use one source for the `fotor-codex` marketplace. Choose the destination first, then run **one** of the replacement sequences below. Run source changes outside another checkout containing the same marketplace catalog.

<details>
<summary>Choose the production or test release channel</summary>

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
codex plugin marketplace remove fotor-codex
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
codex plugin marketplace remove fotor-codex
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

</details>

After switching, open a new task and verify the destination version and production MCP endpoint. Reuse valid production authorization. If a legacy installation or standalone override uses a different service, stop and update that configuration through the host's controls before connecting; do not migrate its credentials automatically.

### Troubleshooting

<details>
<summary>Repository access, stale installations, and sign-in</summary>

- **Repository access:** confirm Git can reach this public repository. GitHub access and Fotor MCP authorization are separate; keep credentials outside repository URLs and plugin files.
- **Unexpected version or endpoint:** inspect the marketplace source, reinstall, and start a new task. Check both the installed MCP declaration and any standalone Fotor override.
- **Authorization required:** use the selected server's native sign-in flow in an external browser. For a matching standalone server named `fotor`, the CLI provides `codex mcp login fotor`.
- **Website or media operation fails:** retain the actual error and task ID when present. Connection, browser login, and media execution have separate outcomes; see the workflow references below.

</details>


## Claude Code

### Claude Code installation

This guide targets **Claude Code CLI** with plugin support and Git access. The client acceptance baseline is **2.1.296**; other CLI versions require their own checks, and Desktop/IDE acceptance remains deferred. Install the whole marketplace package; no interpreter or build step is required.

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --scope user
claude plugin install fotor@fotor-claude-code --scope user
```

Start a new Claude Code session. Check `claude plugin list --json` for `fotor@fotor-claude-code`, version **0.2.0**, and its enabled state. Verify the installed `.mcp.json` uses `https://mcp.fotor.com/mcp` and resolve conflicting host overrides before connecting.

### Claude Code connection

Ask **Connect to Fotor and introduce its main features.** You can also invoke `/fotor:fotor-connect`. For a read-only connection check, ask **Check only the Fotor MCP connection.**

Use `/mcp` to inspect the plugin's server, normally `plugin:fotor:fotor`. Reuse valid Claude-managed Fotor credentials. When native status requires login, use its Authenticate action or the supported CLI entry:

```bash
claude mcp login plugin:fotor:fotor
```

Complete OAuth in an external browser, choose Continue in the localized Continue / Wait / Cancel prompt, and wait for the client to confirm success. In Claude Code CLI, open the verified handoff once in an external browser such as Chrome; if the CLI cannot open it, copy the unused link there manually. In Claude Code Desktop, open the handoff once in its built-in browser after external OAuth. If the selected browser control is unavailable, explain the limitation and provide the unused link with instructions for that browser. Manual delivery is not automatic-login verification; Desktop host acceptance remains separate. See [Claude authentication and browser guidance](plugins/claude-code/fotor/references/claude-code-authentication.md).

MCP credentials remain in Claude's supported store. Model questions do not open the website, and connection does not query credits. Check the actual website login state before claiming success.

### Claude Code updates

Refresh the selected source, update the package, and start a new session:

```bash
claude plugin marketplace update fotor-claude-code
claude plugin update fotor@fotor-claude-code
claude plugin list --json
```

Verify the installed version, source, enabled state, and production MCP endpoint. A local-directory marketplace reads files in place; it does not exercise hosted plugin caching. A cached release needs a version change before an update can be established.

### Claude Code channel

Both channels use production MCP and credits. A test source pins `release/test`; the stable source uses `main`. Ordinary upgrades retain the selected branch.

<details>
<summary>Explicitly replace a user-scoped marketplace source</summary>

Removing a Claude marketplace uninstalls its plugins and can delete their saved options, secrets, and data. Inspect the current scope and preserve needed state first. Use a replacement sequence only when you explicitly choose to change that source, never as routine connection recovery or an ordinary upgrade. The examples below target user scope; project or managed installations must be handled through their own controls.

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
claude plugin marketplace remove fotor-claude-code --scope user
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --scope user
claude plugin install fotor@fotor-claude-code --scope user
claude plugin list --json
```

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
claude plugin marketplace remove fotor-claude-code --scope user
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git#release/test --scope user
claude plugin install fotor@fotor-claude-code --scope user
claude plugin list --json
```

</details>

Use `claude plugin marketplace list --json` to verify the source and branch, then start a new session. Keep existing valid production authorization; do not copy tokens from another client or create a duplicate standalone Fotor server to work around plugin loading.


## Documentation

- [Codex plugin guide](plugins/codex/fotor/README.md)
- [Claude Code plugin guide](plugins/claude-code/fotor/README.md)

### Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

## License

[Apache-2.0](LICENSE)
