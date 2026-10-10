# Fotor Credits and Top-up

Read this workflow for a balance question or an explicit recharge request. Media submission and ordinary website connection do not require a preliminary balance query.

## Query the selected account

1. Follow [MCP authentication](mcp-integration.md#authenticate-mcp) for the installed environment, reusing valid credentials or completing required external-browser OAuth. Discover `get_credits`; if absent, report the unavailable capability without changing environments.
2. Call `get_credits` without arguments. It returns nonnegative numeric `credits` and a regular pricing-page `top_up_url`. This URL reuses an existing browser session but does not sign the user in. It is not a one-time ticket and has no `expires_in` field.
3. Report the actual balance. `credits_increment` in a task response is that task's consumption, not this balance; do not infer available funds from it. A balance observation is not a generation price quote or a guarantee that a later submission can proceed.
4. Treat `credits_unavailable`, authentication errors, invalid/missing balance values, and other failures as unavailable information, never as zero. Do not create accounts, keys, replacement tasks, or credentials to work around a balance error. If a successful response has a valid balance but an unusable pricing link, report the balance and, when a link is selected below, the separate link problem.

## Select the response

| Request and result | Action |
| --- | --- |
| Balance below 100, or an explicit request to top up | Report the balance and deliver the validated pricing link as described below. Let the user click it. |
| Balance of 100 or more; no top-up request | Report only the balance. Keep the returned link out of the response. |
| Explicit balance-only or no-link instruction | Report only the balance, including when it is below 100. |
| Browser navigation is forbidden or unavailable | Deliver the selected link if the user permits links; link delivery needs no browser automation. |
| Query failed | Report the failure; do not infer a low balance or invent a pricing link. |

## Validate and deliver pricing links

Use this rule for both `get_credits.top_up_url` and a submission error's `top_up_url`:

1. Require a nonempty absolute HTTPS URL whose origin exactly matches the selected environment's website in the [connection table](mcp-integration.md#connection), with no embedded username/password. Check URL components rather than hostname substrings. Reject an unknown environment, malformed URL, or origin mismatch without visiting it, changing environments, or rewriting its host.
2. Use the exact returned URL as the Markdown target, with a label meaning "View Fotor plans" in the conversation language. Preserve its path, query, and fragment; the login handoff endpoint, `code` parameter, expiry, and single-consumption rules do not apply to ordinary pricing links. If the link is missing or invalid, explain that the pricing entry is unavailable instead of constructing a replacement.
3. Let the user click the link. A balance or recharge request alone does not call `get_website_url`, open a browser, or refresh a supposed ticket. If the user also explicitly requests a signed-in website session, complete the [website connection flow](mcp-integration.md#connect-to-the-website) separately, then provide the pricing link for them to open in that same browser.

Leave plan selection and payment to the user. A link or a loaded page does not establish website login, a completed payment, or an increased balance. Keep credentials and website-login tickets out of files and diagnostic logs.

## Insufficient credits during media submission

When an image-generation, image-processing, or video submission explicitly returns `insufficient_credits`, report that error. If its `top_up_url` is non-null and valid, deliver it using the pricing-link rules above unless the user prohibited links. Missing or invalid links do not change the error into a balance value; no extra balance query is required.

Stop dependent operations and retain any task ID returned. Do not automatically resubmit, infer that payment completed, or turn the error into `submission_uncertain`. A lost response or an explicit uncertain-submission result still follows the [task recovery workflow](mcp-integration.md#submission-recovery-and-results).
