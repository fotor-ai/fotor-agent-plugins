# Fotor MCP Integration

## Connection

This package selects the **test** environment at `https://test-mcp.fotor.com/mcp` using Streamable HTTP. Preserve this environment throughout a task and its assets.

| Environment | MCP endpoint | Website target | Login handoff endpoint |
| --- | --- | --- | --- |
| test | `https://test-mcp.fotor.com/mcp` | `https://test-www.fotor.com/` | `https://test-api.fotor.com/api/user/mcp/browser-handoff` |

Earlier Codex OAuth and model-query checks passed against the test service. Current service source exposes get_website_url and get_credits; test deployment has been reported updated. Authenticated discovery, website login, and credits/top-up workflows still need live acceptance. Uploads, image processing, and generation also require separate acceptance. Installing this build does not establish host acceptance.

Use the runtime routing, authentication, and website flow below. Keep credentials in the client's supported store. An installed package or a successful model query does not establish website login, successful uploads, image processing, upstream generation, or media quality.

## Runtime workflow

### Route the request

| Intent | Action |
| --- | --- |
| Connect to Fotor, connect to the Fotor website, or connect and open Fotor | Authenticate MCP, then follow the website flow below |
| Connect to Fotor MCP or check MCP connectivity without a website request; explicitly keep the website closed | Perform the requested MCP check; omit the website handoff |
| Introduce features or query models | Answer from current tool descriptions or model queries; omit website navigation unless also requested |
| Check credit balance or top up | Follow the [credits workflow](credits.md); ordinary connection does not query balance |

Equivalent wording in other languages follows the same routing. Uploads, image processing, and media generation alone do not request website access. Ordinary website connection requests authorize the final visible navigation; they do not authorize media submissions.

### Authenticate MCP

1. Confirm the installed endpoint and enabled state. Reuse valid credentials through the host's supported MCP client. If the user forbids all browser use and authentication is needed, report that authorization is required and stop. Otherwise, start OAuth only when credentials are missing or invalid, or the user explicitly requests reauthorization; a network or service error alone does not justify resetting credentials.
2. When OAuth is needed, start the host's supported login flow for the selected server and open its authorization URL in an external browser. Let the user complete login and consent. If external-browser automation is unavailable, explain that limitation and give the user the authorization link to open externally; keep the callback flow active. Do not silently use the in-app browser for OAuth.
3. Wait for the client to confirm authorization success and discover the authenticated tools. A user saying they clicked the button, or a login page loading, is not sufficient evidence that credentials were obtained. On cancellation, timeout, or an authorization error, report the outcome and stop before creating a website handoff. Keep credential values in the client's store, outside conversation output, files, and logs.

For an MCP-only health check, call `list_models` when discovered and report its actual outcome. If it is absent, report the unavailable probe without assuming authentication failed; uploads and the new website-login tool can remain available while generation and credits tools are disabled. Installation and discovery alone do not establish a successful call. Do not request upload addresses, transfer files, submit media, query arbitrary tasks, or call `get_credits` or `get_website_url` for a read-only health check.

### Connect to the website

