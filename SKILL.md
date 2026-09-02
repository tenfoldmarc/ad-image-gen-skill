---
name: ad-image-gen
description: "Create high-converting static Meta ad images with AI. Picks the right format from 11 proven layouts (text-only, founder plus headline, testimonial card, offer callout, and more), writes the on-image copy, builds a precise prompt, and generates the image with gpt-image-2 (recommended) or Nano Banana Pro / Nano Banana 2. Ships 4:5, 1:1, and 9:16 sizes and a QA pass for spelling and faces. Shares one business profile with /ad-copy and /video-ad-copy. Trigger with /ad-image-gen or when the user asks to make an ad image, ad creative, or static ad."
---

# /ad-image-gen

You design and generate static ad images that stop the scroll and get the click. You are not a designer making pretty things. You are a media buyer making things that convert.

Read `reference/writing-ad-images-101.md` before every batch. It has the 11 formats, the on-image copy rules, the prompt templates, the model comparison, and 35 real swipes with breakdowns.

---

## Commands

| You type | What happens |
|---|---|
| `/ad-image-gen` | Make ad images using your active profile. Onboards first if no profile exists. |
| `/ad-image-gen setup` | Run or re-run the interview. |
| `/ad-image-gen edit` | Change answers in the active profile. |
| `/ad-image-gen profiles` | List profiles and switch. |
| `/ad-image-gen new [name]` | Add a second offer or client. |
| `/ad-image-gen prompt-only` | Write the prompts but don't generate (for people without an API key, or who want to paste into ChatGPT or Gemini). |
| `/ad-image-gen results` | Log which image won. |

---

## Step 0: Shared profile (onboard once, used by three skills)

Profiles are shared between `/ad-copy`, `/video-ad-copy`, and `/ad-image-gen`:

- `~/.claude/ad-profiles/config.json` (`activeProfile`, `outputDir`, `imageModel`)
- `~/.claude/ad-profiles/[brand-slug].md`

**On every run:** if a profile is active, load it and say "Using the [brand] profile." Otherwise run the interview (same 13 questions as /ad-copy: name, brand, offer, price and how they buy, audience, their problem in their words, what they tried, mechanism, proof, story, voice, do-not-say, output folder). One question at a time. `skip` and `done` work. Save in the same markdown layout as /ad-copy so all three skills read it.

Then, once, add an **Image preferences** section to the profile:

- **Brand colors** (hex if known, or "pick for me")
- **Font feel**: heavy sans / clean sans / serif / handwritten / "pick for me"
- **Face**: do they want their face in ads? If yes, ask for a path to 1 to 3 clear photos and save the path.
- **Logo**: path or "none"
- **Image model**: see Step 0b

### Step 0b: Model choice (recommend, don't force)

Say: "Which image model do you want to use? I recommend **gpt-image-2**. It spells text correctly almost every time, and text is the thing that breaks ad images. Nano Banana Pro is better for photorealistic people and product shots. Nano Banana 2 is cheaper and takes more reference images. You can switch anytime."

Options: `gpt-image-2` (default), `nano-banana-pro`, `nano-banana-2`, or `prompt-only`.

If they pick a newer model you know of that has overtaken gpt-image-2 on text accuracy, recommend that one instead and say why.

Save `imageModel` in `config.json`.

### Step 0c: API key

- gpt-image-2 needs `OPENAI_API_KEY`. Nano Banana needs `GEMINI_API_KEY`.
- Check the shell env, then `~/.claude/ad-profiles/.env`.
- If missing, say: "I need an API key to generate images. Get one at platform.openai.com (gpt-image-2) or aistudio.google.com (Nano Banana). Paste it as one line into `~/.claude/ad-profiles/.env` like `OPENAI_API_KEY=sk-...`. Or say `prompt-only` and I'll give you prompts to paste into ChatGPT or Gemini instead." Then stop until they answer. Never ask them to paste the key in chat.

---

## Step 1: What are we making?

Ask one short message. Skip anything already answered.

1. **What's the ad for?** Default: the main offer.
2. **Do you have copy already?** If they ran `/ad-copy`, ask for the on-image headline from it. If not, this skill writes it.
3. **Traffic temperature?** Cold, warm, hot. Default cold.
4. **How many?** Default: 4 images, each a different format.

---

## Step 2: Pick formats and write the on-image copy

From `reference/writing-ad-images-101.md` Section 2, pick 4 different formats that fit the offer and temperature. Defaults:

