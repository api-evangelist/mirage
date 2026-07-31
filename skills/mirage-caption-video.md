---
name: Add styled captions to a video with Mirage
description: Pick a caption template, submit an existing video for captioning, poll to completion, and download it.
api: openapi/mirage-openapi-original.json
operations:
  - list_caption_templates_v1_videos_captions_templates_get
  - get_caption_template_v1_videos_captions_templates__template_id__get
  - create_captioned_video_v1_videos_captions_post
  - get_video_status_v1_videos__video_id__get
  - get_video_content_v1_videos__video_id__content_get
---

# Caption a video with Mirage

## Auth
Send `x-api-key: <YOUR_API_KEY>` on every request. Base URL: `https://api.mirage.app`.

## Steps
1. **Choose a caption style** — `list_caption_templates_v1_videos_captions_templates_get`
   (`GET /v1/videos/captions/templates`) to browse templates, and optionally
   `get_caption_template_v1_videos_captions_templates__template_id__get`
   (`GET /v1/videos/captions/templates/{template_id}`) to inspect one. Note the
   template id (e.g. `ctpl_123456789abcdefg`).
2. **Submit the captioning job** — `create_captioned_video_v1_videos_captions_post`
   (`POST /v1/videos/captions`) with the source video and the chosen
   `caption_template_id`. The response is an `MAVideo` with `status: PROCESSING`;
   its `source_video_id` echoes the input and `caption_template_id` the style.
3. **Poll** — `get_video_status_v1_videos__video_id__get` until `status` is
   `COMPLETE`.
4. **Download** — `get_video_content_v1_videos__video_id__content_get`.

## Error handling
Same envelope as generation: `{code, message}` on failure (`rate_limit_exceeded`
= back off), HTTP 422 for request-validation errors. No idempotency key.
