# ADD Framework Integration

How the Assess-Decide-Do framework shaped these skills.

## What is the ADD Framework?

The [Assess-Decide-Do (ADD) Framework](https://dragosroua.com/assess-decide-do-framework/) is a human cognitive framework created by Dragos Roua. When [integrated with Claude](https://github.com/dragosroua/claude-assess-decide-do-mega-prompt), it creates "cognitive alignment" - the AI recognizes whether you're exploring, deciding, or executing, and responds appropriately.

**Three Realms:**
- **Assess:** Gathering information, exploring possibilities, no commitment pressure (adding and editing, system "overload")
- **Decide:** Making choices, resources management, setting intentions (resource allocation, no editing)
- **Do:** Executing, completing, manifesting what was decided (execution, "unloading" the system to the next lieveline)

## How ADD Shaped These Skills

### 1. Assessment Before Action

Every skill has a **preview/dry-run mode** as the default:

```
# SEO Manager - dry-run by default
python scripts/yoast_batch_updater.py --input changes.json
# Shows what WOULD happen, doesn't change anything

# To actually apply:
python scripts/yoast_batch_updater.py --input changes.json --apply
```

**Why:** The Assess realm is about gathering information without commitment. Staying enough time in this space gives you breathing room and perspective.

### 2. Clear Decision Points

Skills present **explicit choices** rather than making assumptions:

```json
{
  "thresholds": {
    "underlinked_min_inbound": 3,
    "overlinked_max_outbound": 50
  }
}
```

**Why:** The Decide realm is about conscious choice-making. Burying decisions inside defaults removes agency. By surfacing choices, users engage their decision-making faculty, "felxing" this muscle too.

### 3. Confirmation Before Execution

Live changes require explicit confirmation:

```
WARNING: LIVE MODE - Changes will be applied to WordPress!
Type 'yes' to confirm:
```

**Why:** The transition from Decide to Do should be with awareness, not accidental. A confirmation prompt creates a clear boundary between intention and action.

### 4. Progress Tracking & Resume

All skills track progress and can resume interrupted operations:

```json
{
  "completed": ["post-123", "post-456"],
  "failed": [],
  "pending": ["post-789"],
  "current": null
}
```

**Why:** The framework prioritizes flow over task-churning. Checkpoints enable frictionless back-and-forth.

### 5. Backups & Rollback

Changes create backups automatically:

```
backups/
└── 2025-01-15_143022/
    ├── manifest.json
    └── original-files...
```

**Why:** Decisions can be revised. The ADD framework doesn't treat decisions as permanent - they're starting points for new cycles (called "livelines" as opposed to "deadline"). Rollback capability supports this.

## The Development Process

Building these skills with ADD-enhanced Claude was qualitatively different from typical development:

**Traditional approach:**
1. Start coding immediately
2. Discover edge cases while implementing
3. Refactor multiple times
4. Documentation as afterthought

**ADD-enhanced approach:**
1. **Assess:** What exactly needs to be built? What are the constraints? What exists already? Why are we doing this?
2. **Decide:** What's the architecture? What are the trade-offs? How big is the scope and what's in, what's out?
3. **Do:** Implementation with clear rails

The result was less thrashing, less token consumption, clearer architecture decisions made upfront, and naturally emerging documentation from the assessment phase.

## Cognitive Load Management

The ADD framework was originally designed for ADHD minds that struggle with cognitive overload. This influenced skill design:

**Separation of concerns:**
- Each skill does one thing well
- Scripts within skills are single-purpose
- Configuration is modular

**Reduced decision fatigue:**
- Sensible defaults but explicit choices when needed
- Preview modes reduce anxiety about changes

**Sequential focus:**
- Batch processing handles one item at a time
- Progress display shows where you are
- Clear completion signals

## Using ADD Principles in Your Own Tools

If you're building tools for content management (or any domain), consider:

1. **Default to preview:** Show what will happen before doing it
2. **Surface decisions:** Don't hide important choices in defaults
3. **Track progress:** Enable resumption of interrupted work
4. **Support rollback:** Decisions are flexible
5. **Separate concerns:** One tool, one job
6. **Completion matters:** Clear signals when done

## Further Reading

- [ADD Framework Mega-Prompt Repository](https://github.com/dragosroua/claude-assess-decide-do-mega-prompt)
- [Why This Matters](https://github.com/dragosroua/claude-assess-decide-do-mega-prompt/blob/main/docs/philosophy/WHY_THIS_MATTERS.md) - Philosophy behind cognitive alignment
- [The Asess Decide Do Framework](https://dragosroua.com/assess-decide-do-framework/) - entry point for for the Assess - Decide - Do framework hub pages
