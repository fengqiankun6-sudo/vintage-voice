# VintageVoice — Website Integration Brief for Elyan Labs Site

## For the Claude instance working on elyanlabs.ai website

### Overview

VintageVoice is a new Elyan Labs project — the first open-source TTS model for historical speech patterns. It preserves extinct and endangered accents by fine-tuning F5-TTS on 164 hours of public domain vintage audio (1888-1955). The project has expanded to include Cajun French language preservation, which is personal to Scott (his family traces to the 1760s Acadian Expulsion).

**GitHub**: https://github.com/Scottcjn/vintage-voice
**HuggingFace**: https://huggingface.co/Scottcjn/vintage-voice (coming soon)

---

## Page 1: Lab Experiments / Test Results

Create a page at `/lab` or `/experiments` showcasing different AI generation tests. This is where we show work-in-progress, proof of concepts, and technical demos.

### Structure

```
/lab
├── VintageVoice TTS Tests
│   ├── Transatlantic Sophia (SadTalker video demo)
│   ├── Edison-era voice samples
│   ├── Audio comparison: Modern vs Vintage delivery
│   └── Training progress dashboard
├── Multi-LLM Collaboration Tests
│   ├── Claude + GPT-5.4 dual-brain reviews
│   ├── Codex as Claude subagent experiments
│   └── Multi-model consensus (PostMath)
├── Hardware AI Tests
│   ├── POWER8 vec_perm PSE benchmarks
│   ├── PowerPC G4/G5 miner fingerprints
│   └── GPU offload (POWER8 → C4130 40GbE)
└── Video Generation Tests
    ├── LTX-2.3 Sophia videos
    ├── SadTalker lip sync demos
    └── ComfyUI pipeline outputs
```

### VintageVoice Section Content

**Hero text**: "Proof of Antiquity for AI Voices — Dead accents preserved on vintage hardware compute."

**Key stats to display**:
- 164 hours of training data
- 44,345 audio segments
- 8 voice presets (with Sophia images for each)
- Training on 2x V100 32GB + RTX 5070
- Built on a $69 refurb hard drive
- Total project cost: ~$138

**Demo video**: `/lab/demos/sophia_transatlantic_enhanced.mp4`
- SadTalker-generated talking head
- 1940s Sophia image + transatlantic Hepburn-style voice
- Lip-synced to vintage-accented speech
- Caption: "Early prototype — Sophia speaking in transatlantic accent (SadTalker + F5-TTS)"

**Voice preset gallery** — display all 8 Sophia images in a grid:

| Image File | Preset | Era | Description |
|------------|--------|-----|-------------|
| `sophia_edison_1890s.png` | Edison | 1890s | Wax cylinder era, Victorian |
| `sophia_cajun_french_1880s.png` | Cajun French | 1880s | Attakapas prairie, oil lamp |
| `sophia_cajun_1920s.png` | Cajun French | 1920s | Cloche hat, kitchen |
| `sophia_cajun_english_1930s.png` | Cajun English | 1930s | Depression schoolteacher |
| `sophia_fireside_1940s.png` | Fireside | 1940s | By the hearth |
| `sophia_transatlantic_1940s.png` | Transatlantic | 1940s | Tweed blazer, B&W |
| `sophia_newsreel_1940s.png` | Newsreel | 1940s | Press hat, EXTRA! |
| `sophia_radio_drama_1940s.png` | Radio Drama | 1940s | RCA mic, dark studio |
| `sophia_wartime_1940s.png` | Wartime | 1940s | Military blazer |
| `sophia_announcer_1950s.png` | Announcer | 1950s | ON AIR, broadcast booth |

All images are in the GitHub repo at `assets/`.

---

## Page 2: VintageVoice System Explanation

Create a dedicated page at `/vintage-voice` or `/projects/vintage-voice`.

### Section 1: The Problem

Modern TTS models sound modern. No AI can speak with a transatlantic accent, a 1940s newsreel cadence, or the speech patterns of someone born before the Civil War. These voices are disappearing from living memory.

Additionally, Cajun French — a UNESCO "severely endangered" language — has approximately 150,000 speakers remaining, mostly elderly. When this generation passes, the language dies with them.

### Section 2: The Solution

VintageVoice fine-tunes F5-TTS (a 337M parameter flow-matching TTS model) on 164 hours of public domain vintage audio sourced from Archive.org. The model learns historical speech patterns — transatlantic prosody, cadence, pronunciation — and applies them to any modern voice via reference audio cloning.

**Key architectural insight**: F5-TTS separates voice identity (from reference audio) from speech style (from training data). Fine-tuning teaches vintage delivery. At inference, you provide any voice as reference, and the model generates speech with that voice but vintage delivery.

**Discovery during development**: The model generates the best transatlantic accent when given a vintage reference audio (e.g., Katharine Hepburn from The Philadelphia Story, 1940). The reference audio provides both voice character AND accent — the fine-tuning reinforces the vintage speech patterns the reference already carries.

### Section 3: Architecture Diagram

