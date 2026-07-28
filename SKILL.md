---
name: zpdf
description: Comprehensive PDF toolkit skill. Use zpdf CLI for all PDF tasks — reading, analysis, conversion, editing, and optimization. Pure Rust implementation with native support for encryption, CJK fonts, forms, and complex layouts.
version: 2.0.0
references:
  - quick-reference.md
  - reading-commands.md
  - analysis-commands.md
  - editing-commands.md
---

# zpdf: Complete PDF Toolkit

Use zpdf CLI for all PDF operations. Load specific reference files as needed:
- **quick-reference.md** — Fast lookup for common operations
- **reading-commands.md** — Extract content (info, text, render, search, convert, export-pptx)
- **analysis-commands.md** — Structured data (tables, forms, outline, links, signatures, attachments)
- **editing-commands.md** — Modify PDFs (fill, merge, split, optimize, annotate, redact, sign)

## Quick Start

```bash
# Always check info first
zpdf info <file.pdf>

# Extract text
zpdf text <file.pdf> -p <page>
zpdf text <file.pdf> --all

# Render to image
zpdf render <file.pdf> -p <page> -o output.png --dpi 150

# Search
zpdf search <file.pdf> "keyword"

# Convert formats
zpdf convert <file.pdf> -o output.md --all
zpdf export-pptx <file.pdf> -o slides.pptx --all

# Extract structured data
zpdf tables <file.pdf> -p <page>
zpdf forms <file.pdf>

# Edit
zpdf fill form.pdf --set field=value -o filled.pdf
zpdf merge a.pdf b.pdf -o combined.pdf
zpdf split doc.pdf --pages 1-5 -o part.pdf
zpdf optimize doc.pdf -o small.pdf --max-image-dim 1024

# Annotate
zpdf annotate doc.pdf -p 1 --kind highlight --rect 100,200,300,220 -o marked.pdf

# Sign
zpdf sign doc.pdf --key key.p8.der --cert cert.der -o signed.pdf
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

## Reference Files

Load these as needed for detailed command syntax:

- **[quick-reference.md](references/quick-reference.md)** — Fast lookup for common operations
- **[reading-commands.md](references/reading-commands.md)** — info, text, render, search, convert, export-pptx
- **[analysis-commands.md](references/analysis-commands.md)** — tables, forms, outline, links, struct, signatures, attachments, validate, compare
- **[editing-commands.md](references/editing-commands.md)** — fill, merge, split, optimize, annotate, redact, sign, pages, set-meta, stamp
- **[zpdf-commands.md](references/zpdf-commands.md)** — Complete reference (all commands, full details)
