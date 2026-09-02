# Ad Image Gen
### A Claude Code Skill by [@tenfoldmarc](https://www.instagram.com/tenfoldmarc)

Tell Claude what you're promoting. It picks 4 proven static ad formats, writes the words that go on each image, builds a precise prompt, generates the images with gpt-image-2 (or Nano Banana if you prefer), then looks at every image and checks the spelling and faces before you see them.

It's built on a swipe file of 35 real ads pulled from Hormozi, Skool, Dan Henry, ClickFunnels, Sabri Suby, Frank Kern, Grant Cardone, Iman Gadzhi, HubSpot, AG1, and more, each broken down by format and pattern. That playbook ships inside the skill.

---

## What It Does

1. First run only: a short interview about your business, one question at a time. Shared with `/ad-copy` and `/video-ad-copy`, so if you ran those, it skips this.
2. Asks your brand colors, font feel, whether you want your face in ads (and where the photos are), and which image model to use. It recommends gpt-image-2 because it spells text right.
3. Asks what today's ad is for and whether the traffic is cold, warm, or hot.
4. Picks 4 different formats (text-only callout, founder plus headline, testimonial card, offer card, and 7 more) and writes the on-image copy for each.
5. Shows you the 4 prompts, then generates the images one at a time in 4:5 (the best-performing feed size).
6. Opens every image and checks spelling, faces, legibility, and safe zones. Regenerates anything that's off.
7. Saves the images and prompts, and learns from your results next time.

No API key? Say `prompt-only` and it hands you prompts to paste into ChatGPT or Gemini.

---

## Commands

| Type this | What it does |
|---|---|
| `/ad-image-gen` | Make 4 ad images for your active profile |
| `/ad-image-gen prompt-only` | Write the prompts, don't generate |
| `/ad-image-gen setup` | Run or re-run the interview |
| `/ad-image-gen edit` | Change one answer (colors, face photos, model) |
| `/ad-image-gen new [name]` | Add a second offer or client |
| `/ad-image-gen profiles` | Switch between profiles |
| `/ad-image-gen results` | Tell it which image won |

---

## Requirements

- A Mac, Linux, or Windows computer
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- Python 3 (already on Mac and most Linux; Windows: python.org)
- One of these, for generating images:
  - An OpenAI API key for gpt-image-2 (recommended): platform.openai.com
  - A Google Gemini API key for Nano Banana Pro / Nano Banana 2: aistudio.google.com
  - Or no key at all: use `prompt-only` mode and paste prompts into ChatGPT or Gemini

The skill tells you exactly where to put the key on first run. You never paste it into chat.

---

## Install

No terminal needed.

### Step 1: Open Claude

Open the Claude Desktop app (or Claude Code, if you already use it).

### Step 2: Paste this message

```
Install this skill for me: https://github.com/tenfoldmarc/ad-image-gen-skill
```

### Step 3: Run the skill

```
/ad-image-gen
```

<details>
<summary>Prefer the terminal? Manual install</summary>

```bash
git clone https://github.com/tenfoldmarc/ad-image-gen-skill ~/.claude/skills/ad-image-gen
```

Then type `claude` and run `/ad-image-gen`.
</details>

---

## Usage

**Four images for a free training**
```
/ad-image-gen Free training on booking sales calls. Cold traffic.
```

**Use the headline from /ad-copy**
```
/ad-image-gen Use this on-image headline: "27 booked calls in 9 days"
```

**No API key**
```
/ad-image-gen prompt-only
```

**Feeding results back**
```
/ad-image-gen results The text-only red one won. $3.80 CPL.
```

---

## What's Inside

- `SKILL.md`: the skill
- `generate.py`: the image generator (OpenAI and Gemini, no extra packages)
- `reference/writing-ad-images-101.md`: the playbook. 11 formats, on-image copy rules, prompt templates, model comparison, Meta sizes, testing framework
- `reference/swipes/`: 35 real ad images with breakdowns

Your profile and your API key live at `~/.claude/ad-profiles/` on your machine, shared with [ad-copy](https://github.com/tenfoldmarc/ad-copy-skill) and [video-ad-copy](https://github.com/tenfoldmarc/video-ad-copy-skill).

---

## Updating

Paste this into Claude:

```
Update the ad-image-gen skill from https://github.com/tenfoldmarc/ad-image-gen-skill
```

---

## Built By

[@tenfoldmarc](https://www.instagram.com/tenfoldmarc). Follow for daily AI automation walkthroughs. Real systems, not theory.
