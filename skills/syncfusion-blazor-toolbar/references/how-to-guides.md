# How-To Guides for Blazor Toolbar Component

## Table of Contents
- [Overview](#overview)
- [Add or Remove Toolbar Items Dynamically](#add-or-remove-toolbar-items-dynamically)
- [Enable or Disable Toolbar Items](#enable-or-disable-toolbar-items)
- [Show or Hide Toolbar Items](#show-or-hide-toolbar-items)
- [Add Toggle Buttons](#add-toggle-buttons)
- [Customize Items with HtmlAttributes and CssClass](#customize-items-with-htmlattributes-and-cssclass)
- [Set Item-Wise Custom Templates](#set-item-wise-custom-templates)
- [Set Tooltips to Toolbar Items](#set-tooltips-to-toolbar-items)
- [Enable Tab Key Navigation](#enable-tab-key-navigation)
- [Customize the Scrolling Distance](#customize-the-scrolling-distance)

## Overview

This guide provides practical, task-based examples for common toolbar operations. Each section includes complete working code and explanations.

## Add or Remove Toolbar Items Dynamically

Toolbar items can be added or removed dynamically by manipulating the underlying data collection.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="AddItem" Content="Add Item"></SfButton>
<SfButton @onclick="RemoveItem" Content="Remove Item"></SfButton>
<br /><br />

<SfToolbar>
    <ToolbarItems>
        @foreach (var item in ToolbarItems)
        {
            <ToolbarItem PrefixIcon="@item.PrefixIcon" 
                         Text="@item.Text" 
                         TooltipText="@item.TooltipText">
            </ToolbarItem>
        }
    </ToolbarItems>
</SfToolbar>

@code {
    public class ToolbarItemData
    {
        public string PrefixIcon { get; set; }
        public string Text { get; set; }
        public string TooltipText { get; set; }
    }
    
    private List<ToolbarItemData> ToolbarItems = new()
    {
        new() { PrefixIcon = "e-icons e-cut", Text = "Cut", TooltipText = "Cut" },
        new() { PrefixIcon = "e-icons e-copy", Text = "Copy", TooltipText = "Copy" },
        new() { PrefixIcon = "e-icons e-paste", Text = "Paste", TooltipText = "Paste" },
        new() { PrefixIcon = "e-icons e-bold", Text = "Bold", TooltipText = "Bold" },
        new() { PrefixIcon = "e-icons e-underline", Text = "Underline", TooltipText = "Underline" }
    };
    
    private void AddItem()
    {
        ToolbarItems.Add(new()
        {
            PrefixIcon = "e-icons e-italic",
            Text = "Italic",
            TooltipText = "Italic"
        });
    }
    
    private void RemoveItem()
    {
        if (ToolbarItems.Count > 0)
        {
            ToolbarItems.RemoveAt(0);
        }
    }
}
```

### Key Points

- Use `@foreach` loop to iterate over a collection
- Update the collection to add or remove items
- The toolbar automatically re-renders when the collection changes
- Check collection count before removing to avoid errors

### Advanced: Insert at Specific Position

```csharp
private void InsertItem(int index)
{
    ToolbarItems.Insert(index, new()
    {
        PrefixIcon = "e-icons e-paste",
        Text = "Paste",
        TooltipText = "Paste"
    });
}

private void RemoveItemAt(int index)
{
    if (index >= 0 && index < ToolbarItems.Count)
    {
        ToolbarItems.RemoveAt(index);
    }
}
```

## Enable or Disable Toolbar Items

Control item enabled state using the `Disabled` property with data binding.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-cut" 
                     OnClick="@OnCutClick" 
                     Text="Cut" 
                     TooltipText="Cut">
        </ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paste" 
                     Disabled="@IsPasteDisabled" 
                     Text="Paste" 
                     TooltipText="Paste">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bold" Text="Bold" TooltipText="Bold"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-italic" Text="Italic" TooltipText="Italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private bool IsPasteDisabled = true;
    
    private void OnCutClick()
    {
        // Simulate cutting text - enables paste
        IsPasteDisabled = false;
    }
}
```

### Key Points

- Use `Disabled` property with data binding
- Update the boolean flag to enable/disable items
- Disabled items appear grayed out and don't respond to clicks

### Multiple Items Example

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Select All" OnClick="@OnSelectAll"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Copy" Disabled="@IsSelectionDisabled"></ToolbarItem>
        <ToolbarItem Text="Cut" Disabled="@IsSelectionDisabled"></ToolbarItem>
        <ToolbarItem Text="Delete" Disabled="@IsSelectionDisabled"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private bool IsSelectionDisabled = true;
    
    private void OnSelectAll()
    {
        // Enable selection-based actions
        IsSelectionDisabled = false;
    }
}
```

## Show or Hide Toolbar Items

Control item visibility using the `Visible` property with data binding.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-cut" 
                     OnClick="@OnCutClick" 
                     Text="Cut" 
                     TooltipText="Cut">
        </ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paste" 
                     Visible="@IsPasteVisible" 
                     Text="Paste" 
                     TooltipText="Paste">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bold" Text="Bold" TooltipText="Bold"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-italic" Text="Italic" TooltipText="Italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private bool IsPasteVisible = false;
    
    private void OnCutClick()
    {
        // Show paste button after cut
        IsPasteVisible = true;
    }
}
```

### Key Points

- Use `Visible` property with data binding
- Hidden items don't occupy space in the toolbar
- Useful for conditional UI based on user permissions or state

### Permission-Based Visibility

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="View" Visible="true"></ToolbarItem>
        <ToolbarItem Text="Edit" Visible="@HasEditPermission"></ToolbarItem>
        <ToolbarItem Text="Delete" Visible="@HasDeletePermission"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator" Visible="@HasAdminPermission"></ToolbarItem>
        <ToolbarItem Text="Settings" Visible="@HasAdminPermission"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private bool HasEditPermission = true;
    private bool HasDeletePermission = false;
    private bool HasAdminPermission = false;
}
```

## Add Toggle Buttons

Create toggle buttons using the `Template` property with Syncfusion Button component's `IsToggle` feature.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem>
            <Template>
                <SfButton CssClass="e-flat" 
                          IconCss="@PlayIconCss" 
                          IsToggle="true" 
                          @onclick="@OnPlayClick">
                </SfButton>
            </Template>
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <SfButton CssClass="e-flat" 
                          IconCss="@ZoomIconCss" 
                          IsToggle="true" 
                          @onclick="@OnZoomClick">
                </SfButton>
            </Template>
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <SfButton CssClass="e-flat" 
                          IconCss="@VisibleIconCss" 
                          IsToggle="true" 
                          Content="@ContentText" 
                          @onclick="@OnVisibleClick">
                </SfButton>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<div style="display:@DisplayValue; margin-top: 20px;">
    <p>This content toggles when you click the visibility button.</p>
</div>

@code {
    private string PlayIconCss = "e-icons e-play";
    private string ZoomIconCss = "e-icons e-zoom-in";
    private string VisibleIconCss = "e-icons e-hide";
    private string ContentText = "Hide";
    private string DisplayValue = "block";
    
    private void OnPlayClick()
    {
        PlayIconCss = PlayIconCss == "e-icons e-play" 
            ? "e-icons e-pause" 
            : "e-icons e-play";
    }
    
    private void OnZoomClick()
    {
        ZoomIconCss = ZoomIconCss == "e-icons e-zoom-in" 
            ? "e-icons e-zoom-out" 
            : "e-icons e-zoom-in";
    }
    
    private void OnVisibleClick()
    {
        if (ContentText == "Hide")
        {
            DisplayValue = "none";
            ContentText = "Show";
            VisibleIconCss = "e-icons e-show";
        }
        else
        {
            DisplayValue = "block";
            ContentText = "Hide";
            VisibleIconCss = "e-icons e-hide";
        }
    }
}
```

### Key Points

- Use `Template` property with `SfButton` component
- Set `IsToggle="true"` on the button
- Update icon or content in click handler based on current state
- Store toggle state in component variables

### Simple Toggle Example

```razor
<ToolbarItem>
    <Template>
        <SfButton IsToggle="true" 
                  Content="@(IsActive ? "Active" : "Inactive")" 
                  @onclick="@(() => IsActive = !IsActive)">
        </SfButton>
    </Template>
</ToolbarItem>

@code {
    private bool IsActive = false;
}
```

## Customize Items with HtmlAttributes and CssClass

Apply custom styling and HTML attributes to toolbar items.

### Using CssClass

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="500">
    <ToolbarItems>
        <ToolbarItem Text="Bold" CssClass="custom-bold"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
        <ToolbarItem Text="Uppercase" CssClass="text-uppercase"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .custom-bold .e-tbar-btn-text {
        font-weight: 900;
        color: #d32f2f;
    }
    
    .text-uppercase .e-tbar-btn-text {
        font-variant: small-caps;
        color: #1976d2;
    }
</style>
```

### Using HtmlAttributes

```razor
<SfToolbar Width="500">
    <ToolbarItems>
        <ToolbarItem Text="Bold" HtmlAttributes="@BoldAttributes"></ToolbarItem>
        <ToolbarItem Text="Italic" HtmlAttributes="@ItalicAttributes"></ToolbarItem>
        <ToolbarItem Text="Underline"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private Dictionary<string, object> BoldAttributes = new()
    {
        { "class", "custom-bold" },
        { "id", "bold-button" },
        { "data-action", "format" },
        { "role", "button" }
    };
    
    private Dictionary<string, object> ItalicAttributes = new()
    {
        { "style", "background-color: #f0f0f0; border-radius: 4px;" },
        { "data-tooltip", "Apply italic formatting" }
    };
}
```

### Combined Example

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Save" 
                     CssClass="primary-action" 
                     HtmlAttributes="@SaveAttributes" 
                     PrefixIcon="e-icons e-save">
        </ToolbarItem>
        <ToolbarItem Text="Cancel" 
                     CssClass="secondary-action" 
                     HtmlAttributes="@CancelAttributes">
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .primary-action .e-tbar-btn {
        background-color: #4caf50;
        color: white;
    }
    
    .secondary-action .e-tbar-btn {
        background-color: #f44336;
        color: white;
    }
</style>

@code {
    private Dictionary<string, object> SaveAttributes = new()
    {
        { "id", "save-btn" },
        { "data-action", "save" }
    };
    
    private Dictionary<string, object> CancelAttributes = new()
    {
        { "id", "cancel-btn" },
        { "data-action", "cancel" }
    };
}
```

## Set Item-Wise Custom Templates

Use the `Template` property to render custom content for toolbar items.

### Basic Template Example

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="300">
    <ToolbarItems>
        <ToolbarItem Text="Cut" TooltipText="Cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <input type="checkbox" title="Accept" checked />
            </Template>
        </ToolbarItem>
        <ToolbarItem Text="Undo"></ToolbarItem>
        <ToolbarItem Text="Redo"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <button class="e-btn">Custom Button</button>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Template with Syncfusion Components

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Inputs

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="File"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Font dropdown -->
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfDropDownList TValue="string" TItem="string" 
                                Placeholder="Select font" 
                                DataSource="@Fonts" 
                                Width="150">
                </SfDropDownList>
            </Template>
        </ToolbarItem>
        
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Font size -->
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfNumericTextBox TValue="int" 
                                  Value="12" 
                                  Min="8" 
                                  Max="72" 
                                  Width="80">
                </SfNumericTextBox>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private List<string> Fonts = new() 
    { 
        "Arial", 
        "Calibri", 
        "Georgia", 
        "Times New Roman", 
        "Verdana" 
    };
}
```

### Advanced Template with Logic

```razor
<ToolbarItem>
    <Template>
        <div class="custom-control">
            <label>@CurrentUser</label>
            <button @onclick="@Logout" class="e-btn e-small">
                Logout
            </button>
        </div>
    </Template>
</ToolbarItem>

<style>
    .custom-control {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 0 8px;
    }
</style>

@code {
    private string CurrentUser = "John Doe";
    
    private void Logout()
    {
        // Logout logic
    }
}
```

## Set Tooltips to Toolbar Items

Add tooltips using the built-in `TooltipText` property or integrate with `SfTooltip` component for advanced features.

### Basic Tooltip with TooltipText

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" TooltipText="Cut selected text (Ctrl+X)"></ToolbarItem>
        <ToolbarItem Text="Copy" TooltipText="Copy to clipboard (Ctrl+C)"></ToolbarItem>
        <ToolbarItem Text="Paste" TooltipText="Paste from clipboard (Ctrl+V)"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Advanced Tooltip with SfTooltip

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Popups

<SfTooltip Target="#toolbar-element [title]">
    <SfToolbar ID="toolbar-element" Width="300">
        <ToolbarItems>
            <ToolbarItem Type="ItemType.Button" Text="Cut" TooltipText="Cut"></ToolbarItem>
            <ToolbarItem Type="ItemType.Button" Text="Copy" TooltipText="Copy"></ToolbarItem>
            <ToolbarItem Type="ItemType.Button" Text="Paste" TooltipText="Paste"></ToolbarItem>
            <ToolbarItem Type="ItemType.Button" Text="Undo" TooltipText="Undo"></ToolbarItem>
            <ToolbarItem Type="ItemType.Button" Text="Redo" TooltipText="Redo"></ToolbarItem>
        </ToolbarItems>
    </SfToolbar>
</SfTooltip>
```

### Key Points

- `TooltipText` property provides simple tooltip functionality
- Use `SfTooltip` component for advanced features (animations, positioning, content templates)
- Target selector `[title]` captures all items with TooltipText
- Always provide tooltips for icon-only buttons

## Enable Tab Key Navigation

Enable tab key navigation using the `TabIndex` property.

### Example with Explicit Order

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="600">
    <ToolbarItems>
        <ToolbarItem SuffixIcon="e-icons e-cut" Text="Cut" TabIndex="1"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-copy" Text="Copy" TabIndex="2"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-paste" Text="Paste" TabIndex="3"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-bold" Text="Bold" TabIndex="4"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-italic" Text="Italic" TabIndex="5"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Example with Natural Order

```razor
<SfToolbar Width="600">
    <ToolbarItems>
        <ToolbarItem SuffixIcon="e-icons e-cut" Text="Cut" TabIndex="0"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-copy" Text="Copy" TabIndex="0"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-paste" Text="Paste" TabIndex="0"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-bold" Text="Bold" TabIndex="0"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-italic" Text="Italic" TabIndex="0"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Key Points

- **TabIndex > 0**: Explicit navigation order (1, 2, 3, ...)
- **TabIndex = 0**: Natural source order navigation
- **TabIndex < 0**: Disables tab navigation for that item
- By default, toolbar uses arrow key navigation
- TabIndex adds Tab/Shift+Tab navigation on top of arrow keys

### Mixed Navigation Example

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Primary" TabIndex="1"></ToolbarItem>
        <ToolbarItem Text="Secondary" TabIndex="2"></ToolbarItem>
        <ToolbarItem Text="Skip" TabIndex="-1"></ToolbarItem>
        <ToolbarItem Text="Tertiary" TabIndex="3"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

In this example, "Skip" button is excluded from tab navigation but still accessible via arrow keys.

## Customize the Scrolling Distance

Customize how far the toolbar scrolls when using navigation arrows in `Scrollable` mode.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="600" ScrollStep="50">
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-cut" TooltipText="Cut"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-copy" TooltipText="Copy"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paste" TooltipText="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bold" TooltipText="Bold"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-underline" TooltipText="Underline"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-italic" TooltipText="Italic"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paint-bucket" TooltipText="Color"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-align-left" TooltipText="Left"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-align-center" TooltipText="Center"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-align-right" TooltipText="Right"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-justify" TooltipText="Justify"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-list-unordered" TooltipText="Bullets"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-list-ordered" TooltipText="Numbering"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Key Points

- `ScrollStep` property defines scroll distance in pixels
- Default value is 30 pixels
- Applies only to `Scrollable` mode (default overflow mode)
- Larger values scroll more items at once
- Smaller values provide finer control

### Different Scroll Step Values

```razor
<!-- Fast scrolling -->
<SfToolbar Width="400" ScrollStep="100">
    <!-- Items -->
</SfToolbar>

<!-- Slow scrolling (default-like) -->
<SfToolbar Width="400" ScrollStep="30">
    <!-- Items -->
</SfToolbar>

<!-- Very fine control -->
<SfToolbar Width="400" ScrollStep="10">
    <!-- Items -->
</SfToolbar>
```

Choose `ScrollStep` value based on:
- Average item width
- Number of visible items
- User preference for scroll speed
- Touch device vs mouse device usage
