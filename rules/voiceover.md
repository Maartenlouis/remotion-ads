---
title: Voiceover Integration
description: ElevenLabs integration, scene JSON format, and timing synchronization
section: video-creation
priority: medium
tags: [voiceover, audio, elevenlabs, timing, sync]
---

# Voiceover Integration for Instagram Ads

Complete guide for generating and integrating professional voiceovers into Remotion Instagram videos.

## Prerequisites

- ElevenLabs API key in `.env.local`

```bash
ELEVENLABS_API_KEY=your_api_key_here
```

---

## Pronunciation Dictionaries

Custom pronunciation dictionaries ensure brand names and technical terms are spoken correctly.

### Creating a Dictionary

1. Copy `dictionaries/template.pls` to `dictionaries/your-brand.pls`
2. Add lexeme entries for words needing custom pronunciation
3. Use `--dictionary your-brand` when generating

### Dictionary Format (PLS)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<lexicon version="1.0"
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      alphabet="ipa" xml:lang="de-DE">

  <!-- Brand name with phonetic pronunciation -->
  <lexeme>
    <grapheme>RF MedKonzept</grapheme>
    <alias>err eff Med Konzept</alias>
  </lexeme>

  <!-- Abbreviation spelled out -->
  <lexeme>
    <grapheme>bAV</grapheme>
    <alias>be ah fau</alias>
  </lexeme>

</lexicon>
```

### Using Dictionaries

```bash
# Use a specific dictionary
node tools/generate.js \
  --scenes scenes.json \
  --dictionary medkonzept \
  --output-dir public/audio/

# Disable dictionary (use default pronunciation)
node tools/generate.js \
  --scenes scenes.json \
  --no-dictionary \
  --output-dir public/audio/

# List available dictionaries
node tools/generate.js --list-dictionaries
```

### How It Works

1. **API Mode**: Dictionary is uploaded to ElevenLabs and applied during generation
2. **Fallback Mode**: If API permissions fail, text is preprocessed with replacements

The tool automatically caches dictionary IDs to avoid re-uploading

---

## Quick Start

```bash
# Generate voiceover from scenes JSON
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-example-scenes.json \
  --with-timestamps \
  --output-dir public/audio/instagram-ads/ad-example/

# With pronunciation dictionary
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-example-scenes.json \
  --dictionary medkonzept \
  --with-timestamps \
  --output-dir public/audio/instagram-ads/ad-example/
```

---

## Scene JSON Format

Create a scenes file for each ad at `remotion/instagram-ads/scenes/ad-{name}-scenes.json`:

```json
{
  "name": "ad-example",
  "voice": "YourVoiceName",
  "character": "narrator",
  "scenes": [
    {
      "id": "scene1",
      "text": "Hook text that grabs attention immediately.",
      "duration": 3.5,
      "character": "dramatic"
    },
    {
      "id": "scene2",
      "text": "Problem description. Multiple issues listed here.",
      "duration": 4.5,
      "character": "narrator"
    },
    {
      "id": "scene3",
      "text": "Solution presentation with key benefit.",
      "duration": 4.0,
      "character": "expert"
    },
    {
      "id": "scene4",
      "text": "Call to action. Your Brand Name.",
      "duration": 3.0,
      "character": "calm"
    }
  ]
}
```

### Character Presets

| Character | Description | Best For |
|-----------|-------------|----------|
| `literal` | Reads text exactly as written | Screen text, quotes |
| `narrator` | Professional storyteller, smooth | Explainers, general content |
| `salesperson` | Enthusiastic, persuasive | Marketing, ads |
| `expert` | Authoritative, confident | Professional content |
| `conversational` | Casual, friendly | Social media |
| `dramatic` | Intense, emotional | Hooks, problem statements |
| `calm` | Soothing, reassuring | Trust-building, CTAs |

---

## Output Files

After generation, the output directory contains:

```
public/audio/instagram-ads/ad-example/
├── ad-example-scene1.mp3      # Individual scene audio
├── ad-example-scene2.mp3
├── ad-example-scene3.mp3
├── ad-example-scene4.mp3
├── ad-example-combined.mp3    # All scenes stitched together
├── ad-example-info.json       # Metadata with actual durations
└── ad-example-captions.json   # Word-level timestamps (if --with-timestamps)
```

### info.json Structure

```json
{
  "name": "ad-example",
  "voice": "YourVoiceName",
  "totalDuration": 15.2,
  "scenes": [
    {
      "id": "scene1",
      "duration": 3.5,
      "actualDuration": 3.42,
      "text": "Hook text...",
      "file": "ad-example-scene1.mp3"
    }
  ]
}
```

**Important:** Use `actualDuration` from info.json in your Remotion composition for precise sync.

---

## Integration with Remotion

### Basic Audio Integration

```tsx
import { Audio, Series, staticFile, useVideoConfig } from "remotion";

