# Configuration Guide

Detailed configuration options for all skills.

## Environment Variables

All skills can be configured via environment variables in `.env`:

```bash
# WordPress (SEO WordPress Manager)
WP_GRAPHQL_URL=https://your-site.com/graphql
WP_USERNAME=your-username
WP_APP_PASSWORD=xxxx xxxx xxxx xxxx xxxx xxxx

# Site Configuration (Link Analyzer, CTA Injector)
SITE_DOMAIN=your-site.com
DIST_PATH=./dist

# HTTP Settings
HTTP_TIMEOUT=5
HTTP_MAX_WORKERS=20
RETRY_COUNT=1

# Safety
DRY_RUN=true
```

## Per-Skill Configuration

Each skill can also be configured via `config.json` in its directory.

### SEO WordPress Manager

`skills/seo-wordpress-manager/config.json`:

```json
{
  "wordpress": {
    "graphql_url": "https://your-site.com/graphql",
    "username": "your-username",
    "app_password": "xxxx xxxx xxxx xxxx xxxx xxxx"
  },
  "batch": {
    "size": 10,
    "delay_seconds": 1,
    "dry_run": true
  },
  "filters": {
    "post_types": ["post", "page"],
    "categories": [],
    "tags": [],
    "date_after": null,
    "date_before": null
  },
  "output": {
    "state_file": "./state/seo_update_progress.json",
    "backup_file": "./state/seo_backup.json",
    "report_file": "./reports/seo_update_report.md"
  },
  "validation": {
    "max_title_length": 60,
    "max_description_length": 160,
    "min_description_length": 120,
    "require_focus_keyphrase": false
  }
}
```

#### Options Explained

| Option | Default | Description |
|--------|---------|-------------|
| `batch.size` | 10 | Posts per batch |
| `batch.delay_seconds` | 1 | Delay between API calls |
| `batch.dry_run` | true | Preview without applying |
| `validation.max_title_length` | 60 | Warn if title exceeds |
| `validation.max_description_length` | 160 | Warn if description exceeds |

---

### Astro CTA Injector

`skills/astro-cta-injector/config.json`:

```json
{
  "content_path": "./src/content/blog",
  "file_patterns": ["**/*.astro", "**/*.md", "**/*.mdx"],
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
    },
    "product": {
      "template": "product.html",
      "default_placement": "end",
      "keywords": ["productivity", "task", "habit"],
      "min_score": 6.0,
      "data": {
        "title": "Try the App",
        "description": "Take productivity to the next level.",
        "button_text": "Get Started",
        "product_url": "/app"
      }
    }
  },
  "scoring": {
    "keyword_weight": 1.0,
    "length_weight": 1.0,
    "title_weight": 1.0,
    "min_word_count": 300
  },
  "output": {
    "state_file": "./state/cta_injection_progress.json",
    "backup_dir": "./backups",
    "report_file": "./reports/cta_injection_report.md"
  },
  "options": {
    "dry_run": true,
    "skip_existing": true,
    "create_backups": true
  }
}
```

#### CTA Type Configuration

Each CTA type has:

| Option | Description |
|--------|-------------|
| `template` | HTML template filename |
| `default_placement` | Where to inject by default |
| `keywords` | Words that indicate relevance |
| `min_score` | Minimum score to be eligible |
| `data` | Variables passed to template |

#### Placement Options

| Value | Description |
|-------|-------------|
| `end` | After all content |
| `after-paragraph-50%` | At 50% of paragraphs |
| `after-paragraph-60%` | At 60% of paragraphs |
| `after-heading` | After first H2 |
| `before-conclusion` | Before last paragraph |

---

### Link Analyzer

`skills/link-analyzer/config.json`:

```json
{
  "dist_path": "./dist",
  "site_domain": "example.com",
  "internal_domains": ["example.com", "www.example.com"],
  "excluded_paths": [
    "/tag/",
    "/category/",
    "/page/",
    "/author/",
    "/search/"
  ],
  "excluded_extensions": [
    ".css", ".js", ".png", ".jpg", ".jpeg", ".gif", ".svg",
    ".ico", ".pdf", ".xml", ".txt", ".woff", ".woff2"
  ],
  "http": {
    "timeout": 5,
    "max_workers": 20,
    "retry_count": 1,
    "user_agent": "Mozilla/5.0 (compatible; LinkChecker/1.0)"
  },
  "thresholds": {
    "underlinked_min_inbound": 3,
    "overlinked_max_outbound": 50,
    "link_sink_min_inbound": 5,
    "link_sink_max_outbound": 2
  },
  "false_positives": {
    "bot_blocker_domains": [
      "linkedin.com",
      "stackoverflow.com",
      "twitter.com",
      "x.com",
      "facebook.com"
    ],
    "image_sites": ["pixabay.com", "unsplash.com"],
    "ignore_status_codes": [403, 503, 520, 999]
  },
  "output": {
    "data_dir": "./data",
    "report_dir": "./reports"
  }
}
```

#### Threshold Configuration

| Option | Default | Description |
|--------|---------|-------------|
| `underlinked_min_inbound` | 3 | Pages with fewer are flagged |
| `overlinked_max_outbound` | 50 | Pages with more are flagged |
| `link_sink_min_inbound` | 5 | Minimum inbound to be a sink |
| `link_sink_max_outbound` | 2 | Maximum outbound to be a sink |

#### False Positive Configuration

Add domains that commonly block bots:

```json
{
  "false_positives": {
    "bot_blocker_domains": [
      "linkedin.com",
      "your-known-blocker.com"
    ]
  }
}
```

## Configuration Precedence

When both environment variables and `config.json` exist:

1. `config.json` values take precedence
2. Environment variables are used as fallback
3. Default values used if neither exists

## Sensitive Data

Never commit sensitive data to git:

```gitignore
# .gitignore
.env
**/config.json
**/state/
**/backups/
```

Use `.env.example` and `config.example.json` as templates.
