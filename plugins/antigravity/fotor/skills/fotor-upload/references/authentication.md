---
platform: antigravity
---

# Fotor in Google Antigravity

## Find the active skill and server

Use the intended project in the CLI or App. In the CLI, `/plugin` → **Installed**
and `agy plugin list --json` show the enabled package, its version, path,
skills and bundled MCP server. In the App, open **Customizations → Installed**.
A CLI install from a local directory stages the package for the current user.
Resolve a same-name plugin, workspace skill or standalone MCP conflict with
the user before making changes.

In the CLI, type `/fotor` and select `/fotor:fotor-connect` from the skill picker. In the App, describe a Fotor task in the intended project.
Use the host's file-reading tool at the active skill's own path. Resolve its
four references under `references/` there. The package above `skills/` has
`plugin.json` for version checks and `mcp_config.json` for the endpoint.

## Native sign-in and tools

The bundled server key is `fotor`. Antigravity documents the qualified server
name `fotor_fotor`. Check the actual session in `/mcp` or the App's installed
MCP servers. Match the endpoint to `https://mcp.fotor.com/mcp`.
Discover the current tools and schemas, including lazy tools offered through
`call_mcp_tool`. Honor the user's server and tool permissions.

Reuse an active Fotor grant. When the host requests sign-in, get the user's
consent before starting one native flow. In the CLI, expand **fotor** in
`/plugin` → **Installed** and select **Sign in**. In the App, open
**Customizations → Installed → Fotor → Accounts → Sign in**. Check that the
account lists `mcp.fotor.com`. The user completes Fotor consent in their browser and enters a code
in the native client if requested. Keep codes and credentials out of chat,
files and tool arguments. In the App, confirm **CONNECTED** on **Accounts**,
then find `list_models` under **MCP**. In the CLI, verify tool discovery
for the authorized service. Record whether the other client actually reuses the grant.

For an explicit image-model query use `list_models` with `media_type=image`.
Tool discovery alone cannot establish that a query succeeded. If a result is
uncertain, retain the observed response or task ID and follow the bounded
recovery rules in [MCP integration](mcp-integration.md).

## Approvals and browser use

Keep the user's permission policy in effect. Apply the generation skill's
three choices before raising quality. Use text and wait for a new user reply
when the host cannot make “Do not generate” the sole default or cannot
distinguish an explicit selection from a timeout. Host permission to run a
tool is separate from the user's choice of paid quality.

A website request needs a user-visible destination. In CLI confirm the user's
external browser. In the App confirm the intended browser surface and the
existing page and login state first. If the requested route is unavailable,
stop before requesting a handoff link. Open the short-lived URL once in the
confirmed browser and ask the user to confirm the final Fotor page. A successful
MCP grant verifies service access only.

Google documents a default of 300 seconds to connect and list tools and 180
seconds per tool call. Preserve explicit host settings. If a submission may
have been accepted, keep the known result and avoid a second submission.
