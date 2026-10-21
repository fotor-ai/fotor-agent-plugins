# Fotor for Codex

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Create images and videos, upload media, and process images with Fotor in Codex.

| Package | Value |
| --- | --- |
| Plugin | `fotor@fotor-ai` |
| Version | `0.5.1-test.1` |
| Release channel | `test` |
| Service environment | `production` |
| Publisher | Fotor |
| Source | [GitHub](https://github.com/fotor-ai/fotor-agent-plugins) |
| License | [Apache-2.0](LICENSE) |

**MCP service:** `https://mcp.fotor.com/mcp`.

Both release channels use Fotor's production service and website, including your production account and credits. The test suffix identifies a plugin prerelease.

## Codex setup

### Installation

Use Codex with plugin support and Git read access to the public repository; GitHub sign-in is not required. Install the complete marketplace package; no build or source synchronization is required. For an existing marketplace with another source, use [Switch release channel](#switch-release-channel).

#### Test release

Install the test build from `release/test` for prerelease validation.

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-ai
```

### Connect

Verify the installed version, enabled state, and selected endpoint before signing in:

```bash
codex plugin list --marketplace fotor-ai --json
```

The installed `.mcp.json` and any standalone Fotor MCP override must select the production endpoint above. Open a new Codex task and ask:

```text
Connect to Fotor and introduce its main features.
```

Reuse valid credentials. If sign-in is required, complete the client's OAuth flow in an external browser, then choose to continue in the localized confirmation prompt; waiting and cancellation are also available. Continue only after the client confirms authorization. The [website connection workflow](skills/fotor-connect/references/mcp-integration.md#connect-to-the-website) then uses one visible in-app browser visit and verifies the resulting website environment and login state. Browser limitations have a manual fallback; credentials stay in the client's supported store.

Ask **Check only the Fotor MCP connection** for a read-only check. Feature and model questions alone do not open the website or query credits.

### Updates

```bash
codex plugin marketplace upgrade fotor-ai
codex plugin add fotor@fotor-ai
codex plugin list --marketplace fotor-ai --json
```

Updates retain the selected source branch. Open a new task afterward and verify its version and effective MCP endpoint.

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

Choose the destination and run only its replacement sequence, outside other checkouts containing the same marketplace catalog.

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

Verify the destination version and production MCP configuration before signing in. Reuse valid production authorization. Stop on a legacy non-production configuration and update it through the host's controls before connecting; do not automatically migrate credentials.

## Workflows

| Request | Guide |
| --- | --- |
| Diagnose native authorization, permissions, or the website browser | [Codex authentication](skills/fotor-connect/references/authentication.md) |
| Connect, discover models, generate media, or check a submitted task | [MCP integration](skills/fotor-connect/references/mcp-integration.md) |
| Upload local media or prepare references for creation | [Media upload workflow](skills/fotor-connect/references/media-upload.md) |
| Check existing credits and reported limits | [Credit information](skills/fotor-connect/references/credits.md) |

The service's current schemas determine available models, input types, and parameters. Model-based generation uses model discovery; uploads, dedicated image upscaling, and background removal use their own contracts. A standalone upload ends after confirmed transfer. Creation continues only with usable inputs and the operation requested by the user.

Credit queries are independent of ordinary connection and generation. The [Codex credit policy](skills/fotor-connect/references/credits.md) reports existing balance and known limitations without recharge, purchase, subscription, upgrade or checkout guidance. Returned `top_up_url` fields are not displayed or opened. Verified informational help pages may be provided when needed; ordinary website connection remains separate. Before any `submit_*_task` call, the skill explains that success may consume Fotor credits, then proceeds under an already explicit task request and host permissions.

## Verification scope

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

The package is self-contained: its manifest, MCP declaration, license, skills, assets, and references remain usable when installed independently. Skill names and counts may evolve.

## Skill-local references

Every packaged skill carries four generated documents in its own `references/` directory, including this platform's authentication guide. Read them as needed; complete runtime documentation does not install or configure MCP.
