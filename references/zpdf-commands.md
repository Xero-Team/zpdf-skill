# zpdf Command Reference

Quick reference for all zpdf CLI commands relevant to PDF reading and analysis.

## Reading Commands

### info — PDF Metadata
```bash
zpdf info <file.pdf>
```

**Output**:
- Page count
- PDF version
- Encryption status
- Document metadata (title, author, etc.)
- Page dimensions

**Example**:
```bash
$ zpdf info report.pdf
Pages: 25
Version: PDF-1.7
Encrypted: No
Title: Annual Report 2024
Author: Example Corp
Page size: 8.5 × 11 inches (612 × 792 pt)
```

**Use when**: Need to know page count before extracting/rendering.

---

### text — Extract Text
```bash
zpdf text <file.pdf> [-p <page>] [--all] [--struct] [--password <pw>]
```

**Flags**:
- `-p <N>` — Extract single page (1-indexed)
- `--all` — Extract all pages (sequential output)
- `--struct` — Preserve structural markup (if available)
- `--password <pw>` — Decrypt with password

**Examples**:
```bash
# Single page
zpdf text doc.pdf -p 5

# All pages
zpdf text doc.pdf --all

# Password-protected
zpdf text secure.pdf --password secret123 --all

# Structured (accessibility tree)
zpdf text doc.pdf -p 1 --struct
```

**Output**: Plain text, UTF-8 encoded, preserves line breaks.

**Use when**: Text-heavy PDFs, search corpus building, simple analysis.

---

### render — Render to Image
```bash
zpdf render <file.pdf> -p <page> -o <output.png> [--dpi <dpi>] [--backend cpu|wgpu] [--password <pw>]
```

**Flags**:
- `-p <N>` — Page number (required)
- `-o <path>` — Output PNG path (required)
- `--dpi <N>` — Resolution (default: 72)
  - 72: Quick preview
  - 150: **Recommended default**
  - 300: High quality
- `--backend cpu|wgpu` — Rendering backend (default: cpu)
- `--password <pw>` — Decrypt with password

**Examples**:
```bash
# Standard quality
zpdf render doc.pdf -p 1 -o page1.png --dpi 150

# High quality
zpdf render diagram.pdf -p 3 -o detail.png --dpi 300

# Password-protected
zpdf render secure.pdf -p 1 -o out.png --dpi 150 --password secret

# GPU rendering (faster for complex pages)
zpdf render complex.pdf -p 10 -o out.png --dpi 150 --backend wgpu
```

**Output**: PNG image, 24-bit RGB.

**Use when**: Layout matters, diagrams present, vision capabilities available.

---

### search — Full-Text Search
```bash
zpdf search <file.pdf> "query" [--password <pw>]
```

**Output**: Page numbers and surrounding context for each match.

**Example**:
```bash
$ zpdf search report.pdf "quarterly revenue"
Page 3: ...showing quarterly revenue growth of 15%...
Page 12: ...projected quarterly revenue for Q3...
Page 18: ...quarterly revenue comparison chart...
```

**Use when**: Finding specific content before reading full pages.

---

## Analysis Commands

### tables — Extract Tables
```bash
zpdf tables <file.pdf> -p <page> [--password <pw>]
```

**Output**: Structured table data (CSV-like format).

**Example**:
```bash
$ zpdf tables financial.pdf -p 5
Table 1 (rows: 10, cols: 4):
Quarter | Revenue | Expenses | Profit
Q1 2024 | $1.2M   | $800K    | $400K
Q2 2024 | $1.5M   | $900K    | $600K
...
```

**Use when**: Extracting tabular data without rendering full page.

---

### forms — Extract Form Fields
```bash
zpdf forms <file.pdf> [--password <pw>]
```

**Output**: List of all interactive form fields with current values.

**Example**:
```bash
$ zpdf forms application.pdf
Field: name (Type: text) = "John Doe"
Field: email (Type: text) = "john@example.com"
Field: agree (Type: checkbox) = checked
Field: country (Type: dropdown) = "United States"
```

**Use when**: Reading filled forms or understanding form structure.

---

### outline — Document Outline
```bash
zpdf outline <file.pdf> [--password <pw>]
```

**Output**: Hierarchical table of contents with page references.

**Example**:
```bash
$ zpdf outline manual.pdf
1. Introduction (page 1)
2. Getting Started (page 5)
   2.1. Installation (page 6)
   2.2. Configuration (page 10)
3. Advanced Topics (page 25)
```

**Use when**: Understanding document structure before deep reading.

---

### links — Extract Hyperlinks
```bash
zpdf links <file.pdf> -p <page> [--password <pw>]
```

**Output**: All hyperlinks on specified page.

**Example**:
```bash
$ zpdf links doc.pdf -p 3
External: https://example.com/documentation
Internal: Page 15 (section 3.2)
Email: mailto:support@example.com
```

