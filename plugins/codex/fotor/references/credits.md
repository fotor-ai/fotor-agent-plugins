# Fotor Credits and Top-up

Read this workflow for a balance question or an explicit recharge request. Media submission and ordinary website connection do not require a preliminary balance query.

## Query the selected account

1. Follow [MCP authentication](mcp-integration.md#authenticate-mcp) for the installed environment, reusing valid credentials or completing required external-browser OAuth. Discover `get_credits`; if absent, report the unavailable capability without changing environments.
2. Call `get_credits` without arguments. It returns nonnegative integer `credits` and a one-time `top_up_url`. It also creates a login handoff, so it is not a side-effect-free connection probe even when the user only asks for the balance.
3. Report the actual balance. `credits_increment` in a task response is that task's consumption, not this balance; do not infer available funds from it. A balance observation is not a generation price quote or a guarantee that a later submission can proceed.
4. Treat `credits_unavailable`, authentication errors, invalid/missing balance values, and other failures as unavailable information, never as zero. Do not create accounts, keys, replacement tasks, or credentials to work around a balance error. A failed top-up handoff does not establish a balance when the tool returned no result.

## Select the page action

| Request and result | Action |
| --- | --- |
| Positive balance; no top-up request | Report only the balance. Keep the returned link out of the response. |
| Zero balance, or explicit request to top up | Use the returned top-up link in a visible built-in browser according to the shared link workflow below. |
| User explicitly forbids web actions | Report the balance without automatic navigation or link disclosure. Provide a manual link only if explicitly requested. |
| A required browser is unavailable | Explain the limitation and provide the complete, unused top-up URL for manual opening, unless the user prohibited that disclosure. |
| Query failed | Report the failure; do not select the zero-balance path. |

Before opening or exposing `top_up_url`, follow [one-time browser links](mcp-integration.md#one-time-browser-links), including endpoint/environment checks, immediate single consumption, and one cause-resolved replacement at most. This response has no `expires_in` or `target_url`: do not require website-response fields or invent a remaining lifetime. When manually delivering the link, use the exact returned URL and a label meaning "View Fotor plans" in the conversation language. Before reporting a successfully opened recharge page, verify that its final HTTPS origin matches the selected website; stop further interaction on a mismatch. The link is short-lived; if an attempted visit reports expiration or prior consumption, apply the bounded replacement rule. Re-querying obtains a new balance observation as well as a new ticket.

The top-up link is for recharge, not a replacement for `get_website_url` when connecting to Fotor. Opening the page completes only the requested entry-point action; leave purchase choices and payment to the user. Confirm actual website state separately, and do not claim a balance increase or payment completion from navigation alone. Keep tickets and credentials out of files and diagnostic logs.
