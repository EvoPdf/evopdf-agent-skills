---
name: evopdf-next-deployment
description: "Install and deploy EvoPdf Next on Windows, Linux, macOS, Docker, Azure App Service and Azure Functions: NuGet package per platform, Linux system packages, license key setup, common runtime errors. Use for setup, deployment and environment questions."
---

# EvoPdf Next — packages, platforms, deployment

## Choose the package
| Target | All components | HTML to PDF only |
|---|---|---|
| Windows x64 | `EvoPdf.Next.Windows` | `EvoPdf.Next.HtmlToPdf.Windows` |
| Windows ARM64 | `EvoPdf.Next.Windows.Arm64` | `EvoPdf.Next.HtmlToPdf.Windows.Arm64` |
| Linux x64 | `EvoPdf.Next.Linux` | `EvoPdf.Next.HtmlToPdf.Linux` |
| Linux ARM64 | `EvoPdf.Next.Linux.Arm64` | `EvoPdf.Next.HtmlToPdf.Linux.Arm64` |
| macOS (Apple Silicon) | `EvoPdf.Next.MacOS` | `EvoPdf.Next.HtmlToPdf.MacOS` |
| Windows x64 + Linux x64 | `EvoPdf.Next` | `EvoPdf.Next.HtmlToPdf` |
Other components follow the same naming (`WordToPdf`, `ExcelToPdf`, `RtfToPdf`, `MarkdownToPdf`, `Core`, `PdfProcessor`). Several platform packages can be referenced in one project.

```
dotnet add package EvoPdf.Next.Windows
```

## Runtime notes
- Windows and macOS: no extra dependencies. Linux: the HTML to PDF Converter needs four system packages and execute permissions on the native runtimes — follow the `evopdf-next-linux` skill. Containers: follow the `evopdf-next-docker` skill (complete Dockerfiles).
- Azure App Service (Windows and Linux) and Azure Functions are supported; Docker on Linux and Windows containers too.
- The rendering engine ships inside the package; nothing to install on the server, xcopy deployment works.

## License key
```csharp
Licensing.LicenseKey = Environment.GetEnvironmentVariable("EVOPDF_LICENSE_KEY");
```
Set once at startup (static). Without a key the output is watermarked (demo mode). Keys are perpetual and offline — no activation call.

## Diagnosing
- Output watermarked → key not set before the first conversion, or set on a different process.
- Timeouts on Linux → missing system packages or blocked network from the container; check `NavigationTimeout` and `BlockedHosts`.
- Fonts missing on Linux → install the fonts the HTML uses (e.g. `fonts-liberation`), or embed web fonts in the HTML.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-windows.htm · …-linux.htm · …-macos.htm · https://www.evopdf.com/evopdf-next-docker
