---
name: evopdf-next-docker
description: "Run EvoPdf Next in Docker containers on Linux and Windows: base images, required native dependencies, Dockerfile layout and publishing the ASP.NET demo."
---

# EvoPdf Next: Docker

This guide explains how to build and run a Docker container for a published ASP.NET Core application using EvoPdf Next on Linux.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

Read `references/dockerfiles.md` for the full, copy-ready Dockerfiles (Linux Debian 12 / ASP.NET Core 8, Ubuntu 24.04 / ASP.NET Core 10, Windows Server Core 2022 and 2025).

## Linux container: the pattern
1. Publish the application for Linux: `dotnet publish -c Release -r linux-x64 -o publish` (or `-r linux-arm64`). The `publish` folder must contain the app DLL and `evopdf_runtimes/` at its root.
2. Base image: any `mcr.microsoft.com/dotnet/aspnet:<version>` (multi-arch; the host architecture is selected automatically).
3. Install the four HTML to PDF dependencies, copy `publish/`, `chmod +x` the two native runtimes, expose the port.

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
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
Base image `mcr.microsoft.com/dotnet/aspnet:8.0-windowsservercore-ltsc2022` (or `10.0-windowsservercore-ltsc2025`). No EvoPdf dependencies to install, but **Server Core images ship almost no fonts**: copy the fonts your documents need into `C:\Windows\Fonts` and register them in the registry (see the reference Dockerfile).

## Rules for generated Dockerfiles
- Never bake a license key into the image; pass it as an environment variable (`Licensing.LicenseKey = Environment.GetEnvironmentVariable("EVOPDF_LICENSE_KEY")`).
- Keep the four `apt` packages and the two `chmod` lines; the documented Dockerfiles require both.
- The application's rendering engine is inside the NuGet package; do not install Chrome or Chromium in the image.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-linux.htm, https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-windows.htm, https://www.evopdf.com/evopdf-next-docker

## Publish EvoPdf Next ASP.NET Demo to Docker on Linux

### Publish the Demo Application to a Folder

```bash
# .NET 10, for the Dockerfile with the ASP.NET Core Runtime 10.0 base image
dotnet publish -c Release -r linux-x64 -o publish/linux-x64-net10.0 EvoPdf_Next_AspNetDemo_Linux_net10.0.csproj

# .NET 8, for the Dockerfile with the ASP.NET Core Runtime 8.0 base image
dotnet publish -c Release -r linux-x64 -o publish/linux-x64-net8.0 EvoPdf_Next_AspNetDemo_Linux_net8.0.csproj
```

### Dockerfile for Linux Debian 12 with Preinstalled ASP.NET Core Runtime 8.0

```text
FROM mcr.microsoft.com/dotnet/aspnet:8.0

# Install EvoPdf dependencies
RUN apt-get update && \
    apt-get install -y \
        libnss3 \
        libatk-bridge2.0-0 \
        libcairo2 \
        libpango-1.0-0 && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/linux-x64-net8.0/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux_net8.0.dll"]
```

### Dockerfile for Linux Ubuntu 24.04 with Preinstalled ASP.NET Core Runtime 10.0

```text
FROM mcr.microsoft.com/dotnet/aspnet:10.0

# Install EvoPdf dependencies
RUN apt-get update && \
    apt-get install -y \
        libnss3 \
        libatk-bridge2.0-0 \
        libcairo2 \
        libpango-1.0-0 && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/linux-x64-net10.0/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux_net10.0.dll"]
```

### Dockerfile for Linux Ubuntu 22.04 with Preinstalled ASP.NET Core Runtime 8.0

```text
FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy

# Install EvoPdf dependencies
RUN apt-get update && \
    apt-get install -y \
        libnss3 \
        libatk-bridge2.0-0 \
        libcairo2 \
        libpango-1.0-0 && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/linux-x64-net8.0/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux_net8.0.dll"]
```

All 8 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-linux.htm

## Publish EvoPdf Next ASP.NET Demo to Docker on Windows

This guide explains how to build and run a Docker container for a published ASP.NET Core application using EvoPdf Next on Windows.

### Publish the Demo Application to a Folder

```bash
# .NET 10, for the Dockerfile with the ASP.NET Core Runtime 10.0 base image
dotnet publish -c Release -r win-x64 -o publish/windows-x64-net10.0 EvoPdf_Next_AspNetDemo_Windows_net10.0.csproj

# .NET 8, for the Dockerfile with the ASP.NET Core Runtime 8.0 base image
dotnet publish -c Release -r win-x64 -o publish/windows-x64-net8.0 EvoPdf_Next_AspNetDemo_Windows_net8.0.csproj
```

### Dockerfile for Windows Server LTSC 2022 with Manually Installed ASP.NET Core Runtime 8.0

```text
FROM mcr.microsoft.com/windows/server:ltsc2022

# Use PowerShell as default shell
SHELL ["powershell", "-Command"]

# Install .NET 8.0 ASP.NET Core Runtime from ZIP
RUN Invoke-WebRequest -Uri 'https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/8.0.22/aspnetcore-runtime-8.0.22-win-x64.zip' -OutFile 'C:\\dotnet.zip'; \
    Expand-Archive -Path 'C:\\dotnet.zip' -DestinationPath 'C:\\dotnet'; \
    Remove-Item 'C:\\dotnet.zip' -Force

ENV DOTNET_ROOT=C:\dotnet

WORKDIR C:/app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/windows-x64-net8.0/ .

EXPOSE 27102
ENV ASPNETCORE_URLS=http://+:27102

ENTRYPOINT ["C:\\dotnet\\dotnet.exe", "C:\\app\\EvoPdf_Next_AspNetDemo_Windows_net8.0.dll"]
```

### Dockerfile for Windows Server LTSC 2025 with Manually Installed ASP.NET Core Runtime 10.0

```text
FROM mcr.microsoft.com/windows/server:ltsc2025

# Use PowerShell as default shell
SHELL ["powershell", "-Command"]

# Install .NET 10.0 ASP.NET Core Runtime from ZIP
RUN Invoke-WebRequest -Uri 'https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/10.0.0/aspnetcore-runtime-10.0.0-win-x64.zip' -OutFile 'C:\\dotnet.zip'; \
    Expand-Archive -Path 'C:\\dotnet.zip' -DestinationPath 'C:\\dotnet'; \
    Remove-Item 'C:\\dotnet.zip' -Force

ENV DOTNET_ROOT=C:\dotnet

WORKDIR C:/app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/windows-x64-net10.0/ .

EXPOSE 27102
ENV ASPNETCORE_URLS=http://+:27102

ENTRYPOINT ["C:\\dotnet\\dotnet.exe", "C:\\app\\EvoPdf_Next_AspNetDemo_Windows_net10.0.dll"]
```

### Copy Fonts from Host to Local Build Context Folder

```powershell
$sourceFontsPath = Join-Path $env:SystemRoot "Fonts"
$targetFontsPath = ".\Fonts"
if (-not (Test-Path $targetFontsPath)) {
    New-Item -ItemType Directory -Path $targetFontsPath | Out-Null
}
# Copy font files
Get-ChildItem $sourceFontsPath -Include *.ttf, *.ttc, *.otf -Recurse |
    Where-Object Name -ne 'lucon.ttf' |
    Copy-Item -Destination $targetFontsPath -Force
```

All 6 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-windows.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
