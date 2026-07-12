---
name: khan-family-cookbook
description: >-
  Build "The Khan Family Cookbook" — the bright, sunny, heirloom recipe book for Noor
  and Yahya Khan. Turns pasted or uploaded recipes into designed cookbook pages
  (cover, colored section dividers with edge tabs, gradient-banner recipe pages,
  polaroid collages, full-page photo "moments") and outputs BOTH self-contained HTML
  and a print-ready US Letter PDF with a 3-ring-binder margin, then updates the
  README table of contents and preps files for github.com/nfairoza/khanfamilyrecipes.
  Use this skill whenever the user wants to add, format, or re-run a recipe for the
  family book; says "put this in our book / book style"; mentions the Khan cookbook,
  biryani/salan/tandoor sections, Noor's or Yahya's recipes, polaroid pages, or
  printing the cookbook — even if they just paste a recipe with no other instruction.
  Also use it to regenerate the whole book, rebuild the PDF, or update the README/TOC.
---

# The Khan Family Cookbook

A bright, happy heirloom cookbook for Noor & Yahya Khan and future generations.
Every run takes one or more recipes and produces designed pages as **both HTML and a
print-ready PDF**, plus an updated README table of contents.

**The design is APPROVED and encoded in `assets/templates.html`. Copy its markup and
CSS — do not redesign from memory.** Noor explicitly rejected muted/cream looks: the
base is pure white `#ffffff` with saturated marigold/coral/turquoise/green/pink
accents. If a page starts looking calm, beige, or tasteful-neutral, it's wrong.

## Where files go

- **On Noor's machine** (Cowork / Claude Desktop / Claude Code with access to her
  filesystem): everything lives in `C:\Documents\khanfamilyrecipes`. Present results
  with `computer://` links.
- **In the claude.ai web container**: that path doesn't exist. Work in
  `/home/claude/khanfamilyrecipes`, copy final HTML + PDF + README to
  `/mnt/user-data/outputs/`, present them, and remind Noor to drop them into
  `C:\Documents\khanfamilyrecipes` locally.

## Workflow (every run)

1. **Collect recipes.** Noor pastes or uploads them. There is no Gmail connector —
   if asked to pull from Gmail, say that needs a Gmail integration that isn't
   connected, and ask her to paste instead.

2. **Attribute the cook.** Each recipe may be tagged Noor's or Yahya's. If untagged,
   infer from style (loose/sensory → Noor; precise measurements → Yahya) and
   **confirm before building** — one short line, not a questionnaire.

3. **Auto-categorize, show, confirm.** Decide each recipe's section from title +
   ingredients using `references/categories.md`. Print a short mapping —
   `Chicken Dum Biryani → Biryani; Mutton Salan → Salans` — and let Noor override
   any with one word. If a recipe needs a new category, create it, assign the next
   free palette hue, and **record it in `references/categories.md`** so the color is
   stable across runs.

4. **Write in the right voice.** Read `references/voices.md` first — it defines how
   each cook's quantities, doneness cues, and notes are written, with examples.
   Never fabricate precision: converting Noor's "a good amount" into grams is
   estimation, so either confirm with her or mark converted values as approximate.

5. **Design the photo pages — fresh per recipe.** Photos are laid out from a
   *design vocabulary*, not fixed templates. Read the "Photo pages" section below,
   pick or invent a motif that fits the dish and the photo count, and compose it.
   **Auto-draft every caption.** Whenever the image is actually viewable — shared
   in chat, or readable from the recipe's `photos/` folder on Noor's machine —
   open it and caption from what's really in the photo, in the book's warm
   handwritten tone ("first scoop", "everyone waiting"). Only when the file can't
   be viewed, fall back to filename + nearby step context and never describe
   contents you haven't seen. Present all drafts in a short list for Noor to edit;
   use any caption she gives verbatim.

