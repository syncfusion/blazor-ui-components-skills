---
name: syncfusion-blazor-file-manager
description: Implement Syncfusion Blazor FileManager component for file browsing and management. Use this when creating hierarchical file systems, handling multiple file operations, implementing data binding patterns, or setting up cloud provider integration. This skill covers file operations, data binding, events handling, and customization options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "File Management"
---

# Implementing Syncfusion Blazor FileManager

## When to Use This Skill

Use this skill when you need to:

- **Create a file management interface** - Build web applications for browsing, uploading, downloading, and managing files
- **Set up cloud storage integration** - Connect to Azure Blob Storage, Amazon S3, Google Drive, SharePoint, or Firebase
- **Implement file operations** - Enable users to create, delete, rename, copy, move, search files and folders
- **Handle large file sets** - Display thousands of files with virtualization and pagination
- **Customize the UI** - Tailor toolbar, context menu, views, and navigation to your needs
- **Add drag-and-drop functionality** - Allow users to drag and drop files for upload or organization
- **Build accessible file systems** - Provide WCAG-compliant file management with keyboard navigation
- **Handle complex workflows** - Implement custom sorting, filtering, previewing, and multi-provider scenarios

## Component Overview

The **Syncfusion Blazor FileManager** is a comprehensive component for file and folder management. It provides:

- **Multiple file operations**: Read, Create, Delete, Rename, Search, Copy, Move, Upload, Download, Get Details, GetSelectedFiles
- **Flexible data binding**: AJAX settings, list objects, or injected services
- **Rich events system**: 20+ events including PageChanging/PageChanged for pagination
- **Multiple UI customizations**: Toolbar, context menu, view modes (Details, LargeIcons), NavigationPaneTemplate
- **File preview capabilities**: ShowThumbnail property for image and file previews
- **Cloud provider support**: Physical files, Azure, AWS S3, Google Drive, SharePoint, Firebase, SQL, FTP
- **Advanced features**: Virtualization, pagination with events, drag-and-drop, custom filtering, sorting, accessibility
- **Layout management**: RefreshLayoutAsync for dynamic resizing and nested component scenarios
- **High performance**: Handles large file sets with virtualization and lazy loading

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation for Web App, Server App, WASM App, and MAUI App
- NuGet package setup and service registration
- Basic component initialization and AJAX configuration
- File provider setup and wwwroot configuration
- First file manager instance in 5 minutes

### File Operations
📄 **Read:** [references/file-operations.md](references/file-operations.md)
- 11 core operations: Read, Create, Delete, Rename, Search, Details, Copy, Move, Upload, Download, GetImage
- Request/response data structures and parameters
- Operation examples with code from documentation
- Public methods: `DownloadFilesAsync(selectedItems)`, `GetSelectedFiles()`
- File preview capabilities with `ShowThumbnail` property
- Sorting functionality with SortBy and SortOrder properties

### Data Binding Patterns
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- AjaxSettings for remote data binding
- List objects with local data and OnRead event
- Injected service pattern for complex scenarios
- FileManagerDirectoryContent structure
- When to use each binding method

### Events and Callbacks
📄 **Read:** [references/events-and-callbacks.md](references/events-and-callbacks.md)
- 20+ file manager events with complete signatures
- Event lifecycle and timing
- Common event handler patterns
- BeforeDownload, BeforeImageLoad, OnFileOpen events
- ItemsDeleting, ItemsDeleted, FolderCreating, FolderCreated events
- Searching, Searched, ItemRenaming, ItemRenamed events

### Customization and UI
📄 **Read:** [references/customization.md](references/customization.md)
- Toolbar customization and item configuration
- Context menu customization
- View modes: Details View, LargeIcons View
- Navigation items customization with NavigationPaneTemplate
- Custom navigation pane templates with icons and metadata
- Styling and CSS customization
- Appearance and theme configuration

### File Providers and Storage
📄 **Read:** [references/file-providers.md](references/file-providers.md)
- Physical file provider (local file system)
- Azure Blob Storage configuration
- Amazon S3 cloud provider
- Google Drive integration
- SharePoint Online provider
- Firebase Real-time Database
- SQL Database provider
- FTP (File Transfer Protocol) provider

