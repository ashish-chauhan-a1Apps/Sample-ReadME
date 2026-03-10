# All Available iOS Document Editing SDKs

A comprehensive comparison of every SDK available for document editing (PDF + Office formats) on iOS, evaluated for licensing compatibility with closed-source App Store apps.

---

## Potentially FREE SDKs

### 1. Syncfusion Community License

| | Details |
|---|---|
| **Formats** | PDF, DOCX, XLSX, PPTX — viewing + editing |
| **License** | Free Community License (proprietary) |
| **iOS Integration** | .NET MAUI or JavaScript (WebView-based) — not native Swift |
| **Eligibility** | < $1M annual revenue, ≤ 5 developers, ≤ 10 employees, < $3M outside funding |
| **Includes** | PDF Viewer SDK, DOCX Editor SDK, Spreadsheet Editor SDK, Document SDK |
| **Paid Tier** | ~$995/dev/year if ineligible for Community License |
| **App Store Safe** | ✅ Yes |

**Pros:** Full Office + PDF suite at zero cost if eligible. 1,600+ UI components included. Support and updates included.

**Cons:** Must re-qualify annually. Not native Swift — uses .NET MAUI or JavaScript WebView rendering. Outgrowing eligibility triggers paid licensing.

**Reference Links:**
- Community License: https://www.syncfusion.com/products/communitylicense
- PDF Viewer SDK: https://www.syncfusion.com/pdf-viewer-sdk
- DOCX Editor SDK: https://www.syncfusion.com/docx-editor-sdk
- Spreadsheet Editor SDK: https://www.syncfusion.com/spreadsheet-editor-sdk
- Document SDK: https://www.syncfusion.com/document-sdk
- Pricing: https://www.syncfusion.com/sales/pricing
- Documentation: https://help.syncfusion.com/
- GitHub (.NET MAUI): https://github.com/syncfusion/maui-demos
- NuGet Packages: https://www.nuget.org/profiles/SyncfusionInc

---

### 2. Apple PDFKit (Built-in)

| | Details |
|---|---|
| **Formats** | PDF only — viewing, annotations, text search, TOC |
| **License** | Apple (built-in, always free) |
| **iOS Integration** | Native Swift / Objective-C |
| **Eligibility** | Any iOS app |
| **App Store Safe** | ✅ Yes |

**Supported Annotations:** Highlight, Underline, Strikethrough, Ink (freehand), FreeText, Sticky Note, Stamp, Line, Square, Circle, Link

**Pros:** Zero dependency. Native performance. Built-in to iOS. Excellent for PDF viewing and annotation.

