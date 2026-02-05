# Rule Template

Use this template when creating new rules for the remotion-ads skill.

## File Structure

```markdown
---
title: Rule Title
description: Brief description of what this rule covers
section: section-name
priority: medium
tags: [tag1, tag2, tag3]
---

# Rule Title

Brief introduction explaining the purpose of this rule.

## When to Use

Describe when this rule should be applied.

## Quick Reference

| Property | Value |
|----------|-------|
| Key 1 | Value 1 |
| Key 2 | Value 2 |

## Implementation

### Basic Usage

\`\`\`tsx
// Code example
\`\`\`

### Advanced Usage

\`\`\`tsx
// More complex example
\`\`\`

## Common Patterns

### Pattern 1

Description and code example.

### Pattern 2

Description and code example.

## Best Practices

1. **Practice 1** - Explanation
2. **Practice 2** - Explanation
3. **Practice 3** - Explanation

## Troubleshooting

### Issue 1

**Problem**: Description of the issue.

**Solution**: How to fix it.

### Issue 2

**Problem**: Description of the issue.

**Solution**: How to fix it.

## Related Rules

- [related-rule.md](related-rule.md) - Brief description
```

## Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `title` | Yes | Human-readable rule title |
| `description` | Yes | One-line description for listings |
| `section` | Yes | Section ID from `_sections.md` |
| `priority` | No | `high`, `medium`, or `low` (default: `medium`) |
| `tags` | No | Array of relevant tags for search |

## Priority Guidelines

| Priority | Use When |
|----------|----------|
| `high` | Core functionality, always needed |
| `medium` | Common use cases, frequently needed |
| `low` | Edge cases, specialized features |

## Section IDs

Valid section IDs (from `_sections.md`):

- `setup` - Project initialization
- `formats` - Instagram dimensions and formats
- `video-creation` - Video production components
- `audio` - Voiceover, sound effects, and music generation
- `assets` - Asset management and carousels

## Writing Guidelines

1. **Start with "Why"** - Explain the purpose before the how
2. **Show, Don't Tell** - Include working code examples
3. **Be Specific** - Use exact values, not "approximately"
4. **Include Constants** - Provide copy-paste ready values
5. **Link Related Rules** - Help users discover related content
6. **Add Troubleshooting** - Address common issues
