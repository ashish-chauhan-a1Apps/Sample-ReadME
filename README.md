# Plan: Native iOS Replacement for Artifex SDKs

## TL;DR

Replace `mupdfdk.xcframework` with Apple's **PDFKit** and `sodk.xcframework` with a **custom OOXML engine** built on `ZIPFoundation` (MIT) + `AEXML` (MIT) for DOCX/XLSX/PPTX parsing/writing, `CoreXLSX` (MIT) for spreadsheet reading, and custom UIKit-based editor views for each format. All dependencies are MIT/Apache-2.0 licensed and App Store safe. This is a **6–12+ month effort** for a small team — the Office document editing portion alone is comparable to building a word processor, spreadsheet, and presentation app from scratch.

---

## Open-Source Dependencies (All App Store Compatible)

| Library | License | Purpose |
|---|---|---|
| **ZIPFoundation** | MIT | Read/write ZIP archives (OOXML files are ZIP) |
| **AEXML** | MIT | Parse/generate XML inside OOXML packages |
| **CoreXLSX** | MIT | Read XLSX workbooks (uses ZIPFoundation + AEXML internally) |
| **PDFKit** | Apple (built-in) | PDF viewing, annotations, text search, TOC |
| **Security.framework** | Apple (built-in) | PKCS#7 digital signatures for PDF |
| **CryptoKit** | Apple (built-in) | Hashing for signatures |
| **QLPreviewController** | Apple (built-in) | Fallback preview for unsupported formats |

---

## Steps

### Phase 1 — PDF Editor (Replace `mupdfdk`) — ~3–4 weeks

1. **Create `PDFEditorViewController`** replacing `DefaultUIViewController.swift`. Build around `PDFView` with full page navigation, zoom, scroll, text search (`PDFDocument.findString()`), and table of contents (`PDFDocument.outlineRoot`).

2. **Implement annotation toolbar** replacing `AnnotationRibbonViewController.swift` and `AnnotationToolViewController.swift`. Use `PDFAnnotation` subclasses:
   - **Natively supported**: Highlight (`.highlight`), Underline (`.underline`), Strikethrough (`.strikeOut`), Ink (`.ink`), FreeText (`.freeText`), Note/StickyNote (`.text`), Stamp (`.stamp`), Line (`.line`), Square (`.square`), Circle (`.circle`), Link (`.link`)
   - **Custom implementation needed**: Squiggle (render as ink with wave pattern), Polygon/Polyline (custom `PDFAnnotation` with CGPath), File Attachment (`.widget` with embedded data)

3. **Implement annotation style controls** — stroke color, fill color, opacity, line thickness — using `PDFAnnotation.color`, `PDFAnnotation.border`, and custom `PDFAnnotation.setValue(_:forAnnotationKey:)` calls.

4. **Implement e-signature** replacing `SigningRibbonViewController.swift`. Use a `PKCanvasView` (PencilKit) or custom drawing view to capture signature → convert to image → place as stamp annotation on the PDF.

