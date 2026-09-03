---
name: evopdf-next-azure
description: "Deploy EvoPdf Next to Azure App Service and Azure Functions on Windows and Linux: packages, plan sizing, Linux startup command for system packages, Installation.ConfigureRuntime for Azure Functions, read-only file system workarounds. Use for any EvoPdf Next question involving Azure."
---

# EvoPdf Next on Azure

EvoPdf Next runs in 64-bit Azure App Service and Azure Functions, Windows and Linux. Windows needs nothing extra; Linux needs the HTML to PDF system packages installed at startup.

## Plan sizing (from the documentation)
- HTML to PDF is CPU/RAM intensive. App Service Windows: **B1** minimum for low volume, **B2** for development, **P1v3** or higher for production; Free/Shared are not suitable. App Service Linux: F1 is the minimum for tests, B2 for development, P1v3+ for production.
- Azure Functions: choose **App Service plan or Premium** — the **Consumption plan is not suitable**; minimum B2 on Linux. Target runtime: *Portable*.

## App Service — Windows
Reference `EvoPdf.Next.Windows` (or single components), `using EvoPdf.Next;`, publish. No further configuration.

## App Service — Linux
Reference `EvoPdf.Next.Linux`. Install the dependencies at every start, because App Service does not persist packages across restarts. Recommended: **Startup Command** (Portal → Configuration → Stack settings), on the same line, before the dotnet command:
```
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0 && dotnet MyApp.dll
```
Save, then Restart; the first start takes a few minutes. Alternative in code: `Installation.ConfigureRuntime(true, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");` before the first conversion.

## Azure Functions — Windows
Reference `EvoPdf.Next.Windows`; App Service or Premium plan, Windows OS. Nothing else.

## Azure Functions — Linux
The file system is read-only outside `/tmp` and runtime files are deployed without execute permission, so use the library's installation API before the first conversion (only the first call configures; later calls are ignored):
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

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-windows.htm · …-app-service-linux.htm · …-azure-function-windows.htm · …-azure-function-linux.htm