```
┌─────────────────────────────────────────────────────┐
│                  TRAINING PIPELINE                    │
│                                                       │
│  Archive.org Public Domain Audio (1888-1955)          │
│  ├── Old Time Radio (The Shadow, Suspense)            │
│  ├── FDR Fireside Chats                               │
│  ├── Newsreels (Movietone, Pathe)                     │
│  ├── Edison Cylinder Recordings                       │
│  └── Cajun French (hymns, Opelousas Sostan, family)   │
│           │                                           │
│           ▼                                           │
│  Whisper Large V3 Turbo (transcription)               │
│           │                                           │
│           ▼                                           │
│  44,345 text-audio aligned segments (164 hours)       │
│           │                                           │
│           ▼                                           │
│  F5-TTS Fine-Tuning (337M params)                     │
│  ├── V100 32GB: Transatlantic preset (epoch 11/50)    │
│  └── RTX 5070:  Edison preset (epoch 15/50)           │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│                 INFERENCE PIPELINE                    │
│                                                       │
│  Vintage Reference Audio ──┐                          │
│  (Hepburn, FDR, newsreel)  │                          │
│                             ▼                         │
│                    F5-TTS Model ──► Generated Speech   │
│                             ▲       (vintage accent   │
│  Text Prompt ──────────────┘        + reference voice) │
│  "Good evening darling..."                            │
│                                                       │
│  Optional: Voice Conversion ──► Sophia's timbre       │
│  Optional: SadTalker/LTX ──► Talking head video       │
└─────────────────────────────────────────────────────┘
```

### Section 4: Cajun French Preservation

This project is personal. Scott Boudreaux's family traces directly to the Acadian Expulsion of 1755-1764. His 6th great-grandfather, Augustine Dit Remi Boudreaux, arrived in the Attakapas region (Opelousas/Lafayette) at age 16, separated from his family.

260 years later, his grandmother (93, with dementia) is one of the last native Cajun French speakers. Her sister and brothers, in their 80s-90s, are among the final generation. VintageVoice is collecting family recordings and public domain Cajun French audio to build the first TTS model for this endangered language.

**UNESCO status**: Cajun French is classified as "severely endangered." Louisiana Creole is "critically endangered."

**What we're collecting**:
- Archive.org Cajun French collections
- Alan Lomax field recordings from Louisiana
- Cajun music with spoken intros (Opelousas Sostan, Amazing Grace in Cajun French)
- Family recordings from native speakers (recording guide available)

**Link to**: [Family Recording Guide](https://github.com/Scottcjn/vintage-voice/blob/main/FAMILY_RECORDING_GUIDE.md)

### Section 5: How It Was Built

**Total cost: ~$138**

| Component | Cost | What |
|-----------|------|------|
| Storage | $69 | 18TB Seagate Expansion (Amazon refurb, tested with dd before purchase) |
| GPUs | $0 | 2x Tesla V100 32GB + RTX 5070 (already in lab) |
| Training data | $0 | Public domain from Archive.org |
| Base model | $0 | F5-TTS open source |
| Electricity | ~$69 | Estimated for training run |

### Section 6: Commercial Applications

- **Film & TV**: Period productions (Boardwalk Empire, The Crown, Peaky Blinders)
- **Video Games**: Historical NPCs (Bioshock, LA Noire, Fallout)
- **Audiobooks**: Vintage narration style
- **Documentaries**: Authentic period narrator voices
- **Museums**: Historical figures speaking in their actual accent
- **Language Preservation**: Endangered dialect documentation and revival

### Section 7: Current Status

Display as a live-updating dashboard if possible:

| Component | Status |
|-----------|--------|
| Training data | **Complete** — 164 hours, 44,345 segments |
| V100 Transatlantic training | **Epoch 11/50** — loss 0.40 |
| RTX 5070 Edison training | **Epoch 15/50** — loss 0.55 |
| Cajun French data collection | **In Progress** — 2.3GB + 438MB curated |
| Voice conversion (Sophia timbre) | **Research** — OpenVoice/RVC pending |
| HuggingFace model release | **After training completes** |

---

## Assets Available

All in the GitHub repo `assets/` folder:
- 10 Sophia era images (high quality, suitable for web)
- Demo video: `sophia_transatlantic_enhanced.mp4`
- Audio samples available on request

## Tone & Style

- Technical but accessible
- Emphasize the preservation angle — this isn't just cool tech, it's saving dying voices
- The "$69 hard drive" and "pawn shop lab" angle is a great hook
- Scott's personal connection to Cajun French gives it emotional weight
- "Proof of Antiquity for AI voices" is the tagline

## Links to Include

- GitHub: https://github.com/Scottcjn/vintage-voice
- RustChain (Proof of Antiquity hardware): https://rustchain.org
- BoTTube (demo videos): https://bottube.ai
- UNESCO Endangered Languages: https://www.unesco.org/en/intangible-cultural-heritage
- Acadian Expulsion: https://en.wikipedia.org/wiki/Expulsion_of_the_Acadians
