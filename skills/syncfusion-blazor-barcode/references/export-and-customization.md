# Export and Customization — Barcode Generator

## Table of Contents
- [Export to Image File](#export-to-image-file)
- [Export as Base64 String](#export-as-base64-string)
- [Supported Export Formats](#supported-export-formats)
- [Customizing Barcode Color](#customizing-barcode-color)
- [Customizing Barcode Dimensions](#customizing-barcode-dimensions)
- [Customizing Display Text](#customizing-display-text)
- [Applying Customization to All Three Generators](#applying-customization-to-all-three-generators)

---

## Export to Image File

Call `.Export(fileName, BarcodeExportType)` on a component reference to download the barcode as an image file in the browser.

**Example with QR Code:**
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<button @onclick="OnExport">Download QR Code</button>

<SfQRCodeGenerator @ref="qrCode"
                   Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Text="Syncfusion"></QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>

@code {
    private SfQRCodeGenerator qrCode;

    private void OnExport()
    {
        qrCode.Export("qr-code", BarcodeExportType.PNG);
    }
}
```

**Example with 1D Barcode:**
```razor
<button @onclick="OnExport">Download Barcode</button>

<SfBarcodeGenerator @ref="barcode"
                    Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>

@code {
    private SfBarcodeGenerator barcode;

    private void OnExport()
    {
        barcode.Export("barcode", BarcodeExportType.JPG);
    }
}
```
⚠️ The export method is Export() (synchronous), not ExportAsync().

---

## Export as Base64 String

Call `.ExportAsBase64Image(BarcodeExportType)` to get the barcode as a **Base64-encoded string** instead of triggering a download. Useful for embedding in emails, PDFs, or sending to an API.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<button @onclick="OnExportBase64">Get Base64</button>

<SfQRCodeGenerator @ref="qrCode"
                   Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>

@if (!string.IsNullOrEmpty(base64String))
{
    <img src="@base64String" alt="QR Code" />
}

@code {
    private SfQRCodeGenerator qrCode;
    private string base64String = "";

    private async void OnExportBase64()
    {
        base64String = await qrCode.ExportAsBase64Image(BarcodeExportType.PNG);
    }
}
```

> **Note:** `ExportAsBase64Image` returns a data URI string (e.g., `data:image/png;base64,...`) that can be used directly as an `<img>` `src`.

---

## Supported Export Formats

| `BarcodeExportType` | Description |
|--------------------|-------------|
| `BarcodeExportType.JPG` | JPEG image — smaller file size, slight quality loss |
| `BarcodeExportType.PNG` | PNG image — lossless, recommended for crisp barcode rendering |

**Recommendation:** Use `PNG` for barcodes whenever possible — lossless compression ensures the bars and modules are sharp, which is important for reliable scanning.

---

## Customizing Barcode Color

Use `ForeColor` on any of the three generators to change the barcode bar/module color. Accepts any valid CSS color (named colors, hex, rgb):

```razor
<!-- Named color -->
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    ForeColor="navy"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>

<!-- Hex color -->
<SfQRCodeGenerator Width="200px" Height="200px"
                   ForeColor="#1565c0"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>

<!-- RGB color -->
<SfDataMatrixGenerator Width="200" Height="150"
                       ForeColor="rgb(21, 101, 192)"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

> **Scanning contrast rule:** Dark bars on a light background scan most reliably. Avoid light-colored barcodes on white backgrounds.

---

## Customizing Barcode Dimensions

`Width` and `Height` control the rendered size of the barcode:

```razor
<!-- SfBarcodeGenerator and SfQRCodeGenerator accept CSS strings -->
<SfBarcodeGenerator Width="300px" Height="200px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>

<!-- SfDataMatrixGenerator accepts numeric values -->
<SfDataMatrixGenerator Width="300" Height="300"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

> **Tip:** For `SfQRCodeGenerator` and `SfDataMatrixGenerator`, use equal `Width` and `Height` for a square aspect ratio. Unequal values will stretch the symbol.

---

## Customizing Display Text

Each generator has a corresponding display text child component. Use it to override the default label or hide it:

### SfBarcodeGenerator
```razor
<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
    <BarcodeGeneratorDisplayText Text="Product Code">
    </BarcodeGeneratorDisplayText>
</SfBarcodeGenerator>
```

### SfQRCodeGenerator
```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Text="Scan to visit"
                                Visibility="true">
    </QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>
```

### SfDataMatrixGenerator
```razor
<SfDataMatrixGenerator Width="200" Height="150"
                       Value="SYNCFUSION">
    <DataMatrixGeneratorDisplayText Text="LOT-001"
                                    Visibility="true">
    </DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>
```

To **hide** the text for any generator, set `Visibility="false"` on the display text child component.

---

## Applying Customization to All Three Generators

Full example showing color, dimensions, custom text, and export together:

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<button @onclick="ExportAll">Export All</button>

<SfBarcodeGenerator @ref="barcode"
                    Width="250px" Height="120px"
                    Type="@BarcodeType.Code128"
                    ForeColor="#212121"
                    Value="SYNCFUSION">
    <BarcodeGeneratorDisplayText Text="Code128 Example"></BarcodeGeneratorDisplayText>
</SfBarcodeGenerator>

<SfQRCodeGenerator @ref="qrCode"
                   Width="200px" Height="200px"
                   ForeColor="#1a237e"
                   ErrorCorrectionLevel="ErrorCorrectionLevel.Medium"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Visibility="false"></QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>

<SfDataMatrixGenerator @ref="dataMatrix"
                       Width="200" Height="200"
                       ForeColor="#b71c1c"
                       Value="SYNCFUSION">
    <DataMatrixGeneratorDisplayText Text="Data Matrix"></DataMatrixGeneratorDisplayText>
</SfDataMatrixGenerator>

@code {
    private SfBarcodeGenerator barcode;
    private SfQRCodeGenerator qrCode;
    private SfDataMatrixGenerator dataMatrix;

    private void ExportAll()
    {
        barcode.Export("barcode-export", BarcodeExportType.PNG);
        qrCode.Export("qr-export", BarcodeExportType.PNG);
        dataMatrix.Export("datamatrix-export", BarcodeExportType.PNG);
    }
}
```
