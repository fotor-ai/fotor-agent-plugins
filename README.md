<p align="center">
  <img src="assets/logo.svg" width="96" alt="Fotor logo" />
</p>

<h1 align="center">Fotor Plugin for Agents</h1>

<p align="center">Create images and videos with Fotor, directly from your AI agent.</p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.3.0-test.2&amp;color=087f8c&amp;style=flat-square" alt="Version 0.3.0-test.2" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=test&amp;color=a56800&amp;style=flat-square" alt="test channel" />
  <img src="https://img.shields.io/static/v1?label=agent&amp;message=Codex+and+Claude+Code+and+Cursor&amp;color=24292f&amp;style=flat-square" alt="Included: Codex and Claude Code and Cursor" />
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" alt="Apache-2.0 license" /></a>
</p>

<p align="center">
  <a href="#supported-agents">Agents</a> ·
  <a href="#features">Features</a> ·
  <a href="#codex">Codex setup</a> · <a href="#claude-code">Claude Code setup</a> · <a href="#cursor">Cursor setup</a> ·
  <a href="#documentation">Documentation</a>
</p>

## Supported Agents

| Agent | Status | Getting started |
| --- | --- | --- |
| **Codex** | Included | [Install and connect](#codex) |
| **Claude Code** | Included | [Install and connect](#claude-code) |
| **Cursor** | Included | [Install and connect](#cursor) |

This snapshot includes Codex and Claude Code and Cursor. Installation and live-service acceptance are separate checks; see the verification scope below.

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

Use a Codex version with plugin support and Git access to this public repository. GitHub sign-in is not required to install the public package. If `fotor-ai` is already registered from another source, follow [Switch release channel](#switch-release-channel).

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-ai
```

### Connect

1. Check the installed plugin and enabled state:

   ```bash
   codex plugin list --marketplace fotor-ai --json
   ```

2. Confirm version **0.3.0-test.2**, release channel **test**, and the **production** MCP endpoint `https://mcp.fotor.com/mcp` in the installed package. Check any standalone Fotor MCP override before authorization; it must use the same production endpoint.
3. Open a new Codex task and ask:

   ```text
   Connect to Fotor and introduce its main features.
   ```

Valid credentials are reused. When authorization is needed, complete Codex's sign-in flow in an external browser, then choose "Signed in and authorized; open Fotor in the in-app browser" in the localized confirmation prompt. You can also choose to keep waiting or cancel. After Codex confirms authorization, the plugin opens the website sign-in link in a visible in-app browser. See the [connection workflow](plugins/codex/fotor/references/mcp-integration.md#connect-to-the-website) for verification and manual fallback.

For a read-only check without opening the website, ask: **Check only the Fotor MCP connection.**

### Updates

Refresh and reinstall from the currently selected channel:

```bash
codex plugin marketplace upgrade fotor-ai
codex plugin add fotor@fotor-ai
codex plugin list --marketplace fotor-ai --json
```

Open a new task and verify the version, enabled state, and endpoint. An update keeps the selected branch; use the next section to change channels.

### Codex marketplace migration

Existing `fotor-codex` installations need this one-time migration; ordinary updates within `fotor-ai` use the Updates section.

Migration changes the installation identity. Before removing anything, record the old source/ref, scope, version and enabled state, and confirm the destination catalog actually declares `fotor-ai`. A source still declaring `fotor-codex` cannot install `fotor@fotor-ai`. Preserve required plugin data using supported host controls. Never copy tokens or clear credentials to make a migration work.

Outside other catalog-bearing checkouts, remove only the old Fotor installation and its registration:

```bash
codex plugin remove fotor@fotor-codex
codex plugin marketplace remove fotor-codex
```

Removal deletes the old installed cache. Then run the Installation commands above for the verified destination. Check `codex plugin list --marketplace fotor-ai --json` and the marketplace listing: only one Fotor installation should be enabled, with the intended version, source and production MCP declaration. Open a new task. If migration fails, remove any partial new installation and restore the recorded old source/ref and plugin through native controls. Skip removals already completed; inspect current state before repeating the sequence.

Credential reuse across the identity change requires separate verification; do not promise that a successful installation preserves authorization.

### Switch release channel

Use one source for the `fotor-ai` marketplace. Choose the destination first, then run **one** of the replacement sequences below. Run source changes outside another checkout containing the same marketplace catalog.

<details>
<summary>Choose the production or test release channel</summary>

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
codex plugin marketplace remove fotor-ai
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-ai
codex plugin list --marketplace fotor-ai --json
```

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
codex plugin marketplace remove fotor-ai
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-ai
codex plugin list --marketplace fotor-ai --json
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

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git#release/test --scope user
claude plugin install fotor@fotor-ai --scope user
```

Start a new Claude Code session. Check `claude plugin list --json` for `fotor@fotor-ai`, version **0.3.0-test.2**, and its enabled state. Verify the installed `.mcp.json` uses `https://mcp.fotor.com/mcp` and resolve conflicting host overrides before connecting.

### Claude Code connection

Ask **Connect to Fotor and introduce its main features.** You can also invoke `/fotor:fotor-connect`. For a read-only connection check, ask **Check only the Fotor MCP connection.**

Use `/mcp` to inspect the plugin's server, normally `plugin:fotor:fotor`. Reuse valid Claude-managed Fotor credentials. When native status requires login, use its Authenticate action or the supported CLI entry:

```bash
claude mcp login plugin:fotor:fotor
```

Complete OAuth in an external browser, choose Continue in the localized Continue / Wait / Cancel prompt, and wait for the client to confirm success. In Claude Code CLI, open the verified handoff once in an external browser such as Chrome; if the CLI cannot open it, copy the unused link there manually. In Claude Code Desktop, open the handoff once in its built-in browser after external OAuth. If the selected browser control is unavailable, explain the limitation and provide the unused link with instructions for that browser. Manual delivery is not automatic-login verification; Desktop host acceptance remains separate. See [Claude authentication and browser guidance](plugins/claude-code/fotor/references/authentication.md).

MCP credentials remain in Claude's supported store. Model questions do not open the website, and connection does not query credits. Check the actual website login state before claiming success.

### Claude Code updates

Refresh the selected source, update the package, and start a new session:

```bash
claude plugin marketplace update fotor-ai
claude plugin update fotor@fotor-ai
claude plugin list --json
```

Verify the installed version, source, enabled state, and production MCP endpoint. A local-directory marketplace reads files in place; it does not exercise hosted plugin caching. A cached release needs a version change before an update can be established.

### Claude Code marketplace migration

Existing `fotor-claude-code` installations need a one-time migration to `fotor-ai`.

Migration changes the installation identity. Before removing anything, record the old source/ref, scope, version and enabled state, and confirm the destination catalog actually declares `fotor-ai`. A source still declaring `fotor-claude-code` cannot install `fotor@fotor-ai`. Preserve required plugin data using supported host controls. Never copy tokens or clear credentials to make a migration work.

The following example is for a user-scoped installation. Use the original scope's controls for project/local installations; managed installations require their administrator. Verify that the native uninstall command supports `--keep-data` before proceeding. If unavailable, stop to preserve needed data through supported controls first.

```bash
claude plugin uninstall fotor@fotor-claude-code --scope user --keep-data
claude plugin marketplace remove fotor-claude-code --scope user
```

`--keep-data` retains the old plugin's persistent data; it does not copy that data, saved options or credentials into the new identity. Marketplace removal can remove remaining plugin state, so inspect the selected scope first. Run the Installation commands above for the verified destination. Check the marketplace source and `claude plugin list --json` for one enabled `fotor@fotor-ai` with the intended version and production MCP configuration. Start a new session.

If migration fails, remove any partial new installation using the same data-preserving approach and restore the recorded old source/ref, scope and plugin. Inspect current state and skip completed removals when retrying. Authorization reuse is a separate check; do not clear or copy credentials.

### Claude Code channel

Both channels use production MCP and credits. A test source pins `release/test`; the stable source uses `main`. Ordinary upgrades retain the selected branch.

<details>
<summary>Explicitly replace a user-scoped marketplace source</summary>

Removing a Claude marketplace uninstalls its plugins and can delete their saved options, secrets, and data. Inspect the current scope and preserve needed state first. Use a replacement sequence only when you explicitly choose to change that source, never as routine connection recovery or an ordinary upgrade. The examples below target user scope; project or managed installations must be handled through their own controls.

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
claude plugin marketplace remove fotor-ai --scope user
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --scope user
claude plugin install fotor@fotor-ai --scope user
claude plugin list --json
```

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
claude plugin marketplace remove fotor-ai --scope user
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git#release/test --scope user
claude plugin install fotor@fotor-ai --scope user
claude plugin list --json
```

</details>

Use `claude plugin marketplace list --json` to verify the source and branch, then start a new session. Keep existing valid production authorization; do not copy tokens from another client or create a duplicate standalone Fotor server to work around plugin loading.


## Cursor

### Cursor installation

This guide covers Cursor IDE and Cursor CLI. Use a complete package with `.cursor-plugin/plugin.json`, `mcp.json`, skills, references, and assets. No interpreter or build step is required. Cloud Agent acceptance is separate.

#### Test release

First confirm [release/test](https://github.com/fotor-ai/fotor-agent-plugins/tree/release/test) contains the Cursor catalog and `plugins/cursor/fotor/` package. Register the selected Git source with the native CLI, then open `/plugin` (`/plugins` on clients that show that alias), select `fotor-ai` and install Fotor. Registration alone does not install the plugin. Both channels use production MCP and accounts.

```bash
agent plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins --git-ref release/test
agent plugin marketplace list --format json
agent
```

The CLI exposes marketplace management commands; installation happens in its native `/plugin` interface, not through an `agent plugin install` shell command. Confirm the selected package is the Cursor package when several platform catalogs coexist. Account-level marketplace registration can be visible outside the current client configuration.

#### Alternative local loading

For IDE local loading, copy the selected complete package into `~/.cursor/plugins/local/fotor/`, reload the window, and inspect **Customize**. Local imports must be permitted. An installed marketplace plugin of the same name takes precedence; verify the actual source before testing. A symlink to a repository outside the local plugin directory is not a supported installation.

For CLI local loading, start a new native session:

```bash
agent --plugin-dir /absolute/path/to/fotor
```

Verify version **0.3.0-test.2**, enabled state, discovered skills, and the `https://mcp.fotor.com/mcp` URL in the installed `mcp.json`. Resolve conflicting host overrides before connecting. Marketplace installation and local loading are separate routes; do not substitute Codex or Claude commands.

### Cursor connection

Ask **Connect to Fotor and introduce its main features.** Select the actual connection skill from the native slash menu for an explicit invocation. Ask **Check only the Fotor MCP connection** to finish without a website link.

Use IDE MCP controls or CLI `/mcp` to inspect the actual plugin server. When the CLI supports them in the same loading context, `agent mcp list`, `agent mcp list-tools <identifier>`, and `agent mcp login <identifier>` provide native inspection and login. Cursor account login and Fotor MCP OAuth are separate.

Reuse valid Fotor credentials. When authorization is required, complete it in an external browser, choose Continue, and wait for client-confirmed success. Website requests use the IDE's visible built-in browser or the CLI's permitted external-browser opener. If unavailable, provide the unused link for the specified browser and await confirmation. Verify the final production website and signed-in state. See [Cursor native guidance](plugins/cursor/fotor/references/authentication.md).

### Cursor updates

For a Git marketplace install, refresh the selected marketplace, then open `/plugin` and use its update action if available:

```bash
agent plugin marketplace update fotor-ai
agent plugin marketplace list --format json
agent
```

Start a new CLI process and verify the installed version, resolved commit, package files, skills and production endpoint. Registration can store a resolved commit instead of a moving branch; a successful refresh message alone does not prove an upgrade. If the source stays pinned, repeat the selected channel's registration command and check a new process first.

If re-registration is rejected or the old package remains active, record the current source and scope, uninstall only that Fotor installation in `/plugin` (retain data when offered), run `agent plugin marketplace remove fotor-ai` for your recorded user-owned marketplace, and repeat the selected channel's registration and native installation steps. Verify the new files in a fresh process. Record this as a marketplace reset and reinstallation, not a successful native refresh. Preserve valid credentials; installation success does not establish authorization continuity. Do not remove a team/global marketplace or use account logout as an upgrade step.

For a local install, obtain the new complete package from the chosen channel, replace only that package, and reload the IDE or start a new CLI session. Local replacement is distinct from Git marketplace refresh and cached upgrades.

### Cursor marketplace migration

The catalog identity is `fotor-ai`; earlier development catalogs used `fotor-cursor`. Local `--plugin-dir` or IDE local-folder loads have no marketplace registration to rename. Replace the complete package, keep its directory basename `fotor`, reload or start a new session, and verify version and source.

For a real old marketplace registration, first verify the destination catalog, record the source and enabled state, and use the owning Cursor installation controls to remove the old Fotor registration and install from the new source. Do not substitute another host's commands. A marketplace rename requires its own migration checks; an installation or update result does not prove migration. Preserve credentials, and restore the recorded old source if the new installation fails.

### Cursor channel

Both channels use production MCP and credits. Select one Git marketplace source or one local package, then verify a fresh session uses the intended revision. A channel change is not an MCP environment change.

<details>
<summary>Choose the production or test release channel</summary>

#### Production release

First confirm [main](https://github.com/fotor-ai/fotor-agent-plugins/tree/main) contains the Cursor catalog and `plugins/cursor/fotor/` package. Register the selected Git source with the native CLI, then open `/plugin` (`/plugins` on clients that show that alias), select `fotor-ai` and install Fotor. Registration alone does not install the plugin. Both channels use production MCP and accounts. Record the current source and installation scope before switching. Keep one selected Fotor source; if re-registration is rejected or keeps the old commit, uninstall that Fotor installation through native controls, remove only your previous user-owned Fotor marketplace and register the selected source again. Preserve credentials and verify the newly installed files in a fresh session.

```bash
agent plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins
agent plugin marketplace list --format json
agent
```

#### Test release

First confirm [release/test](https://github.com/fotor-ai/fotor-agent-plugins/tree/release/test) contains the Cursor catalog and `plugins/cursor/fotor/` package. Register the selected Git source with the native CLI, then open `/plugin` (`/plugins` on clients that show that alias), select `fotor-ai` and install Fotor. Registration alone does not install the plugin. Both channels use production MCP and accounts. Record the current source and installation scope before switching. Keep one selected Fotor source; if re-registration is rejected or keeps the old commit, uninstall that Fotor installation through native controls, remove only your previous user-owned Fotor marketplace and register the selected source again. Preserve credentials and verify the newly installed files in a fresh session.

```bash
agent plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins --git-ref release/test
agent plugin marketplace list --format json
agent
```

</details>


## Documentation

- [Codex plugin guide](plugins/codex/fotor/README.md)
- [Claude Code plugin guide](plugins/claude-code/fotor/README.md)
- [Cursor plugin guide](plugins/cursor/fotor/README.md)

### Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

## License

[Apache-2.0](LICENSE)
