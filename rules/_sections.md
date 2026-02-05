# Rule Sections

This file defines the categories and organization of rules in the remotion-ads skill.

## Section Definitions

### setup
- **title**: Project Setup
- **description**: Initial project configuration, dependencies, and folder structure
- **priority**: 1
- **rules**:
  - setup.md
  - design-system-template.md

### formats
- **title**: Instagram Formats
- **description**: Display formats, dimensions, and safe zones for Instagram
- **priority**: 2
- **rules**:
  - formats.md

### video-creation
- **title**: Video Creation
- **description**: Core video creation components including captions, animations, and reusable components
- **priority**: 3
- **rules**:
  - captions.md
  - animations.md
  - components.md

### audio
- **title**: Audio Production
- **description**: Voiceover, sound effects, and background music generation using ElevenLabs and Suno
- **priority**: 4
- **rules**:
  - voiceover.md
  - sound-effects.md
  - music.md

### assets
- **title**: Assets & Carousels
- **description**: Asset management and static content creation
- **priority**: 5
- **rules**:
  - local-assets.md
  - carousels.md

### copywriting
- **title**: Ad Copywriting
- **description**: Script writing, headline formulas, CTA frameworks, and copy best practices for video and carousel ads
- **priority**: 6
- **rules**:
  - copywriting/ad-copywriting.md
  - copywriting/references/copy-frameworks.md
  - copywriting/references/natural-transitions.md

### distribution
- **title**: Distribution & Promotion
- **description**: Paid ad campaign management, social content strategy, and content repurposing
- **priority**: 7
- **rules**:
  - paid-ads.md
  - social-content.md

## Priority Levels

| Priority | When to Read |
|----------|--------------|
| 1 | Always read first - essential for any project |
| 2 | Read before creating any content |
| 3 | Read when creating video content |
| 4 | Read when working with audio (voiceover, SFX, music) |
| 5 | Read when working with specific asset types |
| 6 | Read when writing ad scripts or copy |
| 7 | Read when planning distribution or campaigns |

## Rule Loading Order

When the skill is invoked, rules should be loaded in this order:

1. **setup.md** - Understand project structure
2. **design-system-template.md** - Check brand configuration
3. **formats.md** - Understand dimension requirements
4. **copywriting/ad-copywriting.md** - If writing ad scripts or copy
5. **captions.md** - If animated captions are needed
6. **animations.md** - For animation implementation
7. **components.md** - For reusable components
8. **voiceover.md** - If audio/voiceover is needed
9. **sound-effects.md** - If transition/ambient SFX are needed
10. **music.md** - If background music is needed
11. **local-assets.md** - For asset management
12. **carousels.md** - For carousel creation
13. **paid-ads.md** - If running paid campaigns
14. **social-content.md** - If planning social distribution

## Tags Index

Common tags used across rules:

| Tag | Description | Rules |
|-----|-------------|-------|
| `setup` | Project initialization | setup.md |
| `branding` | Brand colors, fonts, logos | design-system-template.md |
| `dimensions` | Canvas sizes, safe zones | formats.md |
| `audio` | Voiceover, sound effects, music | voiceover.md, sound-effects.md, music.md |
| `sfx` | Sound effects, transitions | sound-effects.md |
| `music` | Background music | music.md |
| `captions` | Subtitles, text timing | captions.md |
| `animation` | Motion, transitions | animations.md |
| `components` | Reusable React components | components.md |
| `assets` | Images, icons, backgrounds | local-assets.md |
| `carousel` | Static image slideshows | carousels.md |
| `copywriting` | Ad scripts, headlines, CTAs | copywriting/ad-copywriting.md |
| `hooks` | Attention-grabbing openers | copywriting/ad-copywriting.md, copywriting/references/copy-frameworks.md |
| `paid-ads` | Campaign management, targeting | paid-ads.md |
| `social` | Content strategy, scheduling | social-content.md |
| `distribution` | Promotion, repurposing | paid-ads.md, social-content.md |
