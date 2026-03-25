# Events and Interactions

## Table of Contents
- [Component-Level Events](#component-level-events)
- [Tab Events](#tab-events)
- [Item Events](#item-events)
- [File Menu Events](#file-menu-events)
- [Event Patterns](#event-patterns)
- [Complete Example](#complete-example)

## Component-Level Events

### Created Event

Fires when ribbon component is fully initialized:

```razor
<SfRibbon Created="@OnRibbonCreated">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnRibbonCreated()
    {
        Console.WriteLine("Ribbon component created and initialized");
    }
}
```

### TabSelecting Event

Fires before a tab is selected:

```razor
<SfRibbon TabSelecting="@OnTabSelecting">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnTabSelecting(TabSelectingEventArgs args)
    {
        Console.WriteLine($"Selecting tab: {args.SelectedTab}");
        // args.Cancel = true;  // To prevent tab selection
    }
}
```

### TabSelected Event

Fires after a tab is successfully selected:

```razor
<SfRibbon TabSelected="@OnTabSelected">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnTabSelected(TabSelectedEventArgs args)
    {
        Console.WriteLine($"Tab selected: {args.SelectedTab}");
    }
}
```

## Ribbon State Events

### RibbonExpanding Event

Fires before ribbon expands from collapsed state:

```razor
<SfRibbon RibbonExpanding="@OnRibbonExpanding">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnRibbonExpanding(ExpandEventArgs args)
    {
        Console.WriteLine("Ribbon expanding");
    }
}
```

### RibbonCollapsing Event

Fires before ribbon collapses:

```razor
<SfRibbon RibbonCollapsing="@OnRibbonCollapsing">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnRibbonCollapsing(CollapseEventArgs args)
    {
        Console.WriteLine("Ribbon collapsing");
    }
}
```

## Overflow and Launcher Events

### LauncherIconClick Event

Fires when launcher icon is clicked in group:

```razor
<SfRibbon LauncherIconClick="@OnLauncherClick">
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Paragraph" LauncherIcon="e-icons e-launcher">
                    <!-- Group items -->
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    private void OnLauncherClick(LauncherClickEventArgs args)
    {
        Console.WriteLine($"Launcher clicked for group: {args.GroupName}");
        // Open dialog for detailed settings
    }
}
```

### OverflowPopupOpening Event

Fires before overflow popup opens:

```razor
<SfRibbon OverflowPopupOpening="@OnOverflowOpen">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnOverflowOpen(OverflowPopupEventArgs args)
    {
        Console.WriteLine("Overflow popup opening");
    }
}
```

### OverflowPopupClosing Event

Fires before overflow popup closes:

```razor
<SfRibbon OverflowPopupClosing="@OnOverflowClose">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnOverflowClose(OverflowPopupEventArgs args)
    {
        Console.WriteLine("Overflow popup closing");
    }
}
```

## Item Events

### Button Item Events

**OnClick** - When button is clicked:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Save" OnClick="@OnSaveClick">
    </RibbonButtonSettings>
</RibbonItem>

@code {
    private void OnSaveClick(MouseEventArgs args)
    {
        Console.WriteLine("Save button clicked");
    }
}
```

**Created** - When button is initialized:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Cut" Created="@OnButtonCreated">
    </RibbonButtonSettings>
</RibbonItem>

@code {
    private void OnButtonCreated()
    {
        Console.WriteLine("Button created");
    }
}
```

### CheckBox Item Events

**ValueChange** - When checkbox is toggled:

```razor
<RibbonItem Type="RibbonItemType.CheckBox">
    <RibbonCheckBoxSettings Label="Show Ruler" ValueChange="@OnRulerChange">
    </RibbonCheckBoxSettings>
</RibbonItem>

@code {
    private void OnRulerChange(ChangeEventArgs<bool> args)
    {
        Console.WriteLine($"Ruler visibility: {args.Checked}");
    }
}
```

### DropDown Item Events

**ItemSelecting** - When dropdown item is selected:

```razor
<RibbonItem Type="RibbonItemType.DropDown">
    <RibbonDropDownSettings Content="Style" Items="@styleOptions" ItemSelecting="@OnStyleSelect">
    </RibbonDropDownSettings>
</RibbonItem>

@code {
    private void OnStyleSelect(DropDownItemSelectEventArgs args)
    {
        Console.WriteLine($"Style selected: {args.Item.Text}");
    }
}
```

**PopupOpening / PopupOpened** - Dropdown menu lifecycle:

```razor
<RibbonDropDownSettings Content="Style"
                        Items="@styleOptions"
                        PopupOpening="@OnPopupOpen"
                        PopupOpened="@OnPopupOpened"
                        PopupClosing="@OnPopupClose"
                        PopupClosed="@OnPopupClosed">
</RibbonDropDownSettings>

@code {
    private void OnPopupOpen(DropDownPopupOpenEventArgs args) 
        => Console.WriteLine("Dropdown opening");
    
    private void OnPopupOpened(DropDownPopupOpenedEventArgs args) 
        => Console.WriteLine("Dropdown opened");
    
    private void OnPopupClose(DropDownPopupCloseEventArgs args) 
        => Console.WriteLine("Dropdown closing");
    
    private void OnPopupClosed(DropDownPopupClosedEventArgs args) 
        => Console.WriteLine("Dropdown closed");
}
```

### SplitButton Item Events

**Clicked** - Primary button click:

```razor
<RibbonItem Type="RibbonItemType.SplitButton">
    <RibbonSplitButtonSettings Content="Paste" Items="@pasteOptions" Clicked="@OnPasteClick">
    </RibbonSplitButtonSettings>
</RibbonItem>

@code {
    private void OnPasteClick(SplitButtonClickedEventArgs args)
    {
        Console.WriteLine("Paste main button clicked");
    }
}
```

**ItemSelected** - Menu item selected:

```razor
<RibbonSplitButtonSettings Content="Paste"
                           Items="@pasteOptions"
                           ItemSelected="@OnPasteFormat">
</RibbonSplitButtonSettings>

@code {
    private void OnPasteFormat(SplitButtonItemSelectedEventArgs args)
    {
        Console.WriteLine($"Paste format: {args.Item.Text}");
    }
}
```

### ComboBox Item Events

**ValueChange** - Selected value changed:

```razor
<RibbonItem Type="RibbonItemType.ComboBox">
    <RibbonComboBoxSettings DataSource="@fonts"
                            FieldSettings="@fieldSettings"
                            ValueChange="@OnFontChange">
    </RibbonComboBoxSettings>
</RibbonItem>

@code {
    private void OnFontChange(ComboBoxChangeEventArgs args)
    {
        Console.WriteLine($"Font changed: {args.Value}");
    }
}
```

**Filtering** - Text filtered in dropdown:

```razor
<RibbonComboBoxSettings AllowFiltering="true"
                        Filtering="@OnFilter">
</RibbonComboBoxSettings>

@code {
    private void OnFilter(ComboBoxFilterEventArgs args)
    {
        Console.WriteLine($"Filtering: {args.Text}");
    }
}
```

### ColorPicker Item Events

**ValueChange** - Color selected:

```razor
<RibbonItem Type="RibbonItemType.ColorPicker">
    <RibbonColorPickerSettings ValueChange="@OnColorChange">
    </RibbonColorPickerSettings>
</RibbonItem>

@code {
    private void OnColorChange(ColorPickerEventArgs args)
    {
        Console.WriteLine($"Color: {args.Value}");
    }
}
```

### GroupButton Item Events

**ItemClick** - Button in group clicked:

```razor
<RibbonItem Type="RibbonItemType.GroupButton">
    <RibbonGroupButtonSettings Items="@alignOptions" ItemClick="@OnAlign">
    </RibbonGroupButtonSettings>
</RibbonItem>

@code {
    private void OnAlign(GroupButtonClickEventArgs args)
    {
        Console.WriteLine($"Alignment: {args.Item.Content}");
    }
}
```

## Event Patterns

### Complete Ribbon with All Events

```razor
@page "/ribbon-events"
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<div style="margin-bottom: 20px; padding: 10px; background-color: #f0f0f0; border-radius: 4px;">
    <h3>Event Log</h3>
    <div style="max-height: 150px; overflow-y: auto; background-color: white; padding: 10px; border: 1px solid #ccc;">
        @foreach (var log in eventLogs)
        {
            <div>@log</div>
        }
    </div>
</div>

<SfRibbon Created="@OnCreated"
          TabSelecting="@OnTabSelecting"
          TabSelected="@OnTabSelected"
          RibbonExpanding="@OnExpanding"
          RibbonCollapsing="@OnCollapsing"
          LauncherIconClick="@OnLauncher">
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Clipboard">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut" OnClick="@OnCut">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
        <RibbonTab HeaderText="Insert">
            <RibbonGroups>
                <RibbonGroup HeaderText="Text">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Text Box" OnClick="@OnInsertText">
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
    private List<string> eventLogs = new List<string>();

    private void LogEvent(string message)
    {
        eventLogs.Insert(0, $"[{DateTime.Now:HH:mm:ss}] {message}");
        if (eventLogs.Count > 20) eventLogs.RemoveAt(eventLogs.Count - 1);
        StateHasChanged();
    }

    private void OnCreated() => LogEvent("Ribbon created");
    private void OnTabSelecting(TabSelectingEventArgs args) => LogEvent($"Tab selecting: {args.SelectedTab}");
    private void OnTabSelected(TabSelectedEventArgs args) => LogEvent($"Tab selected: {args.SelectedTab}");
    private void OnExpanding(ExpandEventArgs args) => LogEvent("Ribbon expanding");
    private void OnCollapsing(CollapseEventArgs args) => LogEvent("Ribbon collapsing");
    private void OnLauncher(LauncherClickEventArgs args) => LogEvent($"Launcher clicked: {args.GroupName}");
    private void OnCut(MouseEventArgs args) => LogEvent("Cut clicked");
    private void OnInsertText(MouseEventArgs args) => LogEvent("Insert Text clicked");
}
```

## Preventing Default Actions

Cancel events to prevent default behavior:

```razor
<SfRibbon TabSelecting="@OnTabSelectingWithCancel">
    <!-- Ribbon content -->
</SfRibbon>

@code {
    private void OnTabSelectingWithCancel(TabSelectingEventArgs args)
    {
        if (args.SelectedTab == "Insert" && !userHasPermission)
        {
            args.Cancel = true;  // Prevent switching to Insert tab
            Console.WriteLine("Tab switch cancelled - insufficient permissions");
        }
    }

    private bool userHasPermission = false;
}
```

## Best Practices

### 1. Use StateHasChanged() for Dynamic Updates
```csharp
private void OnItemChange()
{
    // Update component state
    isProcessing = true;
    StateHasChanged();
}
```

### 2. Debounce Frequent Events
```csharp
private System.Threading.Timer debounceTimer;

private void OnFilterChange(ComboBoxFilterEventArgs args)
{
    debounceTimer?.Dispose();
    debounceTimer = new System.Threading.Timer(_ => 
    {
        Console.WriteLine($"Filtering: {args.Text}");
    }, null, 300, System.Threading.Timeout.Infinite);
}
```

### 3. Log Events for Debugging
Keep an event log for troubleshooting user actions.

### 4. Validate Before Processing
```csharp
private void OnSave()
{
    if (ValidateForm())
    {
        SaveDocument();
    }
}
```
