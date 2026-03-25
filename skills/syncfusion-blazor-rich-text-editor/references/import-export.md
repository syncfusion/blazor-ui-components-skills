# Import and Export

## Table of Contents
- [Import HTML File](#import-html-file)
- [Import Text File](#import-text-file)
- [Import RTF File](#import-rtf-file)
- [Import from Microsoft Word](#import-from-microsoft-word)
- [Export to Word (DOCX)](#export-to-word-docx)
- [Export to PDF](#export-to-pdf)
- [Export to HTML / RTF Files](#export-to-html--rtf-files)

---

## Import HTML File

Read an HTML file from the server with `StreamReader` and bind it to `@bind-Value`:

```razor
@using System.IO
@using Syncfusion.Blazor.RichTextEditor

<SfRichTextEditor @bind-Value="@HtmlContent" />

@code {
    private string HtmlContent { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        var path = Path.GetFullPath(
            Path.Combine(Directory.GetCurrentDirectory(), "wwwroot", "HtmlFiles", "template.html"));

        using var fs = File.Open(path, FileMode.Open, FileAccess.Read);
        using var sr = new StreamReader(fs);
        HtmlContent = sr.ReadToEnd();
    }
}
```

> Set `EnableHtmlSanitizer="false"` when the imported HTML contains complex formatting you want to preserve exactly (trust the source).

---

## Import Text File

Read a `.txt` file on button click and load it as the editor's value:

```razor
@using System.IO
@using Syncfusion.Blazor.RichTextEditor

<SfRichTextEditor @bind-Value="@Content" EnableHtmlSanitizer="false" />
<button @onclick="ImportText">Import Text File</button>

@code {
    private string Content { get; set; } = "<p>Click the button to import a text file.</p>";

    private void ImportText()
    {
        var path = Path.GetFullPath(
            Path.Combine(Directory.GetCurrentDirectory(), "wwwroot", "content.txt"));

        using var fs = File.Open(path, FileMode.Open, FileAccess.Read);
        using var sr = new StreamReader(fs);
        Content = sr.ReadToEnd();
    }
}
```

---

## Import RTF File

RTF import requires a file upload component and a server-side API that converts RTF to HTML and returns it in a response header:

```razor
@using Syncfusion.Blazor.RichTextEditor
@using Syncfusion.Blazor.Inputs

<SfRichTextEditor @bind-Value="@Content" EnableHtmlSanitizer="false">
    <RichTextEditorImageSettings SaveUrl="api/images/save" Path="../images/" />
</SfRichTextEditor>

<SfUploader>
    <UploaderAsyncSettings SaveUrl="api/import/rtf"
                           RemoveUrl="https://aspnetmvc.syncfusion.com/services/api/uploadbox/Remove" />
    <UploaderEvents Success="@OnUploadSuccess" />
</SfUploader>

@code {
    private string Content { get; set; } = "<div>Example — import an RTF file above.</div>";

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        // Server returns the converted HTML in a custom "rtevalue" response header
        var headers = args.Response.Headers.ToString();
        var parts   = headers.Split("rtevalue: ");
        Content     = parts[1].Split("\r")[0];
    }
}
```

---

## Import from Microsoft Word

Add `ToolbarCommand.ImportWord` to the toolbar and configure `RichTextEditorImportWord` with a service URL that converts DOCX → HTML server-side:

```razor
<SfRichTextEditor Height="400px">
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorImportWord
        ServiceUrl="https://blazor.syncfusion.com/services/production/api/RichTextEditor/ImportFromWord"
        MaxFileSize="10000000" />  <!-- 10 MB cap -->
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.ImportWord },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

### Adding Auth Headers on Upload

Use the `FileUploading` event to inject custom form data (e.g., bearer tokens) before the file is sent:

```razor
<SfRichTextEditor>
    <RichTextEditorEvents FileUploading="@OnFileUploading" />
    <RichTextEditorImportWord ServiceUrl="api/word/import" />
</SfRichTextEditor>

@code {
    private void OnFileUploading(FileUploadingEventArgs args)
    {
        args.CustomFormData = new List<object>
        {
            new { Authorization = "Bearer " + MyAuthService.Token }
        };
    }
}
```

```csharp
// Server-side controller (ASP.NET Core)
[HttpPost("api/word/import")]
public void ImportFromWord(IList<IFormFile> UploadFiles)
{
    var token = Request.Form["Authorization"].ToString(); // retrieve custom header
    // convert DOCX → HTML using Syncfusion.DocIO and return HTML in response header
}
```

---

## Export to Word (DOCX)

Add `ToolbarCommand.ExportWord` and configure a `RichTextEditorExportWord` tag pointing to a server endpoint that returns a `.docx` file:

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorExportWord
        ServiceUrl="https://blazor.syncfusion.com/services/production/api/RichTextEditor/ExportToDocx"
        FileName="document.docx" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.ExportWord },
        new() { Command = ToolbarCommand.ExportPdf },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic }
    };
}
```

**Server-side controller (Syncfusion.DocIO):**

```csharp
[HttpPost("ExportToDocx")]
public FileStreamResult ExportToDocx([FromBody] ExportParam args)
{
    using var document = new WordDocument();
    document.EnsureMinimal();
    document.HTMLImportSettings.ImageNodeVisited += OpenImage;

    bool isValid = document.LastSection.Body.IsValidXHTML(args.Html, XHTMLValidationType.None);
    if (isValid)
        document.Sections[0].Body.Paragraphs[0].AppendHTML(args.Html);

    document.HTMLImportSettings.ImageNodeVisited -= OpenImage;

    var stream = new MemoryStream();
    document.Save(stream, FormatType.Docx);
    stream.Position = 0;
    return File(stream, "application/msword", "Result.docx");
}
```

---

## Export to PDF

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorExportPdf
        ServiceUrl="https://blazor.syncfusion.com/services/production/api/RichTextEditor/ExportToPdf"
        FileName="document.pdf" />
</SfRichTextEditor>
```

**Server-side controller (Syncfusion.DocIO + DocIORenderer):**

```csharp
[HttpPost("ExportToPdf")]
public ActionResult ExportToPdf([FromBody] ExportParam args)
{
    using var wordDoc = new WordDocument();
    wordDoc.EnsureMinimal();
    wordDoc.HTMLImportSettings.ImageNodeVisited += OpenImage;
    wordDoc.LastParagraph.AppendHTML(args.Html);

    var renderer    = new DocIORenderer();
    var pdfDocument = renderer.ConvertToPDF(wordDoc);
    wordDoc.HTMLImportSettings.ImageNodeVisited -= OpenImage;

    var stream = new MemoryStream();
    pdfDocument.Save(stream);
    return File(stream.ToArray(), "application/pdf", "document.pdf");
}

public class ExportParam { public string Html { get; set; } = string.Empty; }
```

> **NuGet packages required:** `Syncfusion.DocIO.Net.Core`, `Syncfusion.DocIORenderer.Net.Core`, `Syncfusion.Pdf.Net.Core`

---

## Export to HTML / RTF Files

### Export to HTML

Convert the `Value` string into a Word document and then save it as HTML using `Syncfusion.DocIO`:

```csharp
public void ExportToHtml(string htmlValue)
{
    // Convert HTML → WordDocument → HTML file
    using var doc = GetWordDocument(htmlValue);
    using var output = new FileStream("output.html", FileMode.Create);
    doc.Save(output, FormatType.Html);
}
```

### Export to RTF

```csharp
public async Task ExportToRtf(string htmlValue)
{
    using var client  = new HttpClient();
    var content       = new StringContent(htmlValue);
    content.Headers.Add("value", htmlValue);
    await client.PostAsync("api/export/rtf", content);
    // trigger JS file download via IJSRuntime
}
```
