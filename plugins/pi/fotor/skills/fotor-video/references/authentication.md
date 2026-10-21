---
platform: pi
---

# Pi Authentication and Browser Guidance

Use this guide for Pi CLI connection, missing tools, OAuth continuation, and website access. Shared routing, deadlines, and link validation remain in [MCP integration](mcp-integration.md#runtime-workflow). Initial compatibility targets Pi 1.3.0; package installation, authorization, tool availability, and website login are separate outcomes.

## Package and native MCP setup

Install one Fotor Pi Package through `pi install <source>` and inspect it with `pi list`. The package exposes skills, including explicit `/skill:fotor-connect` invocation; it does not register an MCP server. Its `mcp.example.json` is reference configuration, not an automatically loaded resource. Use the installed package's README for installation and updates.

Inspect the effective user configuration and trusted-project overrides through Pi's native controls before registering `fotor`. Preserve an existing registration with different settings and ask the user which source they intend. Once the production configuration is confirmed, the native registration command is:

```bash
pi mcp add fotor --url https://mcp.fotor.com/mcp --exposure deferred --description "Connect to Fotor's production MCP service."
```

The example targets the user scope; `--local` selects the trusted project's scope when explicitly requested. Registration does not authorize the account. Test and stable plugin channels both use production services. Never add a static credential header or import another client's token.

## Native state and tool discovery

Use `/mcp` to inspect the actual `fotor` registration, connection status, and tools. Shell-level `pi mcp list` connects every enabled server; prefer the selected server in the interactive panel when other servers are configured. Distinguish a missing registration, disabled MCP, required sign-in, host restriction, and an unknown connection state.

The recommended `deferred` exposure uses Pi's native `tool_search`, followed by the actual discovered tool. Do not require a fixed tool inventory or infer connectivity from skill loading. When native MCP or tool search is disabled, explain the effective setting and use the user's chosen native control. An installed third-party extension owning `/mcp` replaces native session MCP even though shell `pi mcp` commands still use the built-in implementation; report that conflict rather than changing the user's extensions.

Pi does not provide default per-call permission enforcement. Follow the user's scope and any installed host restrictions; conversational confirmation is not a sandbox. For confirmed sign-in requirements, enter native OAuth. For transient or unknown status, use the shared [bounded recovery](mcp-integration.md#bounded-connection-recovery) before offering reauthorization or a later retry. Keep credentials intact and never read credential files, print tokens, or implement a custom MCP/OAuth client for diagnosis.

## OAuth and continuation

Use `/mcp` → Sign in, `/mcp login fotor`, or `pi mcp login fotor`. Pi opens the authorization page in the external browser; when automatic opening is unavailable, provide its native authorization link for the user to open in an external ordinary browser. Keep the native callback listener running. Authorization links and callbacks are sensitive; do not retain them in diagnostic records.

Start with Pi's native dynamic client registration (DCR). If it is specifically incompatible, explain the failure and use Pi's supported CIMD configuration only after the user chooses that alternative. Network errors are not evidence that another registration method is required. Keep Pi's own client identity and issuer validation. Never impersonate another client or silently replace an existing authorization.

After opening the authorization page, ask for a conversational choice localized to the user:

- **Authorized; continue to Fotor:** verify native client-confirmed success before querying tools or requesting a website link.
- **Still signing in; keep waiting:** retain the pending native login while it remains valid; human waiting does not consume the shared automatic recovery budget.
- **Cancel:** stop the current connection attempt and cancel only its pending native login.

A skill cannot create an extension-only selection widget. Text choices are sufficient. A user's success report does not replace the client's callback/token-exchange confirmation. If the native login expires, explain the outcome and obtain a new user decision before restarting authorization. The CLI's own login timeout still applies.

## Website browser and completion

Pi CLI uses an external ordinary browser for the Fotor website. Prepare the intended browser/profile before calling `get_website_url`. Pi's OAuth opener is not a browser-automation tool available to the model. Use an available, permitted native OS opener only when it can reliably reach the intended profile; do not install a browser extension or runtime helper as a prerequisite.

Apply the shared production handoff validation, then pass the unmodified link directly to the chosen browser once. Do not fetch, preview, rewrite, or repeatedly open it. If automatic opening is unavailable or the profile cannot be targeted, immediately provide the complete unconsumed link in the visible Pi conversation and explicitly instruct the user to open it in the intended external ordinary browser. Record that path as manual completion. During acceptance, use separate ordinary OAuth and website profiles, never incognito/private mode.

An opener exit code confirms only that opening was requested. Confirm the final production website and signed-in state through available browser evidence or the user's explicit report; otherwise report website login as pending. A successful connection reply introduces the available main features without automatically listing models or checking credits. MCP-only requests finish after the read-only MCP check, without creating a website link. Preserve the shared single-replacement limit for expired/used links.

## Sessions and credentials

Pi owns Fotor credentials in its agent directory. Native identity is keyed by server name plus URL; changing project scope alone does not isolate credentials. A dedicated `PI_CODING_AGENT_DIR` and unrelated working directory are required for isolated acceptance, with effective storage boundaries checked first. Model-provider authorization is separate from Fotor MCP authorization; the user completes each through native controls.

Keep the same agent directory, server name, URL, and package source for restart-reuse checks. Do not reauthorize to make reuse appear successful. `pi mcp remove fotor` removes configuration but retains stored authorization; `pi mcp logout fotor` deletes that authorization. Only clean a task-owned acceptance store after its checks, never daily credentials. Package removal and channel changes do not require logout. Immediate restart reuse does not establish expiry refresh.
