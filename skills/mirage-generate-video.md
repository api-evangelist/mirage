---
name: Generate an expressive human video with Mirage
description: Submit an image + audio to Mirage Video 1, poll the job to completion, and download the rendered video.
api: openapi/mirage-openapi-original.json
operations:
  - create_video_generation_v1_videos_post
  - get_video_status_v1_videos__video_id__get
  - get_video_content_v1_videos__video_id__content_get
---

# Generate a video with Mirage

Use the Mirage Video API to turn an image appearance reference plus an audio
track into an expressive human video (model `mirage-video-1-latest`).

## Auth
Every request sends the header `x-api-key: <YOUR_API_KEY>` (issued from the
dashboard). Base URL: `https://api.mirage.app`.

## Steps
1. **Submit the job** — `create_video_generation_v1_videos_post`
   (`POST /v1/videos`). Send multipart body with `audio_reference` (WAV or MP3,
   required), `image_reference` (JPEG or PNG, required), and `model`
   (`mirage-video-1-latest`). The response is an `MAVideo` with `status:
   PROCESSING` and an `id` (e.g. `video_abc123def456`). Keep the `id`.
2. **Poll for completion** — `get_video_status_v1_videos__video_id__get`
   (`GET /v1/videos/{video_id}`). Repeat with backoff, reading `status` and
   `progress` (0–100). Stop when `status` is `COMPLETE` (or `FAILED` /
   `CANCELLED`). This API is submit-and-poll; there are no webhooks.
3. **Download the result** — `get_video_content_v1_videos__video_id__content_get`
   (`GET /v1/videos/{video_id}/content`) once `status` is `COMPLETE`.

## Error handling
- On `status: FAILED`, read `error.code` / `error.message` (MAVideoError). A
  `rate_limit_exceeded` code means back off and retry.
- Request validation failures return HTTP 422 with `detail[]`.
- There is no idempotency key — do not blindly retry a submit, or you create a
  duplicate job.
