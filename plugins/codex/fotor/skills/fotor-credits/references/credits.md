# Fotor Credit Information

This Codex policy covers balance questions, credit limitations and credit errors in every skill. It provides account information without a purchase or upgrade workflow. Media submission and ordinary website connection do not require a preliminary balance query.

## Query the selected account

1. Query an existing balance only when the user requests it. Follow [MCP authentication](mcp-integration.md#authenticate-mcp), reuse valid authorization, and discover `get_credits`. A missing tool means the capability is unavailable, not that the balance is zero.
2. Call `get_credits` without arguments. Report the actual nonnegative numeric `credits`, including zero. Ignore `top_up_url`: do not disclose it, open it, or construct a replacement purchase link.
3. `credits_increment` is the consumption reported for a task, not the available balance or a price quote. A balance observation does not guarantee a later submission can proceed.
4. Report authentication errors, `credits_unavailable`, invalid or missing values, and other failures as unavailable information. Never replace an unknown balance with zero, create credentials, or change environments to work around an error.

## Select the response

| Request and result | Action |
| --- | --- |
| Valid existing balance, including low or zero | Report the balance without a recharge recommendation or purchase link. |
| Explicit recharge, purchase, subscription or upgrade request | Explain that this plugin does not handle that operation. Do not start a purchase, open checkout, provide a buying link, or route through another skill. Query balance only if the user also requested it. |
| Feature unavailable with an explicit service reason | Explain that observed reason neutrally. An explicit insufficient-credit result establishes a credit limitation; other errors do not. |
| Feature unavailable with no established reason | State that the cause is unknown; do not infer that buying credits or upgrading would fix it. |
| Balance-only, no-link or no-browser instruction | Honor it; omit unrelated actions and links. |

<a id="validate-and-deliver-pricing-links"></a>

## Informational links

**Codex does not deliver pricing, recharge, purchase, subscription, upgrade or checkout links**, including service-returned `top_up_url` values. Low balance does not override this rule. Do not disguise a buying destination as a help link, rewrite a URL, or use website connection as a route into buying.

When information would help, a known official HTTPS help or feature page may be provided only after its informational purpose and destination are established. It must not directly enter a purchase or upgrade flow. Preserve the exact verified destination and the user's link restrictions. A product origin alone does not prove a page is informational; if its destination or purpose is unknown, explain in text and omit the link. Do not visit a purchase URL merely to validate it.

Ordinary [website connection](mcp-integration.md#connect-to-the-website) remains available for an independently requested website visit; a credit or purchase request does not trigger it. Keep credentials and website-login tickets out of diagnostic records.

## Insufficient credits during media submission

When any `submit_*_task` call explicitly returns `insufficient_credits`, report that the requested operation could not proceed because the service reported insufficient credits. Ignore any `top_up_url` and do not offer a purchase or upgrade path. No additional balance query is required.

Stop dependent operations and retain any returned task ID. Do not automatically resubmit, infer that a purchase completed, or turn this error into `submission_uncertain`. Lost responses and uncertain submissions retain the [task recovery workflow](mcp-integration.md#submission-recovery-and-results). Preserve zero and unknown consumption values without inventing an amount.
