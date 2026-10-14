# Installation and deployment: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Getting Started with EvoPdf Next for .NET on Windows

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-windows.htm

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

### Convert a URL to PDF in ASP.NET

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert a URL to a memory buffer
string htmlPageURL = "http://www.evopdf.com";
byte[] urlToPdfBuffer = converter.ConvertUrl(htmlPageURL);

FileResult fileResult = new FileContentResult(urlToPdfBuffer, "application/pdf");
fileResult.FileDownloadName = "UrlToPdf.pdf";
return fileResult;
```

## Build and Publish Demo Applications

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/build-and-publish-demo-applications.htm

### Windows x64

```bash
dotnet build -c Release -r win-x64 EvoPdf_Next_AspNetDemo_Windows_net10.0.csproj
```

```bash
dotnet publish -c Release -r win-x64 -o publish/windows-x64-net10.0 EvoPdf_Next_AspNetDemo_Windows_net10.0.csproj
```

```bash
cd publish/windows-x64-net10.0
dotnet EvoPdf_Next_AspNetDemo_Windows_net10.0.dll
```

### Windows ARM64

```bash
dotnet build -c Release -r win-arm64 EvoPdf_Next_AspNetDemo_Windows.Arm64_net10.0.csproj
```

```bash
dotnet publish -c Release -r win-arm64 -o publish/windows-arm64-net10.0 EvoPdf_Next_AspNetDemo_Windows.Arm64_net10.0.csproj
```

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
dotnet build -c Release -r linux-x64 EvoPdf_Next_AspNetDemo_Linux_net10.0.csproj
```

```bash
dotnet publish -c Release -r linux-x64 -o publish/linux-x64-net10.0 EvoPdf_Next_AspNetDemo_Linux_net10.0.csproj
```

```bash
cd publish/linux-x64-net10.0
dotnet EvoPdf_Next_AspNetDemo_Linux_net10.0.dll
```

### Linux ARM64

```bash
dotnet build -c Release -r linux-arm64 EvoPdf_Next_AspNetDemo_Linux.Arm64_net10.0.csproj
```

```bash
dotnet publish -c Release -r linux-arm64 -o publish/linux-arm64-net10.0 EvoPdf_Next_AspNetDemo_Linux.Arm64_net10.0.csproj
```

```bash
cd publish/linux-arm64-net10.0
dotnet EvoPdf_Next_AspNetDemo_Linux.Arm64_net10.0.dll
```

### Change Target Framework

```xml
<PropertyGroup>
  <TargetFramework>net10.0</TargetFramework>
</PropertyGroup>
```

### Build and Publish

```bash
dotnet build -c Release -r osx-arm64 EvoPdf_Next_AspNetDemo_MacOS_net10.0.csproj
```

```bash
dotnet publish -c Release -r osx-arm64 -o publish/macos-net10.0 EvoPdf_Next_AspNetDemo_MacOS_net10.0.csproj
```

```bash
cd publish/macos-net10.0
dotnet EvoPdf_Next_AspNetDemo_MacOS_net10.0.dll
```

### Change Target Framework

```xml
<PropertyGroup>
  <TargetFramework>net10.0</TargetFramework>
</PropertyGroup>
```

### Cross-Platform x64

```bash
dotnet build -c Release EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.csproj
```

```bash
dotnet publish -c Release -o publish/portable-net10.0 EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.csproj
```

```bash
cd publish/portable-net10.0
dotnet EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.dll
```

```bash
dotnet publish -c Release -r win-x64 -o publish/multiplatform-windows-x64-net10.0 EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.csproj
```

```bash
cd publish/multiplatform-windows-x64-net10.0
dotnet EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.dll
```

```bash
dotnet publish -c Release -r linux-x64 -o publish/multiplatform-linux-x64-net10.0 EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.csproj
```

```bash
cd publish/multiplatform-linux-x64-net10.0
dotnet EvoPdf_Next_AspNetDemo_MultiPlatform_net10.0.dll
```

### Cross-Platform ARM64

```bash
dotnet build -c Release EvoPdf_Next_AspNetDemo_MultiPlatform.Arm64_net10.0.csproj
```

```bash
dotnet publish -c Release -r win-arm64 -o publish/multiplatform-windows-arm64-net10.0 EvoPdf_Next_AspNetDemo_MultiPlatform.Arm64_net10.0.csproj
```

```bash
cd publish/multiplatform-windows-arm64-net10.0
dotnet EvoPdf_Next_AspNetDemo_MultiPlatform.Arm64_net10.0.dll
```

```bash
dotnet publish -c Release -r linux-arm64 -o publish/multiplatform-linux-arm64-net10.0 EvoPdf_Next_AspNetDemo_MultiPlatform.Arm64_net10.0.csproj
```

```bash
cd publish/multiplatform-linux-arm64-net10.0
dotnet EvoPdf_Next_AspNetDemo_MultiPlatform.Arm64_net10.0.dll
```

### Change Target Framework

```xml
<PropertyGroup>
  <TargetFramework>net10.0</TargetFramework>
</PropertyGroup>
```

### Limit to a Specific Platform

```xml
<PropertyGroup>
  <RuntimeIdentifier>win-x64</RuntimeIdentifier>
</PropertyGroup>
```

```xml
<PropertyGroup>
  <RuntimeIdentifier>linux-x64</RuntimeIdentifier>
</PropertyGroup>
```

## EvoPdf Next for .NET Overview

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/evopdf-next-overview.htm

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

## Getting Started with EvoPdf Next for .NET on Linux

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-linux.htm

### Include EvoPdf.Next Namespace

```csharp
// add this using statement at the top of your C# file
using EvoPdf.Next;
```

### Install Dependency Packages

```bash
sudo apt update && sudo apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0
```

```bash
sudo dnf install -y nss at-spi2-atk cairo pango
```

### Set Execution Permissions for HTML to PDF Converter

```bash
chmod +x evopdf_runtimes/linux-x64/native/evopdf_loadhtml
```

### Set Execution Permissions for PDF Processor

```bash
chmod +x evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor
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

### Convert a URL to PDF in ASP.NET

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert a URL to a memory buffer
string htmlPageURL = "http://www.evopdf.com";
byte[] urlToPdfBuffer = converter.ConvertUrl(htmlPageURL);

FileResult fileResult = new FileContentResult(urlToPdfBuffer, "application/pdf");
fileResult.FileDownloadName = "UrlToPdf.pdf";
return fileResult;
```

## Getting Started with EvoPdf Next for .NET on macOS

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/getting-started-on-macos.htm

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

### Convert a URL to PDF in ASP.NET

```csharp
// create the converter object where you want to perform the conversion
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// convert a URL to a memory buffer
string htmlPageURL = "http://www.evopdf.com";
byte[] urlToPdfBuffer = converter.ConvertUrl(htmlPageURL);

FileResult fileResult = new FileContentResult(urlToPdfBuffer, "application/pdf");
fileResult.FileDownloadName = "UrlToPdf.pdf";
return fileResult;
```
