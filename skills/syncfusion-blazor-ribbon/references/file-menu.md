# File Menu Configuration

Implement Office-style file menu for common file operations (New, Open, Save, Print).

## Enabling File Menu

Show file menu by setting `Visible="true"`:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.Navigations

<SfRibbon>
    <RibbonFileMenuSettings Visible="true" MenuItems="@fileMenuItems">
    </RibbonFileMenuSettings>
    <RibbonTabs>
        <!-- Ribbon tabs -->
    </RibbonTabs>
</SfRibbon>

@code {
    private List<MenuItem> fileMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "New", IconCss = "e-icons e-file-new", Id = "new" },
        new MenuItem { Text = "Open", IconCss = "e-icons e-folder-open", Id = "open" },
        new MenuItem { Text = "Save", IconCss = "e-icons e-save", Id = "save" },
        new MenuItem { Text = "Save As", IconCss = "e-icons e-save", Id = "saveas" },
        new MenuItem { Text = "Print", IconCss = "e-icons e-print", Id = "print" },
        new MenuItem { Text = "Exit", IconCss = "e-icons e-exit", Id = "exit" }
    };
}
```

## Adding Menu Items

Simple menu items with text and icons:

```razor
<RibbonFileMenuSettings Visible="true" MenuItems="@fileMenuItems">
</RibbonFileMenuSettings>

@code {
    private List<MenuItem> fileMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "New", IconCss = "e-icons e-file-new", Id = "new" },
        new MenuItem { Text = "Open", IconCss = "e-icons e-folder-open", Id = "open" },
        new MenuItem { Text = "Recent", IconCss = "e-icons e-clock", Id = "recent" },
        new MenuItem { Text = "Save", IconCss = "e-icons e-save", Id = "save" }
    };
}
```

## Nested Submenus

Create multi-level menu hierarchy:

```razor
<RibbonFileMenuSettings Visible="true" MenuItems="@fileMenuItems">
</RibbonFileMenuSettings>

@code {
    private List<MenuItem> fileMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "New", IconCss = "e-icons e-file-new", Id = "new" },
        new MenuItem { Text = "Open", IconCss = "e-icons e-folder-open", Id = "open" },
        new MenuItem { Text = "Save", IconCss = "e-icons e-save", Id = "save" },
        new MenuItem {
            Text = "Save As",
            IconCss = "e-icons e-save",
            Id = "saveas",
            Items = new List<MenuItem>()
            {
                new MenuItem { Text = "Microsoft Word (.docx)", IconCss = "sf-icon-word", Id = "word" },
                new MenuItem { Text = "Word 97-2003 (.doc)", IconCss = "sf-icon-word", Id = "word97" },
                new MenuItem { Text = "PDF Document (.pdf)", IconCss = "e-icons e-export-pdf", Id = "pdf" },
                new MenuItem { Text = "Plain Text (.txt)", IconCss = "e-icons e-file-text", Id = "txt" }
            }
        },
        new MenuItem { Text = "Print", IconCss = "e-icons e-print", Id = "print" }
    };
}
```

## Submenu Behavior

### Open Submenu on Hover (Default)

Submenu opens when hovering over parent item:

```razor
<RibbonFileMenuSettings Visible="true" 
                        MenuItems="@fileMenuItems"
                        ShowItemOnClick="false">
</RibbonFileMenuSettings>
```

### Open Submenu on Click

Change to click-based submenu opening:

```razor
<RibbonFileMenuSettings Visible="true" 
                        MenuItems="@fileMenuItems"
                        ShowItemOnClick="true">
</RibbonFileMenuSettings>

@code {
    private List<MenuItem> fileMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "Save As", IconCss = "e-icons e-save", Id = "saveas",
            Items = new List<MenuItem>()
            {
                new MenuItem { Text = "Microsoft Word (.docx)", Id = "word" },
                new MenuItem { Text = "PDF Document (.pdf)", Id = "pdf" }
            }
        }
    };
}
```

## Custom Header Text

Change "File" to custom text:

```razor
<RibbonFileMenuSettings Text="Help" Visible="true" MenuItems="@helpMenuItems">
</RibbonFileMenuSettings>

@code {
    private List<MenuItem> helpMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "About", IconCss = "e-icons e-info" },
        new MenuItem { Text = "Documentation", IconCss = "e-icons e-help" },
        new MenuItem { Text = "Contact Support", IconCss = "e-icons e-email" }
    };
}
```

## File Menu Events

### FileMenuOpening / FileMenuClosing

Triggered before menu state changes:

```razor
<RibbonFileMenuSettings Visible="true"
                        MenuItems="@fileMenuItems"
                        FileMenuOpening="@OnMenuOpening"
                        FileMenuClosing="@OnMenuClosing"
                        FileMenuOpened="@OnMenuOpened"
                        FileMenuClosed="@OnMenuClosed"
                        FileMenuItemRendering="@OnItemRender"
                        ItemSelecting="@OnItemSelect">
</RibbonFileMenuSettings>

