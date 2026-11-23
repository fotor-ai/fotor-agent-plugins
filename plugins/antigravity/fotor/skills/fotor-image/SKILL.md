---
name: fotor-image
description: Generate, edit, upscale, or remove image backgrounds with Fotor. Use when the user chooses Fotor for image creation or processing; standalone uploads belong to fotor-upload and video creation belongs to fotor-video.
---

# Fotor Image

Read [MCP integration and runtime workflow](references/mcp-integration.md) before execution. It defines authentication, task recovery, and result delivery. For local images or accessible attachments, follow the shared [media upload workflow](references/media-upload.md); use an existing accessible HTTPS source directly.

## Choose the image operation

| Intent | Tool and inputs | Model selection |
| --- | --- | --- |
| Generate an image | `submit_image_task`, omit or leave `image_urls` empty | Query `list_models` for `media_type: image`, mode `text_to_image`, then the chosen ID's details |
| Edit an image, replace its background, or use image references | `submit_image_task` with `image_urls` and the requested changes | Query image mode `image_to_image`, then the chosen ID's details |
| Enlarge/upscale an existing image | `submit_image_upscale_task` with `image_url`, optional `upscale_ratio` | No model ID or prompt |
| Remove the background to create a transparent image | `submit_background_removal_task` with `image_url` | No model ID, prompt, or mode |

Discover the required tool before proceeding. Standalone image processing does not require a generation model lookup. Removing a background and replacing it with a new scene are different requests; use semantic editing for the latter. Resolve missing source images before submission.

## Match the operation's limits

For generation/editing, preserve the requested subject, composition, style, and consistency constraints. The optional `extra` fields support background and output-format preferences only when supported by the current schema and model. Explain unsupported masks, arbitrary dimensions, or other constraints before changing the request.

For standalone upscaling, `upscale_ratio` defaults to `2.0` and must be finite and positive. The current tool caps output width and height at 2048 pixels. Explain requests exceeding that limit before submitting; a requested multiplier does not guarantee dimensions beyond the cap. Background removal takes only the source URL and returns an asynchronous task for a transparent result. Do not invent provider, resolution, or output-format controls for these fixed operations.

## Choose generation quality before submitting

Compare the parameters you actually intend to send with the selected model and operation's recommended defaults. For a draft, mock-up, sample or ordinary request, keep the requested content, aspect ratio, duration and other independent constraints. Apply reported defaults for unspecified quality settings; if none is published, omit optional fields and ask only for missing required inputs. A style such as “cinematic” does not by itself select a higher tier. A default draft can still consume Fotor credits.

If the planned settings raise quality above the model's default and no applicable consent exists, pause **before** submitting. In the user's language name the concrete change and say it may use more Fotor credits. Use exactly these three choices in this order: **(1) Do not generate** (first, and the sole preselected option); **(2) Generate at the proposed setting**; **(3) Use model defaults for a draft**. Both generation choices start unselected. Name only the single proposed upgrade, leaving unrelated settings at their defaults. In a native form, explicitly mark only the first, no-submission choice as checked or selected; verify this before sending the form. If the host cannot express a safe default or has no selection UI, offer the same three choices in text and wait for a new user reply. Cancellation, timeout (even with a preselected value), silence or an unclear answer never authorizes a submission. An explicit choice authorizes exactly the selected operation; avoid a second confirmation for the same choice and retain native tool permissions.

Reuse a user's explicit batch or standing instruction only for the covered medium and settings. A new instruction such as “draft this one” overrides it. In another conversation reuse only an explicit instruction already available there; do not write memory or settings. Never invent a price, query a balance as a prerequisite, or change model or parameters to simulate a quality tier the operation does not offer.

Use the actual model resolution or quality tier in the choice, for example 4K versus its reported default. Standalone upscaling and background removal keep their existing submission workflow.

## Submit and deliver

Build the final `submit_image_task` arguments from the selected model's defaults for `resolution`, `quality`, and `aspect_ratio`. Apply each requested or approved change to its own field. “Always use 4K for my images” sets only `resolution`. If the user also specifies a shape or ratio, use it. Otherwise compose the scene within the model's default frame. Omit optional `aspect_ratio` when omission applies that default, or pass the reported default if a value is required.

Put the [credit notice](references/mcp-integration.md#credit-consumption-before-submission) in **visible chat text immediately before the submission tool call**. For an ordinary image or a draft, say one sentence in the user's language, such as “Generating this image may use Fotor credits.” Follow it with the authorized call in the same turn. When a one-off draft overrides a batch's approved resolution, give the brief notice for that draft. An informed batch instruction or an earlier quality choice covers the images and settings it names.

Submit the requested operation once, retain its returned task ID and model or operation, and use `get_task` according to the shared lifecycle. `submission_uncertain` may mean the task was created and charged; preserve any known ID and avoid automatic resubmission. Report `credits_increment` only as returned, including zero or unknown values.

For an explicit `insufficient_credits` error from any image operation, follow the shared [submission credit-error workflow](references/credits.md#insufficient-credits-during-media-submission) to report it under this package's credit policy without retrying the submission.

For an explicitly requested sequence, such as generate then upscale, finish the first task and obtain its actual result URL before submitting the next operation. Preserve each task ID and stop the chain if a required result is missing or failed. Generation alone does not request automatic upscaling or background removal.

Deliver the real completed image through an available preview or result link. Describe visual quality or transparency as verified only after inspecting the result.
