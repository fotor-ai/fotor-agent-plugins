---
platform: kilo
---

# Kilo CLI authentication and native use

## Installation and skill ownership

The current host baseline is Kilo CLI 7.8.27. Installation has two user-managed
parts: complete local skill directories in the project's `.kilo/skills/`, and a
native `mcp.fotor` entry in `.kilo/kilo.jsonc`. Use a reviewed source snapshot and
retain its commit, file hashes and modes before copying. Each skill keeps its
`fotor-version.json`, license and four local reference files.

Check effective project and global sources before installation. An existing
same-name service, skill or unexplained override stops the operation for review.
Preserve unrelated files, comments, permissions and explicit user settings.
New native entries use type `remote`, production URL, native `oauth: {}` and
`timeout: 300000`. The named non-interactive `kilo mcp add` command in this
baseline writes global configuration; use the reviewed project configuration
for this project-scoped route.

The source `bundle.json` is Fotor version metadata. `MCP.yaml` and companion
archives remain material for future marketplace work, with a separate listing
transport shape. This local route creates no marketplace ownership receipt.
Desktop, independent CLI marketplace installation and automatic bundle updates
are outside the current support scope; earlier core API results keep their
original experimental scope.

After installation, select the native `/fotor-connect` skill command where
available, or let the model use native `skill` discovery. Verify the actual
completion/selector in the actual CLI. Resolve references relative to the skill
directory returned by the host. Local skill placement does not imply a traditional
plugin root above the skill directory. Read the installed version marker when
verifying provenance; a source README alone does not establish installed bytes.

## Connection and native OAuth

Model-provider login, MCP authorization and Fotor website login are separate
states. Use native MCP status or `kilo mcp list`; a loaded skill is not a connected
service. A connection probe uses `list_models`, not a task lookup.

The service is `https://mcp.fotor.com/mcp`, with native HTTP/OAuth and complete
tool discovery subject to the user's permissions. The expected service ID is
`fotor`; use the tool names actually exposed by the current session. Preserve
existing tool restrictions and explicit configuration.

Reuse a valid grant. When authorization is needed, use the host's native consent
entry, such as `kilo mcp auth fotor` in the target project. The user completes
consent in an ordinary external browser. Observe client completion and callback
cleanup separately from the page result. Confirm whether to continue the pending
request when its continuation was not already authorized.

Keep automatic checks bounded. Respect the native pending-attempt lifetime and
stop after a failed or uncertain attempt rather than restarting it repeatedly.
Use native diagnostics with the user when needed. Keep credentials host-owned;
do not read, copy, reset or inject them to make a connection appear successful.
Record shared authorization and approved renewal accurately.

## Waiting, errors and request recovery

A newly installed Fotor entry sets `timeout` to 300000 milliseconds. This affects
connection, discovery and tool requests. Valid progress can reset the timer;
300 seconds is not a total execution-duration guarantee. Explicit user settings
remain authoritative. Abort/cancellation propagation does not prove that remote
work was cancelled.

Use the task/result rules in [MCP integration](mcp-integration.md#runtime-workflow).
Retain any task ID already observed. A failed transport or missing response is
not permission to submit again. If native error handling omits information,
report what remains known and the missing information; never invent a task ID,
credit value or successful result.

The inspected host/SDK can resend an authenticated request during particular
authorization recovery paths. The normal session wrapper also converts MCP
errors to text; structured-only error details can be lost. These are conditional
host boundaries, not a claim that every Fotor call is duplicated. Core fixture
results, normal production observations and actual media execution are separate.
Keep versioned evidence in release/acceptance documentation instead of repeating
a generic risk warning before ordinary requests.

## Website handoff

Prepare and confirm the user's ordinary external browser before requesting a
handoff. Preserve its current page and login state. If the required browser is
unavailable, stop that route and report the limitation.

After a successful `get_website_url`, validate the production handoff endpoint
and expiry, then immediately navigate once with the returned URL unchanged.
Follow [website handoff](mcp-integration.md#connect-to-the-website) for the response
contract. Avoid exposing a one-time link in the final response. A failed,
expired or uncertain open stops the attempt; do not prefetch, rewrite or consume
the URL again. The user confirms the final website state.

## Lifecycle and verification

Toggle the project entry's `enabled` value to disable or enable MCP. Local skills
may remain discoverable while its connection and tools are disabled.

For replacement or removal, compare current files against the retained install
inventory first. Stop if files or configuration changed unexpectedly. Replace or
remove only the six owned skill directories and the project's Fotor entry;
preserve other skills, settings and native authorization. These are manual file
and configuration operations, not marketplace removal or a native bundle upgrade.
Repeated deployment of identical inputs must preserve contents and permissions.

After reinstallation, reload or fully restart the actual core, start a new
session, verify the installed version/path, and make the approved read-only
query. Closing a window alone does not prove a core restart. Respect other
active sessions and concurrent configuration edits.

Basic connection/model/balance/website checks do not establish live uploads,
image/video/audio completion, safe uncertain-submission recovery or official
marketplace availability. Report the observed scope and unresolved host limits.
