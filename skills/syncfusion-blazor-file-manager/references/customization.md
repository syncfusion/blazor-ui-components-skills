# Customization and UI in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Toolbar Customization](#toolbar-customization)
- [Toolbar Events](#toolbar-events)
- [Context Menu](#context-menu)
- [Context Menu Events](#context-menu-events)
- [View Modes](#view-modes)
- [Multiple Selection](#multiple-selection)
- [Navigation Customization](#navigation-customization)
- [Styling and Appearance](#styling-and-appearance)

## Overview

The FileManager provides extensive customization options for:

- **Toolbar** - Customize buttons, visibility, and custom items
- **Context Menu** - Customize right-click menu items
- **Views** - Switch between Details and LargeIcons view
- **Multi-selection** - Enable range selection for bulk operations
- **Navigation** - Customize breadcrumb and sidebar navigation
- **Styling** - Apply custom CSS and themes

## Toolbar Customization

### Toolbar Visibility

From `toolbar.md`:

Control toolbar visibility using the `Visible` property:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerToolbarSettings Visible="true">
    </FileManagerToolbarSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

To hide the toolbar:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerToolbarSettings Visible="false">
    </FileManagerToolbarSettings>
    <!-- Other configuration -->
</SfFileManager>
```

### Toolbar Items

The toolbar shows different buttons based on selection state:

| Selection Count | Left Toolbar | Right Toolbar |
|--|--|--|
| 0 (none) | SortBy, Refresh, NewFolder, Upload | View, Details |
| 1 (single) | Delete, Download, Rename | Count, View, Details |
| >1 (multiple) | Delete, Download | Count, View, Details |

### Built-in Toolbar Items

- **NewFolder** - Creates a new folder
- **Upload** - Enables file upload
- **Delete** - Deletes selected items
- **Download** - Downloads selected items
- **Rename** - Renames selected item
- **SortBy** - Sort by Name, Size, Date Modified, Date Created
- **Refresh** - Refreshes the file list
- **View** - Toggle between Details and LargeIcons
- **Details** - Shows file details panel
- **Selection** - Select all/none
- **Cut** - Cut selected items
- **Copy** - Copy selected items
- **Paste** - Paste items

### Configure Toolbar Items

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerToolbarSettings ToolbarItems="@Items">
    </FileManagerToolbarSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    private List<ToolBarItemModel> Items = new List<ToolBarItemModel>()
    {
        new ToolBarItemModel() { Name = "NewFolder" },
        new ToolBarItemModel() { Name = "Upload" },
        new ToolBarItemModel() { Name = "Delete" },
        new ToolBarItemModel() { Name = "Download" },
        new ToolBarItemModel() { Name = "Rename" },
        new ToolBarItemModel() { Name = "SortBy" },
        new ToolBarItemModel() { Name = "Refresh" }
    };
}
```

### Custom Toolbar Items

From `add-custom-tool-bar.md`:

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerToolbarSettings ToolbarItems="@Items">
        <FileManagerCustomToolbarItems>
            <FileManagerCustomToolbarItem Name="CustomAction">
                <Template>
                    <SfButton CssClass="e-tbar-btn e-tbtn-txt" OnClick="OnCustomActionClick">
                        <span class="e-tbar-btn-text">Custom Action</span>
                    </SfButton>
                </Template>
            </FileManagerCustomToolbarItem>
        </FileManagerCustomToolbarItems>
    </FileManagerToolbarSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    private List<ToolBarItemModel> Items = new List<ToolBarItemModel>()
    {
        new ToolBarItemModel() { Name = "NewFolder" },
        new ToolBarItemModel() { Name = "Upload" },
        new ToolBarItemModel() { Name = "Delete" },
        new ToolBarItemModel() { Name = "Download" },
        new ToolBarItemModel() { Name = "Refresh" },
        new ToolBarItemModel() { Name = "CustomAction" }
    };

    private void OnCustomActionClick()
    {
        Console.WriteLine("Custom action clicked");
    }
}
```

## Toolbar Events

### ToolbarCreated Event

From `toolbar.md`:

Triggered before the toolbar items are created:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" ToolbarCreated="OnToolbarCreated">
    </FileManagerEvents>
</SfFileManager>

@code {
    public void OnToolbarCreated(ToolbarCreateEventArgs args)
    {
        Console.WriteLine("Toolbar created");
        // Add custom toolbar items or modify existing ones
    }
}
```

### ToolbarItemClicked Event

Triggered when a toolbar item is clicked:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" ToolbarItemClicked="OnToolbarItemClicked">
    </FileManagerEvents>
</SfFileManager>

@code {
    public void OnToolbarItemClicked(ToolbarClickEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Toolbar item clicked: {args.Item.Name}");
    }
}
```

## Context Menu

### Context Menu Structure

From `context-menu.md`:

The FileManager context menu has three configurable sections:

| Menu Type | Properties |
|--|--|
| **File** | Items displayed when right-clicking a file |
| **Folder** | Items displayed when right-clicking a folder |
| **Layout** | Items displayed on empty space |

### Configure Context Menu

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerContextMenuSettings File="@FileMenuItems" 
                                     Folder="@FolderMenuItems"
                                     Layout="@LayoutMenuItems">
    </FileManagerContextMenuSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public string[] FileMenuItems = new string[] { "Delete", "Download", "Rename", "|", "Details" };
    public string[] FolderMenuItems = new string[] { "Open", "Rename", "Delete", "|", "Details" };
    public string[] LayoutMenuItems = new string[] { "NewFolder", "Upload", "Refresh", "|", "View", "SortBy" };
}
```

### Default Menu Items

| Context | Available Items |
|--|--|
| **File Context** | Open, Delete, Rename, Download, Details |
| **Folder Context** | Open, Delete, Rename, Download, Details |
| **Layout Context** | NewFolder, Upload, Refresh, View, SortBy, Details, SelectAll |

### Add Custom Context Menu Items

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerContextMenuSettings File="@FileMenuItems" 
                                     Folder="@FolderMenuItems">
    </FileManagerContextMenuSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" MenuOpened="OnMenuOpened">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public string[] FileMenuItems = new string[] { "Open", "Delete", "Rename", "Download", "Custom" };
    public string[] FolderMenuItems = new string[] { "Open", "Rename", "Delete", "Custom" };

    public void OnMenuOpened(MenuOpenEventArgs<FileManagerDirectoryContent> args)
    {
        // Add icon to custom menu item
        foreach (var item in args.Items)
        {
            if (item.Text == "Custom")
            {
                item.IconCss = "e-icons e-fe-tick";
            }
        }
    }
}
```

### Enable/Disable Context Menu Items

```razor
@code {
    public void OnMenuOpened(MenuOpenEventArgs<FileManagerDirectoryContent> args)
    {
        bool isFile = args.FileDetails.Any(detail => detail.IsFile);

        foreach (var item in args.Items)
        {
            if (item.Text == "Cut")
            {
                item.Disabled = isFile;  // Disable Cut for files
            }
        }
    }
}
```

### Show/Hide Context Menu Items

```razor
@code {
    public void OnMenuOpened(MenuOpenEventArgs<FileManagerDirectoryContent> args)
    {
        bool isFile = args.FileDetails.Any(file => file.IsFile);

        foreach (var item in args.Items)
        {
            if (item.Text == "Cut")
            {
                item.Hidden = !isFile;  // Hide Cut for folders
            }
        }
    }
}
```

## Context Menu Events

### MenuOpened Event

From `context-menu.md`:

Triggered before the context menu is displayed:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" MenuOpened="OnMenuOpened">
    </FileManagerEvents>
</SfFileManager>

@code {
    public void OnMenuOpened(MenuOpenEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Menu opened for items: {args.FileDetails.Count}");
    }
}
```

### OnMenuClick Event

Triggered when a context menu item is clicked:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" OnMenuClick="OnMenuClick">
    </FileManagerEvents>
</SfFileManager>

@code {
    public void OnMenuClick(MenuClickEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Menu item clicked: {args.Item.Text}");
    }
}
```

## View Modes

### Available View Types

From `views.md`:

| ViewType | Description | Use Case |
|--|--|--|
| **Details** | Table layout with columns | Large file sets, detailed info needed |
| **LargeIcons** | Large thumbnail icons | Visual browsing, image galleries |

**Note:** FileManager only has Details and LargeIcons view types. GridView is not available.

### Setting View Mode

```razor
<SfFileManager TValue="FileManagerDirectoryContent" View="ViewType.Details">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Details View with Custom Columns

From `views.md`:

```razor
<SfFileManager TValue="FileManagerDirectoryContent" View="ViewType.Details">
    <FileManagerDetailsViewSettings>
        <FileManagerColumns>
            <FileManagerColumn Field="Name" HeaderText="Name" Width="200"></FileManagerColumn>
            <FileManagerColumn Field="Size" HeaderText="Size" Width="100"></FileManagerColumn>
            <FileManagerColumn Field="DateModified" 
                              HeaderText="Modified" 
                              Format="MM/dd/yyyy h:mm tt"
                              Width="150">
            </FileManagerColumn>
        </FileManagerColumns>
    </FileManagerDetailsViewSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Custom Column Template

```razor
<FileManagerDetailsViewSettings>
    <FileManagerColumns>
        <FileManagerColumn Field="Name" HeaderText="Filename" Width="250">
            <Template>
                @{
                    var data = (context as FileManagerDirectoryContent);
                    <div>
                        <span class="e-list-icon e-fe-file"></span>
                        <span>@data?.Name</span>
                    </div>
                }
            </Template>
        </FileManagerColumn>
        <FileManagerColumn Field="Size" HeaderText="File Size" Width="120">
            <Template>
                @{
                    var data = (context as FileManagerDirectoryContent);
                    <div>@FormatBytes(data?.Size ?? 0)</div>
                }
            </Template>
        </FileManagerColumn>
    </FileManagerColumns>
</FileManagerDetailsViewSettings>

@code {
    private string FormatBytes(long bytes)
    {
        string[] sizes = { "B", "KB", "MB", "GB" };
        double len = bytes;
        int order = 0;
        while (len >= 1024 && order < sizes.Length - 1)
        {
            order++;
            len = len / 1024;
        }
        return $"{len:0.##} {sizes[order]}";
    }
}
```

### LargeIcons View with Custom Template

From `views.md`:

```razor
<SfFileManager TValue="FileManagerDirectoryContent" CssClass="e-fm-template-sample">
    <ChildContent>
        <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                 UploadUrl="/api/FileManager/Upload"
                                 DownloadUrl="/api/FileManager/Download"
                                 GetImageUrl="/api/FileManager/GetImage">
        </FileManagerAjaxSettings>
    </ChildContent>
    <LargeIconsTemplate Context="item">
        @if (item is not null)
        {
            <div class="custom-icon-card">
                <div class="file-name" title="@item.Name">@item.Name</div>
                <div class="@GetFileTypeCssClass(item)"></div>
                <div class="file-date">@item.DateModified.ToString("MM/dd/yyyy")</div>
            </div>
        }
    </LargeIconsTemplate>
</SfFileManager>

@code {
    private string GetFileTypeCssClass(FileManagerDirectoryContent item)
    {
        if (!item.IsFile)
        {
            return $"e-list-icon e-fe-folder";
        }
        var ext = System.IO.Path.GetExtension(item.Name)?.TrimStart('.') ?? string.Empty;
        var type = ExtensionIconClassMap.GetValueOrDefault(ext, "unknown");
        return $"e-list-icon e-fe-{type}";
    }
    
    private static readonly Dictionary<string, string> ExtensionIconClassMap = 
        new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
    {
        { "jpg", "image" }, { "jpeg", "image" }, { "png", "image" }, { "gif", "image" },
        { "mp3", "music" }, { "wav", "music" }, { "mp4", "video" }, { "avi", "video" },
        { "xlsx", "xlsx" }, { "xls", "xlsx" }, { "pptx", "pptx" }, { "ppt", "pptx" },
        { "zip", "zip" }, { "txt", "txt" }, { "pdf", "pdf" }, { "doc", "doc" }, { "docx", "docx" }
    };
}
```

## Multiple Selection

### Enable Multiple Selection

The FileManager has `AllowMultiSelection` property for bulk operations:

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowMultiSelection="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Range Selection

From `multiple-file-selection.md`:

Enable `EnableRangeSelection` for selecting ranges with Shift+Click:

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

### File Selection Events

```razor
<SfFileManager TValue="FileManagerDirectoryContent" AllowMultiSelection="true">
    <FileManagerEvents TValue="FileManagerDirectoryContent" 
                       FileSelected="OnFileSelected"
                       FileSelection="OnFileSelection">
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
    }

    public void OnFileSelection(FileSelectionEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"File selection changed: {args.FileDetails.Count} items");
        // Can cancel selection or modify it
        args.Cancel = false;
    }
}
```

### Bulk Operations Example

```razor
<SfButton OnClick="DeleteSelected">Delete Selected</SfButton>
<SfButton OnClick="DownloadSelected">Download Selected</SfButton>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent" AllowMultiSelection="true">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;

    public async Task DeleteSelected()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            Console.WriteLine($"Deleting {FileManager.SelectedItems.Count} items");
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

## Navigation Customization

### Customize Navigation Items

From `customize-navigation-items.md`:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerNavigationPaneSettings Items="@NavItems">
    </FileManagerNavigationPaneSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    private List<NavigationItemModel> NavItems = new List<NavigationItemModel>()
    {
        new NavigationItemModel() { Text = "Documents", Path = "/Documents/" },
        new NavigationItemModel() { Text = "Pictures", Path = "/Pictures/" },
        new NavigationItemModel() { Text = "Downloads", Path = "/Downloads/" },
        new NavigationItemModel() { Text = "Videos", Path = "/Videos/" }
    };
}
```

## Styling and Appearance

### Theme Configuration

From `styles.md`:

Available themes:

- bootstrap5
- bootstrap4
- material
- material3
- fabric
- fluent
- fluent2
- tailwind
- highcontrast

### Set Theme in HTML

```html
<!-- Add to App.razor <head> section -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### Custom CSS Styling

```razor
<SfFileManager TValue="FileManagerDirectoryContent" CssClass="custom-filemanager">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

<style>
    .custom-filemanager .e-toolbar {
        background-color: #f5f5f5;
    }
    
    .custom-filemanager .e-list-item.e-active {
        background-color: #007bff;
        color: white;
    }
</style>
```

## Complete Customization Example

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<div style="height: 600px;">
    <SfFileManager TValue="FileManagerDirectoryContent" 
                   View="ViewType.Details"
                   AllowMultiSelection="true"
                   EnableRangeSelection="true"
                   CssClass="custom-filemanager">
        
        <FileManagerToolbarSettings Visible="true" ToolbarItems="@ToolbarItems">
        </FileManagerToolbarSettings>
        
        <FileManagerDetailsViewSettings>
            <FileManagerColumns>
                <FileManagerColumn Field="Name" HeaderText="Name" Width="250"></FileManagerColumn>
                <FileManagerColumn Field="Size" HeaderText="Size" Width="100"></FileManagerColumn>
                <FileManagerColumn Field="DateModified" Format="MM/dd/yyyy" HeaderText="Modified"></FileManagerColumn>
            </FileManagerColumns>
        </FileManagerDetailsViewSettings>

        <FileManagerContextMenuSettings File="@FileMenuItems" Folder="@FolderMenuItems">
        </FileManagerContextMenuSettings>

        <FileManagerEvents TValue="FileManagerDirectoryContent" 
                          ToolbarCreated="OnToolbarCreated"
                          ToolbarItemClicked="OnToolbarItemClicked"
                          MenuOpened="OnMenuOpened"
                          OnMenuClick="OnMenuClick"
                          FileSelected="OnFileSelected">
        </FileManagerEvents>

        <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                 UploadUrl="/api/FileManager/Upload"
                                 DownloadUrl="/api/FileManager/Download"
                                 GetImageUrl="/api/FileManager/GetImage">
        </FileManagerAjaxSettings>
    </SfFileManager>
</div>

@code {
    private List<ToolBarItemModel> ToolbarItems = new List<ToolBarItemModel>()
    {
        new ToolBarItemModel() { Name = "NewFolder" },
        new ToolBarItemModel() { Name = "Upload" },
        new ToolBarItemModel() { Name = "Delete" },
        new ToolBarItemModel() { Name = "Refresh" }
    };

    private string[] FileMenuItems = new string[] { "Open", "Delete", "Rename", "Download" };
    private string[] FolderMenuItems = new string[] { "Open", "Rename", "Delete" };

    private void OnToolbarCreated(ToolbarCreateEventArgs args)
    {
        Console.WriteLine("Toolbar created");
    }

    private void OnToolbarItemClicked(ToolbarClickEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Toolbar item clicked: {args.Item.Name}");
    }

    private void OnMenuOpened(MenuOpenEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine("Context menu opened");
    }

    private void OnMenuClick(MenuClickEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Menu item clicked: {args.Item.Text}");
    }

    private void OnFileSelected(FileSelectEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Files selected: {args.FileDetails.Count}");
    }
}
```

## Navigation Pane Template

The navigation pane displays the folder hierarchy in a tree-like structure. You can customize the layout of each folder node using the `NavigationPaneTemplate` property. This allows you to modify folder appearance, add custom icons, or display additional metadata.

### Customize Navigation Pane

From `customize-navigation-items.md`:

```razor
@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent">
    <ChildContent>
        <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                 UploadUrl="/api/FileManager/Upload"
                                 DownloadUrl="/api/FileManager/Download"
                                 GetImageUrl="/api/FileManager/GetImage">
        </FileManagerAjaxSettings>
    </ChildContent>
    <NavigationPaneTemplate>
        <div class="e-nav-pane-node" style="display: inline-flex; align-items: center;">
            @if (context is FileManagerDirectoryContent item)
            {
                <span class="folder-icon" style="margin-right: 8px;">📁</span>
                <span class="folder-name" style="margin-left:8px;">@item.Name</span>
            }
        </div>
    </NavigationPaneTemplate>
</SfFileManager>
```

### Advanced Navigation Pane with Custom Icons and Metadata

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <ChildContent>
        <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                 UploadUrl="/api/FileManager/Upload"
                                 DownloadUrl="/api/FileManager/Download"
                                 GetImageUrl="/api/FileManager/GetImage">
        </FileManagerAjaxSettings>
    </ChildContent>
    <NavigationPaneTemplate>
        <div class="custom-nav-item" style="padding: 5px; border-radius: 4px;">
            @if (context is FileManagerDirectoryContent item)
            {
                <div style="display: flex; justify-content: space-between; align-items: center;">
                    <div style="display: flex; align-items: center;">
                        @if (item.HasChild)
                        {
                            <span style="margin-right: 8px;">📂</span>
                        }
                        else
                        {
                            <span style="margin-right: 8px;">📁</span>
                        }
                        <span>@item.Name</span>
                    </div>
                    <span style="font-size: 12px; color: #999;">@(item.DateModified?.ToString("MM/dd/yyyy") ?? "")</span>
                </div>
            }
        </div>
    </NavigationPaneTemplate>
</SfFileManager>
```

## Next Steps

- See [Advanced Features](advanced-features.md) for additional customization options
- Check [File Providers](file-providers.md) for provider-specific customizations
- Review [Events and Callbacks](events-and-callbacks.md) for event-driven customization
