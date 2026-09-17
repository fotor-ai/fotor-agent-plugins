# Fotor for Codex

Version **0.1.0-test.4**, configured for the **test** MCP service.

The test service has passed Codex OAuth, tool discovery, and read-only model queries. Media submission and task-query tools are exposed; actual image/video execution has not been accepted. Installing this build does not establish host acceptance.

## Installation

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-codex
```

If this private repository requires Git authentication, configure a credential helper before installation. When replacing an existing `fotor-codex` registration, inspect its source and use the client's marketplace remove/add commands to select this source explicitly.

Run marketplace source changes outside another checkout containing the same catalog. Open a new Codex task after installation. Verify the installed version and endpoint `https://test-mcp.fotor.com/mcp`, then reuse valid MCP credentials or complete OAuth with `codex mcp login fotor` in an external browser when needed. The client keeps credentials outside the plugin package.

Ask "Connect to Fotor and introduce its main features" to follow the [website connection flow](plugins/codex/fotor/references/mcp-integration.md#connect-to-the-website): required external-browser OAuth, then the returned sign-in link in a visible in-app browser. If browser automation is unavailable, the flow explains the limitation and provides a manual link.

For an explicitly MCP-only, read-only connection check, discover the actual tools and call `list_models`. Use media/mode filters and a selected model ID to inspect supported parameters. Generation, task lookup, and website sign-in links are separate actions; a model query does not verify upstream media execution.

See the [plugin guide](plugins/codex/fotor/README.md) for connection behavior and the [Apache-2.0 license](LICENSE).

## Updates

```bash
codex plugin marketplace upgrade fotor-codex
codex plugin add fotor@fotor-codex
```

A branch-pinned installation keeps its selected branch. To change channels, explicitly replace the marketplace registration, reinstall, and verify the endpoint in a new task.