6. **Build ONLY the new recipe's pages — never regenerate existing files.** Each
   recipe gets its own self-contained folder, named by slug:
   `recipes/chicken-dum-biryani/` containing `chicken-dum-biryani.html`,
   `chicken-dum-biryani.pdf`, and a `photos/` subfolder for its images. Copy the
   blocks you need from `assets/templates.html` (recipe page, plus collage /
   full-page photo pages if warranted — extra pages go in the same HTML file),
   swap the CSS variables to the category's colors, inline the CSS, and embed
   photos as base64 `data:` URIs or reference them relatively (`photos/…`).
   Existing recipe folders are finished — do not touch them unless Noor asks to
   edit that specific recipe (then regenerate just that folder's files).

   **Print double-sided: every recipe is a 4-page unit so no side is ever blank.**
   Noor prints on a duplex printer and files sheets in a binder, so each printed
   sheet must have content on BOTH sides, and opening to a recipe must show a
   **photo page on the left, the recipe on the right**. Build every recipe as four
   `.page` sections in this exact order (they duplex so pages 2–3 land as the
   facing spread):
   - **Page 1 — title opener:** big Fraunces title, a memory/headnote line, the
     Caveat attribution, on a soft accent-gradient. Fills the back of the divider's
     facing side.
   - **Page 2 — photo page (lands LEFT):** the finished-dish photos, arranged to
     FILL the page for the number of photos there actually are (see the dynamic
     photo rule below).
   - **Page 3 — the recipe (lands RIGHT):** chips → title/rule/attribution/yield →
     ingredients box → method with colored step numbers (two columns) → note box.
     This must fit on ONE page — condense sensibly (two-column method/ingredients),
     since the presentation lives on pages 1–2.
   - **Page 4 — more photos, OR a "notes & pairings" card:** if there are enough
     good photos left over to fill it, a photo grid; otherwise a styled
     notes/serving card (never a grid of empty frames).
   The method NEVER shrinks type to cram — if page 3 overflows, move overflow to
   page 4, don't shave margins.

   **Dynamic photo layout — fit the layout to the photos, never the reverse.**
   Noor sends anywhere from 0 to 10+ photos; the page count and arrangement adapt:
   - **Curate first.** Use only the good, non-redundant shots — ten photos given
     does NOT mean ten used. Always include exactly ONE main finished-dish photo;
     everything else (process, plated, ingredients) is optional, chosen for variety.
     Never place two near-identical shots.
   - **NEVER render an empty / "your photo here" frame.** Placeholder frames are
     forbidden in a recipe that has photos. A recipe with photos shows only real
     photos, sized to fill.
   - **Page count by curated photo count N** (keep it EVEN so no printed side is
     blank):
     - **N = 0** → **2 pages**: opener + recipe. No photo page at all (no empty
       frames while waiting for photos).
     - **N = 1–3** → **4 pages**: opener, photo page (fill variant for 1/2/3),
       recipe, then a **"notes & pairings" card** as page 4 (not photos).
     - **N = 4–6** → **4 pages**: opener, photo page (main + 2–3), recipe, second
       photo page with the rest.
     - **N = 7+** → **6 pages**: add a polaroid-collage page; keep the left/right
       facing rule (photo pages on even pages).
   - **Photo-page fill variants** (page must look full, no dead space): 1 photo =
     one large framed photo centered; 2 = main large on top + one wide below (or two
     equal halves); 3 = main large + a row of two; 4 = 2×2 grid; 5–6 = main + a
     row of two + a row of two/three. Draft a short Caveat caption per photo.
   - When Noor later drops photos into a recipe that had N=0, re-render it into the
     4-page form. Adding/removing photos only re-renders THAT recipe.

   **Frames (optional, casual):** a tasteful frame around photos is welcome — white
   border/rounded, a soft shadow, or an occasional polaroid tilt — but it is NOT
   required to be identical on every recipe. Vary it a little; don't force one frame
   style everywhere. Keep it clean, never busy.

   Verify each render: page count is even, nothing clips (render to PNG, check the
   lowest content row is above the bottom margin), and there are zero empty frames.

   **Dividers are 2 pages** for the same reason: page 1 is the section card (with
   edge tab), page 2 is an "In this section" contents card (`.back` — eyebrow,
   name, blurb, and a `.toc` list of the section's recipes with authors) so the
   back is never blank. Regenerate a divider's page 2 when a recipe is added to
   that section.

   If the recipe opens a **new category**, also generate that category's divider
   (both pages): `dividers/<category>.html` + `.pdf`, and record the color in
   `references/categories.md`.

   Tell Noor to print with **Two-Sided / "Flip on long edge"** so the facing-page
   layout comes out right.

7. **Render each new PDF.** On Noor's Windows machine, use **headless Chrome
   "Print to PDF"** — WeasyPrint's native GTK libraries fail to load on Windows,
   and Chrome is also the highest-fidelity path (renders color emoji and CSS
   transforms exactly like the browser). Use absolute Windows paths:

   ```bash
   "/c/Program Files/Google/Chrome/Application/chrome.exe" --headless --disable-gpu \
     --no-pdf-header-footer \
     --print-to-pdf="C:\Documents\khanfamilyrecipes\recipes\chicken-dum-biryani\chicken-dum-biryani.pdf" \
     "C:\Documents\khanfamilyrecipes\recipes\chicken-dum-biryani\chicken-dum-biryani.html"
   ```

   Fallback (Linux / claude.ai web container, where Chrome isn't present):
   WeasyPrint — `pip install weasyprint pypdf --break-system-packages -q` then
   `python3 -c "from weasyprint import HTML; HTML('in.html').write_pdf('out.pdf')"`
   (note: no color emoji, some transforms differ from a browser).

   Verify with pypdf that the page count matches and pages are 8.5×11in (612×792pt).
   For pixel-perfect polaroid tilt/shadows, opening the HTML in a browser and using
   **Print → Save as PDF** (background graphics on) is also available.

8. **Add the recipe to the index — don't rebuild it from scratch.**
   - `index.html`: the book's landing page/table of contents in the house style —
     cover header, then categories in registry order, each recipe a row (title,
     author, links to its HTML and PDF). Insert the new row under its category;
     create the category heading if it's new.
   - `README.md`: mirror of the same TOC in markdown. Insert the same row.
   Since Noor prints on cardstock for a 3-ring binder, new sheets just get punched
   and filed behind the right divider tab — no full-book reprint needed.

9. **Full-book rebuild is on-demand only.** When Noor asks to "regenerate the
   whole book" or wants one combined PDF, concatenate cover → per-category
   (divider + its recipes) into `khan-family-cookbook.html`/`.pdf`. Never do this
   automatically on a normal add-a-recipe run.

10. **Present + offer GitHub publish.** Present just the new/changed files (recipe
    HTML + PDF, index, README, any new divider). Then give exact push commands
    (see GitHub section). Never assume credentials.

## Design spec (v2 "bright heirloom editorial" — the template is the source of truth)

- **Base:** white `#ffffff`. NEVER cream.
- **Palette:** marigold `#ff8a00`/`#ffb400`, coral `#ff5a4d`, turquoise `#13c4c4`,
  leaf `#5fc23b`, pink `#ff7eb6`. Ink `#241f1a`, soft ink `#7a7468`.
- **Type system:** Fraunces (serif display, weights 600/900) for titles and
  headings; Source Serif 4 for body; Caveat for script (attribution, captions).
  Elegant heirloom serif — no rounded/playful display type.
- **Cover:** sunny multi-radial wash, rainbow conic ring "plate" holding a 2×2
  **photo-wedge collage** (or one photo / emoji fallback), confetti dots, Fraunces
  title with the & italic coral, Caveat script subtitle, byline.
- **Recipe page (editorial header):** small category + region chips → Fraunces
  title with the evocative word in italic accent (`Dum <em>Biryani</em>`) → thick+
  thin **double rule** in accent → Caveat attribution "from the kitchen of Noor" →
  small-caps accent **yield line** ("Feeds a full table · Sunday project") →
  italic **headnote**: a 2–3 sentence real memory, lead-in words bold accent. Ask
  Noor for the memory or draw it from what she's said — never invent one; skip the
  headnote if there isn't one. Then hero photo (classic rounded or **arch crop**),
  two columns (tinted ingredients box, colored step numbers), optional **taped-in
  scan of the original handwritten recipe card** with a Caveat caption (the
  heirloom centerpiece — always offer it when a scan/photo of the original
  exists), note box, italic folio bottom-left, Fraunces page number bottom-right.
- **Section dividers:** full accent-gradient background, white rounded card with a
  circular photo (or emoji) medallion + Fraunces name + gradient rule + italic
  blurb, white pill **edge tab** on the right edge. **No section numbers.**
- **Photo pages — a vocabulary, not templates.** Every recipe's photo page is
  composed fresh so no two recipes look stamped from the same mold. **House
  constants (always):** white photo frames or polaroid borders, Caveat captions,
  colored tape/pins in the palette, the category accent doing the decorating, page
  chassis with binder margin, folio. **The motif (varies per recipe)** — pick or
  invent one that matches the dish's personality and photo count:
  - *scattered polaroids* — tilted, overlapping, taped; for chaotic happy days
  - *circle plate* — a big ring holding photo wedges/semicircles (like the cover);
    for feasts and spreads
  - *arch / dome windows* — one or three arched photos in a row; for breads,
    bakes, anything oven-and-patience
  - *film strip* — a neat vertical or horizontal strip of process shots; for
    step-driven recipes (layering, folding, shaping)
  - *scrapbook* — one big taped photo + torn-paper edges + small snapshots
    tucked around it; for recipes with an original handwritten card
  - *big & little* — one large anchor photo with 2–3 small ones stacked beside;
    the classic editorial spread
  - *facing spread* — a full-bleed "moment" page ordered so it sits opposite the
    collage when the binder is open (moment on the left sheet, collage on the
    right)
  Vary the motif from the previous recipe unless Noor asks for consistency; state
  which motif was chosen and why in one line when presenting. Photo count guide:
  1 = hero only; 2–3 = hero + big & little or a facing moment; 4+ = hero + a full
  collage page. `assets/templates.html` includes starter markup for several
  motifs — treat them as ingredients, not the finished dish.
- **Print:** US Letter, `@page { size: Letter; margin: 0 }`, left padding ~1.15in
  for the 3-hole punch, other margins ~0.7in, `print-color-adjust: exact`, one
  `.page` per sheet with `page-break-after: always`. Faint binder holes onscreen,
  hidden in print.

Category → color assignments live in `references/categories.md` (the registry).

## Folder layout (keep it exactly like this)

```
khanfamilyrecipes/
├── index.html                  ← TOC landing page (also works for GitHub Pages)
├── README.md                   ← markdown mirror of the TOC
├── cover.html / cover.pdf      ← generated once, reused
├── dividers/
│   └── biryani.html/.pdf       ← one per category, generated when category first appears
├── recipes/
│   ├── chicken-dum-biryani/    ← ONE FOLDER PER RECIPE, self-contained
│   │   ├── chicken-dum-biryani.html
│   │   ├── chicken-dum-biryani.pdf
│   │   └── photos/             ← this recipe's photos (when not base64-embedded)
│   └── mutton-salan/
│       ├── mutton-salan.html
│       └── mutton-salan.pdf
└── khan-family-cookbook.pdf    ← combined book, rebuilt only on request
```

A recipe's folder is its complete unit — page, print file, and photos together —
so it can be re-printed, edited, or removed without touching anything else. Other
runs never modify existing recipe folders.

## GitHub publishing

Repo: `github.com/nfairoza/khanfamilyrecipes`. Noor pushes herself — no credentials
in this environment. Prepare all files in the folder, then give her exact commands:

```
cd C:\Documents\khanfamilyrecipes
git add .
git commit -m "Update Khan Family Cookbook"
git push
```

If the repo isn't cloned yet, give clone/init + remote-add steps first. Offer to
include the PDF. To publish the HTML as a viewable page, suggest enabling GitHub
Pages and naming (or copying) the main file to `index.html`.

## Self-check before delivering

☐ White base, saturated accents, gradient banners ☐ correct category color + edge
tab, no numbered sections ☐ voice matches the cook, no invented precision ☐ subtle
signature + folio on every recipe page ☐ right photo layout for the photo count,
captions drafted ☐ Letter size with binder margin, verified with pypdf ☐ new recipe
saved in its own folder under `recipes/` (HTML + PDF + photos), existing folders untouched
☐ index.html and README TOC updated with the new row ☐ GitHub steps offered
☐ presented with links + a short summary.

## Files

- `assets/templates.html` — the approved design: cover, divider, recipe page,
  polaroid collage, full-page photo, all CSS variables. Copy from here.
- `references/categories.md` — auto-sort rules + the persistent category→color
  registry. **Update it whenever a new category is created.**
- `references/voices.md` — Noor's *feel* voice vs Yahya's *measure* voice, with
  examples and the no-fabricated-precision rule.
