# Fotor for Codex

<img src="assets/logo.svg" alt="Fotor logo" width="96" />

| Metadata | Value |
| --- | --- |
| Plugin | `fotor` |
| Version | `0.1.0-test.3` |
| Environment | `test` |
| MCP endpoint | `https://test-mcp.fotor.com/mcp` |
| Publisher | Fotor |
| Source | [GitHub](https://github.com/fotor-ai/fotor-agent-plugins) |
| License | [Apache-2.0](LICENSE) |

The test service has passed Codex OAuth, tool discovery, and read-only model queries. Media submission and task-query tools are exposed; actual image/video execution has not been accepted. Installing this build does not establish host acceptance.

## Install and connect

```bash
codex plugin marketplace add https://github.com/fotor-ai/fotor-agent-plugins.git --ref release/test
codex plugin add fotor@fotor-codex
codex mcp login fotor
```

For an existing marketplace registration, explicitly select the intended source using the client's remove/add commands. Open a new task after installation, discover the actual tools, and verify the selected MCP endpoint. OAuth credentials are stored by the client; website browser login is a separate session.

The package bundles connection/model discovery, image generation/editing, and video generation workflows. Available operations and inputs are determined by the service's actual tool schemas. Verify MCP access with `list_models`, then inspect model details before any generation. Media execution and website handoff require their own requested operation. Read [MCP integration and runtime workflow](references/mcp-integration.md) before using Fotor tools. Skill names and counts may evolve independently of this distribution layout.

The installation package contains its manifest, MCP declaration, license, resources, and workflow instructions. Installation requires no build or source synchronization step.
