# takeoff-plugin

A Claude plugin for running quantity takeoffs on building plans.

It bundles two things:

- **The OpenTakeoff MCP server** — the measuring engine ([OpenTakeoff](https://github.com/mfmitchell24/opentakeoff), Apache-2.0). It opens plan PDFs, sets scale, detects rooms, sweeps symbols, and exports reports and marked-up plan sets.
- **The `plan-takeoff` skill** — the working method: what order to measure in, what to check, and what the readout looks like.

Installing the plugin sets up both. You don't configure the MCP server by hand.

---

## Install

**Requirements:** Claude Desktop, and [Node.js](https://nodejs.org) 20 or later installed on the same computer.

1. Open Claude Desktop and go to **Customize** in the left sidebar.
2. Open the **Plugins** tab.
3. In the **Personal plugins** section, click **+** → **Add marketplace**.
4. Choose **Add from a repository** and enter:
   ```
   lefteartm/takeoff-plugin
   ```
5. Install **plan-takeoff** from that marketplace.
6. Start a new chat and ask: *"what OpenTakeoff tools do you have?"* — you should see tools like `load_plan`, `set_scale` and `one_click`.

If no tools appear, Node.js is usually the cause. Check it's installed by opening a terminal and running `node --version`.

---

## Use it

Attach a plan PDF (or a whole plan set as a `.zip`) and ask for a takeoff. The skill triggers on plans, drawings, quantities, areas, counts and materials lists — you don't have to say "takeoff".

Every run produces:
- a summary in the chat, with scale verification and open items
- an Excel workbook
- a marked-up plan set PDF

**Everything the plugin measures is a proposal, not a finished takeoff.** Shapes land in the OpenTakeoff canvas as dashed pencil marks. Only a person clicking Accept turns them into approved work. Always review the marked-up PDF before pricing anything off it.

---

## Adjusting it

The numbers most worth tailoring are in
`plugins/plan-takeoff/skills/plan-takeoff/references/residential.md` — the defaults table near the top (ceiling height, waste percentages) and the trade checklist.

The readout layout lives in `references/readout-template.md`.

Edit, commit, push. Installed copies pick up the change on the next sync.

---

## How it works

```
.claude-plugin/marketplace.json     marketplace catalog
plugins/plan-takeoff/
  .claude-plugin/plugin.json        plugin manifest
  .mcp.json                         launches opentakeoff-mcp
  skills/plan-takeoff/
    SKILL.md                        the workflow
    references/
      residential.md                residential checklist + defaults
      commercial.md                 commercial checklist
      readout-template.md           report layout
```

## Notes

- Plans stay on your computer. OpenTakeoff runs locally and uploads nothing.
- OpenTakeoff is Apache-2.0, by [Kentucky AI](https://kentucky-ai.com). This repo only packages it — the engine is theirs.
- Units default to metric and Australian residential conventions. Change that in `SKILL.md` if you need imperial.
