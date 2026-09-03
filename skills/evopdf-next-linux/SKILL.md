---
name: evopdf-next-linux
description: "Install and run EvoPdf Next on Linux (Ubuntu, Debian, RedHat, Amazon Linux, x64 and ARM64): NuGet packages, required system packages, execute permissions on the native runtimes, first-line troubleshooting. Use for any EvoPdf Next problem or setup question on Linux."
---

# EvoPdf Next on Linux

Applies to 64-bit Linux, x64 and ARM64. Only the **HTML to PDF Converter** needs system packages; the other components (Word/Excel/RTF/Markdown to PDF, Core, PDF Processor) run without extra dependencies.

## 1. Packages
All components: `EvoPdf.Next.Linux` (x64) or `EvoPdf.Next.Linux.Arm64`. Single components: `EvoPdf.Next.HtmlToPdf.Linux[.Arm64]`, `EvoPdf.Next.WordToPdf.Linux[.Arm64]`, `EvoPdf.Next.ExcelToPdf.Linux[.Arm64]`, `EvoPdf.Next.RtfToPdf.Linux[.Arm64]`, `EvoPdf.Next.MarkdownToPdf.Linux[.Arm64]`, `EvoPdf.Next.PdfProcessor.Linux[.Arm64]`, `EvoPdf.Next.Core.Linux` (covers both architectures). Any combination can be referenced.

## 2. System packages for HTML to PDF
```bash
# Ubuntu 20.04 / 22.04 / 24.04 / 25.04, Debian 12
sudo apt update && sudo apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0

# RedHat 9.5, Amazon Linux 2023
sudo dnf install -y nss at-spi2-atk cairo pango
```
Other distributions need the equivalent NSS, AT-SPI2/ATK bridge, Cairo and Pango packages.

## 3. Execute permissions on the native runtimes
The build copies `evopdf_runtimes/` into the output folder. On Linux the native binaries must be executable; run in the application's output (or publish) folder:
```bash
chmod +x evopdf_runtimes/linux-x64/native/evopdf_loadhtml       # HTML to PDF
chmod +x evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor   # PDF to text / search / image / images extractor
```
For ARM64 the folder is `evopdf_runtimes/linux-arm64/native/`. Permissions can be lost by zip/unzip, some CI artifact steps and file copies from Windows — repeat the command after such steps.

## 4. Code
Identical to any other platform:
```csharp
using EvoPdf.Next;
var converter = new HtmlToPdfConverter();
byte[] pdf = converter.ConvertHtml("<b>Hello</b>", null);
File.WriteAllBytes("hello.pdf", pdf);
```

## Troubleshooting — check in this order
1. **System packages installed?** Run the `apt`/`dnf` command from step 2 — most "it does not work on Linux" reports end here. Missing libraries surface as load failures or timeouts of the HTML to PDF conversion while the other components work.
2. **Execute permission** on `evopdf_loadhtml` / `evopdf_pdfprocessor` (step 3), especially after deployment through a zip or from Windows.
3. **Right package for the architecture** — `uname -m` says `x86_64` → `.Linux`; `aarch64` → `.Linux.Arm64`.
4. **Fonts** — text renders in fallback fonts when the HTML's fonts are not installed; install them (`fonts-liberation`, `fonts-noto`, …) or embed web fonts in the HTML.
5. **Network** — a URL conversion that times out inside a container may be a blocked outbound connection; test with an HTML string first, then raise `NavigationTimeout`.
6. **Watermarked output** — `Licensing.LicenseKey` not set before the first conversion.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-linux.htm
