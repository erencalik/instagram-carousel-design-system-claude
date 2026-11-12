---
name: instagram-carousel-design-system
description: Builds a persistent carousel brand spec and layout library from a short brand interview, then renders each topic as 1080x1350 PNG carousel slides and visually checks them. Use for "set up a carousel system", "brand carousel", "make carousel PNGs".
---

# Carousel System

The goal is to stop re-describing the look on every carousel: the brand becomes a written spec once, then every topic is rendered to real PNGs with the same rules. For slide copy rules use `instagram-aciklama-carousel-metni` if installed. Requires code execution and file creation; a Claude Project is preferred. Reply in the user's language.

## Setup (once)

**1. Folder structure (project files or a connected folder)**
```
Carousels/
  assets/ photos/ screenshots/ logos/ characters/ stickers/
  reference-examples/   (carousel images the user likes)
  exports/              (finished PNGs)
```

**2. Brand interview.** Ask one at a time, wait for the answer, dig into vague answers; ask the user to describe what the brand is, not what they wish it were. Order:
1. Niche, audience, and what the viewer should feel in the first half-second.
2. Background colour: 4 options (warm paper, clean white, soft tint, dark) with hex values.
3. Accent colour: one accent + one ink colour; warn if a choice is too light for text.
4. Headline font: 5 directions by feel, 2 Google Fonts each; show which stay legible at the 146 px grid-square size.
5. Body, note and (optional) monospace fonts.
6. Personality: hand-drawn marks and highlighter, or strict geometric.
7. Handle, usual call to action and comment keyword.
8. Assets on hand: photos, mascot, screenshots, product images.
Then render 3 sample covers at 420x525 and ask for corrections. After approval write `PROJECT-INSTRUCTIONS.md`: hex palette table with roles, pixel type scale, cover/section/CTA slide grammar, export pipeline, hard rules.

Design rules: **two colours only** (one ink + one accent on one background; brand logos are the exception). **Test the headline font at grid size:** at 146 px thin serifs disappear, bold faces with short extenders survive.

**3. Project instruction (a short pointer).** The project instruction box travels with every message and Claude can't edit it; keep it short:
- Read PROJECT-INSTRUCTIONS.md end to end before any carousel work.
- Read LAYOUT-LIBRARY.md when choosing a composition.
- Don't ask about colours, fonts, handle or tone; the brand is fixed.
- Write new rules worth keeping back into PROJECT-INSTRUCTIONS.md.

**4. Layout library.** `LAYOUT-LIBRARY.md`: 8–12 named archetypes (composition, when to use, which assets it needs). Palette and fonts stay fixed in the spec. At minimum: 3 cover types, 6+ section types, 1 closer, a large annotated screenshot, a before/after comparison, a ranked list. For storytelling accounts add: timeline slide, big-number slide, quote slide, sources slide.

## Export pipeline (write it into the spec)

1. **Design at 420x525, scale up.** Take the screenshot with `device_scale_factor = 1080/420 (2.5714)`. Never set the viewport to 1080 wide; the layout reflows.
2. **Embed fonts as base64 woff2.** Install via npm @fontsource instead of linking Google Fonts; the render environment may not reach Google and fonts silently fall back to a system font.
3. **Embed all images as base64.** Check the real format with `file` first (a .png extension can hide a JPEG).
4. **Write the HTML from Python**, not a shell heredoc; interpolation breaks CSS braces.
5. **Overflow check:** before the screenshot, measure every element against the safe bottom and right edges and print anything that overflows.
Also write the safe inner padding and viewport constants into the spec.

## Work loop

1. The user gives the topic and shape (including which slide gets a screenshot).
2. Claude builds and renders, and **inspects the images itself before showing them** (required).
3. The user gives surgical per-slide notes; no full rebuilds.
4. Files land in `exports/` as PNGs, optionally with a draggable HTML preview.

Copy rules: **no orphans** (no unintended short last line under a full line). **Bigger type, fewer words:** don't add sentences, cut.

## Troubleshooting

- Wrong font: Google Fonts failed silently; embed as base64.
- Text runs off the slide: padding was ignored or the overflow check didn't run; print the measurements.
- Every carousel looks the same: pick a different composition per slide from the library.
- Claude forgets the brand: the pointer instruction is missing or wasn't read; have it reread the spec.
- An arrow points at nothing: arrows only connect a label to a specific element.

Credit: based on the method in Michael Ly's guide "How to Build Your Own Instagram Carousel System with Claude" (milleemd.com/guides/instagram-carousel-system).
