# File Operations in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Core File Operations](#core-file-operations)
- [Request and Response Format](#request-and-response-format)
- [Operation Details](#operation-details)
- [Sorting and Filtering](#sorting-and-filtering)
- [Selection Properties and Events](#selection-properties-and-events)
- [Public Methods](#public-methods)
- [Complete Operation Examples](#complete-operation-examples)

## Overview

The FileManager supports 11 core file operations through a unified request-response model. Each operation sends an action name and parameters to the server, which processes the request and returns the result.

**From `file-operations.md` (lines 20-50):**

All operations follow the same pattern:
1. Client sends request with action, path, and parameters
2. Server processes the operation
3. Server returns response with status and data
4. Client updates UI based on response

## Core File Operations

### 1. Read

**Purpose:** Read files and folders from a given path

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "read" |
| path | string | Relative path from which data is read |
| showHiddenItems | boolean | Show or hide hidden items |
| data | FileManagerDirectoryContent | Current directory details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| cwd | FileManagerDirectoryContent | Current working directory details |
| files | FileManagerDirectoryContent[] | List of files and folders |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 55-70):

```csharp
{
    action: "read",
    path: "/",
    showHiddenItems: false,
    data: []
}
```

**Example Response:**

```csharp
{
    cwd: {
        name: "Download",
        size: 0,
        dateModified: "2019-02-28T03:48:19.8319708+00:00",
        dateCreated: "2019-02-27T17:36:15.812193+00:00",
        hasChild: false,
        isFile: false,
        type: "",
        filterPath: "\\Download\\"
    },
    files: [
        {
            name: "Sample Work Sheet.xlsx",
            size: 6172,
            dateModified: "2019-02-27T17:23:50.9651206+00:00",
            dateCreated: "2019-02-27T17:36:15.8151955+00:00",
            hasChild: false,
            isFile: true,
            type: ".xlsx",
            filterPath: "\\Download\\"
        }
    ],
    error: null
}
```

### 2. Create

**Purpose:** Create a new folder in the specified path

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "create" |
| path | string | Path where folder will be created |
| name | string | Name of the new folder |
| data | FileManagerDirectoryContent | Current directory details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| files | FileManagerDirectoryContent[] | Details of created folder |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 105-125):

```csharp
{
    action: "create",
    data: [{ /* directory details */ }],
    name: "HelloFolder",
    path: "/"
}
```

### 3. Delete

**Purpose:** Delete file or folder from the server

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "delete" |
| path | string | Path where items are located |
| names | string[] | Names of items to delete |
| data | FileManagerDirectoryContent | Details of item being deleted |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| files | FileManagerDirectoryContent[] | Details of deleted item(s) |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 190-205):

```csharp
{
    action: "delete",
    path: "/HelloFolder/",
    names: ["file.txt"],
    data: []
}
```

### 4. Rename

**Purpose:** Rename a file or folder

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "rename" |
| path | string | Path where item is located |
| name | string | Current name of item |
| newName | string | New name for item |
| data | FileManagerDirectoryContent | Item details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| files | FileManagerDirectoryContent[] | Details of renamed item |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 130-155):

```csharp
{
    action: "rename",
    data: [{ /* item details */ }],
    newname: "seaview.jpg",
    name: "seaviews.jpg",
    path: "/Pictures/Nature/"
}
```

### 5. Search

**Purpose:** Search for files and folders matching a search string

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "search" |
| path | string | Directory path to search |
| searchString | string | String to search for |
| showHiddenItems | boolean | Include hidden items |
| caseSensitive | boolean | Case-sensitive search |
| data | FileManagerDirectoryContent | Current directory details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| cwd | FileManagerDirectoryContent | Current working directory |
| files | FileManagerDirectoryContent[] | Search results |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 220-245):

```csharp
{
    action: "search",
    path: "/",
    searchString: "*nature*",
    showHiddenItems: false,
    caseSensitive: false,
    data: []
}
```

### 6. Details

**Purpose:** Get detailed information about file(s) or folder(s)

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "details" |
| path | string | Path where items are located |
| names | string[] | Names of items to get details |
| data | FileManagerDirectoryContent | Item details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| details | FileManagerDirectoryContent | Detailed information |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 280-300):

