---
name: fotor-connect
description: Connect to the Fotor website, check only its MCP connection, introduce its features, or query models and parameters. Use for Fotor connection, website access, and capability requests; media creation/processing belongs to fotor-image or fotor-video and standalone uploads belong to fotor-upload and balance/top-up requests belong to fotor-credits.
---

# Fotor Connection and Features

Read [connection routing and authentication](../../references/mcp-integration.md#route-the-request) before connecting. For transient connection, discovery, or read-only probe failures, follow [bounded connection recovery](../../references/mcp-integration.md#bounded-connection-recovery) before reporting failure. Preserve the installed MCP endpoint and keep credentials in the host's supported store.

## Missing tools or authentication failures

Check native authorization before suggesting a reload or new chat. In Codex, read [native authorization diagnosis](../../references/codex-authentication.md) for host-native status, permission recovery, and login routes. If execution restrictions prevent a native check, use the host permission mechanism or connection UI before repeating it; skills require no script interpreter. A confirmed login requirement enters external OAuth even when every Fotor tool is absent. Unknown status requires bounded diagnosis and an explicit reauthorization choice; it is not proof of logout. Other hosts use their own native authentication controls.

## Choose the connection scope

- **Connect to Fotor / open the Fotor website:** Follow [Connect to the website](../../references/mcp-integration.md#connect-to-the-website). Reuse valid MCP credentials; when authorization is needed, use an external browser, collect the [OAuth completion choice](../../references/mcp-integration.md#oauth-completion-choice), and verify client-confirmed success before calling `get_website_url`, validating the handoff endpoint, and consuming the website link once in a visible in-app browser. The response has no `target_url`; verify the final website environment and login state after navigation. Website login does not depend on credit balance.
- **Connect to Fotor MCP / check MCP connectivity:** Authenticate if needed, discover the tools, and call `list_models` when available for a read-only check. If absent, report the unavailable probe without treating it as an authentication failure. Report the actual outcome and endpoint; finish without requesting a website link.
- **Introduce features / query models:** Answer the requested capability question without opening the website. Use the current tool descriptions for features; use model queries for model choices and parameters.

These intent rules also apply to equivalent wording in other languages. An explicit MCP-only or no-website instruction takes precedence over the ordinary website connection flow. Upload, image-processing, and generation requests alone do not request website navigation.

## Explain features and models

For the starter prompt, "Connect to Fotor and introduce its main features.", complete the website connection flow and give a brief feature overview: local image/video/audio uploads, image generation/editing, standalone image upscaling and background removal, video generation from supported inputs, querying submitted tasks, and credit balance/top-up access, limited to tools actually available. Mentioning credits does not call `get_credits`. Report MCP and website outcomes separately if either is incomplete. Describe supported capabilities without claiming successful media execution.

For a model question, use `media_type` and `mode` filters, then query the selected `model_id` for supported parameters and defaults. Show model lists when requested; an internal connection probe does not require displaying its catalog. An empty filtered catalog means no matching models, not a failed connection.
