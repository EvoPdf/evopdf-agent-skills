---
name: evopdf-next-aws
description: "Run EvoPdf Next on AWS: Elastic Beanstalk on Linux (Amazon Linux 2023, x86_64 and Graviton) and on Windows Server, AWS Lambda as a container image, Amazon ECS on Fargate and Amazon EKS. Configuration files, Dockerfiles, sizing and the limits that matter for PDF conversion."
---

# EvoPdf Next: AWS

EvoPdf Next for .NET is a library that can be integrated into applications running in AWS Elastic Beanstalk, AWS Lambda, Amazon ECS on AWS Fargate and Amazon EKS to create and process PDF documents.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Elastic Beanstalk on Linux
The .NET platform for Linux, based on Amazon Linux 2023, x86_64 or arm64 (Graviton; reference `EvoPdf.Next.Linux.Arm64`). Two files in the
project root, copied to the publish folder:
- `.ebextensions/evopdf.config`: `packages: yum:` with `nss`, `at-spi2-atk`, `cairo`, `pango` and the DejaVu font
  packages, a fontconfig file that sets DejaVu Sans as sans-serif, and `container_commands` that `chmod +x` the two
  runtimes in `evopdf_runtimes/linux-x64/native/` (`linux-arm64` on Graviton). Indent with spaces.
- `.platform/nginx/conf.d/evopdf.conf`: `proxy_read_timeout 300;` and `client_max_body_size 50M;` for long
  conversions and uploads (nginx defaults: 60 seconds, 1 MB).
```xml
<ItemGroup>
  <None Include=".ebextensions\**" CopyToPublishDirectory="PreserveNewest" />
  <None Include=".platform\**" CopyToPublishDirectory="PreserveNewest" />
</ItemGroup>
```
Publish with `dotnet publish -c Release -r linux-x64 --self-contained false -o publish` and zip the content of the
publish folder at the root of the archive (no Procfile needed). Archives whose paths use backslashes are not extracted.

## Elastic Beanstalk on Windows
Platform ".NET on Windows Server" (IIS 10, Windows Server 2022 or 2025): nothing to install. Publish for `win-x64`,
zip the content of the publish folder as `site.zip`, then create the application bundle, a zip with `site.zip` and an
`aws-windows-deployment-manifest.json` that declares it under `aspNetCoreWeb` with `"appBundle": "site.zip"`.

## AWS Lambda
The HTML to PDF runtime is larger than the 250 MB limit of a zip deployment, so the function is a **container image**
from `public.ecr.aws/lambda/dotnet:10` (or `:8`) with `dnf install -y nss at-spi2-atk cairo pango` plus fonts, the
publish folder copied to `${LAMBDA_TASK_ROOT}` and `chmod +x` on the runtime. Lambda writes only to `/tmp`; the
documented Dockerfile handles it.
- Build with `docker build --platform linux/amd64 --provenance=false --sbom=false` (Lambda accepts single-manifest
  images); arm64 functions reference `EvoPdf.Next.HtmlToPdf.Linux.Arm64` and build for `linux/arm64`.
- Memory **at least 2048 MB** (CPU scales with memory) and timeout **at least 2 minutes**: the first request in a new
  execution environment also starts the converter. A new function starts with 128 MB and 3 seconds.
- The response is limited to 6 MB and binary bodies travel in base64, so a PDF up to about 4.5 MB fits; return larger
  documents through Amazon S3.

## Amazon ECS on Fargate
Linux image from `mcr.microsoft.com/dotnet/aspnet:10.0` with the four packages (`libnss3 libatk-bridge2.0-0 libcairo2
libpango-1.0-0`) and the `chmod +x` lines, listening on 8080, pushed to Amazon ECR. Task definition with at least
**2 vCPUs ("cpu": "2048") and 4 GB ("memory": "4096")**, logs to CloudWatch. For production run an ECS service behind
an Application Load Balancer. For EKS see the `evopdf-next-kubernetes` skill; the same image is used.

## Rules
- Never deploy the HTML to PDF Converter to Lambda as a zip archive.
- Pass the license key as an environment variable or a secret, never in the image or the bundle.
- On a publish folder created on Windows the execute permissions are lost; keep the `chmod +x` steps.

Overview page: https://www.evopdf.com/aws-html-to-pdf-converter

## Using EvoPdf Next for .NET in AWS Elastic Beanstalk on Linux

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-aws-elastic-beanstalk-linux.htm

## Using EvoPdf Next for .NET in AWS Elastic Beanstalk on Windows

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-aws-elastic-beanstalk-windows.htm

## Using EvoPdf Next for .NET in AWS Lambda

All 7 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-aws-lambda.htm

## Using EvoPdf Next for .NET in Amazon ECS on AWS Fargate

All 9 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-amazon-ecs.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
