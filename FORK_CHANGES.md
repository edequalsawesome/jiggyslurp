# JiggySlurp - Custom Frontmatter Fork

**Fork of [Slurp](https://github.com/inhumantsar/slurp) by inhumantsar**

## What's New

This fork adds two features for custom frontmatter:

### 1. Static Custom Frontmatter (Settings)
Add custom YAML fields to **every** clipped note via plugin settings.

**Use case:** Default metadata like `type: article`, `status: unread`, `project: research`

**How to use:**
1. Open Slurp settings in Obsidian
2. Find "Custom frontmatter" text area under "General"
3. Add YAML fields (one per line):
   ```yaml
   type: article
   status: unread
   project: research
   ```
4. Every clipped note will include these fields

### 2. Dynamic Custom Frontmatter (URI Parameters)
Pass custom frontmatter via the `obsidian://slurp` URI.

**Use case:** AI-generated summaries, custom tags, dynamic metadata per clip

**Syntax:**
```
obsidian://slurp?url=<encoded-url>&tldr=<summary>&type=article&mood=inspired
```

**Any parameter** except `url` becomes a frontmatter field:
- `&tldr=AI generated summary` → `tldr: AI generated summary`
- `&type=tutorial` → `type: tutorial`
- `&rating=5` → `rating: 5`

### iOS Shortcut Example with AI Summary

```
1. Get URL from Share Sheet
2. URL Encode the URL
3. Get summary from Apple Intelligence (Summarize action)
4. URL Encode the summary
5. Build URI: obsidian://slurp?url=<encoded-url>&tldr=<encoded-summary>
6. Open URL
```

## Building & Installing

```bash
# Install dependencies
npm install

# Build the plugin
npm run build

# Copy to Obsidian plugins folder
cp -r /path/to/slurp /path/to/vault/.obsidian/plugins/slurp

# Or create symlink for development
ln -s /path/to/slurp /path/to/vault/.obsidian/plugins/slurp
```

Then enable "Slurp" in Obsidian's Community Plugins settings.

## Example Output

**URI:**
```
obsidian://slurp?url=https://example.com&tldr=Great+article+about+things&rating=5
```

**Settings Custom Frontmatter:**
```yaml
type: article
status: unread
```

**Result:**
```yaml
---
link: https://example.com
byline: John Doe
site: Example Site
date: 2026-02-22T10:30
slurped: 2026-02-22T10:35
title: Example Article
type: article
status: unread
tldr: Great article about things
rating: 5
---

[article content here]
```

## Code Changes

- `src/types.ts` - Added `customFrontmatter?: string` to `ISettings`
- `src/const.ts` - Added default value for `customFrontmatter`
- `src/settings.ts` - Added text area for custom frontmatter
- `main.ts` - Updated URI handler to parse extra parameters
- `main.ts` - Updated `slurp()` and `slurpNewNoteCallback()` to merge custom frontmatter
