---
name: evopdf-next-kubernetes
description: "Run EvoPdf Next in Kubernetes: one Linux container image and one standard Deployment and Service manifest for Azure Kubernetes Service (AKS), Amazon EKS and Google Kubernetes Engine (GKE), with resource requests and limits, registry access and troubleshooting."
---

# EvoPdf Next: Kubernetes

EvoPdf Next for .NET is a library that can be integrated into applications running in Kubernetes clusters to create and process PDF documents.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## The image and the manifest
The image is the Linux ASP.NET Core image of the `evopdf-next-docker` and `evopdf-next-azure` skills: the four HTML to
PDF packages, `chmod +x` on the runtimes, port 8080. A new pod converts from its first request. The manifest contains
only standard resources and is the same in AKS, EKS and GKE; only the image address changes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pdf-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pdf-app
  template:
    metadata:
      labels:
        app: pdf-app
    spec:
      containers:
      - name: pdf-app
        image: myregistry.azurecr.io/pdf-app:1
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: 500m
            memory: 1Gi
          limits:
            memory: 4Gi
```
plus a `Service` of type `LoadBalancer` from port 80 to `targetPort: 8080` (complete file in `references/examples.md`).

- A memory limit of 4 GiB covers complex documents. Keep the CPU request moderate: on a 2 vCPU node the system pods
  already reserve part of the CPU, and a request of a full vCPU can leave the pod without a node.
- Nodes: 2 vCPUs are enough for tests, at least 4 vCPUs for production (`Standard_D4s_v3`, `m5.xlarge`, `n2-standard-4`).
- Registry access: AKS `--attach-acr`; EKS managed node groups pull from the ECR repositories of the account; GKE needs
  the Artifact Registry Reader role for the node pool service account.

## Troubleshooting
- `Pending` with `Insufficient cpu`: lower the CPU request or add larger nodes.
- `ImagePullBackOff`: wrong image address or no registry access.
- Running pod but the address refuses connections: the Service `targetPort` must equal the port in `ASPNETCORE_URLS`.

## Rules
- Store the license key in a Kubernetes Secret and pass it as an environment variable.
- Scale with `kubectl scale deployment/pdf-app --replicas=N`; each pod runs its own converter.

## Using EvoPdf Next for .NET in Kubernetes

All 7 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-kubernetes.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
