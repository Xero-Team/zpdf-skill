# README: zpdf Skill

A comprehensive Claude Code skill for PDF processing using the zpdf CLI tool — a pure Rust PDF toolkit with full read, write, and conversion capabilities.

## Overview

This skill teaches Claude to use zpdf CLI for all PDF tasks: reading, analysis, conversion, editing, optimization, and document creation.

Install zpdf CLI first:
```bash
cargo install zpdf-cli
```

**Key features**:

- **Complete PDF toolkit**: Read, write, convert, merge, split, annotate, sign, encrypt
- **Native encryption**: Password-protected PDFs handled seamlessly
- **CJK support**: Chinese/Japanese/Korean fonts work correctly
- **Multiple backends**: CPU (default) and GPU (wgpu) rendering
- **Structured extraction**: Tables, forms, links, outlines, attachments
- **Format conversion**: PDF → Text/Markdown/HTML/PowerPoint

## When This Skill Activates

Triggers when user asks to:

**Reading/Analysis**:
- "Read this PDF"
- "Extract text/tables from PDF"
- "What's in this PDF?"
- "Summarize PDF"
- "Search PDF for..."

**Conversion**:
- "Convert PDF to Markdown/HTML/PowerPoint"
- "Export PDF pages as images"

**Editing**:
- "Merge/split PDFs"
- "Fill PDF form"
- "Add annotation/watermark to PDF"
- "Compress/optimize PDF"
- "Encrypt/decrypt PDF"
- "Sign PDF digitally"

## Quick Usage

### Reading Commands

```bash
# Get PDF info (ALWAYS run first)
zpdf info <file.pdf>

# Extract text
zpdf text <file.pdf> -p <page>           # Single page
zpdf text <file.pdf> --all               # All pages

# Render to image
zpdf render <file.pdf> -p <page> -o output.png --dpi 150

# Search content
zpdf search <file.pdf> "keyword" [--case-sensitive]

# Convert formats
zpdf convert <file.pdf> -o output.md --format md --all
zpdf export-pptx <file.pdf> -o slides.pptx --all

# Extract structured data
zpdf tables <file.pdf> -p <page> [--csv]
zpdf forms <file.pdf>
zpdf outline <file.pdf>
zpdf links <file.pdf>
zpdf attachments <file.pdf> --extract all --out-dir ./
```

### Editing Commands

```bash
# Fill form
zpdf fill form.pdf --set name="John Doe" --set email="john@example.com" -o filled.pdf

# Merge PDFs
zpdf merge a.pdf b.pdf c.pdf -o combined.pdf

# Split pages
zpdf split document.pdf --pages 1,3-5 -o selected.pdf

# Optimize/compress
zpdf optimize large.pdf -o small.pdf --max-image-dim 2048

# Encrypt
zpdf optimize doc.pdf -o secure.pdf --encrypt aes256 --user-password secret

# Annotate
zpdf annotate doc.pdf -p 1 --kind highlight --rect 100,200,300,250 --color 255,255,0 -o marked.pdf

# Redact
zpdf redact sensitive.pdf -p 2 --rect 50,100,200,150 --fill 0,0,0 -o redacted.pdf

# Sign
zpdf sign doc.pdf --key key.p8.der --cert cert.der --name "Jane Smith" -o signed.pdf

# Stamp
zpdf stamp doc.pdf -p 1 --text "DRAFT" --at 300,700 --size 48 --color 255,0,0 -o stamped.pdf
```

## File Structure

```
zpdf-skill/
├── SKILL.md                      # Main skill definition
├── README.md                     # This file
└── references/
    ├── quick-reference.md        # Fast lookup for common operations
    ├── reading-commands.md       # Reading & extraction commands
    ├── analysis-commands.md      # Structured data & analysis commands
    ├── editing-commands.md       # Modification & optimization commands
    └── zpdf-commands.md          # Complete reference (all commands)
```

## Reference Files

The skill uses split reference files to reduce token overhead:

- **[quick-reference.md](references/quick-reference.md)** — Fast lookup card for common operations (~1.5K tokens)
- **[reading-commands.md](references/reading-commands.md)** — info, text, render, search, convert, export-pptx (~1.8K tokens)
- **[analysis-commands.md](references/analysis-commands.md)** — tables, forms, outline, links, signatures, attachments (~1.5K tokens)
- **[editing-commands.md](references/editing-commands.md)** — fill, merge, split, optimize, annotate, redact, sign (~2.5K tokens)
- **[zpdf-commands.md](references/zpdf-commands.md)** — Complete reference with all details (~6K tokens)

Load only the reference files needed for the specific task.

## Key Features

### 1. Complete Read/Write Cycle

```bash
# Read → Modify → Write
zpdf info doc.pdf
zpdf text doc.pdf -p 1
zpdf fill doc.pdf --set field=value -o updated.pdf
zpdf sign updated.pdf --key key.p8.der --cert cert.der -o final.pdf
```

