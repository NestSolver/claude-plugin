---
name: plan-cut-list
description: Plans cut lists with NestSolver, either parts cut from lengths of bar, pipe, tube, extrusion or lumber (linear), or rectangular parts cut from sheets of plywood, MDF, metal, glass or acrylic for a panel saw or a CNC router, with the cut order, the offcuts worth keeping and the waste. Use when someone asks how to cut parts from their stock, wants a cut list or cutting plan, or wants to waste less material.
---

# Plan a cut list with NestSolver

NestSolver plans two kinds of cut list:

- **Linear**: parts cut from lengths of stock (bars, pipe, tube, extrusion, lumber). Tools: `validate_linear` and `solve_linear`.
- **Sheet**: rectangles cut from sheets, by a panel saw (edge-to-edge cuts in stages) or a CNC router (parts placed anywhere). Tools: `validate_sheet` and `solve_sheet`.

## 1. Gather the job

Ask only for what is missing, in one short message:

- **Unit**: mm, cm, m or in. Use the one the person writes in.
- **Stock**: each stock length (linear) or sheet size, width by height (sheet), and how many are on hand. Leave the count blank when they can buy as many as they need.
- **Parts**: each part's size, how many, and a short label.
- **Kerf**: the width the blade or bit removes. If they don't know it, say you are assuming a typical saw blade, 1/8 in (3 mm), and use that.
- For sheets: **panel saw or CNC router**, and whether **grain** direction matters for any part.
- Optionally, the shortest drop (linear) or smallest offcut (sheet) they would keep.

## 2. Write the request

- Every length is a **decimal string** in the request's unit, like `"1250"` or `"23.625"`. Convert fractions first: 23 5/8 in is `"23.625"`, and a 1/8 in kerf is `"0.125"`.
- **Linear** (`solve_linear`): `unit`, `kerf`, `target` (`"bars"` for the fewest bars, `"waste"` for the least waste, `"drop"` to keep the longest drop), `strict: false`, `stocks: [{"length", "qty", "minimum_drop", "zones": []}]` (`qty` `""` for as many as needed, `minimum_drop` `""` for none) and `parts: [{"length", "quantity", "label"}]`.
- **Sheet** (`solve_sheet`): `unit`, `kerf`, `mode` (`"guillotine"` for a panel saw, `"free"` for a CNC router), `sheets: [{"width", "height", "qty"}]` and `parts: [{"width", "height", "quantity", "label"}]`. Optional: `minimum_leftover`, `target` (`"sheets"`, `"waste"`, `"leftover"` or `"fence"` for the fewest fence settings on a panel saw), a sheet's `grain` (`"width"` or `"height"`) and a part's `grain_along`.
- For a long list, dozens of rows, call the `validate_*` tool first. It is free, and it names each problem at its field. Fix those, then solve.

## 3. Solve, then read the plan back

Call the `solve_*` tool once. Answer with what a shop works from:

- How many bars or sheets are used, and which parts come from each.
- The cut order for each bar or sheet (for a panel saw, by stage).
- The offcuts worth keeping (drops or reusable leftovers), and the yield or waste.
- Anything left out, and why.

Lead with a two-line summary, then a short list or table for each bar or sheet.

Report the plan as NestSolver gives it. Don't redo the layout or swap in your own; if a figure looks wrong, say so and offer to plan again with corrected input.

## When a call is refused

A refused call comes back with `isError: true` and its problems. Explain each in plain words and fix what you can, like a mistyped size or a missing field. If a feature is not on the person's NestSolver plan, say so plainly and plan without it. Don't send the same call again unchanged.
