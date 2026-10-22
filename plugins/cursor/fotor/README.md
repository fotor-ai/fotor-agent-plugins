# Fotor for Cursor and Grok Bot

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Create images and videos, upload media, process images, connect to the website, and check credits with Fotor in Cursor IDE, Cursor CLI, and Grok Bot.

| Package | Value |
| --- | --- |
| Plugin | `fotor` |
| Marketplace | `fotor-ai` |
| Hosts | Cursor IDE / CLI; Grok Bot through the same package |
| Version | `0.5.1` |
| Release channel | `production` |
| Service environment | `production` |
| Source | [GitHub](https://github.com/fotor-ai/fotor-agent-plugins) |
| License | [Apache-2.0](LICENSE) |

Both channels use production accounts and credits. A test suffix identifies a plugin prerelease.

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

If Fotor is absent, check the account, installation scope and team policy through native controls. Use Grok Bot's own connector controls rather than local CLI authentication or copied tokens. Website requests use its actual visible browser and require signed-in-state evidence; an unavailable browser uses a clearly identified manual link. Use readable attachments or Bot-workspace files for uploads. See the [host-specific authentication and browser guide](skills/fotor-connect/references/authentication.md).

After updating the account-installed plugin, recheck it in Grok Bot and use a new conversation. Removing that installation or connection can affect other clients or Bots; preserve valid credentials and inspect the scope first. Website handoff, restart/expiry reuse, and individual media workflows require separate checks; account sharing does not establish all of them.

### Cursor connection

Ask **Connect to Fotor and introduce its main features.** Select the actual connection skill from the native slash menu for an explicit invocation. Ask **Check only the Fotor MCP connection** to finish without a website link.

Use IDE MCP controls or CLI `/mcp` to inspect the actual plugin server; Grok Bot uses **Connect Apps** as described above. When the CLI supports them in the same loading context, `agent mcp list`, `agent mcp list-tools <identifier>`, and `agent mcp login <identifier>` provide native inspection and login. Cursor account login and Fotor MCP OAuth are separate.

Reuse valid Fotor credentials. When authorization is required, complete it in an external browser, choose Continue, and wait for client-confirmed success. Website requests use the IDE's visible built-in browser, the CLI's permitted external-browser opener, or Grok Bot's available visible browser. If unavailable, provide the unused link for the specified browser and await confirmation. Verify the final production website and signed-in state. See [Cursor and Grok Bot native guidance](skills/fotor-connect/references/authentication.md).

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


## Workflows

- [Connection, model selection, and tasks](skills/fotor-connect/references/mcp-integration.md)
- [Cursor and Grok Bot native authentication](skills/fotor-connect/references/authentication.md)
- [Media uploads](skills/fotor-connect/references/media-upload.md)
- [Credit balance and pricing links](skills/fotor-connect/references/credits.md)

The service's current schemas determine supported models and parameters. Upload success, task submission, and completed results are separate states. Ordinary balance queries do not open a browser. Payment stays under the user's control.

## Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

This package is self-contained and script-free. Local loading and local replacement do not establish marketplace listing or cached updates. Skills and their names may evolve.

## Skill-local references

Every packaged skill carries four generated documents in its own `references/` directory, including this platform's authentication guide. Read them as needed; complete runtime documentation does not install or configure MCP.
