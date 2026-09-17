---
name: fotor-image
description: Generate or edit images with Fotor when the user chooses Fotor for image creation or changes to an existing image. Video requests belong to fotor-video; connection and model questions belong to fotor-connect.
---

# Fotor Image

Read [MCP integration and runtime workflow](../../references/mcp-integration.md) before execution. It defines model discovery, remote assets, submission recovery, and result delivery.

## Choose the image operation

Use `list_models` with `media_type: image` and the requested mode, then query the chosen `model_id` for its parameter details.

- **Generate:** Select `text_to_image` and call `submit_image_task` with no reference images (`image_urls` empty or omitted). Preserve the requested subject, composition, style, and supported output preferences.
- **Edit/reference:** Select `image_to_image` and supply the source HTTPS image URLs in `image_urls`. Describe the requested changes and what must remain consistent. If the source is missing or only a local path is available, resolve the asset input before submission.

Select `resolution`, `aspect_ratio`, and `quality` from the model details, using reported defaults for unspecified settings. The tool's optional `extra` fields support background and output-format preferences; send only values supported by the current schema and chosen model. Explain unsupported constraints rather than inventing masks, arbitrary dimensions, or a post-generation upscale step.

## Submit and deliver

Submit once through `submit_image_task`, retain the returned task ID, and use `get_task` according to the shared lifecycle. If submission is uncertain, preserve any known ID and avoid duplicate generation. Deliver the real completed image through an available preview or its result link; do not describe visual changes as verified until you have inspected the image.
