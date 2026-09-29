---
name: evopdf-next-azure
description: "Run EvoPdf Next on Azure App Service and Azure Functions, on both Linux and Windows plans: plan requirements, deployment layout and the settings that matter for the rendering engine."
---

# EvoPdf Next: Azure App Service and Azure Functions

EvoPdf Next for .NET is a library that can be integrated into applications running on Azure App Service for Linux to create and process PDF documents.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

EvoPdf Next runs in 64-bit Azure App Service and Azure Functions, Windows and Linux. Windows needs nothing extra; Linux needs the HTML to PDF system packages installed at startup.

## Plan sizing (from the documentation)
- HTML to PDF is CPU/RAM intensive. App Service Windows: **B1** minimum for low volume, **B2** for development, **P1v3** or higher for production; Free/Shared are not suitable. App Service Linux: F1 is the minimum for tests, B2 for development, P1v3+ for production.
- Azure Functions: choose **App Service plan or Premium**: the **Consumption plan is not suitable**; minimum B2 on Linux. Target runtime: *Portable*.

## App Service on Windows
Reference `EvoPdf.Next.Windows` (or single components), `using EvoPdf.Next;`, publish. No further configuration.

## App Service on Linux
Reference `EvoPdf.Next.Linux`. Install the dependencies at every start, because App Service does not persist packages across restarts. Recommended: **Startup Command** (Portal > Configuration > Stack settings), on the same line, before the dotnet command:
```
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0 && dotnet MyApp.dll
```
Save, then Restart; the first start takes a few minutes. Alternative in code: `Installation.ConfigureRuntime(true, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");` before the first conversion.

## Azure Functions on Windows
Reference `EvoPdf.Next.Windows`; App Service or Premium plan, Windows OS. Nothing else.

## Azure Functions on Linux
The file system is read-only outside `/tmp` and runtime files are deployed without execute permission, so use the library's installation API before the first conversion (a call made after the first conversion takes effect only after a restart):
```csharp
// HTML to PDF: installs the packages and prepares a writable runtime location
Installation.ConfigureRuntime(true, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");
// PDF Processor (text, search, images): copies the runtime to a writable location with execute permission
PdfProcessorInstallation.ConfigureRuntime(true, null);
```
Manual alternative via the SSH console is documented but does not survive restarts as reliably.

## Rules
- Never assume the Consumption plan or the Free tier works for conversions.
- Set `Licensing.LicenseKey` from an App Setting (environment variable), not from source.
- Keep `NavigationTimeout` realistic; outbound calls from Azure to the converted site must be allowed.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-windows.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-linux.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-windows.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-linux.htm

- Using EvoPdf Next for .NET on  Azure App Service for Linux: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-linux.htm
- Using EvoPdf Next for .NET on Azure App Service for Windows: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-windows.htm
- Using EvoPdf Next for .NET in Azure Functions on Linux: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-linux.htm
- Using EvoPdf Next for .NET in Azure Functions on Windows: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-windows.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
