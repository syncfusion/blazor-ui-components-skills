# Contextual Tabs in Blazor Ribbon Component

Contextual tabs are tabs that appear dynamically based on context, such as when a user selects an image, table, or other object. They provide task-specific commands that are only relevant when certain conditions are met.

## When to Use Contextual Tabs

- Image selection (e.g., Picture Format tab in Office)
- Table selection (e.g., Table Design tab in Office)
- Text object selection
- Chart or diagram selection
- Any context-dependent command set

## Basic Contextual Tab

```razor
<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <!-- Regular ribbon tabs -->
        </RibbonTab>
    </RibbonTabs>
    
    <RibbonContextualTabs>
        <RibbonContextualTab @bind-Visible="@isImageSelected">
            <RibbonTabs>
                <RibbonTab HeaderText="Picture Format">
                    <!-- Picture-specific commands -->
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>
    </RibbonContextualTabs>
</SfRibbon>

@code {
    private bool isImageSelected = false;
}
```

## Contextual Tab Properties

### RibbonContextualTab
- `Visible` - Show/hide the contextual tab and its contents
- `IsSelected` - Set the tab as the active/selected tab
- `RibbonTabs` - Collection of tabs within the contextual tab

## Showing/Hiding Contextual Tabs

### Using Visible Property

```razor
<SfRibbon>
    <RibbonContextualTabs>
        <RibbonContextualTab @bind-Visible="@isTableSelected">
            <RibbonTabs>
                <RibbonTab HeaderText="Table Design" ID="TableDesign">
                    <RibbonGroups>
                        <RibbonGroup HeaderText="Table Styles">
                            <!-- Table styling options -->
                        </RibbonGroup>
                    </RibbonGroups>
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>
    </RibbonContextualTabs>
</SfRibbon>

@code {
    private bool isTableSelected = false;

    private void OnTableInserted()
    {
        isTableSelected = true;
    }

    private void OnTableRemoved()
    {
        isTableSelected = false;
    }
}
```

### Using ShowTabAsync/HideTabAsync Methods

```razor
<SfButton @onclick="ShowImageFormatTab">Select Image</SfButton>
<SfButton @onclick="HideImageFormatTab">Deselect Image</SfButton>

<SfRibbon @ref="ribbon">
    <RibbonContextualTabs>
        <RibbonContextualTab @bind-Visible="@isImageVisible">
            <RibbonTabs>
                <RibbonTab HeaderText="Picture Format" ID="PictureFormat">
                    <RibbonGroups>
                        <RibbonGroup HeaderText="Adjust">
                            <!-- Image adjustment options -->
                        </RibbonGroup>
                    </RibbonGroups>
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>
    </RibbonContextualTabs>
</SfRibbon>

@code {
    SfRibbon ribbon;
    private bool isImageVisible = false;

    private async Task ShowImageFormatTab()
    {
        isImageVisible = true;
        await ribbon.ShowTabAsync("PictureFormat");
    }

    private async Task HideImageFormatTab()
    {
        isImageVisible = false;
        await ribbon.HideTabAsync("PictureFormat");
    }
}
```

## Multiple Contextual Tabs

```razor
<SfRibbon>
    <RibbonContextualTabs>
        <RibbonContextualTab @bind-Visible="@isImageSelected" @bind-IsSelected="@isImageTabActive">
            <RibbonTabs>
                <RibbonTab HeaderText="Picture Format">
                    <RibbonGroups>
                        <RibbonGroup HeaderText="Picture Styles">
                            <!-- Image styling options -->
                        </RibbonGroup>
                    </RibbonGroups>
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>

        <RibbonContextualTab @bind-Visible="@isTableSelected" @bind-IsSelected="@isTableTabActive">
            <RibbonTabs>
                <RibbonTab HeaderText="Table Design">
                    <RibbonGroups>
                        <RibbonGroup HeaderText="Table Styles">
                            <!-- Table styling options -->
                        </RibbonGroup>
                    </RibbonGroups>
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>

        <RibbonContextualTab @bind-Visible="@isChartSelected" @bind-IsSelected="@isChartTabActive">
            <RibbonTabs>
                <RibbonTab HeaderText="Chart Design">
                    <RibbonGroups>
                        <RibbonGroup HeaderText="Chart Layouts">
                            <!-- Chart layout options -->
                        </RibbonGroup>
                    </RibbonGroups>
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>
    </RibbonContextualTabs>
</SfRibbon>

@code {
    private bool isImageSelected = false;
    private bool isImageTabActive = false;

    private bool isTableSelected = false;
    private bool isTableTabActive = false;

    private bool isChartSelected = false;
    private bool isChartTabActive = false;
}
```