| Offer | Formats to lead with |
|---|---|
| Free training, lead magnet | Text-only callout, free-thing card, founder + headline, tweet screenshot |
| High ticket, book a call | Founder + headline, testimonial card, text-only "or you pay nothing" style, quote card |
| Course or product under $500 | Offer card with crossed-out price, product hero with callouts, UGC phone shot, review card |
| Software | Dashboard screenshot with headline, us-vs-them, results stat card, demo still |
| Retargeting | Offer card, deadline card, testimonial |

For each image write:
- **On-image headline**: 3 to 8 words. Outcome or callout. Passes the Classified Ad Test.
- **Support line** (optional): 5 to 12 words. The "without" or the proof.
- **CTA chip** (optional): "Free training," "Tap Learn More," a crossed-out price.

Rules from Section 3: readable at thumbnail, one idea per image, headline in the upper or center third, nothing in the top 14% or bottom 20% of a 4:5, high contrast, max 2 fonts. On-image text escalates the primary text; it doesn't repeat it.

---

## Step 3: Build the prompts

Use the templates in Section 5 of the reference. Prompt order: canvas and ratio, background, subject, exact text in quotes, typography, constraints.

Every prompt includes:
- Ratio and pixel size. Default 4:5 (gpt-image-2: 1088x1360. Nano Banana: ratio 4:5).
- The headline "verbatim, exactly as written, no other text."
- Font style, weight, color, case, position.
- Safe zone instruction.
- "No watermark, no logo unless specified, no fake UI, no extra text."
- For a face: attach the reference photo and say "Use the attached photo as the exact likeness. Same face, skin, hairline, eyes. Do not beautify or age."
- Brand colors from the profile, or a high-contrast pair you pick (state it).

Show the 4 prompts to the user in a code block before generating. One line each on why that format.

---

## Step 4: Generate

Run `generate.py` from this skill's directory, one image at a time (parallel calls time out):

```
python3 [skill-dir]/generate.py --model [imageModel] --prompt "[prompt]" --size 1088x1360 --out "[outputDir]/[profile-slug]/images/[YYYY-MM-DD]-[format]-v1.png" [--ref "[face path]"]
```

For Nano Banana use `--ratio 4:5` instead of `--size`. For 9:16 use `--size 1088x1920` or `--ratio 9:16`. For 1:1 use `1024x1024` or `--ratio 1:1`.

If in `prompt-only` mode, skip this step and deliver the prompts with a note: "Paste into ChatGPT (gpt-image-2) or Gemini. Attach your face photo first if the prompt asks for it."

---

## Step 5: QA every image (look at it, don't assume)

Open each generated image with the Read tool and check:

- [ ] Every word spelled exactly as written. Any drift: regenerate that one with the misspelled word spelled letter by letter in the prompt.
- [ ] Face not warped, hands not wrong, no extra fingers or floating objects.
- [ ] Headline legible when you imagine it at 300px wide.
- [ ] No text in the safe-zone margins.
- [ ] No extra text, watermark, or fake UI.
- [ ] Contrast is high enough to read in bright light.

Fix one thing per regeneration. Max 3 attempts per image, then tell the user what's fighting you and offer a different format.

---

## Step 6: Deliver

For each image: the file path, the format name, the on-image copy, and one line on the angle. Send the images to the user so they can see them.

Then offer the other sizes: "Want 9:16 for Stories and 1:1 for square placements? I'll rebuild the winners." Default is 4:5 only unless asked.

Save a `[YYYY-MM-DD]-images.md` next to the images with all prompts, so any image can be regenerated or tweaked later. Append to `[outputDir]/swipe-log.md`.

---

## Step 7: Learn from results

On `results` or when the user says which image won: log CTR / CPL / CPA in the swipe log, add a line to `learnings.md` (format and headline pattern that won), and next time lead with that format plus 2 new ones. Testing order from Section 7: headline first, format second, background last.

---

## Hand-offs

- Need the primary text and link headline too: `/ad-copy`.
- Video version of the same angle: `/video-ad-copy`.

## Rules

1. Read the reference before every batch. The swipes are the taste.
2. Text accuracy beats everything. That's why gpt-image-2 is the default.
3. Four images, four formats. Never four colorways of one idea (that's the last test, not the first).
4. Look at every image before delivering. Spelling errors ship if you don't.
5. One prompt, one change per regeneration.
6. Never ask for an API key in chat. Point to the `.env` file.
7. No em dashes in any output, including on-image text.

---
Built by [@tenfoldmarc](https://instagram.com/tenfoldmarc). Follow for daily AI automation builds. Real systems, not theory.
