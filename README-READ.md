# zpdf-read: Minimal PDF Reading Skill

A lightweight Claude Code skill for reading PDF content only. Provides just the essential commands for extracting text or rendering pages to images.

## Overview

This is a minimal version of the zpdf skill focused solely on reading PDFs. Use this when you only need to extract content from PDFs without editing or conversion features.

Install zpdf CLI first:
```bash
cargo install zpdf-cli
```

**For full PDF toolkit** (editing, merging, signing, etc.), use the main `zpdf` skill instead.

## When to Use This Skill

**Use zpdf-read when:**
- User asks to read, analyze, or summarize PDF content
- Simple text extraction or rendering needed
- Want minimal context overhead

**Use full zpdf skill when:**
- User needs to edit, merge, split, or optimize PDFs
- Form filling, annotations, or digital signatures required
- Format conversion (PDF → PowerPoint, Markdown, HTML)

## Core Commands

### Essential Reading Operations

```bash
# Get PDF metadata (ALWAYS run first)
zpdf info <file.pdf> [--password <pw>]

# Extract text
zpdf text <file.pdf> -p <page>           # Single page
zpdf text <file.pdf> --all               # All pages
zpdf text <file.pdf> --password secret123 -p 1  # Encrypted

# Render to image
zpdf render <file.pdf> -p <page> -o output.png --dpi 150
zpdf render <file.pdf> -p <page> -o output.png --dpi 300  # High quality

# Search content
zpdf search <file.pdf> "keyword"
zpdf search <file.pdf> "phrase" --case-sensitive
```

## Decision Flow

The skill automatically chooses the right approach based on document type:

### 1. Check Document First
```bash
zpdf info document.pdf
```
Reveals: page count, encryption, dimensions

### 2. Choose Strategy

**Text Extraction** (`zpdf text`) — Use when:
- Text-heavy documents (reports, articles, books, contracts)
- Simple content without complex layouts
- User wants text for summaries, search, or analysis
- Speed and efficiency matter

**Image Rendering** (`zpdf render`) — Use when:
- Visual elements present (diagrams, charts, tables, forms)
- Layout/formatting is important
- Complex page designs
- Vision analysis needed

**Hybrid Approach** — Use when:
- Mixed content (text + visuals)
- Extract text from prose pages
- Render pages with diagrams/charts
- Optimal token efficiency

### 3. Handle Large Documents
```bash
# For documents >10 pages: search first
zpdf search large.pdf "keyword"          # Find relevant pages
zpdf text large.pdf -p 5 -p 12          # Extract only those pages
```

## Quick Examples

### Example 1: Text Report
```bash
zpdf info report.pdf                     # 25 pages, no encryption
zpdf search report.pdf "conclusion"      # Found on page 23
zpdf text report.pdf -p 23 > conclusion.txt
```

### Example 2: Presentation Slides
```bash
zpdf info slides.pdf                     # 15 slides
zpdf render slides.pdf -p 1 -o /tmp/slide1.png --dpi 150
zpdf render slides.pdf -p 5 -o /tmp/slide5.png --dpi 150
# Analyze images with vision
```

### Example 3: Encrypted Document
```bash
zpdf info secure.pdf --password secret123
zpdf text secure.pdf --password secret123 --all > content.txt
```

### Example 4: Technical Manual (Mixed Content)
```bash
zpdf info manual.pdf                     # 50 pages
zpdf text manual.pdf -p 1-10 > intro.txt        # Extract text chapters
zpdf render manual.pdf -p 15 -o diagram.png --dpi 150  # Render diagrams
zpdf render manual.pdf -p 32 -o flowchart.png --dpi 150
```

### Example 5: Large Document
```bash
zpdf info thesis.pdf                     # 150 pages
zpdf search thesis.pdf "methodology"     # Pages 25-35
zpdf text thesis.pdf -p 25 > method.txt
zpdf text thesis.pdf -p 26 >> method.txt
# Extract selectively, not all 150 pages
```

## Key Guidelines

1. **Always start with `zpdf info`** — check page count and encryption
2. **DPI choice**: 150 (default, good balance) or 300 (high detail)
3. **Password via flag**: `--password <pw>` (interactive prompts fail in automation)
4. **Temp file location**: Save images to `/tmp/` for auto-cleanup
5. **Large documents**: Search first, extract selectively
6. **Page limit**: Don't render >10 pages without user confirmation

## Strategy Heuristics

| Document Type | Strategy | Command |
|---------------|----------|---------|
| Report, article, book | Text | `zpdf text --all` |
| Slides, presentation | Image | `zpdf render -p N` |
| Form, application | Image | `zpdf render -p N` |
| Mixed text + charts | Hybrid | `text` + `render` |
| Technical manual | Hybrid | `text` + `render` |
| Contract, legal | Text | `zpdf text --all` |
| Diagram-heavy | Image | `zpdf render -p N` |

## Token Efficiency

**Text extraction**: ~5-10 tokens per page (text only)  
**Image rendering**: ~1,500-3,000 tokens per page (vision input)

**Best practice**: Extract text where possible, render only visual-heavy pages.

## Limitations

This minimal skill does NOT support:
- Form filling
- PDF merging/splitting
- Annotations or markup
- Digital signatures
- Format conversion (PDF → PowerPoint, Markdown, HTML)
- Table extraction
- Optimization/compression

**For these features**, use the full `zpdf` skill.

## File Structure

```
zpdf-skill/
├── SKILL-READ.md         # Minimal reading-only skill (this file)
├── README-READ.md        # Documentation (this file)
├── SKILL.md              # Full skill with all features
└── README.md             # Full skill documentation
```

## Version

- **Skill version**: 1.0.0
- **zpdf version**: 0.11.0+
- **Full skill**: Use `zpdf` skill for complete feature set

## License

Part of the zpdf project (MIT License).
