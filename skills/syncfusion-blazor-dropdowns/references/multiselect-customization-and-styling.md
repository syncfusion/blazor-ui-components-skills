# Customization and Styling

## Table of Contents
- [CSS Classes and Theming](#css-classes-and-theming)
- [Placeholder and Labels](#placeholder-and-labels)
- [Icon Support](#icon-support)
- [Popup Customization](#popup-customization)
- [Chip Display Modes](#chip-display-modes)
- [RTL Support](#rtl-support)
- [Dark Mode](#dark-mode)
- [Custom CSS Variables](#custom-css-variables)

## CSS Classes and Theming

### Apply Theme

Built-in themes are applied via CSS file:

```html
<!-- In App.razor <head> -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

**Available themes:**
- `bootstrap5.css` - Bootstrap 5 (default)
- `material.css` - Material Design
- `fluent.css` - Fluent UI
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

### Custom CSS Class

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               CssClass="custom-multiselect">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

<style>
    .custom-multiselect .e-input {
        font-size: 14px;
        border: 2px solid #2196F3;
    }
    
    .custom-multiselect .e-chip {
        background-color: #E3F2FD;
        color: #1976D2;
    }
</style>

@code {
    private List<Item> Items { get; set; } = new();
}
```

### Inline Styles

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               Style="width: 100%; max-width: 500px;">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

## Placeholder and Labels

### Custom Placeholder

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               Placeholder="Select employees or departments">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

### Floating Label

Enable floating label for modern form appearance:

```csharp
<SfMultiSelect DataSource="@Items" 
               @bind-Value="SelectedItems"
               TValue="List<string>"
               TItem="Item"
               FloatLabelType="FloatLabelType.Always"
               Placeholder="Select options">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<string> SelectedItems { get; set; } = new();
}
```

**FloatLabelType options:**
- `Never` - No floating label
- `Auto` - Float on focus or when value present (default)
- `Always` - Always float (label at top)

## Icon Support

### Prefix and Suffix Icons

```csharp
@page "/icons"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               Placeholder="Select items">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectTemplates TItem="Item">
        <PrefixTemplate>
            <span class="e-icons e-search"></span>
        </PrefixTemplate>
    </MultiSelectTemplates>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();
}
```

### Custom Icon Classes

Use Syncfusion icon set or Font Awesome:

```csharp
<!-- Using Syncfusion icons -->
<style>
    .e-icons::before {
        font-family: 'e-icons';
        font-style: normal;
        font-weight: 400;
    }
    
    .e-search::before { content: '\e5f1'; }  /* Search icon */
    .e-close::before { content: '\e285'; }   /* Close icon */
</style>

<!-- Or use Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectTemplates TItem="Item">
        <PrefixTemplate>
            <i class="fas fa-filter"></i>
        </PrefixTemplate>
    </MultiSelectTemplates>
</SfMultiSelect>
```

## Popup Customization

### Popup Width and Height

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               PopupWidth="400px"
               PopupHeight="300px">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

### Popup Position

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               PopupHeight="300px"
               PopupWidth="100%">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

**Behavior:**
- Auto-positions above/below based on available space
- Always respects screen boundaries
- Adjusts width to match input by default

## Chip Display Modes

### Box Mode (Chip Style)

```csharp
<SfMultiSelect DataSource="@Items" 
               @bind-Value="SelectedItems"
               TValue="List<string>"
               TItem="Item"
               Mode="VisualMode.Box"
               Placeholder="Items display as removable chips">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<string> SelectedItems { get; set; } = new();
}
```

### Custom Chip Styling

```csharp
<SfMultiSelect DataSource="@Items" 
               @bind-Value="SelectedItems"
               TValue="List<string>"
               TItem="Item"
               Mode="VisualMode.Box"
               CssClass="custom-chips">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

<style>
    .custom-chips .e-chip {
        border-radius: 20px;
        padding: 6px 12px;
        background-color: #4CAF50;
        color: white;
        font-weight: 500;
    }
    
    .custom-chips .e-chip-close {
        margin-left: 8px;
    }
</style>

@code {
    private List<string> SelectedItems { get; set; } = new();
}
```

### Delimiter Mode

```csharp
<SfMultiSelect DataSource="@Items" 
               Mode="VisualMode.Delimiter"
               Delimiter=","
               Placeholder="Items shown comma-separated">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

## RTL Support

### Enable Right-to-Left Layout

```csharp
@page "/rtl-demo"
@using Syncfusion.Blazor.DropDowns

<div dir="rtl">
    <SfMultiSelect DataSource="@Items" 
                   TValue="List<string>"
                   TItem="Item"
                   EnableRtl="true"
                   Placeholder="اختر العناصر">
        <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    </SfMultiSelect>
</div>

@code {
    private List<Item> Items { get; set; } = new()
    {
        new Item { ID = "1", Name = "عربي" },
        new Item { ID = "2", Name = "فارسي" },
        new Item { ID = "3", Name = "عبري" }
    };

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

**RTL Properties:**
- `EnableRtl="true"` - Enable RTL layout
- Add `dir="rtl"` to parent container
- Icons and text automatically mirror

## Dark Mode

### Enable Dark Theme

```html
<!-- Replace light theme with dark variant -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
```

**Available dark themes:**
- `bootstrap5-dark.css`
- `material-dark.css`
- `fluent-dark.css`
- `tailwind-dark.css`
- `fabric-dark.css`

### System Dark Mode Detection

```csharp
@page "/dark-mode-auto"
@using Syncfusion.Blazor.DropDowns

@if (IsDarkMode)
{
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
}
else
{
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
}

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private bool IsDarkMode { get; set; } = false;

    protected override async Task OnInitializedAsync()
    {
        // Check system preference
        IsDarkMode = await JS.InvokeAsync<bool>(
            "window.matchMedia('(prefers-color-scheme: dark)').matches"
        );
    }

    private List<Item> Items { get; set; } = new();
}
```

### Custom Dark Mode

```csharp
<style>
    .dark-theme .e-input {
        background-color: #1e1e1e;
        color: #ffffff;
        border-color: #444444;
    }
    
    .dark-theme .e-popup {
        background-color: #2d2d2d;
        color: #ffffff;
    }
    
    .dark-theme .e-list-item:hover {
        background-color: #3d3d3d;
    }
</style>

<div class="dark-theme">
    <SfMultiSelect DataSource="@Items" 
                   TValue="List<string>"
                   TItem="Item">
        <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    </SfMultiSelect>
</div>
```

## Custom CSS Variables

### Override Theme Colors

```csharp
<style>
    .custom-multiselect {
        --e-primary: #2196F3;           /* Primary color */
        --e-success: #4CAF50;            /* Success color */
        --e-danger: #F44336;             /* Danger color */
        --e-warning: #FF9800;            /* Warning color */
        --e-info: #00BCD4;               /* Info color */
        --e-border-color: #E0E0E0;      /* Border color */
        --e-bg-color: #FFFFFF;          /* Background */
        --e-text-color: #333333;        /* Text color */
    }
</style>

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               CssClass="custom-multiselect">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();
}
```

### Complete Custom Theme

```csharp
<style>
    .branded-select {
        --e-primary: #6C63FF;          /* Purple brand color */
        --e-border-color: #E9E9F0;
        --e-bg-color: #F7F7FF;
        --e-text-color: #2C2C54;
    }
    
    .branded-select .e-chip {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border-radius: 16px;
    }
    
    .branded-select .e-input {
        font-weight: 500;
        letter-spacing: 0.5px;
    }
</style>

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               CssClass="branded-select"
               Mode="VisualMode.Box"
               Placeholder="Select branded items">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();
}
```

## Key Takeaways

✅ Use built-in themes via CSS for consistency  
✅ `CssClass` for component-specific styling  
✅ `Mode="VisualMode.Box"` for modern chip display  
✅ `FloatLabelType.Always` for professional forms  
✅ `EnableRtl="true"` for international apps  
✅ Dark themes available in `{theme}-dark.css`  
✅ CSS variables override theme colors  

## Next Steps

- **Add templates** → See [Accessibility and Templates](accessibility-and-templates.md)
- **Handle events** → See [Events and API Reference](events-and-api.md)
- **Advanced usage** → See [Advanced Scenarios](advanced-scenarios.md)
