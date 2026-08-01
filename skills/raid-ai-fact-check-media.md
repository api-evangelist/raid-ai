---
name: Fact-check media against the public record
description: Submit an image, audio, or video to the Raid AI asynchronous fact-checking API and poll for the result.
api: docs.raidxai.com/docs/reference
operations: [factCheckingSubmit, factCheckingSubmitFromUrl, factCheckingGetJob]
generated: '2026-07-20'
method: generated
---

# Fact-check media against the public record

Check whether an image, audio, or video matches the public record. Asynchronous:
submit, get a `jobId`, then poll until the job completes.

## Prerequisites
- A Raid AI API key with the **fact-check** scope. Send as `Authorization: Bearer <token>`.
- Base URL: `https://api.raidxai.com`. Max 50 MB per file.

## Steps
1. **Submit the media.**
   - Uploaded file: `factCheckingSubmit` — `POST /api/app/fact-checking/submit`.
   - From a URL (article, social post, media link): `factCheckingSubmitFromUrl` —
     `POST /api/app/fact-checking/submit-from-url`.
   - The response contains a `jobId`.
2. **Poll the job.** Call `factCheckingGetJob` —
   `GET /api/app/fact-checking/jobs/{id}` — until `status` is `Completed`.
   Use polite polling intervals; retry `429` and transient `5xx` with backoff + jitter.
3. **Read the result** once the job is `Completed`.

## Notes
- This is a submit-then-poll flow (no idempotency key; no webhooks documented on
  the public API surface).
- `401 UNAUTHORIZED` for a bad key, `403 FORBIDDEN` if the key lacks the
  `fact-check` scope.
