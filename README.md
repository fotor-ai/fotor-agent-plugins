# Fotor for Codex

Version **0.1.0-test.2**, configured for the **test** MCP service.

This test build verifies installation, OAuth, ping, and calculator calls. Image and video operations are deferred until matching MCP tools are available.

## Installation

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-codex
```

If this private repository requires Git authentication, configure a credential helper before installation. When replacing an existing `fotor-codex` registration, inspect its source and use the client's marketplace remove/add commands to select this source explicitly.

Open a new Codex task after installation. Verify the installed version and endpoint `https://test-mcp.fotor.com/mcp`, then complete OAuth with `codex mcp login fotor` if needed. The client keeps credentials outside the plugin package.

See the [plugin guide](plugins/codex/fotor/README.md) for connection behavior and the [Apache-2.0 license](LICENSE).

## Updates

```bash
codex plugin marketplace upgrade fotor-codex
codex plugin add fotor@fotor-codex
```

A branch-pinned installation keeps its selected branch. To change channels, explicitly replace the marketplace registration, reinstall, and verify the endpoint in a new task.
