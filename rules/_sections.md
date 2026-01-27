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
- **description**: Core video creation components including voiceover, captions, and animations
- **priority**: 3
- **rules**:
  - voiceover.md
  - captions.md
  - animations.md
  - components.md

### assets
- **title**: Assets & Carousels
- **description**: Asset management and static content creation
- **priority**: 4
- **rules**:
  - local-assets.md
  - carousels.md

## Priority Levels

| Priority | When to Read |
|----------|--------------|
| 1 | Always read first - essential for any project |
| 2 | Read before creating any content |
| 3 | Read when creating video content |
| 4 | Read when working with specific asset types |

## Rule Loading Order

When the skill is invoked, rules should be loaded in this order:

1. **setup.md** - Understand project structure
2. **design-system-template.md** - Check brand configuration
3. **formats.md** - Understand dimension requirements
4. **voiceover.md** - If audio/voiceover is needed
5. **captions.md** - If animated captions are needed
6. **animations.md** - For animation implementation
7. **components.md** - For reusable components
8. **local-assets.md** - For asset management
9. **carousels.md** - For carousel creation

## Tags Index

Common tags used across rules:

| Tag | Description | Rules |
|-----|-------------|-------|
| `setup` | Project initialization | setup.md |
| `branding` | Brand colors, fonts, logos | design-system-template.md |
| `dimensions` | Canvas sizes, safe zones | formats.md |
| `audio` | Voiceover, sound | voiceover.md |
| `captions` | Subtitles, text timing | captions.md |
| `animation` | Motion, transitions | animations.md |
| `components` | Reusable React components | components.md |
| `assets` | Images, icons, backgrounds | local-assets.md |
| `carousel` | Static image slideshows | carousels.md |
