# The Khan Family Cookbook

A bright, happy heirloom recipe book for Noor and Yahya Khan, and for all the generations to come. Some recipes are Noor's, told the way our family has always cooked, by feel and by love. Some are Yahya's, measured to the gram. All of them are ours.

## View the cookbook

- **Live page (HTML):** [index.html](index.html) — once GitHub Pages is on, it will be live at https://nfairoza.github.io/khanfamilyrecipes/
- **Print-ready PDF:** [khan-family-cookbook.pdf](khan-family-cookbook.pdf) — US Letter, sized for a 3-ring binder on cardstock

## Table of contents

The cookbook is organized into colored sections, each with its own divider tab. Sections are created automatically as recipes are added.

### Biryani
- Hyderabadi Chicken Dum Biryani — *from the kitchen of Noor*

### Breads & Bakes
- Blueberry Bran Breakfast Muffins — *from the kitchen of Yahya*

*(This list grows as recipes are added. The current file is a design sample.)*

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

## Files

| File | What it is |
|------|------------|
| `index.html` | The cookbook, viewable in a browser / GitHub Pages |
| `khan-family-cookbook.pdf` | Print-ready PDF for the binder |
| `skills/khan-family-cookbook/SKILL.md` | The reusable skill |
| `khan-family-cookbook.skill` | The skill, zipped for one-click install |
| `khan-cookbook-sample.html` / `.pdf` | The approved design sample |
