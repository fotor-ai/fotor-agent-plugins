# Fotor for Codex

<img src="assets/logo.svg" alt="Fotor logo" width="96" />

| Metadata | Value |
| --- | --- |
| Plugin | `fotor` |
| Version | `0.1.0` |
| Environment | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Publisher | Fotor |
| Source | [GitHub](https://github.com/fotor-ai/fotor-agent-plugins) |
| License | [Apache-2.0](LICENSE) |

This build selects the production service. Production connectivity and media operations require separate acceptance; packaging does not establish availability.

## Install and connect

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-codex
```

For an existing marketplace registration, explicitly select the intended source using the client's remove/add commands. Open a new task after installation, discover the actual tools, and verify the selected MCP endpoint. Reuse valid client-stored credentials. When authorization is needed, run the supported OAuth flow (for example, `codex mcp login fotor`) in an external browser and wait for client-confirmed success. Website browser login is a separate session.

Ask "Connect to Fotor and introduce its main features" to authenticate MCP, call `get_website_url`, validate its handoff endpoint, and open the returned link once in a visible in-app browser. The response contains `handoff_url` and `expires_in`; verify the final website environment and authenticated state after navigation before reporting login success. Follow the [website connection flow](references/mcp-integration.md#connect-to-the-website), including its manual fallback. Explicit MCP-only checks and feature/model questions omit website navigation.

The package bundles website connection, feature/model discovery, local-media uploads, image generation/editing, standalone image upscaling and background removal, and video generation workflows. Available operations and inputs are determined by the service's actual tool schemas. For read-only MCP checks, use `list_models` when available. Inspect model details for model-based generation/editing; uploads and fixed image-processing operations need no model choice. Follow the shared [upload workflow](references/media-upload.md) for local files: only a confirmed PUT upload makes its `file_url` ready for use. Website connection requests authorize the handoff; uploads, image processing, and generation follow the specific operation requested by the user. Read [MCP integration and runtime workflow](references/mcp-integration.md) before using Fotor tools. Skill names and counts may evolve independently of this distribution layout.

The installation package contains its manifest, MCP declaration, license, resources, and workflow instructions. Installation requires no build or source synchronization step.

For balance and recharge requests, use the [credits workflow](references/credits.md). A positive balance query returns only the balance; zero balance or an explicit top-up request selects the recharge page unless the user forbids web actions. Opening that page does not perform a payment.
