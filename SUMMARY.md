# zpdf-skill: Summary

## Overview

Two Claude Code skills for PDF processing with zpdf CLI:

1. **zpdf-read** (Minimal) - Reading only, ~70% context reduction
2. **zpdf** (Complete) - Full PDF toolkit with 26 commands

## Files Created

### Minimal Reading Skill
- `SKILL-READ.md` (3.3K, ~90 lines) - Minimal skill with 4 commands
- `README-READ.md` (5.5K, ~200 lines) - Reading-only documentation

### Complete Toolkit Skill
- `SKILL.md` (4.0K, ~120 lines) - Full skill with 26 commands
- `README-FULL.md` (9.7K, ~350 lines) - Complete documentation
- `README.md` (4.1K, ~150 lines) - Overview/index for both skills

### Reference Files (for full skill)
- `references/quick-reference.md` (163 lines) - Fast lookup
- `references/reading-commands.md` (154 lines) - Reading commands
- `references/analysis-commands.md` (201 lines) - Analysis commands
- `references/editing-commands.md` (305 lines) - Editing commands
- `references/zpdf-commands.md` (845 lines) - Complete reference

**Total**: 1,668 lines across 5 reference files

## Command Coverage

### zpdf-read (Minimal)
4 commands: `info`, `text`, `render`, `search`

### zpdf (Complete)
26 commands across 4 categories:

**Reading (6)**: info, text, render, search, convert, export-pptx
**Analysis (9)**: tables, forms, outline, links, struct, signatures, attachments, validate, compare
**Editing (9)**: fill, merge, split, optimize, annotate, redact, sign, pages, set-meta, stamp
**Debug (2)**: dump, debug-stream

## Context Overhead Comparison

| Skill | Commands | Core File | References | Total Size |
|-------|----------|-----------|------------|------------|
| zpdf-read | 4 | ~90 lines | None | ~90 lines |
| zpdf | 26 | ~120 lines | 5 files (1,668 lines) | ~1,788 lines |

**Context reduction**: ~70% using zpdf-read for reading-only tasks

## Decision Guide

**Use zpdf-read when:**
- Reading, analyzing, or summarizing PDFs
- Extracting text or rendering images
- No editing needed
- Want minimal context overhead

**Use zpdf (full) when:**
- Filling forms, merging, splitting PDFs
- Adding annotations or signatures
- Converting to other formats (PowerPoint, HTML)
- Extracting structured data (tables, forms)
- Optimizing or encrypting PDFs

## Token Estimates

**zpdf-read**:
- Skill file: ~250 tokens
- Total context: ~250 tokens

**zpdf (full)**:
- Skill file: ~350 tokens
- Quick reference: ~1,500 tokens
- Reading commands: ~1,800 tokens
- Analysis commands: ~1,500 tokens
- Editing commands: ~2,500 tokens
- Complete reference: ~6,000 tokens
- Total (with all refs): ~8,650 tokens

**Strategy**: Load only needed references for specific tasks.

## Version History

### v2.0.0 (zpdf full skill)
- Comprehensive update with all 26 commands
- Removed efficiency claims
- Split references for token optimization

### v1.0.0 (zpdf-read minimal skill)
- Created minimal reading-only skill
- 4 essential commands
- 70% context reduction

## Installation

```bash
cargo install zpdf-cli
```

Or from zpdf repository:
```bash
cd D:/project/zpdf
cargo build --release -p zpdf-cli
```

## Key Design Principles

1. **Minimal by default**: Use zpdf-read for simple reading tasks
2. **Modular references**: Load only needed reference files
3. **Clear decision flows**: Guide for text vs image extraction
4. **Token efficiency**: Split references save context
5. **Complete when needed**: Full toolkit available for complex tasks

## Git History

```
d27aa92 docs: reorganize README structure for dual-skill setup
ed5e050 feat: add minimal zpdf-read skill for reading-only tasks
0c1e7a1 refactor: split references into focused files to reduce token overhead
9459af2 feat: comprehensive update to zpdf skill with all CLI features
b4fb51a init:init the zpdf skill.
```

## License

MIT License (part of the zpdf project)
