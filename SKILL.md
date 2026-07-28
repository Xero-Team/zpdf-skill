---
name: zpdf
description: Comprehensive PDF toolkit skill. Use zpdf CLI for all PDF tasks — reading, analysis, conversion, editing, and optimization. Pure Rust implementation with native support for encryption, CJK fonts, forms, and complex layouts.
version: 2.0.0
---

# zpdf: Complete PDF Toolkit

Use zpdf CLI for all PDF operations. This skill covers the full zpdf command set.

## Reading & Analysis

### Core Reading Commands

```bash
# Get PDF metadata and page count (ALWAYS run first)
zpdf info <file.pdf> [--password <pw>]

# Extract text from pages
zpdf text <file.pdf> -p <page> [--password <pw>]
zpdf text <file.pdf> --all [--struct] [--password <pw>]

# Render page to image
zpdf render <file.pdf> -p <page> -o output.png [--dpi 150] [--backend cpu|wgpu] [--password <pw>]

# Search content
zpdf search <file.pdf> <query> [-p <page>] [--case-sensitive] [--password <pw>]

# Convert to text/HTML/Markdown
zpdf convert <file.pdf> -o output.md [--format txt|md|html] [--mode text|rich] [-p <page>|--all] [--password <pw>]

# Export to PowerPoint
zpdf export-pptx <file.pdf> -o output.pptx [-p <page>|--pages 1,3-5|--all] [--password <pw>]
```

### Structured Data Extraction

```bash
# Extract tables
zpdf tables <file.pdf> -p <page> [--all] [--csv] [--password <pw>]

# Extract form fields and values
zpdf forms <file.pdf> [--password <pw>]

# Document outline/TOC
zpdf outline <file.pdf> [--password <pw>]

# Extract hyperlinks
zpdf links <file.pdf> [--password <pw>]

# PDF structure tree (accessibility)
zpdf struct <file.pdf> [--password <pw>]

# Digital signatures
zpdf signatures <file.pdf> [--trust roots.pem] [--password <pw>]

# Embedded files
zpdf attachments <file.pdf> [--extract all] [--out-dir ./] [--password <pw>]

# PDF/A validation
zpdf validate <file.pdf> [--profile pdfa-1b|pdfa-2b] [--password <pw>]
```

## Editing & Creation

### Modification Commands

```bash
# Fill form fields
zpdf fill <file.pdf> --set name=value --set email=foo@bar.com -o filled.pdf

# Merge multiple PDFs
zpdf merge a.pdf b.pdf c.pdf -o combined.pdf

# Split pages
zpdf split <file.pdf> --pages 1,3-5 -o output.pdf [--password <pw>]

# Optimize/compress
zpdf optimize <file.pdf> -o optimized.pdf [--max-image-dim 2048] [--password <pw>]
zpdf optimize <file.pdf> -o encrypted.pdf --encrypt aes256 --user-password secret --owner-password admin

# Add annotations
zpdf annotate <file.pdf> -p 1 --kind highlight --rect 100,200,300,250 --color 255,255,0 -o annotated.pdf
zpdf annotate <file.pdf> -p 2 --kind note --at 100,500 --text "Review this" --icon Comment -o noted.pdf
zpdf annotate <file.pdf> -p 3 --kind freetext --rect 50,600,200,650 --text "Important!" --size 12 -o marked.pdf

# Redact content
zpdf redact <file.pdf> -p 1 --rect 100,200,300,250 --fill 0,0,0 -o redacted.pdf [--password <pw>]

# Digital signing
zpdf sign <file.pdf> --key key.p8.der --cert cert.der --name "John Doe" --reason "Approval" -o signed.pdf

# Page operations
zpdf pages <file.pdf> --rotate 1-5:90 --delete 7,9 --order 1,3,2,4-10 -o reordered.pdf

# Set metadata
zpdf set-meta <file.pdf> --title "New Title" --author "Author Name" -o updated.pdf

# Add text stamp
zpdf stamp <file.pdf> -p 1 --text "DRAFT" --at 300,700 --font Helvetica --size 48 --color 255,0,0 -o stamped.pdf
```

## Debugging & Inspection

```bash
# Dump raw object
zpdf dump <file.pdf> <obj_num> <gen_num> [--password <pw>]

# Debug content stream
zpdf debug-stream <file.pdf> <obj_num> <gen_num>

# Visual diff (images)
zpdf compare a.png b.png --out diff.png [--threshold 10]
```

## Decision Flow

### When user asks to read/analyze a PDF:

1. **Check metadata**: `zpdf info <file.pdf>`
2. **Choose strategy**:
   - **Text extraction** → `zpdf text` for prose, search corpus
   - **Image rendering** → `zpdf render` when layout/diagrams matter
   - **Structured data** → `zpdf tables` for tables, `zpdf forms` for forms
   - **Search first** → `zpdf search` for large documents
3. **Extract selectively** — use page numbers from search results

### When user asks to modify a PDF:

1. **Forms** → `zpdf fill`
2. **Combine/split** → `zpdf merge` or `zpdf split`
3. **Compress** → `zpdf optimize`
4. **Mark up** → `zpdf annotate` (highlights, notes, shapes)
5. **Remove content** → `zpdf redact`
6. **Protect** → `zpdf optimize --encrypt`
7. **Sign** → `zpdf sign`

### When user asks to convert from PDF:

- **Text/Markdown/HTML** → `zpdf convert`
- **PowerPoint** → `zpdf export-pptx`
- **Images** → `zpdf render` with `--dpi 150-300`
- **CSV tables** → `zpdf tables --csv`

## Key Rules

1. **Always start with** `zpdf info` to check pages/encryption
2. **Password handling**: Use `--password <pw>` flag (interactive prompts fail in automation)
3. **DPI defaults**: 150 for readable quality, 300 for high detail
4. **Save temp images to /tmp**: `-o /tmp/page.png` for auto-cleanup
5. **Backend choice**: `cpu` (default, everywhere) or `wgpu` (2-3× faster, needs GPU)
6. **Search before bulk extraction**: Find relevant pages first
7. **Page ranges**: Use `-p 1` or `--pages 1,3-5` or `--all`
