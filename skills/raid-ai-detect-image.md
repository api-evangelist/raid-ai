---
name: Detect an AI-generated or deepfaked image
description: Use the Raid AI image-forensics API to get a verdict, confidence, and (on detailed plans) generator attribution for one or more images.
api: docs.raidxai.com/docs/reference
operations: [imageForensicsProcess, imageForensicsProcessFromUrl]
generated: '2026-07-20'
method: generated
---

# Detect an AI-generated or deepfaked image

Determine whether an image is authentic, AI-generated, deepfaked, or digitally
edited using the Raid AI image forensics API. Synchronous — the verdict comes
back in the same response.

## Prerequisites
- A Raid AI API key with the **image** scope (dashboard -> API Keys -> Create Key).
- Send it as `Authorization: Bearer <token>`. A missing/invalid key returns
  `401 UNAUTHORIZED`; a key without the `image` scope returns `403 FORBIDDEN`.
- Base URL: `https://api.raidxai.com`.

## Steps
1. **Have image bytes or a URL.** Formats: `jpg, jpeg, png, webp, heic, heif, tiff`.
   Up to 10 images per request, max 50 MB each.
2. **Call the endpoint.**
   - For uploaded files use `imageForensicsProcess` —
     `POST /api/app/image-forensics/process`, multipart with `imageFiles=@photo.jpg`.
   - For a public image or social/media page URL use `imageForensicsProcessFromUrl` —
     `POST /api/app/image-forensics/process-from-url`.
   ```bash
   curl -X POST "https://api.raidxai.com/api/app/image-forensics/process" \
     -H "Authorization: Bearer $RAID_TOKEN" \
     -F "imageFiles=@/path/to/photo.jpg"
   ```
3. **Read the response.** You get a `verdict`, a `confidence` score, and per-image
   details (on detailed plans, generator attribution + a deep-analysis breakdown).
4. **Tolerate additive changes.** Ignore unknown fields and accept new enum values
   (e.g. new verdict or generator names) — the API evolves additively.

## Error & rate handling
- Retry `429 TOO_MANY_REQUESTS` (empty body) and transient `5xx` with exponential
  backoff + jitter; rate limits are per-key.
- Call from your server only — the token carries full account permissions and credit balance.
