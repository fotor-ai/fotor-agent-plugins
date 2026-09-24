---
name: fotor-image
description: Generate, edit, upscale, or remove image backgrounds with Fotor. Use when the user chooses Fotor for image creation or processing; standalone uploads belong to fotor-upload and video creation belongs to fotor-video.
---

# Fotor Image

Read [MCP integration and runtime workflow](../../references/mcp-integration.md) before execution. It defines authentication, task recovery, and result delivery. For local images or accessible attachments, follow the shared [media upload workflow](../../references/media-upload.md); use an existing accessible HTTPS source directly.

## Choose the image operation

| Intent | Tool and inputs | Model selection |
| --- | --- | --- |
| Generate an image | `submit_image_task`, omit or leave `image_urls` empty | Query `list_models` for `media_type: image`, mode `text_to_image`, then the chosen ID's details |
| Edit an image, replace its background, or use image references | `submit_image_task` with `image_urls` and the requested changes | Query image mode `image_to_image`, then the chosen ID's details |
| Enlarge/upscale an existing image | `submit_image_upscale_task` with `image_url`, optional `upscale_ratio` | No model ID or prompt |
| Remove the background to create a transparent image | `submit_background_removal_task` with `image_url` | No model ID, prompt, or mode |

Discover the required tool before proceeding. Standalone image processing does not require a generation model lookup. Removing a background and replacing it with a new scene are different requests; use semantic editing for the latter. Resolve missing source images before submission.

## Match the operation's limits

For generation/editing, preserve the requested subject, composition, style, and consistency constraints. Select `resolution`, `aspect_ratio`, and `quality` from the model details, using reported defaults for unspecified settings. The optional `extra` fields support background and output-format preferences only when supported by the current schema and model. Explain unsupported masks, arbitrary dimensions, or other constraints before changing the request.

For standalone upscaling, `upscale_ratio` defaults to `2.0` and must be finite and positive. The current tool caps output width and height at 2048 pixels. Explain requests exceeding that limit before submitting; a requested multiplier does not guarantee dimensions beyond the cap. Background removal takes only the source URL and returns an asynchronous task for a transparent result. Do not invent provider, resolution, or output-format controls for these fixed operations.

## Submit and deliver

Submit the requested operation once, retain its returned task ID and model or operation, and use `get_task` according to the shared lifecycle. `submission_uncertain` may mean the task was created and charged; preserve any known ID and avoid automatic resubmission. Report `credits_increment` only as returned, including zero or unknown values.

For an explicitly requested sequence, such as generate then upscale, finish the first task and obtain its actual result URL before submitting the next operation. Preserve each task ID and stop the chain if a required result is missing or failed. Generation alone does not request automatic upscaling or background removal.

Deliver the real completed image through an available preview or result link. Describe visual quality or transparency as verified only after inspecting the result.
