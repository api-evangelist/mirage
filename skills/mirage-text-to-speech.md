---
name: Synthesize speech with Mirage text-to-speech
description: Convert text to speech with a chosen Mirage voice using the mirage-audio-1 model.
api: openapi/mirage-openapi-original.json
operations:
  - generate_text_to_speech_v1_audio_text_to_speech__voice_id__post
---

# Text-to-speech with Mirage

## Auth
Send `x-api-key: <YOUR_API_KEY>` on every request. Base URL: `https://api.mirage.app`.

## Steps
1. **Generate speech** — `generate_text_to_speech_v1_audio_text_to_speech__voice_id__post`
   (`POST /v1/audio/text-to-speech/{voice_id}`). Path param `voice_id` selects
   the voice. Body (`TTSRequest`): `text` (the text to speak) and `model`
   (`mirage-audio-1`).
2. Handle the returned audio job/output per the response schema.

## Error handling
Request-validation errors return HTTP 422 with `detail[]`; a
`rate_limit_exceeded` error code means back off and retry. No idempotency key —
avoid blind retries.
