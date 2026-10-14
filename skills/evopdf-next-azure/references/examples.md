# Azure: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Using EvoPdf Next for .NET on Azure App Service for Linux

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-app-service-linux.htm

### Install Only the Required Dependency Packages

```bash
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0 &&
```

```bash
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0 && dotnet <your_application_name>.dll
```

```csharp
Installation.ConfigureRuntime(false, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");
```

### Install Required Packages via SSH

```bash
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0
```

### Alternative: Deploy a Container Image to App Service

```bash
az appservice plan create --name pdf-plan --resource-group pdf-app-rg --is-linux --sku B2

az webapp create --name pdf-app --resource-group pdf-app-rg --plan pdf-plan \
  --container-image-name myregistry.azurecr.io/pdf-app:1 \
  --container-registry-user myregistry \
  --container-registry-password "$(az acr credential show --name myregistry --query 'passwords[0].value' -o tsv)"

az webapp config appsettings set --name pdf-app --resource-group pdf-app-rg --settings WEBSITES_PORT=8080
```

## Using EvoPdf Next for .NET in Azure Functions on Linux

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-function-linux.htm

### Install Only the Required Dependency Packages

```csharp
Installation.ConfigureRuntime(true, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");
```

```csharp
PdfProcessorInstallation.ConfigureRuntime(true, null);
```

### Set HTML Loader Path

```csharp
converter.HtmlLoaderFilePath = "/tmp/evopdf_runtimes/linux-x64/native/evopdf_loadhtml";
```

### Set PDF Processor Path

```csharp
pdfProcessorComponent.PdfLoaderFilePath = "/tmp/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor";
```

### Install Required Linux Packages via SSH

```bash
apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0
```

### Copy Runtime and Grant Execution Permission

```bash
cp -r /home/site/wwwroot/evopdf_runtimes /tmp/evopdf_runtimes
```

```bash
chmod +x /tmp/evopdf_runtimes/linux-x64/native/evopdf_loadhtml
```

```bash
chmod +x /tmp/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor
```

### Alternative: Deploy a Zip Archive Extracted in the Application Folder

```bash
az group create --name pdf-app-rg --location eastus
az appservice plan create --name pdf-plan --resource-group pdf-app-rg --is-linux --sku B2
az storage account create --name mystorage --resource-group pdf-app-rg --sku Standard_LRS
az functionapp create --name pdf-func --resource-group pdf-app-rg --plan pdf-plan \
  --storage-account mystorage --functions-version 4 \
  --runtime dotnet-isolated --runtime-version 10 --os-type Linux
```

```csharp
Installation.ConfigureRuntime(false, null, "apt update && apt install -y libnss3 libatk-bridge2.0-0 libcairo2 libpango-1.0-0");
```

```bash
dotnet publish -c Release -o publish
az functionapp deployment source config-zip --name pdf-func --resource-group pdf-app-rg --src publish.zip
```

### Alternative: Deploy the Function App as a Container Image

```bash
dotnet publish -c Release -r linux-x64 --self-contained false -o publish
```

```dockerfile
FROM mcr.microsoft.com/azure-functions/dotnet-isolated:4-dotnet-isolated10.0

# Install the packages required by the HTML to PDF Converter
RUN apt-get update && \
    apt-get install -y \
        libnss3 \
        libatk-bridge2.0-0 \
        libcairo2 \
        libpango-1.0-0 && \
    rm -rf /var/lib/apt/lists/*

ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
    AzureFunctionsJobHost__Logging__Console__IsEnabled=true

# Copy the published function app
COPY publish/ /home/site/wwwroot

# Ensure execute permissions for the HTML to PDF Converter and PDF Processor runtimes
RUN chmod +x /home/site/wwwroot/evopdf_runtimes/linux-x64/native/evopdf_loadhtml \
    /home/site/wwwroot/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor
```

```bash
docker build --platform linux/amd64 -t pdf-func .

az functionapp create --name pdf-func --resource-group pdf-app-rg --plan pdf-plan \
  --storage-account mystorage --functions-version 4 \
  --image myregistry.azurecr.io/pdf-func:1 --registry-server myregistry.azurecr.io \
  --registry-username myregistry \
  --registry-password "$(az acr credential show --name myregistry --query 'passwords[0].value' -o tsv)"
```

## Using EvoPdf Next for .NET in Azure Container Apps

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-azure-container-apps.htm

### Build the Container Image

```bash
dotnet publish -c Release -r linux-x64 --self-contained false -o publish
```

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0

# Install the packages required by the HTML to PDF Converter
RUN apt-get update && \
    apt-get install -y \
        libnss3 \
        libatk-bridge2.0-0 \
        libcairo2 \
        libpango-1.0-0 && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the published application
COPY publish/ .

# Ensure execute permissions for the HTML to PDF Converter runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for the PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Listen on port 8080
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

# Start the application
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
docker build --platform linux/amd64 -t pdf-app .
```

### Push the Image to Azure Container Registry

```bash
az group create --name pdf-app-rg --location eastus
az acr create --resource-group pdf-app-rg --name myregistry --sku Basic --admin-enabled true
az acr credential show --name myregistry --query "passwords[0].value" -o tsv
```

```bash
docker login myregistry.azurecr.io -u myregistry
docker tag pdf-app myregistry.azurecr.io/pdf-app:1
docker push myregistry.azurecr.io/pdf-app:1
```

### Create the Container Apps Application

```bash
az extension add --name containerapp --upgrade --yes
az provider register --namespace Microsoft.App --wait
az provider register --namespace Microsoft.OperationalInsights --wait

az containerapp env create --name pdf-env --resource-group pdf-app-rg --location eastus

az containerapp create --name pdf-app --resource-group pdf-app-rg --environment pdf-env \
  --image myregistry.azurecr.io/pdf-app:1 --registry-server myregistry.azurecr.io \
  --registry-username myregistry \
  --registry-password "$(az acr credential show --name myregistry --query 'passwords[0].value' -o tsv)" \
  --ingress external --target-port 8080 --cpu 2 --memory 4Gi --min-replicas 1 --max-replicas 3 \
  --query properties.configuration.ingress.fqdn -o tsv
```

### Troubleshooting

```bash
az containerapp logs show --name pdf-app --resource-group pdf-app-rg --type console --tail 100
az containerapp logs show --name pdf-app --resource-group pdf-app-rg --type system --tail 50
```
