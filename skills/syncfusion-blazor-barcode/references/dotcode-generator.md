# DotCode Generator — SfDotCodeGenerator

## Table of Contents
- [Overview](#overview)
- [Basic Usage](#basic-usage)
- [Data Encoding Capabilities](#data-encoding-capabilities)
- [GS1 DotCode](#gs1-dotcode)
- [Validation Rules](#validation-rules)
- [Customization](#customization)
- [Common Use Cases](#common-use-cases)

---

## Overview

`SfDotCodeGenerator` renders **DotCode** — a high-density, two-dimensional matrix barcode made of a pattern of circular dots arranged in rows and columns. It is designed for industrial printing environments (especially high-speed inkjet) and is widely used in pharmaceutical packaging, healthcare, and product serialization.

**Key characteristics:**
- Encodes numeric, alphanumeric, byte, and GS1 element-string data
- High data density in a compact space
- Supports built-in error detection/correction
- Supports **Structured Append** — spanning data across multiple DotCode symbols
- Optional GS1 compliance mode via `EnableGS1`

### Encoding Modes

| Mode | Data Type | Efficiency |
|---|---|---|
| **Numeric** | Digits 0-9 | 3.33 bits per digit |
| **Alphanumeric** | 0-9, A-Z, space, special chars | 5.5 bits per character |
| **Byte** | Any 8-bit character | 8 bits per character |
| **GS1** | GS1 element strings | Variable (optimized) |

The encoding mode is auto-selected based on the input data.

---

## Basic Usage

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDotCodeGenerator Width="300" Height="250"
                    Value="Product Serialization">
</SfDotCodeGenerator>
```

> Note: `Width` and `Height` accept numeric values (interpreted as pixels), consistent with `SfDataMatrixGenerator`.

**Allowed Input Characters:** Numeric (0-9), uppercase/lowercase alphabetic (A-Z, a-z), and supported special characters.

---

## Data Encoding Capabilities

**Numeric data:**

```razor
<SfDotCodeGenerator Width="300" Height="250" Value="1234567890"></SfDotCodeGenerator>
```

**Alphanumeric data:**

```razor
<SfDotCodeGenerator Width="350" Height="250" Value="DOTCODE-2024-ABC"></SfDotCodeGenerator>
```

---

## GS1 DotCode

DotCode supports a GS1 encoding mode for structured data using GS1 Application Identifiers (AIs) — enable it via the [EnableGS1](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.BarcodeGenerator.SfDotCodeGenerator.html#Syncfusion_Blazor_BarcodeGenerator_SfDotCodeGenerator_EnableGS1) property. This is particularly useful in pharmaceutical and healthcare applications requiring GS1 compliance.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDotCodeGenerator Width="350" Height="250"
                    Value="(01)12345678901231"
                    EnableGS1="true">
</SfDotCodeGenerator>
```

**Allowed Input Characters (GS1 mode):** Numeric, alphanumeric, and supported special characters structured as `(AI)data` pairs.

### Common GS1 Application Identifiers for DotCode

| AI | Description | Example |
|---|---|---|
| `01` | GTIN (Global Trade Item Number) | `(01)12345678901234` |
| `10` | Batch or Lot Number | `(10)ABC123` |
| `11` | Production Date | `(11)200115` |
| `15` | Best Before Date | `(15)251231` |
| `17` | Expiration Date | `(17)251231` |
| `21` | Serial Number | `(21)SN123456` |

```razor
<!-- GS1-enabled DotCode for pharmaceutical use -->
<SfDotCodeGenerator Width="250" Height="200"
                    Value="(01)12345678901234(10)BATCH2024(17)251231"
                    EnableGS1="true"
                    OnValidationFailed="@OnValidationFailed">
</SfDotCodeGenerator>
```

For full GS1 Application Identifier rules, see [gs1-barcodes.md](gs1-barcodes.md).

---

## Validation Rules

### Standard DotCode

- **Length**: Minimum 1 character; maximum depends on symbol size (typically up to 208 characters).
- **Character Set**: Numeric (0-9); Alphanumeric (0-9, A-Z, space, special chars); Byte (all 8-bit values).
- **Encoding Mode**: Auto-selected based on input data.
- **Empty Data**: Not allowed — at least 1 character required.

### GS1 DotCode (`EnableGS1="true"`)

- **Format**: Must use `(AI)data(AI)data...` syntax.
- **AI Format**: 2-4 numeric digits in parentheses.
- **AI Support**: All standard GS1 AIs (01-422, 310n-365n).
- **FNC1 Separator**: Automatically inserted between variable-length AIs.
- **AI Duplication**: Each AI can appear only once.

### OnValidationFailed Errors

| Error | Cause | Solution |
|---|---|---|
| Empty value | No data provided | Supply at least one character |
| Unsupported AI | Invalid Application Identifier | Check AI format (2-4 digits) |
| Invalid AI data | Data doesn't match AI requirements | Verify data length/character set |
| Duplicate AI | Same AI appears multiple times | Remove duplicate AIs |
| Invalid syntax | Missing parentheses in GS1 mode | Use format `(AI)data` |
| Data too long | Exceeds maximum capacity | Reduce data or use structured append |
| Invalid character | Character not supported in mode | Use allowed character set |

```razor
@code
{
    public void OnValidationFailed(ValidationFailedEventArgs args)
    {
        // e.g. Unsupported AI '(99)', invalid data length for (01), duplicate (10)
        Console.WriteLine($"DotCode validation error: {args.Message}");
    }
}
```

---

## Customization

`SfDotCodeGenerator` supports the same core customization properties as the other generators:

| Property | Description |
|---|---|
| `ForeColor` | Dot/text color (default black) |
| `BackgroundColor` | Background color (default white) |
| `Width` / `Height` | Symbol dimensions (numeric, pixels) |
| `OnValidationFailed` | Fires on invalid input or AI errors |

```razor
<SfDotCodeGenerator Width="250" Height="200"
                    BackgroundColor="lightyellow" ForeColor="darkblue"
                    Value="SYNCFUSION">
</SfDotCodeGenerator>
```

---

## Common Use Cases

- **Pharmaceutical serialization** — GS1 DotCode encoding GTIN, lot, and expiration for anti-counterfeiting compliance.
- **Healthcare traceability** — unit-of-use labeling with AI-tagged serial numbers.
- **High-speed industrial printing** — inkjet-printed codes on cartons, cables, or PCB boards where dot-matrix printing is preferred over continuous-tone printing.
- **Product serialization** — unique per-unit tracking codes on small labels.