## Setting Selected State

Use `IsSelected` to automatically activate a contextual tab when it becomes visible:

```razor
<RibbonContextualTab @bind-Visible="@isImageSelected" @bind-IsSelected="@true">
    <RibbonTabs>
        <RibbonTab HeaderText="Picture Format">
            <!-- Commands automatically shown when image selected -->
        </RibbonTab>
    </RibbonTabs>
</RibbonContextualTab>

@code {
    private bool isImageSelected = false;
}
```

## Contextual Tab Example: Image Selection

```razor
<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Insert">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings 
                                        Content="Insert Image" 
                                        IconCss="e-icons e-image"
                                        OnClick="@InsertImage">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>

    <RibbonContextualTabs>
        <RibbonContextualTab @bind-Visible="@isImageSelected" @bind-IsSelected="@isImageTabActive">
            <RibbonTabs>
                <RibbonTab HeaderText="Picture Format" ID="PictureFormat">
                    <RibbonGroups>
                        <RibbonGroup HeaderText="Adjust">
                            <RibbonCollections>
                                <RibbonCollection>
                                    <RibbonItems>
                                        <RibbonItem Type="RibbonItemType.Button">
                                            <RibbonButtonSettings Content="Brightness" IconCss="e-icons e-brightness"></RibbonButtonSettings>
                                        </RibbonItem>
                                        <RibbonItem Type="RibbonItemType.Button">
                                            <RibbonButtonSettings Content="Contrast" IconCss="e-icons e-contrast"></RibbonButtonSettings>
                                        </RibbonItem>
                                    </RibbonItems>
                                </RibbonCollection>
                            </RibbonCollections>
                        </RibbonGroup>
                    </RibbonGroups>
                </RibbonTab>
            </RibbonTabs>
        </RibbonContextualTab>
    </RibbonContextualTabs>
</SfRibbon>

@code {
    private bool isImageSelected = false;
    private bool isImageTabActive = false;

    private void InsertImage()
    {
        isImageSelected = true;
        isImageTabActive = true;
    }
}
```

## Best Practices

- **Logical grouping**: Group related contextual tabs for similar object types
- **Clear naming**: Use descriptive tab names that indicate the object type (e.g., "Picture Format")
- **Auto-selection**: Set `IsSelected="true"` to automatically activate when visible
- **Exclusive contexts**: Show only relevant contextual tabs for the current selection
- **Keyboard navigation**: Ensure contextual tabs are keyboard accessible
- **Visual feedback**: Provide clear indication of which object is selected
- **Consistent commands**: Place similar commands in same location across contextual tabs

## Performance Considerations

- Define contextual tabs once, reuse by toggling `Visible` property
- Avoid creating/destroying contextual tabs dynamically
- Lazy load complex content if needed
- Monitor number of contextual tabs to maintain UI responsiveness

## Contextual Tab Methods

### ShowTabAsync
Programmatically show a contextual tab.

```csharp
await ribbon.ShowTabAsync("TabID");
```

### HideTabAsync
Programmatically hide a contextual tab.

```csharp
await ribbon.HideTabAsync("TabID");
```

## Related Components

- **Gallery** - For style/template selection within contextual tabs
- **File Menu** - For file operations
- **Backstage View** - For application settings
- **KeyTips** - For keyboard navigation of contextual tabs
