# 1D Barcode Types — SfBarcodeGenerator

## Table of Contents
- [Overview](#overview)
- [Choosing a Barcode Type](#choosing-a-barcode-type)
- [Supported BarcodeType Values](#supported-barcodetype-values)
  - [Code39](#code39)
  - [Code39 Extended](#code39-extended)
  - [Code11](#code11)
  - [Code32](#code32)
  - [Code93](#code93)
  - [Code93 Extended](#code93-extended)
  - [Codabar](#codabar)
  - [Code128](#code128)
- [EnableCheckSum](#enablechecksum)
- [OnValidationFailed Event](#onvalidationfailed-event)
- [Barcode Quick Reference Table](#barcode-quick-reference-table)

---

## Overview

`SfBarcodeGenerator` renders 1D (linear) barcodes. Set the `Type` property to a `BarcodeType` enum value to select the symbology. Set `Value` to the string you want encoded.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
```

---

## Choosing a Barcode Type

| If you need to encode… | Use |
|------------------------|-----|
| Uppercase letters + digits (simple inventory) | Code39 |
| Full ASCII characters | Code39Extension or Code93Extension |
| Telecom equipment labels | Code11 |
| Italian pharmaceutical codes | Code32 |
| Denser alphanumeric data | Code93 or Code128 |
| Libraries, blood banks, package delivery | Codabar |
| Any mix of alphanumeric + highest density | Code128 |

**Code128 is the most versatile** — supports all 128 ASCII characters and is a safe default for most applications.

---

## Supported BarcodeType Values

### Code39

**Character set:** Digits 0–9, uppercase A–Z, space, `-`, `+`, `.`, `$`, `/`, `%`  
**Checksum:** Optional (not required for common use)  
**Length:** Variable (keep under 25 chars for practical scan width)

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code39"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
```

---

### Code39 Extended

Enhanced version of Code39. Encodes all 128 ASCII characters including lowercase letters and special keyboard characters. Uses two-symbol combinations to represent characters outside the standard Code39 set.

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code39Extension"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
```

---

### Code11

Primarily used for labeling **telecommunications equipment**.  
**Character set:** Digits 0–9 and dash (`-`), plus start/stop codes.

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code11"
                    Value="112">
</SfBarcodeGenerator>
```

---

### Code32

Used for **Italian pharmaceutical codes** (Pharmacode). Expected input is exactly 8 digits (prefix with `0` if shorter). The 9th checksum digit is calculated automatically.

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code32"
                    Value="01234567">
</SfBarcodeGenerator>
```

> **Note:** Value must be exactly 8 numeric digits. The component calculates the checksum digit internally.

---

### Code93

Designed to complement Code39 with **denser encoding**. Represents the full ASCII set through pairs of characters.  
**Standard mode:** Uppercase A–Z, digits 0–9, `*`, `-`, `$`, `%`, space, `.`, `/`, `+`

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code93"
                    Value="01234567">
</SfBarcodeGenerator>
```

---

### Code93 Extended

Continuous, variable-length, self-checking symbology based on Code93. Can encode **all 128 ASCII characters**.

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code93Extension"
                    Value="syncfusion">
</SfBarcodeGenerator>
```

---

### Codabar

Variable-length barcode encoding 20 characters: `0–9`, `-$:/.+ABCD`.  
Characters A, B, C, D are used as **start/stop codes** and are not part of the data.  
**Common use:** Libraries, blood banks, package delivery.

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Codabar"
                    Value="123456789">
</SfBarcodeGenerator>
```

---

### Code128

The most capable 1D symbology — encodes the **full 128-character ASCII set** with high density. Includes a mandatory checksum digit and supports three code sets:

| Code Set | Characters Encoded |
|----------|--------------------|
| Code Set A | ASCII 0–95 (uppercase + control characters) |
| Code Set B | ASCII 32–127 (uppercase + lowercase + punctuation) |
| Code Set C | Digit pairs 00–99 (numeric data at double density) |

The component automatically selects the optimal code set.

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
```

---

## EnableCheckSum

The `EnableCheckSum` property controls whether a checksum character is appended and validated. The default is `true` for `BarcodeType.Code39`.

Set it to `false` to suppress the extra character displayed at the end of the barcode when you don't want it visible:

```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code39"
                    Value="SYNCFUSION"
                    EnableCheckSum="false">
</SfBarcodeGenerator>
```

> **When to use `EnableCheckSum="false"`:** When the downstream scanner does not expect a checksum character, or when you want to display the barcode without the appended check digit.

---

## OnValidationFailed Event

Fires when `Value` contains characters that are **not valid** for the selected `BarcodeType`. Use this to give the user feedback or log the error.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="@barcodeValue"
                    OnValidationFailed="@HandleValidationFailed">
</SfBarcodeGenerator>

@if (!string.IsNullOrEmpty(validationMessage))
{
    <p style="color: red;">@validationMessage</p>
}

@code {
    private string barcodeValue = "SYNCFUSION";
    private string validationMessage = "";

    private void HandleValidationFailed(ValidationFailedEventArgs args)
    {
        validationMessage = $"Invalid barcode value: {args.Message}";
    }
}
```

---

## Barcode Quick Reference Table

| BarcodeType Enum | Character Set | Checksum | Typical Use |
|-----------------|--------------|----------|-------------|
| `Code39` | A–Z, 0–9, symbols | Optional | Inventory, asset tracking |
| `Code39Extension` | All 128 ASCII | Optional | General alphanumeric |
| `Code11` | 0–9, dash | Built-in | Telecom equipment |
| `Code32` | 0–9 (8 digits) | Auto-calculated | Italian pharmaceuticals |
| `Code93` | A–Z, 0–9, symbols | Built-in | High-density alphanumeric |
| `Code93Extension` | All 128 ASCII | Built-in | Dense full-ASCII data |
| `Codabar` | 0–9, -$:/.+ABCD | Optional | Libraries, blood banks |
| `Code128` | All 128 ASCII | Mandatory | General purpose (recommended) |
