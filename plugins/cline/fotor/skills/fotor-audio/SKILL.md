---
name: fotor-audio
description: Create a soundtrack for an image or video, generate a new track from reference audio, extend/remix/edit a song, synthesize speech or a scripted podcast, or separate vocals and accompaniment with Fotor.
---

# Fotor Audio

Discover the requested tool through the [MCP connection](references/mcp-integration.md#connection). Use `list_models(media_type="audio", mode=...)` for the chosen operation, then inspect a returned `model_id`. Its `operations[]` describes the selector, parameters, limits and output. Match these to the current tool schema. Only the soundtrack tool accepts a caller-selected model. If a required input is missing, ask for it before submitting.

## Choose the operation

| Request | Tool and selector | Required inputs and result |
| --- | --- | --- |
| Compose music for a photo or clip | `submit_soundtrack_task`, discovery mode `soundtrack` | Provide exactly one of `image_url` or `video_url`. Optional music description and model. The output is a separate MP3. The tool does not mix it into the video. |
| Generate a new vocal or instrumental track | `submit_audio_track_task`, `track` | Provide `audio_url`, a supported `generate_type` and a style `prompt`. Use `vocal_gender` only for `Vocals`. |
| Extend a song | `submit_song_edit_task`, `songextend` → `operation=extend` | Provide `audio_url`, lyrics, positive `extend_at_ms`, and `extend_type` of `head` or `tail`. |
| Remix a song | Same tool, `songremix` → `operation=remix` | Provide `audio_url`, lyrics and a new arrangement prompt. Source music must be MP3/M4A, 10–350 seconds, with a detectable beat. |
| Edit a section of a song | Same tool, `songedit` → `operation=edit` | Provide `audio_url`, lyrics and `edit_start_ms` / `edit_end_ms`. Allow a window of at least 3,000 ms and 12,000 ms of source music on either side. |
| Read text aloud | `submit_speech_task`, `text2audio` → `mode=tts` | Supply `content` and a supported built-in `voice`. The result is spoken MP3 audio. |
| Voice a scripted podcast | Same tool, `podcast` → `mode=podcast` | Supply ordered `conversations` with a supported voice and text for each turn. |
| Extract existing vocals and accompaniment | `submit_audio_separation_task`, `audiostem` | Supply `audio_url`. Deliver the two stems as a downloadable ZIP. |

Use integer milliseconds for audio time bounds and only the fields for the selected operation. Verify model-specific length, voice and source-format constraints before submission. Local inputs use the [upload workflow](references/media-upload.md). Pass the confirmed `file_url` or an existing accessible HTTPS source. A task ID cannot substitute for an audio URL. For video generation with reference sound or native output audio, use the video workflow.

## Choose generation quality before submitting

Compare the parameters you actually intend to send with the selected model and operation's recommended defaults. For a draft, mock-up, sample or ordinary request, keep the requested content, aspect ratio, duration and other independent constraints. Apply reported defaults for unspecified quality settings; if none is published, omit optional fields and ask only for missing required inputs. A style such as “cinematic” does not by itself select a higher tier. A default draft can still consume Fotor credits.

If the planned settings raise quality above the model's default and no applicable consent exists, pause **before** submitting. In the user's language name the concrete change and say it may use more Fotor credits. Use exactly these three choices in this order: **(1) Do not generate** (first, and the sole preselected option); **(2) Generate at the proposed setting**; **(3) Use model defaults for a draft**. Both generation choices start unselected. Name only the single proposed upgrade, leaving unrelated settings at their defaults. In a native form, explicitly mark only the first, no-submission choice as checked or selected; verify this before sending the form. If the host cannot express a safe default or has no selection UI, offer the same three choices in text and wait for a new user reply. Cancellation, timeout (even with a preselected value), silence or an unclear answer never authorizes a submission. An explicit choice authorizes exactly the selected operation; avoid a second confirmation for the same choice and retain native tool permissions.

Reuse a user's explicit batch or standing instruction only for the covered medium and settings. A new instruction such as “draft this one” overrides it. In another conversation reuse only an explicit instruction already available there; do not write memory or settings. Never invent a price, query a balance as a prerequisite, or change model or parameters to simulate a quality tier the operation does not offer.

When the audio operation offers no selectable fidelity control, keep its supported voice, format and required inputs. A request for a professional sound does not introduce a bitrate control. Stem separation keeps its existing submission workflow.

## Submit and deliver

Put the [credit notice](references/mcp-integration.md#credit-consumption-before-submission) in **visible chat text immediately before the submission tool call**, including for ordinary speech and quick voice demos with fixed quality. Say one sentence in the user's language, such as “Generating this audio may use Fotor credits.” Follow it with the authorized call in the same turn. For speech, keep the requested words in `content` and the notice in the conversation. A notice already shown for this operation, including in a higher-quality choice, covers this step.

Follow the [task result workflow](references/mcp-integration.md#submission-recovery-and-results) for immediate results, processing, terminal errors and uncertain submissions. Speech, podcasts and separation can complete in the first response. Report the actual status and deliver the returned MP3 or ZIP link. Preserve its format from the originating operation if a later query omits it. A completed response with no URL has no deliverable link.
