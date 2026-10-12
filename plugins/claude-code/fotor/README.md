# Fotor for Claude Code

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Create images and videos, upload media, process images, connect to the website, and check credits with Fotor.

| Package | Value |
| --- | --- |
| Plugin | `fotor@fotor-claude-code` |
| Version | `0.2.0` |
| Release channel | `production` |
| Service environment | `production` |
| Source | [GitHub](https://github.com/fotor-ai/fotor-agent-plugins) |
| License | [Apache-2.0](LICENSE) |

Both channels use production accounts and credits. A test suffix identifies a plugin prerelease.

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

Complete OAuth in an external browser, choose Continue in the localized Continue / Wait / Cancel prompt, and wait for the client to confirm success. In Claude Code CLI, open the verified handoff once in an external browser such as Chrome; if the CLI cannot open it, copy the unused link there manually. In Claude Code Desktop, open the handoff once in its built-in browser after external OAuth. If the selected browser control is unavailable, explain the limitation and provide the unused link with instructions for that browser. Manual delivery is not automatic-login verification; Desktop host acceptance remains separate. See [Claude authentication and browser guidance](references/claude-code-authentication.md).

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


## Workflows

- [Connection, model selection, and tasks](references/mcp-integration.md)
- [Claude Code native authentication](references/claude-code-authentication.md)
- [Media uploads](references/media-upload.md)
- [Credit balance and pricing links](references/credits.md)

The service's current schemas determine supported models and parameters. Upload success, task submission, and completed results are separate states. Ordinary balance queries do not open a browser. Payment stays under the user's control.

## Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

This package is self-contained. Skills remain instructions and non-executable resources; their names and counts may evolve.