### 2. Native Encryption Handling

```bash
# Read encrypted PDFs
zpdf text secure.pdf --password secret123 --all

# Create encrypted PDFs
zpdf optimize doc.pdf -o encrypted.pdf --encrypt aes256 --user-password user123 --owner-password admin456
```

### 3. CJK Font Support

```bash
# Chinese/Japanese/Korean PDFs extract correctly
zpdf text chinese.pdf -p 1
zpdf convert japanese.pdf -o output.md --all
```

### 4. Format Conversion

```bash
# PDF → Markdown (preserves structure)
zpdf convert paper.pdf -o paper.md --format md --mode rich --all

# PDF → HTML (with images)
zpdf convert doc.pdf -o doc.html --format html --images-dir ./images --all

# PDF → PowerPoint
zpdf export-pptx slides.pdf -o presentation.pptx --all

# PDF → Images
for p in 1 2 3; do
  zpdf render doc.pdf -p $p -o page_$p.png --dpi 150
done
```

### 5. Structured Data Extraction

```bash
# Extract tables as CSV
zpdf tables financial.pdf -p 5 --csv > data.csv

# Extract form fields
zpdf forms application.pdf

# Extract embedded files (ZUGFeRD invoices, portfolios)
zpdf attachments invoice.pdf --extract all --out-dir ./extracted/
```

### 6. Document Manipulation

```bash
# Merge with custom page order
zpdf merge intro.pdf --pages 1-3 body.pdf appendix.pdf --pages 1,5 -o final.pdf

# Rotate, delete, reorder pages
zpdf pages doc.pdf --rotate 1-5:90 --delete 7,9 --order 1,3,2,4-10 -o modified.pdf

# Batch processing
for f in *.pdf; do
  zpdf optimize "$f" -o "optimized/$f" --max-image-dim 1024
done
```

### 7. Annotations & Markup

```bash
# Highlight text
zpdf annotate doc.pdf -p 1 --kind highlight --rect 100,200,300,220 --color 255,255,0 -o marked.pdf

# Add sticky note
zpdf annotate doc.pdf -p 2 --kind note --at 100,500 --text "Review this section" --icon Comment -o noted.pdf

# Add text box
zpdf annotate doc.pdf -p 3 --kind freetext --rect 50,600,200,650 --text "Important!" --size 14 --color 255,0,0 -o annotated.pdf

# Draw shapes
zpdf annotate doc.pdf -p 1 --kind circle --rect 200,300,250,350 --color 0,0,255 --width 2 -o circled.pdf
zpdf annotate doc.pdf -p 1 --kind line --rect 100,100,300,200 --color 255,0,0 --width 3 -o lined.pdf
```

### 8. Digital Signatures

```bash
# Sign PDF (requires PKCS#8 key and X.509 cert in DER format)
zpdf sign contract.pdf \
  --key private_key.p8.der \
  --cert certificate.der \
  --name "John Doe" \
  --reason "Contract approval" \
  --location "New York" \
  -o signed_contract.pdf

# Verify signatures
zpdf signatures signed_contract.pdf --trust trusted_roots.pem
```

## Design Principles

1. **Always check info first**: `zpdf info <file>` reveals pages, encryption, structure
2. **Search before bulk extraction**: Find relevant pages to minimize processing
3. **Choose the right tool**:
   - Text extraction → `zpdf text`
   - Layout preservation → `zpdf render`
   - Structured data → `zpdf tables`, `zpdf forms`
   - Format conversion → `zpdf convert`, `zpdf export-pptx`
4. **DPI sweet spots**: 150 for readable, 300 for high-detail
5. **Password via flag**: Never rely on interactive prompts (they fail in automation)
6. **Selective extraction**: Use page ranges (`-p 1`, `--pages 1,3-5`) to target specific content

## Comparison with Alternatives

| Capability           | PyPDF2 | pdfplumber | pdftk | zpdf              |
| -------------------- | ------ | ---------- | ----- | ----------------- |
| Text extraction      | Basic  | Good       | No    | **Excellent**     |
| Table extraction     | No     | Yes        | No    | **Yes (native)**  |
| Image rendering      | No     | Via Pillow | No    | **CPU + GPU**     |
| Form filling         | Manual | Manual     | Yes   | **Native**        |
| Encryption           | Basic  | Basic      | Yes   | **AES-256/RC4**   |
| CJK fonts            | Poor   | Medium     | N/A   | **Full support**  |
| Annotations          | Manual | No         | No    | **All types**     |
| Digital signatures   | No     | No         | No    | **Create/verify** |
| PDF → PowerPoint     | No     | No         | No    | **Yes**           |
| Merge/split          | Yes    | No         | Yes   | **Yes**           |
| Pure Rust (no C)     | No     | No         | No    | **Yes**           |

## Real-World Examples

### Example 1: Research Paper Analysis

