# Docker: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Publish EvoPdf Next ASP.NET Demo to Docker on Linux

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-linux.htm

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

### Building for Linux ARM64

```bash
# .NET 10, for the Dockerfile with the ASP.NET Core Runtime 10.0 base image
dotnet publish -c Release -r linux-arm64 -o publish/linux-arm64-net10.0 EvoPdf_Next_AspNetDemo_Linux.Arm64_net10.0.csproj

# .NET 8, for the Dockerfile with the ASP.NET Core Runtime 8.0 base image
dotnet publish -c Release -r linux-arm64 -o publish/linux-arm64-net8.0 EvoPdf_Next_AspNetDemo_Linux.Arm64_net8.0.csproj
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
COPY publish/linux-arm64-net8.0/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.Arm64_net8.0.dll"]
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
COPY publish/linux-arm64-net10.0/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.Arm64_net10.0.dll"]
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
COPY publish/linux-arm64-net8.0/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.Arm64_net8.0.dll"]
```

## Publish EvoPdf Next ASP.NET Demo to Docker on Windows

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-docker-on-windows.htm

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

### Dockerfile for Windows Server Core LTSC 2022 with Pre-Installed ASP.NET Core Runtime 8.0

```text
FROM mcr.microsoft.com/dotnet/aspnet:8.0-windowsservercore-ltsc2022

SHELL ["powershell", "-Command"]

# Copy fonts from build context into the image Fonts folder
COPY Fonts/ C:/Windows/Fonts/

# Register fonts in container's Windows registry
RUN $ErrorActionPreference = 'Stop'; \
    Get-ChildItem 'C:\Windows\Fonts' -Include *.ttf, *.ttc, *.otf -Recurse | ForEach-Object { \
        $file = $_.Name; \
        $name = [System.IO.Path]::GetFileNameWithoutExtension($file); \
        $regName = $name + ' (TrueType)'; \
        New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts' -Name $regName -PropertyType String -Value $file -Force | Out-Null \
    }

# Set working directory for the app
WORKDIR C:/app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/windows-x64-net8.0/ .

# Expose the application port
EXPOSE 27102

# Configure ASP.NET Core to listen on all interfaces and port 27102
ENV ASPNETCORE_URLS=http://+:27102

# Run the application
ENTRYPOINT ["dotnet", "C:\\app\\EvoPdf_Next_AspNetDemo_Windows_net8.0.dll"]
```

### Dockerfile for Windows Server Core LTSC 2025 with Pre-Installed ASP.NET Core Runtime 10.0

```text
FROM mcr.microsoft.com/dotnet/aspnet:10.0-windowsservercore-ltsc2025

SHELL ["powershell", "-Command"]

# Copy fonts from build context into the image Fonts folder
COPY Fonts/ C:/Windows/Fonts/

# Register fonts in container's Windows registry
RUN $ErrorActionPreference = 'Stop'; \
    Get-ChildItem 'C:\Windows\Fonts' -Include *.ttf, *.ttc, *.otf -Recurse | ForEach-Object { \
        $file = $_.Name; \
        $name = [System.IO.Path]::GetFileNameWithoutExtension($file); \
        $regName = $name + ' (TrueType)'; \
        New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts' -Name $regName -PropertyType String -Value $file -Force | Out-Null \
    }

# Set working directory for the app
WORKDIR C:/app

# Copy the published ASP.NET Core app files from the publish folder into the image
COPY publish/windows-x64-net10.0/ .

# Expose the application port
EXPOSE 27102

# Configure ASP.NET Core to listen on all interfaces and port 27102
ENV ASPNETCORE_URLS=http://+:27102

# Run the application
ENTRYPOINT ["powershell", "-Command", "Start-Sleep -Seconds 3; dotnet C:\\app\\EvoPdf_Next_AspNetDemo_Windows_net10.0.dll"]
```
