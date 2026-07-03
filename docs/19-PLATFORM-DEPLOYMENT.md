# Platform Deployment & Knowledge Base Setup

**Version:** 0.4.0  
**Last Updated:** July 2026  
**Audience:** Documentation Administrators, DevOps, Product Teams

---

## Table of Contents

1. [Overview](#overview)
2. [Zendesk Guide Setup](#zendesk-guide-setup)
3. [GitBook Setup](#gitbook-setup)
4. [GitHub Pages Setup](#github-pages-setup)
5. [Multi-Platform Deployment](#multi-platform-deployment)
6. [Content Optimization](#content-optimization)
7. [SEO & Discoverability](#seo--discoverability)
8. [Analytics & Monitoring](#analytics--monitoring)
9. [Maintenance & Updates](#maintenance--updates)

---

## Overview

### What is Included

```
Markdown Documentation Suite (15 files):
✓ Complete feature reference
✓ User guides and tutorials
✓ Developer documentation
✓ API reference
✓ Troubleshooting guides
✓ Configuration reference
✓ Physics documentation
✓ Export format reference
✓ Keyboard shortcuts
✓ Deployment procedures
✓ Testing documentation
✓ Performance optimization
✓ Architecture reference
✓ Contributing guidelines

Total Content:
- 40,000+ lines
- 200+ code examples
- 30+ diagrams/tables
- 15+ FAQs per major topic
- Keyboard-navigable
- Accessible (WCAG 2.1 AA)
- Mobile-responsive
- SEO-optimized
```

### Deployment Considerations

```
Platform Choice:
├─ Zendesk Guide
│  ├─ Best for: Support/help center
│  ├─ Features: Ticketing, search, analytics
│  ├─ Cost: Starts $29/month
│  ├─ Audience: Support focus
│  └─ Use: Customer-facing help
│
├─ GitBook
│  ├─ Best for: Product docs, developer docs
│  ├─ Features: Versioning, integration, docs-as-code
│  ├─ Cost: Free tier available, Pro $10+/user
│  ├─ Audience: Technical focus
│  └─ Use: Developer/power user docs
│
└─ GitHub Pages
   ├─ Best for: Open source, internal docs
   ├─ Features: Free, version control native
   ├─ Cost: Free
   ├─ Audience: Developer/technical
   └─ Use: Internal/community docs
```

---

## Zendesk Guide Setup

### Prerequisites

```
✓ Zendesk account (Guide plan or higher)
✓ Admin or agent with permission to manage Guide
✓ Custom domain configured (optional but recommended)
✓ SSL certificate (for custom domain)
```

### Step-by-Step Setup

#### **Step 1: Create Guide Workspace**

```
1. Log in to Zendesk admin
2. Navigate: Zendesk Guide → Manage → Guide Admin
3. Create New Guide:
   - Name: "FabSim Knowledge Base"
   - URL: help.fabsim.example.com (or zendesk subdomain)
   - Visibility: Public or Private
   - Help Center Theme: Choose or customize

4. Settings:
   - Enable SSL: Yes (for custom domain)
   - Analytics: Enable
   - Search: Enable
```

#### **Step 2: Create Categories**

```
Category Structure (match doc organization):

1. Getting Started
   - Quick start guide
   - Installation
   - First project

2. User Guide
   - Weave design
   - Simulation
   - Export

3. Features Reference
   - All features detailed
   - With examples

4. Developer Guide
   - Architecture
   - API reference
   - Contributing

5. Troubleshooting
   - Common issues
   - FAQs
   - Error solutions

6. Advanced Topics
   - Physics models
   - Performance tuning
   - Custom development

7. About
   - Release notes
   - Support
   - Feedback
```

#### **Step 3: Import Documentation**

**Option A: Manual Import (Simple)**
```
1. Copy each .md file content
2. In Zendesk: + Article → Create Article
3. Paste content into editor
4. Format using Zendesk editor:
   - Headers → Heading 1/2/3
   - Bold/Italic → formatting
   - Code → Code block
   - Links → Update internal links
5. Assign to category
6. Set visibility and permissions
7. Publish

Time: ~2-3 minutes per article (20 articles × 2.5 min = 50 min)
```

**Option B: Bulk Import (Recommended)**
```
1. Install Zendesk CLI: npm install -g @zendesk/zcli
2. Create import manifest (YAML):
   articles:
   - title: Getting Started
     body: 01-USER-GUIDE.md
     category: Getting Started
   - title: Developer Guide
     body: 03-DEVELOPER-GUIDE.md
     category: Developer Guide
   (repeat for all 15 files)

3. Upload: zcli article import manifest.yaml
4. Verify import
5. Manual formatting (if needed)
6. Publish

Time: ~5 minutes setup + 20 minutes formatting
```

#### **Step 4: Structure & Navigation**

```
Zendesk Navigation Setup:

Category 1: Getting Started
├─ 02-GETTING-STARTED.md
├─ 05-INSTALLATION-SETUP.md
└─ 16-KEYBOARD-SHORTCUTS.md

Category 2: User Guides
├─ 01-USER-GUIDE.md
├─ 13-FEATURES-COMPLETE.md
└─ 14-EXPORT-FORMATS.md

Category 3: Developer & API
├─ 03-DEVELOPER-GUIDE.md
├─ 04-ARCHITECTURE.md
├─ 06-API-REFERENCE.md
└─ 07-CONTRIBUTING.md

Category 4: Advanced Topics
├─ 17-PHYSICS-MODELS.md
├─ 10-TESTING-QUALITY.md
├─ 11-PERFORMANCE.md
└─ 15-SETTINGS-REFERENCE.md

Category 5: Support & Operations
├─ 08-TROUBLESHOOTING.md
├─ 09-DATA-MODELS.md
├─ 12-DEPLOYMENT.md
└─ 00-INDEX.md (Redirect or reference)
```

#### **Step 5: Customize Appearance**

```
1. Guide Admin → Customization
2. Theme:
   - Upload logo (400×100 min)
   - Brand colors (primary, secondary)
   - Font: Choose system fonts
   - Header: Customize navigation

3. Branding:
   - Company name
   - Footer links
   - Social media links
   - Support contact

4. Search:
   - Enable full-text search
   - Highlight keywords
   - Show article count per category

5. Mobile:
   - Responsive theme (default)
   - Test on devices
```

#### **Step 6: Enable Support Integration**

```
Linking to Zendesk Support:
1. Guide Admin → Support Connection
2. Enable: "Submit ticket from article"
3. Configure:
   - Form fields
   - Default category
   - Routing rules

Result:
- Users can submit tickets directly from docs
- Links to support
- Integrated help experience
```

### Zendesk Best Practices

```
✓ Use clear, short titles (< 60 chars)
✓ Front-load important info
✓ Use code blocks for examples
✓ Link related articles
✓ Add keywords for search
✓ Update FAQs from support tickets
✓ Track analytics to find gaps
✓ Use consistent formatting
✓ Test on mobile devices
✓ Monitor search metrics

✗ Don't use overly technical language
✗ Don't embed large files (use links)
✗ Don't duplicate content across articles
✗ Don't forget to update when product changes
✗ Don't ignore user feedback/analytics
```

### Zendesk Analytics

```
Monitor:
1. Most viewed articles
   → Update if no engagement
   → Feature top articles

2. Search queries with no results
   → Add new articles
   → Improve tagging

3. Articles with high bounce rate
   → Clarity issues?
   → Wrong content?
   → Link to related topics

4. Support ticket reduction
   → Track tickets before/after docs
   → Measure ROI

Location: Guide Admin → Analytics
Frequency: Review weekly/monthly
```

---

## GitBook Setup

### Prerequisites

```
✓ GitBook account (free or paid)
✓ GitHub account (for integration)
✓ Repository with documentation files
✓ Basic Git knowledge
```

### Step-by-Step Setup

#### **Step 1: Create GitBook Space**

```
1. Sign in to GitBook
2. Click "Create new"
3. New Space:
   - Name: "FabSim Documentation"
   - Description: "Complete documentation for FabSim"
   - Visibility: Public or Internal
   - Template: Choose or start blank

4. Customize:
   - Upload logo/favicon
   - Set brand colors
   - Choose layout (sidebar, docs)
```

#### **Step 2: Configure GitHub Sync**

```
1. Space Settings → Integrations
2. GitHub:
   - Select repository
   - Select branch (main/docs)
   - Auto-sync: Enable

3. Select folder: /app-documentation

4. Sync configuration:
   - Push to GitHub: Yes (from GitBook)
   - Pull from GitHub: Yes (keep in sync)
   - Conflict resolution: Git wins (version control truth)

Result:
- Edits in GitBook sync to GitHub
- Edits in GitHub sync to GitBook
- Single source of truth
```

#### **Step 3: Structure Navigation**

```
GitBook Table of Contents (SUMMARY.md):

# Summary

## Getting Started
- [Quick Start](02-GETTING-STARTED.md)
- [Installation](05-INSTALLATION-SETUP.md)
- [First Project Tutorial](01-USER-GUIDE.md#getting-started)

## User Documentation
- [Complete User Guide](01-USER-GUIDE.md)
- [All Features](13-FEATURES-COMPLETE.md)
  - [Weave Design](13-FEATURES-COMPLETE.md#2d-weave-design)
  - [3D Simulation](13-FEATURES-COMPLETE.md#3d-simulation--physics)
  - [Model Builder](13-FEATURES-COMPLETE.md#model-builder-ai-assisted)
  - [Export Formats](14-EXPORT-FORMATS.md)
- [Keyboard Shortcuts](16-KEYBOARD-SHORTCUTS.md)
- [Settings Reference](15-SETTINGS-REFERENCE.md)

## Developer Guide
- [Getting Started (Dev)](03-DEVELOPER-GUIDE.md)
- [Architecture](04-ARCHITECTURE.md)
- [API Reference](06-API-REFERENCE.md)
- [Contributing](07-CONTRIBUTING.md)

## Advanced Topics
- [Physics Models](17-PHYSICS-MODELS.md)
- [Testing & QA](10-TESTING-QUALITY.md)
- [Performance Optimization](11-PERFORMANCE.md)

## Operations
- [Deployment](12-DEPLOYMENT.md)
- [Troubleshooting](08-TROUBLESHOOTING.md)
- [Data Models](09-DATA-MODELS.md)

## Reference
- [Architecture Deep Dive](04-ARCHITECTURE.md)
- [API Reference](06-API-REFERENCE.md)
- [Index](00-INDEX.md)
```

#### **Step 4: Create SUMMARY.md**

```markdown
# FabSim Documentation

## Getting Started
* [Welcome](README.md)
* [Quick Start](02-GETTING-STARTED.md)
* [Installation](05-INSTALLATION-SETUP.md)

## User Guide
* [User Guide](01-USER-GUIDE.md)
* [All Features](13-FEATURES-COMPLETE.md)
* [Keyboard Shortcuts](16-KEYBOARD-SHORTCUTS.md)
* [Settings Reference](15-SETTINGS-REFERENCE.md)

## Developer Guide
* [Developer Guide](03-DEVELOPER-GUIDE.md)
* [Architecture](04-ARCHITECTURE.md)
* [API Reference](06-API-REFERENCE.md)
* [Contributing](07-CONTRIBUTING.md)

## Advanced
* [Physics Models](17-PHYSICS-MODELS.md)
* [Export Formats](14-EXPORT-FORMATS.md)
* [Testing & QA](10-TESTING-QUALITY.md)
* [Performance](11-PERFORMANCE.md)

## Operations
* [Deployment](12-DEPLOYMENT.md)
* [Troubleshooting](08-TROUBLESHOOTING.md)
```

#### **Step 5: Add Cover & README**

```
Create book.json:
{
  "title": "FabSim Documentation",
  "description": "Complete documentation for FabSim 0.4.0",
  "author": "Hamed Rezaei",
  "lang": "en",
  "root": "./",
  "plugins": [
    "prism",
    "copy-code-button",
    "search-pro",
    "mermaid",
    "github"
  ],
  "pluginsConfig": {
    "github": {
      "url": "https://github.com/yourusername/FabSim"
    }
  }
}

Create README.md:
(Use as intro/welcome page)
```

#### **Step 6: Enable Features**

```
Space Settings:

1. Publishing:
   - Custom domain: docs.fabsim.example.com
   - SSL: Automatic (let's encrypt)
   - Analytics: Enable

2. Search:
   - Full-text search: Enabled
   - Search analytics: Enabled

3. Versions:
   - Create version for each release
   - Link to previous versions
   - Set current as default

4. Comments:
   - Reader comments: Enable (optional)
   - Moderation: Enable
```

### GitBook Best Practices

```
✓ Use clear nested structure
✓ Link across documents
✓ Use code syntax highlighting
✓ Include table of contents
✓ Version docs with releases
✓ Keep Git repository clean
✓ Use consistent formatting
✓ Test on desktop and mobile
✓ Monitor search analytics
✓ Enable version control

✗ Don't mix docs and code in same directory
✗ Don't ignore sync conflicts
✗ Don't leave old content
✗ Don't forget to commit changes
✗ Don't use absolute paths for links
```

---

## GitHub Pages Setup

### Prerequisites

```
✓ GitHub repository
✓ README.md in root
✓ /app-documentation folder with .md files
✓ Basic Git knowledge
```

### Step-by-Step Setup

#### **Step 1: Enable GitHub Pages**

```
1. Repository Settings → Pages
2. Source: Deploy from branch
3. Branch: main (or docs branch)
4. Folder: / (root) or /docs
5. Custom domain: docs.fabsim.example.com (optional)
6. Enforce HTTPS: Yes

Site URL: username.github.io/FabSim
or: docs.fabsim.example.com (if custom)
```

#### **Step 2: Choose Theme & Build**

```
Option A: Use GitHub Pages Theme (Simple)
1. Settings → Pages → Theme Chooser
2. Select: "Minimal", "Slate", "Architect", etc.
3. Select README.md as homepage
4. Theme applies automatically

Option B: Jekyll Configuration (Recommended)
1. Create _config.yml:
   theme: jekyll-theme-minimal
   title: FabSim Documentation
   description: Complete documentation
   logo: assets/logo.png

2. Create index.md (homepage)
3. Jekyll builds automatically on push
```

#### **Step 3: Create Navigation**

```
Method A: Index Page (Simple)
1. Create docs/index.md (or README.md)
2. List all documentation:
   - [Quick Start](02-GETTING-STARTED.md)
   - [User Guide](01-USER-GUIDE.md)
   - etc.

Method B: Wiki-Style (Better)
1. Enable GitHub Wiki
2. Create Home page
3. Link to all docs
4. Better navigation support

Method C: Table of Contents (Best)
1. Create comprehensive index
2. Use markdown headings for navigation
3. Link to specific sections
```

#### **Step 4: Configure Jekyll Build**

```
Create _config.yml:

# Site
title: FabSim Documentation
description: Complete User & Developer Guide
url: https://docs.fabsim.example.com
repository: username/FabSim

# Build
theme: jekyll-theme-minimal
markdown: kramdown
kramdown:
  input: GFM
  syntax_highlighter: rouge

# Plugins
plugins:
  - jekyll-sitemap
  - jekyll-seo-tag
  - jekyll-gist
  - jekyll-mentions
  - jekyll-github-metadata

# Collections
collections:
  guides:
    output: true
    permalink: /guides/:path/
```

#### **Step 5: Add SEO & Metadata**

```
Create _includes/head-custom.html:

<meta name="keywords" content="fabric simulation, weave design, physics">
<meta name="og:title" content="FabSim Documentation">
<meta name="og:description" content="Complete guide">
<meta name="og:image" content="/assets/og-image.png">
<link rel="canonical" href="https://docs.fabsim.example.com">

Add to each page frontmatter:

---
layout: default
title: Feature Title
description: Short description
keywords: keyword1, keyword2
---
```

#### **Step 6: Auto-Publish Documentation**

```
GitHub Actions Workflow (auto-publish on push):

Create .github/workflows/docs.yml:

name: Publish Docs
on:
  push:
    branches: [main]
    paths: ['app-documentation/**']

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.7
          bundler-cache: true
      - run: bundle exec jekyll build --source app-documentation
      - uses: actions/upload-artifact@v2
        with:
          name: site
          path: _site/
```

### GitHub Pages Best Practices

```
✓ Use meaningful filenames
✓ Add frontmatter to all .md files
✓ Link using relative paths
✓ Include sitemap (auto-generated)
✓ Use code highlighting
✓ Mobile-responsive design
✓ Fast loading times
✓ Clear navigation
✓ Search functionality (see below)

✗ Don't use absolute paths
✗ Don't rely on GitHub wiki (use Pages instead)
✗ Don't ignore performance
✗ Don't forget mobile testing
✗ Don't mix versioning schemes
```

### Adding Search to GitHub Pages

```
Option A: Simple (No Search)
- Users rely on browser Ctrl+F

Option B: Algolia Search (Recommended)
1. Sign up: algolia.com
2. Create index
3. Install: npm install @algolia/autocomplete-js
4. Configure Jekyll plugin
5. Add search box to layout

Option C: Lunr.js (Simple)
1. Add to layout via CDN
2. Index generated automatically
3. Works offline
4. Self-hosted search
```

---

## Multi-Platform Deployment

### Deployment Strategy

```
Phase 1: GitHub Pages (Week 1)
- Fast, free, low-friction
- Internal/community focus
- Version control native

Phase 2: GitBook (Week 2)
- Better formatting, versioning
- Developer-friendly
- Integrates with GitHub

Phase 3: Zendesk Guide (Week 3)
- Customer-facing help center
- Support integration
- Analytics & monitoring
```

### Maintaining Sync

```
GitHub (Source of Truth)
   ↓
   ├→ GitHub Pages (auto via Actions)
   ├→ GitBook (sync configured)
   └→ Zendesk (manual or API)

Workflow:
1. Update .md files in GitHub
2. Commit & push
3. GitHub Actions trigger
4. Publish to Pages (auto)
5. GitBook syncs (auto)
6. Zendesk import (manual or scheduled)
```

### Avoiding Duplicates & Confusion

```
✓ Primary URL: docs.fabsim.example.com (CloudFlare redirect)
✓ GitHub Pages: github.com/user/FabSim/docs (secondary)
✓ GitBook: gitbook.com/yourspace (internal/dev)
✓ Zendesk: help.fabsim.example.com (support focus)

Redirect Strategy:
- docs.fabsim.example.com → Primary platform
- help.fabsim.example.com → Zendesk (support)
- api.fabsim.example.com → API docs (GitHub Pages)
- dev.fabsim.example.com → GitBook (developers)
```

---

## Content Optimization

### For Markdown Rendering

```
Compatibility Checklist:

Headers: # - ###### (1-6 levels)
✓ All platforms support

Bold/Italic: **bold**, *italic*
✓ All platforms support

Code: `inline` and ```blocks```
✓ All platforms support

Links: [text](url)
✓ All platforms support
⚠ Use relative paths for internal links

Images: ![alt](path)
✓ GitHub Pages: full support
✓ GitBook: full support
✓ Zendesk: requires URL, not local

Lists: - * + (ordered, unordered)
✓ All platforms support

Tables: | header | header |
✓ All platforms support

Blockquotes: > text
✓ All platforms support

Code highlighting: ```python
✓ GitHub Pages: via Pygments
✓ GitBook: via highlight.js
✓ Zendesk: limited support
```

### Image & Asset Management

```
Repository Structure:
```
/app-documentation/
├── 01-USER-GUIDE.md
├── 02-...
├── assets/
│   ├── diagrams/
│   │   ├── architecture.png
│   │   ├── workflow.png
│   │   └── physics.svg
│   ├── screenshots/
│   │   ├── weave-design.png
│   │   └── 3d-viewport.png
│   └── icons/
│       ├── tip.svg
│       └── warning.svg
└── _config.yml
```

Image Best Practices:
- PNG: Screenshots, diagrams (~200-500 KB typical)
- SVG: Icons, diagrams (scalable, small)
- JPEG: Photos (if included)
- Max size: 1 MB per image
- Width: 600-800px for docs

Link syntax:
```
GitHub Pages: ![alt](assets/diagrams/file.png)
GitBook: ![alt](/assets/diagrams/file.png)
Zendesk: ![alt](https://domain.com/assets/file.png)
```
```

### Link Handling

```
Internal Links (Relative):
✓ [User Guide](01-USER-GUIDE.md)
✓ [Export Guide](14-EXPORT-FORMATS.md#obj-format)
✓ [Troubleshooting](08-TROUBLESHOOTING.md)

Cross-Document Links (Zendesk):
- Use article titles or IDs
- Format: `/en-us/articles/XXXXX`
- Test after import

External Links (Absolute):
✓ https://github.com/fabsim
✓ https://example.com
```

---

## SEO & Discoverability

### SEO Optimization

```
For all platforms:

1. Titles (60 chars or less):
   ✓ Features Reference Guide
   ✓ Physics Models Explained
   ✓ Export Formats Complete Guide

2. Meta Descriptions (160 chars):
   ✓ "Complete reference for all FabSim export formats including OBJ, STL, WIF with examples"

3. Keywords (3-5 per page):
   - Primary: "FabSim"
   - Secondary: "fabric simulation", "weave design"
   - Long-tail: "how to export OBJ in FabSim"

4. Structure:
   - Clear H1 titles
   - Descriptive H2/H3 subheadings
   - Bulleted lists
   - Tables for comparison

5. Content:
   - Front-load keywords
   - Internal links (5-10 per article)
   - External links (authoritative)
   - Word count: 800-2000 words per article
```

### Search Console Setup

```
GitHub Pages:
1. Google Search Console: console.google.com
2. Add property: docs.fabsim.example.com
3. Verify ownership (DNS or file)
4. Submit sitemap: /sitemap.xml
5. Monitor: Coverage, Performance, Enhancements

GitBook:
1. Settings → SEO
2. Auto-sitemap: Enable
3. Structured data: Enable
4. Custom domain verification

Zendesk:
1. Admin → Customize Design
2. Add meta tags (title, description)
3. Enable analytics
4. Submit to Google Search Console
```

### Analytics Setup

```
GitHub Pages + Google Analytics:
1. Create Google Analytics 4 property
2. Add script to _includes/head.html
3. Track: pages, events, user flow

GitBook Analytics (Built-in):
1. Space Settings → Analytics
2. View: Popular pages, search queries
3. Export: CSV reports

Zendesk Analytics (Built-in):
1. Guide Admin → Analytics
2. View: Article views, search metrics
3. Track: Support ticket reduction
4. Monitor: Bounce rate, time on page
```

---

## Maintenance & Updates

### Version Management

```
Release Cycle:
- GitHub: Link to releases page
- GitBook: Create version per release (e.g., v0.4.0)
- Zendesk: Update in place (track in release notes)

Version Structure:
/releases/
├── v0.3.0/ (archived)
├── v0.4.0/ (current)
└── v0.5.0-beta/ (preview)

Each version:
- Date released
- Link to release notes
- Change summary
```

### Update Process

```
When documentation changes:

1. Update source files in GitHub
2. Commit with message: "docs: add feature X"
3. Push to main branch

4. GitHub Pages: Auto-deploys (~30 seconds)
5. GitBook: Auto-syncs (~1 minute)
6. Zendesk: Manual or API sync

Schedule:
- Minor updates: Weekly
- Major updates: Monthly
- Critical fixes: ASAP
- Bulk migration: Quarterly
```

### Archival Strategy

```
Keep accessible:
- Current version: Prominent
- Previous version: Linked in footer
- Next version (beta): Separate URL

Archive procedure:
1. Move to /archive/ directory
2. Create redirect from old URL
3. Update sitemap
4. Update all cross-references
5. Notify users via blog post

Retention:
- Keep 2 previous major versions
- Archive rest in GitHub (searchable)
- PDFs as backups (optional)
```

### Backup & Recovery

```
Automated Backups:
1. GitHub: Native version control (free)
2. GitBook: Built-in (versioning)
3. Zendesk: Export periodically (CSV)

Manual Backups:
- Monthly: Export all docs to .zip
- Store: Google Drive or S3
- Redundancy: Multiple locations

Recovery:
- From GitHub: Git checkout <version>
- From GitBook: Restore previous version
- From Zendesk: Re-import from backup
```

---

## Migration Checklist

### Pre-Launch

- [ ] All 15 .md files reviewed
- [ ] Links tested (internal & external)
- [ ] Images in place and optimized
- [ ] Code examples verified
- [ ] Mobile responsive tested
- [ ] Search functionality tested
- [ ] SEO metadata added
- [ ] Analytics configured
- [ ] Custom domain verified
- [ ] SSL/TLS certificate active
- [ ] Redirect rules configured
- [ ] Backup strategy planned

### Launch

- [ ] Announce new docs (email, blog)
- [ ] Update website links
- [ ] Update app help menu
- [ ] Update support template links
- [ ] Notify support team
- [ ] Train content team
- [ ] Monitor initial traffic
- [ ] Check analytics

### Post-Launch

- [ ] Weekly: Monitor analytics
- [ ] Monthly: Update content
- [ ] Monthly: Review search queries
- [ ] Quarterly: Full audit
- [ ] Quarterly: User feedback review
- [ ] Yearly: Comprehensive update
- [ ] Continuous: Fix broken links

---

## Getting Help

For deployment support:
- GitHub Pages docs: [pages.github.com](https://pages.github.com)
- GitBook docs: [docs.gitbook.com](https://docs.gitbook.com)
- Zendesk Help: [support.zendesk.com](https://support.zendesk.com)
- Community: Stack Overflow, GitHub Discussions

For documentation issues:
- Contact: support@fabsim.example.com
- Feedback: [Submit issue](https://github.com/fabsim/docs)
