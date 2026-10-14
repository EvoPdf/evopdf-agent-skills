---
name: evopdf-next-azure
description: "Run EvoPdf Next on Azure: App Service and Azure Functions on Linux and Windows, Azure Container Apps and AKS. Plan sizing, the Linux system packages and runtime configuration, and the container image deployment that needs no setup at startup."
---

# EvoPdf Next: Azure

EvoPdf Next for .NET is a library that can be integrated into applications running on Azure App Service, Azure Functions, Azure Container Apps and Azure Kubernetes Service to create and process PDF documents.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

EvoPdf Next runs in 64-bit Azure App Service and Azure Functions, Windows and Linux, and in Linux containers. Windows
needs nothing extra. On Linux the HTML to PDF Converter needs four system packages: either install them at startup, or
deploy a container image that already has them, the option the documentation recommends for production.

## Plan sizing (from the documentation)
- HTML to PDF is CPU and memory intensive. App Service Windows: **B1** minimum for low volume, **B2** for development,
  **P1v3** or higher for production; Free and Shared are not suitable. App Service Linux: F1 for tests, B2 for development,
  P1v3 or higher for production.
- Azure Functions: **App Service plan or Premium**. The **Consumption and Flex Consumption plans are not suitable**.
  On Linux the minimum is B2 and the Visual Studio publish profile uses the *Portable* target runtime; on Windows B1.
- Azure Container Apps: at least **2 vCPUs and 4 GiB** per replica; `--min-replicas 1` keeps a replica ready.

## App Service and Functions on Windows
Reference `EvoPdf.Next.Windows` (or single components), `using EvoPdf.Next;`, publish. No further configuration.

## App Service on Linux
Reference `EvoPdf.Next.Linux`. App Service does not keep installed packages across restarts, so install them at every
start with the **Startup Command** (Portal > Configuration > Stack settings), before the dotnet command:
```
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0 && dotnet MyApp.dll
```
or in code, before the first conversion:
```csharp
Installation.ConfigureRuntime(false, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");
```
Alternative recommended for production: deploy a container image built as for Container Apps
(`az webapp create ... --container-image-name myregistry.azurecr.io/pdf-app:1`, then `WEBSITES_PORT=8080`); no Startup
Command and no runtime configuration in code.

## Azure Functions on Linux
A function app published from Visual Studio runs from its package (`WEBSITE_RUN_FROM_PACKAGE=1`): the application folder
is read-only and the runtime files have no execute permission. Copy the runtime to a writable location before the
first conversion (a call made after the first conversion takes effect only after a restart):
```csharp
// HTML to PDF: installs the packages and copies the runtime to a writable location
Installation.ConfigureRuntime(true, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");
// PDF Processor (text, search, images)
PdfProcessorInstallation.ConfigureRuntime(true, null);
```
- A function app created with the Azure CLI and deployed with `az functionapp deployment source config-zip` runs from
  the extracted folder: use `Installation.ConfigureRuntime(false, null, "apt update && ...")`; the PDF Processor needs
  no configuration.
- A function app deployed as a container image from `mcr.microsoft.com/azure-functions/dotnet-isolated:4-dotnet-isolated10.0`
  with the packages installed needs no configuration in code (recommended for production).

## Azure Container Apps and AKS
Build a Linux image from `mcr.microsoft.com/dotnet/aspnet:10.0` with the four packages and the `chmod +x` of the
runtimes, listening on port 8080 (`ENV ASPNETCORE_URLS=http://+:8080`), push it to Azure Container Registry and create
the application with `az containerapp create ... --ingress external --target-port 8080 --cpu 2 --memory 4Gi`.
The `--target-port` must match the port of `ASPNETCORE_URLS`. For AKS see the `evopdf-next-kubernetes` skill; the
same image is used. The Dockerfile is in `references/examples.md`.

## Rules
- Never assume the Consumption or Flex Consumption plan, or the Free tier, works for conversions.
- Set `Licensing.LicenseKey` from an App Setting (environment variable), not from source.
- Keep `NavigationTimeout` realistic; outbound calls from Azure to the converted site must be allowed.

Overview page: https://www.evopdf.com/azure-html-to-pdf-converter

- Using EvoPdf Next for .NET on Azure App Service for Windows: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-windows.htm
- Using EvoPdf Next for .NET in Azure Functions on Windows: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-windows.htm

## Using EvoPdf Next for .NET on Azure App Service for Linux

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-linux.htm

## Using EvoPdf Next for .NET in Azure Functions on Linux

All 14 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-linux.htm

## Using EvoPdf Next for .NET in Azure Container Apps

All 7 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-container-apps.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
