# The Khan Family Cookbook

A bright, happy heirloom recipe book for Noor and Yahya Khan, and for all the generations to come. Some recipes are Noor's, told the way our family has always cooked, by feel and by love. Some are Yahya's, measured to the gram. All of them are ours.

## View the cookbook

- **Live page (HTML):** [index.html](index.html) — live at https://nfairoza.github.io/khanfamilyrecipes/
- **Cover:** [cover.pdf](cover.pdf) · **Binder spine (1"):** [binder-spine.pdf](binder-spine.pdf)

Each recipe is its own print-ready PDF (US Letter, 3-ring-binder margin) so you can print just the one you need and file it behind the right divider tab.

## Table of contents

The cookbook is organized into colored sections, each with its own divider tab. Sections are created as recipes are added.

### 🫓 Breads & Bakes
[Section divider](dividers/breads-bakes.pdf)

| Recipe | Cook | View | Print |
|--------|------|------|-------|
| Munshi Naan | Yahya | [HTML](recipes/munshi-naan/munshi-naan.html) | [PDF](recipes/munshi-naan/munshi-naan.pdf) |
| Everyday Loaf | Yahya | [HTML](recipes/everyday-loaf/everyday-loaf.html) | [PDF](recipes/everyday-loaf/everyday-loaf.pdf) |

### 🍲 Salans
[Section divider](dividers/salans.pdf)

| Recipe | Cook | View | Print |
|--------|------|------|-------|
| Bhendi Ghosht | Noor | [HTML](recipes/bhendi-ghosht/bhendi-ghosht.html) | [PDF](recipes/bhendi-ghosht/bhendi-ghosht.pdf) |
| Aloo Ghosht White Curry | Noor | [HTML](recipes/aloo-ghosht-white-curry/aloo-ghosht-white-curry.html) | [PDF](recipes/aloo-ghosht-white-curry/aloo-ghosht-white-curry.pdf) |

### 🌶️ Chutneys
[Section divider](dividers/chutneys.pdf)

| Recipe | Cook | View | Print |
|--------|------|------|-------|
| Palak Ulikaram | Noor | [HTML](recipes/palak-ulikaram/palak-ulikaram.html) | [PDF](recipes/palak-ulikaram/palak-ulikaram.pdf) |
| Kakdi Chutney | Noor | [HTML](recipes/kakdi-chutney/kakdi-chutney.html) | [PDF](recipes/kakdi-chutney/kakdi-chutney.pdf) |

### 🦐 Seafood
[Section divider](dividers/seafood.pdf)

| Recipe | Cook | View | Print |
|--------|------|------|-------|
| Jhinga Achari Fry | Noor | [HTML](recipes/jhinga-achari-fry/jhinga-achari-fry.html) | [PDF](recipes/jhinga-achari-fry/jhinga-achari-fry.pdf) |
| Jhinga Palak | Noor | [HTML](recipes/jhinga-palak/jhinga-palak.html) | [PDF](recipes/jhinga-palak/jhinga-palak.pdf) |

### 🥔 Vegetables
[Section divider](dividers/vegetables.pdf)

| Recipe | Cook | View | Print |
|--------|------|------|-------|
| Aloo Fry | Noor | [HTML](recipes/aloo-fry/aloo-fry.html) | [PDF](recipes/aloo-fry/aloo-fry.pdf) |

*(This list grows as recipes are added.)*

## How it's made

This repo includes a reusable Claude skill that builds the cookbook. Paste in a recipe and it:

- auto-sorts it into the right section (and invents new sections as needed),
- writes it in Noor's ancestral voice or Yahya's precise voice,
- lays out hero photos, scattered polaroid collages, and full-page photos,
- drafts captions you can tweak,
- outputs both the HTML and a print-ready PDF,
- and keeps this table of contents up to date.

## Reuse the skill (even from another Claude account)

The skill lives in [`skills/khan-family-cookbook/`](skills/khan-family-cookbook/SKILL.md). Two ways to use it elsewhere:

1. **Install the packaged skill:** download [`khan-family-cookbook.skill`](khan-family-cookbook.skill) and install it in Claude.
2. **Copy the folder:** clone this repo and copy `skills/khan-family-cookbook/` into your Claude skills directory.

The design spec is fully self-contained in `SKILL.md`, so the cookbook looks the same wherever it runs.

## Folder layout

| Path | What it is |
|------|------------|
| `index.html` | Landing page / table of contents (also the GitHub Pages site) |
| `cover.html` / `cover.pdf` | The cover, with the family photo |
| `dividers/` | One divider page per section (e.g. `breads-bakes`) |
| `recipes/<slug>/` | One self-contained folder per recipe: its HTML + print PDF |
| `binder-spine.html` / `.pdf` | 1" binder spine insert |
| `skills/khan-family-cookbook/` | The reusable skill (SKILL.md, references, assets) |
| `khan-family-cookbook.skill` | The skill, zipped for one-click install |
