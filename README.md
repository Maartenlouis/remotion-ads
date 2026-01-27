# Remotion Ads Skill

A Claude Code skill for creating professional Instagram Reels and Carousel ads with [Remotion](https://remotion.dev).

## Features

- **Instagram Reels** (9:16) - Vertical video ads with safe zones
- **Instagram Carousels** (4:5) - Static image slideshows
- **Voiceover Integration** - ElevenLabs TTS with scene-based generation
- **Animated Captions** - TikTok-style word highlighting
- **Reusable Components** - Template scenes, animations, layouts
- **Asset Management** - Backgrounds, icons, illustrations

## Installation

### Via Claude Code Plugin System

```bash
/plugin marketplace add Maartenlouis/remotion-ads
```

Then use `/plugin` to browse and install, or:
```bash
/plugin install remotion-ads@Maartenlouis-remotion-ads
```

### Manual Installation

Clone to your project's `.claude/skills/` directory:

```bash
git clone https://github.com/Maartenlouis/remotion-ads.git .claude/skills/remotion-ads
```

## Quick Start

1. **Configure your brand** - Copy and edit the design system template:
   ```bash
   cp .claude/skills/remotion-ads/rules/design-system-template.md \
      .claude/skills/remotion-ads/rules/design-system.md
   ```

2. **Edit `design-system.md`** with your:
   - Brand colors
   - Fonts (heading + body)
   - Logo path
   - Icon paths

3. **Generate backgrounds**:
   ```bash
   node scripts/generate-backgrounds.js
   ```

4. **Create your first ad** - Ask Claude:
   > "Create an Instagram Reel ad about [your topic]"

## Documentation

| File | Description |
|------|-------------|
| [SKILL.md](SKILL.md) | Main skill reference |
| [rules/setup.md](rules/setup.md) | Project setup guide |
| [rules/design-system-template.md](rules/design-system-template.md) | Brand configuration |
| [rules/voiceover.md](rules/voiceover.md) | ElevenLabs integration |
| [rules/captions.md](rules/captions.md) | Animated captions |
| [rules/animations.md](rules/animations.md) | Spring configs & effects |
| [rules/components.md](rules/components.md) | Template components |
| [rules/formats.md](rules/formats.md) | Instagram display formats |
| [rules/carousels.md](rules/carousels.md) | Carousel specifications |
| [rules/local-assets.md](rules/local-assets.md) | Asset management |

## Instagram Safe Zones

```
┌─────────────────────────────────────┐ 0px
│          TOP DANGER ZONE            │
│         (250px buffer)              │
├─────────────────────────────────────┤ ~285px
│                                     │
│      OPTIMAL CONTENT ZONE           │
│      (880×1350px centered)          │
│                                     │
├─────────────────────────────────────┤ ~1520px
│        BOTTOM DANGER ZONE           │
│         (400px buffer)              │
└─────────────────────────────────────┘ 1920px
```

## Ad Structure (4 Scenes)

| Scene | Purpose | Duration |
|-------|---------|----------|
| **Hook** | Grab attention | 2-4s |
| **Problem** | Establish pain point | 3-5s |
| **Solution** | Present answer | 3-5s |
| **CTA** | Call to action | 2-4s |

## Voiceover Integration

Requires [ElevenLabs skill](https://github.com/Maartenlouis/elevenlabs-remotion-skill) for voice generation:

```bash
# Generate voiceover with timestamps
node .claude/skills/elevenlabs/generate.js \
  --scenes remotion/instagram-ads/scenes/ad-example-scenes.json \
  --with-timestamps \
  --output-dir public/audio/instagram-ads/ad-example/
```

## Export

```bash
# Render Reel
npx remotion render AdExample out/reel.mp4 --codec=h264 --crf=18

# Render Carousel slides
for i in 1 2 3 4 5; do
  npx remotion still remotion/index.ts "Carousel-Slide$i" "out/slide$i.png" --overwrite
done
```

## Requirements

- Node.js 18+
- Remotion installed in your project
- ElevenLabs API key (for voiceover)

## License

MIT License - see [LICENSE](LICENSE) for details.

## Related Skills

- [elevenlabs-remotion-skill](https://github.com/Maartenlouis/elevenlabs-remotion-skill) - AI voiceover generation
