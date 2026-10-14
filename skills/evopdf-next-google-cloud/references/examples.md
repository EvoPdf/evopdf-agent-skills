# Google Cloud: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Using EvoPdf Next for .NET in Google Cloud Run

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-google-cloud-run.htm

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

# Listen on the default port of Cloud Run
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

# Start the application
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
docker build --platform linux/amd64 -t pdf-app .
```

### Push the Image to Artifact Registry

```bash
gcloud config set project PROJECT_ID
gcloud services enable run.googleapis.com artifactregistry.googleapis.com
gcloud artifacts repositories create pdf-images --repository-format=docker --location=us-central1
```

```bash
docker login -u oauth2accesstoken us-central1-docker.pkg.dev
docker tag pdf-app us-central1-docker.pkg.dev/PROJECT_ID/pdf-images/pdf-app:1
docker push us-central1-docker.pkg.dev/PROJECT_ID/pdf-images/pdf-app:1
```

### Deploy the Cloud Run Service

```bash
gcloud run deploy pdf-app --image us-central1-docker.pkg.dev/PROJECT_ID/pdf-images/pdf-app:1 \
  --region us-central1 --allow-unauthenticated \
  --memory 2Gi --cpu 2 --timeout 300 --execution-environment gen2
```

### Troubleshooting

```bash
gcloud run services logs read pdf-app --region us-central1 --limit 100
```
