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

## Delivery — MANDATORY IN EVERY VIDEO (never skip, never ask)

Every video prompt MUST make the person sound like a REAL human talking to a REAL human — never a script read, never a narrator, never a robotic ad read. Bake this into the **Delivery** direction of every single prompt automatically, without the user having to ask:

- Include an explicit **Delivery** paragraph that says: he/she is NOT reading, narrating, or announcing to a camera — they are genuinely talking to real people, like telling a close friend or explaining to a client they care about.
- Conversational, warm, natural human rhythm. Real pauses and emphasis on the key words. Laughs / reacts where natural. Genuinely animated and expressive in voice and face — real excitement, like they actually find this interesting and can't wait to share it. Energy stays up and NEVER trails off, flattens, or goes bland.
- **Controlling the HANDS must never flatten the VOICE.** The calm/minimal-gesture rule (no face/head touching) describes hands ONLY. Keep the vocal and facial energy lively, warm and genuinely excited at all times. Do NOT describe the overall delivery with flat words like "calm," "matter-of-fact," or "grounded" — those bleed into the voice and make it a bland ad read. Energize the per-beat tone cues (e.g. "lights up," "leaning in with real conviction," "genuinely excited to share this").
- Closing lines must land as genuine feeling (sincere advice, real excitement) — NOT a scripted tagline or CTA read.
- Frame it as a candid, overheard-real-moment when casual; as a genuine coach-explaining-to-a-client tone when educational. Never "piece to camera" energy.

### Golden rules learned (apply to every video)
- **Change HOW they say it, not WHAT they say.** Keep the user's exact words unless they ask for changes.
- **Open and close on energetic words** — never end a line on a flat product word like "app."
- **Spell tricky words/brands phonetically so TTS says them right:** write it the way it actually SOUNDS. IMPORTANT: hyphens tell the TTS to spell letter-by-letter, so only hyphenate genes said as letters ("p-p-a-r-g", "f-t-o"). Genes said as words/syllables must be plain words with SPACES, never hyphens (CYP1A2 = "sip one ay two", NOT "sip-one-ay-two"; "In and Out"). Never trust the model to pronounce acronyms/brands. For any non-obvious gene, CONFIRM the pronunciation with the user before generating, then record it in GENE_TALLY.md so it's reused (never re-guessed).
- **Put the phonetic spelling ONCE — only inside the spoken dialogue line.** NEVER also add a separate stage cue like "saying the gene name letter by letter: X" right before the line that already contains X, and don't repeat the spelled letters in the Delivery paragraph either. Duplicating it makes the model say the gene name TWICE (a wasted generation). One instance, in the spoken line, only.
- **One clear hand/phone movement at a time** — no vague "gesturing" (causes weird AI hands). Anchor a hand (on hip / at side).
- **NEVER write "talks with his hands" or "gestures naturally"** — that invites random mismatched gestures (e.g. finger to the head/temple, touching the face). Instead specify calm, minimal, open-palm gestures kept at chest/torso level, and explicitly state: he does NOT touch his face or head, does NOT point at his head, and makes no odd or exaggerated gestures. When in doubt, anchor one hand and keep the other relaxed.
- **Always a modern iPhone**, never "walkie-talkie" (adds an antenna).
- **Friend-filmed = the friend IS the camera and is NEVER in frame** (unless friends are meant to be visible at a table). State "only [person] is visible, no extra hands or arms."
- **~3 words/second. ~45 words max for a 15s clip.** Don't cram — it rushes and gets cut off. Flag when a script is too long and offer a trim (videos are 15s only).
- **Frame WIDE enough that the body isn't cut off on the sides.** In 9:16, a "medium shot waist-up" lets the subject drift too close so arms/shoulders clip the edges when they gesture. Use a medium-wide shot from roughly the hips up, subject centered with clear margin on BOTH sides, never a tight close-up — and state that even when gesturing, arms and shoulders stay fully in frame and are not cropped by the edges.
- **Keep it ONE continuous take — no cuts.** Do NOT describe cutaway-bait: a food spread, product close-ups, or "gesture toward the [object]." Seedance treats those as separate shots and inserts disjointed B-roll that doesn't match, turning one clip into a choppy multi-scene mess. Keep the camera on the person the whole time and state explicitly: "a single continuous shot, no cuts, no edits, no scene changes; the camera stays on him the entire time." Matching props may exist subtly in the background but must NEVER be a described focal object to cut to, and never tell the subject to gesture at them.

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
