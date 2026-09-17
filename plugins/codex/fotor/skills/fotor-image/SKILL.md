---
name: fotor-image
description: Generate or edit images with Fotor when the user chooses Fotor for image creation or changes to an existing image. Video requests belong to fotor-video.
---

# Fotor Image

Before executing a Fotor image request, read [MCP integration and runtime workflow](../../references/mcp-integration.md). It defines connection discovery, asset handling, task recovery, and result delivery.

## Define the operation

- **Generate:** Turn the user's subject, composition, style, and output preferences into inputs supported by the available image-generation tool. Preserve explicit dimensions or aspect ratio. Ask only for information required by the tool or essential to the requested result.
- **Edit:** Identify the source image, requested changes, and elements to preserve. Use the editing operation and any masks or reference inputs it actually supports. Ask for the source image if it is missing; keep unrelated content consistent with the user's request.

Select supported settings from the actual tool schema. If a requested edit or format is unavailable, explain that limitation before proposing a supported option. Treat an existing image edit as an edit to the provided asset rather than an unrelated new generation.

## Complete the request

Follow the shared runtime workflow through completion. Show the returned image when the host supports it, otherwise provide its result link. Describe changes based on the tool result and any image you inspected. If execution is unavailable, return a clearly labeled preparation brief and identify the missing capability.