// Import durations from generated info.json
// Or define manually based on info.json values
const SCENE_DURATIONS = {
  scene1: 3.42,  // actualDuration from info.json
  scene2: 4.35,
  scene3: 4.12,
  scene4: 3.31,
};

export const AdExample: React.FC = () => {
  const { fps } = useVideoConfig();

  // Calculate frames from durations
  const paddingFrames = 5; // Small buffer between scenes
  const scene1Frames = Math.round(SCENE_DURATIONS.scene1 * fps) + paddingFrames;
  const scene2Frames = Math.round(SCENE_DURATIONS.scene2 * fps) + paddingFrames;
  const scene3Frames = Math.round(SCENE_DURATIONS.scene3 * fps) + paddingFrames;

  // Calculate scene4 to fill remaining time (target: 15 seconds)
  const totalTargetFrames = Math.round(15 * fps);
  const scene4Frames = totalTargetFrames - scene1Frames - scene2Frames - scene3Frames;

  return (
    <AbsoluteFill>
      {/* Combined audio track */}
      <Audio src={staticFile("audio/instagram-ads/ad-example/ad-example-combined.mp3")} />

      {/* Scene sequence */}
      <Series>
        <Series.Sequence durationInFrames={scene1Frames}>
          <Scene1Hook />
        </Series.Sequence>
        <Series.Sequence durationInFrames={scene2Frames}>
          <Scene2Problem />
        </Series.Sequence>
        <Series.Sequence durationInFrames={scene3Frames}>
          <Scene3Solution />
        </Series.Sequence>
        <Series.Sequence durationInFrames={scene4Frames}>
          <Scene4CTA />
        </Series.Sequence>
      </Series>
    </AbsoluteFill>
  );
};
```

### Per-Scene Audio (Alternative)

For more precise control, use individual scene audio files:

```tsx
import { Audio, Sequence, staticFile, useVideoConfig } from "remotion";

export const AdWithPerSceneAudio: React.FC = () => {
  const { fps } = useVideoConfig();

  const scene1Frames = Math.round(3.42 * fps);
  const scene2Start = scene1Frames;
  const scene2Frames = Math.round(4.35 * fps);
  // ... etc

  return (
    <AbsoluteFill>
      <Sequence from={0} durationInFrames={scene1Frames}>
        <Audio src={staticFile("audio/instagram-ads/ad-example/ad-example-scene1.mp3")} />
        <Scene1Hook />
      </Sequence>

      <Sequence from={scene2Start} durationInFrames={scene2Frames}>
        <Audio src={staticFile("audio/instagram-ads/ad-example/ad-example-scene2.mp3")} />
        <Scene2Problem />
      </Sequence>

      {/* ... more sequences */}
    </AbsoluteFill>
  );
};
```

---

## Word-Level Timestamps (Captions)

Generate with `--with-timestamps` for animated captions:

```bash
node tools/generate.js \
  --scenes scenes.json \
  --with-timestamps \
  --output-dir public/audio/instagram-ads/ad-example/
