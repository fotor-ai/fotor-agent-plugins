---
name: fotor-connect
description: Connect to Fotor MCP, check its authentication and available tools, or query supported models and parameters. Use for Fotor connection or capability questions; generation requests belong to fotor-image or fotor-video.
---

# Fotor Connection and Models

Read [MCP integration and runtime workflow](../../references/mcp-integration.md) for environment selection, authentication, and the current tool contract.

## Connect and check

Confirm the installed plugin's selected MCP endpoint and enabled state. Use the host's supported MCP discovery and OAuth flow when needed; let the user complete interactive authorization in their chosen browser. Keep credentials in the client's supported store and preserve the configured environment.

Once tools are available, call `list_models` without filters for a read-only connection check. Report the endpoint, authentication outcome, and actual query result. Installation or tool discovery alone does not prove that a call succeeded. If discovery or authentication fails, report that specific failure rather than trying another environment or claiming a connection.

## Query capabilities

Use `media_type` and `mode` filters for the user's operation, then query the selected `model_id` for details. Explain supported settings and defaults from the response; do not treat an empty filtered catalog as a connection error or hardcode a preferred model, model count, or tool count.

Keep connection checks to discovery and model queries. Do not submit media, query arbitrary task IDs, or create website handoff links as a health check. A successful model query verifies MCP access, not the upstream generation service or image/video quality.
