# Ribbon Items: ComboBox, ColorPicker, GroupButton, Template

## Table of Contents
- [ComboBox Items](#combobox-items)
- [ColorPicker Items](#colorpicker-items)
- [GroupButton Items](#groupbutton-items)
- [Template Items](#template-items)
- [Display Options](#display-options)
- [Advanced Patterns](#advanced-patterns)

## ComboBox Items

### Basic ComboBox

Dropdown with editable input and data binding:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.DropDowns

<RibbonItem Type="RibbonItemType.ComboBox">
    <RibbonComboBoxSettings DataSource="@fontFamilies" 
                            FieldSettings="@fieldSettings"
                            AllowFiltering="true">
    </RibbonComboBoxSettings>
</RibbonItem>

@code {
    private class FontItem
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }

    private FieldSettingsModel fieldSettings = new FieldSettingsModel
    {
        Text = "Text",
        Value = "Value"
    };

    private List<FontItem> fontFamilies = new List<FontItem>
    {
        new FontItem { Text = "Arial", Value = "Arial" },
        new FontItem { Text = "Calibri", Value = "Calibri" },
        new FontItem { Text = "Cambria", Value = "Cambria" },
        new FontItem { Text = "Courier New", Value = "Courier" },
        new FontItem { Text = "Times New Roman", Value = "Times" }
    };
}
```

### ComboBox with Value Binding

Track selected value in component:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.DropDowns

<div>
    <p>Selected Font: @selectedFont</p>
    <button @onclick="ApplyFont">Apply Font</button>
</div>

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Font">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.ComboBox">
                                    <RibbonComboBoxSettings @bind-Value="@selectedFont"
                                                           DataSource="@fontFamilies"
                                                           FieldSettings="@fieldSettings"
                                                           AllowFiltering="true"
                                                           Index="0">
                                    </RibbonComboBoxSettings>
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
    private string selectedFont = "Arial";

    private void ApplyFont()
    {
        Console.WriteLine($"Applying font: {selectedFont}");
    }

    private class FontItem
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }

    private FieldSettingsModel fieldSettings = new FieldSettingsModel
    {
        Text = "Text",
        Value = "Value"
    };

    private List<FontItem> fontFamilies = new List<FontItem>
    {
        new FontItem { Text = "Arial", Value = "Arial" },
        new FontItem { Text = "Calibri", Value = "Calibri" },
        new FontItem { Text = "Cambria", Value = "Cambria" }
    };
}
```

### ComboBox Events

Handle selection and filtering:

```razor
<RibbonComboBoxSettings DataSource="@fontFamilies"
                        FieldSettings="@fieldSettings"
                        AllowFiltering="true"
                        PopupOpening="@OnPopupOpen"
                        PopupClosed="@OnPopupClosed"
                        Filtering="@OnFilter"
                        Selecting="@OnSelect"
                        ValueChange="@OnValueChange">
</RibbonComboBoxSettings>

@code {
    private void OnPopupOpen(ComboBoxPopupOpenEventArgs args) 
        => Console.WriteLine("Popup opening");

    private void OnPopupClosed(ComboBoxPopupClosedEventArgs args) 
        => Console.WriteLine("Popup closed");

    private void OnFilter(ComboBoxFilterEventArgs args) 
        => Console.WriteLine($"Filtering: {args.Text}");

    private void OnSelect(ComboBoxSelectEventArgs args) 
        => Console.WriteLine($"Selecting: {args.ItemData}");

    private void OnValueChange(ComboBoxChangeEventArgs args) 
        => Console.WriteLine($"Value changed: {args.Value}");
}
```

### Font Size ComboBox Pattern

Common pattern for size selection:

```razor
<RibbonGroup HeaderText="Font" Orientation="Orientation.Row">
    <RibbonCollections>
        <RibbonCollection>
            <RibbonItems>
                <!-- Font Family -->
                <RibbonItem Type="RibbonItemType.ComboBox" AllowedSizes="RibbonItemSize.Small">
                    <RibbonComboBoxSettings @bind-Value="@selectedFont"
                                           DataSource="@fontFamilies"
                                           FieldSettings="@fieldSettings"
                                           AllowFiltering="true"
                                           Placeholder="Font">
                    </RibbonComboBoxSettings>
                </RibbonItem>
                <!-- Font Size -->
                <RibbonItem Type="RibbonItemType.ComboBox" AllowedSizes="RibbonItemSize.Small">
                    <RibbonComboBoxSettings @bind-Value="@selectedSize"
                                           DataSource="@fontSizes"
                                           FieldSettings="@fieldSettings"
                                           Index="4"
                                           Placeholder="Size">
                    </RibbonComboBoxSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>

@code {
    private string selectedFont = "Arial";
    private string selectedSize = "12";

    private class ComboItem
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }

    private FieldSettingsModel fieldSettings = new FieldSettingsModel
    {
        Text = "Text",
        Value = "Value"
    };

    private List<ComboItem> fontFamilies = new List<ComboItem>
    {
        new ComboItem { Text = "Arial", Value = "Arial" },
        new ComboItem { Text = "Calibri", Value = "Calibri" },
        new ComboItem { Text = "Cambria", Value = "Cambria" }
    };

    private List<ComboItem> fontSizes = new List<ComboItem>
    {
        new ComboItem { Text = "8", Value = "8" },
        new ComboItem { Text = "10", Value = "10" },
        new ComboItem { Text = "12", Value = "12" },
        new ComboItem { Text = "14", Value = "14" },
        new ComboItem { Text = "16", Value = "16" }
    };
}
```

## ColorPicker Items

### Basic ColorPicker

Render a color selection control:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.Inputs

<RibbonItem Type="RibbonItemType.ColorPicker" AllowedSizes="RibbonItemSize.Small">
    <RibbonColorPickerSettings Value="#FF5733">
    </RibbonColorPickerSettings>
</RibbonItem>
```

### ColorPicker with Value Binding

Track selected color:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.Inputs

<div>
    <p>Selected Color: @selectedColor</p>
    <div style="background-color: @selectedColor; width: 50px; height: 50px; border: 1px solid #ccc;"></div>
</div>

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Font Color">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.ColorPicker" AllowedSizes="RibbonItemSize.Small">
                                    <RibbonColorPickerSettings @bind-Value="@selectedColor"
                                                              Mode="ColorPickerMode.Picker">
                                    </RibbonColorPickerSettings>
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
    private string selectedColor = "#FF5733";
}
```

### ColorPicker Events

Handle color selection:

```razor
<RibbonColorPickerSettings Value="#FF5733"
                           Mode="ColorPickerMode.Picker"
                           Created="@OnCreated"
                           Opening="@OnOpening"
                           Opened="@OnOpened"
                           Closing="@OnClosing"
                           ValueChange="@OnValueChange"
                           Selected="@OnSelected">
</RibbonColorPickerSettings>

@code {
    private void OnCreated() => Console.WriteLine("ColorPicker created");
    private void OnOpening(ColorPickerOpenEventArgs args) => Console.WriteLine("Opening");
    private void OnOpened(ColorPickerOpenedEventArgs args) => Console.WriteLine("Opened");
    private void OnClosing(ColorPickerCloseEventArgs args) => Console.WriteLine("Closing");
    private void OnValueChange(ColorPickerEventArgs args) => Console.WriteLine($"Color changed: {args.Value}");
    private void OnSelected(ColorPickerSelectedEventArgs args) => Console.WriteLine($"Selected: {args.Value}");
}
```

## GroupButton Items

### Single Selection GroupButton

Radio button-like behavior:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<RibbonItem Type="RibbonItemType.GroupButton">
    <RibbonGroupButtonSettings Selection="GroupButtonSelection.Single"
                               Items="@alignmentOptions"
                               ItemClick="@OnAlignmentClick">
    </RibbonGroupButtonSettings>
</RibbonItem>

@code {
    private List<GroupButtonItem> alignmentOptions = new List<GroupButtonItem>
    {
        new GroupButtonItem { IconCss = "e-icons e-align-left", Selected = true, Content = "Left" },
        new GroupButtonItem { IconCss = "e-icons e-align-center", Content = "Center" },
        new GroupButtonItem { IconCss = "e-icons e-align-right", Content = "Right" },
        new GroupButtonItem { IconCss = "e-icons e-justify", Content = "Justify" }
    };

    private void OnAlignmentClick(GroupButtonClickEventArgs args)
    {
        Console.WriteLine($"Alignment selected: {args.Item.Content}");
    }
}
```

### Multiple Selection GroupButton

Checkbox-like behavior:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<RibbonItem Type="RibbonItemType.GroupButton">
    <RibbonGroupButtonSettings Selection="GroupButtonSelection.Multiple"
                               HeaderText="Format Styles"
                               Items="@formatOptions"
                               ItemClick="@OnFormatClick">
    </RibbonGroupButtonSettings>
</RibbonItem>

@code {
    private List<GroupButtonItem> formatOptions = new List<GroupButtonItem>
    {
        new GroupButtonItem { IconCss = "e-icons e-bold", Content = "Bold", Selected = true },
        new GroupButtonItem { IconCss = "e-icons e-italic", Content = "Italic" },
        new GroupButtonItem { IconCss = "e-icons e-underline", Content = "Underline" },
        new GroupButtonItem { IconCss = "e-icons e-strikethrough", Content = "Strikethrough" }
    };

    private void OnFormatClick(GroupButtonClickEventArgs args)
    {
        Console.WriteLine($"Format toggled: {args.Item.Content}");
    }
}
```

## Template Items

### Custom HTML Content

Render custom content in ribbon:

```razor
@using Syncfusion.Blazor.Ribbon

<RibbonItem Type="RibbonItemType.Template">
    <ItemTemplate>
        <input type="text" placeholder="Search..." 
               style="width: 150px; padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
    </ItemTemplate>
</RibbonItem>
```

### Template with Input Control

Search box example:

```razor
@using Syncfusion.Blazor.Ribbon

<RibbonItem Type="RibbonItemType.Template">
    <ItemTemplate>
        <div style="display: flex; align-items: center;">
            <label>Search: </label>
            <input @bind="searchText" 
                   type="text" 
                   placeholder="Enter text..."
                   style="margin-left: 8px; padding: 6px; border: 1px solid #ccc; border-radius: 3px;">
            <button @onclick="HandleSearch" style="margin-left: 8px; padding: 6px 12px;">Go</button>
        </div>
    </ItemTemplate>
</RibbonItem>

@code {
    private string searchText = "";

    private void HandleSearch()
    {
        Console.WriteLine($"Searching for: {searchText}");
    }
}
```

### Template with Rating

Star rating example:

```razor
<RibbonItem Type="RibbonItemType.Template">
    <ItemTemplate>
        <div style="display: flex; gap: 3px;">
            @for (int i = 1; i <= 5; i++)
            {
                int star = i;
                <span @onclick="@(() => SetRating(star))" 
                      style="cursor: pointer; font-size: 18px; color: @(rating >= star ? "gold" : "#ccc");">
                    ★
                </span>
            }
            <span style="margin-left: 8px;">@rating/5</span>
        </div>
    </ItemTemplate>
</RibbonItem>

@code {
    private int rating = 3;

    private void SetRating(int value)
    {
        rating = value;
        Console.WriteLine($"Rating set to: {value}");
    }
}
```

## Display Options

### Controlling Item Visibility

Show/hide items based on layout:

```razor
<!-- Always visible (default) -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Auto">
    <RibbonButtonSettings Content="Save"></RibbonButtonSettings>
</RibbonItem>

<!-- Classic layout only -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Classic">
    <RibbonButtonSettings Content="Full Options"></RibbonButtonSettings>
</RibbonItem>

<!-- Simplified layout only -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Simplified">
    <RibbonButtonSettings Content="Compact"></RibbonButtonSettings>
</RibbonItem>

<!-- Overflow popup only -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Overflow">
    <RibbonButtonSettings Content="Advanced"></RibbonButtonSettings>
</RibbonItem>
```

## Advanced Patterns

### Font and Size Selection

Combined selector pattern:

```razor
<RibbonGroup HeaderText="Font" Orientation="Orientation.Row">
    <RibbonCollections>
        <RibbonCollection>
            <!-- Font Family ComboBox -->
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.ComboBox" AllowedSizes="RibbonItemSize.Small">
                    <RibbonComboBoxSettings @bind-Value="@fontFamily"
                                           DataSource="@fonts"
                                           FieldSettings="@fieldSettings"
                                           Index="0">
                    </RibbonComboBoxSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
        <RibbonCollection>
            <!-- Font Size ComboBox -->
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.ComboBox" AllowedSizes="RibbonItemSize.Small">
                    <RibbonComboBoxSettings @bind-Value="@fontSize"
                                           DataSource="@sizes"
                                           FieldSettings="@fieldSettings"
                                           Index="4">
                    </RibbonComboBoxSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>

<!-- Color and Format -->
<RibbonGroup HeaderText="Formatting">
    <RibbonCollections>
        <RibbonCollection>
            <!-- Font Color -->
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.ColorPicker" AllowedSizes="RibbonItemSize.Small">
                    <RibbonColorPickerSettings @bind-Value="@fontColor"
                                              Mode="ColorPickerMode.Picker">
                    </RibbonColorPickerSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
        <RibbonCollection>
            <!-- Format Style (GroupButton) -->
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.GroupButton">
                    <RibbonGroupButtonSettings Selection="GroupButtonSelection.Multiple"
                                               Items="@formats">
                    </RibbonGroupButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>

@code {
    private string fontFamily = "Arial";
    private string fontSize = "12";
    private string fontColor = "#000000";

    private class ComboItem
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }

    private FieldSettingsModel fieldSettings = new FieldSettingsModel
    {
        Text = "Text",
        Value = "Value"
    };

    private List<ComboItem> fonts = new List<ComboItem>
    {
        new ComboItem { Text = "Arial", Value = "Arial" },
        new ComboItem { Text = "Calibri", Value = "Calibri" },
        new ComboItem { Text = "Cambria", Value = "Cambria" }
    };

    private List<ComboItem> sizes = new List<ComboItem>
    {
        new ComboItem { Text = "8", Value = "8" },
        new ComboItem { Text = "10", Value = "10" },
        new ComboItem { Text = "12", Value = "12" },
        new ComboItem { Text = "14", Value = "14" },
        new ComboItem { Text = "16", Value = "16" }
    };

    private List<GroupButtonItem> formats = new List<GroupButtonItem>
    {
        new GroupButtonItem { IconCss = "e-icons e-bold", Content = "B" },
        new GroupButtonItem { IconCss = "e-icons e-italic", Content = "I" },
        new GroupButtonItem { IconCss = "e-icons e-underline", Content = "U" }
    };
}
```

These advanced item types enable rich, interactive ribbon interfaces with data binding and complex UI patterns.
