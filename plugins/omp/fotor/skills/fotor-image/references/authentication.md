---
platform: omp
---

# omp CLI: installation, OAuth and user choices

## Find the installed plugin

Use the active omp profile in the intended Git project. In omp 18.22.2, the
Fotor catalog and plugin have separate installation steps. Check the catalog
source, project scope, and actual package version with native plugin management.
The omp catalog must point to `./plugins/omp/fotor`; the older Claude catalog
can expose the same `fotor@fotor-ai` name with different instructions. Stop on
an unexpected source or a conflicting skill or server. Keep other installations.

Use `omp skill list --json`, the actual `/skill:fotor-connect` command or the
host's skill reading tool. Resolve files from the installed skill's location.
Its parent directory contains the six skills; its grandparent is the package
root. The version is in `.claude-plugin/plugin.json`, the compatible manifest
read by this client. Each skill has its own four references.

## Native connection and OAuth

The bundled HTTP service connects to `https://mcp.fotor.com/mcp` and normally
appears as `fotor:fotor` in `/mcp list`. Use the session's actual registry:
tools can appear on an MCP device instead of as top-level functions. A tool list
is discovery, not a business call. Preserve user restrictions and approvals.

Use a valid grant directly. If authorization is required, stop and obtain the
user's consent before one native `/mcp reauth fotor:fotor` attempt in the
active TUI. Let the user use their ordinary browser and wait for the native
callback within its time limit. No credential copying, hand-entered tokens or
duplicate server entry is needed. Check session tool visibility and let the user
choose whether to continue after login. Browser login, MCP OAuth and model
provider login are separate. Do not retry failed or uncertain submissions.

## Ask before a higher-quality submission

Use the terminal's native `ask` choice when available. Put the actual quality
change and possible credit impact before a single group of three radio options.
The first radio is “Do not generate” and is the only checked input. The two
unselected radios are the specific proposed setting and the default-quality draft.
User preference for a visual style is not a default paid selection. omp can return preselected form values on timeout: a
`timedOut` result, cancellation, missing answer or interruption means no
submission, even if a paid option appears in the returned data. Without an
interactive form, show the choices as text and wait for a new user message.
The host's tool permission dialog and quality consent serve different purposes.

## Recovery and website

Keep the host's effective timeout and approval mode. The default MCP request
deadline in 18.22.2 is 30 seconds. If a request may have reached Fotor, use
its known task ID and the local integration reference rather than resubmitting.
For a website request, first establish the user's selected visible browser
and existing page state; apply the shared one-time link workflow there. If
that route cannot be confirmed, stop before requesting a handoff link.
