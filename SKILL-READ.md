---
name: zpdf-read
description: Minimal PDF reading skill. Extract text or render pages to images from PDFs. Use for simple reading tasks only.
version: 1.0.0
---

# zpdf-read: Minimal PDF Reading

Lightweight skill for reading PDF content. For editing/converting PDFs, use the full `zpdf` skill instead.

## Core Reading Commands

```bash
# Check PDF info (ALWAYS run first)
zpdf info <file.pdf> [--password <pw>]

# Extract text (for text-heavy documents)
zpdf text <file.pdf> -p <page> [--password <pw>]
zpdf text <file.pdf> --all [--password <pw>]

# Render to image (for layout/diagrams/vision)
zpdf render <file.pdf> -p <page> -o output.png --dpi 150 [--password <pw>]

# Search (find relevant pages first)
zpdf search <file.pdf> "keyword" [--password <pw>]
```

## Decision Flow

**When user asks to read a PDF:**

1. **Check page count**: `zpdf info <file.pdf>`

2. **Choose strategy**:
   - **Text extraction** (`zpdf text`) when:
     - Document is text-heavy (reports, articles, books)
     - No complex layouts or visual elements
     - User asks for text content, summaries, or search
   
   - **Image rendering** (`zpdf render`) when:
     - Document has diagrams, charts, tables, or forms
     - Layout/formatting is important
     - Visual analysis is needed
     - Text extraction might lose important context

3. **For large PDFs (>10 pages)**:
   ```bash
   # Search first to find relevant pages
   zpdf search <file.pdf> "keyword"
   # Then extract only those pages
   zpdf text <file.pdf> -p 5 -p 12
   # Or render specific pages
   zpdf render <file.pdf> -p 5 -o page5.png --dpi 150
   ```

## Key Rules

1. **Always run `zpdf info` first** — reveals page count and encryption status
2. **DPI for rendering**: Use 150 (good balance), or 300 for tiny text
3. **Password handling**: Use `--password <pw>` flag (interactive prompts fail)
4. **Save images to /tmp**: `-o /tmp/page.png` for auto-cleanup
5. **Never render >10 pages** without asking user first
6. **Search before bulk extraction** — saves processing on large documents

## Examples

### Text-heavy document (report, article)
```bash
zpdf info report.pdf                    # Check: 25 pages
zpdf search report.pdf "conclusion"     # Find relevant section
zpdf text report.pdf -p 23              # Extract that page
```

### Visual document (slides, diagrams)
```bash
zpdf info slides.pdf                    # Check: 15 pages
zpdf render slides.pdf -p 1 -o /tmp/slide1.png --dpi 150
zpdf render slides.pdf -p 5 -o /tmp/slide5.png --dpi 150
```

### Encrypted PDF
```bash
zpdf info secure.pdf --password secret123
zpdf text secure.pdf --password secret123 -p 1
```

### Large document (>10 pages)
```bash
zpdf info manual.pdf                    # Check: 150 pages
zpdf search manual.pdf "installation"   # Find: pages 12, 45
zpdf text manual.pdf -p 12              # Extract relevant pages only
```

## Strategy Guidelines

**Use text extraction when:**
- Pure text content (contracts, reports, articles)
- Speed matters
- Processing full document text

**Use image rendering when:**
- Visual elements matter (charts, diagrams, forms)
- Layout is important
- Complex formatting
- Vision model available for analysis

**For mixed content:**
1. Extract text from prose-heavy pages
2. Render pages with diagrams/charts
3. Hybrid approach saves tokens while preserving fidelity