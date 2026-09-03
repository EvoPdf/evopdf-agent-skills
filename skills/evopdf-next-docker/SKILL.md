---
name: evopdf-next-docker
description: "Run EvoPdf Next in Docker containers, Linux (Debian/Ubuntu, x64 and ARM64) and Windows Server Core: complete Dockerfiles, publish and build commands, ports, fonts in Windows containers. Use when containerizing a .NET application that uses EvoPdf Next."
---

# EvoPdf Next in Docker

Read `references/dockerfiles.md` for the full, copy-ready Dockerfiles (Linux Debian 12 / ASP.NET Core 8, Ubuntu 24.04 / ASP.NET Core 10, Windows Server Core 2022 and 2025).

## Linux container — the pattern
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

## Windows container — the pattern
Base image `mcr.microsoft.com/dotnet/aspnet:8.0-windowsservercore-ltsc2022` (or `10.0-windowsservercore-ltsc2025`). No EvoPdf dependencies to install, but **Server Core images ship almost no fonts**: copy the fonts your documents need into `C:\Windows\Fonts` and register them in the registry (see the reference Dockerfile). Nano Server images are not suitable.

## Rules for generated Dockerfiles
- Never bake a license key into the image; pass it as an environment variable (`Licensing.LicenseKey = Environment.GetEnvironmentVariable("EVOPDF_LICENSE_KEY")`).
- Keep the four `apt` packages and the two `chmod` lines; they are the two most common causes of "works locally, fails in the container".
- The application's rendering engine is inside the NuGet package; do not install Chrome or Chromium in the image.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-linux.htm · …/publish-to-docker-on-windows.htm · https://www.evopdf.com/evopdf-next-docker
