---
platform: cursor
---

# Cursor and Grok Bot Authentication and Browsers

Read this reference in Cursor IDE, Cursor CLI, or Grok Bot when connecting to Fotor, recovering missing tools, or opening its website. Grok Bot uses the same Cursor package and `fotor-ai` marketplace. Follow [MCP integration](mcp-integration.md) for service selection, OAuth continuation, bounded recovery, and one-time links.

## Select the native client

1. Confirm the installed `fotor` plugin is enabled, its `mcp.json` selects production, and no host override changes the service. The marketplace is `fotor-ai`; use the actual server identifier exposed by the current host. In Grok Bot, inspect its installed Fotor connector and host-exposed configuration rather than assuming access to the local Cursor package cache. A separately configured server is not evidence that the plugin's bundled MCP works.
2. In the IDE, use **Customize** and its MCP connection controls to inspect the plugin, request tool permissions, and authenticate. In the CLI, use `/mcp` in the active conversation. When supported, `agent mcp list`, `agent mcp list-tools <identifier>`, and `agent mcp login <identifier>` are optional native controls. Keep the same plugin-loading options and configuration context when using a separate process. A `--plugin-dir` session may expose a server that another process cannot see.
3. In Grok Bot, use **Connect Apps**, open the installed **Fotor** entry from `fotor-ai`, and inspect its account, connection status and enabled tools. Use its native **Authorize / Authenticate** action or connection card when authorization is required. The IDE and CLI controls in step 2 are for those hosts; do not substitute a locally launched `agent` process for Grok Bot's connection.
4. Respect workspace trust, plugin enablement, managed policy, and tool permissions. Ask the user to resolve a denied permission through the current host's controls. Keep permission denial separate from missing authorization; neither an empty tool list nor a network error proves logout.
5. Reuse valid host-managed Fotor credentials. Explicit missing/invalid authorization enters native OAuth. A successful authorization with unavailable tools calls for recovery in the active client. An unknown state follows the shared recovery budget; after exhaustion, offer **Reauthorize Fotor** or **Try later**. Start OAuth only if the user chooses reauthorization. Preserve credentials on network errors.

Cursor account login enables the agent product; Fotor MCP OAuth authorizes the Fotor service. Keep these two sign-ins separate. Use native status evidence for each and never extract or copy tokens from Cursor, Codex, or Claude stores. `agent logout` signs out of Cursor and is not a Fotor recovery action. Disabling a server is not credential revocation.

Grok Bot's account-installed Fotor may appear before it is authorized. Treat **Installed**, **Connected**, enabled tool listings, and successful tool calls as separate evidence. Grok Bot manages connector authorization on the service backend; a local Cursor credential or browser login does not establish Grok Bot authorization. Account-level connection changes may affect other clients or Bots, so preserve existing connections during diagnosis.

Keep diagnosis inside those native controls. Do not open `mcp-auth.json`, credential files, or OS credential stores with file, shell, or interpreter tools, including when checking whether credentials exist. A client reporting `needsAuth` after a restart is an unresolved reuse result; reading stored tokens cannot establish that the service accepts them. Report the native state and follow the permitted authentication or recovery route. On the validated CLI baseline, the dynamic `mcp_auth` tool rejects interactive authentication as IDE-only; use the active CLI's `/mcp` controls instead of calling that tool as a probe.

## External OAuth and continuation

Use the selected server's native authentication flow in an external normal browser. Let Grok Bot's connection card manage its own callback; no local callback listener is required from the plugin. Keep the native flow active and follow the shared [OAuth completion choice](mcp-integration.md#oauth-completion-choice). For an IDE website request, label Continue as continuing in Cursor's built-in browser; for CLI, say external browser; for Grok Bot, name the visible browser actually selected below. Use a structured choice when available, or a short explicit text choice in the native conversation.

Proceed only after both the user's Continue choice and client-confirmed authorization success. Wait or stop when selected; account login, a launched page, or user confirmation alone does not prove the MCP callback completed. Rediscover tools in the active session and perform the permitted read-only check before requesting a website link. MCP-only requests finish without website or balance actions.

## Visible website navigation

Prepare the browser route before calling `get_website_url`:

| Host | Website route | Manual fallback |
| --- | --- | --- |
| Cursor IDE | Navigate once in Cursor's visible built-in browser when available and permitted | Provide the unused link and tell the user to open it in that built-in browser |
| Cursor CLI | Open once in an external browser through available, permitted native host capabilities | Provide the complete unused link and tell the user to open it in an external browser such as Chrome |
| Grok Bot | Use Grok Bot's available, permitted visible browser, normally on its hosted computer, after confirming the intended session | Explain the unavailable route and provide the unused link for immediate user opening in an explicitly identified browser; record manual completion separately |

Honor the user's selected browser/profile and actual host permission controls. If an opener cannot select the intended profile, explain the limitation before requesting a link and use the manual route. Opening a default browser first can consume the ticket in the wrong session. No bundled helper, interpreter program, hidden browser, or HTTP probe is needed.

Validate the exact production handoff endpoint and returned lifetime through the shared workflow. After one visible navigation, inspect the final production website and signed-in state; in an external browser without observation tools, ask the user to confirm the result. A successful launch does not prove website login. Until confirmed, report **MCP connected; website login awaiting completion or verification**. Keep automatic navigation and user-confirmed manual completion distinct. Use only the shared single replacement allowance after resolving an expired or consumed link's cause.

Grok Bot's computer browser and the user's local browser are separate sessions. Discover the available browser capability in the active Bot rather than assuming a Cursor IDE browser namespace or a local Chrome executable. A user-confirmed login in a local fallback browser does not establish that the Bot's computer is signed in. Website instructions describe the intended route; plugin connection and image generation alone do not validate it.

## Loading and session boundaries

For local IDE development, copy the complete package into `~/.cursor/plugins/local/fotor/`, then reload the window and inspect Customize. Local imports must be allowed; an installed marketplace plugin with the same name takes precedence. Links outside the local plugin directory are not a supported substitute for copying the package.

For CLI development, use `agent --plugin-dir <package-directory>` and inspect the resulting native session. After replacing local files, start a new CLI session or reload the IDE, then verify the source and version. Local replacement does not establish marketplace refresh or cache-upgrade behavior. Use the current host's marketplace controls for marketplace installs, and verify their actual source before updating.

For Grok Bot, install Fotor through the Cursor account's marketplace controls, sign in with that same account, then verify Fotor in **Connect Apps**. Local IDE folders and CLI `--plugin-dir` loads do not prove account-level distribution. If Fotor is absent, inspect the account, installation scope and applicable team policy through native controls; report the missing installation instead of adding a duplicate MCP server. After an account-installed update, check the actual Grok Bot version/source information and available skills in a new conversation; neither a local file replacement nor a shared version label proves that the Bot loaded new files.

Validate skills against the package's actual inventory. Installation, native authorization, tool calls, model-driven skill behavior, and website login are separate results. Cursor Cloud Agent behavior requires separate acceptance.

## Grok Bot files and attachments

Use media attachments whose bytes the active Bot can read, files available in its workspace, or service-accessible HTTPS references. A path on the user's computer is not automatically available on the Bot's hosted computer. If only an attachment name or inaccessible path is available, ask for an accessible attachment or URL before following the shared [upload workflow](media-upload.md). Host-native file access and transfer capabilities determine what can be uploaded; no bundled scripts or local interpreter setup are required.