```

### captions.json Structure

```json
{
  "remotion": {
    "captions": [
      {
        "text": "Hook ",
        "startMs": 0,
        "endMs": 280,
        "timestampMs": 0,
        "sceneId": "scene1"
      },
      {
        "text": "text ",
        "startMs": 280,
        "endMs": 520,
        "timestampMs": 280,
        "sceneId": "scene1"
      }
    ]
  }
}
```

See [captions.md](captions.md) for complete caption integration guide.

---

## Regenerating Individual Scenes

If a scene needs adjustments:

```bash
# Regenerate scene2 with new text
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-example-scenes.json \
  --scene scene2 \
  --new-text "Updated text for scene 2" \
  --output-dir public/audio/instagram-ads/ad-example/

# Regenerate with different character
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-example-scenes.json \
  --scene scene3 \
  --character dramatic \
  --output-dir public/audio/instagram-ads/ad-example/
```

The tool automatically:
- Uses request stitching from previous scenes for consistent prosody
- Updates info.json with new metadata
- Updates scenes.json if `--new-text` is provided

---

## Text Replacement (Phonetic vs Display)

For brand names or words that need different pronunciation vs display:

```tsx
// In your composition
const TEXT_REPLACEMENTS: Record<string, string> = {
  // Example: TTS says "Acmee" but display should show "ACME"
  // "Acmee": "ACME",
  // "Fonetic": "Phonetic",
  // Add your brand-specific replacements
};

const getDisplayText = (text: string): string => {
  const trimmed = text.trim();
  const replaced = TEXT_REPLACEMENTS[trimmed] || trimmed;
  return replaced.toUpperCase();  // Optional: ALL CAPS for captions
};
```

Use phonetic spelling in scenes.json for correct pronunciation, then display the correct spelling in captions using the replacement map.

---

## Timing Validation

Validate generated audio matches expected durations:

```bash
node tools/generate.js --validate public/audio/instagram-ads/ad-example/
```

Output:
```
🔍 Validating ad-example (4 scenes)

✅ scene1: 3.42s (expected: 3.5s)
   👍 8 words @ 2.3 words/sec
⚠️ scene2: 4.85s (expected: 4.5s)
   ⚠️ Audio 0.35s longer than expected
❌ scene3: 3.00s (expected: 4.0s)
   ❌ Audio 1.00s shorter than expected
✅ scene4: 3.31s (expected: 3.0s)

📊 Total duration: 14.58s (expected: 15.00s)
```

---

## Best Practices

### Script Writing

1. **Keep scenes short**: 3-5 seconds each for Instagram
2. **Use punctuation**: Periods = pauses, commas = brief breaks
3. **Write numbers out**: "fünfhundert" not "500"
4. **Spell out abbreviations**: "24 Stunden" not "24h"
5. **Front-load key messages**: Users may scroll away

### Voice Selection

| Content Type | Recommended Voice Style |
|--------------|------------------------|
| Legal/professional | Expert, calm |
| Marketing/sales | Salesperson, narrator |
| Educational | Narrator, expert |
| Social/casual | Conversational |
| Emotional hook | Dramatic |

### Timing Tips

1. Target 15 seconds total for Instagram Reels
2. Hook scene: 2-4 seconds (grab attention fast)
3. Problem scene: 3-5 seconds (establish pain point)
4. Solution scene: 3-5 seconds (present answer)
5. CTA scene: 2-4 seconds (clear next step)

---

## Workflow Summary

```bash
# 1. Create scenes JSON
vim remotion/instagram-ads/scenes/ad-new-scenes.json

# 2. Generate voiceover with timestamps (and optional dictionary)
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-new-scenes.json \
  --with-timestamps \
  --character narrator \
  --dictionary your-brand \
  --output-dir public/audio/instagram-ads/ad-new/

# 3. Check actual durations in info.json
cat public/audio/instagram-ads/ad-new/ad-new-info.json

# 4. Update composition with actual durations
# 5. Preview in Remotion Studio
npx remotion studio

# 6. If scene needs adjustment, regenerate just that scene
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-new-scenes.json \
  --scene scene2 \
  --new-text "Adjusted text" \
  --output-dir public/audio/instagram-ads/ad-new/

# 7. Render final video
npx remotion render AdNew out/ad-new.mp4 --codec=h264 --crf=18
```
