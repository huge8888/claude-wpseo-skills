# Troubleshooting

Common issues and solutions.

## General Issues

### Skills Not Detected by Claude

**Symptoms:** Claude doesn't recognize or use the skills.

**Solutions:**
1. Verify skills are in the correct location:
   ```bash
   ls ~/.claude/skills/
   # Should show: seo-wordpress-manager, astro-cta-injector, link-analyzer
   ```

2. Check SKILL.md files exist:
   ```bash
   cat ~/.claude/skills/seo-wordpress-manager/SKILL.md
   ```

3. Restart Claude Code

4. Be explicit in your request:
   ```
   "Use the SEO WordPress Manager skill to update my meta descriptions"
   ```

### Python Import Errors

**Symptoms:** `ModuleNotFoundError` when running scripts.

**Solutions:**
1. Ensure shared module is copied:
   ```bash
   ls ~/.claude/skills/shared/
   # Should show: config_loader.py, utils.py
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Check Python version:
   ```bash
   python --version
   # Requires 3.10+
   ```

---

## SEO WordPress Manager

### "GraphQL request failed: 401"

**Cause:** Authentication failed.

**Solutions:**
1. Verify Application Password:
   - Go to WordPress → Users → Profile → Application Passwords
   - Create a new password if needed
   - Copy exactly (including spaces)

2. Check username matches WordPress user

3. Test connection:
   ```bash
   python scripts/wp_graphql_client.py --action test
   ```

### "updatePostSeo is not defined"

**Cause:** GraphQL mutation not registered.

**Solutions:**
1. Add mutation to `functions.php` (see Setup Guide)
2. Clear any WordPress caching
3. Verify WPGraphQL plugin is active

### "GraphQL URL not accessible"

**Cause:** GraphQL endpoint not reachable.

**Solutions:**
1. Test URL in browser: `https://your-site.com/graphql`
2. Check WPGraphQL plugin is active
3. Verify SSL certificate is valid
4. Check for firewall/CDN blocking

### Slow Performance

**Solutions:**
1. Increase `batch.delay_seconds` to reduce server load
2. Reduce `batch.size` for smaller batches
3. Check server resources

---

## Astro CTA Injector

### "No eligible posts found"

**Cause:** No posts meet the scoring criteria.

**Solutions:**
1. Lower `min_score` threshold:
   ```json
   { "min_score": 3.0 }
   ```

2. Add more keywords:
   ```json
   { "keywords": ["your", "relevant", "keywords"] }
   ```

3. Check content path is correct:
   ```bash
   ls ./src/content/blog/
   ```

4. Verify file patterns match:
   ```json
   { "file_patterns": ["**/*.md", "**/*.astro"] }
   ```

### "Template not found"

**Cause:** Template file missing or misnamed.

**Solutions:**
1. Check templates directory:
   ```bash
   ls skills/astro-cta-injector/templates/
   ```

2. Verify template name in config matches file

### Posts Already Have CTA

**Symptoms:** Posts being skipped but you want to re-inject.

**Solutions:**
1. Remove existing CTA markers from content
2. Use different CTA type
3. Modify duplicate detection patterns in script

### Malformed HTML After Injection

**Cause:** Injection point calculation incorrect.

**Solutions:**
1. Use `end` placement (safest)
2. Check content structure has proper HTML tags
3. Preview before applying to catch issues

---

## Link Analyzer

### "No pages found"

**Cause:** Dist path incorrect or no HTML files.

**Solutions:**
1. Verify dist path:
   ```bash
   ls ./dist/*.html
   ```

2. Build your site first:
   ```bash
   npm run build
   ```

3. Check config path:
   ```json
   { "dist_path": "./dist" }
   ```

### Too Many False Positives

**Cause:** Bot blockers returning errors.

**Solutions:**
1. Use `--filter` flag:
   ```bash
   python scripts/http_checker.py --input outbound.json --filter
   ```

2. Add known blockers to config:
   ```json
   {
     "false_positives": {
       "bot_blocker_domains": ["new-blocker.com"]
     }
   }
   ```

### Orphan Pages Incorrect

**Cause:** Excluded paths too aggressive.

**Solutions:**
1. Review excluded paths:
   ```json
   { "excluded_paths": ["/tag/", "/category/"] }
   ```

2. Some legitimate pages may be in excluded patterns

### HTTP Check Taking Too Long

**Cause:** Many URLs, rate limiting.

**Solutions:**
1. Run HTTP check separately from graph analysis
2. Reduce workers if server is slow:
   ```json
   { "http": { "max_workers": 10 } }
   ```

3. Reduce timeout:
   ```json
   { "http": { "timeout": 3 } }
   ```

### Memory Issues on Large Sites

**Cause:** Holding too much data in memory.

**Solutions:**
1. Process in chunks
2. Exclude unnecessary paths
3. Run analyses separately

---

## Getting Help

If you're still stuck:

1. Check the [GitHub Issues](https://github.com/dragosroua/claude-content-skills/issues)
2. Search closed issues for similar problems
3. Open a new issue with:
   - Error message (full traceback)
   - Config (sanitized - no passwords)
   - Steps to reproduce
   - Python version
   - OS
