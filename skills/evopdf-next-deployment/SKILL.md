---
name: evopdf-next-deployment
description: "Install and deploy EvoPdf Next: choosing the NuGet package per platform and architecture, the native runtime folder, runtime configuration and execute permissions, and building and publishing the demo applications."
---

# EvoPdf Next: installation and deployment

EvoPdf Next for .NET is a library that can be integrated into any type of .NET application to create and process PDF documents.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`Installation`**: Contains global installation instructions that configure the behavior of the library within the application

1 types, 9 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

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
- Windows and macOS: no extra dependencies. Linux: the HTML to PDF Converter needs four system packages and execute permissions on the native runtimes; see the Linux section of this skill. Containers: follow the `evopdf-next-docker` skill (complete Dockerfiles).
- Azure App Service (Windows and Linux) and Azure Functions are supported; Docker on Linux and Windows containers too.
- The rendering engine ships inside the package; nothing to install on the server, xcopy deployment works.

## License key
```csharp
Licensing.LicenseKey = Environment.GetEnvironmentVariable("EVOPDF_LICENSE_KEY");
```
Set once at startup (static). Without a key the output is watermarked (demo mode). Keys are perpetual and work offline, with no activation call.

## Diagnosing
- Output watermarked: the license key is not set in the process that converts, or the evaluation key of
  the samples was left in the code.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-windows.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-linux.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-macos.htm, https://www.evopdf.com/evopdf-next-docker

## Getting Started with EvoPdf Next for .NET on Windows

### Include EvoPdf.Next Namespace

```csharp
// add this using statement at the top of your C# file
using EvoPdf.Next;
```

### Convert an HTML string to PDF

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert an HTML string to a memory buffer
byte[] htmlToPdfBuffer = converter.ConvertHtml("<b>Hello World</b> from EVO PDF !", null);

// write the memory buffer to a PDF file
System.IO.File.WriteAllBytes("HtmlToMemory.pdf", htmlToPdfBuffer);
```

### Convert a URL to PDF

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert a URL to a memory buffer
string htmlPageURL = "http://www.evopdf.com";
byte[] urlToPdfBuffer = converter.ConvertUrl(htmlPageURL);

// write the memory buffer to a PDF file
System.IO.File.WriteAllBytes("UrlToMemory.pdf", urlToPdfBuffer);
```

### Convert an HTML string to PDF in ASP.NET

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert an HTML string to a memory buffer
byte[] htmlToPdfBuffer = converter.ConvertHtml("<b>Hello World</b> from EVO PDF !", null);

FileResult fileResult = new FileContentResult(htmlToPdfBuffer, "application/pdf");
fileResult.FileDownloadName = "HtmlToPdf.pdf";
return fileResult;
```

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-windows.htm

## Build and Publish Demo Applications

This topic explains how to build and publish the EvoPdf Next ASP.NET demo applications which can be downloaded as a ZIP file from the EvoPdf website. It should be used after completing the Getting Started guide for Windows and the corresponding Getting Started guide for Linux. The topic includes instructions for the Windows, Linux, macOS and Cross-Platform demos, covering both x64 and ARM64 architectures.

### Windows x64

```bash
cd publish/windows-x64-net10.0
dotnet EvoPdf_Next_AspNetDemo_Windows_net10.0.dll
```

### Windows ARM64

```bash
cd publish/windows-arm64-net10.0
dotnet EvoPdf_Next_AspNetDemo_Windows.Arm64_net10.0.dll
```

### Change Target Framework

```xml
<PropertyGroup>
  <TargetFramework>net10.0</TargetFramework>
</PropertyGroup>
```

### Linux x64

```bash
cd publish/linux-x64-net10.0
dotnet EvoPdf_Next_AspNetDemo_Linux_net10.0.dll
```

All 16 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/build-and-publish-demo-applications.htm

## EvoPdf Next for .NET Overview

### EvoPdf.Next Namespace

```csharp
// add this using statement at the top of your C# file
using EvoPdf.Next;
```

### Sample C# Code to Convert an HTML String to PDF

```csharp
// create the converter object in your code where you want to run conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert an HTML string to a memory buffer
byte[] htmlToPdfBuffer = converter.ConvertHtml("<b>Hello World</b> from EVO PDF !", null);

// write the data from the memory buffer to a PDF file
System.IO.File.WriteAllBytes("HtmlToMemory.pdf", htmlToPdfBuffer);
```

### Convert a URL to PDF

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert a URL to a memory buffer
string htmlPageURL = "http://www.evopdf.com";
byte[] urlToPdfBuffer = converter.ConvertUrl(htmlPageURL);

// write the data from the memory buffer to a PDF file
System.IO.File.WriteAllBytes("UrlToMemory.pdf", urlToPdfBuffer);
```

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/evopdf-next-overview.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
