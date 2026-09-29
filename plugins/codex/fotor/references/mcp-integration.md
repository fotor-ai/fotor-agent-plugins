# Fotor MCP Integration

## Connection

This package selects the **production** environment at `https://mcp.fotor.com/mcp` using Streamable HTTP. Preserve this environment throughout a task and its assets.

| Environment | MCP endpoint | Website target | Login handoff endpoint |
| --- | --- | --- | --- |
| production | `https://mcp.fotor.com/mcp` | `https://www.fotor.com/` | `https://api.fotor.com/api/user/mcp/browser-handoff` |

Production OAuth, authenticated tool discovery, and read-only model queries passed on 2026-09-24. New-build host installation, automatic website login, credit/top-up actions, uploads, and media execution still require separate acceptance.

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

1. Confirm the installed endpoint and enabled state. If Fotor tools are absent or a call fails authentication, diagnose native authorization before choosing a reload/new-chat fallback. In Codex, follow [native authorization diagnosis](codex-authentication.md), which remains usable without Fotor remote tools; other hosts use their native connection controls. Reuse usable credentials through the host's supported MCP client. A confirmed missing/invalid authorization enters OAuth; unknown status remains inconclusive and uses the explicit choice in that workflow after bounded diagnosis. If the user forbids all browser use and authentication is needed, explain that requirement and stop. Otherwise, start OAuth only for missing/invalid credentials or explicit reauthorization, preserving existing credentials on network errors.
2. When OAuth is needed, start the host's supported login flow for the selected server and open its authorization URL in an external browser. Let the user complete login and consent. If external-browser automation is unavailable, explain that limitation and give the user the authorization link to open externally; keep the callback flow active. Do not silently use the in-app browser for OAuth.
3. After opening the external authorization page or supplying the manual external link, collect the [OAuth completion choice](#oauth-completion-choice). Keep the callback flow active while awaiting the answer.
4. When this request started OAuth, proceed only after an explicit continue choice and client-confirmed authorization success. If the callback is pending, explain the delay and wait within the client's login timeout. OAuth cancellation, login expiry, or denied authorization stops the flow before website handoff. A user reporting completion or a login page loading is not proof that credentials were obtained.
5. After credential reuse or successful OAuth, establish a usable connection in the client serving this request and discover the authenticated tools. Apply [bounded connection recovery](#bounded-connection-recovery) to eligible transient failures; a successful OAuth result alone does not establish tool availability. Keep credential values in the client's store, outside conversation output, files, and logs.

For an MCP-only health check, call `list_models` when discovered and report its actual outcome. If a successful discovery omits it, report the unavailable probe without assuming authentication failed; uploads and the new website-login tool can remain available while generation and credits tools are disabled. A failed or incomplete discovery is a connection problem, not proof that a tool is absent. Installation and discovery alone do not establish a successful call. Do not request upload addresses, transfer files, submit media, query arbitrary tasks, or call `get_credits` or `get_website_url` for a read-only health check.

### OAuth completion choice

Use the host's structured question/choice UI to ask whether the user has completed external login and authorization. Localize the question and labels to the conversation language. For a website connection, offer:

| Choice | Next action |
| --- | --- |
| Signed in and authorized; open Fotor in the in-app browser | Verify client-confirmed OAuth success, then continue the website flow. |
| Not finished; keep waiting | Keep the current callback active within its timeout and wait for the user to explicitly choose to continue. |
| Cancel connection | Stop this connection attempt and cancel the pending login through the client when supported. Preserve credentials already stored; cancellation does not request logout or revocation. |

A preselected option, no answer, elapsed time, or a successful callback alone does not select Continue. Request `get_website_url` only after both user continuation and client success, so its short-lived ticket is not created while waiting for a choice. If structured input is unavailable, present the same choices in text and wait for an explicit reply. If the user reports an error, report the failed authorization and stop rather than treating the answer as completion.

Skip this prompt when the request reuses valid credentials without starting OAuth. For an MCP-only check or another task requiring authentication, replace the first label with "Signed in and authorized; continue the requested task" and preserve the original scope; the choice does not add website navigation.

### Bounded connection recovery

Apply this policy only to read-only OAuth metadata retrieval, MCP connection/tool discovery, and the `list_models` connection probe. Eligible failures are connection interruptions, request timeouts, temporary HTTP 5xx responses, and errors such as "failed to resolve OAuth metadata before using stored credentials" caused by an HTTP request failure. An OAuth-related network error does not establish invalid credentials. Read-only native authorization diagnosis shares the same retry counter and time budget. First resolve known [execution restrictions](codex-authentication.md#execution-permissions) through host permissions or its connection UI; unchanged access restrictions are not transient retries. Other inconclusive results use the remaining limits, then the explicit unknown-state choice rather than silently entering OAuth.

1. Use one retry counter and one **90-second automatic-work budget** for the whole connection/discovery/probe sequence, starting with its first automatic request. After the initial attempt, allow at most **two retries**, waiting **2 seconds** before retry 1 and **5 seconds** before retry 2. Count automatic requests, reconnect/refresh work, and backoff waits. Pause accounting while the user completes external login, answers a connection choice, or decides a host permission request; the client's own OAuth timeout still applies. Preserve spent time and retries across stages, successful intermediate requests, OAuth completion, and refreshes.
2. Before another wait or request, check the remaining budget and cancellation state. If the remaining time cannot cover the next backoff, stop rather than shortening the delay. Cap request timeouts to the remaining budget where the host supports it. Stop at the first exhausted limit. If the host cannot interrupt an in-flight request, report any overrun and start no further work after it returns; these skill instructions cannot enforce the host's transport deadline.
3. Retain the selected environment, stored credentials, and the user's Continue / Wait / Cancel decision. Briefly report the retry number in the conversation language. Continue does not need another confirmation; Wait keeps continuation pending and Cancel ends it. This loop never restarts OAuth, repeats token exchange, logs out, or clears credentials.
4. Retry the failed read-only operation. When initialization or tool availability requires recovery, use an available host-supported refresh/reconnect in the client serving the current request, then rediscover its tools. A separate diagnostic client does not refresh the original chat. Before declaring the host unable to recover, classify native authorization using the [authentication workflow](#authenticate-mcp). Missing remote tools do not rule out a native login command or connection UI. After authorization is classified or completed, if the current host still offers no usable recovery path, report that specific limitation and the reload/new-session next step; do not invent commands, extract tokens, or create an ad hoc credential client.
5. Require successful discovery and, when `list_models` is available, a successful call before resuming the requested workflow. A catalog or saved OAuth status alone is insufficient; an empty filtered model result is a valid response. If successful discovery genuinely omits the probe, report that capability gap and retain the existing workflow for available tools. After exhaustion, report the failed stage and retries used without claiming a website connection or assuming the user must log in again.

Confirmed invalid credentials, denied permissions, invalid parameters, environment mismatches, certificate-validation errors, and genuinely absent tools are not transient retries. Stop this recovery loop and explain the specific condition; use the supported authorization flow only when missing/invalid credentials or an explicit reauthorization request warrant it. User cancellation stops recovery immediately.

This budget covers automatic MCP connection recovery, not website navigation or media execution. Keep website/top-up ticket creation and consumption, upload requests/transfers, media submissions, and task queries outside this retry loop. Their existing workflows retain their own stopping conditions; in particular, preserve uncertain submissions and the one-time-link replacement limit.

### Connect to the website

1. Complete MCP authentication and authenticated tool discovery, using [bounded connection recovery](#bounded-connection-recovery) for eligible failures. When available, require a successful `list_models` read-only check without dumping its catalog before requesting a website link. Determine whether a visible in-app browser is available before creating the short-lived link. Only a successful discovery that omits `get_website_url` establishes a missing website capability; an unavailable chat tool surface or failed discovery does not establish the service version. Report MCP and website outcomes separately, without substituting a top-up link or claiming ordinary page access as automatic login.
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
