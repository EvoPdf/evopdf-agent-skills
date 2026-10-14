---
name: evopdf-next-google-cloud
description: "Run EvoPdf Next on Google Cloud: Cloud Run services from a Linux container image, in the first or second generation execution environment, with Artifact Registry, and Google Kubernetes Engine. Dockerfile, sizing, timeouts and CPU allocation for PDF conversion."
---

# EvoPdf Next: Google Cloud

EvoPdf Next for .NET is a library that can be integrated into applications running in Google Cloud Run and Google Kubernetes Engine to create and process PDF documents.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Cloud Run
- Image: `mcr.microsoft.com/dotnet/aspnet:10.0` (or `8.0`) with the four HTML to PDF packages (`libnss3
  libatk-bridge2.0-0 libcairo2 libpango-1.0-0`), the publish folder and `chmod +x` on the runtimes; listen on 8080, the
  port Cloud Run sends requests to by default. Cloud Run runs x86_64 images: `docker build --platform linux/amd64`.
- Push to a Docker repository in Artifact Registry (`REGION-docker.pkg.dev/PROJECT_ID/REPOSITORY/IMAGE:TAG`).
- Deploy with `gcloud run deploy ... --memory 2Gi --cpu 2 --timeout 300 --execution-environment gen2`:
  2 vCPUs and 2 GiB are the recommended minimum; the second generation environment converts faster.
- CPU is allocated only while a request is handled. Conversions that run in background tasks after the response need
  `--no-cpu-throttling`.
- The first request to a new instance also starts the converter and takes longer.

## Google Kubernetes Engine
The same image runs in GKE with the standard manifest of the `evopdf-next-kubernetes` skill. The node pool service
account needs the Artifact Registry Reader role to pull the image (a 403 Forbidden in the pull events means it is
missing).

## Rules
- Pass the license key as an environment variable or a Secret Manager secret, never in the image.
- If the deployment reports that the container does not listen on the expected port, align `ASPNETCORE_URLS` in the
  Dockerfile with the service port, or set `--port`.

Overview page: https://www.evopdf.com/google-cloud-html-to-pdf-converter

## Using EvoPdf Next for .NET in Google Cloud Run

All 7 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-google-cloud-run.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
