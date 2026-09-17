---
name: fotor-video
description: Generate or edit videos with Fotor when the user chooses Fotor for video creation, image-to-video, or changes to an existing video. Still-image requests belong to fotor-image.
---

# Fotor Video

Before executing a Fotor video request, read [MCP integration and runtime workflow](../../references/mcp-integration.md). It defines connection discovery, asset handling, task recovery, and result delivery.

## Define the operation

- **Generate:** Identify the subject, action, visual style, and camera behavior. Carry through requested duration, aspect ratio, and audio preferences only using supported parameters.
- **Animate an image:** Identify the source image and intended motion. Use this mode only when the connected Fotor tools support image-to-video, and preserve the user's stated appearance constraints.
- **Edit:** Identify the source video, requested modifications, and relevant time ranges. Keep requested duration, audio, and unchanged sections intact where the tool supports those constraints. Explain capability limits that would change the requested result.

Obtain missing source assets and required settings before submission. Distinguish creating a new clip from editing an existing one when choosing the tool; a text prompt alone does not establish that a tool supports video editing.

## Complete the request

Follow the shared runtime workflow, retaining the task identifier for asynchronous operations. Return the completed video through an available preview or its result link. Report the actual job state if it is still running or failed, and describe visual or audio quality only when you inspected it. If execution is unavailable, provide a clearly labeled preparation brief and identify the missing capability.
