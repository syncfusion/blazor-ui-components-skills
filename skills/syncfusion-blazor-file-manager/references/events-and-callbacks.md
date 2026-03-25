# Events and Callbacks in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Lifecycle Events](#lifecycle-events)
- [File Operation Events](#file-operation-events)
- [UI Interaction Events](#ui-interaction-events)
- [Download and Preview Events](#download-and-preview-events)
- [Event Handling Patterns](#event-handling-patterns)

## Overview

The FileManager provides 20+ events that fire during different operations and user interactions. Events enable you to:

- Validate operations before they execute
- Customize behavior for specific scenarios
- Log or audit file operations
- Cancel operations when needed
- Respond to user interactions

**From `file-operations.md` (lines 1050-1100):**

All events are bound using the `FileManagerEvents` component with the `TValue` generic parameter.

## Lifecycle Events

### Created

**Trigger:** When FileManager component is initialized

**Arguments:** None

**Usage:**

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" Created="OnCreated"></FileManagerEvents>
</SfFileManager>

@code {
    public void OnCreated()
    {
        Console.WriteLine("FileManager created");
        // Initialize custom logic
    }
}
```

### Destroyed

**Trigger:** When FileManager component is destroyed

**Arguments:** None

**Usage:**

```razor
@code {
    public void OnDestroyed()
    {
        Console.WriteLine("FileManager destroyed");
        // Cleanup logic
    }
}
```

## File Operation Events

### ItemsDeleting

**Trigger:** Before items are deleted

**Arguments:**
- `Path` (string) - Directory path
- `Files` (List<FileManagerDirectoryContent>) - Items to delete
- `Cancel` (bool) - Set to true to prevent deletion

**Example: Prevent large deletions**

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       ItemsDeleting="OnItemsDeleting">
    </FileManagerEvents>
</SfFileManager>

@code {
    public async Task OnItemsDeleting(ItemsDeleteEventArgs<FileManagerDirectoryContent> args)
    {
        if (args.Files.Count > 10)
        {
            args.Cancel = true;
            // Show user warning
        }
    }
}
```

### ItemsDeleted

**Trigger:** After items are successfully deleted

**Arguments:** Response object with details

**Usage:**

```razor
@code {
    public async Task OnItemsDeleted(ItemsDeletedEventArgs<FileManagerDirectoryContent> args)
    {
        // Log deletion
        Console.WriteLine($"Deleted {args.Response.Files.Count} items");
    }
}
```

### FolderCreating

**Trigger:** Before a new folder is created

**Arguments:**
- `Path` (string) - Parent directory path
- `FolderName` (string) - New folder name
- `Cancel` (bool) - Set to true to prevent creation

**Example: Name validation**

```razor
@code {
    public async Task OnFolderCreating(FolderCreateEventArgs<FileManagerDirectoryContent> args)
    {
        if (args.FolderName.Contains(" "))
        {
            args.Cancel = true;
            // Show error: "Spaces not allowed in folder names"
        }
    }
}
```

### FolderCreated

**Trigger:** After folder is successfully created

**Arguments:** Response with new folder details

**Usage:**

```razor
@code {
    public async Task OnFolderCreated(FolderCreatedEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Created folder: {args.Response.Files[0].Name}");
    }
}
```

### ItemRenaming

**Trigger:** Before item is renamed

**Arguments:**
- `Path` (string) - Item directory path
- `File` (FileManagerDirectoryContent) - File to rename
- `NewName` (string) - New name
- `Cancel` (bool) - Set to true to prevent rename

**Example: Name pattern validation**

```razor
@code {
    public async Task OnItemRenaming(ItemRenameEventArgs<FileManagerDirectoryContent> args)
    {
        // Prevent renaming system files
        if (args.File.Name.StartsWith("."))
        {
            args.Cancel = true;
        }
    }
}
```

### ItemRenamed

**Trigger:** After item is successfully renamed

**Arguments:** Response with renamed item details

### Searching

**Trigger:** When user searches for files

**Arguments:**
- `Path` (string) - Search path
- `SearchText` (string) - Search query
- `CaseSensitive` (bool) - Case sensitivity

**Usage:**

```razor
@code {
    public async Task OnSearching(SearchEventArgs<FileManagerDirectoryContent> args)
    {
        // Custom search logic
        Console.WriteLine($"Searching for: {args.SearchText}");
    }
}
```

### Searched

**Trigger:** After search completes

**Arguments:** Response with search results

### ItemsMoving

**Trigger:** Before items are copied or moved

**Arguments:**
- `Path` (string) - Source path
- `TargetPath` (string) - Destination path
- `IsCopy` (bool) - True if copy, false if move
- `Files` (List<FileManagerDirectoryContent>) - Items being moved
- `Cancel` (bool) - Set to true to prevent operation

**Example: Destination validation**

```razor
@code {
    public async Task OnItemsMoving(ItemsMoveEventArgs<FileManagerDirectoryContent> args)
    {
        // Prevent moving to read-only location
        if (args.TargetPath.Contains("ReadOnly"))
        {
            args.Cancel = true;
        }
    }
}
```

### ItemsMoved

**Trigger:** After items are copied or moved successfully

**Arguments:** Response with operation details

## UI Interaction Events

### OnFileOpen

**Trigger:** Before file or folder is opened

**Arguments:**
- `File` (FileManagerDirectoryContent) - Item being opened
- `Cancel` (bool) - Set to true to prevent opening

**Example: Prevent opening certain file types**

```razor
@code {
    public async Task OnFileOpen(FileOpenEventArgs<FileManagerDirectoryContent> args)
    {
        if (args.File.Type == ".exe")
        {
            args.Cancel = true;
        }
    }
}
```

### OnFileLoad

**Trigger:** Before file is rendered in the view

**Arguments:**
- `File` (FileManagerDirectoryContent) - File being rendered
- `Module` (string) - View type (Details, LargeIcon, etc.)

**Usage:**

```razor
@code {
    public void OnFileLoad(FileLoadEventArgs<FileManagerDirectoryContent> args)
    {
        // Customize file display
        Console.WriteLine($"Loading {args.File.Name} in {args.Module} view");
    }
}
```

### BeforePopupOpen

**Trigger:** Before dialog box opens

**Arguments:**
- `DialogType` (string) - Type of dialog
- `Cancel` (bool) - Set to true to prevent opening

**Example: Prevent delete dialog for certain conditions**

```razor
@code {
    public void OnBeforePopupOpen(BeforePopupOpenCloseEventArgs args)
    {
        if (args.DialogType == "Delete")
        {
            // Custom validation
        }
    }
}
```

### BeforePopupClose

**Trigger:** Before dialog box closes

**Arguments:**
- `DialogType` (string) - Type of dialog

### PopupOpened

**Trigger:** After dialog opens

**Arguments:** Dialog details

### PopupClosed

**Trigger:** After dialog closes

**Arguments:** Dialog details

### OnSend

**Trigger:** Before HTTP request is sent to server

**Arguments:**
- Request customization options

**Usage:**

```razor
@code {
    public void OnSend(BeforeSendEventArgs args)
    {
        // Add custom headers
        // Modify request parameters
    }
}
```

### OnSuccess

**Trigger:** After HTTP request succeeds

**Arguments:**
- Response data
- Status code

**Example: Log successful operations**

```razor
@code {
    public void OnSuccess(SuccessEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Operation successful: {args.Result}");
    }
}
```

### OnError

**Trigger:** When HTTP request fails

**Arguments:**
- `Error` (object) - Error details
- `Exception` (Exception) - Exception if any

**Example: Handle errors gracefully**

```razor
@code {
    public void OnError(FailureEventArgs args)
    {
        if (args.Error != null)
        {
            Console.WriteLine($"Error: {args.Error.ToString()}");
        }
    }
}
```

## Download and Preview Events

### BeforeDownload

**Trigger:** Before download request is sent

**Arguments:**
- `DownloadData` (DownloadData) - Files to download
- `Cancel` (bool) - Set to true to prevent download

**Example: Validate download permissions**

From `file-operations.md` (lines 680-710):

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             DownloadUrl="/api/FileManager/Download">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       BeforeDownload="OnBeforeDownload">
    </FileManagerEvents>
</SfFileManager>

@code {
    public void OnBeforeDownload(BeforeDownloadEventArgs<FileManagerDirectoryContent> args)
    {
        // Prevent downloading certain file types
        foreach (var file in args.DownloadData.DownloadFileDetails)
        {
            if (file.IsFile && file.Type == ".exe")
            {
                args.Cancel = true;
                break;
            }
        }
    }
}
```

### BeforeImageLoad

**Trigger:** Before image is loaded for preview

**Arguments:**
- `ImageUrl` (string) - Image URL
- `FileDetails` (FileManagerDirectoryContent) - Image file details
- `Cancel` (bool) - Set to true to prevent loading

**Example: Custom image loading**

```razor
@code {
    public async Task OnBeforeImageLoad(BeforeImageLoadEventArgs<FileManagerDirectoryContent> args)
    {
        // Custom image URL transformation
        if (!args.ImageUrl.StartsWith("https"))
        {
            args.ImageUrl = "https://secure-server" + args.ImageUrl;
        }
    }
}
```

## Upload Events

### ItemsUploading

**Trigger:** Before files are uploaded

**Arguments:**
- `Files` (List<FileInfo>) - Files being uploaded
- `Cancel` (bool) - Set to true to prevent upload

**Example: File validation before upload**

```razor
@code {
    public async Task OnItemsUploading(ItemsUploadingEventArgs<FileManagerDirectoryContent> args)
    {
        const long maxSize = 10 * 1024 * 1024; // 10 MB
        
        foreach (var file in args.Files)
        {
            if (file.File.Size > maxSize)
            {
                args.Cancel = true;
                break;
            }
        }
    }
}
```

### ItemsUploaded

**Trigger:** After files are successfully uploaded

**Arguments:**
- `Files` (List<FileInfo>) - Uploaded files
- `Path` (string) - Upload destination

**Example: Log uploads**

```razor
@code {
    public async Task OnItemsUploaded(ItemsUploadedEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Uploaded {args.Files.Count} files to {args.Path}");
    }
}
```

### UploadListCreated

**Trigger:** Before each file is rendered in upload dialog

**Arguments:**
- Upload file details

**Usage:**

```razor
@code {
    public void OnUploadListCreated(UploadListCreateArgs args)
    {
        // Customize upload list display
    }
}
```

## Event Handling Patterns

### Pattern 1: Multiple Events in Single Component

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       Created="OnCreated"
                       ItemsDeleting="OnItemsDeleting"
                       FolderCreating="OnFolderCreating"
                       BeforeDownload="OnBeforeDownload"
                       OnError="OnError">
    </FileManagerEvents>
</SfFileManager>

@code {
    public void OnCreated() { /* ... */ }
    public async Task OnItemsDeleting(ItemsDeleteEventArgs<FileManagerDirectoryContent> args) { /* ... */ }
    public async Task OnFolderCreating(FolderCreateEventArgs<FileManagerDirectoryContent> args) { /* ... */ }
    public void OnBeforeDownload(BeforeDownloadEventArgs<FileManagerDirectoryContent> args) { /* ... */ }
    public void OnError(FailureEventArgs args) { /* ... */ }
}
```

### Pattern 2: Validation and Cancellation

```razor
@code {
    public async Task OnItemsDeleting(ItemsDeleteEventArgs<FileManagerDirectoryContent> args)
    {
        // Validate
        if (!CanDelete(args.Files))
        {
            args.Cancel = true;
            return;
        }

        // Log
        LogOperation("Delete", args.Files);
    }

    private bool CanDelete(List<FileManagerDirectoryContent> files)
    {
        return files.All(f => !f.Name.StartsWith("system_"));
    }

    private void LogOperation(string operation, List<FileManagerDirectoryContent> files)
    {
        // Log to database or file
    }
}
```

### Pattern 3: Error Handling

```razor
@code {
    public void OnError(FailureEventArgs args)
    {
        if (args.Error != null)
        {
            // Log error
            Console.WriteLine($"FileManager Error: {args.Error}");

            // Show user-friendly message
            ShowErrorNotification("An error occurred. Please try again.");
        }
    }

    private void ShowErrorNotification(string message)
    {
        // Display notification to user
    }
}
```

## Next Steps

- See [File Operations](file-operations.md) for operation details
- Check [Upload and Download](upload-download.md) for upload-specific event handling
- Review [Advanced Features](advanced-features.md) for additional event usage patterns
