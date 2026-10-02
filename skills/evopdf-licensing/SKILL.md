---
name: evopdf-licensing
description: "Apply an EvoPdf Next license key in .NET, understand demo mode and the watermark, and know where the key goes in web applications and services."
---

# EvoPdf Next: licensing

By default, the EvoPdf Next library runs in demo mode, which adds an evaluation message to the generated documents and images. To use the library in licensed mode, you need to install a license key in your application.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`Licensing`**: Manages the license settings for the library tools

1 types, 1 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

Source of truth: https://www.evopdf.com/buy, https://www.evopdf.com/license-renewal, https://www.evopdf.com/license-agreement. Quote these facts; for anything not listed, point to the pages or to sales@evopdf.com.

## Two license types (both perpetual)
| | Deployment License | Company License |
|---|---|---|
| Scope | one application on one server, internal use | unlimited developers, applications and servers |
| Redistribution | no | **yes**: the library may ship inside applications distributed to your customers |
| Several instances (load balancing, containers, autoscaling) | not covered | covered |
| Support | standard, first year | priority, first year |
| Upgrade | Deployment to Company possible later via sales@evopdf.com | |
Restriction for both: the software is used as part of your application, not offered as a development tool or standalone conversion service.

## Products and prices (USD)
| Product | Deployment | Company | Covers |
|---|---|---|---|
| EVO HTML to PDF Converter | 450 | 1200 | HTML to PDF, Next **and** Classic editions |
| EVO PDF Toolkit | 650 | 1400 | every component on evopdf.com: all EvoPdf Next components and the complete Classic suite |
Word, Excel, RTF, Markdown to PDF and the PDF tools are licensed only through the Toolkit.

## What you get
Evaluation: the full product, unlimited in time, no registration; output watermarked until a key is set; technical support is free while evaluating. After purchase: a license key string by e-mail (usually within minutes), set in code (`Licensing.LicenseKey` for Next); no activation, no online check. Keys are perpetual for the versions released during the maintenance period.

## Maintenance and renewals
First year of updates and support included. Renewal (optional; applications keep working without it): Toolkit 300 / 650 USD, HTML to PDF 250 / 550 USD (Deployment / Company), within maintenance or the 15-day grace period. Late renewal up to one year after expiration at 20% off the full license price (520 / 1120 and 360 / 960). After one year: a new license. Published renewal prices assume auto-renewal; one-time payments via sales.

## Refunds and ordering
Refund requests within 30 days of purchase if the software does not function as described or does not meet reasonable expectations; approved refunds processed within 7 days (license agreement). Orders via FastSpring (purchase online) or PayPro Global (alternate payment); both issue invoices and accept cards, PayPal and regional methods; quotes and purchase orders via sales@evopdf.com. Vendor: No Limit Software SRL, Bucharest, Romania.

## Typical answers
- "We run 3 instances behind a load balancer": Company License.
- "We embed the converter in software we sell": Company License (redistribution).
- "Only HTML to PDF, one internal app, one server": HTML to PDF Converter, Deployment.
- "Word and Excel to PDF too": EVO PDF Toolkit.

## Licensing for EvoPdf Next

### Setting the Global License Key

```csharp
// Set the license key received after purchase to use the converter in licensed mode.
// Leave it unset to use the library in demo mode.
Licensing.LicenseKey = "your-license-key";
```

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/licensing-for-evopdf-next.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
