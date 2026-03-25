# Data Binding in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Data Binding Methods](#data-binding-methods)
- [AJAX Settings](#ajax-settings)
- [List Objects with Events](#list-objects-with-events)
- [Injected Service Pattern](#injected-service-pattern)
- [FileManagerDirectoryContent](#filemanagerdirectorycontent)
- [Comparison and Selection](#comparison-and-selection)

## Overview

The Blazor FileManager supports multiple data binding methods to load file and folder data. Each method serves different scenarios:

- **AjaxSettings** - For remote REST APIs
- **List Objects** - For local data with event handlers
- **Injected Service** - For dependency-injected services with complex logic

**From `data-binding.md` (lines 1-50):**

The FileManager uses the `SfFileManager` component with generic type `TValue` (typically `FileManagerDirectoryContent`) to load and manage file data from various sources.

## Data Binding Methods

### Method 1: AjaxSettings (Remote REST API)

**Best for:** External APIs, cloud services, distributed systems

**Advantages:**
- Simple configuration
- Decoupled from Blazor component
- Scales well for large datasets
- Works with any backend service

**Example:**

From `data-binding.md` (lines 75-95):

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/SampleData/FileOperations">
    </FileManagerAjaxSettings>
</SfFileManager>
```

**With Upload/Download:**

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/SampleData/FileOperations"
                             UploadUrl="/api/SampleData/Upload"
                             DownloadUrl="/api/SampleData/Download"
                             GetImageUrl="/api/SampleData/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Method 2: List Objects with Events

**Best for:** Local data, server-side rendering, integrated services

**Advantages:**
- Full control over data
- Event-driven architecture
- Easy debugging
- No separate API needed

**Basic Pattern:**

From `data-binding.md` (lines 120-180):

```razor
@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" OnRead="OnReadAsync"></FileManagerEvents>
</SfFileManager>

@code
{
    List<FileManagerDirectoryContent> Data { get; set; }

    protected override void OnInitialized()
    {
        Data = GetData();
    }

    private async Task OnReadAsync(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        string path = args.Path;
        List<FileManagerDirectoryContent> fileDetails = args.Folder;
        FileManagerResponse<FileManagerDirectoryContent> response = new FileManagerResponse<FileManagerDirectoryContent>();
        
        if (path == "/")
        {
            string ParentId = Data
                .Where(x => string.IsNullOrEmpty(x.ParentId))
                .Select(x => x.Id).First();
            response.CWD = Data
                .Where(x => string.IsNullOrEmpty(x.ParentId)).First();
            response.Files = Data
                .Where(x => x.ParentId == ParentId).ToList();
        }
        else
        {
            var childItem = fileDetails.Count > 0 && fileDetails[0] != null ? fileDetails[0] : Data
                .Where(x => x.FilterPath == path).First();
            response.CWD = childItem;
            response.Files = Data
                .Where(x => x.ParentId == childItem.Id).ToList();
        }
        
        await Task.Yield();
        args.Response = response;
    }

    private List<FileManagerDirectoryContent> GetData()
    {
        List<FileManagerDirectoryContent> data = new List<FileManagerDirectoryContent>();
        data.Add(new FileManagerDirectoryContent()
        {
            CaseSensitive = false,
            DateCreated = new DateTime(2022, 1, 2),
            DateModified = new DateTime(2022, 2, 3),
            FilterPath = "",
            FilterId = "",
            HasChild = true,
            Id = "0",
            IsFile = false,
            Name = "Files",
            ParentId = null,
            ShowHiddenItems = false,
            Size = 1779448,
            Type = "folder"
        });
        // Add more items...
        return data;
    }
}
```

### Method 3: Injected Service

**Best for:** Complex scenarios, shared business logic, testability

**Advantages:**
- Dependency injection
- Reusable service
- Easy to test
- Separates concerns

**Service Setup:**

From `data-binding.md` (lines 250-350):

```csharp
using Syncfusion.Blazor.FileManager;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

public class FileManagerService
{
    List<FileManagerDirectoryContent> Data = new List<FileManagerDirectoryContent>();
    
    public FileManagerService()
    {
        InitializeData();
    }

    public async Task<FileManagerResponse<FileManagerDirectoryContent>> ReadAsync(
        string path, 
        List<FileManagerDirectoryContent> fileDetails)
    {
        FileManagerResponse<FileManagerDirectoryContent> response = 
            new FileManagerResponse<FileManagerDirectoryContent>();
        
        if (path == "/")
        {
            string ParentId = Data
                .Where(x => string.IsNullOrEmpty(x.ParentId))
                .Select(x => x.Id).First();
            response.CWD = Data
                .Where(x => string.IsNullOrEmpty(x.ParentId)).First();
            response.Files = Data
                .Where(x => x.ParentId == ParentId).ToList();
        }
        else
        {
            var id = fileDetails.Count > 0 && fileDetails[0] != null ? 
                fileDetails[0].Id : 
                Data.Where(x => x.FilterPath == path).Select(x => x.ParentId).First();
            
            response.CWD = Data
                .Where(x => x.Id == (fileDetails.Count > 0 && fileDetails[0] != null ? 
                    fileDetails[0].Id : id)).First();
            response.Files = Data
                .Where(x => x.ParentId == (fileDetails.Count > 0 && fileDetails[0] != null ? 
                    fileDetails[0].Id : id)).ToList();
        }
        
        await Task.Yield();
        return await Task.FromResult(response);
    }

    public async Task<FileManagerResponse<FileManagerDirectoryContent>> DeleteAsync(
        string path, 
        string[] names, 
        FileManagerDirectoryContent[] files)
    {
        FileManagerResponse<FileManagerDirectoryContent> response = 
            new FileManagerResponse<FileManagerDirectoryContent>();
        
        // Delete logic...
        
        return await Task.FromResult(response);
    }

    private void InitializeData()
    {
        // Initialize with file data...
    }
}
```

**Component Usage:**

From `data-binding.md` (lines 370-420):

```razor
@using Syncfusion.Blazor.FileManager
@inject FileManagerService FileManagerService

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       OnRead="OnReadAsync" 
                       ItemsDeleting="ItemsDeletingAsync" 
                       FolderCreating="FolderCreatingAsync" 
                       Searching="SearchingAsync" 
                       ItemRenaming="ItemRenamingAsync" 
                       ItemsMoving="ItemsMovingAsync" 
                       ItemsUploaded="ItemsUploadedAsync" 
                       BeforeDownload="BeforeDownload" 
                       BeforeImageLoad="BeforeImageLoadAsync">
    </FileManagerEvents>
</SfFileManager>

@code{
    public async Task OnReadAsync(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        args.Response = await FileManagerService.ReadAsync(args.Path, args.Folder.ToList());
    }

    public async Task ItemsDeletingAsync(ItemsDeleteEventArgs<FileManagerDirectoryContent> args)
    {
        string[] names = args.Files.Select(x => x.Name).ToArray();
        args.Response = await FileManagerService.DeleteAsync(args.Path, names, args.Files.ToArray());
    }

    public async Task FolderCreatingAsync(FolderCreateEventArgs<FileManagerDirectoryContent> args)
    {
        args.Response = await FileManagerService.CreateAsync(args.Path, args.FolderName);
    }

    public async Task SearchingAsync(SearchEventArgs<FileManagerDirectoryContent> args)
    {
        args.Response = await FileManagerService.SearchAsync(args.Path, args.SearchText, false, false);
    }

    public async Task ItemRenamingAsync(ItemRenameEventArgs<FileManagerDirectoryContent> args)
    {
        args.Response = await FileManagerService.RenameAsync(args.Path, args.File.Name, args.NewName, false, false, args.File);
    }

    public async Task ItemsMovingAsync(ItemsMoveEventArgs<FileManagerDirectoryContent> args)
    {
        string[] names = args.Files.Select(x => x.Name).ToArray();
        if (args.IsCopy)
        {
            args.Response = await FileManagerService.CopyAsync(args.Path, args.TargetPath, names, args.TargetData, args.Files.ToArray());
        }
        else
        {
            args.Response = await FileManagerService.MoveAsync(args.Path, args.TargetPath, names, args.TargetData, args.Files.ToArray());
        }
    }
}
```

**Register Service in Program.cs:**

From `data-binding.md` (lines 435-445):

```csharp
using YourNamespace;

builder.Services.AddSyncfusionBlazor();
builder.Services.AddSingleton<FileManagerService>();
```

## AJAX Settings

### FileManagerAjaxSettings Properties

**From `data-binding.md` (lines 60-75):**

| Property | Type | Required | Purpose |
|--|--|--|--|
| `Url` | string | Yes | Endpoint for file operations |
| `UploadUrl` | string | No | Upload endpoint |
| `DownloadUrl` | string | No | Download endpoint |
| `GetImageUrl` | string | No | Image preview endpoint |

### Simple AJAX Example

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Full AJAX Example with All Endpoints

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

## List Objects with Events

### Event-Based Data Binding

The FileManager provides events for each file operation. By handling these events, you can provide data from any source.

### Event Reference

| Event | Trigger | Response Property |
|--|--|--|
| OnRead | Folder opened or data refreshed | args.Response |
| ItemsDeleting | Before deletion | args.Response |
| ItemsDeleted | After deletion | args.Response |
| FolderCreating | Before folder creation | args.Response |
| FolderCreated | After folder creation | args.Response |
| Searching | Search initiated | args.Response |
| Searched | Search completed | args.Response |
| ItemRenaming | Before rename | args.Response |
| ItemRenamed | After rename | args.Response |
| ItemsMoving | Before copy/move | args.Response |
| ItemsMoved | After copy/move | args.Response |

### Using Multiple Events

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       OnRead="OnRead"
                       ItemsDeleting="OnDelete"
                       FolderCreating="OnCreate"
                       ItemRenaming="OnRename"
                       Searching="OnSearch">
    </FileManagerEvents>
</SfFileManager>

@code {
    async Task OnRead(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        // Handle read event
    }

    async Task OnDelete(ItemsDeleteEventArgs<FileManagerDirectoryContent> args)
    {
        // Handle delete event
    }

    async Task OnCreate(FolderCreateEventArgs<FileManagerDirectoryContent> args)
    {
        // Handle create event
    }

    async Task OnRename(ItemRenameEventArgs<FileManagerDirectoryContent> args)
    {
        // Handle rename event
    }

    async Task OnSearch(SearchEventArgs<FileManagerDirectoryContent> args)
    {
        // Handle search event
    }
}
```

## FileManagerDirectoryContent

### Structure and Properties

**From `file-operations.md` (lines 35-60):**

```csharp
public class FileManagerDirectoryContent
{
    // Identity
    public string Id { get; set; }
    public string ParentId { get; set; }
    public string Name { get; set; }
    
    // Paths
    public string FilterPath { get; set; }
    public string FilterId { get; set; }
    
    // Metadata
    public bool IsFile { get; set; }
    public bool HasChild { get; set; }
    public string Type { get; set; }
    
    // Timestamps
    public DateTime DateCreated { get; set; }
    public DateTime DateModified { get; set; }
    
    // Size
    public long Size { get; set; }
    
    // Options
    public bool CaseSensitive { get; set; }
    public bool ShowHiddenItems { get; set; }
}
```

### Creating Data Items

```csharp
var folder = new FileManagerDirectoryContent()
{
    Id = "1",
    ParentId = null,
    Name = "Root",
    FilterPath = "",
    HasChild = true,
    IsFile = false,
    Type = "folder",
    DateCreated = DateTime.Now,
    DateModified = DateTime.Now,
    Size = 0
};

var file = new FileManagerDirectoryContent()
{
    Id = "2",
    ParentId = "1",
    Name = "document.pdf",
    FilterPath = "/Root/",
    HasChild = false,
    IsFile = true,
    Type = ".pdf",
    DateCreated = DateTime.Now,
    DateModified = DateTime.Now,
    Size = 102400
};
```

## Comparison and Selection

### When to Use Each Method

| Method | Use When | Pros | Cons |
|--|--|--|--|
| **AjaxSettings** | Using external API or microservice | Simple, scalable, decoupled | Network overhead |
| **List Objects** | Data in same application, server-side rendering | Full control, no network calls | Limited scalability |
| **Injected Service** | Complex logic, dependency injection needed | Testable, reusable, organized | More setup |

### Recommendation

- **Small datasets** (<1000 items) - List Objects or Injected Service
- **Medium datasets** (1000-10000 items) - Injected Service with pagination
- **Large datasets** (>10000 items) - AjaxSettings with server-side filtering
- **Cloud storage** - AjaxSettings with cloud provider endpoints

## Next Steps

- See [Events and Callbacks](events-and-callbacks.md) for event handler patterns
- Check [File Providers](file-providers.md) for specific provider setup
- Review [Upload and Download](upload-download.md) for file transfer handling
