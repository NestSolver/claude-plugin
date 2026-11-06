---
name: plan-load
description: Plans loads with NestSolver, either cartons on pallets or pallets and cargo in trucks, trailers and shipping containers, with stacking rules, weight limits, the centre of gravity and loading steps, and for a semi-trailer the axle loads checked against US Interstate or EU road rules. Use when someone asks how much fits on a pallet or in a truck or container, how to load it, or whether a load is within its limits.
---

# Plan a load with NestSolver

A load runs in the background: start it, wait for it to finish, then read the plan.

## 1. Gather the load

- **Unit**: mm, cm, m or in. Weights follow it: kilograms with mm, cm and m, pounds with in.
- **Spaces**: what is being loaded, like a pallet, a truck, a trailer or a container, with its inside length, width and height and its weight limit. Saved spaces, including trailers set up with their axles, come from `list_spaces` (free): send a row as it is.
- **Cargo**: each item's length, width, height, weight, quantity and a short label, and when it matters, whether it must stay upright, how many can stack, and how much weight can sit on it.

Every length and weight is a decimal string in the request's unit, like `"1200"`.

## 2. Start the load

Call `start_load` with `unit`, `spaces: [{"kind", "label", "length", "width", "height", "weight_limit", "quantity"}]` (`kind` is `"pallet"`, `"carton"` or `"vehicle"`; `quantity` `""` for as many as needed) and `cargo: [{"label", "length", "width", "height", "weight", "quantity", "turning"}]` (`turning` is `"upright"` or `"any"`). It answers at once with the job's `id`.

## 3. Wait, then read the plan

Call `get_job_status` with that id every few seconds, not faster, until its status is `done` or `failed`. If it takes a while, tell the person it is still running. When it is `done`, call `get_plan_report` with the plan id it gives.

Answer with:

- What went into each space, and how it is stacked.
- The total weight and where the load balances (its centre of gravity).
- The loading steps, in order.
- Each compliance check (payload, balance, and axle loads under the road rules), whether it passes, and the nearest fix for any that fail.
- Anything left out, and why.

Report the plan as NestSolver gives it. Don't rearrange the load yourself; if a figure looks wrong, say so and offer to plan again with corrected input.

## When a call is refused

A refused call comes back with `isError: true` and its problems. Explain each in plain words and fix what you can. If a feature is not on the person's NestSolver plan, or the month's loads are used, say so plainly. Don't send the same call again unchanged.
