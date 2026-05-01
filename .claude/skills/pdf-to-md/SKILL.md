---
name: pdf-to-md
description: Convert a PDF (or any document) to clean markdown so Claude can read it efficiently. Tries Microsoft MarkItDown first; falls back to OpenDataLoader-PDF for complex layouts (multi-column papers, dense tables, math). Use when user says "convert this PDF", "make this document readable", "turn paper.pdf into markdown", "preprocess this document", or before reading any PDF for analysis.
argument-hint: "[input-file] [optional: output-file]"
allowed-tools: ["Bash", "Read", "Write"]
---

# /pdf-to-md — Convert any document to clean markdown

Markdown is Claude's native format. PDFs cost 2–3× more tokens and bury content under formatting artifacts. Converting first cuts token usage **50–70%** with no quality loss, and improves comprehension on multi-column or table-heavy PDFs.

## Inputs

- `$1` — input file path. Supported: `.pdf`, `.docx`, `.pptx`, `.xlsx`, `.html`, `.epub`, `.csv`, `.xml`.
- `$2` — output path (optional). If omitted, writes alongside the input with the `.md` extension.

## Steps

### 1. Validate input

Check that `$1` exists and identify the format from the extension.

```bash
test -f "$1" || { echo "File not found: $1" && exit 1; }
EXT="${1##*.}"
OUT="${2:-${1%.*}.md}"
```

### 2. Quick first pass with MarkItDown

`markitdown` handles every supported format in one call. Run it first.

```bash
markitdown "$1" > "$OUT"
```

If the input is **not** a PDF, jump to step 4 (verify). MarkItDown's output is sufficient for DOCX / PPTX / HTML / EPUB.

### 3. PDF triage — decide whether to fall back to OpenDataLoader-PDF

PDFs are the only format where layout matters. Inspect the MarkItDown output for failure signals:

- **Output is suspiciously short** for a multi-page input (e.g., < 50 words for a 5-page PDF) — extraction failed.
- **Tables collapsed to single lines** — column structure was lost.
- **Lines from different columns interleaved** — multi-column reading order broken.
- **Math/equations rendered as `□□□` or random characters** — no semantic capture.

If **any** trigger fires, rerun with OpenDataLoader-PDF (PDF specialist with XY-cut++ reading order):

```bash
opendataloader "$1" -o "$OUT" --format markdown
```

Otherwise, the MarkItDown pass is fine — proceed to step 4.

### 4. Verify

Read the first 30 lines of `$OUT`. Confirm:

- Headers preserved as `#` / `##` / `###`
- Tables visible as markdown pipe tables (where applicable)
- No raw XML, PDF metadata, or `<embed>` tags leaked through
- Body text reads as continuous prose, not fragmented column-by-column

If verification fails on a PDF that already used OpenDataLoader-PDF, escalate to the user — the PDF may be scanned-only and need OCR (Tesseract, Cloud Vision) before any markdown conversion will work.

### 5. Report

Tell the user:

- Output path (`$OUT`)
- Word count (run `wc -w "$OUT"`)
- Which tool produced the final output and **why** (note the trigger if you fell back)
- Any sections that looked suspect

## Install (one-time)

```bash
pip install markitdown
# OpenDataLoader-PDF — see https://github.com/opendataloader-project/opendataloader-pdf for install instructions
```

## When NOT to use this skill

- Source is already `.md` / `.qmd` / `.tex` — don't bother converting.
- You need bounding-box source citations (RAG with click-to-source) — call OpenDataLoader-PDF directly with its full feature set; this skill simplifies the output.
- The PDF is scanned-only with no embedded text layer — neither tool helps. Run OCR first (Tesseract, Cloud Vision, etc.).

## Why this skill exists

A repeated pattern: user drops a PDF into `master_supporting_docs/`, asks Claude to read it, and the response uses 3× the tokens of the same content as markdown — sometimes with garbled multi-column reading. This skill closes that loop. Convert once at intake; feed Claude the clean version forever.

## References

- [microsoft/markitdown](https://github.com/microsoft/markitdown) — Microsoft's multi-format converter.
- [opendataloader-project/opendataloader-pdf](https://github.com/opendataloader-project/opendataloader-pdf) — PDF specialist with structured output.
- [Claude API PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) — for the cases where you genuinely want PDF-as-input (e.g., signed contracts where layout matters).
