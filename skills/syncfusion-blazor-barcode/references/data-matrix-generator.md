# Data Matrix Generator — SfDataMatrixGenerator

## Overview

`SfDataMatrixGenerator` renders **Data Matrix** codes — a 2D barcode consisting of a grid of dark and light dots arranged in a square or rectangular symbol. Data Matrix codes are ideal for encoding small amounts of data in a compact area.

**Key characteristics:**
- Encodes **numeric or alphanumeric** data
- Very small physical footprint (suitable for tiny labels)
- Widely used in **printed media**: labels, letters, product packaging
- Easily read with a barcode scanner or mobile phone camera
- Common in **pharmaceuticals**, **electronics manufacturing**, and **logistics**

---

## Basic Usage

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDataMatrixGenerator Width="200" Height="150"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

> Note: `Width` and `Height` accept numeric values (interpreted as pixels) without the `px` suffix, unlike `SfBarcodeGenerator` and `SfQRCodeGenerator` which accept CSS strings.

---

## Customizing Color

Use `ForeColor` to change the barcode module color. Useful for matching the barcode to a label design or brand palette.

```razor
<SfDataMatrixGenerator Width="200" Height="150"
                       ForeColor="red"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

> **Scanning tip:** Always maintain high contrast between `ForeColor` and the background. Dark bars on a white background provide the most reliable scanning.

---

## Customizing Dimensions

Adjust `Width` and `Height` to fit the barcode within your label or UI layout:

```razor
<SfDataMatrixGenerator Width="300" Height="300"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

Square dimensions are recommended since Data Matrix is inherently a square symbol — non-square dimensions will stretch it.

---

## Customizing Display Text

By default, a text label is displayed below the Data Matrix code. Use `DataMatrixGeneratorDisplayText` to change or hide it:

```razor
<!-- Custom text label -->
<SfDataMatrixGenerator Width="200" Height="150"
                       Value="SYNCFUSION">
    <DataMatrixGeneratorDisplayText Text="Product Code">
    </DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>

<!-- Hide the text label -->
<SfDataMatrixGenerator Width="200" Height="150"
                       Value="SYNCFUSION">
    <DataMatrixGeneratorDisplayText Visibility="false">
    </DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>
```

---

## OnValidationFailed Event

Fires when `Value` contains characters that are not valid for Data Matrix encoding. Use it to surface errors to users.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDataMatrixGenerator Width="200" Height="150"
                       Value="@dataValue"
                       OnValidationFailed="@HandleValidationFailed">
</SfDataMatrixGenerator>

@if (!string.IsNullOrEmpty(errorMessage))
{
    <p style="color: red;">@errorMessage</p>
}

@code {
    private string dataValue = "SYNCFUSION";
    private string errorMessage = "";

    private void HandleValidationFailed(ValidationFailedEventArgs args)
    {
        errorMessage = $"Invalid Data Matrix value: {args.Message}";
    }
}
```

---

## Common Use Cases

### Product Label (compact numeric code)
```razor
<SfDataMatrixGenerator Width="150" Height="150"
                       Value="0123456789">
    <DataMatrixGeneratorDisplayText Visibility="false">
    </DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>
```

### Pharmaceutical / Shipping Label
```razor
<SfDataMatrixGenerator Width="200" Height="200"
                       Value="LOT-A4B2-EXP20260101">
    <DataMatrixGeneratorDisplayText Text="LOT-A4B2">
    </DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>
```

### Electronics PCB Marking
```razor
<SfDataMatrixGenerator Width="100" Height="100"
                       Value="SN12345678">
    <DataMatrixGeneratorDisplayText Visibility="false">
    </DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>
```

---

## Comparison: Data Matrix vs QR Code

| Feature | Data Matrix | QR Code |
|---------|------------|---------|
| Shape | Square/Rectangular | Square |
| Best for | Small labels, compact spaces | URLs, marketing, general use |
| Logo embedding | Not supported | Supported |
| Error correction | Built-in | Configurable (L/M/Q/H) |
| Common industries | Pharma, electronics, logistics | Consumer, marketing, 2FA |
| Component | `SfDataMatrixGenerator` | `SfQRCodeGenerator` |

Use **Data Matrix** when space is extremely limited or for industrial/manufacturing scenarios. Use **QR Code** for consumer-facing applications or when logo embedding is needed.
