# Fotor for DeepSeek Harness

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Use Fotor's shared skills with DeepSeek Harness Web and a separately installed MCP bridge.

| Field | Value |
| --- | --- |
| Bundle | `fotor` |
| Version | `0.5.0-test.5` |
| Release channel | `test` |
| Service environment | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Host baseline | Linux DeepSeek Harness Web 0.2.1-rc.2 |
| Bridge baseline | mcp-remote 0.15.1 / Node 24.17.0 |
| License | Apache-2.0 |

## Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

The Bundle only declares host-owned components and resources. The bridge is installed separately and owns OAuth. Complete support requires real controlled reauthorization recovery as well as installation, calls, website access and restart reuse; consult the evidence for the installed release. Media execution and expiry-driven refresh are not established by static checks.

<a id="dsh"></a>

## DeepSeek Harness

DeepSeek Harness is abbreviated as `dsh` in commands and native identifiers.

<a id="dsh-installation"></a>

### DeepSeek Harness installation

#### Test release

First verify that the selected revision contains the DeepSeek Harness Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup.

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#release/test&path:/plugins/dsh/fotor" --ignore-scripts
```

This is a declarative Bundle for Linux DeepSeek Harness Web `0.2.1-rc.2`. It has no runtime module, dependency or lifecycle script. The public source must contain `plugins/dsh/fotor` before this command applies. Keep the Git subdirectory selector: the repository root is a Pi Package, not the DeepSeek Harness Bundle. Desktop, headless and ACP are outside this baseline.

**Installing Fotor does not install mcp-remote.** You, or an Agent acting on your installation request, install the pinned bridge outside the Bundle; DeepSeek Harness later starts the configured bridge. Read the [complete installation and usage guide](INSTALL.md) before running the install command above:

1. Check the fixed DeepSeek Harness and Node baselines and obtain the complete DeepSeek Harness package.
2. Install the external bridge using the supplied JSON manifest and reviewed lock, with lifecycle scripts disabled.
3. Install Fotor, resolve its actual directory and apply the complete external configuration.
4. Enable skills first; authorize with the standalone bridge client before enabling MCP.
5. Verify actual queries, then use MCP-only or website connection as requested.

The package includes all setup materials. No internal repository document or automatic dependency installer is required. Use one selected source and preserve the external bridge and authorization store on updates.

<a id="dsh-configuration-and-connection"></a>

### DeepSeek Harness configuration and connection

Installation adds two disabled components. Follow the guide's native `root` and dependency queries to resolve the installed path and source; `why fotor` alone proves neither. Copy the [full native configuration example](configuration.example.yml) outside that directory. Fill all absolute paths and select a free callback port. Add both complete override rows to the chosen profile's native patch, or use its native `--patch` launch option. Overrides replace whole `config` objects; they do not deep-merge fields.

The skill provider scans only the installed Fotor skill directory, with default roots disabled. Enable skills first; leave the MCP row disabled. The MCP client directly invokes the separately installed bridge using structured `command`, `args`, `env` and `cwd`. Keep `MCP_REMOTE_CONFIG_DIR` dedicated to Fotor, outside package updates, and keep native automatic reconnect disabled. Resolve any existing `fotor` registration before enabling another.

Follow [DeepSeek Harness authentication](skills/fotor-connect/references/authentication.md) for standalone bridge authorization, native status verification and controlled recovery. OAuth is bridge-managed. Authorize with the fixed bridge client first, then enable the proxy using exactly the same store and connection arguments. User login, consent and Continue / Wait / Cancel remain explicit. Use the confirmed daily ordinary browser for authorization and website access; preserve its login state and data.

Invoke `/fotor-connect` through the actual skill menu or ask "Connect to Fotor and introduce its main features." Verify the loaded skill's source and perform a real model query. For MCP-only checks, do not request a website link. Website handoff opens the original production link once, using native single-command approval when necessary; user confirmation establishes the final login state. Manual opening is a separate fallback, and an already signed-in browser does not establish first-time website login.

<a id="dsh-updates"></a>

### DeepSeek Harness updates

```bash
dsh plugin --profile web update fotor --ignore-scripts
dsh plugin --profile web why fotor
```

Inspect the resolved Git commit, version and files, update the absolute skill directory if its location changed, and restart into a new session. Retain the original pnpm store; see the guide when a dedicated `--store-dir` was selected. Verify only one Fotor skill source. Keep the bridge version, credential directory and security arguments unchanged. If native update remains on the old commit, retain that failure and use native removal/addition of the recorded source only as a separately reported reinstall path; do not modify caches.

<a id="dsh-source-selection-and-removal"></a>

### DeepSeek Harness source selection and removal

#### Production release

First verify that the selected revision contains the DeepSeek Harness Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup. Record the old source and scope, remove only this Fotor Bundle through native controls, then add the selected source. Preserve the external bridge and its credential directory; check the installed skill path before enabling the new package.

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#main&path:/plugins/dsh/fotor" --ignore-scripts
```

#### Test release

First verify that the selected revision contains the DeepSeek Harness Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup. Record the old source and scope, remove only this Fotor Bundle through native controls, then add the selected source. Preserve the external bridge and its credential directory; check the installed skill path before enabling the new package.

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#release/test&path:/plugins/dsh/fotor" --ignore-scripts
```

Before removal, disable the two Fotor components and remove only their external override rows. Run `dsh plugin --profile web remove fotor --config.ignore-scripts=true` in the original profile and store. This baseline does not accept `--ignore-scripts` for removal. Preserve the external Fotor credential store and browser state for a later reinstall; local store deletion is not server-side revocation. Configuration, installation, OAuth, tool execution, restart reuse and controlled reauthorization recovery require separate evidence.


## Workflows

Start with [connection and model discovery](skills/fotor-connect/references/mcp-integration.md#runtime-workflow), then select the skill matching the request. Follow [media preparation](skills/fotor-connect/references/media-upload.md) for local references and [credits](skills/fotor-connect/references/credits.md) for balance/pricing requests. These instructions do not establish host acceptance of every media operation.

## Bundle contents

- `package.json` and `cordis.patch.yml`: the declarative Bundle and two initially disabled components.
- `configuration.example.yml`: complete local setup example; never auto-applied.
- `INSTALL.md`, `bridge-package.example.json` and `bridge-package-lock.example.json`: guide and inert inputs copied outside the Bundle for a separate bridge installation.
- `skills/`, including each skill's `references/`: shared workflows and DeepSeek Harness bridge/browser guidance.
- `assets/logo.svg`, `README.md` and `LICENSE`: package-local documentation and branding.

## License

[Apache-2.0](LICENSE)

## Skill-local references

Every packaged skill carries four generated documents in its own `references/` directory, including this platform's authentication guide. Read them as needed; complete runtime documentation does not install or configure MCP.
