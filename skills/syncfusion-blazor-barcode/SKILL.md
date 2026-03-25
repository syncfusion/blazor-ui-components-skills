---
name: syncfusion-blazor-barcode
description: "Build and troubleshoot barcode generation in Blazor using SfBarcodeGenerator, SfQRCodeGenerator, and SfDataMatrixGenerator. Trigger for 1D barcodes (Code39, Code128, Codabar), QR codes with logo and error correction, Data Matrix, checksum validation, and exporting barcodes to images in Syncfusion Blazor apps."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Syncfusion Blazor Barcode Generator

Syncfusion's Blazor Barcode Generator package (`Syncfusion.Blazor.BarcodeGenerator`) provides three components for generating barcodes in Blazor applications:

| Component | Use For |
|-----------|---------|
| `SfBarcodeGenerator` | 1D linear barcodes (Code39, Code128, Codabar, etc.) |
| `SfQRCodeGenerator` | QR codes with optional logo embedding |
| `SfDataMatrixGenerator` | 2D Data Matrix codes for compact encoding |

All three work in **Blazor Server**, **Blazor WebAssembly**, and **Blazor Web App** (.NET 8+).

---

## When to Use This Skill

- User needs to render or display a barcode in a Blazor component
- User asks about QR code generation (e.g., product links, 2FA, contact cards)
- User needs Data Matrix codes (pharmaceutical, logistics, label printing)
- User is configuring `SfBarcodeGenerator`, `SfQRCodeGenerator`, or `SfDataMatrixGenerator`
- User asks about exporting a barcode as an image (JPG/PNG) or Base64
- User asks about barcode types, error correction, checksums, or validation events
- User is embedding a logo in a QR code

---

## Navigation Guide

### Setting Up the Project
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation
- `_Imports.razor` namespace configuration
- Service registration in `Program.cs`
- Stylesheet and script references
- Setup for Blazor Server, WebAssembly, and Web App
- Adding the first barcode component to a page

### 1D Barcode Types and Configuration
📄 **Read:** [references/barcode-types.md](references/barcode-types.md)
- All supported `BarcodeType` enum values with descriptions
- Code39, Code39 Extended, Code11, Code32, Code93, Codabar, Code128
- Choosing the right type for your use case
- `EnableCheckSum` configuration
- `OnValidationFailed` event handling

### QR Code Generator
📄 **Read:** [references/qr-code-generator.md](references/qr-code-generator.md)
- Basic QR code usage
- Error correction levels (L, M, Q, H)
- Embedding a logo image in the QR code center
- Customizing logo size
- Color, dimension, and display text customization
- `OnValidationFailed` event

### Data Matrix Generator
📄 **Read:** [references/data-matrix-generator.md](references/data-matrix-generator.md)
- Basic Data Matrix code usage
- Color, dimension, and display text customization
- Common use cases (pharmaceuticals, labels)
- `OnValidationFailed` event

### Exporting and Customizing Barcodes
📄 **Read:** [references/export-and-customization.md](references/export-and-customization.md)
- Export barcode to image file (JPG/PNG)
- Export as Base64 string
- `ForeColor` for custom barcode color
- `Width` / `Height` for sizing
- `BarcodeGeneratorDisplayText` for label text
- Applies to all three generator types

---

## Quick Start Examples

### 1D Barcode
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
```

### QR Code
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>
```

### Data Matrix
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDataMatrixGenerator Width="200" Height="150"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

---

## Key Properties

| Property | Applies To | Description |
|----------|-----------|-------------|
| `Value` | All | The data string to encode |
| `Width` | All | Width of the barcode (px or %) |
| `Height` | All | Height of the barcode (px or %) |
| `Type` | `SfBarcodeGenerator` | `BarcodeType` enum — selects the 1D symbology |
| `ForeColor` | All | Color of the barcode bars (e.g., `"red"`, `"#333"`) |
| `EnableCheckSum` | `SfBarcodeGenerator` | Adds/validates checksum digit (default `true` for Code39) |
| `ErrorCorrectionLevel` | `SfQRCodeGenerator` | QR error recovery: Low, Medium, Quartile, High |
| `OnValidationFailed` | All | Event triggered when `Value` contains invalid characters |

---

## Common Use Cases

| Need | Component | Read |
|------|-----------|------|
| Product/inventory labels | `SfBarcodeGenerator` (Code128/Code39) | barcode-types.md |
| URL / contact / 2FA QR code | `SfQRCodeGenerator` | qr-code-generator.md |
| Branded QR code with logo | `SfQRCodeGenerator` + `QRCodeLogo` | qr-code-generator.md |
| Pharmaceutical / shipping labels | `SfDataMatrixGenerator` | data-matrix-generator.md |
| Download barcode as image | `.Export()` method | export-and-customization.md |
| Embed barcode in email/PDF | `.ExportAsBase64Image()` | export-and-customization.md |