@code {
    private void OnMenuOpening(FileMenuOpenEventArgs args)
    {
        Console.WriteLine("File menu opening");
    }

    private void OnMenuClosing(FileMenuCloseEventArgs args)
    {
        Console.WriteLine("File menu closing");
    }

    private void OnMenuOpened(FileMenuOpenedEventArgs args)
    {
        Console.WriteLine("File menu opened");
    }

    private void OnMenuClosed(FileMenuClosedEventArgs args)
    {
        Console.WriteLine("File menu closed");
    }

    private void OnItemRender(FileMenuItemRenderEventArgs args)
    {
        Console.WriteLine($"Rendering item: {args.Item.Text}");
    }

    private void OnItemSelect(FileMenuItemSelectEventArgs args)
    {
        Console.WriteLine($"Item selected: {args.Item.Text}");
    }
}
```

## Complete Example with Actions

Functional file menu with item selection handling:

```razor
@page "/file-menu-demo"
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons
@using Syncfusion.Blazor.Navigations

<h2>Document Editor with File Menu</h2>

<div style="margin-bottom: 20px;">
    <p>Last Action: @lastAction</p>
</div>

<SfRibbon>
    <RibbonFileMenuSettings Visible="true" 
                            MenuItems="@fileMenuItems"
                            ShowItemOnClick="true"
                            ItemSelecting="@OnFileMenuItemSelect">
    </RibbonFileMenuSettings>
    
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Clipboard">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    private string lastAction = "None";

    private List<MenuItem> fileMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "New", IconCss = "e-icons e-file-new", Id = "new" },
        new MenuItem { Text = "Open", IconCss = "e-icons e-folder-open", Id = "open" },
        new MenuItem { Text = "Recent", IconCss = "e-icons e-clock", Id = "recent",
            Items = new List<MenuItem>()
            {
                new MenuItem { Text = "Document1.docx", Id = "doc1" },
                new MenuItem { Text = "Document2.docx", Id = "doc2" },
                new MenuItem { Text = "Document3.docx", Id = "doc3" }
            }
        },
        new MenuItem { Text = "Save", IconCss = "e-icons e-save", Id = "save" },
        new MenuItem {
            Text = "Save As",
            IconCss = "e-icons e-save",
            Id = "saveas",
            Items = new List<MenuItem>()
            {
                new MenuItem { Text = "Microsoft Word (.docx)", IconCss = "sf-icon-word", Id = "word" },
                new MenuItem { Text = "Word 97-2003 (.doc)", IconCss = "sf-icon-word", Id = "word97" },
                new MenuItem { Text = "PDF Document (.pdf)", IconCss = "e-icons e-export-pdf", Id = "pdf" },
                new MenuItem { Text = "Plain Text (.txt)", IconCss = "e-icons e-file-text", Id = "txt" }
            }
        },
        new MenuItem { Text = "Print", IconCss = "e-icons e-print", Id = "print" },
        new MenuItem { Text = "Exit", IconCss = "e-icons e-exit", Id = "exit" }
    };

    private void OnFileMenuItemSelect(FileMenuItemSelectEventArgs args)
    {
        string itemId = args.Item.Id;
        
        lastAction = itemId switch
        {
            "new" => "Creating new document",
            "open" => "Opening document",
            "recent" => "Opening recent document",
            "save" => "Saving document",
            "saveas" => "Save As dialog",
            "word" => "Saving as .docx",
            "pdf" => "Saving as PDF",
            "print" => "Printing document",
            "exit" => "Exiting application",
            _ => $"Selected: {args.Item.Text}"
        };

        Console.WriteLine($"File menu action: {lastAction}");
    }
}
```

## Recent Documents Pattern

Common pattern for recent file list:

```razor
@code {
    private List<MenuItem> BuildRecentDocuments()
    {
        var recentDocs = new List<MenuItem>()
        {
            new MenuItem { Text = "Recent", IconCss = "e-icons e-clock", Id = "recent",
                Items = new List<MenuItem>()
            }
        };

        // Add recent documents dynamically
        var documents = new[] { "Report_2026.docx", "Proposal_Final.docx", "Budget_Q1.xlsx" };
        
        foreach (var doc in documents)
        {
            recentDocs[0].Items.Add(new MenuItem { Text = doc, Id = doc.ToLower() });
        }

        return recentDocs;
    }
}
```

## Icons

Common file menu icons:
- `e-icons e-file-new` - New document
- `e-icons e-folder-open` - Open
- `e-icons e-save` - Save
- `e-icons e-print` - Print
- `e-icons e-clock` - Recent
- `e-icons e-exit` - Exit
- `e-icons e-export-pdf` - PDF
- `sf-icon-word` - Word document
- `e-icons e-help` - Help

## Best Practices

### 1. Logical Menu Organization
```
File Menu
├── New
├── Open
├── Recent → [Document list]
├── Save
├── Save As → [Format options]
├── Print
└── Exit
```

### 2. Icon Consistency
Use recognizable icons for common operations.

### 3. Keyboard Shortcuts
Consider adding keyboard shortcut hints in menu:
```razor
new MenuItem { Text = "Save (Ctrl+S)", IconCss = "e-icons e-save", Id = "save" }
```

### 4. Separator Items
```razor
new MenuItem { Separator = true },  // Visual separator
```

### 5. Disabled Menu Items
```razor
new MenuItem { Text = "Save", IconCss = "e-icons e-save", Disabled = true }
```

## Troubleshooting

**Menu not appearing:**
- Verify `Visible="true"` is set
- Check `MenuItems` collection has items

**Submenu not opening:**
- Confirm `ShowItemOnClick` matches your UX preference
- Verify submenu items are in `Items` collection

**Events not firing:**
- Confirm event handler signature matches (e.g., `FileMenuItemSelectEventArgs`)
- Check event handler is bound with `@`
