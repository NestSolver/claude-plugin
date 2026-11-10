---
name: nest-parts
description: Nests irregular parts on sheets with NestSolver (true-shape nesting), using parts from DXF, DWG or SVG drawings (shared in the chat or saved before) plus rectangles and circles, with part spacing, rotation and common-line cutting, for laser, plasma, waterjet, router or knife cutting. Use when someone wants to nest parts, shares a drawing to nest or cut, fits shapes on sheet metal or other sheets, or asks how many sheets their parts need.
---

# Nest parts with NestSolver

A nest runs in the background: start it, wait for it to finish, then read the plan.

## 1. Find the parts

- **A drawing shared in the chat** (DXF, DWG or SVG): import it with `import_parts`, sending its `filename` and `content` (a DXF's or SVG's text, or a DWG's bytes in base64 with `encoding: "base64"`), and `unit` when the person says it. It saves every closed outline as a part and answers each part's `id`, size and holes; those ids go in the nest's `library`. Each call saves new parts, so import a drawing once.
- **Saved parts** come from drawings imported before, here or in NestSolver's web app. Call `list_parts` (free) and match the names the person uses to the parts' labels. Each part's `id` goes in the nest's `library`.
- **Rectangles and circles** can be typed by size instead, as `typed` rows.
- If a part they name is not saved, say so: they can share its drawing here, or import it in NestSolver's web app, under Presets, then Parts.

## 2. Start the nest

Call `start_nest` with:

- `unit` (mm, cm, m or in). Every length is a decimal string in it, like `"1500"`.
- `sheets: [{"width", "height", "quantity"}]`.
- `library: {"<part id>": "<quantity>"}` for saved parts, and `typed: [{"kind": "rectangle", "width", "height", "quantity", "label"}]` or `{"kind": "circle", "diameter", ...}` for typed shapes.
- `part_spacing` (the gap between parts) and `edge_spacing` (the clear strip along every sheet edge), when the person gives them.
- `common_lines: true` with `kerf`, when they want straight edges of neighbouring parts cut once (laser, plasma, waterjet).

It answers at once with the job's `id`.

## 3. Wait, then read the plan

Call `get_job_status` with that id every few seconds, not faster, until its status is `done` or `failed`. If it takes a while, tell the person it is still running. When it is `done`, call `get_plan_report` with the plan id it gives.

Answer with the sheets used, the parts on each sheet, the yield, anything left out and why, and the export links from the report (DXF, SVG and cutter files).

Report the plan as NestSolver gives it. Don't redo the layout or swap in your own; if a figure looks wrong, say so and offer to nest again with corrected input.

## When a call is refused

A refused call comes back with `isError: true` and its problems. Explain each in plain words and fix what you can. If nesting or a feature is not on the person's NestSolver plan, say so plainly. Don't send the same call again unchanged.
