# Images, Video, and Audio
## Table of Contents
- [Image Settings Properties](#image-settings-properties)
- [Insert Image From URL](#insert-image-from-url)
- [Upload and Save Images Server-Side](#upload-and-save-images-server-side)
- [Image Save Format](#image-save-format)
- [Image Restrictions](#image-restrictions)
- [Image Quick Toolbar](#image-quick-toolbar)
- [Server-Side Delete on Image Removal](#server-side-delete-on-image-removal)
- [Video Settings](#video-settings)
- [Audio Settings](#audio-settings)
---
## Image Settings Properties
Use `RichTextEditorImageSettings` to control image behaviour:
| Property | Type | Description |
|---|---|---|
| `AllowedTypes` | `List<string>` | Allowed file extensions, e.g. `new() { ".jpg", ".png", ".gif" }` |
| `SaveUrl` | `string` | API endpoint to receive the uploaded image |
| `Path` | `string` | Server path prefix appended to saved file name (must be under `wwwroot`) |
| `SaveFormat` | `SaveFormat` | `Blob` (default) or `Base64` |
| `Display` | `ImageDisplay` | Default display: `Inline` or `Break` |
| `Width` / `Height` | `string` | Default dimensions when inserted |
| `MinWidth` / `MaxWidth` | `string` | Resize constraints |
| `MinHeight` / `MaxHeight` | `string` | Resize constraints |
| `EnableResize` | `bool` | Whether to show resize handles (default `true`) |
| `ResizeByPercent` | `bool` | Resize using percentage instead of pixel values |
| `MaxFileSize` | `double` | Maximum upload file size in bytes |
---
## Insert Image From URL
No configuration needed — the `Image` toolbar item opens a dialog where users can paste an online URL:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfSmartRichTextEditor>
@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Image }
    };
}
```
If no `SaveUrl` or `Path` is set, locally browsed images are inserted as `blob:` URLs (temporary, lost on reload) or Base64, depending on `SaveFormat`.
---
## Upload and Save Images Server-Side
**Component:**
```razor
<SfSmartRichTextEditor>
    <RichTextEditorImageSettings SaveUrl="api/Image/Save" Path="./Images/" />
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfSmartRichTextEditor>
```
**Minimal ASP.NET Core controller:**
```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using System.Net.Http.Headers;
[ApiController]
public class ImageController : ControllerBase
{
    private readonly IWebHostEnvironment _env;
    public ImageController(IWebHostEnvironment env) => _env = env;
    [HttpPost("[action]")]
    [Route("api/Image/Save")]
    public void Save(IList<IFormFile> UploadFiles)
    {
        foreach (var file in UploadFiles)
        {
            string targetPath = Path.Combine(_env.ContentRootPath, "wwwroot", "Images");
            Directory.CreateDirectory(targetPath);
            string fileName = ContentDispositionHeaderValue
                .Parse(file.ContentDisposition).FileName.Trim('"');
            string fullPath = Path.Combine(targetPath, fileName);
            if (!System.IO.File.Exists(fullPath))
            {
                using var fs = System.IO.File.Create(fullPath);
                file.CopyTo(fs);
                Response.StatusCode = 200;
            }
            else
            {
                Response.StatusCode = 204; // Already exists
            }
        }
    }
}
```
> The `Path` property must be a directory under `wwwroot`. You cannot save outside `wwwroot` in Blazor static file serving.
---
## Image Save Format
Force Base64 encoding (no server needed, but increases HTML payload size):
```razor
<SfSmartRichTextEditor>
    <RichTextEditorImageSettings SaveFormat="SaveFormat.Base64" />
</SfSmartRichTextEditor>
```
Use Base64 for: small images, self-contained HTML exports, prototyping.
Use server save for: production apps, large images, long-term storage.
---
## Image Restrictions
Limit file size (in bytes):
```razor
<SfSmartRichTextEditor>
    <RichTextEditorImageSettings MaxFileSize="5000000" AllowedTypes='new() { ".jpg", ".png", ".webp" }' />
</SfSmartRichTextEditor>
```
> Size restriction only applies to file browse uploads, not to images pasted as hyperlinks.
---
## Image Quick Toolbar
Show editing actions when a user clicks an inserted image:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorQuickToolbarSettings Image="@ImageTools" />
</SfSmartRichTextEditor>
@code {
    private List<ImageToolbarItemModel> ImageTools = new()
    {
        new() { Command = ImageToolbarCommand.Replace },
        new() { Command = ImageToolbarCommand.Align },
        new() { Command = ImageToolbarCommand.Caption },
        new() { Command = ImageToolbarCommand.Remove },
        new() { Command = ImageToolbarCommand.HorizontalSeparator },
        new() { Command = ImageToolbarCommand.OpenImageLink },
        new() { Command = ImageToolbarCommand.EditImageLink },
        new() { Command = ImageToolbarCommand.RemoveImageLink },
        new() { Command = ImageToolbarCommand.HorizontalSeparator },
        new() { Command = ImageToolbarCommand.Display },
        new() { Command = ImageToolbarCommand.AltText },
        new() { Command = ImageToolbarCommand.Dimension }
    };
}
```
---
## Server-Side Delete on Image Removal
The `ImageDelete` event fires after an image is removed from the editor. Use it to delete the file from your server:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorImageSettings
        SaveUrl="api/Image/Save"
        Path="./Images/"
        RemoveUrl="api/Image/Delete" />
    <RichTextEditorEvents ImageDelete="OnImageDeleted" />
</SfSmartRichTextEditor>
@code {
    @inject HttpClient Http
    private async Task OnImageDeleted(AfterImageDeleteEventArgs args)
    {
        var fileName = args.Src.Split('/').Last();
        var form = new MultipartFormDataContent();
        form.Add(new ByteArrayContent(Array.Empty<byte>()), "UploadFiles", fileName);
        await Http.PostAsync("api/Image/Delete", form);
    }
}
```
> Without this handler, deleting an image in the editor only removes the `<img>` tag — the file remains on disk.
---
## Video Settings
Add `ToolbarCommand.Video` to the toolbar, then configure `RichTextEditorVideoSettings`:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorVideoSettings
        SaveUrl="api/Video/Save"
        Path="./Videos/"
        AllowedTypes='new() { ".mp4", ".webm" }'
        EnableResize="true"
        Width="300px"
        Height="200px" />
</SfSmartRichTextEditor>
```
Videos can also be inserted as **embedded URLs** (e.g. YouTube iframes) from the dialog — no upload needed.
Quick toolbar commands for video: `Replace`, `Edit`, `Remove`, `Display`, `Dimension`.
---
## Audio Settings
Add `ToolbarCommand.Audio` to the toolbar, then configure `RichTextEditorAudioSettings`:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorAudioSettings
        SaveUrl="api/Audio/Save"
        Path="./Audio/"
        AllowedTypes='new() { ".mp3", ".wav", ".ogg" }' />
</SfSmartRichTextEditor>
```
Audio can be inserted from a web URL or uploaded from the local machine.