# Fotor MCP Integration

## Connection

This package selects the **test** environment at `https://test-mcp.fotor.com/mcp` using Streamable HTTP. Preserve this environment throughout a task and its assets.

The test service has passed Codex OAuth, tool discovery, and read-only model queries. Media submission and task-query tools are exposed; actual image/video execution has not been accepted. Installing this build does not establish host acceptance.

Complete the service's OAuth flow through Codex when authentication is required. Discover the available tools and use their actual input schemas. Keep credentials in the client's supported credential store. Verify a connection with `list_models`; a successful catalog query does not verify upstream generation or media quality. An installed package alone does not establish a working connection.

## Runtime workflow

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
| `get_video_agent_url` | Obtain a short-lived website sign-in link when the user requests the video Agent page |

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

### Video Agent website handoff

Call `get_video_agent_url` only when the user requests access to the Fotor video Agent page. Present `handoff_url` immediately as a clickable link with its returned `expires_in` lifetime, which is at most 60 seconds. The link works once: do not fetch, preview, or pre-open it. Obtain a fresh link if the user reports expiry. Treat the link as a temporary sign-in credential and keep it out of repository files and diagnostic logs.
