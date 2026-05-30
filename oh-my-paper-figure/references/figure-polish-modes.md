# Figure Polish Modes Reference

Use this reference only when the user explicitly asks for beautification, advanced paper style, visual polishing, or a named motif. Do not load or apply these modes for baseline figure replication.

## Extension Mode Rules

- Preserve the original figure logic, labels, arrows, and category semantics before adding polish.
- Apply one named motif at a time unless the user asks for a broader redesign.
- Keep every polished element editable as PowerPoint shapes, not raster overlays.
- Prefer subtle visual hierarchy over decorative detail: thin outlines, restrained fills, consistent offsets, and clean ports.
- Mention intentional upgrades in the final response so the user can distinguish faithful replication from aesthetic enhancement.

## Stacked Register File Motif

Use for register files, memories, spike/state banks, queues, SRAM banks, or repeated storage blocks when the user asks for an upgraded paper-style representation.

1. Draw two or three back plates first, each offset by 6-14 px.
2. Use lighter fills on back plates and the category stroke color.
3. Draw the front rounded rectangle last with the actual label.
4. Add thin side edge lines if the stack needs to read as a bank rather than a shadow.
5. Keep arrows anchored to the front plate unless showing a bus that addresses the whole stack.

Suggested helper:

```powershell
Add-RegStack(label,x,y,w,h,fill,line)
```

Do not apply this motif to arbitrary boxes. If the source uses plain blocks and the user did not ask for visual upgrading, keep plain blocks.

## Bus And Port Polish

Use when a block diagram has many arrows entering a repeated module, controller, memory, or compute array.

- Replace crowded point-to-point arrows with a short shared bus plus clean orthogonal branches.
- Add small port ticks only where they clarify fan-in or fan-out.
- Keep primary data paths black or dark; keep control paths medium gray and dashed.
- Avoid adding ports to every block if it makes the figure busier than the source.

## Hierarchy Polish

Use when the source has nested conceptual regions or weak grouping.

- Add light container outlines only when they clarify a subsystem boundary.
- Keep containers behind content with low-contrast fills or no fill.
- Align repeated modules to a grid before adding decorative hierarchy.
- Do not put cards inside cards; use grouping bands or faint boundaries instead.

## Category Palette Polish

Use when the user asks for a cleaner or more paper-like palette beyond direct color matching.

- Keep source category identities recognizable.
- Reduce saturation slightly for large filled regions.
- Darken strokes relative to fills for print readability.
- Use one accent color for the main path and quieter colors for secondary modules.
