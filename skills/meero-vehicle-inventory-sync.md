---
name: carcutter-vehicle-inventory-sync
description: Keep a dealer inventory in sync with CarCutter — upsert vehicle records, read status, pull the list of vehicles still waiting to be photographed, and delete records.
api: Car-Cutter API
base_url: https://api.car-cutter.com
operations:
  - POST /vehicle/submission
  - POST /vehicle/list
  - GET /vehicle/status
  - GET /vehicle/shotlist
  - DELETE /vehicle/delete
generated: '2026-08-25'
method: generated
source: openapi/meero-carcutter-openapi.json
---

# Sync a dealer inventory into CarCutter

> **Note on identifiers.** The five vehicle operations in the published contract carry
> **no `operationId`**. They are referenced here by method and path, which is how they
> are actually addressable today. `overlays/meero-carcutter-overlay.yaml` proposes
> operationIds in CarCutter's own naming style.

## The key you choose is the key you live with

`vehicle_id` is caller-supplied and the contract advises **"Use the VIN if possible"**.
It is the natural key for everything else — images and feature annotations hang off it.
Pick it once, at intake, and never change it.

## Steps

1. **Upsert a vehicle** — `POST /vehicle/submission`, `multipart/form-data`, required
   field `vehicle_id`. Optional: `vehicle_manufacturer`, `vehicle_model`,
   `vehicle_model_ext`, `vehicle_color`, `vehicle_location`, `vehicle_lot`,
   `custom_reference`, `suggested_guideline_id`.
   This is the one **safely retryable** write in the API: "If a vehicle with that id
   already exists, it is updated and its status is set to new." Note the side effect —
   re-sending an unchanged record **resets `app_status` to `new`**, which will put the
   vehicle back on the shot list. Only send changes.

2. **Read one vehicle** — `GET /vehicle/status?vehicle_id=<id>`. Returns the `Vehicle`
   object; `app_status` is `new`, `active` or `deleted`.

3. **List vehicles** — `POST /vehicle/list`. There is **no pagination** — no limit,
   offset or cursor — so the response grows with the account. Budget for that.

4. **Find work to do** — `GET /vehicle/shotlist` returns the vehicles still awaiting
   capture. This is the natural driver for a photographer dispatch loop.

5. **Delete** — `DELETE /vehicle/delete`. This sets the vehicle to the `deleted` state.
   **There is no documented undo and no stated window.** Treat it as irreversible and
   confirm before calling.

## Field conventions to watch

- `app_status_updated` is formatted `2024/03/01, 09:59:55` — **not** ISO 8601. Parse it
  explicitly rather than handing it to a standard date parser.
- `suggested_guideline_id` and `scene_id` reference objects that have **no list
  operation** in the API. You cannot discover valid values programmatically; get them
  from your CarCutter account configuration.
- `custom_reference` is the only free-text correlation field. Use it to carry your own
  DMS/IMS record id.
