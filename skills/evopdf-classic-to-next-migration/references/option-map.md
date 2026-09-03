# Option map — EvoPdf Classic → EvoPdf Next

| Classic | Next | Note |
|---|---|---|
| `using EvoPdf;` | `using EvoPdf.Next;` | one namespace for every component |
| `converter.LicenseKey` | `Licensing.LicenseKey` (static) | set once, before the first conversion |
| `HtmlViewerWidth` | `HtmlViewerWidth` | same meaning; 96 DPI in Next |
| `ConversionDelay` | `ConversionDelay` | seconds |
| `NavigationTimeout` | `NavigationTimeout` | |
| `JavaScriptEnabled` | `JavaScriptEnabled` | |
| `PdfDocumentOptions.PdfPageSize` / `PdfPageOrientation` | same | |
| `PdfDocumentOptions.FitWidth` | `PdfDocumentOptions.AutoResizePdfPageWidth` (inverse semantics) | Classic scaled content to page; Next sizes the page to content unless `false` |
| `PdfDocumentOptions.AutoSizePdfPage` | default behaviour | |
| `PdfDocumentOptions.SinglePage` | `AutoResizePdfPageHeight = true` (+ width `true`) | |
| `PdfDocumentOptions.TopSpacing` / `Y` | margins (`TopMargin`, …) or CSS | |
| `ClipHtmlView`, `StretchToFit` | — | page width follows `HtmlViewerWidth` |
| `PdfHeaderOptions` / `PdfFooterOptions` (`HeaderHeight`, `HeaderBackColor`, `AddElement`) | `PdfDocumentOptions.PdfHtmlHeader` / `PdfHtmlFooter` (`Html`, `Height`, `AutoSizeContentHeight`, …) | one HTML template each |
| `TextElement("Page &p; of &P;")` | `{page_number}` / `{total_pages}` in the template HTML | |
| `LineElement`, `HtmlToPdfElement` in header | HTML/CSS inside the template | |
| `PdfFooterOptions.PageNumberingStartIndex` | `PdfHtmlFooter.PageNumberOffset` / `TotalPagesOffset` | |
| `PrepareRenderPdfPageEvent` → `Page.ShowHeader` | `ShowInFirstPage` / `ShowInOddPages` / `ShowInEvenPages` | |
| `PdfDocumentOptions.ShowHeader` / `ShowFooter` | presence of the template + `ShowIn…` | |
| `PdfSecurityOptions`, `PdfViewerPreferences`, `PdfDocumentInfo` | same names | |
| `MediaType` | `MediaType` | both default to screen |
| `HttpRequestHeaders`, `HttpRequestCookies`, `AuthenticationOptions` | same | |