### Upload and Download
📄 **Read:** [references/upload-download.md](references/upload-download.md)
- File upload configuration and events
- Directory upload (folder upload)
- File download with single and ZIP support
- Large file handling
- Upload progress tracking
- Download prevention and validation
- BeforeDownload event for custom logic

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Virtualization for large file sets
- Pagination configuration with PageChanging and PageChanged events
- Drag and drop functionality
- Restrict drag-and-drop upload
- Custom filtering and search
- Nested items handling with RefreshLayoutAsync method
- Component integration in dialogs and containers
- Accessibility features (WCAG compliance)
- Keyboard navigation support
- Multiple file selection

## Quick Start Example

```razor
@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

**Server-side setup (Program.cs):**
```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
builder.Services.AddControllers();

app.UseRouting();
app.MapControllers();
```

**HTML setup (App.razor):**
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

## Common Patterns

### Pattern 1: Local Data with OnRead Event
```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" OnRead="OnReadAsync"></FileManagerEvents>
</SfFileManager>

@code {
    private async Task OnReadAsync(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        // Load data from your service
        var response = await YourService.GetFilesAsync(args.Path);
        args.Response = response;
    }
}
```

### Pattern 2: Cloud Storage with Azure Provider
```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/AzureProvider/FileOperations"
                             UploadUrl="/api/AzureProvider/Upload"
                             DownloadUrl="/api/AzureProvider/Download"
                             GetImageUrl="/api/AzureProvider/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Pattern 3: Programmatic Download
```razor
@ref="FileManager"

<SfButton OnClick="DownloadFiles">Download Selected</SfButton>
<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent">
    <!-- configuration -->
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;
    
    public async Task DownloadFiles()
    {
        await FileManager.DownloadFilesAsync(FileManager.SelectedItems);
    }
}
```

### Pattern 4: Custom Event Handling
```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       ItemsDeleting="OnItemsDeleting"
                       BeforeDownload="OnBeforeDownload">
    </FileManagerEvents>
</SfFileManager>

@code {
    public async Task OnItemsDeleting(ItemsDeleteEventArgs<FileManagerDirectoryContent> args)
    {
        // Validate before delete
        if (args.Files.Count > 10)
        {
            args.Cancel = true;
        }
    }

    public void OnBeforeDownload(BeforeDownloadEventArgs<FileManagerDirectoryContent> args)
    {
        // Custom download logic
    }
}
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `TValue` | Generic | FileManagerDirectoryContent | Data model type |
| `Url` | string | - | AJAX endpoint for file operations |
| `UploadUrl` | string | - | Upload endpoint |
| `DownloadUrl` | string | - | Download endpoint |
| `GetImageUrl` | string | - | Image preview endpoint |
| `SortBy` | string | Name | Sort field (Name, Size, DateModified, DateCreated) |
| `SortOrder` | SortOrder | Ascending | Sort direction (None, Ascending, Descending) |
| `View` | ViewType | LargeIcons | Display mode (Details, LargeIcons) |
| `EnableVirtualization` | bool | false | Enable virtual scrolling for large datasets |
| `RootPath` | string | - | Root directory path |
| `ShowHiddenItems` | bool | false | Show hidden files and folders |
| `EnablePagination` | bool | false | Enable pagination support |
| `PageSize` | int | 100 | Number of items per page |
| `DirectoryUpload` | bool | false | Allow directory upload |

## Common Use Cases

**1. Internal Document Management**
- Setup with physical file provider
- Use toolbar for CRUD operations
- Enable search and filtering
- Add event handlers for audit logging

**2. Cloud-Based Team Collaboration**
- Connect to Azure or AWS S3
- Enable drag-and-drop upload
- Add custom toolbar for sharing
- Implement access control via events

**3. Media Library**
- Enable image preview with GetImageUrl
- Use LargeIcons view for thumbnails
- Implement pagination for performance
- Add custom filtering by file type

**4. Backup and Archive System**
- Connect to multiple storage providers
- Enable download with compression
- Use virtualization for large datasets
- Add custom sorting and filtering

**5. Public File Sharing Portal**
- Restrict operations via event handlers
- Disable delete and rename
- Enable download and preview only
- Customize navigation and UI

## Next Steps

1. **Choose your setup**: Start with [Getting Started](references/getting-started.md) for your platform
2. **Pick your data source**: Read [Data Binding Patterns](references/data-binding.md) to choose AJAX, local, or service
3. **Configure file operations**: Check [File Operations](references/file-operations.md) for available actions
4. **Handle events**: Review [Events and Callbacks](references/events-and-callbacks.md) for required handlers
5. **Customize UI**: See [Customization](references/customization.md) for toolbar and themes
6. **Choose storage**: Select provider in [File Providers](references/file-providers.md)
