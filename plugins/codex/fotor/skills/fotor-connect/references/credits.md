# Fotor credit balance

## Query the selected account

1. When the user requests their own balance, follow [MCP authentication](mcp-integration.md#authenticate-mcp). Reuse valid authorization and discover `get_credits`. If it is unavailable, report that and stop.
2. Call `get_credits` without arguments. The authenticated session identifies the account. Report only the nonnegative numeric `credits` value, including zero, and finish. A balance query needs no website visit.
3. If authentication fails, the service reports an error, or the balance is missing or invalid, report it as unavailable. An unknown balance is not zero. Never infer a balance from task consumption.

## Insufficient credits during media submission

When a submission explicitly reports `insufficient_credits`, explain that the task could not proceed for that reason. Retain any known task ID and reported consumption, stop dependent operations, and finish without extra links, a balance query or another submission. A lost response is a different outcome; follow [task recovery](mcp-integration.md#submission-recovery-and-results).
