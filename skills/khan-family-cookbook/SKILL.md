---
name: khan-family-cookbook
description: Build the Khan Family Cookbook — turn pasted/uploaded recipes into a bright, happy heirloom recipe book. Use when adding recipes, regenerating the cookbook, or producing print-ready pages. Auto-categorizes recipes into colored sections, handles Noor's ancestral voice vs Yahya's precise voice, builds hero/polaroid-collage/full-page photo layouts, outputs HTML + print-ready PDF, maintains a README table of contents, and prepares files to push to github.com/nfairoza/khanfamilyrecipes.
---

# Khan Family Cookbook

Build "The Khan Family Cookbook" — a bright, happy, heirloom recipe book for Noor and Yahya Khan and future generations. Each run takes one or more recipes and produces designed cookbook pages as **both HTML and a print-ready PDF**.

Default output folder: `C:\Documents\khanfamilyrecipes` (or the current working directory if that path does not exist — this skill is meant to be reusable from any machine or Claude account).

This skill encodes an APPROVED design. Match it exactly. Do not drift toward muted/cream looks — the cream/dull look was explicitly rejected. The book must feel bright, sunny, and happy.

## What to do each run

1. **Collect recipes.** The user pastes or uploads them. Each recipe may be tagged as Noor's or Yahya's. If untagged, infer from style (loose/sensory = Noor, precise measurements = Yahya) and confirm.
2. **Auto-categorize, then show and confirm.** Decide each recipe's section from its title + ingredients. Print a short list like "Chicken Dum Biryani -> Biryani; Mutton Salan -> Salans" and let the user override any with one word before building. Create new categories on the fly when needed and assign each a palette color (see below).
3. **Write in the right voice** (see Voices).
4. **Lay out photos** using one or more of the three layouts (hero, polaroid collage, full-page). Auto-draft captions from filename + nearby recipe context; the user edits the ones they care about.
5. **Generate HTML**, then render to **PDF** with WeasyPrint.
6. **Save both** to the output folder and present with links.
7. **Update the README** table of contents.
8. **Offer GitHub publish** steps (prepare files + give push commands; do not assume credentials).

## Design spec (APPROVED — "Fresh & sunny", brightened)

- **Base:** white paper `#ffffff` (NEVER cream — cream reads dull).
- **Palette:** marigold `#ff8a00` / `#ffb400`, coral `#ff5a4d`, turquoise `#13c4c4`, leaf green `#5fc23b`, pink `#ff7eb6`. Ink `#241f1a`, soft ink `#7a7468`.
- **Cover:** multi-radial sunny gradient, rainbow conic-gradient "plate" circle with a centered food emoji/photo, scattered confetti dots, title "The Khan Family & Cookbook" (the `&` in italic coral), subtitle "recipes, memories & a little chaos in the kitchen", "A collection by Noor & Yahya Khan".
- **Recipe pages:** bold gradient **title banner** (accent -> accent-2) with white text and a white category chip; white-bordered **hero photo** with rounded corners; two columns — tinted **ingredients box** (accent-soft bg) on the left, **method** with colored step numbers on the right; a bordered **note** box for the "Amma's note / Yahya's note" line.
- **Signatures:** subtle, bottom-right, italic "from the kitchen of **Noor**/**Yahya**" with the name in a script font in the accent color. Keep it light, not loud.
- **Folio:** bottom-left small label "The Khan Family Cookbook · <Category>".
- **Section dividers:** full accent-gradient background, white rounded card with a big emoji, the category name, a rule, and a one-line blurb. An **edge tab** (white pill on the right edge) names the section and acts as the physical folder divider. **Do NOT number sections.**

### Per-category color shift
Each category keeps the family look but shifts accent color. Suggested mapping (extend as needed, keep within the palette):
- Biryani -> orange `--accent:#ff7a00; --accent-2:#ffc233; --accent-soft:#fff1d8`
- Breads & Bakes -> teal `--accent:#0aa6a6; --accent-2:#6ee0d6; --accent-soft:#e2fbf8`
- Rice -> marigold `#ffb400`
- Salans (curries) -> coral `#ff5a4d`
- Tandoori & BBQ -> deep red/ember `#e23d2e`
- Soups & Broths -> green `#5fc23b`
- Sandwiches & Snacks -> pink `#ff7eb6`
- Regional (Hyderabadi/Andhra/Korean/Chinese) -> pick an unused palette hue and stay consistent.
Assign new categories the next free hue. Record assignments so a category always gets the same color across runs.

