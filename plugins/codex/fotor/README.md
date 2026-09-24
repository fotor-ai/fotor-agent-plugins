# Fotor for Codex

<img src="assets/logo.svg" width="72" alt="Fotor logo" />

Create images and videos, upload media, and process images with Fotor in Codex.

| Package | Value |
| --- | --- |
| Plugin | `fotor@fotor-codex` |
| Version | `0.1.0-test.6` |
| Environment | `test` |
| Publisher | Fotor |
| Source | [GitHub](https://github.com/fotor-ai/fotor-agent-plugins) |
| License | [Apache-2.0](LICENSE) |

**MCP service:** `https://test-mcp.fotor.com/mcp`.
## Codex setup

### Installation

Use Codex with plugin support and Git read access to the private repository. Install the complete marketplace package; no build or source synchronization is required. For an existing marketplace with another source, use [Switch release channel](#switch-release-channel).

#### Test channel

Use the test branch for prerelease validation.

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-codex
```

### Connect

Verify the installed version, enabled state, and selected endpoint before signing in:

```bash
codex plugin list --marketplace fotor-codex --json
```

The installed `.mcp.json` and any standalone Fotor MCP override must select the intended environment. Open a new Codex task and ask:

```text
Connect to Fotor and introduce its main features.
```

Reuse valid credentials. If sign-in is required, complete the client's OAuth flow in an external browser and wait for client-confirmed success. The [website connection workflow](references/mcp-integration.md#connect-to-the-website) then uses one visible in-app browser visit and verifies the resulting website environment and login state. Browser limitations have a manual fallback; credentials stay in the client's supported store.

Ask **Check only the Fotor MCP connection** for a read-only check. Feature and model questions alone do not open the website or query credits.

### Updates

```bash
codex plugin marketplace upgrade fotor-codex
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

Updates retain the selected source branch. Open a new task afterward and verify its version and effective MCP endpoint.

### Switch release channel

Choose the destination and run only its replacement sequence, outside other checkouts containing the same marketplace catalog.

<details>
<summary>Channel replacement commands</summary>

#### Test channel

Use the test branch for prerelease validation.

```bash
codex plugin marketplace remove fotor-codex
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

#### Production candidate (before merge)

Use this versioned branch while an existing production candidate is under review.

```bash
codex plugin marketplace remove fotor-codex
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/v0.1.0
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

#### Stable production (after merge)

Use the default branch only after the production release has been merged into `main`.

```bash
codex plugin marketplace remove fotor-codex
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-codex
codex plugin list --marketplace fotor-codex --json
```

</details>

Verify the destination version and effective MCP configuration before signing in. Keep production and test authorization separate.

## Workflows

| Request | Guide |
| --- | --- |
| Connect, discover models, generate media, or check a submitted task | [MCP integration](references/mcp-integration.md) |
| Upload local media or prepare references for creation | [Media upload workflow](references/media-upload.md) |
| Check credits or open the recharge entry | [Credits and top-up](references/credits.md) |

The service's current schemas determine available models, input types, and parameters. Model-based generation uses model discovery; uploads, dedicated image upscaling, and background removal use their own contracts. A standalone upload ends after confirmed transfer. Creation continues only with usable inputs and the operation requested by the user.

Credit queries are independent of ordinary connection and generation. A positive balance query ends with the balance; zero balance or an explicit top-up request selects the recharge entry subject to user browsing restrictions. Payment remains under the user's control.

## Verification status

Earlier test OAuth and model queries passed. The last recorded test discovery attempt on 2026-09-24 timed out, so current test connectivity remains unverified. New-build host installation, automatic website login, credit/top-up actions, uploads, and media execution still require separate acceptance.

The package is self-contained: its manifest, MCP declaration, license, skills, assets, and references remain usable when installed independently. Skill names and counts may evolve.
