---
name: oh-my-paper-figure
description: Recreate academic paper figures, concept diagrams, architecture block diagrams, circuit-style schematics, algorithm flowcharts, and screenshots of technical diagrams as editable PowerPoint slides. Supports two explicit modes: fast mode for time/token-efficient redraws with strong visual fidelity, and fine mode for stricter final-PPTX render-and-compare QA when the user asks for higher completion quality or strict checking.
---

# Oh My Paper Figure

Turn a reference figure into an editable PowerPoint diagram, not a pasted screenshot. Optimize for revisability: separate boxes, arrows, labels, tables, callouts, repeated motifs, and grouped subassemblies where useful.

## Mode Selection

Select exactly one mode at the start. Do not execute both mode checklists.

- Fast mode: default unless the user explicitly asks for fine, strict, high-fidelity QA, final-PPTX render QA, publication-polish, or exhaustive checking. Use when the user wants a quick redraw, lower token/time cost, or normal paper-figure replication. Target strong visual fidelity, roughly 95%+ for major structure and labels.
- Fine mode: use only when the user asks for stricter review, higher completion, precise verification, final-PPTX render-and-compare QA, or when the figure is high-stakes/dense enough that the user clearly values quality over time.

If the user names a mode, follow it. If mode is ambiguous, choose fast mode and mention that fine mode is available.

## Shared Workflow

1. Inspect the source figure.
   - Identify canvas ratio, modules, arrows, dashed control paths, repeated blocks, equations, legends, dimensions, and color categories.
   - Decide what must be faithful and what can be approximated.

2. Rebuild as editable PPT objects.
   - Use PowerPoint shapes, text boxes, lines, arrows, freeforms, tables, and grouped objects.
   - Do not use the source image as the final visual unless the user explicitly asks for a trace/background/raster-only output.
   - Keep text live and editable. Use conservative plain-text math approximations when equation rendering is not required.

3. Match the figure system.
   - Reuse palette, line weights, dashed styles, typography rhythm, spacing, and arrow grammar.
   - Recreate structure first; decorative polish comes second.
   - Keep all major nodes, arrows, labels, equations, legends, and repeated motifs that carry meaning.

## Fast Mode

Use this checklist only in fast mode:

1. Generate the editable PPTX.
2. Export a PNG preview from the generated slide or saved PPTX.
3. Visually inspect the preview for obvious issues:
   - major shape or label missing
   - severe text overflow, unexpected wrapping, or illegible formulas
   - arrows disconnected, hidden, or crossing important text
   - canvas ratio or major layout clearly off
   - edge cropping or legend clipping
4. Iterate only on visible issues that would materially hurt the redraw.
5. Deliver the PPTX and preview PNG.

Fast mode should be economical: avoid lengthy extra QA, repeated render cycles, or optional tooling unless a visible problem appears.

## Fine Mode

Use this checklist only in fine mode:

1. Generate the editable PPTX.
2. Save and close the PPTX before final QA.
3. Reopen the saved PPTX from disk to confirm PowerPoint can load the actual deliverable.
4. Export the relevant slide(s) to PNG from the reopened PPTX.
5. Compare the rendered PNG against the source figure.
6. Fix the PPTX source script or shapes, then repeat the reopen-and-render cycle if any issue is found.

Fine-mode checks:

- Canvas ratio and major module proportions close to the source.
- All major shapes, repeated blocks, arrows, labels, equations, legends, and annotations present.
- Text readable, not clipped, not unexpectedly wrapped, and not shifted into arrows or shapes.
- Arrows and lines connected to intended modules, not broken, floating, or hidden behind shapes.
- Slide edges not cropping content; legends and labels have enough margin.
- Fonts and math symbols rendered as expected; note substitutions or simplified formulas.
- Color categories, line weights, dashed styles, and arrow grammar reasonably match the source.
- PPTX opens, renders, and saves successfully after the final edit.

## PowerPoint Implementation Notes

Prefer PowerPoint COM automation on Windows when Office is installed and the user wants real `.pptx` files. Use the Presentations skill/artifact workflow only when it is clearly more appropriate.

Fast-mode export pattern:

```powershell
$ppt = New-Object -ComObject PowerPoint.Application
$presentation = $ppt.Presentations.Add($true)
$presentation.PageSetup.SlideWidth = 1280
$presentation.PageSetup.SlideHeight = 900
$slide = $presentation.Slides.Add(1, 12)
# Add shapes, lines, text...
$presentation.SaveAs($outPptx)
$slide.Export($previewPng, "PNG", 1280, 900)
$presentation.Close()
$ppt.Quit()
```

Fine-mode final render pattern:

```powershell
$presentation.SaveAs($outPptx)
$presentation.Close()

# Final QA render: reopen the saved PPTX and export from the actual deliverable.
$deck = $ppt.Presentations.Open($outPptx, $false, $false, $false)
$deck.Slides.Item(1).Export($previewPng, "PNG", 1280, 900)
$deck.Close()
$ppt.Quit()
```

For detailed implementation patterns, read `references/ppt-figure-workflow.md` when you need concrete PowerPoint COM recipes, color sampling snippets, routed-arrow patterns, or QA checklists.

For optional beautification modes, read `references/figure-polish-modes.md` only when the user explicitly asks to polish, beautify, make the figure look more advanced/paper-like, or use a named motif such as a stacked register-file bank.

## Final Response Checklist

Before final response, confirm according to the selected mode:

- The PPTX exists.
- A PNG preview was exported after the last meaningful edit.
- No source image is pasted as the only content.
- Fast mode: obvious structural, text, arrow, and cropping issues were checked.
- Fine mode: the final preview was exported from the reopened saved PPTX.
- Any intentional simplification or approximation is called out briefly.
