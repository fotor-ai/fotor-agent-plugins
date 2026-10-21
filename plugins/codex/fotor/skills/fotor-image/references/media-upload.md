# Fotor Media Upload

Read this workflow for standalone uploads and when an image or video task needs local references. Use already accessible HTTPS references directly for creation; upload only the specific local files or client-accessible attachments needed by the user's request. An attachment label without readable bytes is not an uploadable file.

## Prepare the input

1. Follow [MCP authentication](mcp-integration.md#authenticate-mcp), preserving the installed environment. Discover `get_upload_url` before attempting an upload. Uploads need no model selection and can be available even when model discovery or generation tools are absent. An upload request does not request a website handoff.
2. Confirm the file can be read by the client that will send its bytes. Determine its basename, actual media type, and exact nonzero byte size. Pass only the basename, including its extension, as `file_name`, and an integer as `size_bytes`.
3. Check the service's supported type and configured size limit. The current upload contract accepts the extensions below, case-insensitively; SVG, documents, and archives are unsupported. An extension determines the signed Content-Type, but does not validate the file's contents or prove that a selected generation model accepts it. Apply the model's additional format/reference constraints when preparing creation inputs.

| Media | Supported extensions |
| --- | --- |
| Image | `jpg`, `jpeg`, `png`, `webp`, `gif`, `bmp`, `avif`, `heic`, `heif` |
| Video | `mp4`, `mov`, `webm`, `m4v`, `avi`, `mkv` |
| Audio | `mp3`, `wav`, `m4a`, `aac`, `ogg`, `flac`, `opus` |

Service limits can change. Report a size/type rejection before continuing; do not silently convert, compress, or replace the user's file. If the tool is absent or storage is unconfigured, explain that upload is unavailable. For creation, an appropriate accessible HTTPS reference is a possible user-supplied alternative.

## Transfer and confirm

1. Call `get_upload_url` with the checked `file_name` and `size_bytes` only when the client is ready to send the file. Require the returned `upload_url`, `file_url`, `method`, `headers`, and positive `expires_in`; the current method is `PUT`.
2. Use a client-supported HTTP/file-transfer capability to PUT the unchanged raw bytes to `upload_url`, applying only the returned headers. Send neither multipart/form-data nor Fotor OAuth credentials. A browser using File/Blob supplies Content-Length itself; a shell/HTTP client must preserve the exact signed byte count and Content-Type. Do not fetch, preview, or follow a redirect from the signed upload address.
3. Keep signed URLs in transient tool state. Avoid command tracing, verbose HTTP output, and errors that echo the full signed URL; never record it in repository files or diagnostic logs. Use the returned address unchanged rather than reconstructing its signature or host.
4. Confirm an actual upload response with a 2xx status before marking the file uploaded or passing `file_url` to another tool. Requesting the address alone does not transfer bytes, and a returned `file_url` is not proof that the object exists. Report inability to send the file when the host lacks the required capability.

If the file changes after signing, recalculate its metadata and request a new link. A timeout with no response leaves upload success unconfirmed. Within the active request, allow at most one retry per file after resolving the cause: PUT the same unchanged bytes to the still-valid address, or obtain a new link if expired. The upload address permits repeated PUTs to the same object during its lifetime; it is distinct from the one-time website handoff. Stop after another failure and report the file as unsuccessful; do not let dependent creation proceed with that URL. This upload retry rule does not authorize retrying an uncertain media-task submission.

## Deliver or continue

Process multiple files one at a time, preserving input order and the association between each file and its successful URL. Report name, media type, exact byte size, and `file_url` for successes; report a concise reason for each failure without exposing signatures. A partial batch is not an all-success result. If a creation task needs a failed input, resolve it before submission rather than silently omitting it.

For standalone uploads, deliver those results and finish. For requested creation, use the uploaded URLs in the appropriate reference fields or image-processing `image_url`, keeping the same environment. File acceptance by storage does not establish model compatibility or media-task success.

`expires_in` describes the maximum upload-address lifetime; temporary signing credentials may expire earlier. It does not describe file retention. Signature expiry does not delete an uploaded file, and `file_url` is not a presigned download URL. Do not promise permanent storage, a deletion time, or long-term URL availability unless the service provides that information.
