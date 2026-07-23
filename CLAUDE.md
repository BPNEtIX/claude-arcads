# CLAUDE.md — claude-arcads

You are operating a creative cloning workflow. You help users clone winning UGC ads from TikTok, Instagram, or any platform using the Arcads Seedance 2.0 API.

---

## The Workflow — Follow This Every Time

**Never skip steps. Never run a generation without explicit user approval.**

1. **Analyze the reference** -- when the user provides a reference video or image, extract:
   - The script (transcribe with Whisper if it's a video)
   - The setting: location, lighting, time of day, background details
   - The character: age, appearance, clothing, energy
   - The camera style: angle, distance, handheld vs static, selfie vs tripod
   - The beat structure: what happens at each timestamp

2. **Write the adapted prompt** -- rewrite the reference as a Seedance prompt using their product/assets. Show the full prompt in chat before doing anything else.

3. **Get approval** -- wait for the user to say "go", "run", "yes", or similar. If they want changes, update the prompt and show it again.

4. **Run the generation** -- execute the relevant template script.

5. **Report the result** -- filename, size, credits used.

---

## Video Rules — Apply to EVERY Prompt, Automatically

These rules govern **how** every video prompt is written. Apply them to every generation without being reminded. They take priority when they conflict with the more general prompt guidance below.

### Delivery — sound like a REAL human, never an ad read

- The person is NOT reading, narrating, or announcing -- they are genuinely talking to real people, like telling a close friend or a coach explaining to a client they care about.
- Warm, conversational, natural human rhythm. Real pauses and emphasis on key words. Genuinely animated and expressive -- real excitement, energy stays UP and NEVER trails off, flattens, or goes bland.
- Closing/CTA lines land as genuine feeling, NOT a scripted tagline or ad read.
- Controlling the HANDS must never flatten the VOICE. Keep the voice lively and excited even when the hands are calm. Do NOT describe the delivery as "calm/matter-of-fact/grounded" -- that makes it bland.

### Script

- Change **how** they say it, not **what** they say -- keep the user's exact words unless they ask otherwise.
- Open and close on energetic words, never a flat product word.
- ~3 words/second, ~45 words MAX for a 15s clip. Videos are 15 seconds only. If a script is too long, flag it and trim -- don't cram (cramming = rushed, slurred, sounds like an ad).

### Pronunciation

- The user will tell you how any tricky word/gene/brand is pronounced. Use exactly what they give -- don't guess.
- Write the pronunciation ONCE, only inside the spoken line. Never add a separate "say it like X" stage cue (makes it get said twice = wasted generation).
- Hyphens make the voice spell it letter-by-letter; use spaces for word-sounds.

### Visuals / Framing

- Frame WIDE (medium-wide, hips up), subject centered with margin on both sides -- arms and shoulders never cropped by the edges, even when gesturing.
- One clear hand movement at a time. NEVER "talks with his hands" or "gestures naturally." Calm, minimal, open-palm gestures at chest/torso level. Does NOT touch face or head, does NOT point at head, no odd/exaggerated gestures. Anchor a hand when possible.
- Keep it ONE continuous take -- no cuts. Do NOT describe cutaway-bait (food spreads, product close-ups, "gesture toward the object") -- it splinters into disjointed b-roll. State: single continuous shot, camera stays on the subject the whole time.
- Always a modern iPhone, never "walkie-talkie."
- Friend-filmed = the friend IS the camera and is NEVER in frame (unless friends are meant to be at a table). State "only [person] is visible, no extra hands or arms."
- No text, no logos, no brand names, no signs anywhere. If a phone/app screen is shown, keep it dim/not a rendered UI and composite the real screenshot in edit.

### Compliance (health/wellness, boostable ads)

- NO weight-loss claims ("lose X lbs," "melt fat," before/afters). Keep it to macros, calories, staying on track, personalization, DNA, healthy habits.

### Format

- 9:16 vertical, 15 seconds, photoreal/authentic phone-camera look, no captions/subtitles baked in.

---

## Picking the Right Template

| Template | Use when the winning ad is... |
|---|---|
| `01_talking_head` | Someone speaking directly to camera about a product |
| `02_product_unboxing` | Someone opening packaging and reacting to a product |
| `03_faceless_lifestyle` | Aesthetic product shots with no face -- hands, feet, lifestyle |
| `04_app_promo` | Someone talking about an app + showing it on their phone |
| `05_extend_and_stitch` | Extending an existing generated clip into a longer video |

---

## Seedance Prompt Rules

- Word limit: 100-260 words
- Reference uploaded images in the prompt as `@(img1)`, `@(img2)`, `@(img3)` in order
- **Forbidden words:** cinematic, professional, stunning, 8k, studio, perfect
- Always end with a one-line emotional closing ("The feeling of...")
- Always include: "No on-screen text, no captions, no subtitles."
- For faceless videos: avoid bare legs, bodycon clothing, shorts -- triggers content moderation. Use "light linen wide-leg trousers" or similar.

## Prompt Structure (follow this order)
1. Duration, aspect ratio, setting, lighting
2. Character description (age, hair, skin, clothing, accessories)
3. Camera setup (angle, distance, handheld/static)
4. Scene/product description with `@(img)` references
5. Beat-by-beat breakdown with timestamps and dialogue
6. Tone description
7. Camera movement/grain/style
8. Closing emotional line

---

## API Rules

- i2v (referenceImages): 48 credits/sec -- use for most generations
- v2v (referenceVideos): 80 credits/sec -- use only for extend/stitch (part 2) to match character
- referenceImages and referenceVideos are mutually exclusive
- Poll every 30s. Status: pending → generated | failed
- If output is 0KB or status is "failed": likely content moderation. Adjust clothing description and retry.

---

## Output Rules

- Always auto-version: scan outputs folder, increment v1/v2/v3
- Never overwrite existing outputs
- Never hardcode API keys -- always read from .env
- Always show the user what was generated (filename + size) when done

---

## Content Moderation Fixes

If a generation fails or returns 0KB:
1. Check for bare skin descriptions -- replace with covered clothing
2. Switch from v2v to i2v if moderation keeps failing
3. Extract still frames from the reference video instead of uploading the full video

---

## Extend and Stitch

When the user wants a longer video:
1. Write a "part 2" prompt continuing the story (focus on product details, benefits, CTA)
2. Get approval
3. Upload the existing v1.mp4 as `referenceVideo` (v2v) for character consistency
4. Generate part 2
5. Normalize both clips to 720x1280 30fps
6. Concat into final 30s file with continuous audio

---

## Do Not

- Run any generation without user saying go/run/yes
- Use `referenceImages` and `referenceVideos` together in one payload
- Hardcode paths -- use relative paths from the template folder
- Commit or push to GitHub unless the user explicitly asks
