---
platform: dsh
---

# DeepSeek Harness authentication and browser access

DeepSeek Harness is abbreviated as `dsh` in commands and native identifiers.

## Ownership and baseline

This Bundle targets Linux DeepSeek Harness Web `0.2.1-rc.2`, with separately installed `mcp-remote 0.15.1` and Node `24.17.0`. DeepSeek Harness owns the stdio client and tool permissions; the external bridge owns OAuth and its credential store. Do not describe this as native remote OAuth in DeepSeek Harness. Desktop, headless and ACP have separate acceptance scopes.

Use this guide with the [shared runtime workflow](mcp-integration.md#runtime-workflow). Installation, authorization, actual calls, website access, restart reuse and reauthorization recovery are distinct outcomes. Do not infer full support from one successful call.

## Inspect and configure

Installation declares two disabled components. It does not install mcp-remote, register a working connection, scan unconfigured skill roots or start OAuth. For first installation, a missing bridge, unconfigured paths or missing skills, open the installed package's README and follow its `INSTALL.md` guide before continuing. That guide contains the external bridge installation recipe and matching JSON examples; no internal development document is required. The user or an Agent acting on their installation request runs those steps separately.

Copy `configuration.example.yml` outside the package. Inspect the effective profile, home patch and launch overlays before changing them. Stop on a conflicting `fotor` service or duplicate skill provider; preserve the existing configuration.

Fill every path with its actual absolute location. Only the installed Fotor skill directory belongs in `customSkillDirs`; retain `includeDefaultRoots: false`. Use the independent Fotor provider name. Configure the external Node executable, bridge proxy, working directory and Fotor-only `MCP_REMOTE_CONFIG_DIR`. Keep the bridge store separate from model-provider credentials and other clients. Preserve it across package updates. Overrides replace the entire `config` object, including security and reconnect settings; they do not deep-merge individual fields.

Enable only the skill provider first. Use `/fotor-connect` when exposed by the native skill menu, or the native `skill` tool for the actual discovered Fotor connection entry. Confirm its installed resource path. Missing skills, a missing executable, a disabled component, permission denial, unauthenticated MCP and unknown state require different responses; none justifies clearing credentials.

Use DeepSeek Harness's native `read` tool for the known package references. Its Linux shell sandbox can hide other host files under a private temporary-directory mount; shell `ls` failure is not proof that native file reading will fail. If the native reader cannot access this package, report that limit and stop. Do not scan unrelated installations, use another platform's guide, or widen standing sandbox permissions to work around the lookup.

## First authorization

Install and verify the pinned bridge outside this package with a dependency lock and package integrity. Never download or upgrade it during connection with an unpinned `npx` command. Keep the stdio proxy stopped while the standalone authorization client runs.

Confirm the actual opener in this launch environment uses the user's daily ordinary browser. A task-specific desktop routing override may be needed; do not change the system default, create an incognito or headless session, or clear browser data. The model-provider login is independent of Fotor OAuth.

Run the separately installed client's native entrypoint, using the same endpoint, resource, callback port/host and credential directory configured for the proxy:

Use a native interactive terminal, or a host execution facility that keeps the client's input channel open until discovery completes. Closing stdin can make the bridge exit successfully before its tools response arrives; require actual discovery and completion rather than exit code alone.

```bash
MCP_REMOTE_CONFIG_DIR=/absolute/path/to/dsh-home/mcp-auth/fotor /absolute/path/to/node /absolute/path/to/bridge/node_modules/mcp-remote/dist/client.js https://mcp.fotor.com/mcp <callback-port> --host <callback-host> --transport http-only --resource https://mcp.fotor.com/mcp --auth-timeout 600 --connect-timeout 20 --headers-timeout 20 --body-timeout 20
```

The user completes account login, consent and Continue / Wait / Cancel in conversation. After Continue, verify the actual client connected, discovered tools and finished successfully; a browser success page alone is insufficient. Limit human OAuth waiting to ten minutes and stop this client's callback listener on cancellation or timeout. Do not assume a command-line timeout enforces every callback wait.

After the standalone client exits and its callback listener closes, enable the configured MCP component and restart the selected DeepSeek Harness profile. Discover actual tools and execute a read-only `list_models` check. Do not expose the whole model catalog, query credits or request a website link for an MCP-only task.

## Website access

Prepare the user's daily ordinary browser before requesting a short-lived link. Preserve its existing Fotor login state; do not sign out, replace its profile or clear data to manufacture a fresh-login test. Confirm the intended browser/profile can be targeted before calling `get_website_url`.

Follow [one-time browser link rules](mcp-integration.md#one-time-browser-links): validate the production handoff endpoint and returned expiry, then open the exact original link once. A missing `target_url` must be reported truthfully, not invented. Do not prefetch, preview, rewrite or repeat a consumed link.

Use an available host opener and the exact confirmed daily browser profile. If the DeepSeek Harness shell sandbox prevents desktop access, request native approval for that single command with a clear purpose; do not change the session's standing permissions. A background browser job may need a result check for that same job, which must not launch the link again. Do not add browser diagnostics or a temporary web proxy.

If automatic opening is unavailable, provide the unconsumed original link in the native conversation for the user to open once in that browser. Record this as manual opening. Wait for confirmation of the final production website and login state. A successful command or a pre-existing signed-in page does not prove a signed-out-to-signed-in transition. Report MCP and website results separately, then introduce the main capabilities.

## Bounded recovery and controlled reauthorization

Use the shared two-retry limit, waits of two/five seconds and ninety-second automatic-stage budget. Human login time is excluded. Include the bridge's own actions in the record; do not add a second retry or OAuth loop. Disabling DeepSeek Harness reconnect does not guarantee the bridge will never open a new authorization page internally.

On an authorization failure, cancellation, permission denial or unknown state, stop the current call. Do not treat a login-page CORS/preflight rejection as the same failure as missing OAuth discovery state. A missing-discovery-state message does not prove that the issuer actually changed.

For user-requested reauthorization, use this single sequence:

1. Stop the failed operation, this profile's proxy and any leftover callback listener. Do not start a competing authorization flow.
2. Preserve the same credential directory and connection/security arguments. Do not read, copy or edit tokens.
3. Run the standalone authorization client above. Complete the new native consent flow only when explicitly requested, with the same Continue / Wait / Cancel handling and native-result verification.
4. Stop the standalone client, restart the configured DeepSeek Harness MCP and prove recovery with an actual read-only query. A successful consent page without that query is not recovery.

A cached client connection without a new consent flow is reuse, not invalid-grant recovery. Never clear a global authorization directory, substitute a new empty store, weaken issuer/resource/PKCE/TLS checks or sign out the daily website to force a test. Exercise recovery only after a real failure or an available native operation that can be limited to this exact temporary grant. If no safe trigger exists, report recovery as unverified rather than claiming complete acceptance. Expiry-driven refresh likewise requires actual evidence.

## Updating and removing

Use the native Bundle commands in the package README. After updating, inspect the resolved source, version, file contents and actual skill directory; package-manager links outside the package are distinct from forbidden symlinks inside its resources. Keep only one enabled Fotor skill source. Preserve the external bridge store when replacing a package.

Disable only this Bundle's two components and remove its local overrides before native removal. Cleanup of an isolated acceptance environment may delete that environment's local store after all related processes stop; that does not revoke server-side consent. Leave daily installations, credentials and browser login unchanged. Keep package, model-session, bridge, website, reuse and recovery evidence separate.
