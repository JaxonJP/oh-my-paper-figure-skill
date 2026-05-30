# PPT Figure Workflow Reference

Use this reference when implementing an editable paper-figure redraw with PowerPoint automation.

## Build Strategy

Start with a coordinate plan:

- Choose slide size from the source aspect ratio, such as `1280x900` for 4:3-ish figures or `1600x1125` for denser diagrams.
- Define reusable helpers for boxes, text, lines, bidirectional arrows, dashed polylines, dots, stacked blocks, and small code/table cells.
- Draw large modules first, then major arrows, then secondary arrows, then labels and small adornments.
- Export a PNG preview after every substantial revision.

Recommended helper set:

- `RGB(r,g,b)` returning a PowerPoint integer color.
- `Add-Box(x,y,w,h,fill,line,radius,dash,weight)`.
- `Add-Text(text,x,y,w,h,size,bold,align,font)`.
- `Add-Line(x1,y1,x2,y2,arrow,weight,dash,color)`.
- `Add-BiLine(...)` for recurrent/state update arrows.
- `Add-Polyline(points, arrow, weight, dash, color)` for routed control paths.

## Dashed Control Path Routing

Do not let dashed paths become a pile of diagonal crossings.

- Prefer orthogonal routing: horizontal bus plus vertical branches.
- Keep dashed lines medium gray and slightly thinner than primary data arrows.
- Use one clean loop for controller feedback instead of duplicate parallel verticals.
- Route around text and boxes; do not pass through labels unless the source diagram clearly does so.
- Add small tees or short ports only when they clarify bus taps.

## Color Sampling

PowerShell can sample a local PNG without extra packages:

```powershell
Add-Type -AssemblyName System.Drawing
$bmp = [System.Drawing.Bitmap]::FromFile($imgPath)
$c = $bmp.GetPixel($x, $y)
$bmp.Dispose()
```

For average fill colors, sample a rectangle and ignore:

- near-white pixels: `R,G,B > 248`
- near-black pixels: `R,G,B < 35`

For strokes, direct averages are usually too pale. Use the sampled hue as guidance, then choose a darker line color that visually matches the reference outline.

Typical mapping:

- sampled green fill -> compare/decision modules
- sampled yellow fill -> compute/recurrent layer modules
- sampled blue fill -> arithmetic/update/register/controller modules
- sampled purple fill -> readout/FC modules
- sampled orange fill -> state/reset modules
- sampled gray fill -> memory/table/note modules

## Visual QA

After exporting preview PNG, inspect for:

- broken line breaks such as words split into single letters
- literal escape sequences from scripting mistakes, such as `` `r`` appearing in text
- arrows crossing text or terminating inside labels
- dashed routes that imply unintended dataflow
- table cells with clipped formulas
- category colors too saturated or too far from source
- repeated blocks not aligned to a grid

Iterate the PPT and export another preview after fixes. The final preview must be from the final saved PPTX.
