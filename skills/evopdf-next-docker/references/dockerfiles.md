# Reference Dockerfiles (from the EvoPdf Next documentation)

The demo application is used as the example; replace `EvoPdf_Next_AspNetDemo_Linux.dll` / `…_Windows.dll` and the port with your own.

## Linux Debian 12 — ASP.NET Core Runtime 8.0
```dockerfile
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
COPY publish/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.dll"]
```

## Linux Ubuntu 24.04 — ASP.NET Core Runtime 10.0
```dockerfile
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
COPY publish/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.dll"]
```

## Linux Ubuntu 22.04 (jammy) — ASP.NET Core Runtime 8.0
```dockerfile
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
COPY publish/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.dll"]
```

## Linux ARM64
```dockerfile
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
COPY publish/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.Arm64.dll"]
```

## Linux variant 5
```dockerfile
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
COPY publish/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.Arm64.dll"]
```

## Linux variant 6
```dockerfile
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
COPY publish/ .

# Ensure execute permissions for EvoPdf HTML to PDF runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_loadhtml

# Ensure execute permissions for EvoPdf PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-arm64/native/evopdf_pdfprocessor

# Expose the port used by the app
EXPOSE 27101

# Set the ASP.NET Core application to listen on port 27101
ENV ASPNETCORE_URLS=http://+:27101

# Start the application
ENTRYPOINT ["dotnet", "EvoPdf_Next_AspNetDemo_Linux.Arm64.dll"]
```

## Windows Server Core LTSC 2022 — ASP.NET Core Runtime 8.0
```dockerfile
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
COPY publish/ .

EXPOSE 27102
ENV ASPNETCORE_URLS=http://+:27102

ENTRYPOINT ["C:\\dotnet\\dotnet.exe", "C:\\app\\EvoPdf_Next_AspNetDemo_Windows.dll"]
```

## Windows Server Core LTSC 2025 — ASP.NET Core Runtime 10.0
```dockerfile
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
COPY publish/ .

EXPOSE 27102
ENV ASPNETCORE_URLS=http://+:27102

ENTRYPOINT ["C:\\dotnet\\dotnet.exe", "C:\\app\\EvoPdf_Next_AspNetDemo_Windows.dll"]
```

## Windows variant 3
```dockerfile
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
COPY publish/ .

# Expose the application port
EXPOSE 27102

# Configure ASP.NET Core to listen on all interfaces and port 27102
ENV ASPNETCORE_URLS=http://+:27102

# Run the application
ENTRYPOINT ["dotnet", "C:\\app\\EvoPdf_Next_AspNetDemo_Windows.dll"]
```

## Windows variant 4
```dockerfile
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
COPY publish/ .

# Expose the application port
EXPOSE 27102

# Configure ASP.NET Core to listen on all interfaces and port 27102
ENV ASPNETCORE_URLS=http://+:27102

# Run the application
ENTRYPOINT ["powershell", "-Command", "Start-Sleep -Seconds 3; dotnet C:\\app\\EvoPdf_Next_AspNetDemo_Windows.dll"]
```

## Build and run
```bash
docker build -t evopdf-next-demo-image-linux .
docker run -d -p 27101:27101 --name evopdf-next-demo-app-linux evopdf-next-demo-image-linux
# Windows containers
docker build -t evopdf-next-demo-image-windows .
docker run -d -p 27102:27102 --name evopdf-next-demo-app-windows evopdf-next-demo-image-windows
```
