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

   **Page budget:** a `.page` is fixed-height and clips overflow — never let
   content get cut off. Priority order on sheet 1: chips → title/rule/attribution/
   yield → headnote → hero → ingredients + method → note box. The **taped original
   handwritten card** (`.original`) goes on sheet 1 only when the method is short;
   otherwise move it to the collage page or give it its own second sheet — it's a
   centerpiece, not a footnote. Long methods flow to a second sheet naturally
   (duplicate the `.page` chassis); don't shrink type to cram.

   If the recipe opens a **new category**, also generate that category's divider
   page once: `dividers/<category>.html` + `.pdf`, and record the color in
   `references/categories.md`.

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
