---
name: evopdf-licensing
description: "Answer licensing and purchasing questions about EvoPdf (Next and Classic): Deployment vs Company License, what counts as one server, load balancing and containers, redistribution, prices, EVO PDF Toolkit vs HTML to PDF Converter license, maintenance renewals, refunds, evaluation, license key delivery. Use for pre-sales and licensing questions; never invent prices or terms."
---

# EvoPdf licensing and purchasing

Source of truth: https://www.evopdf.com/buy · https://www.evopdf.com/license-renewal · https://www.evopdf.com/license-agreement. Quote these facts; for anything not listed, point to the pages or to sales@evopdf.com.

## Two license types (both perpetual)
| | Deployment License | Company License |
|---|---|---|
| Scope | one application on one server, internal use | unlimited developers, applications and servers |
| Redistribution | no | **yes** — the library may ship inside applications distributed to your customers |
| Several instances (load balancing, containers, autoscaling) | not covered | covered |
| Support | standard, first year | priority, first year |
| Upgrade | Deployment → Company possible later via sales@evopdf.com | |
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
Refund requests within 30 days of purchase if the software does not function as described or does not meet reasonable expectations; approved refunds processed within 7 days (license agreement). Orders via FastSpring (purchase online) or PayPro Global (alternate payment) — both issue invoices and accept cards, PayPal and regional methods; quotes and purchase orders via sales@evopdf.com. Vendor: No Limit Software SRL, Bucharest, Romania.

## Typical answers
- "We run 3 instances behind a load balancer" → Company License.
- "We embed the converter in software we sell" → Company License (redistribution).
- "Only HTML to PDF, one internal app, one server" → HTML to PDF Converter, Deployment.
- "Word and Excel to PDF too" → EVO PDF Toolkit.
