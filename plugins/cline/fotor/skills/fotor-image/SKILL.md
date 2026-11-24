---
name: fotor-image
description: Generate, edit, upscale, or remove image backgrounds with Fotor. Resolve above-default image quality before tool approval. Use fotor-upload for standalone uploads and fotor-video for video creation.
---

# Fotor Image

Read [MCP integration and runtime workflow](references/mcp-integration.md) before execution. It defines authentication, task recovery, and result delivery. For local images or accessible attachments, follow the shared [media upload workflow](references/media-upload.md); use an existing accessible HTTPS source directly.

## Choose generation quality before submitting

For image generation and editing, look up the chosen model and operation's
recommended defaults. Compare the quality you actually plan to send with those
defaults. Keep the user's subject, text and aspect ratio whichever quality they
choose. A draft or ordinary request uses the recommended quality by default.
Omit optional parameters with unknown defaults and ask only for missing required
values. Use the operation's supported settings.

**When the proposed tier exceeds the recommended default, obtain a quality
choice before calling `submit_image_task` or opening Cline's tool approval.**
A one-off request such as “generate this at 4K” specifies a resolution. If the
user has yet to accept the additional credit use and has no matching batch or
standing preference, the next action is a question. In the user's language,
name the default and the proposed tier and explain that the higher tier may
use more Fotor credits. **End this turn at the question.**

For example, when the reported default is 1K, ask in the user's language:

> 4K may use more Fotor credits than the default 1K. Which would you prefer?
>
> 1. Do not generate.
> 2. Generate at 4K.
> 3. Use 1K for a draft.

Use the actual tiers in this order. If Cline's question form lets you preselect
only “Do not generate,” use it. Otherwise send the three options as chat text.
A preselection, silence, timeout or unclear reply leaves the task unsubmitted.
After an explicit reply, submit once at the chosen tier, or stop on cancellation.
A draft keeps the requested frame even if the model defaults to a different frame.

An explicit acceptance of the extra credit use for this image, or an instruction
such as “use 4K for this batch,” covers the specified image(s) and settings.
Reuse it without asking again. A later request for a draft takes precedence.
Across conversations, use only an explicit instruction already available there.
Leave memory and configuration unchanged. Never estimate a price or query a
balance to make this decision. Standalone upscaling and background removal
follow their own workflows.

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

## Submit and deliver

Build the final `submit_image_task` arguments from the selected model's defaults for `resolution`, `quality`, and `aspect_ratio`. Apply each requested or approved change to its own field. “Always use 4K for my images” sets only `resolution`. If the user also specifies a shape or ratio, use it. Otherwise compose the scene within the model's default frame. Omit optional `aspect_ratio` when omission applies that default, or pass the reported default if a value is required.

For an ordinary or draft image requested at default quality, give the
[credit notice](references/mcp-integration.md#credit-consumption-before-submission)
in visible chat immediately before submitting, then proceed. For an above-default
image, the quality question above takes precedence over the general instruction
to continue after a credit notice. Only a covered preference or an informed user
choice permits the submission tool call. Cline's ordinary tool approval follows
that choice. An informed choice covers the credit notice for that submission.

Submit the requested operation once, retain its returned task ID and model or operation, and use `get_task` according to the shared lifecycle. `submission_uncertain` may mean the task was created and charged; preserve any known ID and avoid automatic resubmission. Report `credits_increment` only as returned, including zero or unknown values.

For an explicit `insufficient_credits` error from any image operation, follow the shared [submission credit-error workflow](references/credits.md#insufficient-credits-during-media-submission) to report it under this package's credit policy without retrying the submission.

For an explicitly requested sequence, such as generate then upscale, finish the first task and obtain its actual result URL before submitting the next operation. Preserve each task ID and stop the chain if a required result is missing or failed. Generation alone does not request automatic upscaling or background removal.

Deliver the real completed image through an available preview or result link. Describe visual quality or transparency as verified only after inspecting the result.
