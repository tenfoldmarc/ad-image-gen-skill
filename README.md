# Ad Image Gen
### A Claude Code Skill by [@tenfoldmarc](https://www.instagram.com/tenfoldmarc)

Tell Claude what you're promoting. It picks 4 proven static ad formats, writes the words that go on each image, builds a precise prompt, generates the images with gpt-image-2 (or Nano Banana Pro if you prefer), looks at every image and checks spelling and faces before you see them, then asks which ones you approve so it gets better every batch.

It's built on a swipe file of 35 real ads pulled from Hormozi, Skool, Dan Henry, ClickFunnels, Sabri Suby, Frank Kern, Grant Cardone, Iman Gadzhi, HubSpot, AG1, and more, each broken down by format and pattern. That playbook ships inside the skill.

---

## What It Does

1. First run only: a short interview about your business, one question at a time. Shared with `/ad-copy` and `/video-ad-copy`, so if you ran those, it skips this.
2. Asks your brand colors, font feel, and whether you want your face in ads. Then it looks at what you already have connected to Claude (image connectors, API keys) and recommends gpt-image-2 because it spells text right. Nano Banana Pro is the other option. Nothing found? It asks if you have a key, or runs in prompt-only mode.
3. Asks what today's ad is for and whether the traffic is cold, warm, or hot.
4. Picks 4 different formats (text-only callout, founder plus headline, testimonial card, offer card, and 7 more) and writes the on-image copy for each.
5. Shows you the 4 prompts, then generates the images one at a time in 4:5 (the best-performing feed size).
6. Opens every image and checks spelling, faces, legibility, and safe zones. Regenerates anything that's off.
7. Asks which images you approve and why. Approved and rejected images are stored with your notes, and the next batch starts from what you liked.
8. Logs which ads actually won in your ad account and leads with those patterns.

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
| `/ad-image-gen feedback` | Approve or reject the last batch, leave notes |
| `/ad-image-gen results` | Tell it which image won in your ad account |

---

## Requirements

- A Mac, Linux, or Windows computer
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- Python 3 (already on Mac and most Linux; Windows: python.org)
- One of these, for generating images (the skill finds what you have and sets it up):
  - An image connector already in your Claude (Arcads, Higgsfield, fal, or any that runs gpt-image-2 or Nano Banana Pro)
  - An OpenAI API key for gpt-image-2 (recommended): platform.openai.com
  - A Google Gemini API key for Nano Banana Pro: aistudio.google.com
  - Or nothing: use `prompt-only` mode and paste prompts into ChatGPT or Gemini

If you use a key, the skill tells you the one file to put it in. You never paste it into chat, and it never leaves your machine.

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

**Approving a batch**
```
/ad-image-gen feedback
```
It shows the images, you say "1 and 3, and the font on 2 is too thin." It sorts them and remembers.

**Feeding results back**
```
/ad-image-gen results The text-only red one won. $3.80 CPL.
```

---

## What's Inside

- `SKILL.md`: the skill
- `generate.py`: the image generator for API keys (OpenAI gpt-image-2 and Gemini Nano Banana Pro, no extra packages)
- `reference/writing-ad-images-101.md`: the playbook. 11 formats, on-image copy rules, prompt templates, model comparison, Meta sizes, testing framework
- `reference/swipes/`: 35 real ad images with breakdowns

Your profile, your feedback history, and any API key live at `~/.claude/ad-profiles/` and your output folder on your machine, shared with [ad-copy](https://github.com/tenfoldmarc/ad-copy-skill) and [video-ad-copy](https://github.com/tenfoldmarc/video-ad-copy-skill).

---

## Updating

Paste this into Claude:

```
Update the ad-image-gen skill from https://github.com/tenfoldmarc/ad-image-gen-skill
```

---

## Built By

[@tenfoldmarc](https://www.instagram.com/tenfoldmarc). Follow for daily AI automation walkthroughs. Real systems, not theory.
