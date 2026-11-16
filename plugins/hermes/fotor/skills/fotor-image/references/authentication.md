---
platform: hermes
---

# Hermes Agent authentication and native use

## Plugin and MCP are separate

The implementation baseline is Hermes Agent 0.23.1. The portable Agent Plugins
v1 package installs six instruction-only skills. A separate native `mcp_servers`
configuration registers Fotor; installing or enabling the plugin does not
establish a service connection. Select the existing target profile explicitly
with `hermes -p <profile>` and preserve its model, permissions and other plugins.

Use the native plugin scanner and consent flow for a reviewed fixed Git source.
Stop at an existing same-name plugin, skill or MCP configuration until ownership
is resolved. Do not overwrite user changes. Native skill names are
`fotor:fotor-connect`, `fotor:fotor-image`, `fotor:fotor-video`,
`fotor:fotor-audio`, `fotor:fotor-upload` and `fotor:fotor-credits`.
Use the actual selector offered by the client or native `skills_list` /
`skill_view`; resolve local references from the installed path returned by the
host. The root `plugin.json` records the plugin version; file hashes establish
which package was actually installed.

`skill_view` returns the absolute `skill_dir`. Use that value directly for local
references; the package root is two parent directories above it. Read the root
`plugin.json` with `read_file` when checking the installed version. Do not derive
paths from a profile label: `default` does not imply a `profiles/default/`
directory. If the host supplies no usable path, report the missing location
instead of guessing or searching credential/configuration directories.

## Connection, approvals and OAuth

Model-provider login, MCP authorization and website login are independent.
A loaded skill or successful plugin scan is not evidence of MCP availability.
Inspect native status before making a requested call. Use `list_models` for an
approved connection probe; `get_task` only queries an existing task.

A new native Fotor entry has `url: https://mcp.fotor.com/mcp`, `auth: oauth`,
`enabled: false` and `trust: untrusted`. Enable it only after native authorization
and connection are ready. Keep full tool discovery and existing user restrictions;
use the actual tool names exposed in the current session. The example is not an
auto-loaded MCP manifest and contains no credentials.

`untrusted` delegates consent to Hermes: tools annotated as read-only can run
without an additional confirmation; other tools use the host approval flow.
Inspect the effective approval mode before acceptance. A global bypass or a
session override can change this behavior; stop and report it rather than
changing the user's permission policy. Approval alone is not a successful call.

Reuse an available grant. If authorization is needed, use the native
`hermes -p <profile> mcp login fotor` flow with the user's confirmation. The user
completes it in an ordinary external browser. Observe client completion,
callback-listener release and tool discovery separately from the page result.
Respect native limits and bound a single attempt to at most fifteen minutes.
A failed, cancelled or uncertain attempt stops; a further attempt needs the
user's direction. Keep credentials host-owned and never read, copy or reset them.

## Timeouts and uncertain results

The plugin supplies no timeout override. In 0.23.1, the native tool-call default
is 300 seconds and connection has its own limit. Preserve explicit user values.
Cancellation or a client timeout does not prove that remote work was cancelled.
Follow [task/result handling](mcp-integration.md#runtime-workflow), retain known
task IDs, and report unknown outcomes without resubmitting the operation.

The inspected 0.23.1 authentication-recovery path can retry a request under
particular errors. MCP errors containing only structured data can lose details
when the host renders them as text. These are versioned host conditions, not a
claim that every Fotor request repeats or loses information. Retain available
status, task ID and credit values; distinguish zero from unknown. Preserve the
original failed result and stop affected operations if their safety cannot be
established. Do not replace missing fields with guessed values or reconstruct
credentials. Normal queries do not prove uncertain-submission recovery.

## Website handoff

Prepare the selected browser and confirm its current homepage and login state
before requesting a link. CLI uses the user's ordinary external browser;
Desktop uses the native in-app `desktop_preview` entry attached to the current
session. If that route is unavailable, stop it without switching browser types.
OAuth always uses the ordinary external browser.

After one successful `get_website_url`, validate the production endpoint and
expiry, then immediately open the unchanged returned URL once. Follow the
[website contract](mcp-integration.md#connect-to-the-website); do not prefetch,
rewrite, repeatedly open or display the one-time URL. Failed, expired or
uncertain results stop the attempt without requesting another link. Ask the
user to confirm the final page. An already signed-in browser is not evidence
of a newly established website login.

## Lifecycle and acceptance scope

Use native plugin `disable`, `enable`, `remove` and the recorded fixed source
for reinstallation. MCP is independent: its configuration can remain while
skills are disabled. Toggle only `mcp_servers.fotor.enabled` for MCP activation.
For credential-preserving configuration removal, use native
`config unset mcp_servers.fotor`, then reconstruct only the recorded entry when
reinstalling. Do not use `mcp remove` for this acceptance path: in 0.23.1 it also
removes OAuth tokens. Record configuration removal as such, not as a pass of
that destructive native command.

Before each change compare owned files and configuration with their inventory;
stop on concurrent user edits. Preserve unrelated plugins, models and settings.
Fully restart the actual gateway only after confirming that other sessions will
not be interrupted. If both clients share a gateway or installation, count the
operation once and verify each client in a new session afterward.

Local-source package checks, mock calls, CLI conversations, Desktop UI results,
production queries and browser navigation are distinct evidence. Basic queries
do not establish media execution, public-source upgrades, publication or an
official catalog listing. Keep failures and incomplete checks visible.
