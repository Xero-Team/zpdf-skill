# Analysis Commands Reference

Commands for extracting structured data and analyzing PDF content.

## tables — Extract Tables
```bash
zpdf tables <file.pdf> [-p <page>] [--all] [--csv] [--password <pw>]
```

**Flags**:
- `-p <N>` — Extract tables from single page
- `--all` — Extract from all pages
- `--csv` — Output in CSV format
- `--password <pw>` — Decrypt with password

**Examples**:
```bash
zpdf tables financial.pdf -p 5
zpdf tables financial.pdf -p 5 --csv > data.csv
zpdf tables report.pdf --all --csv > all_tables.csv
```

**Output**: Structured table data (text or CSV format).

---

## forms — Extract Form Fields
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

**Use when**: Reading filled forms or understanding form structure before filling.

---

## outline — Document Outline
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

## links — Extract Hyperlinks
```bash
zpdf links <file.pdf> [--password <pw>]
```

**Output**: All hyperlinks in the document.

**Example**:
```bash
$ zpdf links doc.pdf
Page 3: https://example.com/documentation
Page 3: Page 15 (internal link)
Page 5: mailto:support@example.com
```

**Use when**: Extracting references, checking external resources.

---

## struct — PDF Structure Tree
```bash
zpdf struct <file.pdf> [--password <pw>]
```

**Output**: Accessibility structure (tagged PDF markup).

**Use when**: Understanding document semantics, accessibility analysis.

---

## signatures — Verify Digital Signatures
```bash
zpdf signatures <file.pdf> [--trust <roots.pem>] [--password <pw>]
```

**Flags**:
- `--trust <path>` — PEM file with trusted root certificates
- `--password <pw>` — Decrypt with password

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

## attachments — Embedded Files
```bash
zpdf attachments <file.pdf> [--extract <index|name|all>] [--out-dir <dir>] [--password <pw>]
```

**Flags**:
- `--extract <spec>` — Extract by index, name, or all
- `--out-dir <dir>` — Output directory (default: current dir)
- `--password <pw>` — Decrypt with password

**Examples**:
```bash
zpdf attachments invoice.pdf
zpdf attachments invoice.pdf --extract all --out-dir ./extracted/
```

**Use when**: Finding or extracting embedded documents (PDF portfolios, ZUGFeRD invoices).

---

## validate — PDF/A, PDF/UA & PDF/X Validation
```bash
zpdf validate <file.pdf> [--profile pdfa-1b|pdfa-2b|pdfa-3b|pdfua-1|pdfua-2|pdfx-1a|pdfx-3|pdfx-4|pdfx-6] [--password <pw>]
```

**Flags**:
- `--profile <spec>` — Conformance profile to validate against:
  - `pdfa-1b` / `pdfa-2b` — PDF/A archival conformance (encryption, `/ID`, header version, XMP `pdfaid`, `GTS_PDFA1` output intent + ICC, font embedding, forbidden features incl. JS/Launch actions, embedded files, transparency, forbidden annotation subtypes)
  - `pdfa-3b` — PDF/A-3 archival conformance (all PDF/A-2b checks, plus the `/AF` associated-files rules: embedded files in `/Names /EmbeddedFiles` must be referenced from a catalog/page `/AF` array, and each `/AF` file spec must carry `/AFRelationship`; permits embedded files/transparency/`FileAttachment` unlike A-1b)
  - `pdfua-1` — PDF/UA-1 accessibility conformance (tagged, structure tree, `/Lang`, figure alt-text, headings, role mapping, table structure, annotation `/OBJR` coverage, `/StructParents`)
  - `pdfua-2` — PDF/UA-2 accessibility conformance (all PDF/UA-1 checks plus a PDF 2.0 header requirement)
  - `pdfx-1a` / `pdfx-3` — PDF/X-1a / X-3 (ISO 15930, PDF 1.3): `/GTS_PDFX` output intent with an embedded **CMYK** (`/N=4`) ICC profile, all fonts embedded, every page has `/TrimBox` or `/ArtBox` with `MediaBox ⊇ BleedBox ⊇ TrimBox ⊇ ArtBox` containment, no encryption/JS/Launch/transparency, forbidden annotation subtypes (3D/Sound/Movie/FileAttachment) + Print flag, `/Trapped` required. X-1a additionally forbids RGB of any kind (DeviceRGB/CalRGB/CalGray/ICCBased N=3); X-3 allows calibrated colour
  - `pdfx-4` — PDF/X-4 (ISO 15930-4, PDF 1.4–1.7): as X-3 but permits transparency/layers and forbids only uncalibrated DeviceRGB
  - `pdfx-6` — PDF/X-6 (ISO 15930-7, PDF 2.0): as X-4 on a PDF 2.0 header
- `--password <pw>` — Decrypt with password

**Output**: Validation result with any violations and the XMP-claimed version (exits with code 3 on FAIL).

**Examples**:
```bash
# PDF/A-1b archival check
zpdf validate archive.pdf --profile pdfa-1b

# PDF/A-3b check (e.g. a ZUGFeRD invoice with an embedded XML /AF)
zpdf validate invoice.pdf --profile pdfa-3b

# PDF/UA-1 accessibility check (after `zpdf tag`)
zpdf validate doc.pdf --profile pdfua-1

# PDF/X-4 print-production check (before sending to a print shop)
zpdf validate brochure.pdf --profile pdfx-4
```

**Use when**: Checking PDF/A compliance for archival, PDF/UA for accessibility, or PDF/X for print production (e.g. before sending a file to a print shop).

---

## compare — Visual Diff
```bash
zpdf compare <a.png> <b.png> [--out <diff.png>] [--threshold <0-255>]
```

**Flags**:
- `--out <path>` — Output difference image
- `--threshold <N>` — Color difference threshold (default: 10)

**Output**: Visual differences between two rendered pages.

**Use when**: Tracking document changes, regression testing.

---

## Common Patterns

### Extract All Structured Data
```bash
zpdf outline doc.pdf > structure.txt
zpdf links doc.pdf > links.txt
zpdf forms doc.pdf > fields.txt
zpdf attachments doc.pdf --extract all --out-dir ./files/
```

### Table Extraction Pipeline
```bash
# Find pages with tables
zpdf search report.pdf "Table" | grep "Page" | awk '{print $2}' | sort -u > pages.txt

# Extract tables from those pages
for p in $(cat pages.txt); do
  echo "=== Page $p ===" >> tables.csv
  zpdf tables report.pdf -p $p --csv >> tables.csv
done
```

### Verify Signed Documents
```bash
zpdf signatures contract.pdf --trust trusted_roots.pem > verification.txt
if grep -q "Valid: Yes" verification.txt; then
  echo "Signature verified"
else
  echo "Signature invalid"
fi
```