1. Complete MCP authentication and authenticated tool discovery. When available, use `list_models` for a read-only connection check without dumping its catalog. Determine whether a visible in-app browser is available before creating the short-lived link. A missing `get_website_url` means website login is unavailable in that service version; report the MCP and website outcomes separately, without substituting a top-up link or claiming ordinary page access as automatic login.
2. Call `get_website_url` without arguments. Its public response contains `handoff_url` and integer `expires_in` from 0 to 600; it does not expose `target_url`. Require a usable link and remaining lifetime greater than zero. A zero lifetime is already expired: use the bounded replacement rule below. Missing, malformed, or out-of-range fields are a handoff failure. The server configures and validates the destination; before navigation, the plugin can check only the handoff endpoint against the [connection table](#connection), not the hidden target. Never decode the ticket, rewrite its URL, or change environments to recover a mismatch.
3. Follow [one-time browser links](#one-time-browser-links) for the returned `handoff_url`, using the actual remaining lifetime. This final visible navigation is the requested website visit and consumes the ticket.
4. Inspect the resulting page in that same browser session. Compare its HTTPS origin with the selected website in the connection table and verify authenticated state before reporting the website connected. A valid handoff endpoint alone does not prove the hidden redirect target is correct. On a different environment or unexpected origin, stop further interaction and report the mismatch; do not follow it as a fallback. A link, a generic page, or a login/error screen is insufficient. If state is unclear, ask the user to confirm it. Website failure does not justify resetting working MCP credentials.
5. Introduce the available main features after the connection attempt and report partial outcomes accurately. Do not automatically list models or query credits; website login works independently of balance and billing-account registration.

### One-time browser links

Apply this section only when the website or credits workflow selected a browser/link action. Website and top-up links have distinct purposes. This plugin's connection and selected top-up flows use one final visible in-app navigation. The service descriptions instead prescribe a user-clicked link to prevent early consumption; the plugin's selected browser action is the intended final visit, never a pre-opening check. Honor an explicit user preference for manual delivery and any host restriction; use the manual fallback when direct navigation is unavailable.

- Parse the returned URL before use. It must have the exact HTTPS origin and path of the selected environment's [login handoff endpoint](#connection), no username/password or fragment, and one nonempty `code` query parameter only. Check URL components rather than hostname substrings. Reject an unknown environment or mismatch without fetching the address, rewriting the host, or switching services.
- Immediately navigate once in a visible in-app browser. Do not first fetch, preview, probe, or open the ticket in a hidden tab. After navigation, treat it as consumed and never offer it as a reusable login link.
- When the required browser is unavailable, explain the limitation and present the complete unused link for immediate manual opening. For website links, include the returned lifetime; credits links do not expose an expiry field. Manual completion stays pending until confirmed. Honor explicit user restrictions on browsing or link disclosure; do not silently switch browsers.
- If a link expired or a failed navigation may have consumed it, resolve the cause or establish a manual route before requesting one replacement for the still-requested action. Stop after a repeated failure. Record outcomes without credentials, authorization codes, or one-time URLs in repository files or diagnostic logs.

### Discover capabilities and choose a model

Inspect the connected service's actual tool descriptions and input schemas. The table below describes the current contract, not a fixed tool-count requirement. If a required operation is absent, explain the missing capability and provide only a clearly labeled preparation brief when useful.

| Tool | Purpose and required decision |
| --- | --- |
| `list_models` | Discover model IDs and modes; query a selected ID for its supported parameters and defaults |
| `get_upload_url` | Obtain a signed PUT address; follow the [upload workflow](media-upload.md) to actually transfer local media |
| `submit_image_task` | Generate without reference images; edit/reference with `image_urls` |
| `submit_image_upscale_task` | Upscale `image_url` with an optional positive ratio; no model ID or prompt |
| `submit_background_removal_task` | Remove the background from `image_url`; no model ID or prompt |
| `submit_text_to_video_task` | Generate video from a text prompt |
| `submit_first_frame_video_task` | Animate `image_url` as the first frame |
| `submit_first_last_frame_video_task` | Use ordered `start_image_url` and `end_image_url` |
| `submit_multimodal_video_task` | Use supported image, video, and/or audio references |
| `get_task` | Query an existing image-processing, image-generation, or video task by `task_id` |
| `get_website_url` | Obtain an environment-bound, one-time website login link for the verified MCP user |
| `get_credits` | Query available balance and obtain a top-up link; apply the [credits workflow](credits.md) |

For model-based generation/editing, start with `list_models`, optionally filtering by `media_type` and `mode`. Uploads and fixed image-processing operations use their own tool contracts without model selection. Filters combine with AND; an empty filtered result is not evidence of a failed connection. Then query `list_models` with the chosen `model_id` before submitting. Model IDs, available modes, and catalog sizes can change; an unknown or retired ID requires a new selection, not an invented replacement.

Use the returned resolutions, aspect ratios, qualities, duration values/ranges, reference types, and defaults. Honor an explicit model or output preference; explain an unsupported constraint before changing it. When no preference is given, choose a suitable supported model and use its reported defaults. `duration=0` selects `default_duration`; duration ranges are inclusive integer seconds, while a list permits only its listed values. `explicit_aspect_ratio_modes` requires a concrete supported ratio instead of `auto`; for multimodal references this condition applies when images are supplied without videos.

### Inputs and native capabilities

Generation references and image-processing inputs must be accessible HTTPS URLs. For local files and client-accessible attachments, follow the shared [media upload workflow](media-upload.md). Use the successful upload's `file_url`, never its signed `upload_url` or a filesystem path, as a task input. Preserve the selected environment and ordered references, including first/last frames. Storage acceptance does not establish the model's input compatibility.

Use native model resolutions and supported output options for generation. Explicit image upscaling uses `submit_image_upscale_task`; its current positive, finite ratio defaults to 2 and its output width and height are capped at 2048 pixels. Background removal uses `submit_background_removal_task` for a transparent image. Neither fixed operation requires a model ID or prompt. These tools do not establish masks, video upscaling, timeline/range editing, or dubbing. Run a sequence of operations only when requested, waiting for each required result before the next submission.

### Submission, recovery, and results

1. Submit the requested operation once and retain its returned `task_id`, environment, and selected model or fixed `operation`. A submitted task is not a completed artifact.
2. Query that task with `get_task`. Each call performs one lookup; the client may repeat queries at reasonable intervals within the active task's execution budget. The tool does not start background polling.
3. Respect `is_terminal` and the actual status. `processing` does not distinguish queued, running, or upstream-unavailable tasks. Report `unknown` as uncertain. Stop polling when `is_terminal` is true and report the terminal outcome, including failure, timeout, cancellation, or completion. If state fields conflict, report the inconsistency rather than inventing progress.
4. On completion, deliver the actual `result_url` when present. If it is missing, report completion without an available artifact link. Result URLs may expire. Prefer a host-supported media preview and describe quality only after inspecting the result.
5. An explicit `submission_uncertain` error or a lost submission response can mean a task was created and charged. Use a known task ID for recovery. Without one, report the uncertain submission and stop; do not automatically resubmit or assume an unprovided idempotency mechanism. On a query error or polling limit, preserve the same ID and report the last observed state.

`credits_increment` reports the increment returned by that particular submission or query, not a balance or a price quote. Preserve `0` as zero and `null` as unknown. Do not add submission and polling values together as if each observation were a new charge. Report missing or inconsistent values without estimating usage.

Task lookup is scoped to the selected API key's Business. Another Business cannot retrieve its results, and a task ID must not be moved between environments to recover a failure. Key lookup can register a developer or create a missing key, so `get_task` is not a side-effect-free connection probe. Use `list_models` for read-only checks; it reads the model catalog without developer lookup, key creation, or generation requests.
