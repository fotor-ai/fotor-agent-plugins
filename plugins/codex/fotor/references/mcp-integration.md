# Fotor MCP Integration

## Connection

This package selects the **test** environment at `https://test-mcp.fotor.com/mcp` using Streamable HTTP. Preserve this environment throughout a task and its assets.

This test build verifies installation, OAuth, ping, and calculator calls. Image and video operations are deferred until matching MCP tools are available.

Complete the service's OAuth flow through Codex when authentication is required. Discover the available tools and use their actual input schemas. Keep credentials in the client's supported credential store. An installed package alone does not establish a working connection or media capability.

## Runtime workflow

Use this workflow for both image and video requests:

1. Discover the Fotor tools available in the current session and inspect the relevant descriptions and input schemas. Use the configured environment and honor the user's environment choice. Keep a task and its assets in the same environment; do not automatically switch between test and production on failure.
2. If no suitable Fotor tool is available, explain the missing connection or unsupported operation. You may prepare a prompt or editing brief, clearly identifying it as preparation. Do not claim that media was generated, invent tool names or results, or silently substitute another provider.
3. Collect only missing inputs needed for the chosen operation. Upload or reference user-provided assets using the service's supported mechanism. A local file path is usable by a remote service only when its tool contract explicitly supports it.
4. Submit the operation using the actual schema. Retain returned asset and task identifiers. If submission times out and acceptance is uncertain, use any supported status or idempotency mechanism before retrying; when recovery is unavailable, report the uncertain state rather than creating a duplicate task.
5. For asynchronous jobs, use the returned status mechanism and polling guidance within the host's execution limits. Keep the existing task identifier throughout polling. On failure, cancellation, or a wait limit, report the observed state and identifier; do not treat a queued or running task as finished.
6. Return the real artifact or result URL after the service confirms completion. Prefer an available media preview. Summarize the requested operation and any material unmet requirement; distinguish a returned result from a result that was actually inspected.

