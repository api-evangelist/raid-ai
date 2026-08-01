---
name: Detect an AI-generated or cloned voice
description: Use the Raid AI voice-analysis API to detect AI-generated or cloned speech, with optional transcription and intelligence analysis.
api: docs.raidxai.com/docs/reference
operations: [voiceAnalysisProcess, voiceAnalysisProcessFromUrl]
generated: '2026-07-20'
method: generated
---

# Detect an AI-generated or cloned voice

Analyze an audio file and determine whether the voice is real or AI-generated /
cloned. Synchronous.

## Prerequisites
- A Raid AI API key with the **audio** scope. Send as `Authorization: Bearer <token>`.
- Base URL: `https://api.raidxai.com`. Formats: `wav, mp3, flac, m4a, opus`, max 50 MB.

## Steps
1. **Choose the workflow.** `input.WorkflowType 4` (default) detects AI-generated /
   cloned voice; other workflows transcribe speech, run intelligence analysis, or
   combine both.
2. **Call the endpoint.**
   - Uploaded file: `voiceAnalysisProcess` — `POST /api/app/voice-analysis/process`.
   - From a URL (direct audio link or video/social page whose audio track is
     extracted): `voiceAnalysisProcessFromUrl` — `POST /api/app/voice-analysis/process-from-url`.
   ```bash
   curl -X POST "https://api.raidxai.com/api/app/voice-analysis/process" \
     -H "Authorization: Bearer $RAID_TOKEN" \
     -F "audioFile=@/path/to/clip.wav"
   ```
3. **Read the verdict** and confidence score; use them to flag synthetic speech.

## Error & rate handling
- `401 UNAUTHORIZED` (bad key), `403 FORBIDDEN` (missing `audio` scope),
  `429 TOO_MANY_REQUESTS` (back off with jitter). Call server-side only.
