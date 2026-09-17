---
name: fotor-connect
description: Connect to the Fotor website, check only its MCP connection, introduce its features, or query models and parameters. Use for Fotor connection, website access, and capability requests; media creation belongs to fotor-image or fotor-video.
---

# Fotor Connection and Features

Read [connection routing and authentication](../../references/mcp-integration.md#route-the-request) before connecting. Preserve the installed MCP endpoint and keep credentials in the host's supported store.

## Choose the connection scope

- **Connect to Fotor / open the Fotor website:** Follow [Connect to the website](../../references/mcp-integration.md#connect-to-the-website). Reuse valid MCP credentials; when authorization is needed, use an external browser and wait for client-confirmed success before opening the returned website link in a visible in-app browser.
- **Connect to Fotor MCP / check MCP connectivity:** Authenticate if needed, discover the tools, and call `list_models` for a read-only check. Report the actual outcome and endpoint; finish without requesting a website link.
- **Introduce features / query models:** Answer the requested capability question without opening the website. Use the current tool descriptions for features; use model queries for model choices and parameters.

These intent rules also apply to equivalent wording in other languages. An explicit MCP-only or no-website instruction takes precedence over the ordinary website connection flow. Generation requests alone do not request website navigation.

## Explain features and models

For the starter prompt, "Connect to Fotor and introduce its main features.", complete the website connection flow and give a brief feature overview: image generation/editing, video generation from supported inputs, and querying submitted tasks, limited to tools actually available. Report MCP and website outcomes separately if either is incomplete. Describe supported capabilities without claiming successful media execution.

For a model question, use `media_type` and `mode` filters, then query the selected `model_id` for supported parameters and defaults. Show model lists when requested; an internal connection probe does not require displaying its catalog. An empty filtered catalog means no matching models, not a failed connection.