**Use when**: Extracting references, checking external resources.

---

### attachments — List Embedded Files
```bash
zpdf attachments <file.pdf> [--password <pw>]
```

**Output**: Names and sizes of embedded files (PDF portfolios, ZUGFeRD invoices).

**Example**:
```bash
$ zpdf attachments invoice.pdf
Attachment: invoice_data.xml (2.3 KB)
Attachment: logo.png (15.7 KB)
```

**Use when**: Finding embedded documents within PDFs.

---

### signatures — Verify Digital Signatures
```bash
zpdf signatures <file.pdf> [--trust <cert.pem>] [--password <pw>]
```

**Output**: Signature validity, signer identity, timestamp.

**Example**:
```bash
$ zpdf signatures signed.pdf
Signature 1: Valid
  Signer: John Doe (john@example.com)
  Timestamp: 2024-07-26 10:30:45 UTC
  Certificate: CN=John Doe, O=Example Corp
  Valid: Yes
```

**Use when**: Verifying document authenticity.

---

## Advanced Commands

### struct — PDF Structure Tree
```bash
zpdf struct <file.pdf> -p <page> [--password <pw>]
```

**Output**: Accessibility structure (tagged PDF markup).

**Use when**: Understanding document semantics, accessibility analysis.

---

### debug-stream — Debug PDF Objects
```bash
zpdf debug-stream <file.pdf> <obj-num> <gen-num>
```

**Output**: Decoded content stream or object data.

**Use when**: Debugging malformed PDFs or investigating structure.

---

### compare — Visual Diff
```bash
zpdf compare <file1.pdf> <file2.pdf> [--pages <range>]
```

**Output**: Visual differences between two PDF versions.

**Use when**: Tracking document changes.

---

## Password Handling

All read commands support `--password <pw>` flag:

```bash
zpdf text secure.pdf --password secret123 -p 1
zpdf render secure.pdf -p 1 -o out.png --dpi 150 --password secret123
zpdf info secure.pdf --password secret123
```

**Important**: 
- Always provide password via flag in automation
- Interactive password prompts don't work in scripts
- Wrong password returns error immediately

---

## Common Patterns

### Pattern: Preview then Extract
```bash
# Step 1: Check document
zpdf info document.pdf

# Step 2: Preview first page
zpdf render document.pdf -p 1 -o preview.png --dpi 150

# Step 3: Extract needed pages
zpdf text document.pdf -p 3 > page3.txt
```

### Pattern: Search then Read
```bash
# Step 1: Find relevant pages
zpdf search report.pdf "budget allocation" | grep "Page" | cut -d' ' -f2

# Step 2: Extract those pages
for page in 5 12 18; do
  zpdf text report.pdf -p $page >> relevant.txt
done
```

### Pattern: Batch Image Extraction
```bash
# Get page count
pages=$(zpdf info doc.pdf | grep "Pages:" | awk '{print $2}')

# Render all pages
for p in $(seq 1 $pages); do
  zpdf render doc.pdf -p $p -o page_$p.png --dpi 150
done
```

### Pattern: Table Extraction
```bash
# Extract tables from multiple pages
for p in 5 7 9 11; do
  echo "=== Page $p ===" >> tables.txt
  zpdf tables financial.pdf -p $p >> tables.txt
done
```

---

## Error Handling

Common errors and solutions:

| Error | Cause | Solution |
|-------|-------|----------|
| "File not found" | Path incorrect | Check file exists |
| "Invalid password" | Wrong password | Verify password |
| "Page out of range" | Page number > total | Check `zpdf info` first |
| "Corrupted PDF" | Malformed file | Try `zpdf info` to diagnose |
| "Permission denied" | File locked | Close PDF viewer/reader |

---

## Output Formats

| Command | Output Format | Encoding |
|---------|---------------|----------|
| text | Plain text | UTF-8 |
| render | PNG image | 24-bit RGB |
| tables | Structured text | UTF-8 |
| forms | Key-value pairs | UTF-8 |
| outline | Hierarchical text | UTF-8 |
| links | URL list | UTF-8 |
| signatures | Status report | UTF-8 |

---

## Performance Tips

1. **DPI Choice**:
   - 72 DPI: ~20-40 KB per page, 0.5s render
   - 150 DPI: ~50-150 KB per page, 1-2s render
   - 300 DPI: ~200-500 KB per page, 3-5s render

2. **Backend Selection**:
   - `cpu`: Default, works everywhere
   - `wgpu`: 2-3× faster for complex pages, requires GPU

3. **Selective Extraction**:
   - Use `zpdf search` to find pages before extracting
   - Use `zpdf outline` to understand structure
   - Extract page ranges, not entire documents

4. **Caching**:
   - zpdf parses on every invocation
   - Reuse extracted text/images when possible
   - Don't re-render same page multiple times
