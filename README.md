# remotion-ads

A Claude Code skill for creating professional video ads and explainer videos with [Remotion](https://www.remotion.dev/), ElevenLabs voiceover, and animated word-by-word captions.

## Supported Formats

| Format | Aspect | Resolution | Duration | Scenes |
|--------|--------|-----------|----------|--------|
| Instagram Reels | 9:16 | 1080x1920 | 15-60s | 4 |
| Instagram Reels (Ohneis) | 9:16 | 1080x1920 | 15-45s | Beat-driven |
| Website Explainers | 16:9 | 1920x1080 | 60-160s | 6 |
| Carousels | 4:5 | 1080x1350 | Static | 5-10 slides |

## Installation

### As a Claude Code skill

```bash
# Install in your project
claude skill install remotion-ads

# Or add the folder to your project's .claude/skills/ directory
```

### Prerequisites (in your Remotion project)

```bash
npm install remotion @remotion/cli @remotion/bundler @remotion/google-fonts
```

Optional:

```bash
# For caption support
npx remotion add @remotion/captions

# For background generation
npm install canvas
```

### Environment Variables

Create `.env.local` in your project root:

```bash
ELEVENLABS_API_KEY=your_key_here   # For voiceover generation
GEMINI_API_KEY=your_key_here       # For AI icon generation (optional)
```

## Quick Start

### 1. Configure your brand

Copy and fill in the brand config template:

```bash
cp references/brand-config-template.md references/brand-config.md
```

This covers colors, fonts, caption styling, voice presets, backgrounds, and compliance rules.

### 2. Create a pronunciation dictionary (optional)

Copy and customize the dictionary template for brand-specific pronunciations:

```bash
cp assets/dictionaries/template.pls assets/dictionaries/your-brand.pls
```

### 3. Write a scene JSON

```json
{
  "name": "ad-example",
  "voice": "VoiceName",
  "character": "narrator",
  "dictionary": "your-brand",
  "scenes": [
    { "id": "scene1", "text": "Hook text.", "duration": 3.5, "character": "dramatic" },
    { "id": "scene2", "text": "Problem.", "duration": 4.5 },
    { "id": "scene3", "text": "Solution.", "duration": 4.0, "character": "expert" },
    { "id": "scene4", "text": "CTA.", "duration": 3.0, "character": "calm" }
  ]
}
```

### 4. Generate voiceover

```bash
node scripts/generate.js \
  --scenes scenes.json \
  --with-timestamps \
  --dictionary your-brand \
  --output-dir public/audio/ad-example/
```

This produces per-scene MP3s, a combined audio file, word-level captions JSON, and an info file with actual durations.

### 5. Build Remotion composition and render

```bash
npx remotion render AdExample out/ad-example.mp4 --codec=h264 --crf=18
```

## URL-to-Video Workflow

Generate a scene JSON directly from any web page:

```bash
node scripts/url-to-scenes.js \
  --url https://example.com/page \
  --format reels \
  --output scenes.json
```

The script extracts headings, FAQ questions, CTA buttons, key terms, and internal links, then maps them into a scene structure with source annotations. Review and edit the output, then continue with voiceover generation.

Options:

| Flag | Description | Default |
|------|-------------|---------|
| `--url`, `-u` | URL to extract from (required) | - |
| `--format`, `-f` | `reels` (4 scenes) or `longform` (6 scenes) | `reels` |
| `--output`, `-o` | Output file path | stdout |
| `--voice`, `-v` | Voice name | `TODO` |
| `--dictionary`, `-d` | Dictionary name | `TODO` |
| `--language`, `-l` | Content language | auto-detect |

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/generate.js` | ElevenLabs voiceover with word timestamps, pronunciation dictionaries, and request stitching |
| `scripts/url-to-scenes.js` | Extract web page content into a scene JSON |
| `scripts/suno-direct.ts` | Background music generation via Suno |

### generate.js commands

```bash
# Generate all scenes with timestamps
node scripts/generate.js --scenes scenes.json --with-timestamps --output-dir public/audio/

# Regenerate a single scene
node scripts/generate.js --scenes scenes.json --scene scene2 --output-dir public/audio/

# Validate timing of generated audio
node scripts/generate.js --validate public/audio/ad-example/

# List available voices
node scripts/generate.js --list-voices

# Forced alignment on existing audio
node scripts/generate.js --align audio.mp3 --align-text "Your transcript"
```

## Voice Character Presets

| Character | Style | Stability | Similarity | Style |
|-----------|-------|-----------|------------|-------|
| `dramatic` | Intense, emotional | 0.3 | 0.8 | 0.7 |
| `narrator` | Professional, smooth | 0.5 | 0.75 | 0.4 |
| `expert` | Authoritative | 0.6 | 0.85 | 0.3 |
| `calm` | Soothing, reassuring | 0.7 | 0.8 | 0.2 |
| `conversational` | Casual, friendly | 0.45 | 0.7 | 0.5 |

## Project Structure

```
remotion-ads/
├── SKILL.md                          # Skill definition (loaded by Claude Code)
├── scripts/
│   ├── generate.js                   # ElevenLabs voiceover generator
│   ├── url-to-scenes.js              # URL → scene JSON extractor
│   └── suno-direct.ts                # Suno music generator
├── references/                       # Documentation (loaded on demand)
│   ├── ohneis-style.md               # Beat-driven cinematic reel template (@ohneis652 style)
│   ├── brand-config-template.md      # Brand config: colors, fonts, voice, captions
│   ├── setup.md                      # Project setup guide
│   ├── formats.md                    # Dimension specs and safe zones
│   ├── voiceover.md                  # ElevenLabs TTS, timing, dictionaries
│   ├── captions.md                   # Animated caption styles
│   ├── animations.md                 # Spring configs and transitions
│   ├── components.md                 # Reusable Remotion components
│   ├── website-videos.md             # 16:9 long-form format (6 scenes)
│   ├── url-to-video.md               # URL-to-video workflow
│   ├── sound-effects.md              # ElevenLabs SFX generation
│   ├── music.md                      # Background music
│   ├── local-assets.md               # Backgrounds, icons, illustrations
│   ├── carousels.md                  # 4:5 carousel design
│   ├── ad-copywriting.md             # Script frameworks and hooks
│   ├── copy-frameworks.md            # Headline templates
│   ├── natural-transitions.md        # Human-sounding transitions
│   ├── paid-ads.md                   # Meta campaign strategy
│   └── social-content.md             # Content calendar
└── assets/
    └── dictionaries/                 # Pronunciation dictionaries (.pls)
        ├── template.pls
        └── example.pls
```

## License

MIT