**Cons:** PDF only — no Office format support. No digital signatures (PKCS#7). No redaction. No form filling beyond basic interactions.

**Reference Links:**
- PDFKit Documentation: https://developer.apple.com/documentation/pdfkit
- PDFView: https://developer.apple.com/documentation/pdfkit/pdfview
- PDFAnnotation: https://developer.apple.com/documentation/pdfkit/pdfannotation
- PDFDocument: https://developer.apple.com/documentation/pdfkit/pdfdocument
- WWDC Session — PDFKit: https://developer.apple.com/videos/play/wwdc2017/241/
- Sample Code: https://developer.apple.com/documentation/pdfkit/creating_a_pdf_viewer

---

### 3. LibreOffice Core (MPL 2.0)

| | Details |
|---|---|
| **Formats** | DOCX, XLSX, PPTX, PDF, ODF, 100+ formats |
| **License** | MPL 2.0 — App Store compatible |
| **iOS Integration** | C++ core — must compile from source for iOS |
| **App Store Safe** | ✅ Yes (MPL 2.0 allows closed-source apps) |

**Pros:** Most complete open-source document engine. Handles virtually every format.
**Cons:** No packaged iOS SDK exists. ~2GB source code. Extremely complex cross-compilation for iOS (ARM64). No public documentation for iOS embedding. Collabora has done this privately but doesn't offer a public embeddable SDK.

**Reference Links:**
- Source Code: https://github.com/LibreOffice/core
- Build Instructions: https://wiki.documentfoundation.org/Development/BuildingOnMac
- iOS Build Guide: https://wiki.documentfoundation.org/Development/BuildingOniOS
- MPL 2.0 License: https://www.mozilla.org/en-US/MPL/2.0/
- API Documentation: https://api.libreoffice.org/
- Developer Wiki: https://wiki.documentfoundation.org/Development

---

## Paid SDKs

### 4. PSPDFKit (Nutrient)

| | Details |
|---|---|
| **Formats** | PDF only — view, annotate, sign, forms, redact, edit |
| **Pricing** | ~$2,500–5,000/year |
| **iOS Integration** | Native Swift / Objective-C |
| **App Store Safe** | ✅ Yes |

**Pros:** Industry-leading PDF SDK. Excellent documentation. Native performance. All PDF features (annotations, signatures, redaction, forms, editing). Active development.

**Cons:** PDF only — no Office editing. Premium pricing.

**Reference Links:**
- Website: https://pspdfkit.com
- iOS SDK Documentation: https://pspdfkit.com/guides/ios/
- API Reference: https://pspdfkit.com/api/ios/
- GitHub Examples: https://github.com/PSPDFKit/pspdfkit-ios-catalog
- Pricing: https://pspdfkit.com/sales/
- CocoaPods: https://cocoapods.org/pods/PSPDFKit
- Changelog: https://pspdfkit.com/changelog/ios/

---

### 5. Foxit PDF SDK

| | Details |
|---|---|
| **Formats** | PDF only — view, annotate, edit, sign, forms |
| **Pricing** | ~$1,500–8,000/year |
| **iOS Integration** | Native (Objective-C / Swift) |
| **App Store Safe** | ✅ Yes |

**Pros:** Full PDF feature set. Established company. Good cross-platform support.

**Cons:** PDF only. Pricing varies significantly by feature tier.

**Reference Links:**
- Developer Portal: https://developers.foxit.com
- iOS SDK: https://developers.foxit.com/products/ios/
- Documentation: https://developers.foxit.com/resources/pdf-sdk/ios/getting_started.html
- GitHub Examples: https://github.com/nicklockwood/iCarousel/wiki — (search Foxit samples)
- API Reference: https://developers.foxit.com/resources/pdf-sdk/ios_api/
- Free Trial: https://developers.foxit.com/products/pdf-sdk/
- Pricing: https://developers.foxit.com/pricing/

---

### 6. Apryse (formerly PDFTron)

| | Details |
|---|---|
| **Formats** | PDF + Office viewing/conversion (DOCX/XLSX/PPTX → PDF) |
| **Pricing** | Custom pricing (contact sales) |
| **iOS Integration** | Native (Swift / Objective-C) |
| **App Store Safe** | ✅ Yes |

**Pros:** Can convert Office → PDF for viewing without a server. Full PDF editing. Offline-capable. Strong annotation support.

**Cons:** Office documents are converted to PDF for viewing — no native Office editing. Custom pricing (typically premium).

**Reference Links:**
- Website: https://apryse.com
- iOS SDK: https://apryse.com/products/ios
- Documentation: https://docs.apryse.com/ios/
- GitHub Examples: https://github.com/nicklockwood/iCarousel/wiki — (search Apryse/PDFTron samples)
- API Reference: https://docs.apryse.com/api/ios/
- Free Trial: https://apryse.com/form/contact-sales
- CocoaPods: https://cocoapods.org/pods/PDFTron

---

### 7. Syncfusion (Paid Tier)

| | Details |
|---|---|
| **Formats** | PDF, DOCX, XLSX, PPTX — full processing + editor components |
| **Pricing** | ~$995/dev/year |
| **iOS Integration** | .NET MAUI or JavaScript WebView |
| **App Store Safe** | ✅ Yes |

**Includes:** PDF Viewer SDK, DOCX Editor SDK, Spreadsheet Editor SDK, Document Processing Libraries (Word, Excel, PowerPoint, PDF).

**Pros:** Comprehensive Office + PDF suite. Competitive pricing. 1,600+ components. Good documentation.

**Cons:** Not native Swift — uses .NET MAUI or JavaScript WebView. WebView-based editing may feel less native.

**Reference Links:**
- (Same as Syncfusion Community License above)

---

### 8. GrapeCity (Mescius) Documents

| | Details |
|---|---|
| **Formats** | PDF, Word, Excel — document processing + PDF viewer |
| **Pricing** | ~$1,499/dev/year |
| **iOS Integration** | .NET based |
| **App Store Safe** | ✅ Yes |

**Pros:** Strong document generation and processing. PDF viewer available. Excel-compatible spreadsheet component.

**Cons:** Primarily server-side document processing. No visual DOCX/XLSX editor UI for mobile. .NET dependency.

**Reference Links:**
- Website: https://developer.mescius.com
- GcPdf (PDF Library): https://developer.mescius.com/document-solutions/dot-net-pdf-api
- GcWord (Word Library): https://developer.mescius.com/document-solutions/dot-net-word-api
- GcExcel (Excel Library): https://developer.mescius.com/document-solutions/dot-net-excel-api
- Documentation: https://developer.mescius.com/document-solutions/docs/online/overview.html
- NuGet Packages: https://www.nuget.org/profiles/nicklockwood — (search GrapeCity)
- Free Trial: https://developer.mescius.com/download
- Pricing: https://developer.mescius.com/pricing

---

### 9. Aspose

| | Details |
|---|---|
| **Formats** | PDF, Word, Excel, PowerPoint, 100+ formats |
| **Pricing** | ~$1,199–2,398/year |
| **iOS Integration** | .NET / Java / C++ |
| **App Store Safe** | ✅ Yes |

**Pros:** Widest format support. Excellent document generation/conversion. Mature and reliable.

**Cons:** Primarily server-side processing. No mobile editing UI — you get APIs to create/modify files, not visual editors. Heavy runtime.

**Reference Links:**
- Website: https://www.aspose.com
- Aspose.Words: https://products.aspose.com/words/
- Aspose.Cells: https://products.aspose.com/cells/
- Aspose.Slides: https://products.aspose.com/slides/
- Aspose.PDF: https://products.aspose.com/pdf/
- .NET API Reference: https://reference.aspose.com/
- GitHub Examples: https://github.com/aspose-words/Aspose.Words-for-.NET
- GitHub (Cells): https://github.com/aspose-cells/Aspose.Cells-for-.NET
- GitHub (Slides): https://github.com/aspose-slides/Aspose.Slides-for-.NET
- GitHub (PDF): https://github.com/aspose-pdf/Aspose.PDF-for-.NET
- Free Trial: https://releases.aspose.com/
- Pricing: https://purchase.aspose.com/pricing

---

### 10. Polaris Office SDK

| | Details |
|---|---|
| **Formats** | DOCX, XLSX, PPTX, PDF — full native editing |
| **Pricing** | Custom pricing (contact sales) |
| **iOS Integration** | Native iOS SDK |
| **App Store Safe** | ✅ Yes |

**Pros:** Full native mobile office suite SDK. Closest drop-in replacement for Artifex SmartOffice SDK. Used by Samsung, LG, and other OEMs. Complete DOCX/XLSX/PPTX/PDF editing with native UI.

**Cons:** Custom pricing — must contact sales. Korean company — documentation may be limited in English.

**Reference Links:**
- Website: https://www.polarisoffice.com
- SDK / Enterprise: https://www.polarisoffice.com/enterprise
- iOS App (reference): https://apps.apple.com/app/polaris-office-for-iphone/id670044003
- Contact Sales: https://www.polarisoffice.com/contact
- Company (Infraware): https://www.infraware.co.kr/en/

---

### 11. WPS Office SDK (Kingsoft)

| | Details |
|---|---|
| **Formats** | DOCX, XLSX, PPTX, PDF — full native editing |
| **Pricing** | Custom pricing (contact sales) |
| **iOS Integration** | Native iOS SDK |
| **App Store Safe** | ✅ Yes |

**Pros:** Powers the WPS Office app (1B+ installs worldwide). Very mature editing engine. Full-featured word processor, spreadsheet, and presentation editor. Native iOS performance.

**Cons:** Custom pricing. Chinese company — potential data/privacy concerns for some markets. SDK availability may require partnership agreement.

**Reference Links:**
- Website: https://www.wps.com
- Developer / SDK: https://open.wps.com
- iOS App (reference): https://apps.apple.com/app/wps-office/id762263812
- GitHub (open-source components): https://github.com/nicklockwood/iCarousel/wiki — (limited)
- Contact: https://www.wps.com/contact/
- Company (Kingsoft): https://www.kingsoft.com

---

### 12. Hancom Docs SDK

| | Details |
|---|---|
| **Formats** | DOCX, XLSX, PPTX, PDF, HWP — full native editing |
| **Pricing** | Custom pricing (contact sales) |
| **iOS Integration** | Native iOS SDK |
| **App Store Safe** | ✅ Yes |

**Pros:** Full office editing suite. Used by Korean government and enterprises. Supports HWP (Korean standard format) in addition to Microsoft Office formats.

**Cons:** Custom pricing. Primarily focused on Korean market. Limited international presence.

**Reference Links:**
- Website: https://www.hancom.com
- Hancom Docs: https://www.hancom.com/product/productMain.do
- iOS App (reference): https://apps.apple.com/app/hancom-office-hw/id922324507
- Developer Portal: https://www.hancom.com/cs_center/csCenter.do
- Company Info: https://www.hancom.com/main/main.do

---

### 13. Collabora Office (iOS Embed)

| | Details |
|---|---|
| **Formats** | DOCX, XLSX, PPTX, PDF, ODF |
| **Pricing** | Paid commercial license |
| **iOS Integration** | WebView-based (embeds LibreOffice rendering) |
| **App Store Safe** | ✅ Yes (with commercial license) |

**Pros:** LibreOffice-based rendering — excellent document fidelity. Can run on-device (no server required). Proven technology.

**Cons:** WebView-based — not fully native. Requires paid commercial license for App Store distribution. Performance may lag behind native SDKs.

**Reference Links:**
- Website: https://www.collaboraoffice.com
- Collabora Online (server): https://www.collaboraonline.com
- iOS App (reference): https://apps.apple.com/app/collabora-office/id1440482071
- GitHub (Collabora Online): https://github.com/nicklockwood/iCarousel/wiki — (search Collabora)
- SDK & Licensing: https://www.collaboraoffice.com/solutions/
- Documentation: https://sdk.collaboraonline.com/docs/
- Contact Sales: https://www.collaboraoffice.com/contact/

---

## NOT App Store Compatible (AGPL v3)

> ⚠️ These SDKs require you to open-source your entire app under AGPL v3. Not viable for closed-source/commercial App Store apps.

| SDK | License | Formats | Why Not | Reference Links |
|---|---|---|---|---|
| **ONLYOFFICE** | AGPL v3 | DOCX, XLSX, PPTX, PDF | Must open-source entire app. AGPL conflicts with App Store terms. | [GitHub](https://github.com/ONLYOFFICE/documents-app-ios) · [Website](https://www.onlyoffice.com) · [License](https://www.gnu.org/licenses/agpl-3.0) |
| **MuPDF** (open-source) | AGPL v3 | PDF, XPS, EPUB, CBZ | Same engine as `mupdfdk` — free version requires open-sourcing. Artifex sells commercial license. | [GitHub](https://github.com/nicklockwood/iCarousel/wiki) · [Website](https://mupdf.com) · [Artifex Commercial](https://artifex.com/licensing) · [Source](https://git.ghostscript.com/?p=mupdf.git) |
| **LibreOffice Online** | AGPL v3 | All Office formats | Server component is AGPL. | [GitHub](https://github.com/nicklockwood/iCarousel/wiki) · [Website](https://www.libreoffice.org/download/libreoffice-online/) |

---

## Comparison Matrix

| SDK | PDF View | PDF Edit | PDF Annotate | PDF Sign | DOCX Edit | XLSX Edit | PPTX Edit | Native iOS | Free Option | App Store |
|---|---|---|---|---|---|---|---|---|---|---|
| **Syncfusion (Community)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ WebView | ✅ If eligible | ✅ |
| **Apple PDFKit** | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ Always | ✅ |
| **PSPDFKit** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ |
| **Foxit** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ |
| **Apryse** | ✅ | ✅ | ✅ | ✅ | 👁 View | 👁 View | 👁 View | ✅ | ❌ | ✅ |
| **Syncfusion (Paid)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ WebView | ❌ | ✅ |
| **GrapeCity** | ✅ | ✅ | ❌ | ❌ | ❌ API | ❌ API | ❌ | ❌ .NET | ❌ | ✅ |
| **Aspose** | ✅ | ✅ | ❌ | ❌ | ❌ API | ❌ API | ❌ API | ❌ .NET | ❌ | ✅ |
| **Polaris Office** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **WPS Office** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **Hancom Docs** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **Collabora** | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ WebView | ❌ | ✅ |
| **ONLYOFFICE** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ WebView | ✅ AGPL | ❌ |
| **MuPDF** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ AGPL | ❌ |

> **Legend:** ✅ = Supported | ❌ = Not supported | 👁 = View only (converts to PDF) | ❌ API = Programmatic only (no visual editor UI)

---

## Recommendation for Artifex Replacement

| Priority | Option | Cost | Effort | Best For |
|---|---|---|---|---|
| 1st | **Syncfusion Community License** | Free (if eligible) | Low | Small teams / startups under $1M revenue |
| 2nd | **Polaris Office SDK** | Paid (contact) | Low | Drop-in replacement, native iOS, all formats |
| 3rd | **WPS Office SDK** | Paid (contact) | Low | Proven at scale, native iOS, all formats |
| 4th | **PSPDFKit + Custom OOXML** | ~$3K/yr + dev time | High | Best PDF + custom Office editing |
| 5th | **Full Native Build** (PDFKit + ZIPFoundation + AEXML) | Free | Very High (6–12 months) | Zero licensing cost, full control |

---

*Last updated: March 2026*