```bash
# 12-page paper with algorithms and charts
zpdf info paper.pdf                          # Check structure
zpdf text paper.pdf -p 1 > intro.txt         # Extract prose sections
zpdf render paper.pdf -p 4 -o algo.png --dpi 150    # Render algorithm diagrams
zpdf render paper.pdf -p 9 -o chart.png --dpi 150   # Render charts
zpdf links paper.pdf > references.txt        # Extract citations
```

### Example 2: Contract Review & Signing

```bash
# Read, annotate, and sign contract
zpdf info contract.pdf
zpdf text contract.pdf --all > contract.txt  # Extract for review
zpdf search contract.pdf "liability" > key_sections.txt

# Add review notes
zpdf annotate contract.pdf -p 3 --kind note --at 100,500 \
  --text "Verify this clause" --icon Comment -o reviewed.pdf

# Sign after approval
zpdf sign reviewed.pdf --key key.p8.der --cert cert.der \
  --name "Jane Smith" --reason "Agreement" -o signed_contract.pdf
```

### Example 3: Form Processing Pipeline

```bash
# Batch fill forms
zpdf forms template.pdf                      # List available fields
zpdf fill template.pdf \
  --set name="John Doe" \
  --set email="john@example.com" \
  --set date="2026-07-28" \
  -o filled_john.pdf

# Merge multiple filled forms
zpdf merge filled_john.pdf filled_jane.pdf filled_bob.pdf -o all_submissions.pdf
```

### Example 4: Document Optimization & Publishing

```bash
# Optimize large PDF for web
zpdf optimize large_report.pdf -o web_report.pdf \
  --max-image-dim 1024                       # Downsample images

# Encrypt for distribution
zpdf optimize web_report.pdf -o secure_report.pdf \
  --encrypt aes256 \
  --user-password "read2026" \
  --owner-password "admin2026"

# Add watermark
zpdf stamp secure_report.pdf -p 1 --text "CONFIDENTIAL" \
  --at 300,50 --size 36 --color 128,128,128 -o final_report.pdf
```

### Example 5: PDF to PowerPoint Conversion

```bash
# Convert slide deck PDF to editable PowerPoint
zpdf export-pptx slides.pdf -o presentation.pptx --all

# Convert selected pages only
zpdf export-pptx deck.pdf -o summary.pptx --pages 1,5-10,20
```

### Example 6: Data Extraction for Analysis

```bash
# Extract tables from financial report
zpdf search financial_report.pdf "revenue" > pages.txt  # Find relevant pages
zpdf tables financial_report.pdf -p 5 --csv > q1_data.csv
zpdf tables financial_report.pdf -p 12 --csv > q2_data.csv

# Extract embedded spreadsheets (ZUGFeRD invoices)
zpdf attachments invoice.pdf --extract all --out-dir ./data/
```

### Example 7: Bulk Document Processing

```bash
# Process all PDFs in directory
for pdf in documents/*.pdf; do
  basename=$(basename "$pdf" .pdf)
  
  # Extract text
  zpdf text "$pdf" --all > "text/${basename}.txt"
  
  # Optimize
  zpdf optimize "$pdf" -o "optimized/${basename}.pdf" --max-image-dim 1500
  
  # Generate preview
  zpdf render "$pdf" -p 1 -o "previews/${basename}.png" --dpi 72
done
```

### Example 8: Redaction & Privacy

```bash
# Redact sensitive information
zpdf redact personnel.pdf -p 1 \
  --rect 100,200,300,220 \
  --rect 100,400,300,420 \
  --fill 0,0,0 \
  -o redacted.pdf

# Remove metadata
zpdf set-meta redacted.pdf \
  --title "" --author "" --subject "" --keywords "" \
  -o clean.pdf
```

## Installation

zpdf is built as part of this project (zpdf-cli crate). To use it:

```bash
# From this repository
cd D:/project/zpdf
cargo build --release -p zpdf-cli
# Binary: target/release/zpdf.exe

# Or install globally
cargo install zpdf-cli

# Verify installation
zpdf info
```

## Version

- **Skill version**: 2.0.0
- **zpdf version**: 0.11.0+ (with export-pptx, full annotation support, digital signatures)
- **Tested on**: Windows 11, Linux, macOS

## License

This skill documentation is part of the zpdf project and follows the same license (MIT).

## Documentation Links

- **[SKILL.md](SKILL.md)**: Core skill definition with decision flows and quick start
- **[references/quick-reference.md](references/quick-reference.md)**: Fast lookup card for common operations
- **[references/reading-commands.md](references/reading-commands.md)**: Reading & extraction commands
- **[references/analysis-commands.md](references/analysis-commands.md)**: Structured data & analysis commands
- **[references/editing-commands.md](references/editing-commands.md)**: Modification & optimization commands
- **[references/zpdf-commands.md](references/zpdf-commands.md)**: Complete reference with all details
