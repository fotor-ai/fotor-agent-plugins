# Fotor for Codex

Version **0.1.0**, configured for the **production** MCP service.

This build selects the production service. Production connectivity and media operations require separate acceptance; packaging does not establish availability.

## Installation

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git
codex plugin add fotor@fotor-codex
```

If this private repository requires Git authentication, configure a credential helper before installation. When replacing an existing `fotor-codex` registration, inspect its source and use the client's marketplace remove/add commands to select this source explicitly.

Run marketplace source changes outside another checkout containing the same catalog. Open a new Codex task after installation. Verify the installed version and endpoint `https://mcp.fotor.com/mcp`, then reuse valid MCP credentials or complete OAuth with `codex mcp login fotor` in an external browser when needed. The client keeps credentials outside the plugin package.

Ask "Connect to Fotor and introduce its main features" to follow the [website connection flow](plugins/codex/fotor/references/mcp-integration.md#connect-to-the-website): required external-browser OAuth, then the environment-checked `get_website_url` sign-in link in a visible in-app browser. If browser automation is unavailable, the flow explains the limitation and provides a manual link.

For an explicitly MCP-only, read-only connection check, discover the actual tools and call `list_models`. Use media/mode filters and a selected model ID to inspect supported parameters. Upload-address requests, file transfers, image processing, generation, task lookup, and website sign-in links are separate actions; a model query does not verify upstream media execution.

Use the [upload workflow](plugins/codex/fotor/references/media-upload.md) to upload local images, videos, or audio independently or as creation references. Image upscaling and background removal use dedicated tools without model selection.

See the [plugin guide](plugins/codex/fotor/README.md) for connection behavior and the [Apache-2.0 license](LICENSE).

## Updates

```bash
codex plugin marketplace upgrade fotor-codex
codex plugin add fotor@fotor-codex
```

A branch-pinned installation keeps its selected branch. To change channels, explicitly replace the marketplace registration, reinstall, and verify the endpoint in a new task.

For balance and recharge requests, use the [credits workflow](plugins/codex/fotor/references/credits.md). A positive balance query returns only the balance; zero balance or an explicit top-up request selects the recharge page unless the user forbids web actions. Opening that page does not perform a payment.
