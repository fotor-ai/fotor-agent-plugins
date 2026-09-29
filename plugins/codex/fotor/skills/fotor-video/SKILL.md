---
name: fotor-video
description: Generate videos with Fotor from text, first or last frames, or supported image/video/audio references. Still-image requests belong to fotor-image; website access, connection, and model questions belong to fotor-connect.
---

# Fotor Video

Read [MCP integration and runtime workflow](../../references/mcp-integration.md) before execution. It defines discovery, task recovery, and the one-time website handoff. For local references or client-accessible attachments, follow the shared [media upload workflow](../../references/media-upload.md); use existing accessible HTTPS references directly.

## Choose the workflow

For a generation request, use `list_models` with `media_type: video` and the intended mode, then query the selected `model_id` for details.

| Input and intent | Mode | Submission tool and inputs |
| --- | --- | --- |
| Text prompt | `text_to_video` | `submit_text_to_video_task` |
| Starting image and motion | `first_frame_video` | `submit_first_frame_video_task`, `image_url` |
| Ordered starting and ending images | `first_last_frame_video` | `submit_first_last_frame_video_task`, `start_image_url` and `end_image_url` |
| Image, video, and/or audio references | `multimodal_reference_video` | `submit_multimodal_video_task`, supported URL arrays |

Preserve the user's subject, action, camera motion, and source constraints. Obtain missing assets before submission. Reference-guided generation can use an existing video, but these tools do not establish timeline editing, trimming, precise time-range replacement, or post-generation dubbing.

## Match the model contract

- Use supported native resolutions, aspect ratios, and duration values. `duration=0` selects the model's default duration; explain an unsupported explicit request before changing it.
- Supply a concrete supported `aspect_ratio` for first-frame and first/last-frame tools. Honor the model's `explicit_aspect_ratio_modes`; multimodal inputs containing images but no videos also require checking this rule.
- For multimodal mode, supply at least one reference and check `reference_types`. An empty list means multimodal reference video is unsupported. Preserve reference order and send accessible HTTPS URLs. Construct the prompt with the [reference markers](#reference-markers-in-video-prompts) below.
- `audio_urls` provides reference audio; `audio_enable` independently requests generated output audio. Enable output audio only when `native_audio` confirms support. Reference audio does not imply output audio support.
- Use at most eight audio references and honor any smaller `max_audio_references` value. When `audio_requires_visual_reference` is true, audio must accompany an image or video. Encode literal commas in audio URLs as `%2C` without re-encoding already escaped values.

## Reference markers in video prompts

For `multimodal_reference_video`, identify reference materials in the submitted `prompt` with these exact markers:

| Reference type | Marker | First reference |
| --- | --- | --- |
| Image | `<<<image_n>>>` | `<<<image_1>>>` |
| Video | `<<<video_n>>>` | `<<<video_1>>>` |
| Audio | `<<<audio_n>>>` | `<<<audio_1>>>` |

`n` is a decimal index starting at **1**, numbered separately for each media type in its submitted reference order. Keep the lowercase type, underscore, and three angle brackets on each side literal in the tool's prompt text. The markers refer to the media supplied through the tool's URL inputs; those inputs still carry the actual accessible HTTPS URLs.

Before submission, verify that every marker maps to a supplied reference of the matching type. If the selected references or their order change, update the prompt's mapping. Resolve a missing reference before submitting a prompt that uses its marker.

Example for a model supporting two images, one video, and one audio reference:

```text
Use the character in <<<image_1>>> and the setting in <<<image_2>>>. Follow the camera motion in <<<video_1>>> and the rhythm of <<<audio_1>>>.
```

## Finish the request

Submit once, retain the returned ID, and follow the shared `get_task` lifecycle, including `submission_uncertain` recovery and the returned `credits_increment`. Upload success alone does not establish generation success. Deliver the actual completed result; a submitted or processing task is not a finished video. Report audio/visual quality only after inspection.

Website access belongs to `fotor-connect`. For a request that also includes connecting to the Fotor website, follow the shared [website connection flow](../../references/mcp-integration.md#connect-to-the-website); navigation and generation remain separate requested operations.
