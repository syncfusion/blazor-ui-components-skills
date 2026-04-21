# Item Configuration in Blazor Toolbar Component

## Table of Contents
- [Overview](#overview)
- [Item Properties Reference](#item-properties-reference)
- [Align Property](#align-property)
- [CssClass Property](#cssclass-property)
- [Disabled Property](#disabled-property)
- [HtmlAttributes Property](#htmlattributes-property)
- [Id Property](#id-property)
- [Overflow Property](#overflow-property)
- [PrefixIcon Property](#prefixicon-property)
- [ShowAlwaysInPopup Property](#showalwaysinpopup-property)
- [ShowTextOn Property](#showtexton-property)
- [SuffixIcon Property](#suffixicon-property)
- [TabIndex Property](#tabindex-property)
- [Template Property](#template-property)
- [Text Property](#text-property)
- [TooltipText Property](#tooltiptext-property)
- [Type Property](#type-property)
  - [Button Type](#button-type)
  - [Separator Type](#separator-type)
  - [Input Type](#input-type)
  - [Spacer Type](#spacer-type)
- [Visible Property](#visible-property)
- [Width Property](#width-property)
- [Integrating Other Components](#integrating-other-components)

## Overview

The Blazor Toolbar can be configured with 18 different item properties that control appearance, behavior, and functionality. Each `ToolbarItem` supports these properties for complete customization.

## Item Properties Reference

Quick reference of all available properties:

| Property | Type | Description |
|----------|------|-------------|
| `Align` | ItemAlign | Left, Center, or Right alignment |
| `CssClass` | string | Custom CSS classes |
| `Disabled` | bool | Enables/disables the item |
| `HtmlAttributes` | Dictionary | Custom HTML attributes |
| `Id` | string | Unique identifier |
| `Overflow` | OverflowOption | Show, Hide, or None priority |
| `PrefixIcon` | string | Icon before text |
| `ShowAlwaysInPopup` | bool | Always show in popup |
| `ShowTextOn` | DisplayMode | Toolbar, Overflow, or Both |
| `SuffixIcon` | string | Icon after text |
| `TabIndex` | int | Tab navigation order |
| `Template` | RenderFragment | Custom content |
| `Text` | string | Button text |
| `TooltipText` | string | Hover tooltip |
| `Type` | ItemType | Button, Separator, Input, Spacer |
| `Visible` | bool | Controls visibility |
| `Width` | string | Item width |

## Align Property

Specifies the alignment of toolbar items. Each item aligns independently according to its `Align` property.

**Values:**
- `ItemAlign.Left` - Aligns item to the left (default)
- `ItemAlign.Center` - Aligns item at the center
- `ItemAlign.Right` - Aligns item to the right

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Home" Align="ItemAlign.Left"></ToolbarItem>
        <ToolbarItem Text="My Page" Align="ItemAlign.Center"></ToolbarItem>
        <ToolbarItem Text="Search" Align="ItemAlign.Right"></ToolbarItem>
        <ToolbarItem Text="Settings" Align="ItemAlign.Right"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** For more flexible alignment without using the Align property, use the Spacer type. See the alignment-spacer.md reference for details.

## CssClass Property

Adds single or multiple CSS classes to toolbar items for custom styling.

**Example:**

```razor
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
    }
    
    .text-uppercase .e-tbar-btn-text {
        font-variant: small-caps;
    }
</style>
```

## Disabled Property

Enables or disables a toolbar item. When `true`, the item appears grayed out and cannot be clicked.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste" Disabled="true"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Practical Use:** Disable items based on application state (e.g., disable "Paste" when clipboard is empty).

## HtmlAttributes Property

Adds custom HTML attributes such as `id`, `class`, `style`, `role`, or data attributes to the toolbar item element.

**Example:**

```razor
<SfToolbar Width="500">
    <ToolbarItems>
        <ToolbarItem Text="Bold" HtmlAttributes="@BoldAttributes"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
        <ToolbarItem Text="Underline"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    Dictionary<string, object> BoldAttributes = new()
    {
        { "class", "custom-bold" },
        { "id", "bold-button" },
        { "data-action", "format" }
    };
}
```

**Behavior Notes:**
- **style attribute**: New styles replace existing ones
- **class attribute**: New classes are added (don't replace existing ones)

## Id Property

Specifies a unique ID for the toolbar item's button or input element.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Id="home-btn" Text="Home" Align="ItemAlign.Left"></ToolbarItem>
        <ToolbarItem Id="search-btn" Text="Search" Align="ItemAlign.Right"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Use Cases:**
- Targeting items with JavaScript or CSS
- Accessibility ARIA references
- Integration with other components

## Overflow Property

Specifies display priority when toolbar content overflows. Only applicable to `Popup` mode.

**Values:**
- `OverflowOption.Show` - Always displays on toolbar (primary priority)
- `OverflowOption.Hide` - Always displays in popup (secondary priority)
- `OverflowOption.None` - No priority, moves to popup based on order (default)

**Example:**

```razor
<SfToolbar Width="300px" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Text="Cut" Overflow="OverflowOption.Show"></ToolbarItem>
        <ToolbarItem Text="Copy" Overflow="OverflowOption.Show"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        <ToolbarItem Text="Bold"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

In this example, "Cut" and "Copy" always remain on the toolbar, while other items move to popup when space is limited.

## PrefixIcon Property

Defines CSS classes for an icon displayed before the text content. The icon appears before text if text is available; otherwise, only the icon renders.

**Example:**

```razor
<SfToolbar Width="600">
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-cut" Text="Cut"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-copy" Text="Copy"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paste" Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bold" Text="Bold"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-italic" Text="Italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Icon-Only Buttons:**

```razor
<ToolbarItem PrefixIcon="e-icons e-cut" TooltipText="Cut"></ToolbarItem>
<ToolbarItem PrefixIcon="e-icons e-copy" TooltipText="Copy"></ToolbarItem>
```

## ShowAlwaysInPopup Property

Forces an item to always appear in the popup, even if there's space on the toolbar. Only works in `Popup` mode.

**Example:**

```razor
<SfToolbar Width="300px" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Text="Cut" ShowAlwaysInPopup="true" TooltipText="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy" TooltipText="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste" TooltipText="Paste"></ToolbarItem>
        <ToolbarItem Text="Bold" TooltipText="Bold"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** This property doesn't work for items with `Overflow="OverflowOption.Show"`.

## ShowTextOn Property

Controls where button text appears in `Popup` mode.

**Values:**
- `DisplayMode.Toolbar` - Text visible only on toolbar
- `DisplayMode.Overflow` - Text visible only in popup
- `DisplayMode.Both` - Text visible in both locations (default)

**Example:**

```razor
<SfToolbar Width="300" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-cut" Text="Cut" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-copy" Text="Copy" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paste" Text="Paste" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bold" Text="Bold" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

This creates icon-only buttons on the toolbar, but displays both icon and text in the overflow popup.

## SuffixIcon Property

Defines CSS classes for an icon displayed after the text content.

**Example:**

```razor
<SfToolbar Width="600">
    <ToolbarItems>
        <ToolbarItem SuffixIcon="e-icons e-cut" Text="Cut"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-copy" Text="Copy"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-upload-1" Text="Upload"></ToolbarItem>
        <ToolbarItem SuffixIcon="e-icons e-download" Text="Download"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** If both `PrefixIcon` and `SuffixIcon` are specified, only the `PrefixIcon` is displayed.

## TabIndex Property

Enables tab key navigation for toolbar items. By default, navigation uses arrow keys only.

**Values:**
- `0` - Natural source order navigation
- `> 0` - Explicit navigation order
- `< 0` - Disables tab navigation for the item

**Example with Explicit Order:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" TabIndex="1"></ToolbarItem>
        <ToolbarItem Text="Copy" TabIndex="2"></ToolbarItem>
        <ToolbarItem Text="Paste" TabIndex="3"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Example with Natural Order:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" TabIndex="0"></ToolbarItem>
        <ToolbarItem Text="Copy" TabIndex="0"></ToolbarItem>
        <ToolbarItem Text="Paste" TabIndex="0"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

See the accessibility.md reference for complete keyboard navigation details.

## Template Property

Allows custom content rendering using a RenderFragment. Enables integration with any Blazor component or HTML.

**Basic HTML Example:**

```razor
<SfToolbar Width="300">
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <input type="checkbox" title="Accept" checked />
            </Template>
        </ToolbarItem>
        <ToolbarItem Text="Undo"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Blazor Component Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <button class="e-btn">Custom Button</button>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## Text Property

Specifies the text content displayed on the toolbar button.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## TooltipText Property

Sets the tooltip text displayed when hovering over the toolbar item.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" TooltipText="Cut selected text"></ToolbarItem>
        <ToolbarItem Text="Copy" TooltipText="Copy to clipboard"></ToolbarItem>
        <ToolbarItem Text="Paste" TooltipText="Paste from clipboard"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Best Practice:** Always provide `TooltipText` for icon-only buttons to improve accessibility.

## Type Property

Specifies the type of toolbar item to render.

**Values:**
- `ItemType.Button` - Standard button (default)
- `ItemType.Separator` - Visual divider line
- `ItemType.Input` - Container for custom input controls
- `ItemType.Spacer` - Flexible space for alignment

### Button Type

Creates a button with text, icons, or both. This is the default type.

**Properties:**
- `Text` - Button text
- `PrefixIcon` - Icon before text
- `SuffixIcon` - Icon after text
- `Width` - Button width
- `Id` - Unique identifier

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Button" Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** `Type="ItemType.Button"` can be omitted as it's the default.

### Separator Type

Adds a vertical line to visually separate toolbar items.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Undo"></ToolbarItem>
        <ToolbarItem Text="Redo"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** Separators at the first or last position are not visible.

### Input Type

Creates a container for Syncfusion input components like NumericTextBox, DropDownList, RadioButton, or CheckBox.

**NumericTextBox Example:**

```razor
@using Syncfusion.Blazor.Inputs

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfNumericTextBox Width="150" Value="1" Format="c2"></SfNumericTextBox>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**DropDownList Example:**

```razor
@using Syncfusion.Blazor.DropDowns

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfDropDownList TValue="string" TItem="string" DataSource="@Options" Width="120"></SfDropDownList>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private List<string> Options = new() { "Option 1", "Option 2", "Option 3" };
}
```

**CheckBox Example:**

```razor
@using Syncfusion.Blazor.Buttons

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfCheckBox Checked="true" Label="Enable Feature"></SfCheckBox>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Spacer Type

Adds flexible empty space for aligning toolbar items. Spacers expand to fill available space.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="File"></ToolbarItem>
        <ToolbarItem Text="Edit"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Settings"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

See the alignment-spacer.md reference for detailed alignment patterns.

## Visible Property

Controls whether an item is visible or hidden.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" Visible="true"></ToolbarItem>
        <ToolbarItem Text="Copy" Visible="true"></ToolbarItem>
        <ToolbarItem Text="Paste" Visible="false"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Use Case:** Hide items based on user permissions or application state.

## Width Property

Specifies the width of a toolbar button item.

**Example:**

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" Width="100px"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## Integrating Other Components

The `Type="ItemType.Input"` with `Template` property allows integration of any Syncfusion component.

**Complete Example with Multiple Components:**

```razor
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Buttons

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfNumericTextBox Width="150" Value="1" Format="c2"></SfNumericTextBox>
            </Template>
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfDropDownList TValue="string" TItem="Game" 
                                DataSource="@Games" Width="120" Index="2">
                    <DropDownListFieldSettings Value="Text"></DropDownListFieldSettings>
                </SfDropDownList>
            </Template>
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfCheckBox Checked="true" Label="CheckBox"></SfCheckBox>
            </Template>
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <SfRadioButton Label="Radio" Name="default" Value="Radio" @bind-Checked="CheckedValue"></SfRadioButton>
            </Template>
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>

@code {
    private string CheckedValue = "Radio";
    
    public class Game
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
    
    private List<Game> Games = new()
    {
        new() { Id = "Game2", Text = "Badminton" },
        new() { Id = "Game4", Text = "Cricket" },
        new() { Id = "Game5", Text = "Football" }
    };
}
```

This demonstrates NumericTextBox, DropDownList, CheckBox, and RadioButton integration within a toolbar.
