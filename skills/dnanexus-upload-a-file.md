---
name: Upload a file into a DNAnexus project safely
description: Create a file object with a nonce so retries cannot duplicate it, upload its parts, close it, and confirm it reached the closed state before anything downstream consumes it.
api: https://api.dnanexus.com
docs: https://documentation.dnanexus.com/developer/api/introduction-to-data-object-classes/files
method: generated
generated: '2026-08-04'
grounded_in: https://documentation.dnanexus.com/developer/api/api-directory
operations:
  - /file/new
  - /file-xxxx/upload
  - /file-xxxx/close
  - /file-xxxx/describe
  - /class-xxxx/setProperties
  - /class-xxxx/addTags
---

# Upload a file into a DNAnexus project

## Steps

1. **Create the file object — with a nonce.**
   `POST /file/new` with `{"project": "project-…", "name": "sample.bam", "folder": "/raw", "nonce": "<unique-string>"}`.
   The `nonce` is what makes this call safe to retry: reissuing the identical request within one hour returns an exact replica of the original response instead of creating a second file. The nonce must be ≤128 bytes UTF-8, and the rest of the input must be byte-for-byte identical on the retry — any change returns `InvalidInput` (422).
   The response carries the new `file-xxxx` ID; the object is in the `open` state.

2. **Request an upload URL per part.**
   `POST /file-xxxx/upload` with `{"size": <bytes>, "md5": "<hex>", "index": <part number>}`. The response gives the URL and headers to `PUT` that part to. Repeat per part for a multipart upload.
   Maximum file size is 5 TB on Amazon regions and 4.75 TB on Azure regions.

3. **Close the file.**
   `POST /file-xxxx/close` with `{}`. The object moves `open` → `closing` → `closed`.

4. **Wait for `closed` before using it.**
   Poll `POST /file-xxxx/describe` with `{}` until `state` is `closed`. A file in `closing` cannot be used as job input, and calling a method that requires a closed object earlier returns `InvalidState` (422).

5. **Tag and annotate (optional).**
   `POST /class-xxxx/setProperties` with `{"properties": {"sample": "NA12878"}}` and `POST /class-xxxx/addTags` with `{"tags": ["validated"]}` on the file ID, so the object is findable later via `/system/findDataObjects`.

## Rules

- **Always** send a `nonce` on `/file/new`. It is the only documented way to make creation idempotent across a dropped response.
- The one-hour replay guarantee is the ceiling. After an hour the same nonce may create a new object.
- `PermissionDenied` (401) here usually means the token has only `VIEW` on the project; uploading needs at least `UPLOAD`.
- On `5xx`, retry with the same nonce and the same body.
