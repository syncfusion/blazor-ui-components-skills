# Backstage View in Blazor Ribbon Component

The Backstage view is a full-screen menu typically accessed via the File menu button. It displays options like New, Open, Save, Print, and other file-related operations, along with application settings and user information.

## When to Use Backstage

- File management operations (New, Open, Save, Save As)
- Application settings and preferences
- User account information
- Recent files/documents list
- Help and feedback
- Share options
- Print preview and options

## Basic Backstage Setup

```razor
<SfRibbon>
    <RibbonBackstageMenuSettings Text="File" Visible="true">
        <BackstageMenuItems>
            <BackstageMenuItem ID="new" Text="New" IconCss="e-icons e-file-new">
                <div>New document content here</div>
            </BackstageMenuItem>
            <BackstageMenuItem ID="open" Text="Open" IconCss="e-icons e-folder-open">
                <div>Open document content here</div>
            </BackstageMenuItem>
        </BackstageMenuItems>
    </RibbonBackstageMenuSettings>
    
    <RibbonTabs>
        <!-- Ribbon tabs -->
    </RibbonTabs>
</SfRibbon>
```

## Backstage Properties

### RibbonBackstageMenuSettings
- `Text` - Label for the backstage button (default: "File")
- `Visible` - Show/hide the backstage menu (default: false)
- `KeyTip` - Keyboard shortcut for opening backstage
- `Width` - Width of the backstage view
- `Height` - Height of the backstage view
- `BackButtonSettings` - Configuration for the back/close button
- `BackstageItemClick` - Event handler for backstage item clicks

### BackstageMenuItem
- `ID` - Unique identifier for the menu item
- `Text` - Display text for the menu item
- `IconCss` - Icon class for visual indicator
- `Separator` - Whether to show a divider line
- `IsFooter` - Display item at bottom of menu (default: false)
- `KeyTip` - Keyboard shortcut for the item

## Basic Backstage Example

```razor
<SfRibbon>
    <RibbonBackstageMenuSettings Text="File" Visible="true">
        <BackstageMenuItems>
            <BackstageMenuItem ID="home" Text="Home" IconCss="e-icons e-home">
                @GetContent("home")
            </BackstageMenuItem>
            <BackstageMenuItem ID="new" Text="New" IconCss="e-icons e-file-new">
                @GetContent("new")
            </BackstageMenuItem>
            <BackstageMenuItem ID="open" Text="Open" IconCss="e-icons e-folder-open">
                @GetContent("open")
            </BackstageMenuItem>
            <BackstageMenuItem ID="save" Text="Save" IconCss="e-icons e-save">
                @GetContent("save")
            </BackstageMenuItem>
        </BackstageMenuItems>
    </RibbonBackstageMenuSettings>
    
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <!-- Home tab content -->
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    RenderFragment GetContent(string item) => item switch
    {
        "home" => @<div class="backstage-content">
            <h2>Welcome</h2>
            <p>Recent documents appear here</p>
        </div>,
        "new" => @<div class="backstage-content">
            <h2>New Document</h2>
            <p>Create a new document</p>
        </div>,
        "open" => @<div class="backstage-content">
            <h2>Open Document</h2>
            <p>Browse and open existing documents</p>
        </div>,
        "save" => @<div class="backstage-content">
            <h2>Save Document</h2>
            <p>Save current document</p>
        </div>,
        _ => @<div>Unknown item</div>
    };
}
```

## Backstage Separators

Use separators to visually group related menu items:

```razor
<BackstageMenuItems>
    <BackstageMenuItem ID="new" Text="New" IconCss="e-icons e-file-new">
        <!-- Content -->
    </BackstageMenuItem>
    <BackstageMenuItem ID="open" Text="Open" IconCss="e-icons e-folder-open">
        <!-- Content -->
    </BackstageMenuItem>
    <BackstageMenuItem ID="save" Text="Save" IconCss="e-icons e-save">
        <!-- Content -->
    </BackstageMenuItem>
    <BackstageMenuItem Separator="true"></BackstageMenuItem>
    <BackstageMenuItem ID="print" Text="Print" IconCss="e-icons e-print">
        <!-- Content -->
    </BackstageMenuItem>
    <BackstageMenuItem ID="export" Text="Export" IconCss="e-icons e-export">
        <!-- Content -->
    </BackstageMenuItem>
</BackstageMenuItems>
```

## Footer Items

Add items to the bottom of the menu using `IsFooter="true"`:

```razor
<BackstageMenuItems>
    <!-- Regular items -->
    <BackstageMenuItem ID="new" Text="New" IconCss="e-icons e-file-new">
        <!-- Content -->
    </BackstageMenuItem>
    
    <!-- Separator before footer items -->
    <BackstageMenuItem Separator="true"></BackstageMenuItem>
    
    <!-- Footer items -->
    <BackstageMenuItem ID="options" Text="Options" IconCss="e-icons e-settings" IsFooter="true">
        <div class="backstage-content">
            <h3>Application Options</h3>
            <!-- Options content -->
        </div>
    </BackstageMenuItem>
    <BackstageMenuItem ID="account" Text="Account" IconCss="e-icons e-people" IsFooter="true">
        <div class="backstage-content">
            <h3>User Account</h3>
            <!-- Account content -->
        </div>
    </BackstageMenuItem>
</BackstageMenuItems>
```

## Back Button Customization

```razor
<RibbonBackstageMenuSettings 
    Text="File" 
    Visible="true"
    BackButtonSettings="@backButtonConfig">
    <BackstageMenuItems>
        <!-- Items -->
    </BackstageMenuItems>
</RibbonBackstageMenuSettings>

@code {
    BackstageBackButtonSettings backButtonConfig = new BackstageBackButtonSettings
    {
        Text = "Back to Document",
        IconCss = "e-icons e-back",
        Visible = true
    };
}
```

## Backstage Events

```razor
<RibbonBackstageMenuSettings 
    Text="File" 
    Visible="true"
    BackstageItemClick="OnBackstageItemClick">
    <BackstageMenuItems>
        <BackstageMenuItem ID="new" Text="New" IconCss="e-icons e-file-new">
            <!-- Content -->
        </BackstageMenuItem>
    </BackstageMenuItems>
</RibbonBackstageMenuSettings>

@code {
    private void OnBackstageItemClick(BackstageItemClickEventArgs args)
    {
        Console.WriteLine($"Clicked: {args.Item.ID}");
        // Handle the click
    }
}
```

## Backstage with Custom Layout

```razor
<RibbonBackstageMenuSettings Text="File" Visible="true">
    <Template>
        <div style="display: flex; width: 100%; height: 100%">
            <!-- Left menu -->
            <div style="width: 150px; background: #f0f0f0; padding: 10px">
                <ul style="list-style: none; padding: 0">
                    <li @onclick="() => SelectContent('new')">New</li>
                    <li @onclick="() => SelectContent('open')">Open</li>
                    <li @onclick="() => SelectContent('save')">Save</li>
                </ul>
            </div>
            
            <!-- Right content -->
            <div style="flex: 1; padding: 20px">
                @GetContent(selectedContent)
            </div>
        </div>
    </Template>
</RibbonBackstageMenuSettings>

@code {
    private string selectedContent = "new";

    private void SelectContent(string content)
    {
        selectedContent = content;
    }

    RenderFragment GetContent(string item) => item switch
    {
        "new" => @<div><h2>New Document</h2></div>,
        "open" => @<div><h2>Open Document</h2></div>,
        "save" => @<div><h2>Save Document</h2></div>,
        _ => @<div></div>
    };
}
```

## Best Practices

- **Logical organization**: Group related items together
- **Use separators**: Clearly separate different sections (File, Recent, Settings)
- **Icon consistency**: Use appropriate icons for each menu item
- **Content clarity**: Provide clear, descriptive content for each backstage view
- **Performance**: Avoid heavy content rendering for better performance
- **Keyboard navigation**: Implement KeyTips for accessibility
- **Recent items**: Show recently used documents in the backstage view
- **Save location**: Consider adding save location options to backstage

## Common Backstage Structure

```
File Menu
├── Home (Recent documents)
├── New (New document templates)
├── Open (Browse/open files)
├── Save (Save options)
├── Save As (Save with different name/format)
├── Print (Print preview and options)
├── ---separator---
├── Export (Export to different formats)
├── Share (Sharing options)
├── ---separator---
└── Options (Application settings) [Footer]
```

## Performance Considerations

- Use content templates to lazy load backstage content
- Avoid heavy rendering in backstage items
- Minimize backstage view height/width when possible
- Cache backstage content if frequently accessed

## Accessibility

- Ensure all menu items have meaningful icons and labels
- Implement KeyTips for keyboard navigation
- Provide meaningful descriptions for content
- Support screen readers with proper ARIA attributes

## Related Components

- **File Menu** - For simpler file operations
- **Contextual Tabs** - For context-specific commands
- **Gallery** - For style/template selection
- **KeyTips** - For keyboard shortcuts
