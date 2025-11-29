# Documentation

Welcome to the Claude Content Skills documentation.

## Quick Navigation

### Getting Started
- [Installation Guide](integration/SETUP.md) - Step-by-step installation
- [Configuration](integration/CONFIGURATION.md) - Setting up credentials and options
- [Troubleshooting](integration/TROUBLESHOOTING.md) - Common issues and solutions

### Skills Documentation
- [SEO WordPress Manager](skills/SEO_WORDPRESS_MANAGER.md) - Batch Yoast SEO updates
- [Astro CTA Injector](skills/ASTRO_CTA_INJECTOR.md) - Intelligent CTA placement
- [Link Analyzer](skills/LINK_ANALYZER.md) - Comprehensive link analysis

### Philosophy & Design
- [Why These Skills](philosophy/WHY_THESE_SKILLS.md) - The problems we're solving
- [ADD Framework Integration](philosophy/ADD_FRAMEWORK.md) - How cognitive alignment shaped development

---

## About Claude Code Skills

Claude Code skills are a mechanism for extending Claude's capabilities in specific domains. Unlike slash commands (which require explicit user invocation), skills are **model-invoked** - Claude automatically detects when to use them based on conversation context.

### Skill Structure

Each skill follows this structure:

```
skill-name/
├── SKILL.md           # Required: Skill definition with YAML frontmatter
├── reference.md       # Optional: Detailed reference documentation
├── config.example.json # Optional: Configuration template
└── scripts/           # Optional: Supporting scripts
```

The `SKILL.md` file is the entry point:

```yaml
---
name: skill-name
description: Description that helps Claude know when to use this skill.
  Include trigger phrases and use cases.
---

# Skill Documentation

Detailed instructions for Claude when this skill is active...
```

### How Claude Uses Skills

1. User makes a request
2. Claude scans available skills' descriptions
3. If a skill matches the request context, Claude activates it
4. Claude follows the skill's documentation to respond appropriately
5. Claude uses the skill's scripts and tools as needed

---

## Development Philosophy

These skills were developed with the [Assess-Decide-Do (ADD) Framework](https://github.com/dragosroua/claude-assess-decide-do-mega-prompt), which influenced several design decisions:

### Assessment-First Design
Each skill includes preview/dry-run modes because the ADD framework emphasizes thorough assessment before action. You always see what will happen before it happens.

### Clear Decision Points
Skills present explicit choices (batch size, thresholds, placement strategies) rather than making assumptions. This respects the user's decision-making authority.

### Execution Safety
Once in "Do" mode, skills include progress tracking, backups, and resume capability. Completing tasks matters, and interruptions shouldn't mean starting over.

---

## Contributing to Documentation

Documentation improvements are always welcome. When contributing:

1. Match the existing tone (direct, practical, no fluff)
2. Include real examples where possible
3. Document edge cases you discover
4. Keep the ADD framework principles in mind

---

<p align="center">
  <sub>Part of <a href="https://github.com/dragosroua/claude-content-skills">Claude Content Skills</a></sub>
</p>
