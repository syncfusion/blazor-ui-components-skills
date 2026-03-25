# Gallery Items in Blazor Ribbon Component

The Ribbon supports a gallery view, allowing users to interact with a collection of related items such as icons, text, or images. Gallery items can display preset styles, themes, or formatting options in a visually organized grid layout.

## When to Use Gallery

- Displaying style or formatting options (e.g., table styles, paragraph styles)
- Showing preset themes or templates
- Presenting a collection of visual items for selection
- Offering quick access to commonly used design elements

## Basic Gallery Implementation

```razor
<RibbonItem Type="RibbonItemType.Gallery">
    <RibbonGallerySettings Groups="@galleryGroups">
    </RibbonGallerySettings>
</RibbonItem>
```

## Gallery Structure

A gallery consists of:
- **Groups**: Logical groupings within the gallery
- **Items**: Individual selectable items within each group
- **Popup**: Extended view showing all available options
- **Templates**: Custom rendering for items and popup

## Key Properties

### Gallery Settings
- `Groups` - Collection of gallery groups to display
- `ItemCount` - Number of items to show in the ribbon (default: 3)
- `SelectedItemIndex` - Index of the currently selected item
- `PopupWidth` - Width of the gallery popup
- `PopupHeight` - Height of the gallery popup
- `Template` - Custom template for gallery items
- `PopupTemplate` - Custom template for gallery popup

### Gallery Group
- `Header` - Title for the group in popup
- `Items` - Collection of gallery items
- `ItemWidth` - Width of each item
- `ItemHeight` - Height of each item
- `CssClass` - Custom CSS class for styling

### Gallery Item
- `Content` - Text content to display
- `IconCss` - Icon class for the item
- `Disabled` - Whether the item is disabled
- `CssClass` - Custom CSS class for individual styling

## Example: Style Gallery

```razor
<RibbonGroup HeaderText="Styles">
    <RibbonCollections>
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Gallery">
                    <RibbonGallerySettings 
                        ItemCount="4"
                        Groups="@styleGroups"
                        @bind-SelectedItemIndex="@selectedStyleIndex">
                    </RibbonGallerySettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>

@code {
    private int selectedStyleIndex = 0;

    private List<GalleryGroup> styleGroups = new List<GalleryGroup>
    {
        new GalleryGroup
        {
            Header = "Paragraph Styles",
            ItemWidth = "100",
            ItemHeight = "40",
            Items = new List<GalleryItem>
            {
                new GalleryItem { Content = "Normal" },
                new GalleryItem { Content = "Heading 1" },
                new GalleryItem { Content = "Heading 2" },
                new GalleryItem { Content = "Title" }
            }
        }
    };
}
```

## Gallery with Multiple Groups

```razor
<RibbonItem Type="RibbonItemType.Gallery">
    <RibbonGallerySettings Groups="@transitionGroups">
    </RibbonGallerySettings>
</RibbonItem>

@code {
    private List<GalleryGroup> transitionGroups = new List<GalleryGroup>
    {
        new GalleryGroup
        {
            Header = "Subtle Transitions",
            Items = new List<GalleryItem>
            {
                new GalleryItem { Content = "None", IconCss = "e-icons e-rectangle" },
                new GalleryItem { Content = "Fade", IconCss = "e-icons e-send-backward" },
                new GalleryItem { Content = "Reveal", IconCss = "e-icons e-bring-forward" }
            }
        },
        new GalleryGroup
        {
            Header = "Exciting Transitions",
            Items = new List<GalleryItem>
            {
                new GalleryItem { Content = "Zoom", IconCss = "e-icons e-zoom-to-fit" },
                new GalleryItem { Content = "Wipe", IconCss = "e-icons e-send-backward" },
                new GalleryItem { Content = "Pop", IconCss = "e-icons e-bring-forward" }
            }
        }
    };
}
```

## Customizing Gallery Items

### Using CSS Classes

