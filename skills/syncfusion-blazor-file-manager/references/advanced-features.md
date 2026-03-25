# Advanced Features in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Virtualization](#virtualization)
- [Pagination](#pagination)
- [Drag and Drop](#drag-and-drop)
- [Custom Filtering](#custom-filtering)
- [Nested Items](#nested-items)
- [Search Functionality](#search-functionality)
- [Accessibility](#accessibility)
- [Multiple File Selection](#multiple-file-selection)

## Overview

The FileManager provides advanced features for handling complex scenarios:

- **Virtualization** - Efficiently display thousands of items
- **Pagination** - Load items page-by-page
- **Drag & Drop** - Intuitive file organization with events
- **Custom Filtering** - Advanced search capabilities
- **Accessibility** - WCAG compliance
- **Multi-selection** - Bulk operations

## Virtualization

**Best for:** Large datasets (1000+ items)

From `virtualization.md`:

Virtualization enables dynamic loading of files without degrading performance. The component loads items based on viewport size.

### Enable Virtualization

```razor
<SfFileManager TValue="FileManagerDirectoryContent" 
               View="ViewType.Details" 
               EnableVirtualization="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Virtualization in LargeIcons View

```razor
<SfFileManager TValue="FileManagerDirectoryContent" 
               View="ViewType.LargeIcons" 
               EnableVirtualization="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Virtualization Limitations

**From `virtualization.md` (lines 40-60):**

- `SelectAllAsync()` method doesn't select all items with virtualization
- CTRL+A keyboard shortcut only selects visible items
- Selected items are not maintained while scrolling

### Best Practice

Use virtualization for:
- Browsing/exploring large file sets
- Performance-critical scenarios
- Mobile and low-bandwidth applications

Avoid for:
- Operations requiring all items selected
- Complex multi-item workflows

## Pagination

**Best for:** Large datasets with navigation control

From `pagination.md`:

### Pagination Properties

| Property | Type | Default | Purpose |
|--|--|--|--|
| `AllowPaging` | bool | false | Enable/disable pagination |
| `PageSize` | int | 100 | Items per page (configured via FileManagerPageSettings) |
| `CurrentPage` | int | 1 | Current page number (configured via FileManagerPageSettings) |
| `NumericItemsCount` | int | - | Number of numeric buttons in pager |
| `PageSizes` | int[] | null | Available page size options in dropdown |
| `Template` | RenderFragment | null | Custom pager template |

### Enable Pagination

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25"></FileManagerPageSettings>
</SfFileManager>
```

### Configure Page Size

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="50"></FileManagerPageSettings>
</SfFileManager>
```

### Set Initial Page

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25" CurrentPage="2"></FileManagerPageSettings>
</SfFileManager>
```

### Customize Numeric Page Buttons

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25" NumericItemsCount="5"></FileManagerPageSettings>
</SfFileManager>
```

### Page Sizes Dropdown

Allow users to change page size dynamically:

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25" PageSizes="@(new List<int>(){10, 25, 50})"></FileManagerPageSettings>
</SfFileManager>
```

### Programmatic Page Navigation

Use `GoToPageAsync()` method to navigate to specific pages:

```razor
<SfButton OnClick="GoToPage2">Go to Page 2</SfButton>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25"></FileManagerPageSettings>
</SfFileManager>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;

    private async Task GoToPage2()
    {
        await FileManager.GoToPageAsync(2);
    }
}
```

### Custom Pager Template

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25">
        <Template>
            <button @onclick="NavigateToPage">Go To Specific Page</button>
        </Template>
    </FileManagerPageSettings>
</SfFileManager>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;

    private async Task NavigateToPage()
    {
        await FileManager.GoToPageAsync(3);
    }
}
```

### Pagination Events

**PageChanging Event**

Triggered **before** the page changes. Use this to validate or prevent page navigation.

From `pagination.md`:

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 15px;">
    <SfButton OnClick="GoToPage2">Go to Page 2</SfButton>
    <span style="margin-left: 15px;">Current Page: @CurrentPage</span>
</div>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25"></FileManagerPageSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       PageChanging="OnPageChanging">
    </FileManagerEvents>
</SfFileManager>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;
    private int CurrentPage = 1;

    private async Task GoToPage2()
    {
        await FileManager.GoToPageAsync(2);
    }

    public async Task OnPageChanging(PageChangingEventArgs  args)
    {
        Console.WriteLine($"PageChanging: Moving from {args.CurrentPageIndex} to {args.NextPageIndex}");
        
        // Can cancel page change if validation fails
        if (args.NextPageIndex > 10)
        {
            args.Cancel = true;
            Console.WriteLine("Cannot navigate beyond page 10");
        }
    }
}
```

**PageChanged Event**

Triggered **after** the page changes successfully. Use this to perform actions after pagination.

From `pagination.md`:

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 15px;">
    <SfButton OnClick="GoToPage3">Go to Page 3</SfButton>
    <span style="margin-left: 15px;">Page Status: @PageStatus</span>
</div>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25"></FileManagerPageSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       PageChanged="OnPageChanged">
    </FileManagerEvents>
</SfFileManager>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;
    private string PageStatus = "Page 1";

    private async Task GoToPage3()
    {
        await FileManager.GoToPageAsync(3);
    }

    public async Task OnPageChanged(PageChangedEventArgs args)
    {
        CurrentPage = args.NextPageIndex;
        PageStatus = $"Page {args.NextPageIndex} loaded";
        
        Console.WriteLine($"PageChanged: Now on page {args.NextPageIndex}");
        Console.WriteLine($"Previous page was: {args.CurrentPageIndex}");
    }
}
```

**Combined Pagination Events**

Handle both events to implement comprehensive page navigation:

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 15px;">
    <SfButton OnClick="GoToPage2">Page 2</SfButton>
    <SfButton OnClick="GoToPage5">Page 5</SfButton>
    <span style="margin-left: 15px;">@PaginationMessage</span>
</div>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" AllowPaging="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerPageSettings PageSize="25"></FileManagerPageSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       PageChanging="OnPageChanging"
                       PageChanged="OnPageChanged">
    </FileManagerEvents>
</SfFileManager>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;
    private int CurrentPage = 1;
    private string PaginationMessage = "Current: Page 1";
    private bool IsNavigating = false;

    private async Task GoToPage2() => await FileManager.GoToPageAsync(2);
    private async Task GoToPage5() => await FileManager.GoToPageAsync(5);

    public async Task OnPageChanging(PageChangingEventArgs  args)
    {
        IsNavigating = true;
        PaginationMessage = $"Loading page {args.NextPageIndex}...";
        
        // Validate page range
        if (args.NextPageIndex > 20)
        {
            args.Cancel = true;
            PaginationMessage = "Maximum 20 pages available";
        }
    }

    public async Task OnPageChanged(PageChangedEventArgs args)
    {
        IsNavigating = false;
        CurrentPage = args.NextPageIndex;
        PaginationMessage = $"Current: Page {args.NextPageIndex}";
        
        // Perform post-navigation actions
        Console.WriteLine($"Successfully navigated to page {args.NextPageIndex}");
        
        // Example: Log analytics, update UI state, etc.
        await LogPageNavigation(args.CurrentPageIndex, args.NextPageIndex);
    }

    private async Task LogPageNavigation(int from, int to)
    {
        // Example logging
        Console.WriteLine($"User navigated from page {from} to page {to}");
    }
}
```

## Drag and Drop

**Best for:** Intuitive file organization and movement

From `drag-and-drop.md`:

### Enable Drag and Drop

Drag and drop is **enabled by default** for:
- Moving files between folders
- Uploading files from desktop
- Organizing items

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Control Drag and Drop

Use `AllowDragAndDrop` property on **SfFileManager** (not upload settings):

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowDragAndDrop="false">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Disable Drag and Drop

To disable drag-drop operations entirely:

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowDragAndDrop="false">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Drag and Drop Events

From `drag-and-drop.md`:

#### OnFileDragStart Event

Triggered when user starts dragging a file:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       OnFileDragStart="OnDragStart">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnDragStart(FileDragEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Started dragging {args.FileDetails.Count} items");
    }
}
```

#### OnFileDragStop Event

Triggered when user stops dragging:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       OnFileDragStop="OnDragStop">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnDragStop(FileDragEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine("Stopped dragging");
    }
}
```

#### FileDropped Event

Triggered when files are dropped:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       FileDropped="OnFileDropped">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnFileDropped(FileDragEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Dropped {args.FileDetails.Count} files at {args.DropPath}");
        
        // Can validate and cancel drop
        if (args.DropPath.Contains("ReadOnly"))
        {
            args.Cancel = true;
        }
    }
}
```

### Drag and Drop Move Operation

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       ItemsMoving="OnItemsMoving"
                       ItemsMoved="OnItemsMoved">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public async Task OnItemsMoving(ItemsMoveEventArgs<FileManagerDirectoryContent> args)
    {
        // Validate move operation
        if (args.TargetPath.Contains("ReadOnly"))
        {
            args.Cancel = true;
            Console.WriteLine("Cannot move to read-only folder");
        }
    }

    public async Task OnItemsMoved(ItemsMoveEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Moved {args.FileDetails.Count} items");
    }
}
```

## Custom Filtering

**Best for:** Advanced search and filtering scenarios

From `perform-custom-filtering.md`:

### Implement Custom Filtering

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Inputs

<SfTextBox Placeholder="Filter by name" @bind-Value="@FilterText" 
           ValueChanged="@OnFilterChanged">
</SfTextBox>

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       OnRead="OnRead">
    </FileManagerEvents>
</SfFileManager>

@code {
    private string FilterText = "";
    private List<FileManagerDirectoryContent> AllData = new();

    private async Task OnFilterChanged(string value)
    {
        FilterText = value;
    }

    private async Task OnRead(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        var allItems = await FetchAllItems(args.Path);
        
        // Apply filter
        var filtered = allItems.Where(x => 
            x.Name.Contains(FilterText, StringComparison.OrdinalIgnoreCase)
        ).ToList();

        FileManagerResponse<FileManagerDirectoryContent> response = 
            new FileManagerResponse<FileManagerDirectoryContent>()
        {
            CWD = allItems.FirstOrDefault(),
            Files = filtered
        };
        
        args.Response = response;
    }
}
```

### Filter by File Type

```razor
@code {
    private string SelectedFileType = "";

    private async Task OnRead(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        var allItems = await FetchAllItems(args.Path);
        
        // Filter by file type
        var filtered = !string.IsNullOrEmpty(SelectedFileType) 
            ? allItems.Where(x => x.Type == SelectedFileType).ToList()
            : allItems;

        FileManagerResponse<FileManagerDirectoryContent> response = 
            new FileManagerResponse<FileManagerDirectoryContent>()
        {
            CWD = allItems.FirstOrDefault(),
            Files = filtered
        };
        
        args.Response = response;
    }
}
```

### Filter by Size Range

```razor
@code {
    private long MinSize = 0;
    private long MaxSize = long.MaxValue;

    private async Task OnRead(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        var allItems = await FetchAllItems(args.Path);
        
        // Filter by size
        var filtered = allItems.Where(x => 
            x.Size >= MinSize && x.Size <= MaxSize
        ).ToList();

        FileManagerResponse<FileManagerDirectoryContent> response = 
            new FileManagerResponse<FileManagerDirectoryContent>()
        {
            CWD = allItems.FirstOrDefault(),
            Files = filtered
        };
        
        args.Response = response;
    }
}
```

## Nested Items

**Best for:** Hierarchical file structures

From `nested-items.md`:

### Nested Item Structure

The FileManager supports hierarchical navigation:

```csharp
var rootFolder = new FileManagerDirectoryContent
{
    Id = "1",
    ParentId = null,
    Name = "Root",
    HasChild = true,
    IsFile = false
};

var subfolder = new FileManagerDirectoryContent
{
    Id = "2",
    ParentId = "1",
    Name = "SubFolder",
    HasChild = true,
    IsFile = false
};

var file = new FileManagerDirectoryContent
{
    Id = "3",
    ParentId = "2",
    Name = "document.pdf",
    HasChild = false,
    IsFile = true
};
```

### Navigation Example

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       OnRead="OnRead">
    </FileManagerEvents>
</SfFileManager>

@code {
    private List<FileManagerDirectoryContent> AllItems = new();

    private async Task OnRead(ReadEventArgs<FileManagerDirectoryContent> args)
    {
        // Filter items by parent ID to show nested structure
        var parentId = args.Folder?.FirstOrDefault()?.Id;
        
        var items = AllItems.Where(x => 
            (string.IsNullOrEmpty(parentId) && string.IsNullOrEmpty(x.ParentId)) ||
            x.ParentId == parentId
        ).ToList();

        FileManagerResponse<FileManagerDirectoryContent> response = 
            new FileManagerResponse<FileManagerDirectoryContent>()
        {
            Files = items
        };
        
        args.Response = response;
    }
}
```

### RefreshLayoutAsync

**Purpose:** Refresh the FileManager layout when inside a container (e.g., Dialog) that changes size

**Signature:**

From `nested-items.md`:

```csharp
public async Task RefreshLayoutAsync()
```

**Use Case:** When FileManager is placed inside a Dialog component and the dialog is opened, the layout needs to be recalculated.

**Example: FileManager Inside Dialog**

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="@ShowDialog">Open File Manager</SfButton>

<SfDialog Width="800px" Height="600px" ShowCloseIcon="true" AllowDragging="true" 
          Visible="@IsDialogVisible" OnOpen="@OnDialogOpened">
    <DialogTemplates>
        <Header>File Manager</Header>
        <Content>
            <SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent">
                <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                         UploadUrl="/api/FileManager/Upload"
                                         DownloadUrl="/api/FileManager/Download"
                                         GetImageUrl="/api/FileManager/GetImage">
                </FileManagerAjaxSettings>
            </SfFileManager>
        </Content>
        <FooterTemplate>
            <SfButton OnClick="@HideDialog">Close</SfButton>
        </FooterTemplate>
    </DialogTemplates>
</SfDialog>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;
    private SfDialog Dialog;
    private bool IsDialogVisible = false;

    private void ShowDialog()
    {
        IsDialogVisible = true;
    }

    private void HideDialog()
    {
        IsDialogVisible = false;
    }

    private async Task OnDialogOpened()
    {
        // Refresh FileManager layout when dialog is opened
        if (FileManager != null)
        {
            await FileManager.RefreshLayoutAsync();
        }
    }
}
```

**Example: Dynamic Resizing**

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.Buttons

<div>
    <SfButton OnClick="@ShowDialog">Open File Manager</SfButton>
    <SfButton OnClick="@ToggleDialogSize">Resize Dialog</SfButton>
</div>

<SfDialog @ref="Dialog" 
          Width="@DialogWidth" 
          Height="@DialogHeight" 
          ShowCloseIcon="true" 
          AllowDragging="true"
          AllowResizing="true"
          OnResizeStop="@OnDialogResized">
    <DialogTemplates>
        <Header>File Manager in Dialog</Header>
        <Content>
            <SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent">
                <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                         UploadUrl="/api/FileManager/Upload"
                                         DownloadUrl="/api/FileManager/Download"
                                         GetImageUrl="/api/FileManager/GetImage">
                </FileManagerAjaxSettings>
            </SfFileManager>
        </Content>
    </DialogTemplates>
</SfDialog>

@code {
    private SfFileManager<FileManagerDirectoryContent> FileManager;
    private SfDialog Dialog;
    private string DialogWidth = "800px";
    private string DialogHeight = "600px";
    private bool IsDialogVisible = false;

    private void ShowDialog()
    {
        IsDialogVisible = true;
    }

    private void ToggleDialogSize()
    {
        DialogWidth = DialogWidth == "800px" ? "1000px" : "800px";
        DialogHeight = DialogHeight == "600px" ? "700px" : "600px";
    }

    private async Task OnDialogResized()
    {
        // Refresh layout when dialog is resized
        if (FileManager != null)
        {
            await FileManager.RefreshLayoutAsync();
        }
    }
}
```

## Search Functionality

**Best for:** Finding files across directories

### Implement Search

```razor
@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       Searching="OnSearching">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public async Task OnSearching(SearchEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Searching for: {args.SearchText}");
        // Implement search logic
    }
}
```

### Search with Wildcards

```csharp
// From file-operations.md
case "search":
    return this.operation.ToCamelCase(this.operation.Search(
        args.Path, 
        args.SearchString,      // "*pattern*" format
        args.ShowHiddenItems, 
        args.CaseSensitive
    ));
```

## Accessibility

**Best for:** WCAG compliance, inclusive design

From `accessibility.md`:

### Keyboard Navigation

The FileManager supports:

- **Tab** - Navigate between elements
- **Enter** - Open selected file/folder
- **Delete** - Delete selected items
- **Ctrl+A** - Select all items (visible items with virtualization)
- **Ctrl+C** - Copy items
- **Ctrl+X** - Cut items
- **Ctrl+V** - Paste items
- **F2** - Rename selected item
- **Shift+Click** - Range selection

### ARIA Attributes

```razor
<SfFileManager TValue="FileManagerDirectoryContent" 
               AriaLabel="File Manager Application">
    <!-- Configuration -->
</SfFileManager>
```

### Screen Reader Support

The FileManager provides:
- Proper heading levels
- ARIA labels for all controls
- Item descriptions
- Status messages

### High Contrast Support

```html
<!-- In App.razor, choose high contrast theme -->
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

## Multiple File Selection

**Best for:** Bulk operations

### Enable Multi-Selection

Multi-selection is enabled by default. Users can:

- **Click + Ctrl** - Select multiple items
- **Click + Shift** - Select range (with `EnableRangeSelection`)
- **Toolbar "SelectAll"** - Select all visible items

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowMultiSelection="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Enable Range Selection

From `multiple-file-selection.md`:

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

### Programmatic Multi-Selection

```razor
<SfButton OnClick="SelectMultiple">Select Multiple Files</SfButton>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent"
               AllowMultiSelection="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent>? FileManager;

    public async Task SelectMultiple()
    {
        // FileManager.SelectedItems contains all selected items
        var selectedCount = FileManager?.SelectedItems.Count ?? 0;
        Console.WriteLine($"Selected {selectedCount} items");
    }
}
```

### Bulk Operations

```razor
@code {
    public async Task DeleteSelected()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            // Perform operation on all selected items
            var selectedNames = FileManager.SelectedItems
                .Select(x => x.Name)
                .ToArray();
            
            Console.WriteLine($"Deleting: {string.Join(", ", selectedNames)}");
        }
    }

    public async Task DownloadSelected()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            await FileManager.DownloadFilesAsync(FileManager.SelectedItems);
        }
    }
}
```

### Virtualization with Selection Limitation

**From `virtualization.md` (lines 40-60):**

When virtualization is enabled:
- Cannot use `SelectAllAsync()` to select all items
- CTRL+A only selects visible items
- Selected state not preserved during scrolling

## Complete Advanced Features Example

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 20px;">
    <SfButton OnClick="SelectMultiple">Select All Visible</SfButton>
    <SfButton OnClick="DownloadSelected">Download Selected</SfButton>
    <span style="margin-left: 20px;">Selected: @(FileManager?.SelectedItems.Count ?? 0)</span>
</div>

<SfFileManager @ref="FileManager" 
               TValue="FileManagerDirectoryContent" 
               View="ViewType.Details"
               AllowMultiSelection="true"
               EnableRangeSelection="true"
               AllowDragAndDrop="true"
               EnableVirtualization="false"
               EnablePagination="true"
               PageSize="50">
    
    <FileManagerDetailsViewSettings>
        <FileManagerColumns>
            <FileManagerColumn Field="Name" HeaderText="Name" Width="200"></FileManagerColumn>
            <FileManagerColumn Field="Size" HeaderText="Size" Width="100"></FileManagerColumn>
            <FileManagerColumn Field="DateModified" HeaderText="Modified" Width="150"></FileManagerColumn>
        </FileManagerColumns>
    </FileManagerDetailsViewSettings>

    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       ItemsMoving="OnItemsMoving"
                       OnSearching="OnSearching"
                       FileDropped="OnFileDropped">
    </FileManagerEvents>
    
    <FileManagerUploadSettings DirectoryUpload="true">
    </FileManagerUploadSettings>

    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent>? FileManager;

    public async Task SelectMultiple()
    {
        Console.WriteLine($"Selected: {FileManager?.SelectedItems.Count}");
    }

    public async Task DownloadSelected()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            await FileManager.DownloadFilesAsync(FileManager.SelectedItems);
        }
    }

    public async Task OnItemsMoving(ItemsMoveEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Moving to {args.TargetPath}");
    }

    public async Task OnSearching(SearchEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Searching for: {args.SearchText}");
    }

    public void OnFileDropped(FileDragEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Dropped at {args.DropPath}");
    }
}
```

## Next Steps

- See [File Operations](file-operations.md) for search operation details
- Check [Events and Callbacks](events-and-callbacks.md) for event patterns
- Review [Upload and Download](upload-download.md) for bulk file operations
