# GS1 Barcode Standards — SfBarcodeGenerator, SfQRCodeGenerator, SfDataMatrixGenerator

## Table of Contents
- [Overview](#overview)
- [GS1 Application Identifier (AI) Basics](#gs1-application-identifier-ai-basics)
- [GS1 Code 128 (Code 128 with AIs)](#gs1-code-128-code-128-with-ais)
- [ITF-14](#itf-14)
- [GS1 DataBar Family](#gs1-databar-family)
  - [GS1 DataBar Omnidirectional](#gs1-databar-omnidirectional)
  - [GS1 DataBar Stacked](#gs1-databar-stacked)
  - [GS1 DataBar Stacked Omnidirectional](#gs1-databar-stacked-omnidirectional)
  - [GS1 DataBar Limited](#gs1-databar-limited)
  - [GS1 DataBar Expanded](#gs1-databar-expanded)
  - [GS1 DataBar Expanded Stacked](#gs1-databar-expanded-stacked)
- [GS1 QR Code](#gs1-qr-code)
- [GS1 Data Matrix](#gs1-data-matrix)
- [GS1 DotCode](#gs1-dotcode)
- [Choosing the Right GS1 Symbology](#choosing-the-right-gs1-symbology)
- [Common Application Identifiers Reference](#common-application-identifiers-reference)

---

## Overview

GS1 standards define globally recognized barcode symbologies for encoding product identification and supply-chain data using **GS1 Application Identifiers (AIs)**. The Syncfusion Blazor Barcode suite supports GS1-compliant encoding across all four components:

| Component | GS1 Symbologies Supported |
|-----------|---------------------------|
| `SfBarcodeGenerator` | GS1 Code 128, ITF-14, GS1 DataBar (all 6 variants) — via `BarcodeType` enum |
| `SfQRCodeGenerator` | GS1 QR Code — via `EnableGS1="true"` |
| `SfDataMatrixGenerator` | GS1 Data Matrix — via `EnableGS1="true"` |
| `SfDotCodeGenerator` | GS1 DotCode — via `EnableGS1="true"` |

These symbologies are the backbone of **retail point-of-sale scanning**, **logistics/warehouse case tracking**, **inventory management**, **product traceability**, and **pharmaceutical/healthcare serialization**.

---

## GS1 Application Identifier (AI) Basics

Every GS1-compliant value string follows the pattern:

```
(AI1)data1(AI2)data2...
```

**Example:** `(01)09506000134352(17)261231(10)ABC123`

### AI Syntax Rules

| Rule | Description | Example |
|---|---|---|
| **Brackets** | AI must be enclosed in parentheses | `(01)` not `01` |
| **Position** | AI comes immediately before its data | `(01)12345678901234` |
| **Format** | 2-4 numeric digits only | `(01)`, `(100)`, `(3103)` |
| **Sequence** | AIs are concatenated with no separator | `(01)data1(10)data2` |
| **Uniqueness** | Each AI can appear only once per barcode | ❌ `(01)data1(01)data2` |
| **Fixed-length AIs** | Data length is implicit (no separator needed) | `(01)` is always 14 digits |
| **Variable-length AIs** | FNC1 separator is inserted automatically | `(10)`, `(21)`, `(240)` |

### Common Application Identifiers

| AI | Description | Data Format | Length |
|---|---|---|---|
| `00` | SSCC (Serial Shipping Container Code) | Numeric | 18 |
| `01` | GTIN (Global Trade Item Number) | Numeric | 14 |
| `02` | GTIN of contained trade items | Numeric | 14 |
| `10` | Batch or Lot Number | Alphanumeric | 1-20 |
| `11` | Production Date (YYMMDD) | Numeric | 6 |
| `12` | Due Date (YYMMDD) | Numeric | 6 |
| `13` | Packaging Date (YYMMDD) | Numeric | 6 |
| `15` | Best Before Date (YYMMDD) | Numeric | 6 |
| `16` | Sell By Date (YYMMDD) | Numeric | 6 |
| `17` | Expiration Date (YYMMDD) | Numeric | 6 |
| `21` | Serial Number | Alphanumeric | 1-20 |
| `240` | Additional Product Identification | Alphanumeric | 1-30 |
| `400` | Customer Purchase Order Number | Alphanumeric | 1-30 |
| `410` | Ship To GLN | Numeric | 13 |
| `422` | Country of Origin | Numeric | 3 |

All GS1 components raise `OnValidationFailed` for: missing parentheses, unsupported AI, invalid data length for a given AI, and duplicate AIs.

---

## GS1-Code128 (Code 128 with AIs)

`BarcodeType.GS1Code128` — a linear barcode based on Code 128 that encodes structured AI data with FNC1 separators. Used for shipping labels, batch/lot tracking, and expiration dating in retail and logistics.

**Allowed Input Characters:** Numeric (0-9), uppercase/lowercase alphabetic (A-Z, a-z), and supported ASCII special characters — structured using valid AIs.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfBarcodeGenerator Width="300px" Height="200px"
                    Type="@BarcodeType.GS1Code128"
                    Value="(01)09506000134352(17)261231(10)ABC123">
    <BarcodeGeneratorDisplayText Text="Product Serialization" />
</SfBarcodeGenerator>
```

**Validation rules:** requires `(AI)data` format, 2-4 digit numeric AI, AI-specific data length, no duplicate AIs.

---

## ITF-14

`BarcodeType.ITF14` — the GS1 standard for encoding 14-digit GTINs on outer cases and shipping cartons. Automatically calculates the GS1 Mod-10 check digit and renders bearer bars (top/bottom/left/right framing) per GS1 spec. Common for logistics and warehouse case-level tracking.

**Allowed Input Characters:** Numeric (0-9) only.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<!-- 13 digits: 14th check digit auto-calculated -->
<SfBarcodeGenerator Width="300px" Height="150px"
                    Type="@BarcodeType.ITF14"
                    Value="1234567890123">
    <BarcodeGeneratorDisplayText Text="Carton Tracking" />
</SfBarcodeGenerator>
```

**Validation rules:**
- Accepts 13 or 14 numeric digits only.
- If 13 digits: the 14th (check) digit is auto-calculated via GS1 Mod-10.
- If 14 digits: the check digit is verified.
- Supports GTIN-8/12/13/14 (shorter GTINs are padded to 14 digits).

---

## GS1 DataBar Family

GS1 DataBar symbols are compact linear barcodes for encoding 14-digit GTINs, offering omnidirectional scanning in a smaller footprint than GS1 Code 128 or standard EAN/UPC symbols. All variants are selected via `BarcodeType` on `SfBarcodeGenerator`.

### GS1 DataBar Omnidirectional

`BarcodeType.GS1DataBarOmnidirectional` — single-row GTIN barcode readable from any direction at POS. Ideal for retail consumer products.

```razor
<SfBarcodeGenerator Width="300px" Height="200px"
                    Type="@BarcodeType.GS1DataBarOmnidirectional"
                    Value="12345678901231">
    <BarcodeGeneratorDisplayText Text="Retail Product" />
</SfBarcodeGenerator>
```

### GS1 DataBar Stacked

`BarcodeType.GS1DataBarStacked` — two-row stacked GTIN encoding for small packages where horizontal space is limited. Not omnidirectional (unidirectional scanning).

```razor
<SfBarcodeGenerator Width="350px" Height="200px"
                    Type="@BarcodeType.GS1DataBarStacked"
                    Value="12345678901231">
    <BarcodeGeneratorDisplayText Text="Compact Product Labeling" />
</SfBarcodeGenerator>
```

### GS1 DataBar Stacked Omnidirectional

`BarcodeType.GS1DataBarStackedOmnidirectional` — combines the compact stacked layout with omnidirectional scanning support.

```razor
<SfBarcodeGenerator Width="350px" Height="200px"
                    Type="@BarcodeType.GS1DataBarStackedOmnidirectional"
                    Value="12345678901231">
    <BarcodeGeneratorDisplayText Text="Compact Retail Item" />
</SfBarcodeGenerator>
```

### GS1 DataBar Limited

`BarcodeType.GS1DataBarLimited` — narrowest DataBar variant, for small trade items where full-size EAN/UPC won't fit.

```razor
<SfBarcodeGenerator Width="200px" Height="100px"
                    Type="@BarcodeType.GS1DataBarLimited"
                    Value="12345678901231">
    <BarcodeGeneratorDisplayText Text="Small Retail Item" />
</SfBarcodeGenerator>
```

> **Note:** GS1 DataBar Limited restricts valid GTINs — the check digit must be in the range 0-4.

### GS1 DataBar Expanded

`BarcodeType.GS1DataBarExpanded` — supports multiple AIs (up to 74 numeric or 41 alphanumeric characters), suitable for variable-measure products and coupons.

```razor
<SfBarcodeGenerator Width="400px" Height="100px"
                    Type="@BarcodeType.GS1DataBarExpanded"
                    Value="(01)09506000134352(17)271231(10)LOT12345(21)SN987654321">
    <BarcodeGeneratorDisplayText Text="Product Traceability" />
</SfBarcodeGenerator>
```

### GS1 DataBar Expanded Stacked

`BarcodeType.GS1DataBarExpandedStacked` — multi-row variant of DataBar Expanded, reducing horizontal space while keeping multi-AI capability.

```razor
<SfBarcodeGenerator Width="400px" Height="300px"
                    Type="@BarcodeType.GS1DataBarExpandedStacked"
                    Value="(01)09506000134352(17)271231(10)LOT12345">
    <BarcodeGeneratorDisplayText Text="Supply Chain Tracking" />
</SfBarcodeGenerator>
```

### GS1 DataBar Validation Rules

**GTIN-only variants (Omnidirectional, Stacked, Stacked Omnidirectional, Limited):**
- Accepts GTIN-8, GTIN-12, GTIN-13, or GTIN-14 (bare `3456789012345` or bracketed `(01)3456789012345`).
- 13-digit input → 14th check digit auto-calculated.
- 14-digit input → check digit verified/auto-corrected.
- 8- or 12-digit input → padded to 14 digits with leading zeros.
- Digits only; no multiple AIs allowed.

**Expanded variants (Expanded, Expanded Stacked):**
- `(AI)data` format, AI is 2-4 numeric digits.
- No duplicate AIs.
- Max capacity: 74 numeric or 41 alphanumeric characters total.
- FNC1 separator auto-inserted between variable-length AIs.

---

## GS1 QR Code

Set `EnableGS1="true"` on `SfQRCodeGenerator` to switch it into GS1 mode. The QR Code then includes an FNC1 header indicating GS1 mode and parses/validates AI data.

**Allowed Input Characters:** Numeric, alphabetic, and supported special characters, structured as `(AI)data` pairs.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<!-- GS1 QR Code with GTIN, expiration date, and batch -->
<SfQRCodeGenerator Width="250px" Height="250px"
                   Value="(01)09506000134352(17)271231(10)ABC123"
                   EnableGS1="true">
    <QRCodeGeneratorDisplayText Text="Product Code" Visibility="true" Alignment="Alignment.Center" />
</SfQRCodeGenerator>
```

**GS1 QR Code format:**
- Prefix: FNC1 character (GS1 mode indicator).
- AI format: `(AI)data`, multiple AIs concatenated directly.
- Max capacity depends on QR version and `ErrorCorrectionLevel`.
- Supports all standard GS1 AIs (01-422, 310n-365n).

GS1 QR Code can be combined with `ErrorCorrectionLevel`, `ForeColor`, `BackgroundColor`, and other standard QR customization properties (see [qr-code-generator.md](qr-code-generator.md)).

---

## GS1 Data Matrix

Set `EnableGS1="true"` on `SfDataMatrixGenerator` for GS1-compliant Data Matrix encoding — combining high data density with AI-based structured data. Common in pharmaceutical, healthcare, and manufacturing serialization.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDataMatrixGenerator Width="300px" Height="250px"
                       Value="(01)09506000134369(17)280131(10)LOT001"
                       EnableGS1="true">
</SfDataMatrixGenerator>
```

**Allowed Input Characters:** Numeric, alphanumeric, and supported special characters using AI-tagged structure.

See [data-matrix-generator.md](data-matrix-generator.md) for non-GS1 usage and color/dimension customization (applies equally in GS1 mode).

---

## GS1 DotCode

Set `EnableGS1="true"` on `SfDotCodeGenerator` for GS1-compliant DotCode encoding — commonly used in pharmaceutical and healthcare serialization requiring high-speed industrial printing (inkjet).

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDotCodeGenerator Width="350" Height="250"
                    Value="(01)12345678901231"
                    EnableGS1="true">
</SfDotCodeGenerator>
```

Full details, encoding modes, and validation rules: [dotcode-generator.md](dotcode-generator.md).

---

## Choosing the Right GS1 Symbology

| Need | Use |
|------|-----|
| Case/carton-level tracking with GTIN | ITF-14 |
| Shipping label with GTIN + batch + expiry | GS1 Code 128 |
| Small consumer product, omnidirectional POS scan | GS1 DataBar Omnidirectional |
| Small package, limited horizontal space | GS1 DataBar Stacked / Stacked Omnidirectional |
| Very small trade item | GS1 DataBar Limited |
| Variable-measure item or coupon with multiple AIs | GS1 DataBar Expanded / Expanded Stacked |
| High data capacity + mobile scanning + AIs | GS1 QR Code |
| Very compact label, high density, AIs | GS1 Data Matrix |
| Industrial high-speed inkjet printing + AIs | GS1 DotCode |

## Common Application Identifiers Reference

| Use Case | AI Example | Description |
|---|---|---|
| GTIN Tracking | `(01)12345678901234` | Global Trade Item Number |
| Batch Number | `(10)BATCH2024` | Production batch identification |
| Expiration | `(17)251231` | Expiration date (YYMMDD) |
| Serial Number | `(21)SN123456789` | Unique product serial number |
| Multi-AI Pharma | `(01)12345678901234(10)LOT001(17)251231` | GTIN + lot + expiration |
