<p align="center">
  <img src="assets/logo.svg" width="96" alt="Fotor logo" />
</p>

<h1 align="center">Fotor Plugin for Agents</h1>

<p align="center">Create images and videos with Fotor, directly from your AI agent.</p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.5.1&amp;color=087f8c&amp;style=flat-square" alt="Version 0.5.1" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=production&amp;color=238636&amp;style=flat-square" alt="production channel" />
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" alt="Apache-2.0 license" /></a>
</p>

<p align="center">
  <a href="#codex"><img src="https://img.shields.io/static/v1?label=&amp;message=Codex&amp;color=24292f&amp;style=flat-square" alt="Codex" /></a>
  <a href="#claude-code"><img src="https://img.shields.io/static/v1?label=&amp;message=Claude+Code&amp;color=24292f&amp;style=flat-square" alt="Claude Code" /></a>
  <a href="#cursor"><img src="https://img.shields.io/static/v1?label=&amp;message=Cursor&amp;color=24292f&amp;style=flat-square" alt="Cursor" /></a>
  <a href="#grok-bot-installation-and-authorization"><img src="https://img.shields.io/static/v1?label=&amp;message=Grok+Bot&amp;color=24292f&amp;style=flat-square" alt="Grok Bot" /></a>
  <a href="#pi"><img src="https://img.shields.io/static/v1?label=&amp;message=Pi&amp;color=24292f&amp;style=flat-square" alt="Pi" /></a>
  <a href="#dsh"><img src="https://img.shields.io/static/v1?label=&amp;message=DeepSeek+Harness&amp;color=24292f&amp;style=flat-square" alt="DeepSeek Harness" /></a>
</p>

<p align="center">
  <a href="#supported-agents">Agents</a> ·
  <a href="#features">Features</a> ·
  <a href="#codex">Codex setup</a> · <a href="#claude-code">Claude Code setup</a> · <a href="#cursor">Cursor setup</a> · <a href="#pi">Pi setup</a> · <a href="#dsh">DeepSeek Harness setup</a> ·
  <a href="#documentation">Documentation</a>
</p>

## Supported Agents

