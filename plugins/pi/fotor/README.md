# Fotor for Pi

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Create images and videos with Fotor through Pi's native MCP tools and shared skills.

| Field | Value |
| --- | --- |
| Package | `fotor` |
| Version | `0.4.0-test.1` |
| Release channel | `test` |
| Service environment | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Initial host baseline | Pi CLI 1.3.0 |
| License | Apache-2.0 |

## Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

This instruction-only Package contains no Extension or lifecycle script. Installing it loads skills; native MCP registration and authorization are separate steps.

## Pi

### Pi installation

#### Test release

Install the test build from `release/test` for prerelease validation. First verify that the selected revision contains the Pi package and root Pi manifest. Installation loads skills only; register the Fotor MCP separately.

```bash
pi install https://github.com/fotor-ai/fotor-agent-plugins@release/test
pi list
```

Pi installs a Package, not a Marketplace entry. The root manifest selects only the Pi skills. Git installation requires Git and Pi's configured package manager even though Fotor has no dependencies or runtime scripts. A local independent package can instead be installed with `pi install /absolute/path/to/fotor`; local sources are loaded in place.

### Pi MCP registration and connection

Package installation does not register MCP. Inspect `/mcp` and the effective user/project configuration first; preserve a different existing `fotor` registration and resolve that choice before adding it. The [declarative example](mcp.example.json) describes the production entry:

```bash
pi mcp add fotor --url https://mcp.fotor.com/mcp --exposure deferred --description "Connect to Fotor's production MCP service."
```

Use Pi's native `/mcp` sign-in or `pi mcp login fotor` when authorization is required. Complete OAuth in an external ordinary browser, choose Continue / Wait / Cancel conversationally, and verify the native callback before proceeding. Pi's model-provider login is separate. Follow [Pi authentication](references/authentication.md) for tool discovery, overrides, and recovery.

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


## Workflows

Start with [connection and model discovery](references/mcp-integration.md#runtime-workflow), then follow the skill matching the request. Use [media preparation](references/media-upload.md) for local references and [credits](references/credits.md) for balance/pricing requests. Runtime guidance and assets remain inside this independent package.

## Package contents

- `package.json`: Pi skill resource selection; no executable resources or dependencies.
- `mcp.example.json`: native configuration example, not an auto-loaded server registration.
- `skills/`: the currently packaged Fotor task instructions.
- `references/`: shared workflows and Pi-owned authentication guidance.
- `assets/logo.svg`, `README.md`, and `LICENSE`: package-local branding and documentation.

## License

[Apache-2.0](LICENSE)
