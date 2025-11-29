# Why These Skills

The problems that led to building these tools.

## The Context

These skills emerged from a real project: migrating and optimizing a 1,300+ post WordPress blog to a static Astro site. What started as "just move the content" became a deep dive into content management at scale.

## Problem 1: SEO Metadata at Scale

**The situation:** 1,300 posts, many with missing or generic meta descriptions. Some had titles over 100 characters, some way less than that. Focus keyphrases were mostly absent.

**Manual approach:** Open each post in WordPress, edit Yoast fields, save. At 2 minutes per post, that's 43+ hours of excruciatingly boring work.

**The need:** Batch update Yoast fields with:
- Preview before applying (don't blindly change 1,300 posts)
- Progress tracking (don't lose your place)
- Resume capability (caan easily pick up where you left off)

**Result:** SEO WordPress Manager skill.

## Problem 2: CTA Injection

**The situation:** Static site generated, but no calls-to-action in the content. Newsletter signup, app promotion, related content - none of it existed consistently in the 1,300 posts.

**Manual approach:** Edit each markdown file, find the right spot, paste CTA HTML, try to be consistent. Multiply by 1,300.

**The need:**
- Automatically find relevant posts for each CTA type
- Place CTAs intelligently (not just at the beginning / end)
- Preview before touching files
- Ability to rollback mistakes (room for A/B testing)

**Result:** Astro CTA Injector skill.

## Problem 3: Link Structure Understanding

**The situation:** 15+ years of content means link rot. External sites die, internal links break during migration, and some posts become orphaned - linked from nowhere.

**Manual approach:** Check links one by one. Maybe use an online broken link checker. Have no idea about internal link structure.

**The need:**
- Find broken external links (with smart filtering for bot blockers)
- Find broken internal links
- Understand link graph structure:
  - Which pages are orphaned?
  - Which pages receive links but don't pass them?
  - Which pages are under-linked?

**Result:** Link Analyzer skill.

## Why Skills, Not Scripts

These started as Python scripts. But scripts require:
- Remembering commands and flags
- Context switching from conversation to terminal
- Manual orchestration of multi-step workflows

Skills integrate directly with Claude Code:
- Natural language invocation
- Claude handles orchestration
- Preview and confirmation built into conversation
- No context switching

## The ADD Framework Connection

These skills were built with Claude enhanced by the [Assess-Decide-Do Framework](https://github.com/dragosroua/claude-assess-decide-do-mega-prompt). This influenced the design:

**Assessment Phase:**
- Every skill has preview/dry-run mode
- You always see what will happen before it happens
- Gathering information precedes action

**Decision Phase:**
- Skills present clear choices (thresholds, strategies, batch sizes)
- User makes explicit decisions
- No hidden assumptions

**Execution Phase:**
- Progress tracking throughout
- Resume capability if interrupted
- Backups before changes
- Clear completion reporting

The result: tools that respect the user's cognitive flow rather than demanding immediate decisions or hiding complexity.

## What These Skills Aren't

- **Not AI-generated content:** These skills help manage content, not create it
- **Not magic:** They automate tedious work, not judgment calls, please keep using the space between your ears
- **Not one-size-fits-all:** Configuration exists because needs differ - your needs may be different than mine, so fill in the blanks

## The Underlying Philosophy

Content at scale is a solved problem for big companies with big engineering teams and deep pockets. For independent creators and small teams, it's a question of "do it manually or accept technical debt."

These skills aim to simplify content operations tooling - production-quality automation that individuals can actually use.
