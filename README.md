# zpdf-skill

Claude Code skills for PDF processing using the zpdf CLI toolkit.

## Skills Available

### 1. zpdf-read (Minimal - Recommended for Reading)
**Use for**: Reading PDF content only

A lightweight skill with just 4 essential commands for extracting PDF content.

- **File**: `SKILL-READ.md`
- **Commands**: `info`, `text`, `render`, `search` (4 commands)
- **Size**: ~600 lines, ~70% smaller than full skill
- **Use when**: Reading, analyzing, or summarizing PDFs

[Documentation](README-READ.md)

### 2. zpdf (Complete Toolkit)
**Use for**: Full PDF operations (reading + editing + conversion)

Comprehensive skill with all 26 zpdf commands for reading, editing, and converting PDFs.

- **File**: `SKILL.md`
- **Commands**: All 26 commands across reading, analysis, and editing
- **Size**: Complete with split references for token efficiency
- **Use when**: Need to edit, merge, sign, annotate, or convert PDFs

[Documentation](README.md)

## Quick Decision Guide

**Choose zpdf-read when user asks to:**
- "Read this PDF"
- "What's in this PDF?"
- "Summarize this document"
- "Extract text from PDF"
- "Search PDF for..."

**Choose zpdf (full) when user asks to:**
- "Fill this PDF form"
- "Merge these PDFs"
- "Convert PDF to PowerPoint"
- "Add annotations to PDF"
- "Sign this PDF"
- "Optimize/compress PDF"
- "Extract tables from PDF"

## Installation

```bash
cargo install zpdf-cli
```

Or build from the zpdf repository:
```bash
cd D:/project/zpdf
cargo build --release -p zpdf-cli
```

## File Structure

```
zpdf-skill/
├── README.md                     # This file
│
├── SKILL-READ.md                 # Minimal reading skill (4 commands)
├── README-READ.md                # Reading skill documentation
│
├── SKILL.md                      # Full skill (26 commands)
├── README.md                     # Full skill documentation
│
└── references/                   # Reference files for full skill
    ├── quick-reference.md        # Fast lookup (~1.5K tokens)
    ├── reading-commands.md       # Reading commands (~1.8K tokens)
    ├── analysis-commands.md      # Analysis commands (~1.5K tokens)
    ├── editing-commands.md       # Editing commands (~2.5K tokens)
    └── zpdf-commands.md          # Complete reference (~6K tokens)
```

## Skill Comparison

| Feature | zpdf-read | zpdf (full) |
|---------|-----------|-------------|
| **Commands** | 4 | 26 |
| **Read text** | ✅ | ✅ |
| **Render images** | ✅ | ✅ |
| **Search** | ✅ | ✅ |
| **Info/metadata** | ✅ | ✅ |
| **Extract tables** | ❌ | ✅ |
| **Extract forms** | ❌ | ✅ |
| **Fill forms** | ❌ | ✅ |
| **Merge/split** | ❌ | ✅ |
| **Annotate** | ❌ | ✅ |
| **Sign** | ❌ | ✅ |
| **Optimize** | ❌ | ✅ |
| **Convert formats** | ❌ | ✅ |
| **Context size** | Small (~600 lines) | Large (~2000 lines + refs) |

## Usage Examples

### Using zpdf-read (Minimal)

```bash
# Check document
zpdf info report.pdf

# Extract text
zpdf text report.pdf --all > content.txt

# Render page with diagrams
zpdf render slides.pdf -p 5 -o diagram.png --dpi 150

# Search for keyword
zpdf search manual.pdf "installation"
```

### Using zpdf (Full)

```bash
# All reading commands from zpdf-read, plus:

# Extract tables
zpdf tables financial.pdf -p 5 --csv

# Fill form
zpdf fill application.pdf --set name="John Doe" -o filled.pdf

# Merge PDFs
zpdf merge a.pdf b.pdf -o combined.pdf

# Convert to PowerPoint
zpdf export-pptx slides.pdf -o presentation.pptx --all

# Sign document
zpdf sign contract.pdf --key key.p8.der --cert cert.der -o signed.pdf
```

## Best Practices

1. **Start with zpdf-read** for simple reading tasks
2. **Only load zpdf (full)** when editing/conversion is needed
3. **Always run `zpdf info` first** to check page count and encryption
4. **Use search before bulk extraction** on large documents
5. **Choose text vs image strategically** (see decision flows in each skill)

## Version

- **zpdf-read**: v1.0.0 (minimal reading skill)
- **zpdf**: v2.0.0 (complete toolkit)
- **zpdf CLI**: 0.11.0+

## License

MIT License (part of the zpdf project)