```razor
<RibbonItem Type="RibbonItemType.Gallery">
    <RibbonGallerySettings Groups="@customGroups">
    </RibbonGallerySettings>
</RibbonItem>

<style>
    .e-ribbon-gallery-item.status-good {
        background: #c7ebc9;
        border-left: 4px solid #107c10;
    }

    .e-ribbon-gallery-item.status-bad {
        background: #ffb6b6;
        border-left: 4px solid #d42b1f;
    }

    .e-ribbon-gallery-item.status-neutral {
        background: #eedd9d;
        border-left: 4px solid #ffb900;
    }
</style>

@code {
    private List<GalleryGroup> customGroups = new List<GalleryGroup>
    {
        new GalleryGroup
        {
            Items = new List<GalleryItem>
            {
                new GalleryItem { Content = "Good", CssClass = "status-good" },
                new GalleryItem { Content = "Bad", CssClass = "status-bad" },
                new GalleryItem { Content = "Neutral", CssClass = "status-neutral" }
            }
        }
    };
}
```

## Gallery Events

The following events are available:

| Event | Args | Description |
|-------|------|-------------|
| PopupOpening | GalleryPopupOpenEventArgs | Triggers before gallery popup opens |
| PopupClosing | GalleryPopupCloseEventArgs | Triggers before gallery popup closes |
| ItemHover | GalleryItemHoverEventArgs | Triggers when hovering over an item |
| ItemRendering | GalleryItemRenderEventArgs | Triggers during item rendering |
| Selecting | GallerySelectEventArgs | Triggers before item selection |
| Selected | GallerySelectedEventArgs | Triggers after item selection |

### Event Handler Example

```razor
<RibbonItem Type="RibbonItemType.Gallery">
    <RibbonGallerySettings 
        Groups="@galleryGroups"
        Selected="OnGalleryItemSelected"
        PopupOpening="OnPopupOpening">
    </RibbonGallerySettings>
</RibbonItem>

@code {
    private void OnGalleryItemSelected(GallerySelectedEventArgs args)
    {
        // Handle item selection
        Console.WriteLine($"Selected item: {args.Item.Content}");
    }

    private void OnPopupOpening(GalleryPopupOpenEventArgs args)
    {
        // Handle popup opening
        Console.WriteLine("Gallery popup is opening");
    }
}
```

## Gallery Popup Configuration

```razor
<RibbonItem Type="RibbonItemType.Gallery">
    <RibbonGallerySettings 
        PopupWidth="400"
        PopupHeight="300"
        Groups="@galleryGroups">
    </RibbonGallerySettings>
</RibbonItem>
```

## Custom Gallery Templates

```razor
<RibbonItem Type="RibbonItemType.Gallery">
    <Template Context="context">
        <div class="custom-gallery-item">
            <span>@context.Content</span>
        </div>
    </Template>
    <PopupTemplate Context="context">
        <div class="custom-gallery-popup">
            <table>
                <tr>
                    <td>@context.Content</td>
                </tr>
            </table>
        </div>
    </PopupTemplate>
</RibbonItem>
```

## Best Practices

- **Limit visible items**: Keep the `ItemCount` small (2-5) for ribbon display
- **Use groups**: Organize related items into logical groups in the popup
- **Provide clear labels**: Use descriptive content text for gallery items
- **Set appropriate dimensions**: Adjust `ItemWidth` and `ItemHeight` for consistent appearance
- **Handle selection**: Implement `Selected` event to respond to user choices
- **Use icons**: Combine icons with text for visual clarity
- **Template consistency**: Ensure custom templates match gallery item structure

## Common Patterns

### Table Styles Gallery
Display preset table formatting styles for quick selection.

### Paragraph Styles Gallery
Show built-in or custom paragraph styles for formatting.

### Color Schemes Gallery
Present color palette options for theme selection.

### Shape Gallery
Display shape options for drawing or inserting.

## Performance Tips

- Limit the number of items per group to maintain performance
- Use templates judiciously to avoid excessive rendering
- Implement lazy loading for large galleries if needed
- Cache gallery groups to avoid recreation on each render

## Related Components

- **File Menu** - For file operations and settings
- **Contextual Tabs** - For context-specific options
- **Backstage View** - For application settings and information
