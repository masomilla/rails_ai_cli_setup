---
name: create-blog-post
description: Creates comprehensive blog post documenting implemented features with screenshots and Italian text. Uses Playwright for screenshots and ImageMagick for processing.
---

Create blog post for implemented feature with screenshots and Italian text. Use Playwright for capture, ImageMagick for processing.

## Usage

```bash
/create-blog-post [issue-id|description]
```

**Parameters:**
- `issue-id` (optional): GitHub issue number (e.g., `961`) - will fetch feature details using `gh issue view`
- `description` (optional): Free text description of the feature to document

**Examples:**
```bash
/create-blog-post 961                              # Document feature from GitHub issue #961
/create-blog-post                                   # Auto-detect from context (plan.md or git diff)
/create-blog-post Event reminder system            # Document feature from text description
```

**Information Source Priority:**
1. **GitHub Issue** - If issue ID provided: `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`
2. **Text Description** - If free text provided: Use as feature description
3. **Implementation Plan** - Check `.agent_session/plan.md` AND `.agent_session/context.md` for feature details and context
4. **Git Diff** - Fallback: `git diff main...HEAD` to understand changes

## Prerequisites

- Feature implemented and merged
- Server running: `bin/dev`
- Playwright MCP available
- User logged in at localhost:3000

## Process

### 1. Plan Content Structure

**Content sections** (Italian, professional but friendly):
- Intro (feature value)
- "I vantaggi di [feature]" (benefits as `<b>Title:</b> Description`)
- "Come funziona e dove trovarla" (step-by-step with screenshots)
- "Dettagli utili" (optional tech details)
- "Note importanti" (requirements/limits)
- Conclusion ("Inizia subito con [feature]")

**Writing style**:
- Benefits → how-to flow
- Concise paragraphs
- Bold important terms: `<b>Term</b>`
- Reference specific UI elements
- Real data in screenshots

### 2. Capture Screenshots

**Use Playwright**:
```javascript
mcp__playwright__browser_navigate("http://localhost:3000/[path]")
mcp__playwright__browser_resize(1280, 800)
mcp__playwright__browser_take_screenshot(".playwright-mcp/[nn]_desc.png")
```

**Guidelines**:
- 2-4 focused screenshots
- Sequential naming: `01_desc.png`, `02_desc.png`
- Focus on key UI elements
- Readable text at reduced size

### 3. Process Screenshots

**Resize**:
```bash
convert input.png -resize 800x -quality 85 output.png
```

**Add red boxes** (selective):
```bash
convert input.png -resize 800x -quality 85 \
  -stroke red -strokewidth 2 -fill none \
  -draw "rectangle X1,Y1,X2,Y2" output.png
```

**When to use boxes**:
- ✓ New buttons/controls
- ✓ Form sections to configure
- ✓ Menu items to click
- ✗ Overview screenshots
- ✗ Single-element obvious screenshots

**Move to assets**:
```bash
mkdir -p app/assets/images/blog/[slug]
mv .playwright-mcp/*.png app/assets/images/blog/[slug]/
```

### 4. Create ERB Directly

Write directly to `app/views/blog_posts/posts/_[slug].html.erb` following content plan:

**Format**:
```erb
<div>Intro...</div>

<h2>Section Title</h2>
<div><b>Benefit:</b> Description...</div>

<h2>Come funziona e dove trovarla</h2>
<div>Instructions...</div>
<div><%= image_tag 'blog/[slug]/01.png', alt: 'Desc', class: 'rounded-lg shadow-sm' %></div>

<h2>Dettagli utili</h2>
<ul data-editing-info='{"orderedStyleType":1,"unorderedStyleType":1}' style="margin-top: 0px; margin-bottom: 0px;">
<li style="list-style-type: disc;">Item</li>
</ul>

<h2>Inizia subito</h2>
<div>Conclusion...</div>
```

**Rules**:
- `<h2>` main sections, `<h3>` subsections
- `<div>` for paragraphs (not `<p>`)
- `<b>` for emphasis (not `<strong>`)
- `<code>` for technical terms
- Benefits: `<b>Title:</b> Description`
- Clean HTML, no extra `<br>`

### 5. Add Metadata

Add to TOP of `config/blog_posts.yml`:

```yaml
---
- slug: feature_slug_matching_filename
  title: 'Nuova funzionalità: Full Feature Name'
  created_at: 'YYYY-MM-DD'
  abstract: |-
    <div>Compelling 2-3 sentence summary highlighting main benefit and call to action.<br>
    </div>
```

**Guidelines**:
- Slug matches ERB filename (no `_` or `.html.erb`)
- Engaging title
- ISO date (YYYY-MM-DD)
- Abstract sells value (<250 chars)

### 6. Compile & Verify

```bash
yarn build && yarn build:css
# Visit: http://localhost:3000/blog/[slug]
```

**Check**:
- [ ] Images load
- [ ] Red boxes correct
- [ ] Text readable
- [ ] Clear hierarchy
- [ ] No broken layouts
- [ ] Appears in index

### 7. Cleanup & Commit

```bash
rm -rf .playwright-mcp/*.png

git add app/views/blog_posts/posts/_[slug].html.erb \
  app/assets/images/blog/[slug]/ config/blog_posts.yml

git commit -m "Add blog post: [feature]"
```

## Tips

- Plan screenshots beforehand (2-4 key moments)
- Test red box placement
- Keep concise (benefits + key steps)
- Use real data
- Proofread Italian text
- Mobile-friendly (800px width)

## Avoid

- ❌ Too many screenshots
- ❌ High-res images
- ❌ Red boxes everywhere
- ❌ Generic text
- ❌ Missing alt text
- ❌ Inconsistent formatting

## Workflow

1. `/create-plan` - Design feature
2. `/implement-plan` - Build
3. `/create-blog-post` - Document
4. `/pr` - Create PR with blog post

## Example Session Flow

```
User: /create-blog-post for the event reminder feature

Assistant: I will create a comprehensive blog post for the event reminder feature.

1. First, let me draft the content based on the implementation from issue #961
2. Capture screenshots using Playwright
3. Process images with ImageMagick
4. Create the ERB view
5. Update blog_posts.yml
6. Compile assets and verify
```
