# Ribbon Items: Button, Checkbox, DropDown, SplitButton

## Table of Contents
- [Button Items](#button-items)
- [Checkbox Items](#checkbox-items)
- [DropDown Items](#dropdown-items)
- [SplitButton Items](#splitbutton-items)
- [Item Sizing](#item-sizing)
- [Disabled Items](#disabled-items)
- [Display Modes](#display-modes)

## Button Items

### Basic Button

Render a simple clickable button:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save">
    </RibbonButtonSettings>
</RibbonItem>
```

### Button with Click Handler

Handle button clicks with event callback:

```razor
@using Syncfusion.Blazor.Ribbon

<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Save" 
                          IconCss="e-icons e-save"
                          OnClick="@SaveDocument">
    </RibbonButtonSettings>
</RibbonItem>

@code {
    private void SaveDocument(MouseEventArgs args)
    {
        // Handle save action
        Console.WriteLine("Save clicked!");
    }
}
```

### Toggle Button (IsToggle)

Create a button that toggles between active/inactive states:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Repeat" 
                          IconCss="e-icons e-repeat"
                          IsToggle="true"
                          OnClick="@ToggleRepeat">
    </RibbonButtonSettings>
</RibbonItem>

@code {
    private bool repeatEnabled = false;

    private void ToggleRepeat(MouseEventArgs args)
    {
        repeatEnabled = !repeatEnabled;
        Console.WriteLine($"Repeat: {repeatEnabled}");
    }
}
```

### Button Properties

```razor
<RibbonButtonSettings Content="Print"
                      IconCss="e-icons e-print"
                      IsToggle="false"
                      Disabled="false"
                      CssClass="custom-button">
</RibbonButtonSettings>
```

**Available Properties:**
- `Content` - Button display text
- `IconCss` - Icon CSS class (e-icons library)
- `IsToggle` - Enable toggle behavior
- `Disabled` - Disable the button
- `CssClass` - Custom CSS class for styling
- `OnClick` - Click event handler
- `Created` - Initialization event

## Checkbox Items

### Basic Checkbox

Render a checkbox with label:

```razor
<RibbonItem Type="RibbonItemType.CheckBox">
    <RibbonCheckBoxSettings Label="Show Ruler" Checked="true">
    </RibbonCheckBoxSettings>
</RibbonItem>
```

### Checkbox with Event Handler

Handle checkbox state changes:

```razor
@using Syncfusion.Blazor.Ribbon

<RibbonItem Type="RibbonItemType.CheckBox">
    <RibbonCheckBoxSettings Label="Show Ruler"
                            Checked="@showRuler"
                            ValueChange="@OnRulerChange">
    </RibbonCheckBoxSettings>
</RibbonItem>

@code {
    private bool showRuler = true;

    private void OnRulerChange(ChangeEventArgs<bool> args)
    {
        showRuler = args.Checked;
        Console.WriteLine($"Ruler visibility: {showRuler}");
    }
}
```

### Multiple Checkboxes (View Options)

Common pattern for display toggles:

```razor
<RibbonGroup HeaderText="Show" Orientation="Orientation.Column">
    <RibbonCollections>
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.CheckBox">
                    <RibbonCheckBoxSettings Label="Ruler" Checked="true" ValueChange="@OnRulerChange">
                    </RibbonCheckBoxSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.CheckBox">
                    <RibbonCheckBoxSettings Label="Gridlines" Checked="false" ValueChange="@OnGridChange">
                    </RibbonCheckBoxSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.CheckBox">
                    <RibbonCheckBoxSettings Label="Navigation Pane" Checked="true" ValueChange="@OnNavChange">
                    </RibbonCheckBoxSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>

@code {
    private void OnRulerChange(ChangeEventArgs<bool> args) => Console.WriteLine($"Ruler: {args.Checked}");
    private void OnGridChange(ChangeEventArgs<bool> args) => Console.WriteLine($"Grid: {args.Checked}");
    private void OnNavChange(ChangeEventArgs<bool> args) => Console.WriteLine($"Nav: {args.Checked}");
}
```

### Checkbox Properties

```razor
<RibbonCheckBoxSettings Label="Show Gridlines"
                        Checked="false"
                        Disabled="false"
                        CssClass="custom-checkbox"
                        ValueChange="@HandleChange"
                        Created="@OnCreated">
</RibbonCheckBoxSettings>
```

**Available Properties:**
- `Label` - Checkbox label text
- `Checked` - Initial state
- `Disabled` - Disable the checkbox
- `CssClass` - Custom CSS class
- `ValueChange` - Change event handler
- `Created` - Initialization event

## DropDown Items

### Basic DropDown

Display a dropdown menu with options:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<RibbonItem Type="RibbonItemType.DropDown">
    <RibbonDropDownSettings Content="Header" 
                            IconCss="e-icons e-header" 
                            Items="@headerOptions">
    </RibbonDropDownSettings>
</RibbonItem>

@code {
    private List<DropDownMenuItem> headerOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Insert Header" },
        new DropDownMenuItem{ Text = "Edit Header" },
        new DropDownMenuItem{ Text = "Remove Header" }
    };
}
```

### DropDown with Selection Handler

Handle item selection:

```razor
<RibbonItem Type="RibbonItemType.DropDown">
    <RibbonDropDownSettings Content="Style" 
                            IconCss="e-icons e-style" 
                            Items="@styleOptions"
                            ItemSelecting="@OnStyleSelect">
    </RibbonDropDownSettings>
</RibbonItem>

@code {
    private List<DropDownMenuItem> styleOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Normal" },
        new DropDownMenuItem{ Text = "Heading 1" },
        new DropDownMenuItem{ Text = "Heading 2" },
        new DropDownMenuItem{ Text = "Title" }
    };

    private void OnStyleSelect(DropDownItemSelectEventArgs args)
    {
        Console.WriteLine($"Selected style: {args.Item.Text}");
    }
}
```

### DropDown Events

```razor
<RibbonDropDownSettings Content="Format"
                        Items="@formatOptions"
                        Created="@OnCreated"
                        PopupOpening="@OnPopupOpen"
                        PopupOpened="@OnPopupOpened"
                        PopupClosing="@OnPopupClose"
                        PopupClosed="@OnPopupClosed"
                        ItemRendering="@OnItemRender"
                        ItemSelecting="@OnItemSelect">
</RibbonDropDownSettings>

@code {
    private void OnCreated() => Console.WriteLine("DropDown created");
    private void OnPopupOpen(DropDownPopupOpenEventArgs args) => Console.WriteLine("Popup opening");
    private void OnPopupOpened(DropDownPopupOpenedEventArgs args) => Console.WriteLine("Popup opened");
    private void OnPopupClose(DropDownPopupCloseEventArgs args) => Console.WriteLine("Popup closing");
    private void OnPopupClosed(DropDownPopupClosedEventArgs args) => Console.WriteLine("Popup closed");
    private void OnItemRender(DropDownItemRenderEventArgs args) => Console.WriteLine($"Rendering: {args.Item.Text}");
    private void OnItemSelect(DropDownItemSelectEventArgs args) => Console.WriteLine($"Selected: {args.Item.Text}");
}
```

## SplitButton Items

### Basic SplitButton

Combine a primary button with a dropdown menu:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<RibbonItem Type="RibbonItemType.SplitButton">
    <RibbonSplitButtonSettings Content="Paste" 
                               IconCss="e-icons e-paste" 
                               Items="@pasteOptions">
    </RibbonSplitButtonSettings>
</RibbonItem>

@code {
    private List<DropDownMenuItem> pasteOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Keep Source Format" },
        new DropDownMenuItem{ Text = "Merge Format" },
        new DropDownMenuItem{ Text = "Keep Text Only" }
    };
}
```

### SplitButton with Primary Action

Handle both primary button click and menu selection:

```razor
<RibbonItem Type="RibbonItemType.SplitButton">
    <RibbonSplitButtonSettings Content="Paste" 
                               IconCss="e-icons e-paste" 
                               Items="@pasteOptions"
                               Clicked="@OnPasteClick"
                               ItemSelected="@OnPasteOptionSelect">
    </RibbonSplitButtonSettings>
</RibbonItem>

@code {
    private List<DropDownMenuItem> pasteOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Keep Source Format" },
        new DropDownMenuItem{ Text = "Merge Format" },
        new DropDownMenuItem{ Text = "Keep Text Only" }
    };

    private void OnPasteClick(SplitButtonClickedEventArgs args)
    {
        Console.WriteLine("Primary paste action");
    }

    private void OnPasteOptionSelect(SplitButtonItemSelectedEventArgs args)
    {
        Console.WriteLine($"Paste format selected: {args.Item.Text}");
    }
}
```

### SplitButton Events

```razor
<RibbonSplitButtonSettings Content="Paste"
                           Items="@pasteOptions"
                           Created="@OnCreated"
                           Clicked="@OnClicked"
                           PopupOpening="@OnPopupOpen"
                           PopupOpened="@OnPopupOpened"
                           PopupClosing="@OnPopupClose"
                           PopupClosed="@OnPopupClosed"
                           ItemRendering="@OnItemRender"
                           ItemSelected="@OnItemSelect">
</RibbonSplitButtonSettings>

@code {
    private void OnCreated() => Console.WriteLine("SplitButton created");
    private void OnClicked(SplitButtonClickedEventArgs args) => Console.WriteLine("Main button clicked");
    private void OnPopupOpen(SplitButtonPopupOpenEventArgs args) => Console.WriteLine("Popup opening");
    private void OnPopupOpened(SplitButtonPopupOpenedEventArgs args) => Console.WriteLine("Popup opened");
    private void OnPopupClose(SplitButtonPopupCloseEventArgs args) => Console.WriteLine("Popup closing");
    private void OnPopupClosed(SplitButtonPopupClosedEventArgs args) => Console.WriteLine("Popup closed");
    private void OnItemRender(SplitButtonItemRenderEventArgs args) => Console.WriteLine($"Rendering: {args.Item.Text}");
    private void OnItemSelect(SplitButtonItemSelectedEventArgs args) => Console.WriteLine($"Selected: {args.Item.Text}");
}
```

## Item Sizing

### Size Options

Control item visual size:

```razor
<!-- Large button (prominent primary action) -->
<RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Large">
    <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
    </RibbonButtonSettings>
</RibbonItem>

<!-- Medium button (default) -->
<RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Medium">
    <RibbonButtonSettings Content="Copy">
    </RibbonButtonSettings>
</RibbonItem>

<!-- Small button (secondary action) -->
<RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
    <RibbonButtonSettings Content="Cut">
    </RibbonButtonSettings>
</RibbonItem>
```

### Sizing Pattern

Primary actions (Large) → Secondary actions (Small):

```razor
<RibbonGroup HeaderText="Clipboard">
    <RibbonCollections>
        <!-- Collection 1: Large primary action -->
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.SplitButton" AllowedSizes="RibbonItemSize.Large">
                    <RibbonSplitButtonSettings Content="Paste" Items="@pasteOptions">
                    </RibbonSplitButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
        <!-- Collection 2: Small secondary actions -->
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
                    </RibbonButtonSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy">
                    </RibbonButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

## Disabled Items

### Disable Individual Items

```razor
<RibbonItem Type="RibbonItemType.Button" Disabled="true">
    <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
    </RibbonButtonSettings>
</RibbonItem>
```

### Dynamic Enable/Disable

Toggle item state based on conditions:

```razor
@using Syncfusion.Blazor.Ribbon

<button @onclick="TogglePaste">Toggle Paste Button</button>

<RibbonItem Type="RibbonItemType.Button" Disabled="@disablePaste">
    <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
    </RibbonButtonSettings>
</RibbonItem>

@code {
    private bool disablePaste = false;

    private void TogglePaste()
    {
        disablePaste = !disablePaste;
    }
}
```

## Display Modes

### DisplayOptions

Control item visibility across different ribbon layouts:

```razor
<!-- Always visible (default) -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Auto">
    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
    </RibbonButtonSettings>
</RibbonItem>

<!-- Only in classic (expanded) layout -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Classic">
    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy">
    </RibbonButtonSettings>
</RibbonItem>

<!-- Only in simplified layout -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Simplified">
    <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
    </RibbonButtonSettings>
</RibbonItem>

<!-- Only in overflow popup -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Overflow">
    <RibbonButtonSettings Content="Special" IconCss="e-icons e-special">
    </RibbonButtonSettings>
</RibbonItem>
```

## Complete Example

Full example with various basic item types:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <!-- Clipboard Group -->
                <RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
                    <RibbonCollections>
                        <!-- Paste (SplitButton, Large) -->
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.SplitButton" AllowedSizes="RibbonItemSize.Large">
                                    <RibbonSplitButtonSettings Content="Paste" IconCss="e-icons e-paste" Items="@pasteOptions" Clicked="@OnPaste">
                                    </RibbonSplitButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                        <!-- Cut, Copy (Buttons, Small) -->
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut" OnClick="@OnCut">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                                    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy" OnClick="@OnCopy">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small" Disabled="true">
                                    <RibbonButtonSettings Content="Format" IconCss="e-icons e-format">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>

                <!-- Show Group (Checkboxes) -->
                <RibbonGroup HeaderText="Show" Orientation="Orientation.Column">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.CheckBox">
                                    <RibbonCheckBoxSettings Label="Ruler" Checked="true">
                                    </RibbonCheckBoxSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.CheckBox">
                                    <RibbonCheckBoxSettings Label="Gridlines" Checked="false">
                                    </RibbonCheckBoxSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>

                <!-- Header Group (Dropdown) -->
                <RibbonGroup HeaderText="Header/Footer">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.DropDown">
                                    <RibbonDropDownSettings Content="Header" IconCss="e-icons e-header" Items="@headerOptions" ItemSelecting="@OnHeaderSelect">
                                    </RibbonDropDownSettings>
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
    private List<DropDownMenuItem> pasteOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Keep Source Format" },
        new DropDownMenuItem{ Text = "Merge Format" },
        new DropDownMenuItem{ Text = "Keep Text Only" }
    };

    private List<DropDownMenuItem> headerOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Insert Header" },
        new DropDownMenuItem{ Text = "Edit Header" },
        new DropDownMenuItem{ Text = "Remove Header" }
    };

    private void OnPaste(SplitButtonClickedEventArgs args) => Console.WriteLine("Paste");
    private void OnCut(MouseEventArgs args) => Console.WriteLine("Cut");
    private void OnCopy(MouseEventArgs args) => Console.WriteLine("Copy");
    private void OnHeaderSelect(DropDownItemSelectEventArgs args) => Console.WriteLine($"Header: {args.Item.Text}");
}
```

This covers the fundamental ribbon item types used in most applications.
