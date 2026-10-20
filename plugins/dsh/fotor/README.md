# Fotor for DeepSeek Harness

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Use Fotor's shared skills with dsh Web and a separately installed MCP bridge.

| Field | Value |
| --- | --- |
| Bundle | `fotor` |
| Version | `0.5.0-test.2` |
| Release channel | `test` |
| Service environment | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Host baseline | Linux dsh Web 0.2.1-rc.2 |
| Bridge baseline | mcp-remote 0.15.1 / Node 24.17.0 |
| License | Apache-2.0 |

## Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

The Bundle only declares host-owned components and resources. The bridge is installed separately and owns OAuth. Complete support requires real controlled reauthorization recovery as well as installation, calls, website access and restart reuse; consult the evidence for the installed release. Media execution and expiry-driven refresh are not established by static checks.

## dsh

### dsh installation

#### Test release

First verify that the selected revision contains the dsh Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup. 

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#release/test&path:/plugins/dsh/fotor"
```

This is a declarative Bundle for Linux dsh Web `0.2.1-rc.2`. It has no runtime module, dependency or lifecycle script. The public source must contain `plugins/dsh/fotor` before this command applies. Keep the Git subdirectory selector: the repository root is a Pi Package, not the dsh Bundle. Desktop, headless and ACP are outside this baseline.

Before installing, independently install and verify Node `24.17.0` and `mcp-remote 0.15.1` outside the Bundle. Use a dedicated bridge directory, a dependency lock, package integrity verification and disabled install lifecycle scripts. Do not use `npx latest` during connection. A complete local Bundle can be installed with `dsh plugin --profile web add /absolute/path/to/plugins/dsh/fotor`.

### dsh configuration and connection

Installation adds two disabled components. Find the installed package with `dsh plugin --profile web why fotor`, resolve its actual directory, and copy the [full native configuration example](configuration.example.yml) outside that directory. Fill all absolute paths and select a free callback port. Add both complete override rows to the chosen profile's native patch, or use its native `--patch` launch option. Overrides replace whole `config` objects; they do not deep-merge fields.

The skill provider scans only the installed Fotor skill directory, with default roots disabled. Enable skills first; leave the MCP row disabled. The MCP client directly invokes the separately installed bridge using structured `command`, `args`, `env` and `cwd`. Keep `MCP_REMOTE_CONFIG_DIR` dedicated to Fotor, outside package updates, and keep native automatic reconnect disabled. Resolve any existing `fotor` registration before enabling another.

Follow [dsh authentication](references/authentication.md) for standalone bridge authorization, native status verification and controlled recovery. OAuth is bridge-managed. Authorize with the fixed bridge client first, then enable the proxy using exactly the same store and connection arguments. User login, consent and Continue / Wait / Cancel remain explicit. Use the confirmed daily ordinary browser for authorization and website access; preserve its login state and data.

Invoke `/fotor-connect` through the actual skill menu or ask "Connect to Fotor and introduce its main features." Verify the loaded skill's source and perform a real model query. For MCP-only checks, do not request a website link. Website handoff opens the original production link once, using native single-command approval when necessary; user confirmation establishes the final login state. Manual opening is a separate fallback, and an already signed-in browser does not establish first-time website login.

### dsh updates

```bash
dsh plugin --profile web update fotor
dsh plugin --profile web why fotor
```

Inspect the resolved Git commit, version and files, update the absolute skill directory if its location changed, and restart into a new session. Verify only one Fotor skill source. Keep the bridge version, credential directory and security arguments unchanged. If native update remains on the old commit, retain that failure and use native removal/addition of the recorded source only as a separately reported reinstall path; do not modify caches.

### dsh source selection and removal

#### Production release

First verify that the selected revision contains the dsh Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup. Record the old source and scope, remove only this Fotor Bundle through native controls, then add the selected source. Preserve the external bridge and its credential directory; check the installed skill path before enabling the new package. 

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#main&path:/plugins/dsh/fotor"
```

#### Test release

First verify that the selected revision contains the dsh Bundle. Install the fixed external bridge separately, then add this package to the chosen profile. Both components stay disabled until local setup. Record the old source and scope, remove only this Fotor Bundle through native controls, then add the selected source. Preserve the external bridge and its credential directory; check the installed skill path before enabling the new package. 

```bash
dsh plugin --profile web add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#release/test&path:/plugins/dsh/fotor"
```

Before removal, disable the two Fotor components and remove only their external override rows. Run `dsh plugin --profile web remove fotor` in the original profile. Preserve the external Fotor credential store and browser state for a later reinstall; local store deletion is not server-side revocation. Configuration, installation, OAuth, tool execution, restart reuse and controlled reauthorization recovery require separate evidence.


## Workflows

Start with [connection and model discovery](references/mcp-integration.md#runtime-workflow), then select the skill matching the request. Follow [media preparation](references/media-upload.md) for local references and [credits](references/credits.md) for balance/pricing requests. These instructions do not establish host acceptance of every media operation.

## Bundle contents

- `package.json` and `cordis.patch.yml`: the declarative Bundle and two initially disabled components.
- `configuration.example.yml`: complete local setup example; never auto-applied.
- `skills/` and `references/`: shared workflows and dsh bridge/browser guidance.
- `assets/logo.svg`, `README.md` and `LICENSE`: package-local documentation and branding.

## License

[Apache-2.0](LICENSE)
