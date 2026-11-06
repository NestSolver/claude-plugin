# NestSolver for Claude

Plan cutting and loading jobs from a plain request in Claude. This plugin
connects Claude to [NestSolver](https://nestsolver.com) and teaches it how
to use NestSolver's tools well:

- **plan-cut-list**: cut lists for bars, pipe, tube, extrusion or lumber,
  and for sheets of plywood, MDF, metal, glass or acrylic, for a panel saw
  or a CNC router: which parts come from each bar or sheet, the cut order,
  the offcuts worth keeping and the waste.
- **nest-parts**: true-shape nesting of parts saved from DXF, DWG or SVG
  drawings, with spacing, rotation and common-line cutting, and links to
  the plan's cutter files.
- **plan-load**: cartons on pallets and pallets in trucks and containers,
  with stacking rules, the centre of gravity, loading steps, and axle loads
  checked against US Interstate or EU road rules.

## Install

In Claude, add NestSolver from the
[connector directory](https://claude.ai/directory/connectors/nestsolver).

In Claude Code:

```
/plugin install nestsolver --marketplace NestSolver/claude-plugin
```

or, on older versions:

```
/plugin marketplace add NestSolver/claude-plugin
/plugin install nestsolver@nestsolver
```

Then run `/mcp`, choose `nestsolver`, and sign in.

## What it needs

A NestSolver account. Connecting asks you to sign in and pick a workspace;
there is no key to copy. Linear and sheet cut lists and 5 loads a month
work on the Free plan.

NestSolver sees only the cut lists, parts and cargo sent through its tools,
never your conversation.

## Links

- Docs: https://docs.nestsolver.com/mcp
- Privacy: https://nestsolver.com/privacy
- Terms: https://nestsolver.com/terms
- Support: support@nestsolver.com
