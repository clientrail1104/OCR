# NeonOCR — GitHub-ready browser OCR

A single-page, client-side OCR application with a dark neumorphic interface and animated neon lighting. It supports front/back card uploads, multi-page PDFs, TIFF/HEIC conversion, DOCX/XLSX text extraction, and OCR processing with conservative field extraction.

## Supported input formats
PNG, JPG/JPEG, WEBP, TIFF/TIF, HEIC, PDF, DOCX, XLSX/XLS, CSV.

The UI rejects individual files above **500 MB**.

## Run on GitHub Pages
1. Put `index.html` and `README.md` into a repository.
2. Enable GitHub Pages for the repository's main branch and root folder.
3. Open the Pages URL.
4. Upload the front and back images of a card as separate files; PDF pages are automatically enumerated.

No server is required for the static demo. Uploaded document bytes are processed in the browser.

## Accuracy / no-fabrication design
This project deliberately does **not** claim 100% OCR accuracy. Tesseract.js is used for OCR and returns text/word data in the browser. Multiple OCR passes, preprocessing, and conservative format checks reduce common mistakes, but they cannot mathematically eliminate errors on low-quality images, handwriting, glare, watermarks, stamps, crossed-out text, or unusual scripts.

The extractor follows an evidence-first rule:
- A field is filled only when OCR text near the expected label or a strict pattern supports it.
- Otherwise the field shows `Not reliably detected` in red with a simple reason.
- It never turns an unsupported guess into a value.
- Gender handling does not convert `P` into `F` or `F` into `P`, and M/F values are only accepted from explicit tokens.
- Numeric identifiers use stricter patterns than general text. This reduces contamination from decorative/watermark/crossed-out numbers, but cannot guarantee perfect separation on every scan.
- Addresses are kept as full nearby text rather than deliberately truncated at punctuation.

## OCR languages
The default is English + Malay. Tesseract.js supports multiple trained languages, but “any language” cannot be guaranteed from a small static build because each language requires its own trained data. Add or preload more traineddata packs when your deployment needs them.

## 500 MB note
The file-size guard is 500 MB, but actual processing speed and memory depend on the browser and device. Large PDFs are rendered page-by-page. Very large images are downscaled for OCR to limit memory pressure while preserving a useful resolution.

## Production recommendation
For high-assurance identity/document verification, use this front end as the UI layer and connect it to a server-side OCR/document stack (for example PaddleOCR + OpenCV + PyMuPDF) with document-specific validation, duplicate OCR passes, MRZ checksums, barcode decoding, image-quality scoring, and human review for unresolved fields. A browser-only app cannot honestly promise zero OCR errors.

## Security
Mammoth documentation notes that DOCX input is not sanitized by the library; this project extracts text locally but you should still avoid displaying arbitrary converted HTML as trusted content. For a production deployment, isolate parsing and sanitize any generated HTML if it is ever rendered.

## Main dependencies
- Tesseract.js — OCR in the browser
- PDF.js — PDF page rendering
- Mammoth — DOCX text extraction
- SheetJS — XLS/XLSX/CSV parsing
- heic2any — HEIC conversion
- UTIF — TIFF decoding
- jsQR — optional QR decoding helper

CDN links are declared directly in `index.html` for easy GitHub Pages deployment.
