# DesignX Image Generation Pipeline
## Claude Code Walkthrough

This pipeline generates all the visual assets for the DesignX + DLS style guide using AI image generation.

---

## Architecture

```
prompts.json        →  generate.py      →  outputs/
(20+ brand prompts)    (Ideogram API)       ├── designx_dark/
                                            │   ├── dx-social-salary.png
                                            │   ├── dx-youtube-thumb.png
                                            │   └── ...
                                            ├── dls_light/
                                            │   ├── dls-speaker-olive.png
                                            │   ├── dls-talk-title.png
                                            │   └── ...
                                            ├── archetypes/
                                            │   ├── arch-reveal-ally.png
                                            │   └── ...
                                            ├── gradient_spectrum/
                                            │   └── spectrum-poster.png
                                            └── manifest.json
```

---

## Step 1: Get Your Ideogram API Key

1. Go to [ideogram.ai](https://ideogram.ai) and sign in (free account works)
2. Click your **Profile icon** → **API Beta**
3. Accept the Developer Agreement
4. Click **Manage Payment** → add a card (you're only charged when you generate)
5. Click **Create API Key** → **copy it immediately** (shown only once)

**Cost**: ~$0.04-0.08 per image. Full style guide (~25 images) ≈ **$1-2 total**.

---

## Step 2: Open Claude Code

Open your terminal and navigate to wherever you want this project:

```bash
# If you haven't used Claude Code much, just open terminal and type:
claude

# Or if you want to work in a specific folder:
cd ~/Projects/designx-style-guide
claude
```

---

## Step 3: Tell Claude Code What to Do

Copy-paste this into Claude Code as your first message:

```
I have a visual asset generation pipeline for DesignX brand.

Here's what I need:
1. Install the requests library: pip install requests
2. Set my Ideogram API key as an environment variable
3. Run the generate.py script to create branded images

My API key is: [paste your key here]

The files are:
- prompts.json (20+ brand-specific image prompts)
- generate.py (calls Ideogram API and downloads results)

Let's start with a dry run to see all the prompts, then generate 
the DLS speaker cards first since those are the most complex.
```

Or, if you want to do it step by step yourself:

---

## Step 3 (Manual): Run It Yourself

### Set your API key
```bash
export IDEOGRAM_API_KEY="ideo_your_key_here"
```

### Install dependencies
```bash
pip install requests
```

### See all available assets
```bash
python generate.py --list
```

Output:
```
📋 Available assets:

  ┌─ DESIGNX_DARK (8 assets)
  │  dx-social-salary              → DesignX Social Card — Salary Tool
  │  dx-social-jobs                → DesignX Social Card — Job Board
  │  dx-social-community           → DesignX Social Card — Community
  │  dx-youtube-thumb              → YouTube Thumbnail Template
  │  dx-newsletter-hero            → Newsletter Hero Banner
  │  dx-gradient-hero-purple       → Platform Hero — DLS/Events Section
  │  dx-gradient-hero-salary       → Platform Hero — Salary Section
  │  dx-gradient-hero-jobs         → Platform Hero — Jobs Section
  └─
  ┌─ DLS_LIGHT (6 assets)
  │  dls-speaker-olive             → DLS Speaker Card — Olive
  │  dls-speaker-coral             → DLS Speaker Card — Coral
  │  dls-speaker-teal              → DLS Speaker Card — Teal
  │  dls-talk-title                → DLS Talk Title Card
  │  dls-sponsor                   → DLS Sponsor/Patron Card
  │  dls-website-hero              → DLS Website Hero
  └─
  ┌─ ARCHETYPES (6 assets)
  │  arch-reveal-ally              → Archetype Reveal — The Ally
  │  ...5 more
  └─
  ┌─ GRADIENT_SPECTRUM (1 asset)
  │  spectrum-poster               → 5-Zone Gradient Spectrum Poster
  └─

  Total: 21 assets
```

### Dry run (see prompts, no API calls)
```bash
python generate.py --dry-run
```

### Generate specific assets
```bash
# One at a time
python generate.py --ids dls-speaker-olive

# Multiple
python generate.py --ids dls-speaker-olive dls-speaker-coral dls-speaker-teal

# With 3 variations to choose from
python generate.py --ids dls-speaker-olive --variations 3
```

### Generate by category
```bash
python generate.py --category dls_light
python generate.py --category designx_dark
python generate.py --category archetypes
```

### Generate everything
```bash
python generate.py
```

---

## Step 4: Review and Curate

After generation, browse `outputs/` and pick the best results.

**For Claude Code**: Just say:
```
Open the outputs folder and show me what was generated. 
Which ones look good? Let's regenerate the ones that 
didn't match the brand spec.
```

### Common tweaks to try:

| Problem | Fix |
|---------|-----|
| Text not rendering cleanly | Add `--variations 3` and pick the best |
| Colors too saturated | Edit prompt in prompts.json: add "muted, desaturated" |
| Too photorealistic | Add "flat illustration, graphic design" to prompt |
| Not enough grain/texture | Add "heavy film grain, visible noise, risograph print" |
| Bauhaus shapes look wrong | Specify "flat solid color rectangles, no rotation, no 3D" |

---

## Step 5: Assemble Into Style Guide

Once you have the images, bring them back here to Claude.ai:

```
I generated the DesignX visual assets. Here are the best ones:
[upload the images]

Now assemble them into the visual style guide, slotting each 
image into the matching section of designx-strategy-v2.html.
```

Or in Claude Code:
```
Take the generated images from outputs/ and create an HTML 
style guide that embeds each image in the appropriate section 
alongside the CSS mockups we already have.
```

---

## Manual Midjourney Process (for Hero Shots)

For the 5-10 images where you want absolute peak quality, run these 
prompts directly in Midjourney Discord:

### In Discord, paste:

**DLS Speaker Card:**
```
/imagine Trading card portrait on olive green #7A8B5D background. 
Overlapping Bauhaus rectangles and circles in red, yellow, blue, 
orange — flat, unrotated. B&W halftone portrait center-right, 
visible dot pattern. Heavy paper grain texture. Serif italic 
'Sarah Chen' bottom-left. Zine risograph aesthetic. --ar 3:4 --v 6.1
```

**Archetype Reveal:**
```
/imagine Full screen solid teal #0D9488 background. Large white 
serif italic text 'The Architect' centered. Subtitle 'System-thinker 
· Pattern-finder' at 50% opacity below. Film grain. Minimal, 
cinematic. --ar 16:9 --v 6.1
```

**Gradient Spectrum Poster:**
```
/imagine Five gradient zones flowing left to right: deep purple-black, 
vibrant violet, green-blue-cyan, warm coral-amber, cream-parchment. 
Organic blob shapes within each zone. Design system documentation 
poster. Modern, clean, abstract. --ar 3:1 --v 6.1
```

Save the best results and drop them into the same `outputs/` folder.

---

## File Reference

| File | What it does |
|------|-------------|
| `prompts.json` | All 21 asset prompts with brand specs, aspect ratios, negative prompts |
| `generate.py` | Python script that calls Ideogram API, downloads images, saves manifest |
| `outputs/` | Generated images organized by category |
| `outputs/manifest.json` | Auto-generated log of all generated images with URLs |

---

## Extending the Prompt Library

To add new asset types, edit `prompts.json`:

```json
{
  "id": "dx-new-asset",
  "name": "My New Asset",
  "aspect": "LANDSCAPE_16_9",
  "use": "Where this asset gets used",
  "prompt": "Your detailed prompt here...",
  "negative": "things to exclude"
}
```

Add it to the appropriate category array (`designx_dark`, `dls_light`, `archetypes`, or `gradient_spectrum`).

### Aspect ratio options:
- `LANDSCAPE_16_9` → social cards, YouTube, heroes, reveals
- `PORTRAIT_4_3` → speaker cards, trading cards
- `SQUARE` → Instagram, sponsor cards, share cards

---

## Troubleshooting

**"Missing IDEOGRAM_API_KEY"**
→ Run: `export IDEOGRAM_API_KEY="your-key"` in the same terminal session

**API returns 401**
→ Key is invalid or expired. Generate a new one at ideogram.ai

**API returns 429**
→ Rate limited. Script has built-in 2s delays. Wait a minute and retry.

**Images don't match brand**
→ Edit prompts.json. The global_style_suffix applies to all prompts. 
  Tweak individual prompts for specific issues.

**Want to switch to a different API**
→ Only generate.py needs to change. prompts.json works with any service.
  The prompts are API-agnostic — they're just detailed English descriptions.
