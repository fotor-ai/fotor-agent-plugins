# Fotor MCP Integration

## Connection

This package selects the **test** environment at `https://test-mcp.fotor.com/mcp` using Streamable HTTP. Preserve this environment throughout a task and its assets.

The test service has passed Codex OAuth, tool discovery, and read-only model queries. Media submission and task-query tools are exposed; actual image/video execution has not been accepted. Installing this build does not establish host acceptance.

Use the runtime routing, authentication, and website flow below. Keep credentials in the client's supported store. An installed package or a successful model query does not establish website login, upstream generation, or media quality.

## Runtime workflow

### Route the request

| Intent | Action |
| --- | --- |
| Connect to Fotor, connect to the Fotor website, or connect and open Fotor | Authenticate MCP, then follow the website flow below |
| Connect to Fotor MCP or check MCP connectivity without a website request; explicitly keep the website closed | Perform the requested MCP check; omit the website handoff |
| Introduce features or query models | Answer from current tool descriptions or model queries; omit website navigation unless also requested |

Equivalent wording in other languages follows the same routing. Media generation alone does not request website access. Ordinary website connection requests authorize the final visible navigation; they do not authorize media submissions.

### Authenticate MCP

1. Confirm the installed endpoint and enabled state. Reuse valid credentials through the host's supported MCP client. If the user forbids all browser use and authentication is needed, report that authorization is required and stop. Otherwise, start OAuth only when credentials are missing or invalid, or the user explicitly requests reauthorization; a network or service error alone does not justify resetting credentials.
2. When OAuth is needed, start the host's supported login flow for the selected server and open its authorization URL in an external browser. Let the user complete login and consent. If external-browser automation is unavailable, explain that limitation and give the user the authorization link to open externally; keep the callback flow active. Do not silently use the in-app browser for OAuth.
3. Wait for the client to confirm authorization success and discover the authenticated tools. A user saying they clicked the button, or a login page loading, is not sufficient evidence that credentials were obtained. On cancellation, timeout, or an authorization error, report the outcome and stop before creating a website handoff. Keep credential values in the client's store, outside conversation output, files, and logs.

For an MCP-only health check, call `list_models` once tools are available and report its actual outcome. Installation and discovery alone do not establish a successful call. Do not submit media, query arbitrary tasks, or create website sign-in links for a health check.

### Connect to the website

1. Complete MCP authentication above. Determine whether a visible in-app browser is available before requesting the short-lived link. Use the manual fallback below when it is unavailable; do not switch to an external browser silently.
2. Call `get_video_agent_url` and use its returned `handoff_url` and `expires_in`. The destination is the service-configured Fotor Video Agent page; preserve the returned URL rather than constructing a homepage, changing environments, or appending MCP tokens. Missing or invalid response fields are a handoff failure.
3. Immediately navigate once to that URL in a visible in-app browser for the user. This is the requested destination visit and consumes the one-time link. Do not first fetch, probe, preview, or open it in a hidden tab, and do not offer the consumed URL as a reusable login link. Use the returned lifetime without assuming a fixed expiry limit.
4. Inspect the resulting page through that same browser session. Report the website as connected only when its authenticated state is observable; a returned URL or a loaded login/error page alone is insufficient. If the state is unclear, ask the user to confirm it. Report MCP success separately if website access fails or remains unverified.

If the in-app browser cannot be opened, explain the limitation and present the unused link immediately for the user to click, with its returned lifetime; manual completion remains pending until confirmed. If navigation was attempted, treat that link as potentially consumed. Resolve the failure or confirm a manual route before requesting one replacement for the still-requested visit; stop and report repeated failure rather than creating a retry loop. Keep handoff URLs out of repository files and diagnostic logs.

### Discover capabilities and choose a model

Inspect the connected service's actual tool descriptions and input schemas. The table below describes the current contract, not a fixed tool-count requirement. If a required operation is absent, explain the missing capability and provide only a clearly labeled preparation brief when useful.

| Tool | Purpose and required decision |
| --- | --- |
| `list_models` | Discover model IDs and modes; query a selected ID for its supported parameters and defaults |
| `submit_image_task` | Generate without reference images; edit/reference with `image_urls` |
| `submit_text_to_video_task` | Generate video from a text prompt |
| `submit_first_frame_video_task` | Animate `image_url` as the first frame |
| `submit_first_last_frame_video_task` | Use ordered `start_image_url` and `end_image_url` |
| `submit_multimodal_video_task` | Use supported image, video, and/or audio references |
| `get_task` | Query one existing task by `task_id` |
| `get_video_agent_url` | Obtain the one-time sign-in link for a requested Fotor website connection |

Start with `list_models`, optionally filtering by `media_type` and `mode`. Filters combine with AND; an empty filtered result is not evidence of a failed connection. Then query `list_models` with the chosen `model_id` before submitting. Model IDs, available modes, and catalog sizes can change; an unknown or retired ID requires a new selection, not an invented replacement.

Use the returned resolutions, aspect ratios, qualities, duration values/ranges, reference types, and defaults. Honor an explicit model or output preference; explain an unsupported constraint before changing it. When no preference is given, choose a suitable supported model and use its reported defaults. `duration=0` selects `default_duration`; duration ranges are inclusive integer seconds, while a list permits only its listed values. `explicit_aspect_ratio_modes` requires a concrete supported ratio instead of `auto`; for multimodal references this condition applies when images are supplied without videos.

### Inputs and native capabilities

Reference inputs must be accessible HTTPS URLs. The current tools do not include a local-file upload operation. If only a local asset is available, obtain a suitable URL or use an available, authorized upload mechanism; do not pass a filesystem path as a remote URL. Keep ordered references, including first/last frames, in their intended positions.

Use native model resolutions and supported output options. The current tools do not promise masks, timeline/range editing, post-generation upscaling, or dubbing. Image editing and reference-guided video generation should be described as those operations rather than unrestricted editing.

### Submission, recovery, and results

1. Submit the requested operation once and retain its returned `task_id`, environment, and selected model. A submitted task is not a completed artifact.
2. Query that task with `get_task`. Each call performs one lookup; the client may repeat queries at reasonable intervals within the active task's execution budget. The tool does not start background polling.
3. Respect `is_terminal` and the actual status. `processing` does not distinguish queued, running, or upstream-unavailable tasks. Report `unknown` as uncertain. Stop polling when `is_terminal` is true and report the terminal outcome, including failure, timeout, cancellation, or completion. If state fields conflict, report the inconsistency rather than inventing progress.
4. On completion, deliver the actual `result_url` when present. If it is missing, report completion without an available artifact link. Result URLs may expire. Prefer a host-supported media preview and describe quality only after inspecting the result.
5. If the submission response is lost, use a known task ID for recovery. Without one, report the uncertain submission and stop; do not automatically resubmit or assume an unprovided idempotency mechanism. On a query error or polling limit, preserve the same ID and report the last observed state.

Task lookup is scoped to the selected API key's Business. Another Business cannot retrieve its results, and a task ID must not be moved between environments to recover a failure. Key lookup can register a developer or create a missing key, so `get_task` is not a side-effect-free connection probe. Use `list_models` for read-only checks; it reads the model catalog without developer lookup, key creation, or generation requests.
