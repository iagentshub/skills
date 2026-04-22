---
id: EDIT_PDF
name: PDF Editing with nano-pdf
description: Edit PDFs with natural language instructions using the nano-pdf CLI. Allows changing text, titles, and content on specific pages.
icon: 📄
category: productivity
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Skill: PDF Editing with nano-pdf

`nano-pdf` applies edits to specific pages of a PDF using natural language instructions.

## Requirements

- `nano-pdf` installed (`pip install nano-pdf` or `uv tool install nano-pdf`)

## Installation

```bash
uv tool install nano-pdf
# or
pip install nano-pdf
```

## Basic usage

```bash
nano-pdf edit document.pdf 1 "Change the title to 'Q3 Results' and fix the subtitle typo"
```

## Examples

```bash
# Change the cover title (page 1)
nano-pdf edit presentation.pdf 1 "Replace the title with 'Annual Report 2026'"

# Update a date on the cover
nano-pdf edit report.pdf 1 "Change the date to 'April 2026'"

# Fix text on a specific page
nano-pdf edit contract.pdf 3 "Correct 'Article 2.1' to 'Article 2.2' in the third paragraph"

# Add information to a page
nano-pdf edit brochure.pdf 2 "Add 'Revised version' below the subtitle"
```

## Notes

- Page numbers may be 0-based or 1-based depending on the version; if the result looks off by one page, try the other.
- Always verify the resulting PDF before distributing.
- The tool uses AI to interpret the instruction; be specific for best results.
- For complex or multi-page edits, prefer editing the source document (Word, InDesign, etc.) and regenerating the PDF.
