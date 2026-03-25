# Events

## Table of Contents
- [Wiring Events](#wiring-events)
- [Complete Event Reference](#complete-event-reference)
  - [Lifecycle Events](#lifecycle-events)
  - [Focus & Interaction](#focus--interaction)
  - [Value & Content](#value--content)
  - [Toolbar Command Lifecycle](#toolbar-command-lifecycle)
  - [Dialog Events](#dialog-events)
  - [Quick Toolbar Events](#quick-toolbar-events)
  - [Image Events](#image-events)
  - [Media Events](#media-events)
  - [Resize Events](#resize-events)
  - [Toolbar Status Events](#toolbar-status-events)
  - [Export Events](#export-events)
  - [Slash Menu Events](#slash-menu-events)
- [Common Patterns](#common-patterns)

---

## Wiring Events

All events are registered through the `RichTextEditorEvents` child component:

```razor
<SfRichTextEditor @bind-Value="@Content">
    <RichTextEditorEvents
        Created="@OnCreated"
        ValueChange="@OnValueChange"
        OnActionBegin="@OnActionBegin"
        Blur="@OnBlur"
        Focus="@OnFocus" />
</SfRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;

    private void OnCreated(object args) { /* editor is ready */ }
    private void OnValueChange(Syncfusion.Blazor.RichTextEditor.ChangeEventArgs args) { Content = args.Value; }
    private void OnActionBegin(ActionBeginEventArgs args) { /* before toolbar command executes */ }
    private void OnBlur(BlurEventArgs args) { /* editor lost focus */ }
    private void OnFocus(Syncfusion.Blazor.RichTextEditor.FocusEventArgs args) { /* editor gained focus */ }
}
```

---

## Complete Event Reference

### Lifecycle Events

#### `Created`
Fires after the editor renders for the first time.

**EventArgs:** `Object` — no properties

---

#### `Destroyed`
Fires after the component is disposed.

**EventArgs:** `DestroyedEventArgs` — no properties (marker class)

---

### Focus & Interaction

#### `Focus`
Fires when the editor gains focus.

**EventArgs: `Syncfusion.Blazor.RichTextEditor.FocusEventArgs`**

| Property | Type | Description |
|---|---|---|
| `IsInteracted` | `bool` | `true` if focus was triggered by user interaction |

---

#### `Blur`
Fires when the editor loses focus.

**EventArgs: `BlurEventArgs`**

| Property | Type | Description |
|---|---|---|
| `IsInteracted` | `bool` | `true` if blur was triggered by user interaction |

---

#### `OnToolbarClick`
Fires when a toolbar item is clicked.

**EventArgs: `ToolbarClickEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the toolbar click action |
| `Item` | `Item?` | Data for the clicked toolbar item |
| `OriginalEvent` | `object?` | The underlying JavaScript event object |
| `RequestType` | `string?` | Type of operation requested by the toolbar action |

---

#### `SelectionChanged`
Fires when the selection range changes (not fired for a collapsed cursor).

**EventArgs: `SelectionChangedEventArgs`**

| Property | Type | Description |
|---|---|---|
| `EditorMode` | `EditorMode` | `HTML` or `Markdown` |
| `SelectedContent` | `string?` | Full HTML markup of the current non-empty selection |

---

### Value & Content

#### `ValueChange`
Fires on blur **or** at `SaveInterval`.

**EventArgs: `Syncfusion.Blazor.RichTextEditor.ChangeEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Value` | `string?` | Current HTML content of the editor |

---

#### `BeforePasteCleanup`
Fires before pasted content is sanitised.

**EventArgs: `PasteCleanupArgs`**

| Property | Type | Description |
|---|---|---|
| `Value` | `string?` | The pasted content — modify to transform paste output |
| `FilesData` | `List<FileInfo>?` | Image file data included in the paste |

---

#### `AfterPasteCleanup`
Fires after pasted content is sanitised.

**EventArgs: `PasteCleanupArgs`**

| Property | Type | Description |
|---|---|---|
| `Value` | `string?` | The cleaned paste content |
| `FilesData` | `List<FileInfo>?` | Image file data included in the paste |

---

### Toolbar Command Lifecycle

#### `OnActionBegin`
Fires before a toolbar command executes.

**EventArgs: `ActionBeginEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the action |
| `RequestType` | `string?` | Name of the command being initiated |
| `ExportValue` | `string?` | Content to export (set during export actions) |

---

#### `OnActionComplete`
Fires after a toolbar command completes.

**EventArgs: `ActionCompleteEventArgs`**

| Property | Type | Description |
|---|---|---|
| `EditorMode` | `string?` | `"HTML"` or `"Markdown"` |
| `RequestType` | `string?` | Name of the completed command |

---

### Dialog Events

#### `OnDialogOpen`
Fires before a dialog opens (cancellable via `args.Cancel`).

**EventArgs: `BeforeOpenEventArgs`** *(Syncfusion.Blazor.Popups — see Popups API)*

---

#### `DialogOpened`
Fires after a dialog finishes opening.

**EventArgs:** `DialogOpenEventArgs` — no properties (marker class)

---

#### `OnDialogClose`
Fires before a dialog closes (cancellable via `args.Cancel`).

**EventArgs: `BeforeCloseEventArgs`** *(Syncfusion.Blazor.Popups — see Popups API)*

---

#### `DialogClosed`
Fires after a dialog has fully closed.

**EventArgs: `DialogCloseEventArgs`**

| Property | Type | Description |
|---|---|---|
| `IsInteracted` | `bool` | `true` if the dialog was closed by user action |

---

### Quick Toolbar Events

#### `OnQuickToolbarOpen`
Fires before the quick toolbar opens.

**EventArgs: `BeforeQuickToolbarOpenArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to prevent the quick toolbar from opening |
| `PositionX` | `int` | Horizontal position *(deprecated)* |
| `PositionY` | `int` | Vertical position *(deprecated)* |

---

#### `QuickToolbarOpened`
Fires after the quick toolbar opens.

**EventArgs:** `QuickToolbarEventArgs` — no properties (marker class)

---

#### `QuickToolbarClosed`
Fires after the quick toolbar closes.

**EventArgs:** `QuickToolbarEventArgs` — no properties (marker class)

---

### Image Events

#### `OnImageSelected`
Fires when an image is selected or dragged into the insert dialog.

**EventArgs: `SelectedEventArgs`** *(Syncfusion.Blazor.Inputs — see Inputs API)*

---

#### `BeforeUploadImage`
Fires before image upload begins (cancellable).

**EventArgs: `ImageUploadingEventArgs`** *(inherits `FileUploadingEventArgs`)*

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the upload |
| `CurrentRequest` | `object?` | The XMLHttpRequest instance |
| `CustomFormData` | `object?` | Extra key/value pairs to append to the upload |
| `FilesData` | `List<FileInfo>?` | Files preparing for upload |

---

#### `OnImageUploadSuccess`
Fires when an image is successfully uploaded.

**EventArgs: `ImageSuccessEventArgs`** *(inherits `FileUploadSuccessEventArgs`)*

| Property | Type | Description |
|---|---|---|
| `DetectImageSource` | `ImageInputSource?` | How the image arrived: `Uploaded`, `Dropped`, or `Pasted` |
| `DetectMediaSource` | `MediaInputSource?` | How the media arrived: `Uploaded`, `Dropped`, or `Pasted` |
| `File` | `FileInfo?` | Uploaded file details |
| `Operation` | `string?` | Upload operation type |
| `Response` | `ResponseEventArgs?` | HTTP response information |
| `StatusText` | `string?` | Status message |

---

#### `OnImageUploadFailed`
Fires when an image upload fails.

**EventArgs: `ImageFailedEventArgs`** *(inherits `FileUploadFailedEventArgs`)*

| Property | Type | Description |
|---|---|---|
| `File` | `FileInfo?` | Details of the failed file |
| `Operation` | `string?` | Upload operation type |
| `Response` | `ResponseEventArgs?` | HTTP response information |
| `StatusText` | `string?` | Textual description of the failure |

---

#### `ImageUploadChange`
Fires when an image is uploaded and inserted into editor content.

**EventArgs: `ImageUploadChangeEventArgs`**

| Property | Type | Description |
|---|---|---|
| `ImageUrl` | `string?` | Image URL inserted into the editor |
| `Files` | `List<UploadFiles>?` | List of image files that were uploaded |

---

#### `OnImageRemoving`
Fires when an image is removed from the insert dialog.

**EventArgs: `RemovingEventArgs`** *(Syncfusion.Blazor.Inputs — see Inputs API)*

---

#### `OnImageDrop`
Fires when an image is being dropped into editor content (cancellable).

**EventArgs: `BeforeImageDropEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to block the image drop/insert |

---

#### `ImageDelete`
Fires when an image is deleted from editor content.

**EventArgs: `AfterImageDeleteEventArgs`** *(inherits `MediaDeletedEventArgs`)*

| Property | Type | Description |
|---|---|---|
| `Src` | `string?` | Source URL of the deleted image |

---

### Media Events

#### `FileSelected`
Fires when media is selected or dragged into the insert media dialog.

**EventArgs: `SelectedEventArgs`** *(Syncfusion.Blazor.Inputs — see Inputs API)*

---

#### `FileUploading`
Fires when selected media begins uploading to the server.

**EventArgs: `FileUploadingEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the upload |
| `CurrentRequest` | `object?` | The XMLHttpRequest instance |
| `CustomFormData` | `object?` | Extra key/value pairs to append to the upload |
| `FilesData` | `List<FileInfo>?` | Files preparing for upload |

---

#### `FileUploadSuccess`
Fires when media is successfully uploaded.

**EventArgs: `FileUploadSuccessEventArgs`**

| Property | Type | Description |
|---|---|---|
| `DetectImageSource` | `ImageInputSource?` | How the image arrived: `Uploaded`, `Dropped`, or `Pasted` |
| `DetectMediaSource` | `MediaInputSource?` | How the media arrived: `Uploaded`, `Dropped`, or `Pasted` |
| `File` | `FileInfo?` | Uploaded file details |
| `Operation` | `string?` | Upload operation type |
| `Response` | `ResponseEventArgs?` | HTTP response information |
| `StatusText` | `string?` | Status message |

---

#### `FileUploadFailed`
Fires when a media upload fails.

**EventArgs: `FileUploadFailedEventArgs`**

| Property | Type | Description |
|---|---|---|
| `File` | `FileInfo?` | Details of the failed file |
| `Operation` | `string?` | Upload operation type |
| `Response` | `ResponseEventArgs?` | HTTP response information |
| `StatusText` | `string?` | Textual description of the failure |

---

#### `FileUploadChange`
Fires when media is uploaded and inserted into editor content.

**EventArgs: `FileUploadChangeEventArgs`**

| Property | Type | Description |
|---|---|---|
| `FileUrl` | `string?` | Media URL inserted into the editor |
| `Files` | `List<UploadFiles>?` | List of media files that were uploaded |

---

#### `FileRemoving`
Fires when selected media is removed from the upload location.

**EventArgs: `RemovingEventArgs`** *(Syncfusion.Blazor.Inputs — see Inputs API)*

---

#### `OnMediaDrop`
Fires when media files are dropped into the editor (cancellable).

**EventArgs: `MediaDropEventArgs`** *(extends `DragEventArgs` — all standard drag/mouse properties inherited)*

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to prevent the drop; default `false` |
| `MediaType` | `string?` | `"Image"`, `"Video"`, `"Audio"`, or `null` |

---

#### `MediaDeleted`
Fires when media is deleted from editor content.

**EventArgs: `MediaDeletedEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Src` | `string?` | Source URL of the deleted media element |

---

### Resize Events

#### `OnResizeStart`
Fires when image resize begins.

**EventArgs: `ResizeArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the resize |
| `RequestType` | `string?` | Identifies this as a resize start event |

---

#### `OnResizeStop`
Fires when image resize ends.

**EventArgs: `ResizeArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the resize completion |
| `RequestType` | `string?` | Identifies this as a resize stop event |

---

### Toolbar Status Events

#### `UpdatedToolbarStatus`
Fires when toolbar item states are updated.

**EventArgs: `ToolbarStatusEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Html` | `HtmlStatus?` | Active/inactive state of each HTML toolbar item |
| `Markdown` | `MarkdownStatus?` | Active/inactive state of each Markdown toolbar item |
| `Redo` | `bool` | `true` if redo is currently available |
| `Undo` | `bool` | `true` if undo is currently available |

---

### Export Events

#### `OnExport`
Fires before PDF/Word export request is sent.

**EventArgs: `ExportingEventArgs`**

| Property | Type | Description |
|---|---|---|
| `ExportType` | `string?` | `"Pdf"` or `"Word"` |
| `RequestHeader` | `Dictionary<string, string>?` | Custom HTTP headers (e.g. `Authorization`) |
| `CustomFormData` | `Dictionary<string, string>?` | Additional form-data for the request body |

---

#### `OnExportFailure`
Fires when PDF/Word export HTTP request fails.

**EventArgs: `ExportFailureEventArgs`**

| Property | Type | Description |
|---|---|---|
| `ExportType` | `string?` | `"Word"` or `"Pdf"` |
| `Message` | `string?` | Error message, e.g. `"HTTP error! Status: 404"` |
| `StatusCode` | `int` | HTTP status code, e.g. `404`, `500` |

---

### Slash Menu Events

#### `SlashMenuItemSelecting`
Fires when a slash command item is selected (cancellable).

**EventArgs: `SlashMenuSelectEventArgs`**

| Property | Type | Description |
|---|---|---|
| `Cancel` | `bool` | Set `true` to prevent the slash command from executing |
| `ItemData` | `SlashMenuItemModel?` | The slash menu item being selected |

---

## Common Patterns

### Auto-save with `ValueChange`

```razor
<SfRichTextEditor SaveInterval="3000" AutoSaveOnIdle="true">
    <RichTextEditorEvents ValueChange="@SaveToDatabase" />
</SfRichTextEditor>

@code {
    private async Task SaveToDatabase(Syncfusion.Blazor.RichTextEditor.ChangeEventArgs args)
    {
        await MyService.SaveContentAsync(args.Value);
    }
}
```

### Cancel a Dialog with `OnDialogOpen`

```razor
<SfRichTextEditor>
    <RichTextEditorEvents OnDialogOpen="@PreventImageDialog" />
</SfRichTextEditor>

@code {
    private void PreventImageDialog(BeforeOpenEventArgs args)
    {
        // Cancel image insert dialog when in read-only context
        args.Cancel = true;
    }
}
```

### Delete Image from Server on `ImageDelete`

```razor
<SfRichTextEditor>
    <RichTextEditorEvents ImageDelete="@OnImageDeleted" />
</SfRichTextEditor>

@code {
    [Inject] private HttpClient Http { get; set; } = default!;

    private async Task OnImageDeleted(AfterImageDeleteEventArgs args)
    {
        var fileName = Path.GetFileName(args.Src);
        await Http.DeleteAsync($"/api/images/{fileName}");
    }
}
```

### Intercept Toolbar Actions with `OnActionBegin`

```razor
<SfRichTextEditor>
    <RichTextEditorEvents OnActionBegin="@LogAction" />
</SfRichTextEditor>

@code {
    private void LogAction(ActionBeginEventArgs args)
    {
        Console.WriteLine($"Toolbar action: {args.Name}");
    }
}
```

### Detect Text Selection with `SelectionChanged`

```razor
<SfRichTextEditor>
    <RichTextEditorEvents SelectionChanged="@OnSelection" />
</SfRichTextEditor>

@code {
    private void OnSelection(Syncfusion.Blazor.RichTextEditor.SelectionChangedEventArgs args)
    {
        // args.SelectedHTML contains the HTML of the selected range
    }
}
```

---

## Event Args Reference

Complete property listing for every event argument class in `Syncfusion.Blazor.RichTextEditor`.

### `ActionBeginEventArgs`

Used by: `OnActionBegin`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to cancel the action before it executes |
| `RequestType` | `string?` | get/set | Name of the command being initiated |
| `ExportValue` | `string?` | get/set | Content to export during export actions |

### `ActionCompleteEventArgs`

Used by: `OnActionComplete`

| Property | Type | Access | Description |
|---|---|---|---|
| `EditorMode` | `string?` | get | `"HTML"` or `"Markdown"` |
| `RequestType` | `string?` | get/set | Name of the completed command |

### `AfterImageDeleteEventArgs`

Used by: `ImageDelete` — inherits all members from `MediaDeletedEventArgs`

| Property | Type | Access | Description |
|---|---|---|---|
| `Src` | `string?` | get/set | Source URL of the deleted image (inherited) |

### `BeforeImageDropEventArgs`

Used by: `OnImageDrop`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to block the image drop/insert |

### `BeforeQuickToolbarOpenArgs`

Used by: `OnQuickToolbarOpen`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to prevent the quick toolbar from opening |
| `PositionX` | `int` | get/set | Horizontal position *(deprecated)* |
| `PositionY` | `int` | get/set | Vertical position *(deprecated)* |

### `BlurEventArgs`

Used by: `Blur`

| Property | Type | Access | Description |
|---|---|---|---|
| `IsInteracted` | `bool` | get/set | `true` if blur was triggered by user interaction |

### `Syncfusion.Blazor.RichTextEditor.ChangeEventArgs`

Used by: `ValueChange`

| Property | Type | Access | Description |
|---|---|---|---|
| `Value` | `string?` | get | Updated editor HTML content |

### `DestroyedEventArgs`

Used by: `Destroyed` — no own properties (marker class).

### `DialogCloseEventArgs`

Used by: `DialogClosed`

| Property | Type | Access | Description |
|---|---|---|---|
| `IsInteracted` | `bool` | get | `true` if the dialog was closed by the user |

### `DialogOpenEventArgs`

Used by: `DialogOpened` — no own properties (marker class).

### `ExportFailureEventArgs`

Used by: `OnExportFailure`

| Property | Type | Access | Description |
|---|---|---|---|
| `ExportType` | `string?` | get | `"Word"` or `"Pdf"` |
| `Message` | `string?` | get/set | Error message, e.g. `"HTTP error! Status: 404"` |
| `StatusCode` | `int` | get/set | HTTP status code, e.g. `404`, `500`; default `0` |

### `ExportingEventArgs`

Used by: `OnExport`

| Property | Type | Access | Description |
|---|---|---|---|
| `ExportType` | `string?` | get | `"Pdf"` or `"Word"` |
| `RequestHeader` | `Dictionary<string, string>?` | get/set | Custom HTTP headers for the export request (e.g. `Authorization`) |
| `CustomFormData` | `Dictionary<string, string>?` | get/set | Additional form-data key/value pairs for the request body |

### `FileUploadChangeEventArgs`

Used by: `FileUploadChange`

| Property | Type | Access | Description |
|---|---|---|---|
| `FileUrl` | `string?` | get/set | Media URL to insert into editor content |
| `Files` | `List<UploadFiles>?` | get/set | List of media files ready for upload |

### `FileUploadFailedEventArgs`

Used by: `FileUploadFailed` — base class for `ImageFailedEventArgs`

| Property | Type | Access | Description |
|---|---|---|---|
| `File` | `FileInfo?` | get | Details of the failed file |
| `Operation` | `string?` | get | Upload operation type |
| `Response` | `ResponseEventArgs?` | get/set | HTTP response information |
| `StatusText` | `string?` | get/set | Textual description of the failure |

### `FileUploadSuccessEventArgs`

Used by: `FileUploadSuccess` — base class for `ImageSuccessEventArgs`

| Property | Type | Access | Description |
|---|---|---|---|
| `DetectImageSource` | `ImageInputSource?` | get/set | How the image was provided: `Uploaded`, `Dropped`, or `Pasted` |
| `DetectMediaSource` | `MediaInputSource?` | get/set | How the media was provided: `Uploaded`, `Dropped`, or `Pasted` |
| `File` | `FileInfo?` | get/set | Uploaded file details |
| `Operation` | `string?` | get | Upload operation type |
| `Response` | `ResponseEventArgs?` | get/set | HTTP response information |
| `StatusText` | `string?` | get | Status message |

### `FileUploadingEventArgs`

Used by: `FileUploading` — base class for `ImageUploadingEventArgs`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to cancel the upload |
| `CurrentRequest` | `object?` | get/set | The XMLHttpRequest instance |
| `CustomFormData` | `object?` | get/set | Extra key/value pairs to append to the upload request |
| `FilesData` | `List<FileInfo>?` | get/set | Files preparing for upload |

### `Syncfusion.Blazor.RichTextEditor.FocusEventArgs`

Used by: `Focus`

| Property | Type | Access | Description |
|---|---|---|---|
| `IsInteracted` | `bool` | get/set | `true` if focus was triggered by user interaction |

### `ImageFailedEventArgs`

Used by: `OnImageUploadFailed` — inherits all properties from `FileUploadFailedEventArgs`; no own properties.

### `ImageSuccessEventArgs`

Used by: `OnImageUploadSuccess` — inherits all properties from `FileUploadSuccessEventArgs`; no own properties.

### `ImageUploadChangeEventArgs`

Used by: `ImageUploadChange`

| Property | Type | Access | Description |
|---|---|---|---|
| `ImageUrl` | `string?` | get/set | Image URL to insert into editor content |
| `Files` | `List<UploadFiles>?` | get/set | List of image files for upload |

### `ImageUploadingEventArgs`

Used by: `BeforeUploadImage` — inherits all properties from `FileUploadingEventArgs`; no own properties.

### `MediaDeletedEventArgs`

Used by: `MediaDeleted` — base class for `AfterImageDeleteEventArgs`

| Property | Type | Access | Description |
|---|---|---|---|
| `Src` | `string?` | get/set | Source URL of the deleted media element |

### `MediaDropEventArgs`

Used by: `OnMediaDrop` — extends `DragEventArgs` (all standard mouse/drag properties inherited)

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to prevent the media drop; default `false` |
| `MediaType` | `string?` | get/set | Type of dropped media: `"Image"`, `"Video"`, `"Audio"`, or `null` |

### `PasteCleanupArgs`

Used by: `BeforePasteCleanup` and `AfterPasteCleanup`

| Property | Type | Access | Description |
|---|---|---|---|
| `Value` | `string?` | get/set | The pasted content (modify to transform paste output) |
| `FilesData` | `List<FileInfo>?` | get/set | Image file data included in the paste |

### `QuickToolbarEventArgs`

Used by: `QuickToolbarOpened`, `QuickToolbarClosed` — no own properties (marker class).

### `ResizeArgs`

Used by: `OnResizeStart`, `OnResizeStop`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to cancel the resize action |
| `RequestType` | `string?` | get/set | Indicates whether this is a resize start or stop event |

### `SelectionChangedEventArgs`

Used by: `SelectionChanged`

| Property | Type | Access | Description |
|---|---|---|---|
| `EditorMode` | `EditorMode` | get | `HTML` or `Markdown` |
| `SelectedContent` | `string?` | get | Full HTML markup of the current non-empty selection |

### `SlashMenuSelectEventArgs`

Used by: `SlashMenuItemSelecting`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to prevent the slash command from executing |
| `ItemData` | `SlashMenuItemModel?` | get/set | The slash menu item being selected |

### `ToolbarClickEventArgs`

Used by: `OnToolbarClick`

| Property | Type | Access | Description |
|---|---|---|---|
| `Cancel` | `bool` | get/set | Set `true` to cancel the toolbar click action |
| `Item` | `Item?` | get/set | Data for the clicked toolbar item |
| `OriginalEvent` | `object?` | get/set | The underlying JavaScript event object |
| `RequestType` | `string?` | get | Type of operation requested by the toolbar action |

### `ToolbarStatusEventArgs`

Used by: `UpdatedToolbarStatus`

| Property | Type | Access | Description |
|---|---|---|---|
| `Html` | `HtmlStatus?` | get/set | Active/inactive status of each HTML toolbar item |
| `Markdown` | `MarkdownStatus?` | get/set | Active/inactive status of each Markdown toolbar item |
| `Redo` | `bool` | get/set | `true` if redo is currently available |
| `Undo` | `bool` | get/set | `true` if undo is currently available |