## Voices

- **Noor (ancestral / touch-and-feel):** loose, sensory, warm. "a good amount", "to your family's bravery", "cook until the kitchen smells unbearable in the best way", "you will know", "add extra ghee for whoever you love most". Convert precise measures to feel-based phrasing.
- **Yahya (precise):** exact cups + grams, F (C), explicit times, internal temps, "in 3 additions", "no less", "measured, not guessed". Keep it confident and exact.

Each recipe gets one signature matching its author.

## Three photo layouts

1. **Hero** — single photo on the recipe page, white border, rounded.
2. **Polaroid collage page** — scattered polaroids with varying tilt, size, and orientation, colored tape on top, handwritten-style captions. Insta/polaroid happy feel. Vary rotation and overlap so it looks casual.
3. **Full-page photo** — full-bleed single photo filling the whole letter page, soft dark gradient at the bottom, white title + a short memory caption.

Pick per recipe based on how many photos exist: 1 photo = hero only; 2-3 = hero + maybe a full-page; 4+ = hero + collage. Offer a full-page "moment" page for standout dishes.

### Captions
Auto-draft a caption for every photo from its filename and the nearby recipe/step context (e.g. `biryani-layering.jpg` near the dum step -> "layering the dum"). Present drafts; the user edits the few they want. Use any caption they provide verbatim. Only describe actual image contents for photos shared directly in the chat.

## Print / output requirements

- **US Letter, 8.5in x 11in.** `@page { size: Letter; margin: 0 }`.
- **Binder margin:** extra left padding ~`1.15in` for 3-hole punch + reinforcement labels; other margins ~`0.7in`. Cardstock, 3-ring binder.
- Use `print-color-adjust: exact` so the bright colors print.
- One `.page` per physical page, `page-break-after: always`.
- Onscreen, show faint binder holes; hide them in print.
- Keep the HTML **self-contained** (inline CSS, embed photos as base64 or reference local files kept alongside).

### Rendering the PDF
Use WeasyPrint (Chromium/Playwright downloads are often blocked in sandboxes):
```
python3 -c "from weasyprint import HTML; HTML('khan-family-cookbook.html').write_pdf('khan-family-cookbook.pdf')"
```
Install if needed: `pip install weasyprint --break-system-packages -q`. Verify page count and 8.5x11 size with pypdf.

Note: WeasyPrint does not render emoji in color and handles some transforms differently than a browser. For the real book with actual photos this is fine. For pixel-perfect rotation/shadows, opening the HTML in a browser and using "Print to PDF" is the highest-fidelity path; mention this option.

## README / table of contents

Maintain `README.md` in the output folder with: a short intro, a table of contents listing each category and the recipes under it (with the author), and links to the HTML and PDF. Update it every run.

## GitHub publishing

Repo: `github.com/nfairoza/khanfamilyrecipes`. The user pushes themselves (no credentials assumed). Prepare all files, then give exact commands:
```
cd C:\Documents\khanfamilyrecipes
git add .
git commit -m "Update Khan Family Cookbook"
git push
```
If the repo isn't cloned yet, give clone/init + remote-add steps first. The cookbook HTML is named `index.html` so GitHub Pages can serve it as a live page (enable Pages on the `main` branch in repo settings).

## Reusing this skill from another Claude account

This skill lives in the repo at `skills/khan-family-cookbook/`. To reuse it elsewhere: clone the repo, then either zip `skills/khan-family-cookbook` as `khan-family-cookbook.skill` and install it, or copy the folder into the target's skills directory. The full design spec above is self-contained, so the skill reproduces the approved look anywhere.

## Self-check before delivering

White base (not cream), bright saturated accents, gradient title banners, per-category color + edge tab, no numbered sections, both author voices correct, subtle signatures, three photo layouts available, captions drafted, US Letter with binder margin, BOTH HTML and PDF saved, README TOC updated, GitHub steps offered. Present files with links and a short summary.
