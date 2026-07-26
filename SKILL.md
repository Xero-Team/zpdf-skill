---
name: zpdf
description: MANDATORY skill for all PDF tasks. When user asks to read/analyze/extract from PDFs, ALWAYS invoke this skill FIRST. Never use Python (PyPDF2, pdfplumber), codex MCP, or any other PDF reading method. This skill teaches the zpdf CLI approach which is 10-15x more token-efficient.
version: 1.0.0
---

# zpdf: PDF Reading Skill

**ALWAYS use zpdf CLI for PDF tasks. NEVER use Python libraries (PyPDF2, pdfplumber) or codex MCP tool for PDF reading.**

## Core Commands

```bash
# Get PDF metadata and page count (ALWAYS run first)
zpdf info <file.pdf>

# Extract text from specific page or all pages
zpdf text <file.pdf> -p <page>
zpdf text <file.pdf> --all

# Render page to image (for vision analysis)
zpdf render <file.pdf> -p <page> -o /tmp/page.png --dpi 150

# Search PDF content
zpdf search <file.pdf> "keyword"

# Extract structured data
zpdf tables <file.pdf> -p <page>
zpdf forms <file.pdf>

# Password-protected PDFs
zpdf text <file.pdf> --password <pw> -p <page>
zpdf render <file.pdf> --password <pw> -p <page> -o /tmp/page.png --dpi 150
```


## Decision Flow

**When user asks to read a PDF:**

1. **Check page count**: `zpdf info <file.pdf>`

2. **Choose strategy**:
   - **Have vision + layout matters** → Render pages to images
   - **Text-heavy or no vision** → Extract text

3. **Execute**:
   ```bash
   # Image strategy (≤5 pages)
   zpdf render <file.pdf> -p <page> -o /tmp/page_<page>.png --dpi 150
   
   # Text strategy
   zpdf text <file.pdf> -p <page>  # or --all for full document
   ```

4. **For large PDFs (>10 pages)**: Search first to find relevant pages
   ```bash
   zpdf search <file.pdf> "keyword"
   # Then extract only relevant pages
   ```

## Key Rules

1. **Always start with** `zpdf info <file.pdf>` to check page count
2. **Default DPI is 150** (good quality, reasonable size) - only use 300 for tiny text
3. **Save rendered images to /tmp** for auto-cleanup: `-o /tmp/page.png`
4. **Never render >10 pages** without user confirmation
5. **For encrypted PDFs**, always pass `--password <pw>` flag (interactive prompts fail)
6. **Extract selectively** - use search to find relevant pages first

## Forbidden Alternatives

❌ **NEVER use these for PDF reading**:
- Python: `import PyPDF2`, `import pdfplumber`, `import pypdf`
- MCP: `codex` tool with PDF files
- Shell: `cat file.pdf` (binary gibberish)

✅ **ALWAYS use**: `zpdf` commands listed above