5. **Implement digital signatures (PKCS#7)** — this is the most complex PDF feature. Use `Security.framework` to:
   - Parse PKCS#12 certificate files (`.p12`/`.pfx`)
   - Create a signature dictionary in the PDF's cross-reference table
   - Compute the byte range hash and sign with `SecKeyCreateSignature`
   - Embed the PKCS#7 signature blob into the PDF
   - This requires direct PDF byte manipulation since PDFKit doesn't support form-based digital signatures natively.

6. **Implement redactions** replacing `RedactionRibbonViewController.swift`. Two modes:
   - **Text-based redaction**: Use `PDFSelection` to find text → overlay black `PDFAnnotation(.square)` with fill
   - **Area-based redaction**: Let user draw rectangle → add black square annotation
   - **Apply/Finalize**: Re-render each page containing redactions into a new `PDFPage` via `CGContext` (flattening annotations and removing underlying content), rebuild `PDFDocument` from rendered pages.

7. **Implement print** using `UIPrintInteractionController` with `PDFDocument.dataRepresentation()` — straightforward replacement for `ARDKPrintPageRenderer`.

8. **Remove entire `mupdf-default-ui/` folder** (~25 files) and replace with the new PDFKit-based UI.

---

### Phase 2 — OOXML Engine Core (Foundation for DOCX/XLSX/PPTX) — ~4–6 weeks

9. **Create `OOXMLPackage` class** — a generic OOXML package reader/writer that:
   - Opens `.docx`/`.xlsx`/`.pptx` files using ZIPFoundation
   - Parses `[Content_Types].xml` and `_rels/.rels` to discover parts
   - Reads/writes individual XML parts using AEXML
   - Handles shared resources: images (`word/media/`), fonts, themes (`word/theme/`), styles (`word/styles.xml`)
   - Saves back to ZIP preserving all unmodified parts (round-trip fidelity)

10. **Create `OOXMLStyles` class** — parses `styles.xml` to resolve named styles (headings, normal, etc.) into concrete font/size/color/spacing values. Shared across DOCX/PPTX.

11. **Create `OOXMLTheme` class** — parses `theme1.xml` to resolve theme colors (accent1-6, dk1, lt1, etc.) and theme fonts. Required for accurate color rendering.

12. **Create `OOXMLRelationships` class** — handles `.rels` files for resolving hyperlinks, images, embedded objects.

---

### Phase 3 — DOCX Editor (Replace `sodk` for Word) — ~8–12 weeks

13. **Create `DOCXDocument` model** — parses `word/document.xml` into a Swift model:
    - `DOCXBody` → array of `DOCXParagraph`
    - `DOCXParagraph` → paragraph properties (`pPr`: alignment, indentation, spacing, list style, numbering) + array of `DOCXRun`
    - `DOCXRun` → run properties (`rPr`: font, size, bold, italic, underline, strikethrough, color, highlight) + content (text, image, break, tab)
    - `DOCXTable` → rows → cells → paragraphs (nested)
    - `DOCXDrawing` → inline/anchor images with size/position

14. **Create `DOCXNumbering` class** — parses `word/numbering.xml` to resolve bullet/numbered list definitions (abstract numbering → concrete numbering instances → level formats).

15. **Create `DOCXRenderer`** — converts `DOCXDocument` model into an `NSAttributedString` for display, mapping:
    - `w:b` → `.bold` trait
    - `w:i` → `.italic` trait
    - `w:u` → `.underlineStyle`
    - `w:strike` → `.strikethroughStyle`
    - `w:sz` → `.font` size (half-points → points)
    - `w:color` → `.foregroundColor`
    - `w:highlight` → `.backgroundColor`
    - `w:jc` → `.paragraphStyle.alignment`
    - `w:numPr` → list prefixes (•, 1., 2., etc.) via `NSTextList` or manual prefixing
    - `w:ind` → `.paragraphStyle.headIndent` / `.firstLineHeadIndent`
    - Images → `NSTextAttachment`

16. **Create `DOCXEditorViewController`** replacing `DocUIViewController.swift`. Build on `UITextView` with `NSTextStorage`:
    - Display the rendered `NSAttributedString`
    - Intercept text changes via `NSTextStorageDelegate` to update the `DOCXDocument` model in real-time
    - Map cursor position / selection range back to the correct `DOCXParagraph` and `DOCXRun` in the model

17. **Create `DOCXEditToolbar`** replacing `DocEditRibbonViewController.swift`: font picker, size picker, B/I/U/S toggles, text color, highlight color, alignment (L/C/R/J), bullet list, numbered list, indent left/right. Each action modifies both the `NSAttributedString` in the text view and the corresponding `DOCXRun.rPr` or `DOCXParagraph.pPr` in the model.

18. **Create `DOCXImageInserter`** replacing `DocDrawInsertRibbonViewController.swift`: use `PHPickerViewController` / `UIImagePickerController` to select image → insert into `DOCXDocument` model as a `DOCXDrawing` inline element → add image to `word/media/` → update relationships → insert `NSTextAttachment` at cursor position.

19. **Create `DOCXTrackChanges`** replacing `DocReviewRibbonViewController.swift`:
    - Parse `w:ins`, `w:del`, `w:rPr/w:rPrChange` elements from the XML
    - Render insertions in green underline, deletions in red strikethrough
    - Accept = remove the `w:ins`/`w:del` wrapper, keeping/removing content
    - Reject = reverse
    - Toggle tracking = wrap new edits in `w:ins`/`w:del` elements with author + timestamp
    - **This is one of the most complex features** — accurate track changes requires careful XML manipulation.

20. **Create `DOCXWriter`** — serializes the `DOCXDocument` model back to XML and writes to ZIP:
    - Serialize each paragraph/run/table back to OOXML XML using AEXML
    - Preserve all XML elements the parser didn't explicitly handle (round-trip unknown elements)
    - Write modified `word/document.xml` back into the ZIP package
    - Copy over all unmodified parts (styles, theme, fonts, etc.)

21. **Create ink drawing overlay** — use `PKCanvasView` (PencilKit) as an overlay on the text view for freehand drawing. Serialize strokes as inline images when saving.

---

### Phase 4 — XLSX Editor (Replace `sodk` for Excel) — ~8–12 weeks

22. **Leverage `CoreXLSX`** (MIT) for reading — it already parses worksheets, shared strings, styles, cell values, formulas, and merge cells. Build model on top of it.

23. **Create `XLSXDocument` model** extending CoreXLSX:
    - `XLSXWorkbook` → array of `XLSXSheet`
    - `XLSXSheet` → 2D grid of `XLSXCell`
    - `XLSXCell` → value (string/number/date/bool/formula), style (font, fill, border, number format, alignment)
    - Freeze pane info, column widths, row heights, merge ranges

24. **Create `SpreadsheetGridView`** — a custom `UICollectionView` with `UICollectionViewCompositionalLayout` or a fully custom `UIScrollView`-based grid:
    - Fixed row/column headers (freeze panes support)
    - Cell selection (single cell, range, row, column)
    - Cell editing via `UITextField` overlay or inline editing
    - Cell formatting display (font, color, background, borders, alignment, number format)
    - Scrollable with potentially thousands of rows/columns (virtualized rendering)

25. **Create `XLSXEditToolbar`** replacing `XlsEditRibbonViewController.swift`: font, size, B/I/U/S, text color, fill color, cut/copy/paste/delete.

26. **Create `XLSXCellsToolbar`** replacing `XlsCellsRibbonViewController.swift`: insert/delete rows/columns, merge/unmerge, freeze panes, cell alignment (9-position), number format picker (General, Number, Currency, Percentage, Date, Fraction, Accounting, Custom).

27. **Create `FormulaEngine`** — a formula parser and evaluator. This is the hardest part of the spreadsheet:
    - Parse formula strings (tokenizer → AST)
    - Evaluate standard functions: SUM, AVERAGE, COUNT, MIN, MAX, IF, VLOOKUP, CONCATENATE, DATE, NOW, etc.
    - Handle cell references (A1, $A$1), ranges (A1:B10), cross-sheet references
    - Implement dependency graph for recalculation order
    - Consider using an existing MIT-licensed formula parser if available, or build from core math/string/date function sets

28. **Create `XLSXFormulaBar`** replacing `XlsFormulasRibbonViewController.swift`: formula input field, SUM quick button, formula category picker (Date & Time, Financial, Logical, Lookup, Math, Statistics, Text, Engineering, Information).

29. **Create `XLSXSheetTabBar`** replacing `XlsSheetSelector`: horizontal tab bar at bottom showing sheet names, add/rename/delete sheets.

30. **Create `XLSXWriter`** — serialize the model back to XLSX:
    - Write `xl/worksheets/sheet*.xml` with cell data, formulas, merge cells
    - Write `xl/sharedStrings.xml` for string deduplication
    - Write `xl/styles.xml` for cell formatting
    - Preserve all unmodified parts via round-trip ZIP handling

---

### Phase 5 — PPTX Editor (Replace `sodk` for PowerPoint) — ~6–8 weeks

31. **Create `PPTXDocument` model** — parse PPTX OOXML:
    - `PPTXPresentation` → array of `PPTXSlide`
    - `PPTXSlide` → array of `PPTXShape` (positioned on a fixed-size canvas)
    - `PPTXShape` → type (textbox, autoshape, image, group) + geometry + text content (paragraphs/runs like DOCX) + position/size in EMU (English Metric Units)
    - Parse `ppt/slides/slide*.xml`, `ppt/slideLayouts/`, `ppt/slideMasters/`

32. **Create `SlideCanvasView`** — a custom `UIView`-based canvas:
    - Render shapes as positioned/sized subviews on a scaled canvas
    - Text shapes use `UITextView` for inline editing
    - Image shapes use `UIImageView`
    - Autoshapes rendered via `CAShapeLayer` with predefined `CGPath` geometries for: Diamond, Ellipse, Triangle, RtTriangle, Arrow, LeftRightArrow, Pentagon, Star, Rect, RoundRect, Callout shapes
    - Selection handles (8 resize points + rotation handle)
    - Drag to move, handles to resize

33. **Create `PPTXEditToolbar`** replacing `PptEditRibbonViewController.swift`: font, size, B/I/U/S, text color, background color, alignment for selected text shape.

34. **Create `PPTXDrawInsertToolbar`** replacing `PptDrawInsertRibbonViewController.swift`: freehand drawing (PencilKit overlay), shape picker (15 shapes), image insert, arrange (z-order), stroke/fill color pickers.

35. **Create `PPTXSlideSorter`** — thumbnail strip at left/bottom showing all slides, reorder via drag-and-drop, add/delete slides.

36. **Create `PPTXWriter`** — serialize back to PPTX:
    - Write `ppt/slides/slide*.xml` with shape trees
    - Handle relationships for images
    - Preserve slide layouts, masters, themes via round-trip

37. **Implement slideshow mode** — present slides full-screen using a custom `UIPageViewController` or swipe-based `UICollectionView`, rendering each slide's canvas view in presentation mode.

---

### Phase 6 — Integration Layer — ~2–3 weeks

38. **Refactor `ARDKEditorManager.swift`** into a new `DocumentEditorManager` that:
    - Detects file type by extension
    - Routes PDF → `PDFEditorViewController`
    - Routes DOC/DOCX → `DOCXEditorViewController`
    - Routes XLS/XLSX → `XLSXEditorViewController`
    - Routes PPT/PPTX → `PPTXEditorViewController`
    - Routes other formats (EPUB, XPS, CBZ, FB2, SVG) → `QLPreviewController` (read-only)
    - Exposes unified API: `openDocument(url:)`, `saveDocument()`, `exportAsPDF()`, `printDocument()`, `shareDocument()`

39. **Implement export-to-PDF** for Office documents:
    - DOCX: Render `NSAttributedString` to PDF via `UIGraphicsPDFRenderer`
    - XLSX: Render grid to PDF page-by-page via `UIGraphicsPDFRenderer`
    - PPTX: Render each slide canvas to PDF pages via `UIGraphicsPDFRenderer`

40. **Update all consuming files** to use the new `DocumentEditorManager`:
    - `WindowManager.swift`
    - `DocumentsViewController.swift`
    - `DocumentPreviewViewController.swift`
    - `TemplatesViewController.swift`
    - `TemplatePreviewViewController.swift`
    - `IpadTemplatesViewController.swift`
    - `AIHomeViewModel.swift`

41. **Migrate `MarkdownDocInserter`** — rewrite `MarkdownDocInserter.swift` to insert formatted content into the `DOCXDocument` model instead of using SODKDoc APIs.

42. **Remove all Artifex SDK artifacts**:
    - Delete `mupdfdk.xcframework` and `sodk.xcframework` from the project
    - Delete `mupdf-default-ui/` folder (~25 files)
    - Delete `sodk-default-ui/` folder (~50 files)
    - Clean `project.pbxproj` of all framework references
    - Remove `SODKLib` initialization from `Utility.swift`

---

## Verification

- **PDF**: Open/annotate/sign/redact/save PDFs → verify with Adobe Acrobat
- **DOCX**: Open a formatted DOCX → edit text/formatting → save → reopen in Microsoft Word to verify round-trip fidelity
- **XLSX**: Open spreadsheet with formulas → edit cells → verify formula recalculation → save → reopen in Excel
- **PPTX**: Open presentation → edit text/shapes → run slideshow → save → reopen in PowerPoint
- **Export**: Export each format to PDF → verify rendering
- **Edge cases**: Large files (1000+ pages/rows), complex formatting (nested tables, merged cells), CJK text, RTL text

---

## Decisions

- **UITextView over TextKit 2 custom layout**: UITextView provides built-in editing, selection, keyboard handling, and accessibility. A custom TextKit 2 layout manager would give more control (pagination, columns) but adds months of work. Start with UITextView, migrate later if needed.
- **CoreXLSX over custom parser for XLSX**: CoreXLSX (MIT) already handles the complex XLSX reading. Only the writer and grid UI need to be built from scratch.
- **PencilKit for drawing overlays**: Built-in, handles Apple Pencil pressure sensitivity, exports to image easily. Better than a custom drawing engine.
- **Round-trip XML preservation**: The OOXML spec has ~6,000 pages. The parser should store unknown/unhandled XML elements as-is and write them back unchanged, to avoid destroying formatting the app doesn't understand.
- **Legacy DOC/XLS/PPT formats**: These are binary formats (not XML). Recommendation: support read-only via `QLPreviewController` and offer "Convert to DOCX" on open. Building a binary Office format parser is impractical.
- **EPUB/XPS/CBZ/FB2/SVG**: Use `QLPreviewController` or `WKWebView` for read-only viewing. These are niche formats in the current app.

---

## Effort Estimate

| Phase | Effort | Priority |
|---|---|---|
| Phase 1 — PDF Editor | 3–4 weeks | P0 — highest value, lowest risk |
| Phase 2 — OOXML Engine | 4–6 weeks | P0 — foundation for all Office editing |
| Phase 3 — DOCX Editor | 8–12 weeks | P1 — core product feature |
| Phase 4 — XLSX Editor | 8–12 weeks | P1 — formula engine is the biggest risk |
| Phase 5 — PPTX Editor | 6–8 weeks | P2 — can ship later |
| Phase 6 — Integration | 2–3 weeks | P0 — ties everything together |
| **Total** | **~6–12 months** | For a team of 2–3 iOS devs |

---

## Risk Callouts

- **OOXML round-trip fidelity** is the #1 risk. Microsoft Word uses thousands of XML features. Your parser will handle a subset — documents with unhandled features may lose formatting on save. Mitigate by preserving unknown XML verbatim.
- **Formula engine completeness** — Excel has 400+ functions. Start with the ~30 most common (SUM, AVERAGE, IF, VLOOKUP, COUNT, etc.) and expand over time.
- **Track changes** is extremely complex in OOXML. Consider deferring or simplifying to basic insert/delete tracking only.