```csharp
{
    action: "details",
    path: "/FileContents/",
    names: ["All Files"],
    data: []
}
```

### 7. Copy

**Purpose:** Copy files or folders to target location

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "copy" |
| path | string | Source path |
| names | string[] | Files to copy |
| targetPath | string | Destination path |
| data | FileManagerDirectoryContent | File details |
| renameFiles | string[] | Renamed file details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| cwd | FileManagerDirectoryContent | Current working directory |
| files | FileManagerDirectoryContent[] | Copied file details |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 345-365):

```csharp
{
    action: "copy",
    path: "/",
    names: ["6.png"],
    renameFiles: ["6.png"],
    targetPath: "/Videos/"
}
```

### 8. Move

**Purpose:** Move (cut/paste) files or folders to target location

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "move" |
| path | string | Source path |
| names | string[] | Files to move |
| targetPath | string | Destination path |
| data | FileManagerDirectoryContent | File details |
| renameFiles | string[] | Renamed file details |

**Response:**

| Parameter | Type | Description |
|--|--|--|
| cwd | FileManagerDirectoryContent | Current working directory |
| files | FileManagerDirectoryContent[] | Moved file details |
| error | ErrorDetails | Error information if any |

**Example Request:**

From `file-operations.md` (lines 390-410):

```csharp
{
    action: "move",
    path: "/",
    names: ["6.png"],
    renameFiles: ["6.png"],
    targetPath: "/Videos/"
}
```

### 9. Upload

**Purpose:** Upload files to specified directory

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "Save" |
| path | string | Upload destination path |
| uploadFiles | IList<IFormFile> | Files being uploaded |
| size | long | File size |

**Response:** Empty string on success, error object on failure

**Example Request:**

From `file-operations.md` (lines 550-600):

```csharp
{
    uploadFiles: (binary),
    path: /,
    action: Save
}
```

### 10. Download

**Purpose:** Download files (single or multiple as ZIP)

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "download" |
| path | string | File path |
| names | string[] | Files to download |
| data | FileManagerDirectoryContent | File details |

**Response:** File stream

**Example Request:**

From `file-operations.md` (lines 625-655):

```csharp
{
    action: "download",
    path: "/",
    names: ["1.png"],
    data: [{ /* file details */ }]
}
```

### 11. GetImage

**Purpose:** Get image file for preview

**Request Parameters:**

| Parameter | Type | Description |
|--|--|--|
| action | string | "GetImage" |
| path | string | Image file path |
| id | string | Image file ID |

**Response:** Image file stream

**Example Request:**

From `file-operations.md` (lines 740-760):

```csharp
{
    action: "GetImage",
    path: "/1.png",
    id: "image_1"
}
```

## Request and Response Format

### FileManagerDirectoryContent Structure

**From `file-operations.md` (lines 800-850):**

Used in both requests and responses to represent files and folders.

| Field | Type | Description |
|--|--|--|
| name | string | File or folder name |
| dateCreated | string | UTC date string when created |
| dateModified | string | UTC date string when last modified |
| filterPath | string | Relative path to file or folder |
| hasChild | boolean | Whether folder has children |
| isFile | boolean | Whether item is file (true) or folder (false) |
| size | number | File size in bytes |
| type | string | File extension |

### ErrorDetails Structure

| Field | Type | Description |
|--|--|--|
| code | string | Error code |
| message | string | Error message description |
| fileExists | string[] | List of duplicate file names (if applicable) |

## Operation Details

### Complete API Reference Table

| Operation | HTTP Method | Endpoint | Purpose | Key Parameters |
|--|--|--|--|--|
| Read | POST | /api/FileManager/FileOperations | List directory contents | path, showHiddenItems |
| Create | POST | /api/FileManager/FileOperations | Create new folder | path, name |
| Delete | POST | /api/FileManager/FileOperations | Delete items | path, names |
| Rename | POST | /api/FileManager/FileOperations | Rename item | path, name, newName |
| Search | POST | /api/FileManager/FileOperations | Search files | path, searchString, caseSensitive |
| Details | POST | /api/FileManager/FileOperations | Get item details | path, names |
| Copy | POST | /api/FileManager/FileOperations | Copy items | path, targetPath, names |
| Move | POST | /api/FileManager/FileOperations | Move items | path, targetPath, names |
| Upload | POST | /api/FileManager/Upload | Upload files | path, uploadFiles |
| Download | POST | /api/FileManager/Download | Download files | path, names |
| GetImage | POST | /api/FileManager/GetImage | Preview image | path, id |

