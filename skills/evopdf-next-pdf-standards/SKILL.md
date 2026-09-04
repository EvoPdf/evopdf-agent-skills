---
name: evopdf-next-pdf-standards
description: "Generate PDF/UA (accessibility) and PDF/A (archival) compliant documents with EvoPdf Next: PdfStandard, AccessibilityOptions, tagged PDF. Use when a .NET PDF must be accessible, archivable or both."
---

# EvoPdf Next — PDF/UA and PDF/A

```csharp
var o = converter.PdfDocumentOptions;
o.PdfStandard = PdfStandard.PdfUa1;          // accessible, tagged (ISO 14289-1)
// o.PdfStandard = PdfStandard.PdfA2b;       // archival, level B
// o.PdfStandard = PdfStandard.PdfUa1PdfA2b; // both at once
o.AccessibilityOptions.AddMissingImageAlternateText = true;   // PdfAccessibilityOptions; applies when a tagged standard is selected
o.AccessibilityOptions.InsertMissingTableHeaders = true;
converter.PdfDocumentInfo.Language = "en-US";                 // document language lives in PdfDocumentInfo
converter.PdfDocumentInfo.Title = "Invoice 2026-001";
```
- `PdfStandard.None` (default) = plain PDF. Other PDF/A conformance levels (2a/3a/3b/3u/4/4f) are members of the same enum; the documentation lists the exact names.
- Tagged output needs semantic HTML: headings in order, `alt` on images, tables with `<th>`, a document `<title>` and `lang`. The converter maps HTML structure to PDF tags.
- Fonts are embedded as subsets automatically (required by both standards). Avoid JavaScript-dependent content in PDF/A.
- The Core PDF API (`PdfDocument`) can also create PDF/UA and PDF/A documents from text, images, graphics and attachments.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/
