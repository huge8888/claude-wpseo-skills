# Link Analyzer

Comprehensive link analysis for static sites.

## Overview

This skill provides deep analysis of your site's link structure:

| Analysis Type | What It Finds |
|---------------|---------------|
| **Outbound Links** | All external links, categorized by type |
| **Internal Links** | Broken internal links |
| **HTTP Validation** | External links that return errors |
| **Link Graph** | Orphans, under-linked, over-linked, link sinks |

## The Real Value: Link Graph Metrics

While broken link checking is common, the **link graph analysis** is where this skill shines:

### Orphan Pages
Pages with **zero inbound internal links**.

**Why it matters:** Search engines may not discover or index these pages. They have no internal authority signals.

**Common causes:**
- Removed from navigation but still published
- New pages not yet linked
- Content reorganization without updating links

### Under-linked Pages
Pages with fewer than N inbound links (configurable, default: 3).

**Why it matters:** These pages lack internal authority signals and may rank poorly despite good content.

### Over-linked Pages
Pages with more than N outbound links (configurable, default: 50).

**Why it matters:** Each link passes less equity (dilution). Pages with 100+ links may appear spammy.

### Link Sinks
Pages that receive many links but pass very few.

**Why it matters:** These pages accumulate authority but don't distribute it to other pages. Your site's link equity gets "stuck" here.

**Common examples:**
- About/Contact pages (often heavily linked, rarely link out)
- Landing pages
- Cornerstone content without related links

## Configuration

Create `skills/link-analyzer/config.json`:

```json
{
  "dist_path": "./dist",
  "site_domain": "example.com",
  "internal_domains": ["example.com", "www.example.com"],
  "excluded_paths": ["/tag/", "/category/", "/page/"],
  "thresholds": {
    "underlinked_min_inbound": 3,
    "overlinked_max_outbound": 50,
    "link_sink_min_inbound": 5,
    "link_sink_max_outbound": 2
  },
  "http": {
    "timeout": 5,
    "max_workers": 20
  }
}
```

## Usage

### Via Claude (Recommended)

```
"Analyze my site's internal linking structure"
"Find orphan pages that aren't linked from anywhere"
"Which pages are link sinks?"
"Check for broken external links"
"Show me under-linked content"
```

### Via Scripts (Direct)

```bash
# Full analysis (recommended)
python scripts/analyze.py --dist ./dist --output-dir ./results

# Individual analyses
python scripts/outbound_links.py --dist ./dist --output outbound.json
python scripts/internal_links.py --dist ./dist --output internal.json
python scripts/link_graph.py --dist ./dist --output graph.json --report report.md

# HTTP validation (slow, run separately)
python scripts/http_checker.py --input outbound.json --filter --output http_results.json
```

## Output: Link Graph Report

```markdown
# Link Graph Analysis Report

**Total Pages:** 1,500
**Total Internal Links:** 25,000

## Summary

| Issue | Count |
|-------|-------|
| Orphan Pages | 15 |
| Under-linked Pages | 47 |
| Over-linked Pages | 3 |
| Link Sinks | 23 |

## Orphan Pages (Critical)

These pages have **zero inbound internal links**.

- `/old-page-nobody-links-to/`
- `/draft-accidentally-published/`
- ...

## Under-linked Pages (<3 inbound)

| Page | Inbound Links |
|------|---------------|
| `/great-article/` | 1 |
| `/hidden-gem/` | 2 |

## Link Sinks

| Page | Inbound | Outbound |
|------|---------|----------|
| `/about/` | 15 | 0 |
| `/contact/` | 12 | 1 |
```

## HTTP Validation

### False Positive Filtering

Many "broken" links aren't actually broken - they're bot blockers:

| Site | Behavior |
|------|----------|
| LinkedIn | Returns 999 |
| Twitter/X | Returns 520 |
| StackOverflow | Returns 403 |
| Facebook | Returns 403 |

The `--filter` flag automatically excludes these known false positives.

### Real Broken Links

After filtering, you get actionable broken links:

```
404 - Page Not Found: 25 links
- https://old-blog.com/post (5 occurrences)

Dead Domains: 8 links
- https://defunct-site.com (3 occurrences)

Timeouts: 4 links
- https://slow-affiliate.com (2 occurrences)
```

## Interpreting Results

### Priority Order

1. **Orphan pages** (Critical) - Fix immediately
2. **Broken internal links** (High) - User experience issue
3. **Under-linked pages** (Medium) - SEO opportunity
4. **Link sinks** (Medium) - Authority distribution
5. **Broken external links** (Lower) - Fix when convenient

### Action Items

**For Orphan Pages:**
- Add to relevant hub/pillar pages
- Include in "Related Posts" sections
- Add to navigation if important
- Delete if truly obsolete

**For Under-linked Pages:**
- Find topically related content
- Add contextual links from high-authority pages
- Create hub pages that link to clusters

**For Link Sinks:**
- Add "Related Articles" section
- Link to deeper content
- Add internal links to body content

**For Broken Links:**
- Remove dead links
- Replace with alternatives
- Use Internet Archive for valuable references

## Performance Considerations

### HTTP Checking is Slow

Checking 500 external links with rate limiting takes time:
- ~2 minutes with 20 parallel workers
- Longer with more retries or lower worker count

Run HTTP checks separately from graph analysis:

```bash
# Fast: Graph analysis only
python scripts/link_graph.py --dist ./dist

# Slow: HTTP validation
python scripts/http_checker.py --input outbound.json
```

### Large Sites

For sites with 1000+ pages:
- Graph analysis: ~30 seconds
- Internal link check: ~1 minute
- HTTP validation: Several minutes

## Troubleshooting

### "No pages found"
- Check `dist_path` points to your build output
- Ensure files have `.html` extension

### Missing pages in graph
- Check `excluded_paths` isn't too aggressive
- Verify pages exist in dist folder

### False positives in HTTP check
- Add domains to `false_positives.bot_blocker_domains` in config
- Use `--filter` flag

## Related Files

- `skills/link-analyzer/SKILL.md` - Skill definition
- `skills/link-analyzer/reference.md` - Metrics definitions
- `skills/link-analyzer/scripts/link_graph.py` - Graph analysis code