## Sorting and Filtering

### SortBy Property

**From `file-operations.md` (lines 460-490):**

Controls which field is used for sorting.

```csharp
public string SortBy { get; set; } // Default: "Name"
```

**Valid values:**
- Name (default)
- Size
- DateModified
- DateCreated

### SortOrder Property

Controls the sort direction.

```csharp
public SortOrder SortOrder { get; set; } // Default: Ascending
```

**Valid values:**
- None - No sorting
- Ascending - Alphabetically or numerically ascending
- Descending - Alphabetically or numerically descending

### Example: Custom Sorting

```razor
<SfFileManager TValue="FileManagerDirectoryContent" 
               SortBy="Size" 
               SortOrder="SortOrder.Descending">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Custom Sort Comparer

**From `file-operations.md` (lines 510-550):**

For complex sorting logic, implement IComparer<Object>:

```razor
<SfFileManager TValue="FileManagerDirectoryContent" SortComparer="new NaturalSortComparer()">
    <FileManagerDetailsViewSettings>
        <FileManagerColumns>
            <FileManagerColumn Field="Name" HeaderText="Name" SortComparer="new NaturalSortComparer()"></FileManagerColumn>
            <FileManagerColumn Field="DateModified" Format="MM/dd/yyyy h:mm tt" HeaderText="Modified"></FileManagerColumn>
            <FileManagerColumn Field="Size" HeaderText="Size"></FileManagerColumn>
        </FileManagerColumns>
    </FileManagerDetailsViewSettings>
</SfFileManager>

@code {
    public class NaturalSortComparer : IComparer<Object>
    {
        public int Compare(Object x, Object y)
        {
            // Implementation for natural sorting (1, 2, 10 instead of 1, 10, 2)
            // ... sorting logic ...
            return comparisonResult;
        }
    }
}
```

## Selection Properties and Events

### Selection Properties

| Property | Type | Default | Purpose |
|--|--|--|--|
| `AllowMultiSelection` | bool | true | Allow selecting multiple files and folders |
| `EnableRangeSelection` | bool | false | Allow Shift+Click range selection |
| `SelectedItems` | IList<FileManagerDirectoryContent> | empty | Currently selected items |

### Configure Selection

```razor
<SfFileManager TValue="FileManagerDirectoryContent" 
               AllowMultiSelection="true"
               EnableRangeSelection="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### FileSelected Event

Triggered when a file or folder is selected:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" FileSelected="OnFileSelected">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnFileSelected(FileSelectEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"File selected: {args.FileDetails.Count} items");
        foreach (var file in args.FileDetails)
        {
            Console.WriteLine($"  - {file.Name} ({file.Size} bytes)");
        }
    }
}
```

### FileSelection Event

Triggered when file selection changes:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" FileSelection="OnFileSelection">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnFileSelection(FileSelectionEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"File selection changed: {args.FileDetails.Count} items");
        
        // Can cancel or modify selection
        args.Cancel = false;
    }
}
```

### Get Selected Items

```razor
<SfButton OnClick="GetSelection">Get Selected Items</SfButton>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" AllowMultiSelection="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;

    public void GetSelection()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            foreach (var item in FileManager.SelectedItems)
            {
                Console.WriteLine($"Selected: {item.Name}");
            }
        }
    }
}
```

## Public Methods

### DownloadFilesAsync

**Purpose:** Download selected files programmatically

**Signature:**

From `file-operations.md` (lines 600-620):

```csharp
public async Task DownloadFilesAsync(IEnumerable<FileManagerDirectoryContent> selectedItems)
```

**Parameters:**
- `selectedItems` - Collection of files to download

**Returns:** Task (completes when download starts)

