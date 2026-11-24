---
platform: cline
---

# Connect Fotor in Cline

## Find the installed skill

In the intended project, open `fotor-connect` for a native Cline Git install.
Use `cline config plugins --json` and `cline config skills --json` to identify
the registered module and its bundled skills. A portable installation under
`~/.agents/plugins/fotor/` exposes `fotor:fotor-connect` through Agent Plugins.
In Desktop inspect the installed source in **Customize → Installed**.
Use the name the current session exposes. If both sources are active, ask
the user to choose the installation before using a Fotor skill.

Read `SKILL.md` and the four references from the directory Cline returns. From
that skill directory, the package manifest is two levels up at `plugin.json`.
Check the manifest name and version if the active copy is unclear. A workspace,
personal skill, or a second portable package can supply a similar name. Resolve
conflicting sources with the user before changing an installation.

## Authorize the remote service

Use the configured service named `fotor` with Streamable HTTP at
`https://mcp.fotor.com/mcp`. Inspect its native connection status and the
tools offered in this conversation. `cline config mcp --json` inventories
settings-backed servers. Keep diagnostic output local and omit credentials.
The package's `mcp.example.json` illustrates the entry. Registration,
OAuth and tool permissions belong to Cline and persist independently of the
six skills.

Reuse the current grant. If the native service requests authorization, tell
the user that a fresh Fotor MCP sign-in replaces the same account's previous
MCP login, including one on another client. Wait for explicit permission.
In the interactive CLI, open `cline mcp` and select **Authorize OAuth** for
the matching entry. In Desktop, use the remote server's OAuth action.
For a new registration select **OAuth** and leave the client ID empty for
dynamic registration. The user completes consent in the external browser
and returns to the same Hub. Leave credentials and one-time URLs in those
native controls. Check that tools were discovered before carrying out the
user's request. A model-provider login and a Fotor website login each have
their own scope.

## Tools, approvals and recovery

Choose the actual Fotor tool and its schema from the active session. Preserve
server restrictions, timeouts and one-time approvals. For a request for image
models use `list_models` with `media_type=image`. A successful inventory or
OAuth callback alone does not confirm a business call.

For generation follow the active media skill. When a selected quality tier is
above the current model's recommended default, use Cline's follow-up question
or wait for a text choice. Submit only after the applicable choice. Keep the
host's tool approval separate. On cancellation, expiry or an ambiguous reply
leave the request unsubmitted. After an uncertain submission, retain a returned
task ID and use the [task workflow](mcp-integration.md#runtime-workflow).

For a requested website handoff, confirm the intended browser and its current
login state, then follow the [website workflow](mcp-integration.md). Open
the returned URL unchanged once in the agreed browser. When that browser is
unavailable, stop before requesting a ticket. Upload only the user's selected
files. [Media preparation](media-upload.md) · [Own credit balance](credits.md)
