---
platform: claude-code
---

# Claude Code Native Authentication and Browsers

The package declares its Fotor service in `.mcp.json`. Check its endpoint and native host overrides before connecting.

Read this reference in Claude Code when connecting to Fotor, recovering missing tools, or opening its website. Use [MCP integration](mcp-integration.md) for the shared service, routing, OAuth continuation, and retry rules. This workflow requires no bundled runtime scripts.

## Native status and permissions

1. Check the installed plugin's enabled state and production endpoint. Its identity is `fotor@fotor-ai`; its bundled server normally appears as `plugin:fotor:fotor`. Select the actual plugin server shown by `/mcp`. A separate server called `fotor` is not evidence that the bundled server works.
2. Use the interactive `/mcp` panel for native connection status, Authenticate, permission requests, and available reconnect controls. When the installed CLI supports them, `claude mcp get plugin:fotor:fotor` and `claude mcp login plugin:fotor:fotor` are optional native routes. A shell command observes its own process, not necessarily the active conversation's live connection. A session-only `--plugin-dir` load may not be visible to a separate CLI process; use `/mcp` in that session instead of creating another server.
3. Respect workspace trust, plugin enablement, managed policy, and tool permission decisions. If execution is restricted, use the host's permission UI or ask the user to use `/mcp`; do not change broad permission settings. A denied permission is not missing OAuth authorization and is not a transient network retry.
4. Classify evidence before choosing recovery:

| Native evidence | Action |
| --- | --- |
| Explicit Authenticate / missing or invalid authorization | Start native OAuth for this plugin server in an external browser. |
| Connected or usable cached authorization | Discover tools and make the requested read-only check; cached state alone does not prove a successful call. |
| Plugin disabled, untrusted, or blocked by policy | Explain that specific host requirement and use its controls. |
| Timeout, metadata request failure, or otherwise unknown | Apply the shared bounded recovery policy; keep existing credentials. |
| Connected server but current session has no usable tools | Use available native reconnect/reload, then a new session if necessary; report the actual result. |

After bounded diagnosis still leaves authorization unknown, offer localized choices: **Reauthorize the Fotor connection** or **Try again later**. Only explicit reauthorization starts OAuth in this state. Neither choice clears stored credentials. Do not use logout, server removal, marketplace removal, or another client's tokens as recovery.

Plugin tool names include both plugin and server namespaces, such as `mcp__plugin_fotor_fotor__list_models`. Discover the actual callable tools rather than assuming bare server names or a fixed inventory. Authentication success in another process does not load tools into the current session.

## External OAuth and continuation

Use Claude's native OAuth mechanism. External login and consent remain user actions. When the browser cannot be launched, give the native authorization link for external use and keep the callback waiting. Follow [OAuth completion choice](mcp-integration.md#oauth-completion-choice): ask Continue / Wait / Cancel, then require both explicit Continue and client-confirmed success. Never request a token or callback URL in chat. If the native CLI itself requests a redirect in a headless flow, the user enters it directly into that native prompt.

For website continuation, use **Signed in and authorized; open Fotor in the external browser** in Claude Code CLI, or **Signed in and authorized; open Fotor in the built-in browser** in Claude Code Desktop. For MCP-only work, use **Signed in and authorized; continue the MCP check**. If a structured choice tool is unavailable, present the same choices in text and wait. Reusing valid credentials does not require another authorization prompt.

## Visible website navigation

Identify whether the current host is Claude Code CLI or Claude Code Desktop before requesting `get_website_url`. Use host/session evidence; if the host is unclear, ask the user rather than inferring it from a missing browser tool. Select the website browser by host, not by whichever browser happens to be available:

| Host | MCP OAuth | Website handoff |
| --- | --- | --- |
| Claude Code CLI | External browser through native OAuth | External browser such as Chrome; the CLI has no built-in browser |
| Claude Code Desktop | External browser through native OAuth | Claude Code Desktop's built-in browser |

In the CLI, use an already available, permitted native URL-opening control to open the validated handoff in the external browser once. If there is no suitable control, immediately provide the complete unused link and explicitly ask the user to copy it into an external browser such as Chrome on their machine. A browser extension is not required; do not install or enable one. Do not imply that a separate Codex conversation supplies Claude CLI with an in-app browser.

In Desktop, after native authorization is confirmed and the user chooses Continue, open the validated link once in its built-in browser using the host's permitted browser control. Select the built-in browser explicitly rather than relying on a global link-opening preference. If this browser/control is unavailable, explain that limitation and provide the unused link for the user to open manually in Desktop's built-in browser; keep website completion pending. Do not silently substitute an external browser for the website step. Desktop's behavior is specified here, but its actual host acceptance remains separate from CLI acceptance.

Both routes use one final visible visit. The website request authorizes this user-facing navigation, not a background fetch, preview, or link check. Follow the shared endpoint and final-page checks. Do not change browser preferences or bypass permissions to complete the visit. Report website success only when signed-in state is observed or explicitly confirmed by the user, distinguishing user confirmation from automated inspection. A successful browser launch alone proves neither navigation completion nor login.

Until that evidence is available, report **MCP connected; website login awaiting user completion or verification**. Keep the complete link unconsumed for manual delivery, state which browser to use, and preserve the shared single replacement limit. Stop after a repeated failure; do not automatically retry a potentially consumed link.

## Installation and session boundaries

The CLI acceptance baseline is 2.1.296. `claude --plugin-dir <package-directory>` is a session-only source load. Marketplace installation uses `claude plugin install fotor@fotor-ai`; refresh with `claude plugin marketplace update fotor-ai`, then `claude plugin update fotor@fotor-ai`, and start a new session. Local-directory sources read files in place and do not prove release-cache updates.

Do not treat a successful manifest check, an empty content scan, or a fresh login as proof of skill execution, tool loading, or website login. Report those outcomes separately.
