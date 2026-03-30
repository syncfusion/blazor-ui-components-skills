# Customization and Styling in TreeView

## Table of Contents
- [Icon Customization](#icon-customization)
- [Dynamic Icons](#dynamic-icons)
- [Text Wrapping](#text-wrapping)
- [Template Customization](#template-customization)
- [CSS Styling](#css-styling)
- [Themes](#themes)
- [Common Patterns](#common-patterns)

## Icon Customization

### Customize Expand/Collapse Icons

Override default expand/collapse icons with custom icons:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

<style>
    /* Override expand icon */
    .e-treeview .e-list-item.e-expanded>.e-expand-icon::before {
        content: "\e729";  /* Custom icon content */
    }
    
    /* Override collapse icon */
    .e-treeview .e-list-item:not(.e-expanded)>.e-expand-icon::before {
        content: "\e726";  /* Custom icon content */
    }
</style>
```

### Use Font Awesome Icons

```html
<!-- Add Font Awesome library -->

<style>
    .e-treeview .e-expand-icon {
        font-family: 'FontAwesome';
        font-size: 14px;
    }
    
    .e-treeview .e-list-item.e-expanded>.e-expand-icon::before {
        content: "\f078";  /* fa-chevron-down */
    }
    
    .e-treeview .e-list-item:not(.e-expanded)>.e-expand-icon::before {
        content: "\f054";  /* fa-chevron-right */
    }
</style>
```

### Node Icons

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ImageUrl="Icon"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public string? Icon { get; set; }  // CSS class or image URL
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Documents", Icon = "e-icons e-folder" },
        new MailItem { Id = "2", FolderName = "Settings", Icon = "e-icons e-settings" }
    };
}
```

## Dynamic Icons

### Get Dynamic Icons Based on Data

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="FileItem">
    <TreeViewFieldsSettings TValue="FileItem"
        Id="Id"
        Text="FileName"
        ImageUrl="GetIconClass"
        DataSource="@FileList">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class FileItem
    {
        public string? Id { get; set; }
        public string? FileName { get; set; }
        public string? FileType { get; set; }  // "folder", "file", "image", etc.
    }

    string GetIconClass(FileItem item)
    {
        return item.FileType switch
        {
            "folder" => "e-icons e-folder",
            "image" => "e-icons e-image",
            "document" => "e-icons e-document",
            "music" => "e-icons e-music",
            _ => "e-icons e-file"
        };
    }

    List<FileItem> FileList = new()
    {
        new FileItem { Id = "1", FileName = "Documents", FileType = "folder" },
        new FileItem { Id = "2", FileName = "photo.jpg", FileType = "image" }
    };
}
```

### Icon Colors Based on Status

```css
/* Icon color based on status */
.e-treeview .e-list-item.completed .e-list-icon {
    color: green;
}

.e-treeview .e-list-item.pending .e-list-icon {
    color: orange;
}

.e-treeview .e-list-item.failed .e-list-icon {
    color: red;
}
```

## Text Wrapping

### Enable Text Wrapping with AllowTextWrap

The `AllowTextWrap` property enables text wrapping for node labels when they exceed the node width:

```csharp
@using Syncfusion.Blazor.Navigations

<!-- Enable text wrapping for long node labels -->
<SfTreeView TValue="MailItem" AllowTextWrap="true" Height="400px">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }  // Long text will wrap
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { 
            Id = "1", 
            FolderName = "Very Long Folder Name That Exceeds Default Width and Will Wrap to Next Line" 
        },
        new MailItem { 
            Id = "2", 
            FolderName = "Another folder with a description that is quite lengthy" 
        }
    };
}
```

**AllowTextWrap Property:**
- **Default value:** `false`
- **When `true`:** Text wraps to multiple lines if it exceeds node width
- **When `false`:** Text is truncated with ellipsis

### Text Wrapping with Custom Width

Combine `AllowTextWrap` with CSS to control wrapping behavior:

```csharp
<SfTreeView TValue="MailItem" AllowTextWrap="true" CssClass="custom-wrap">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

<style>
    /* Control text wrapping width */
    .custom-wrap .e-list-item .e-list-text {
        max-width: 250px;
        word-wrap: break-word;
        word-break: break-word;
    }
    
    /* Optional: Increase node height for wrapped text */
    .custom-wrap .e-list-item {
        height: auto;
        min-height: 32px;
        padding: 6px 0;
    }
</style>
```

### Text Wrapping Examples

**Without AllowTextWrap (Default):**
- Long text: "This is a very long folder name..." → Truncated
- Node height: 32px (fixed)

**With AllowTextWrap="true":**
- Long text wraps to multiple lines
- Node height expands automatically
- Improves readability of lengthy labels

```csharp
// Example comparison
<div>
    <h4>Without Text Wrap (AllowTextWrap="false")</h4>
    <SfTreeView TValue="Item" AllowTextWrap="false">
        <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
    </SfTreeView>
    
    <h4>With Text Wrap (AllowTextWrap="true")</h4>
    <SfTreeView TValue="Item" AllowTextWrap="true">
        <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
    </SfTreeView>
</div>
```

## Template Customization

### Custom Node Template

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="EmployeeItem">
    <TreeViewFieldsSettings TValue="EmployeeItem" DataSource="@EmployeeList" />
    <TreeViewTemplates TValue="EmployeeItem">
        <NodeTemplate>
            <div class="custom-node">
                <span class="emp-icon">👤</span>
                <span class="emp-name">@((context as EmployeeItem).Name)</span>
                <span class="emp-role">(@((context as EmployeeItem).Role))</span>
            </div>
        </NodeTemplate>
    </TreeViewTemplates>
</SfTreeView>

@code {
    public class EmployeeItem
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Role { get; set; }
        public string? ReportsTo { get; set; }
    }

    List<EmployeeItem> EmployeeList = new()
    {
        new EmployeeItem { Id = "1", Name = "Alice", Role = "Manager" },
        new EmployeeItem { Id = "2", Name = "Bob", Role = "Developer" }
    };
}

<style>
    .custom-node {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 4px;
    }
    
    .emp-icon {
        font-size: 18px;
    }
    
    .emp-role {
        color: #666;
        font-size: 12px;
    }
</style>
```

### Template with Buttons

```csharp
<TreeViewTemplates TValue="FolderItem">
    <NodeTemplate>
        <div style="display: flex; justify-content: space-between; align-items: center; width: 100%;">
            <span>@((context as FolderItem).Name)</span>
            <button @onclick="() => DeleteNode((context as FolderItem).Id)">Delete</button>
        </div>
    </NodeTemplate>
</TreeViewTemplates>

@code {
    async Task DeleteNode(string nodeId)
    {
        MyFolder.RemoveAll(x => x.Id == nodeId);
        StateHasChanged();
    }
}
```

## CSS Styling

### Override TreeView Styles

```css
/* Change background color */
.e-treeview {
    background-color: #f5f5f5;
}

/* Style node text */
.e-treeview .e-list-item .e-list-text {
    font-size: 14px;
    font-weight: 500;
}

/* Hover effect */
.e-treeview .e-list-item:hover {
    background-color: #e3f2fd;
}

/* Selected node */
.e-treeview .e-list-item.e-active {
    background-color: #2196f3;
    color: white;
}

/* Expanded node */
.e-treeview .e-list-item.e-expanded {
    background-color: #f0f0f0;
}
```

### Custom Indentation

```css
/* Change indentation width */
.e-treeview .e-list-item {
    padding-left: 32px;  /* Default is 24px */
}

.e-treeview .e-list-item .e-list-item {
    padding-left: 64px;
}
```

### Node Height

```css
/* Increase node height */
.e-treeview .e-list-item {
    height: 40px;
    line-height: 40px;
}
```

## Themes

### Apply Built-in Theme

```html
<!-- Bootstrap 5 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

### Custom Theme with CSS Variables

```css
:root {
    --treeview-bg: #ffffff;
    --treeview-text: #333333;
    --treeview-hover: #f0f0f0;
    --treeview-selected: #2196f3;
    --treeview-selected-text: #ffffff;
}

.e-treeview {
    background-color: var(--treeview-bg);
    color: var(--treeview-text);
}

.e-treeview .e-list-item:hover {
    background-color: var(--treeview-hover);
}

.e-treeview .e-list-item.e-active {
    background-color: var(--treeview-selected);
    color: var(--treeview-selected-text);
}
```

### Dark Theme

```css
/* Dark Mode */
.dark-mode .e-treeview {
    background-color: #1e1e1e;
    color: #e0e0e0;
}

.dark-mode .e-treeview .e-list-item:hover {
    background-color: #333333;
}

.dark-mode .e-treeview .e-list-item.e-active {
    background-color: #1976d2;
}

.dark-mode .e-expand-icon {
    color: #b0b0b0;
}
```

## Common Patterns

### Pattern 1: Responsive Styling

```css
/* Mobile */
@media (max-width: 768px) {
    .e-treeview .e-list-item {
        height: 32px;
        font-size: 14px;
    }
    
    .e-treeview .e-expand-icon {
        width: 20px;
    }
}

/* Desktop */
@media (min-width: 769px) {
    .e-treeview .e-list-item {
        height: 40px;
        font-size: 14px;
    }
}
```

### Pattern 2: Status-Based Styling

```csharp
public class TaskItem
{
    public string? Id { get; set; }
    public string? Name { get; set; }
    public string? Status { get; set; }  // "completed", "pending", "failed"
    
    public string GetStatusClass()
    {
        return Status switch
        {
            "completed" => "status-completed",
            "failed" => "status-failed",
            _ => "status-pending"
        };
    }
}

<TreeViewTemplates TValue="TaskItem">
    <NodeTemplate>
        <div class="@((context as TaskItem).GetStatusClass())">
            @((context as TaskItem).Name)
        </div>
    </NodeTemplate>
</TreeViewTemplates>

<style>
    .status-completed {
        color: green;
        text-decoration: line-through;
    }
    
    .status-failed {
        color: red;
        font-weight: bold;
    }
    
    .status-pending {
        color: orange;
    }
</style>
```

### Pattern 3: Level-Based Indentation

```css
/* Different styling per level */
.e-treeview .e-list-item[data-level="0"] {
    background-color: #f0f0f0;
    font-weight: bold;
}

.e-treeview .e-list-item[data-level="1"] {
    padding-left: 40px;
}

.e-treeview .e-list-item[data-level="2"] {
    padding-left: 60px;
    font-size: 13px;
}
```

### Pattern 4: Conditional Icon Color

```csharp
<TreeViewTemplates TValue="FileItem">
    <NodeTemplate>
        <span style="@GetIconStyle((context as FileItem))">
            @GetIconClass((context as FileItem))
        </span>
        <span>@((context as FileItem).FileName)</span>
    </NodeTemplate>
</TreeViewTemplates>

@code {
    string GetIconStyle(FileItem item)
    {
        return item.FileType switch
        {
            "folder" => "color: #1976d2;",
            "image" => "color: #ff9800;",
            "error" => "color: #f44336;",
            _ => "color: #757575;"
        };
    }
}
```

## Best Practices

1. **Use CSS classes** instead of inline styles for reusability
2. **Override theme variables** for consistent branding
3. **Provide light and dark** theme options
4. **Test responsiveness** on mobile devices
5. **Use semantic markup** in templates
6. **Cache template results** for performance
7. **Keep templates simple** for better performance

## Troubleshooting

**Issue: Custom styles not applying**
- Use CSS specificity selector (`.e-treeview` prefix)
- Check CSS file is loaded after theme
- Inspect element to see applied styles

**Issue: Icons not displaying**
- Verify icon font is loaded
- Check icon class name exists
- Use browser DevTools to verify CSS

**Issue: Template not rendering**
- Ensure TreeViewTemplates is nested under SfTreeView
- Add TValue property to TreeViewTemplates tag
- Check template context type matches TValue
- Verify no syntax errors in template HTML

## Next Steps

- Use [advanced-features.md](advanced-features.md) for integration patterns
- Check [accessibility.md](accessibility.md) for accessible styling
- Review [events-handling.md](events-handling.md) for dynamic styling
