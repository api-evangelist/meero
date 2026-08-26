---
name: carcutter-vehicle-feature-hotspots
description: Create, read and remove CarCutter feature hotspots — the annotations that highlight equipment, upgrades and visible damage on a vehicle's interactive media.
api: Car-Cutter API
base_url: https://api.car-cutter.com
operations:
  - featureSubmit
  - featuresGet
  - featureGet
  - featureDelete
  - featuresDelete
generated: '2026-08-25'
method: generated
source: openapi/meero-carcutter-openapi.json
---

# Annotate a vehicle with feature hotspots

Hotspots are the callouts rendered over vehicle media by the CarCutter WebPlayer
(`@car-cutter/wc-webplayer` and its React/Next/Vue wrappers) — equipment, upgrades,
reconditioning, scratches, dents.

## Steps

1. **Submit features** — `POST /vehicle/feature/submission` (`featureSubmit`).
   The body accepts either a single `{vehicle: {...}}` or a batch
   `{vehicles: [{...}]}`. Each vehicle carries `id`, an `images[]` array
   (`url`, `match` of `url` or `filename`, `angle[]`) and a `features[]` array
   (`feature` name plus `description.short` and `description.long`).
   A `200` returns one entry per vehicle, each either `{vehicle_id, state: "enqueued"}`
   or `{vehicle_id, error: {...}}` — so **a 200 can still contain per-vehicle failures**.
   Iterate the array; do not treat the status code as the whole answer.
   Oversized batches return `413`.

2. **Read all features on a vehicle** — `GET /vehicle/features` (`featuresGet`).

3. **Read one feature** — `GET /vehicle/feature` (`featureGet`).

4. **Delete one feature** — `DELETE /vehicle/feature/delete` (`featureDelete`).

5. **Delete every feature on a vehicle** — `DELETE /vehicle/features/delete`
   (`featuresDelete`). This is the highest-consequence call in the contract: one
   `vehicle_id`, no confirmation parameter, no bulk restore, and the annotation text you
   wrote is not recoverable from the API afterwards. **Read the features first and keep
   your own copy before calling it.** Rebuilding means re-running `featureSubmit` with
   content you must already hold.

## Errors

`400` invalid input, `401` unauthorized, `413` too many vehicles in one submission,
`500` internal error — all as a bespoke `{code, message}` object. There is no RFC 9457
problem type and no machine-readable error code beyond the HTTP status.
