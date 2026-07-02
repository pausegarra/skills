# xpdf

`xpdf` is a skill for PDF creation, extraction, rendering, and visual verification.

## What It Does

- Applies when PDF layout, rendering, or final visual quality matters.
- Uses Poppler tools such as `pdftoppm` and `pdfinfo` for page rendering and inspection.
- Uses Python packages such as `reportlab`, `pdfplumber`, and `pypdf` for generation and extraction.
- Requires visual checks from rendered PNGs before delivering PDF artifacts.

## Files

- `SKILL.md`: Full operational workflow, dependencies, rendering commands, and quality rules.
- `scripts/render_pdf.py`: Renders PDF pages to PNG files.
- `scripts/pdf_summary.py`: Prints metadata, page dimensions, and text statistics.
