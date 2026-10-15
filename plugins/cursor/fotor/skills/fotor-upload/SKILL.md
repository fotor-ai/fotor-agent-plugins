---
name: fotor-upload
description: Upload local images, videos, or audio to Fotor and return usable media URLs. Use for standalone Fotor uploads or preparing local references for Fotor image and video tasks.
---

# Fotor Upload

Follow the shared [media upload workflow](../../references/media-upload.md) for file checks, authentication, byte transfer, bounded recovery, and delivery. A returned upload address is not a completed upload.

For a standalone request, finish with each file's name, media type, byte size, and successfully uploaded `file_url`; report failures separately. Uploading alone does not request media generation or website navigation.

For a combined creation request, preserve the file-to-URL mapping and reference order, then continue the requested image or video workflow with successfully uploaded inputs. Resolve missing required inputs before submitting that task.
