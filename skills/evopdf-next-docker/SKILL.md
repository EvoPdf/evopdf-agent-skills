---
name: evopdf-next-docker
description: "Run EvoPdf Next in Docker containers on Linux and Windows: base images, required native dependencies, Dockerfile layout and publishing the ASP.NET demo."
---

# EvoPdf Next: Docker

This guide explains how to build and run a Docker container for a published ASP.NET Core application using EvoPdf Next on Linux.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

Read `references/dockerfiles.md` for the full, copy-ready Dockerfiles of the documentation (Linux x64 and ARM64: Ubuntu 24.04 with ASP.NET Core 10, Debian 12 and Ubuntu 22.04 with ASP.NET Core 8; Windows Server LTSC 2025 and 2022, Windows Server Core with the ASP.NET Core runtime).

## Linux container: the pattern
1. Publish the application for Linux: `dotnet publish -c Release -r linux-x64 -o publish` (or `-r linux-arm64`). The `publish` folder must contain the app DLL and `evopdf_runtimes/` at its root.
2. Base image: any `mcr.microsoft.com/dotnet/aspnet:<version>` (multi-arch; the host architecture is selected automatically).
3. Install the four HTML to PDF dependencies, copy `publish/`, `chmod +x` the two native runtimes, expose the port.

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0
RUN apt-get update && apt-get install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0 && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY publish/ .
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml \
 && chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
ENTRYPOINT ["dotnet", "MyApp.dll"]
```
```bash
docker build -t myapp .
docker run -d -p 8080:8080 --name myapp myapp
# ARM64 image built on an x64 host:
docker build --platform linux/arm64 -t myapp-arm64 .
docker run -d --platform linux/arm64 -p 8080:8080 myapp-arm64
```
For ARM64 the runtime folder is `evopdf_runtimes/linux-arm64/native/`.

## Windows container: the pattern
Base image `mcr.microsoft.com/dotnet/aspnet:10.0-windowsservercore-ltsc2025` (or `8.0-windowsservercore-ltsc2022`), or a Windows Server image with the ASP.NET Core runtime installed in the Dockerfile. No EvoPdf dependencies to install, but **Server Core images ship almost no fonts**: copy the fonts your documents need into `C:\Windows\Fonts` and register them in the registry (see the reference Dockerfile).

## Rules for generated Dockerfiles
- Never bake a license key into the image; pass it as an environment variable (`Licensing.LicenseKey = Environment.GetEnvironmentVariable("EVOPDF_LICENSE_KEY")`).
- Keep the four `apt` packages and the two `chmod` lines; the documented Dockerfiles require both.
- The application's rendering engine is inside the NuGet package; do not install Chrome or Chromium in the image.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-linux.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-windows.htm, https://www.evopdf.com/evopdf-next-docker

## Publish EvoPdf Next ASP.NET Demo to Docker on Linux

This guide explains how to build and run a Docker container for a published ASP.NET Core application using EvoPdf Next on Linux.

All 14 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-linux.htm

## Publish EvoPdf Next ASP.NET Demo to Docker on Windows

This guide explains how to build and run a Docker container for a published ASP.NET Core application using EvoPdf Next on Windows.

All 9 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-windows.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
