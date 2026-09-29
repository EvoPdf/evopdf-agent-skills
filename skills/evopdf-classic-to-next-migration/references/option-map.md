# Option map: EvoPdf Classic to EvoPdf Next

| Classic | Next | Note |
|---|---|---|
| `using EvoPdf;` | `using EvoPdf.Next;` | one namespace for every component |
| `converter.LicenseKey` | `Licensing.LicenseKey` (static) | set once, before the first conversion |
| `HtmlViewerWidth` | `HtmlViewerWidth` | same meaning; 96 DPI in Next |
| `ConversionDelay` | `ConversionDelay` | seconds |
| `NavigationTimeout` | `NavigationTimeout` | |
| `JavaScriptEnabled` | `JavaScriptEnabled` | |
| `PdfDocumentOptions.PdfPageSize` / `PdfPageOrientation` | same | |
| `PdfDocumentOptions.FitWidth = true` (default) | `FitBrowserWindowToPage(PdfPageSize.A4)` (default) | Classic laid the page out at 1024 pixels and shrank it to the page width; a new Next converter does the same |
| `FitWidth = false`, `AutoSizePdfPage = true` | `PageWidthFromBrowserWindow()` | the page width follows the content in both |
| `FitWidth = false`, `AutoSizePdfPage = false` | none | Classic cut the content at the page width; Next scales it to the page (`AutoResizeHtmlViewerWidth`, true by default) |
| `StretchToFit` | a zoom above 100 | `HtmlViewerZoom` accepts 10 to 200 |
| `FitHeight` | none | Next never scales to the page height |
| `PdfDocumentOptions.SinglePage` | `PageWidthFromBrowserWindow(1024, singlePage: true)` | sets `AutoResizePdfPageHeight = true`; the page is as tall as the content |
| `TopSpacing` / `BottomSpacing` | header and footer template `Margins` | without a header or footer, the page margins |
| `X`, `Y`, `Width`, `Height` | page margins | no destination rectangle; `Y`, an offset on the first page only, has no equivalent |
| `ClipHtmlView` | none | content wider than the browser window is laid out at its own width and scaled to the page (`AutoResizeHtmlViewerWidth`, true by default) |
| `PdfHeaderOptions` / `PdfFooterOptions` (`HeaderHeight`, `HeaderBackColor`, `AddElement`) | `PdfDocumentOptions.PdfHtmlHeader` / `PdfHtmlFooter` (`Html`, `Height`, `AutoSizeContentHeight`, ...) | one HTML template each |
| `TextElement("Page &p; of &P;")` | `{page_number}` / `{total_pages}` in the template HTML | |
| `LineElement`, `HtmlToPdfElement` in header | HTML/CSS inside the template | |
| `PdfFooterOptions.PageNumberingStartIndex` | `PdfHtmlFooter.PageNumberOffset` / `TotalPagesOffset` | |
| `PrepareRenderPdfPageEvent` with `Page.ShowHeader` | `ShowInFirstPage` / `ShowInOddPages` / `ShowInEvenPages` | |
| `PdfDocumentOptions.ShowHeader` / `ShowFooter` | presence of the template + `ShowIn...` | |
| `PdfSecurityOptions`, `PdfViewerPreferences`, `PdfDocumentInfo` | same names | |
| `MediaType` | `MediaType` | both default to screen |
| `HttpRequestHeaders`, `HttpRequestCookies`, `AuthenticationOptions` | same | |