| Agent | Status | Getting started |
| --- | --- | --- |
| **Codex** | Included | [Install and connect](#codex) |
| **Claude Code** | Included | [Install and connect](#claude-code) |
| **Cursor** | Included | [Install and connect](#cursor) |
| **Grok Bot** | Via Cursor package | [Install and connect](#grok-bot-installation-and-authorization) |
| **Pi** | Included | [Install and connect](#pi) |
| **DeepSeek Harness** | Separate repository; external bridge | [Install and connect](#dsh) |

This snapshot includes Codex and Claude Code and Cursor and Pi. Installation and live-service acceptance are separate checks; see the verification scope below.

Both release channels connect to Fotor's production service and website. The test channel previews plugin changes; it uses your production account and credits.

Each skill includes its own four runtime reference documents. Platform installation, MCP registration and any external bridge setup remain separate prerequisites.

## Features

| Capability | What you can do |
| --- | --- |
| **Images** | Generate images or edit them with prompts and references. |
| **Videos** | Generate from text, frames, or supported media references. |
| **Media uploads** | Upload local images, videos, and audio for reuse. |
| **Image processing** | Upscale images and remove backgrounds. |
| **Website connection** | Open Fotor with your authorized MCP identity. |
| **Credits** | Check existing credits and understand reported limits under the platform's credit policy. |
| **Task results** | Check submitted tasks and retrieve available results. |

Available models, parameters, and operations come from the connected Fotor service.

## Codex

### Installation

Use a Codex version with plugin support and Git access to this public repository. GitHub sign-in is not required to install the public package. If `fotor-ai` is already registered from another source, follow [Switch release channel](#switch-release-channel).

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-ai
```

### Connect

1. Check the installed plugin and enabled state:

   ```bash
   codex plugin list --marketplace fotor-ai --json
   ```

2. Confirm version **0.5.1**, release channel **production**, and the **production** MCP endpoint `https://mcp.fotor.com/mcp` in the installed package. Check any standalone Fotor MCP override before authorization; it must use the same production endpoint.
3. Open a new Codex task and ask:

   ```text
   Connect to Fotor and introduce its main features.
   ```

Valid credentials are reused. When authorization is needed, complete Codex's sign-in flow in an external browser, then choose "Signed in and authorized; open Fotor in the in-app browser" in the localized confirmation prompt. You can also choose to keep waiting or cancel. After Codex confirms authorization, the plugin opens the website sign-in link in a visible in-app browser. See the [connection workflow](plugins/codex/fotor/skills/fotor-connect/references/mcp-integration.md#connect-to-the-website) for verification and manual fallback.

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

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required.

```bash
claude plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --scope user
claude plugin install fotor@fotor-ai --scope user
```

Start a new Claude Code session. Check `claude plugin list --json` for `fotor@fotor-ai`, version **0.5.1**, and its enabled state. Verify the installed `.mcp.json` uses `https://mcp.fotor.com/mcp` and resolve conflicting host overrides before connecting.

### Claude Code connection

Ask **Connect to Fotor and introduce its main features.** You can also invoke `/fotor:fotor-connect`. For a read-only connection check, ask **Check only the Fotor MCP connection.**

Use `/mcp` to inspect the plugin's server, normally `plugin:fotor:fotor`. Reuse valid Claude-managed Fotor credentials. When native status requires login, use its Authenticate action or the supported CLI entry:

```bash
claude mcp login plugin:fotor:fotor
```

Complete OAuth in an external browser, choose Continue in the localized Continue / Wait / Cancel prompt, and wait for the client to confirm success. In Claude Code CLI, open the verified handoff once in an external browser such as Chrome; if the CLI cannot open it, copy the unused link there manually. In Claude Code Desktop, open the handoff once in its built-in browser after external OAuth. If the selected browser control is unavailable, explain the limitation and provide the unused link with instructions for that browser. Manual delivery is not automatic-login verification; Desktop host acceptance remains separate. See [Claude authentication and browser guidance](plugins/claude-code/fotor/skills/fotor-connect/references/authentication.md).

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

This guide covers Cursor IDE, Cursor CLI, and Grok Bot through the same Cursor package. Use a complete package with `.cursor-plugin/plugin.json`, `mcp.json`, skills, references, and assets. No interpreter or build step is required. Cursor Cloud Agent acceptance is separate.

#### Production release

First confirm [main](https://github.com/fotor-ai/fotor-agent-plugins/tree/main) contains the Cursor catalog and `plugins/cursor/fotor/` package. Register the selected Git source with the native CLI, then open `/plugin` (`/plugins` on clients that show that alias), select `fotor-ai` and install Fotor. Registration alone does not install the plugin. Both channels use production MCP and accounts.

```bash
agent plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins
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

Verify version **0.5.1**, enabled state, discovered skills, and the `https://mcp.fotor.com/mcp` URL in the installed `mcp.json`. Resolve conflicting host overrides before connecting. Marketplace installation and local loading are separate routes; do not substitute Codex or Claude commands.

### Grok Bot installation and authorization

1. Install Fotor from the `fotor-ai` marketplace in Cursor using the selected Git source above. A marketplace registration alone does not install the plugin. Use an account-level installation; local IDE folders and CLI `--plugin-dir` loads do not establish account sharing.
2. Sign in to Grok Bot with that same Cursor account. Open **Connect Apps**, select the installed/connected list, and find **Fotor** from `fotor-ai`. Check version **0.5.1** and source when the host exposes them; account sharing alone does not verify the loaded files.
3. For a connection that needs authorization, choose the native **Authorize / Authenticate** action and complete login and consent in the external normal browser. Return to Grok Bot; during a chat flow, choose Continue and verify the native connection result. Reuse valid authorization when available.
4. Confirm **Connected**, inspect available tools, and ask **Check only the Fotor MCP connection.** A successful read-only call verifies access. This request does not open a website or query balance.

If Fotor is absent, check the account, installation scope and team policy through native controls. Use Grok Bot's own connector controls rather than local CLI authentication or copied tokens. Website requests use its actual visible browser and require signed-in-state evidence; an unavailable browser uses a clearly identified manual link. Use readable attachments or Bot-workspace files for uploads. See the [host-specific authentication and browser guide](plugins/cursor/fotor/skills/fotor-connect/references/authentication.md).

After updating the account-installed plugin, recheck it in Grok Bot and use a new conversation. Removing that installation or connection can affect other clients or Bots; preserve valid credentials and inspect the scope first. Website handoff, restart/expiry reuse, and individual media workflows require separate checks; account sharing does not establish all of them.

### Cursor connection

Ask **Connect to Fotor and introduce its main features.** Select the actual connection skill from the native slash menu for an explicit invocation. Ask **Check only the Fotor MCP connection** to finish without a website link.

Use IDE MCP controls or CLI `/mcp` to inspect the actual plugin server; Grok Bot uses **Connect Apps** as described above. When the CLI supports them in the same loading context, `agent mcp list`, `agent mcp list-tools <identifier>`, and `agent mcp login <identifier>` provide native inspection and login. Cursor account login and Fotor MCP OAuth are separate.

Reuse valid Fotor credentials. When authorization is required, complete it in an external browser, choose Continue, and wait for client-confirmed success. Website requests use the IDE's visible built-in browser, the CLI's permitted external-browser opener, or Grok Bot's available visible browser. If unavailable, provide the unused link for the specified browser and await confirmation. Verify the final production website and signed-in state. See [Cursor and Grok Bot native guidance](plugins/cursor/fotor/skills/fotor-connect/references/authentication.md).

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


## Pi

### Pi installation

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required. First verify that the selected revision contains the Pi package and root Pi manifest. Installation loads skills only; register the Fotor MCP separately.

```bash
pi install https://github.com/fotor-ai/fotor-agent-plugins
pi list
```

Pi installs a Package, not a Marketplace entry. The root manifest selects only the Pi skills. Git installation requires Git and Pi's configured package manager even though Fotor has no dependencies or runtime scripts. A local independent package can instead be installed with `pi install /absolute/path/to/fotor`; local sources are loaded in place.

### Pi MCP registration and connection

Package installation does not register MCP. Inspect `/mcp` and the effective user/project configuration first; preserve a different existing `fotor` registration and resolve that choice before adding it. The [declarative example](plugins/pi/fotor/mcp.example.json) describes the production entry:

```bash
pi mcp add fotor --url https://mcp.fotor.com/mcp --exposure deferred --description "Connect to Fotor's production MCP service."
```

Use Pi's native `/mcp` sign-in or `pi mcp login fotor` when authorization is required. Complete OAuth in an external ordinary browser, choose Continue / Wait / Cancel conversationally, and verify the native callback before proceeding. Pi's model-provider login is separate. Follow [Pi authentication](plugins/pi/fotor/skills/fotor-connect/references/authentication.md) for tool discovery, overrides, and recovery.

Ask "Connect to Fotor and introduce its main features." or invoke `/skill:fotor-connect`. The website flow opens a fresh validated link in an external ordinary browser when the host can target it; otherwise it supplies a link for immediate manual opening. A native opener's success does not prove website login. An explicit MCP-only request does not create a website link.

### Pi updates

Update only the recorded Fotor source, using the same installation scope:

```bash
pi update <recorded-source>
pi list
```

`pi update --extensions` updates all installed Packages; bare `pi update` updates Pi itself. Restart Pi and verify the resolved revision, package version, actual skills and production MCP. Git tags and commit selectors remain pinned; check actual installed files after a branch update.

### Pi channel

Git Package identity is the repository URL without its ref. Keep one Fotor source per configuration. Record the old source and scope for recovery, remove only that Package, and install the chosen channel; retain MCP registration and authorization. Only use a channel after it contains the Pi package. Append `--local` to native install/remove commands only when using project scope.

#### Production release

Install the released plugin from the default `main` branch. No branch selector is required. First verify that the selected revision contains the Pi package and root Pi manifest. Installation loads skills only; register the Fotor MCP separately. Record the current source and scope, remove that package with `pi remove <recorded-source>`, then install the selected source in the same scope. Keep MCP configuration and credentials; verify one active Fotor package in a new session.

```bash
pi install https://github.com/fotor-ai/fotor-agent-plugins
pi list
```

#### Test release

Install the test build from `release/test` for prerelease validation. First verify that the selected revision contains the Pi package and root Pi manifest. Installation loads skills only; register the Fotor MCP separately. Record the current source and scope, remove that package with `pi remove <recorded-source>`, then install the selected source in the same scope. Keep MCP configuration and credentials; verify one active Fotor package in a new session.

```bash
pi install https://github.com/fotor-ai/fotor-agent-plugins@release/test
pi list
```


<a id="dsh"></a>

## DeepSeek Harness

DeepSeek Harness is abbreviated as `dsh` in commands and native identifiers. Its Fotor Bundle is distributed only through the separate **[dsh-fotor-plugin repository](https://github.com/fotor-ai/dsh-fotor-plugin)**. This snapshot contains installation guidance, not a DeepSeek Harness package. The repository root remains the Pi entry.

### DeepSeek Harness installation

#### Production release

First verify that the selected revision contains the DeepSeek Harness Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup.

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/dsh-fotor-plugin.git" --ignore-scripts
```

**Installing Fotor does not install mcp-remote.** Install the pinned external bridge from the independent package's JSON materials, resolve the installed Bundle path and configure skills before authorizing and enabling MCP. Both native components are disabled by default. Follow the [complete installation guide](https://github.com/fotor-ai/dsh-fotor-plugin/blob/HEAD/INSTALL.md), [configuration example](https://github.com/fotor-ai/dsh-fotor-plugin/blob/HEAD/configuration.example.yml) and [authorization instructions](https://github.com/fotor-ai/dsh-fotor-plugin/blob/HEAD/INSTALL.md#6-authorize-then-enable-the-connection). Preserve the daily browser and external credential store.

The declared host range is `>=0.2.1-rc.2 <0.3.0-0`; actual acceptance is limited to the recorded Linux Web 0.2.1-rc.2 baseline. A matching version is not automatically tested.

### Migrate an existing subdirectory installation

Existing installations from this repository do not migrate through an ordinary update. Record the source, profile, package-manager store, installed path and external configuration; stop both Fotor components and remove only their external override rows. Natively remove the old Bundle using `dsh plugin --profile web remove fotor --config.ignore-scripts=true`, then add the independent source above in the original profile/store. Resolve the new path and restore one complete configuration with the new absolute skill directory. Keep the external bridge, credential directory, model configuration and connection/security parameters unchanged.

Retain exactly one `fotor` source and one component pair. Future native updates follow the independent repository. Historical v0.5.0 snapshots remain unchanged; the old subdirectory is not a continuing installation or update channel. See [maintenance instructions](https://github.com/fotor-ai/dsh-fotor-plugin/blob/HEAD/INSTALL.md#7-use-update-or-remove-fotor).


## Documentation

- [Codex plugin guide](plugins/codex/fotor/README.md)
- [Claude Code plugin guide](plugins/claude-code/fotor/README.md)
- [Cursor plugin guide](plugins/cursor/fotor/README.md)
- [Pi plugin guide](plugins/pi/fotor/README.md)
- [DeepSeek Harness installation guide](https://github.com/fotor-ai/dsh-fotor-plugin/blob/HEAD/INSTALL.md) (separate repository)

### Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

## License

[Apache-2.0](LICENSE)
