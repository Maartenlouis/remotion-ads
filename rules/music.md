---
title: Background Music Generation
description: Generate AI background music using Suno via browser-use for Instagram Reels and video content
section: audio
priority: medium
tags: [music, audio, suno, background, instrumental]
---

# Background Music Generation with Suno

Generate professional AI background music for Instagram Reels using Suno via browser-use API.

**Key Features:**
- Automatic artist/song reference conversion (removes trademarks, preserves style)
- Automatic fade out for smooth Remotion integration
- TypeScript-based CLI tool

---

## Setup Instructions

### Step 1: Get Your API Keys

You need two API keys:

1. **Browser-Use API Key**: Get from [browser-use.com](https://browser-use.com)
2. **Suno Authorization Token**: Get from browser DevTools (see below)

### Step 2: Get Your Suno Authorization Token

1. Go to [suno.com](https://suno.com) and log in
2. Open Developer Tools:
   - **Chrome/Edge**: Press `F12` or `Ctrl+Shift+I` (`Cmd+Option+I` on Mac)
   - **Firefox**: Press `F12` or `Ctrl+Shift+I` (`Cmd+Option+I` on Mac)
3. Go to the **Network** tab
4. Make any action on the Suno website
5. Find a request to `studio-api.suno.ai`
6. Copy the `authorization` header value (without "Bearer ")

### Step 3: Add Tokens to Environment

Create or edit `.env.local` in your project root:

```bash
# .env.local
BROWSER_USE_API_KEY=bu_your_key_here
SUNO_API_KEY=eyJhbGciOiJSUzI1NiIsIn...
```

### Step 4: Verify Setup

```bash
npx tsx tools/suno-direct.ts -p "Test ambient music" -i -o test.mp3
```

### Token Expiration

Suno tokens expire after approximately **24 hours**. If you get authorization errors, repeat Step 2 to get a fresh token.

---

## Quick Start

```bash
# Generate instrumental background music with fade out
npx tsx tools/suno-direct.ts \
  -p "Ambient, cinematic background for professional video" \
  -t "ambient, cinematic, professional" \
  -i \
  -o public/audio/background.mp3

# Generate with artist style reference (auto-converted!)
npx tsx tools/suno-direct.ts \
  -p "In the style of Tom Misch, groovy and jazzy" \
  -i \
  -o public/audio/groovy-background.mp3
# → Automatically converts to: "groovy electric guitar, jazzy neo-soul funk..."

# Custom fade duration (5 seconds)
npx tsx tools/suno-direct.ts \
  -p "Dramatic cinematic music" \
  -t "cinematic, dramatic, orchestral" \
  -i \
  --fade 5 \
  -o public/audio/dramatic.mp3

# No fade out
npx tsx tools/suno-direct.ts \
  -p "Upbeat intro music" \
  --fade 0 \
  -o public/audio/intro.mp3
```

---

## Artist/Song Style Conversion

The tool **automatically detects and converts artist or song references** to style descriptions. This removes trademarked names while preserving the musical style.

### How It Works

When you mention an artist like "Lana Del Rey" or a song like "Bohemian Rhapsody", the tool:

1. Detects the reference in your prompt or tags
2. Converts it to a style description (e.g., "dreamy cinematic pop, melancholic vocals, vintage Americana")
3. Adds relevant style tags automatically
4. Shows you the conversion before generating

### Examples

| Your Input | Converted To |
|------------|--------------|
| "Like Hans Zimmer" | "epic cinematic scores, powerful orchestration, electronic elements, dramatic builds" |
| "In the style of Billie Eilish" | "dark whisper-pop, minimalist beats, atmospheric production, ASMR-like vocals" |
| "Daft Punk vibes" | "French house, vocoder vocals, disco funk, robotic themes, groovy basslines" |
| "Similar to Bohemian Rhapsody" | "operatic rock, multi-section structure, dramatic dynamics, harmonized vocals" |

### List All Known Artists

```bash
python3 tools/suno.py --list-styles
```

Shows all 50+ artists and 20+ songs with their style descriptions, organized by genre:
- Pop, Rock, Hip-Hop, R&B/Soul
- Electronic, Alternative, Country
- Jazz, Classical/Cinematic, Latin, Metal

### Skip Conversion

If you want to use artist names directly (not recommended):
```bash
python3 tools/suno.py --prompt "..." --no-convert
```

---

## Options

| Option | Description | Default |
|--------|-------------|---------|
| `--prompt`, `-p` | Lyrics or description of the music | Required |
| `--tags`, `-t` | Style/genre tags (comma-separated) | `ambient, professional` |
| `--title` | Song title | Auto-generated |
| `--instrumental`, `-i` | Generate without vocals | false |
| `--output`, `-o` | Output file path | `output.mp3` |
| `--output-dir` | Output directory for batch | `public/audio/` |
| `--count`, `-n` | Number of variations | 1 |
| `--wait-timeout` | Max wait for generation (seconds) | 300 |
| `--help`, `-h` | Show help | - |

---

## Recommended Tags by Content Type

### For Video Backgrounds (Most Common)
```
ambient, cinematic, professional, minimal, corporate, soft, elegant, background
```

### For Instagram Reels
```
upbeat, modern, energetic, trendy, social media, short form, catchy
```

### For Dramatic/Legal Content
```
dramatic, tension, suspense, cinematic, orchestral, building, serious
```

### For Professional/Corporate
```
trustworthy, professional, corporate, clean, minimal, subtle, business
```

### For Emotional Content
```
emotional, hopeful, inspiring, heartfelt, reassuring, warm
```

---

## Prompting Best Practices

### Structure Your Prompts

Include these elements for best results:

1. **Mood/Emotion**: "professional", "dramatic", "hopeful"
2. **Genre/Style**: "ambient", "cinematic", "electronic"
3. **Duration**: "30 seconds", "1 minute"
4. **Structure**: "starts subtle, builds in middle, fades at end"
5. **Special instructions**: "no vocals", "suitable for voiceover"

### Example Prompts

#### For Instagram Reel Background (Recommended)
```
Professional, trustworthy ambient music for a legal services video.
Subtle piano with soft strings. Starts quietly, builds slightly in middle,
gentle fade at end. 30 seconds. Instrumental only, suitable for voiceover on top.
```

#### For Dramatic Hook Scene
```
Dramatic tension building intro, 5 seconds of suspense,
then transitions to hopeful/reassuring tone. Cinematic feel.
Instrumental only. Total 15 seconds.
```

#### For CTA/Outro
```
Uplifting, positive outro music. Clean and professional.
Builds to confident finish. 10 seconds. No vocals.
```

---

## Integration with Remotion

### Basic Background Music

```tsx
import { Audio, staticFile } from "remotion";

export const AdWithMusic: React.FC = () => {
  return (
    <AbsoluteFill>
      {/* Background music - low volume */}
      <Audio
        src={staticFile("audio/instagram-ads/ad-example/background.mp3")}
        volume={0.25}
      />

      {/* Voiceover on top - full volume */}
      <Audio
        src={staticFile("audio/instagram-ads/ad-example/ad-example-combined.mp3")}
        volume={1.0}
      />

      {/* Visual content */}
      <VideoContent />
    </AbsoluteFill>
  );
};
```

### Volume Ducking During Voiceover

Lower background music when voiceover is speaking:

```tsx
import { Audio, interpolate, useCurrentFrame, useVideoConfig } from "remotion";

export const AdWithDucking: React.FC<{
  voiceoverStart: number;  // Frame where voiceover begins
  voiceoverEnd: number;    // Frame where voiceover ends
}> = ({ voiceoverStart, voiceoverEnd }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Fade duration in frames
  const fadeFrames = Math.round(0.5 * fps);  // 0.5 second fade

  // Duck music volume during voiceover
  const musicVolume = interpolate(
    frame,
    [
      voiceoverStart - fadeFrames,  // Start fading down
      voiceoverStart,                // Fully ducked
      voiceoverEnd,                  // Still ducked
      voiceoverEnd + fadeFrames,     // Fade back up
    ],
    [0.35, 0.12, 0.12, 0.35],  // From 35% to 12% during voiceover
    { extrapolateLeft: "clamp", extrapolateRight: "clamp" }
  );

  return (
    <>
      <Audio
        src={staticFile("audio/instagram-ads/ad-example/background.mp3")}
        volume={musicVolume}
      />
      <Audio
        src={staticFile("audio/instagram-ads/ad-example/ad-example-combined.mp3")}
        volume={1.0}
      />
    </>
  );
};
```

### Scene-Based Volume Control

Different music volume for different scenes:

```tsx
import { Audio, Sequence, staticFile, useVideoConfig } from "remotion";

export const AdWithSceneMusic: React.FC = () => {
  const { fps } = useVideoConfig();

  // Scene frame calculations
  const scene1Frames = Math.round(3.5 * fps);
  const scene2Frames = Math.round(4.5 * fps);
  const scene3Frames = Math.round(4.0 * fps);
  const scene4Frames = Math.round(3.0 * fps);

  const scene2Start = scene1Frames;
  const scene3Start = scene2Start + scene2Frames;
  const scene4Start = scene3Start + scene3Frames;
  const totalFrames = scene4Start + scene4Frames;

  return (
    <AbsoluteFill>
      {/* Music louder during hook (no voiceover) */}
      <Sequence from={0} durationInFrames={scene1Frames}>
        <Audio
          src={staticFile("audio/instagram-ads/ad-example/background.mp3")}
          volume={0.4}
          startFrom={0}
        />
      </Sequence>

      {/* Music quieter during voiceover scenes */}
      <Sequence from={scene1Frames} durationInFrames={scene2Frames + scene3Frames}>
        <Audio
          src={staticFile("audio/instagram-ads/ad-example/background.mp3")}
          volume={0.15}
          startFrom={Math.round(scene1Frames / fps * 1000)}  // Continue from music position
        />
      </Sequence>

      {/* Music louder for CTA */}
      <Sequence from={scene4Start} durationInFrames={scene4Frames}>
        <Audio
          src={staticFile("audio/instagram-ads/ad-example/background.mp3")}
          volume={0.3}
          startFrom={Math.round(scene4Start / fps * 1000)}
        />
      </Sequence>

      {/* Voiceover */}
      <Audio src={staticFile("audio/instagram-ads/ad-example/ad-example-combined.mp3")} />
    </AbsoluteFill>
  );
};
```

---

## Workflow for Instagram Ads

### Complete Audio Workflow

```bash
# 1. Generate background music first
python3 tools/suno.py \
  --prompt "Professional, trustworthy ambient music. Starts subtle, builds slightly, fades at end. 20 seconds." \
  --tags "ambient, professional, corporate" \
  --instrumental \
  --output public/audio/instagram-ads/ad-new/background.mp3

# 2. Generate voiceover with timestamps
node tools/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-new-scenes.json \
  --with-timestamps \
  --output-dir public/audio/instagram-ads/ad-new/

# 3. Preview in Remotion Studio
npx remotion studio

# 4. Adjust music if needed (too long, wrong mood, etc.)
python3 tools/suno.py \
  --prompt "Shorter version, 15 seconds, same professional ambient style" \
  --tags "ambient, professional" \
  --instrumental \
  --output public/audio/instagram-ads/ad-new/background.mp3

# 5. Render final video
npx remotion render AdNew out/ad-new.mp4 --codec=h264 --crf=18
```

---

## Music Length Guidelines

| Video Duration | Music Duration | Notes |
|----------------|----------------|-------|
| 15 seconds | 18-20 seconds | Extra for fade out |
| 30 seconds | 33-35 seconds | Extra for transitions |
| 60 seconds | 65-70 seconds | Buffer for editing |

**Tip**: Always generate music slightly longer than your video. You can trim or fade out in Remotion.

---

## Volume Guidelines

| Scenario | Music Volume | Voiceover Volume |
|----------|--------------|------------------|
| No voiceover | 0.35-0.50 | N/A |
| With voiceover | 0.10-0.20 | 1.0 |
| Dramatic moment | 0.30-0.40 | 0.8-1.0 |
| CTA (quiet music) | 0.15-0.25 | 1.0 |

---

## Output Format

Generated files are saved as MP3. For optimal Remotion compatibility:

```bash
# Convert to WAV (uncompressed, better quality)
ffmpeg -i background.mp3 -acodec pcm_s16le background.wav

# Convert to AAC for Instagram (if needed)
ffmpeg -i background.mp3 -c:a aac -b:a 192k background.m4a
```

---

## Cost & Credits

Suno uses a credit-based system:

| Plan | Credits/Month | Approx Songs |
|------|---------------|--------------|
| Free | ~50/day | ~10 |
| Pro | 2,500 | ~500 |
| Premier | 10,000 | ~2,000 |

Each generation uses ~5 credits for a single song (2 variations are generated by default).

---

## Troubleshooting

### "Authorization failed"
Your token has expired. Get a new one from Suno DevTools (see Prerequisites).

### "Rate limited"
Wait a few minutes or check your remaining credits at suno.com.

### "Generation timed out"
Increase `--wait-timeout` or try a simpler prompt.

### Music doesn't match video length
1. Specify exact duration in prompt: "30 seconds exactly"
2. Generate slightly longer and trim
3. Use Remotion's `startFrom` and `endAt` props

### Music too loud/quiet
Adjust `volume` prop in Remotion. Start with 0.2 for background music with voiceover.

---

## Best Practices

1. **Always use `--instrumental`** for video backgrounds to avoid competing with voiceover
2. **Specify duration** in the prompt for predictable length
3. **Generate 2-3 variations** and pick the best (`--count 3`)
4. **Match mood to content**: dramatic for problems, hopeful for solutions
5. **Keep background music subtle**: 15-25% volume during voiceover
6. **Add fade out** at the end of your video for professional finish
7. **Test on mobile** with headphones to ensure music isn't overpowering
