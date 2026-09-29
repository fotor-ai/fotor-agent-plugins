<p align="center">
  <img src="plugins/codex/fotor/assets/logo.svg" width="96" alt="Fotor logo" />
</p>

<h1 align="center">Fotor Plugin for Agents</h1>

<p align="center">Create images and videos with Fotor, directly from your AI agent.</p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.1.1&amp;color=087f8c&amp;style=flat-square" alt="Version 0.1.1" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=production&amp;color=238636&amp;style=flat-square" alt="production channel" />
  <img src="https://img.shields.io/static/v1?label=agent&amp;message=Codex&amp;color=24292f&amp;style=flat-square" alt="Available for Codex" />
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" alt="Apache-2.0 license" /></a>
</p>

<p align="center">
  <a href="#supported-agents">Agents</a> ·
  <a href="#features">Features</a> ·
  <a href="#codex">Codex setup</a> ·
  <a href="#documentation">Documentation</a>
</p>

## Supported Agents

| Agent | Status | Getting started |
| --- | --- | --- |
| **Codex** | Available | [Install and connect](#codex) |
| Claude Code | Planned | Installation instructions will accompany its release. |

This distribution includes the Codex plugin. Additional agent integrations will be documented when released.

## Features

| Capability | What you can do |
| --- | --- |
| **Images** | Generate images or edit them with prompts and references. |
| **Videos** | Generate from text, frames, or supported media references. |
| **Media uploads** | Upload local images, videos, and audio for reuse. |
| **Image processing** | Upscale images and remove backgrounds. |
| **Website connection** | Open Fotor with your authorized MCP identity. |
| **Credits** | Check your balance and open the top-up page. |
| **Task results** | Check submitted tasks and retrieve available results. |

Available models, parameters, and operations come from the connected Fotor service.

## Codex

### Installation

Use a Codex version with plugin support and Git access to this private repository. Configure Git authentication through a credential helper before installing. If `fotor-codex` is already registered from another source, follow [Switch release channel](#switch-release-channel).

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

2. Confirm version **0.1.1** and the **production** MCP endpoint `https://mcp.fotor.com/mcp` in the installed package. Check any standalone Fotor MCP override before authorization; it must select the intended environment.
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

After switching, open a new task and verify the destination version and effective MCP endpoint. Authorize the destination environment when needed; keep its credentials separate from the other environment.

### Troubleshooting

<details>
<summary>Repository access, stale installations, and sign-in</summary>

- **Repository access:** confirm your Git credential helper can read this private repository. Keep credentials outside repository URLs and plugin files.
- **Unexpected version or endpoint:** inspect the marketplace source, reinstall, and start a new task. Check both the installed MCP declaration and any standalone Fotor override.
- **Authorization required:** use the selected server's native sign-in flow in an external browser. For a matching standalone server named `fotor`, the CLI provides `codex mcp login fotor`.
- **Website or media operation fails:** retain the actual error and task ID when present. Connection, browser login, and media execution have separate outcomes; see the workflow references below.

</details>

## Documentation

- [Codex plugin guide](plugins/codex/fotor/README.md)
- [Connection, models, and task workflows](plugins/codex/fotor/references/mcp-integration.md)
- [Media upload workflow](plugins/codex/fotor/references/media-upload.md)
- [Credits and top-up workflow](plugins/codex/fotor/references/credits.md)

### Verification status

Production OAuth, authenticated tool discovery, and read-only model queries passed on 2026-09-24. New-build host installation, automatic website login, credit/top-up actions, uploads, and media execution still require separate acceptance.

## License

[Apache-2.0](LICENSE)
