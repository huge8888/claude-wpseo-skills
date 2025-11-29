# Astro CTA Injector

Inject Call-to-Action blocks into Astro site content with intelligent placement.

## Overview

This skill automates CTA injection into static site content:
- Score posts for relevance to specific CTA types
- Choose optimal placement based on content structure
- Preview changes before applying
- Maintain backups for easy rollback

## How It Works

### 1. Content Scoring

Posts are analyzed and scored (0-10) based on:

| Factor | Weight | Description |
|--------|--------|-------------|
| Keyword density | 5 points max | How often relevant keywords appear |
| Content length | 4 points max | Longer content = better candidate |
| Title match | 1 point max | Keywords in title |

Posts scoring above the threshold (default: 5.0) are eligible for CTA injection.

### 2. Placement Strategy

| Strategy | Position | Best For |
|----------|----------|----------|
| `end` | After all content | Non-intrusive CTAs |
| `after-paragraph-50%` | Midpoint | Newsletter signups |
| `after-paragraph-60%` | Later in content | Product recommendations |
| `after-heading` | After first H2 | Announcements |
| `before-conclusion` | Before last paragraph | Strong finish |

### 3. Template System

CTAs are generated from HTML templates with variable substitution:

```html
<!-- templates/newsletter.html -->
<aside class="cta cta-newsletter" data-cta-type="newsletter">
  <h3>{{title}}</h3>
  <p>{{description}}</p>
  <form action="{{form_url}}" method="post">
    <input type="email" placeholder="Your email" required />
    <button type="submit">{{button_text}}</button>
  </form>
</aside>
```

## Configuration

Create `skills/astro-cta-injector/config.json`:

```json
{
  "content_path": "./src/content/blog",
  "file_patterns": ["**/*.astro", "**/*.md"],
  "cta_types": {
    "newsletter": {
      "template": "newsletter.html",
      "default_placement": "after-paragraph-50%",
      "keywords": ["tip", "guide", "learn", "strategy"],
      "min_score": 5.0,
      "data": {
        "title": "Get More Insights",
        "description": "Subscribe for weekly tips.",
        "button_text": "Subscribe",
        "form_url": "/api/newsletter"
      }
    }
  }
}
```

## Usage

### Via Claude (Recommended)

```
"Add newsletter CTAs to my productivity articles"
"Inject product promotion into posts about habits"
"Preview where CTAs would be placed"
"Score my posts for newsletter relevance"
```

### Via Scripts (Direct)

```bash
# Score posts for a CTA type
python scripts/score_posts.py \
  --content-path ./src/content/blog \
  --cta-type newsletter \
  --output scored_posts.json

# Preview what will be injected
python scripts/preview_injection.py \
  --input scored_posts.json \
  --placement after-paragraph-50%

# Inject CTAs (dry-run by default)
python scripts/inject_ctas.py \
  --input scored_posts.json \
  --cta-type newsletter

# Actually apply changes
python scripts/inject_ctas.py \
  --input scored_posts.json \
  --cta-type newsletter \
  --apply

# Rollback from backup
python scripts/inject_ctas.py \
  --rollback 2025-01-15_143022
```

## Scoring Output

The scoring script produces:

```json
{
  "cta_type": "newsletter",
  "keywords": ["tip", "guide", "learn"],
  "summary": {
    "total_posts": 150,
    "eligible": 45,
    "already_have_cta": 12
  },
  "eligible_posts": [
    {
      "file_path": "./src/content/blog/productivity-tips.astro",
      "title": "10 Productivity Tips",
      "word_count": 1250,
      "total_score": 8.5,
      "matched_keywords": ["tip", "strategy", "improve"],
      "eligible": true
    }
  ]
}
```

## Duplicate Detection

The injector automatically skips posts that already have a CTA:

```python
# Checks for these patterns in content:
- data-cta-type="newsletter"
- class="cta-newsletter"
- <!-- CTA:newsletter -->
```

## Backup & Rollback

Every injection creates a timestamped backup:

```
backups/
└── 2025-01-15_143022/
    ├── manifest.json
    └── src/content/blog/
        ├── post-1.astro
        └── post-2.astro
```

To rollback:
```bash
python scripts/inject_ctas.py --rollback 2025-01-15_143022
```

## Custom Templates

Create new templates in `skills/astro-cta-injector/templates/`:

```html
<!-- templates/ebook.html -->
<aside class="cta cta-ebook" data-cta-type="ebook">
  <img src="{{image_url}}" alt="{{title}}" />
  <h3>{{title}}</h3>
  <p>{{description}}</p>
  <a href="{{download_url}}" class="cta-button">
    Download Free
  </a>
</aside>
```

Then add to config:

```json
{
  "cta_types": {
    "ebook": {
      "template": "ebook.html",
      "default_placement": "end",
      "keywords": ["guide", "framework", "comprehensive"],
      "data": {
        "title": "Free Ebook",
        "description": "Download our comprehensive guide",
        "image_url": "/images/ebook-cover.png",
        "download_url": "/ebook"
      }
    }
  }
}
```

## CSS Recommendations

```css
.cta {
  margin: 2rem 0;
  padding: 1.5rem;
  border-radius: 8px;
  background: var(--cta-bg, #f5f5f5);
}

.cta-newsletter {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.cta-product {
  border: 2px solid var(--accent-color);
  background: white;
}

.cta-title {
  margin: 0 0 0.5rem;
  font-size: 1.25rem;
}
```

## Troubleshooting

### "No eligible posts found"
- Check `min_score` threshold (try lowering it)
- Verify keywords match your content
- Check file patterns match your files

### "Template not found"
- Ensure template file exists in `templates/` directory
- Check filename matches config

### Posts already have CTA but still being targeted
- The duplicate detection pattern may not match your CTA HTML
- Add your pattern to the detection logic

## Related Files

- `skills/astro-cta-injector/SKILL.md` - Skill definition
- `skills/astro-cta-injector/reference.md` - Detailed reference
- `skills/astro-cta-injector/templates/` - CTA templates
