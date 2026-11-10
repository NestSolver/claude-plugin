# NestSolver

Plan cutting and loading jobs from a plain request. This plugin connects
Claude to NestSolver, a planner for shops, and teaches Claude how to use it:

- **Cut lists** (`plan-cut-list`): parts cut from lengths of bar, pipe,
  tube, extrusion or lumber, or from sheets of plywood, MDF, metal, glass or
  acrylic for a panel saw or a CNC router. You get which parts come from
  each bar or sheet, the cut order, the offcuts worth keeping and the waste.
- **True-shape nesting** (`nest-parts`): parts from DXF, DWG or SVG
  drawings, shared in the chat or saved before, plus rectangles and
  circles, nested on sheets with spacing, rotation and common-line cutting,
  with links to the plan's cutter files.
- **Load planning** (`plan-load`): cartons on pallets and pallets in trucks
  and containers, with stacking rules, the centre of gravity, loading steps,
  and a semi-trailer's axle loads checked against US Interstate or EU road
  rules.

## How to use it

Install the plugin from Claude's directory. In Claude Code, run:

```
/plugin install nestsolver --marketplace NestSolver/claude-plugin
```

or, on older versions, `/plugin marketplace add NestSolver/claude-plugin`
then `/plugin install nestsolver@nestsolver`.

Then connect NestSolver when Claude asks, or from the plugin's Connectors
tab (in Claude Code, run `/mcp` and choose `nestsolver`): sign in to your NestSolver account and pick a
workspace. There is no key to copy. In Claude's apps, skills run with
**Code execution and file creation** turned on in your settings.

Then ask in plain words, for example:

- "Cut 4 rails of 1250 mm and 6 stiles of 840 mm from 6 m bars with a 3 mm kerf. Keep drops over 300 mm."
- "Lay out 12 shelves of 600 x 300 mm on 2440 x 1220 mm plywood with a 4 mm kerf, for a panel saw."
- "Nest 20 of my saved part Bracket A on 3000 x 1500 mm steel sheets with 5 mm spacing."
- "Load 40 cartons of 600 x 400 x 300 mm, 18 kg each, onto a EUR pallet."

## What it sends

The plugin is instructions and a connection; it runs no code of its own.
When Claude plans a job, it sends the job to NestSolver's MCP server at
https://mcp.nestsolver.com: the stock, parts, sheets, cargo and settings you
give it, and the ids of parts or spaces saved in your workspace. NestSolver
plans the job in your workspace and keeps nests and loads for 30 days. It
never receives your conversation.

You need a NestSolver account. Linear and sheet cut lists, 5 nests (up to
50 parts each) and 5 loads a month work on the Free plan.

## Links

- Docs: https://docs.nestsolver.com/mcp
- Privacy: https://nestsolver.com/privacy
- Terms: https://nestsolver.com/terms
- Support: support@nestsolver.com