**Example:**

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="DownloadSelectedFiles">Download</SfButton>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;
    
    public async Task DownloadSelectedFiles()
    {
        if (FileManager.SelectedItems.Count > 0)
        {
            await FileManager.DownloadFilesAsync(FileManager.SelectedItems);
        }
    }
}
```

### GetSelectedFiles

**Purpose:** Retrieve details of currently selected files and folders

**Signature:**

From `multiple-file-selection.md` (lines 45-55):

```csharp
public List<FileManagerDirectoryContent> GetSelectedFiles()
```

**Returns:** List of selected FileManagerDirectoryContent items

**Example:**

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="GetCurrentSelection">Get Selected Files</SfButton>
<div>Selected: @SelectedCount files</div>

<SfFileManager @ref="FileManager" AllowMultiSelection="true" TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" FileSelection="OnFileSelection">
    </FileManagerEvents>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;
    List<FileManagerDirectoryContent> SelectedFiles = new List<FileManagerDirectoryContent>();
    int SelectedCount = 0;

    public void GetCurrentSelection()
    {
        SelectedFiles = FileManager.GetSelectedFiles();
        SelectedCount = SelectedFiles.Count;
        
        // Process selected files
        foreach (var file in SelectedFiles)
        {
            Console.WriteLine($"Selected: {file.Name} ({file.Size} bytes)");
        }
    }

    public void OnFileSelection(FileSelectionEventArgs<FileManagerDirectoryContent> args)
    {
        var selectedDetails = args.FileDetails;
        Console.WriteLine($"File selected: {selectedDetails.Name}");
    }
}
```

## Display File Thumbnails

**Purpose:** Show thumbnail previews for files in the FileManager

**Property:** `ShowThumbnail`

From `previewing-files.md`:

```razor
@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent" ShowThumbnail="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

**ShowThumbnail with File Preview:**

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.PdfViewerServer

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" ShowThumbnail="true" AllowMultiSelection="false">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" OnFileOpen="OpenFilePreview">
    </FileManagerEvents>
</SfFileManager>

<SfDialog Width="800px" Height="600px" ShowCloseIcon="true" AllowDragging="true" Visible="@IsDialogVisible">
    <DialogTemplates>
        <Header>Preview: @DialogTitle</Header>
        <Content>
            @if (CurrentFileType == ".pdf")
            {
                <SfPdfViewerServer DocumentPath="@DocumentPath" Height="500px" Width="100%"></SfPdfViewerServer>
            }
            else
            {
                <p>Preview not available for this file type.</p>
            }
        </Content>
    </DialogTemplates>
</SfDialog>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;
    bool IsDialogVisible = false;
    string DialogTitle = "";
    string DocumentPath = "";
    string CurrentFileType = "";

    private void OpenFilePreview(FileOpenEventArgs<FileManagerDirectoryContent> args)
    {
        if (args.FileDetails.Type == ".pdf")
        {
            DialogTitle = args.FileDetails.Name;
            CurrentFileType = ".pdf";
            DocumentPath = "wwwroot\\Files" + args.FileDetails.FilterPath + args.FileDetails.Name;
            IsDialogVisible = true;
        }
    }
}
```

## Complete Operation Examples

### Example 1: Implementing Read Operation

From `getting-started-with-web-app.md` (lines 280-300):

```csharp
case "read":
    // reads the file(s) or folder(s) from the given path
    return this.operation.ToCamelCase(this.operation.GetFiles(args.Path, args.ShowHiddenItems));
```

### Example 2: Implementing Delete Operation

From `file-operations.md` (lines 280-295):

```csharp
case "delete":
    // deletes the selected file(s) or folder(s) from the given path
    return this.operation.ToCamelCase(this.operation.Delete(args.Path, args.Names));
```

### Example 3: Implementing Copy Operation

From `file-operations.md` (lines 395-410):

```csharp
case "copy":
    // copies the selected file(s) or folder(s) from a path and pastes into target path
    return this.operation.ToCamelCase(this.operation.Copy(args.Path, args.TargetPath, 
        args.Names, args.RenameFiles, args.TargetData));
```

### Example 4: Implementing Move Operation

From `file-operations.md` (lines 445-460):

```csharp
case "move":
    // cuts the selected file(s) or folder(s) from a path and pastes into target path
    return this.operation.ToCamelCase(this.operation.Move(args.Path, args.TargetPath, 
        args.Names, args.RenameFiles, args.TargetData));
```

## Next Steps

- See [Events and Callbacks](events-and-callbacks.md) for handling operation results
- Check [Upload and Download](upload-download.md) for advanced upload/download scenarios
- Review [Advanced Features](advanced-features.md) for search and filtering
