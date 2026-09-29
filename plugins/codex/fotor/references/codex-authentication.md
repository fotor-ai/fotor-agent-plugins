# Codex Authorization Diagnosis

Use this Codex-only workflow when Fotor tools are absent or authentication prevents a call. Other hosts use their own connection and authorization controls. Skills contain instructions and declarative resources; they do not ship or generate diagnostic scripts, require an interpreter, or install a runtime.

## Inspect before choosing a recovery

1. Confirm the installed package, enabled state, and selected MCP endpoint. Prefer current Fotor-specific status or errors exposed by the owning host. A fresh `Auth required` or explicit not-logged-in result is enough to select authorization even when every remote tool is absent. Missing tools alone do not establish logout.
2. If more evidence is needed, use the host's native connection/status interface. Use a native CLI only when it is already available and offers a credential-safe status view. Keep the original workspace, configuration, credential-store policy, and endpoint. Inspect only necessary Fotor state; raw `mcp get/list --json` inventories may contain request headers, environment values, or unrelated service configuration. If safe inspection is unavailable, use the connection UI instead of dumping inventories or writing a filtering script.
3. Before retrying an inconclusive check, determine whether the host reports a restriction that prevents that check from accessing the network or native credential store. Follow [execution permissions](#execution-permissions) for an explicit permission error or a host-declared restriction affecting the operation. Otherwise, retry eligible transient failures within the shared [automatic-work budget](mcp-integration.md#bounded-connection-recovery), then use the unknown-state choice below.

| Native evidence | Required action |
| --- | --- |
| `Auth required`, `not_logged_in`, or equivalent confirmed login requirement | Start supported native OAuth for the selected Fotor environment. |
| OAuth/bearer credentials reported present | Verify tools and a read-only call in this chat. Local presence does not establish provider acceptance; a fresh host/service authentication rejection takes precedence. |
| Execution permission denied, or required network/credential-store access unavailable in the current tool context | Follow execution permissions; this is not a login result or a transient retry. |
| `unknown`, an unrecognized status, or a timeout without evidence of execution restrictions | Keep authorization inconclusive; use bounded diagnosis and the explicit choice below. |
| Disabled server or a different/ambiguous endpoint | Resolve enablement or configuration through host controls before login; preserve the selected environment. |
| No supported safe status interface | Explain the limitation and use the host's Fotor connection UI. If the login requirement still cannot be established, offer the explicit unknown-state choice. |

## Execution permissions

A native command started inside a restricted execution tool can return `unknown` even when the desktop client can correctly identify a login requirement. Repeating it under unchanged restrictions does not test recovery. Use the host's actual permission result or policy; an inherited environment marker alone is not proof of current access.

- Prefer an available status action in the owning host. If a necessary, credential-safe native operation needs additional access and the execution tool supports permission requests, explain the blocked operation and request only that operation through its normal approval mechanism. For example, a host exposing `exec_command` may support `sandbox_permissions="require_escalated"` with a justification; use it only when the current tool schema and permission policy allow it.
- After approval, rerun the same operation once with the same endpoint, workspace, and native credential store. Count the rerun within the existing retry limit and automatic-work budget; exclude time awaiting a human permission decision. Preserve spent time and attempts. A permission grant is not proof of authorization; classify the actual result before starting OAuth.
- If approval is denied, unavailable, or the approved operation is still restricted, stop that command path and provide the host's Fotor connection UI as the manual route. Do not repeatedly request the same permission, remove sandbox flags, change credential storage, or create another client. A permission-review timeout is not approval; follow only the host's explicit retry allowance.
- Once execution is usable, a still-unknown authorization result follows the choice below. Successful diagnosis establishes neither current-chat tools nor website login.

## Unknown authorization choice

When permitted diagnosis remains inconclusive, explain that login necessity could not be established. Use localized structured choices, or explicit text replies if unavailable:

- **Reauthorize and connect:** The explicit choice requests native reauthorization; retain existing credentials until the host replaces them through its supported flow.
- **Try later:** Stop this connection attempt and preserve configuration and credentials.

A preselection or no answer does not request reauthorization. This is separate from the [OAuth completion choice](mcp-integration.md#oauth-completion-choice) after the external login page opens. Human permission, login, and choice waits do not consume the automatic-work budget; answers do not reset retries or spent time.

## Authorize, then verify this chat

Prefer the owning host's supported login action. When the native Codex CLI is already available, its single command can initiate login without loaded Fotor tools:

```console
codex mcp login fotor
```

Keep the installed environment and native credential store fixed. Use the host permission mechanism if required. With no CLI, use the native connection UI; do not install an interpreter or generate a replacement client. Follow [external-browser authorization and its completion choice](mcp-integration.md#authenticate-mcp), keep the callback active, and require both explicit continuation and client-confirmed success.

After OAuth, rediscover tools in the original chat and use its available `list_models` probe. Use supported refresh/reconnect in that owning host when needed. A standalone CLI login saves authorization but does not promise a desktop-runtime refresh. If the host has no recovery capability, report **authorization completed; chat tools still unavailable** and provide the reload/new-session next step. Proceed to the website workflow only after current-chat readiness checks pass.
