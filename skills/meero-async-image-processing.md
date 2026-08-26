---
name: carcutter-async-image-processing
description: Submit vehicle photos to the CarCutter (Meero/Diffusely) API for AI background replacement, cutting and retouching, then poll for completion and download the finished images.
api: Car-Cutter API
base_url: https://api.car-cutter.com
operations:
  - asyncSubmit
  - asyncStatus
  - asyncResult
generated: '2026-08-25'
method: generated
source: openapi/meero-carcutter-openapi.json
---

# Process vehicle images asynchronously with CarCutter

Use this for volume work. The synchronous path (`syncSubmit`) blocks on a single
image; this one enqueues a batch and lets you collect results later.

## Before you start

- You need a CarCutter bearer token. Send it as `Authorization: Bearer <token>` on
  every call. There is no OAuth flow and no scopes for this API.
- Every submission is **billable**. If the account credit balance is exhausted the
  API returns `402` with `"Credits exceeded. Please contact sales."` — that is a
  commercial quota, not a rate limit, and it carries no `Retry-After`.
- There is **no idempotency key** on this path. A retried submission is processed and
  charged again. Track what you have already submitted on your side.

## Steps

1. **Submit** — `POST /vehicle/image/submission` (`asyncSubmit`), `multipart/form-data`.
   Supply the image as an upload (`image`) or a URL (`image_url`), plus `cut_type`
   (`complete`, `normal`, `blur` or `none`). Optional controls include `guideline`,
   `scene_id`, `resolution` (e.g. `1600x1200`), `processing_speed` (`normal` or
   `lazy`), the `padding_*` family, and `blurring_license_plate`.
   A `200` returns an `ImageStatusResponse`: `data.images[]`, each with `image`,
   `angle`, `status`, `phase` and `quality`.

2. **Poll** — `GET /vehicle/image/status` (`asyncStatus`) until each image reaches a
   terminal state. `status` is one of `processing`, `undefined`, `raw`, `final`,
   `error`, `expired`, `unknown`; `phase` walks `downloading → analyzing → cutting →
   qa-ing → retouching → ready`. The contract warns that **more values may be added**,
   so treat any unrecognised value as non-terminal rather than failing.
   There is no webhook or callback — polling is the only completion signal.

3. **Download** — `GET /vehicle/image/result` (`asyncResult`) returns the processed
   image (`image/jpeg` by default; the output format is set in your account
   configuration). If multiple outputs are configured for one input, **only the first
   is returned**.

## Handling the responses

- `400` — invalid input. Check that exactly one of `image` / `image_url` is present
  and that `cut_type` is a valid enum value.
- `401` — bad or missing bearer token. Note that in practice this endpoint family
  returns an **HTML** 401 body, not the JSON `UnauthorizedResponse` the contract
  documents; do not assume the error body parses as JSON.
- `402` — out of credits. Stop; retrying will not help.
- `404` on `asyncResult` — no such image.
- `410` on `asyncResult` — the result **expired**. Results are retained for a limited
  time that CarCutter does not publish. Download promptly; if you get a `410` you must
  resubmit the source image (and pay again).
- Errors are a bespoke `{code, message}` object, not RFC 9457 problem details, and
  live responses have been observed nesting it under an `error` key. Parse defensively.

## What this API will not do

There is no cancel operation for an in-flight execution, no dry-run mode, and no
rate-limit headers to back off on.
