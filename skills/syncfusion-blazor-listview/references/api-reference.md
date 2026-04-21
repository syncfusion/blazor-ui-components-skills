# Syncfusion Blazor ListView API Reference

## Overview

The `SfListView<TValue>` component displays data in an interactive hierarchical structure with features like data-binding, templates, grouping, virtualization, and selection capabilities. This API reference covers all properties, methods, and events.

## Table of Contents

- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Event Arguments Reference](#event-arguments-reference)
  - [ClickEventArgs](#clickeventargst)
  - [ActionEventsArgs](#actioneventsargs)
  - [ActionFailureEventsArgs](#actionfailureeventsargs)
  - [BackEventArgs](#backeventargst)
- [Field Settings](#field-settings)
- [Templates](#templates)
- [Supporting Enums and Classes](#supporting-enums-and-classes)
  - [AnimationSettings](#animationsettings)
  - [SortOrder Enum](#sortorder-enum)
  - [CheckBoxPosition Enum](#checkboxposition-enum)
  - [SelectedItems](#selecteditemstvalue-class)
- [Complete Usage Example](#complete-usage-example)
- [Additional Property Examples](#additional-property-examples)
  - [Property: Enabled](#property-enabled)
  - [Property: EnablePersistence](#property-enablepersistence)
  - [Property: EnableRtl](#property-enablertl)
  - [Property: HtmlAttributes](#property-htmlattributes)
  - [Property: ID](#property-id)
  - [Property: ShowIcon](#property-showicon)
  - [Property: Width](#property-width)
- [Navigation Guide](#navigation-guide)

---

## Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Animation` | AnimationSettings | SlideLeft (400ms) | Effect, Duration, Easing | Animation settings for nested navigation | [view](#animationsettings) |
| `CheckBoxPosition` | CheckBoxPosition | Left | Left, Right | Position of checkbox in list items | [view](item-selection-and-actions.md#checkbox-position-left-vs-right) |
| `CssClass` | string | String.Empty | Any CSS class(es) | Custom CSS class(es) for styling | [view](styling-and-customization.md#css-selectors-and-classes) |
| `DataSource` | IEnumerable<TValue> | null | Any collection | Data to be rendered in the ListView | [view](data-binding-and-source.md#local-data-arrays) |
| `Enabled` | bool | true | true, false | Enable/disable component interaction | [example](#property-enabled) |
| `EnablePersistence` | bool | false | true, false | Persist checked items between page reloads | [example](#property-enablepersistence) |
| `EnableRtl` | bool | false | true, false | Enable right-to-left text direction | [example](#property-enablertl) |
| `EnableVirtualization` | bool | false | true, false | Enable virtualization for large datasets | [view](virtualization-and-performance.md#enable-virtualization) |
| `GroupTemplate` | RenderFragment<ComposedItemModel<TValue>> | null | Custom HTML/Components | Custom template for group headers | [view](templating-and-rendering.md#group-header-templates) |
| `HeaderTemplate` | RenderFragment | null | Custom HTML/Components | Custom template for header title | [view](templating-and-rendering.md#header-template) |
| `HeaderTitle` | string | String.Empty | Any text | Header title text | [view](getting-started.md#listview-with-header) |
| `Height` | string | - | CSS height value | Component height (px, %, em, etc) | [view](virtualization-and-performance.md#enable-virtualization) |
| `HtmlAttributes` | Dictionary<string, object> | null | Key-value pairs | Additional HTML attributes for root element | [example](#property-htmlattributes) |
| `ID` | string | - | Any ID value | Component element ID | [example](#property-id) |
| `Query` | Query | null | Query with where/select | DataManager query for filtering/selection | [view](data-binding-and-source.md#odata-v4-integration) |
| `ShowCheckBox` | bool | false | true, false | Display/hide checkboxes in list items | [view](item-selection-and-actions.md#enable-checkbox-selection) |
| `ShowHeader` | bool | false | true, false | Display/hide header section | [view](getting-started.md#listview-with-header) |
| `ShowIcon` | bool | false | true, false | Display/hide list item icons | [example](#property-showicon) |
| `SortOrder` | SortOrder | None | None, Ascending, Descending | Sort order for list items | [view](#sortorder-enum) |
| `Template` | RenderFragment<TValue> | null | Custom HTML/Components | Custom template for list items | [view](templating-and-rendering.md#item-templates) |
| `Width` | string | - | CSS width value | Component width (px, %, em, etc) | [example](#property-width) |

---

## Methods

| Method | Parameters | Return Type | Description | Reference |
|--------|------------|-------------|-------------|-----------|
| `GetCheckedItemsAsync()` | None | Task<SelectedItems<TValue>> | Get all currently checked items | [view](item-selection-and-actions.md#retrieve-checked-items) |
| `CheckItemsAsync(listItems)` | IEnumerable<TValue> items (optional) | Task | Check specific items or all items | [example](#method-checkitemsasync) |
| `UncheckItemsAsync(listItems)` | IEnumerable<TValue> items (optional) | Task | Uncheck specific items or all items | [example](#method-uncheckitemsasync) |
| `EnableItemAsync(listItem)` | TValue item | Task | Enable a previously disabled item | [example](#method-enableitemasync) |
| `DisableItemAsync(listItem)` | TValue item | Task | Disable a specific item | [example](#method-disableitemasync) |
| `RemoveItems(listItems)` | IEnumerable<TValue> items | void | Remove items from the ListView | [example](#method-removeitems) |
| `UpdateListViewDataSource(updateSortedData, dataSource)` | bool updateSortedData (optional), IEnumerable<TValue> dataSource (optional) | void | Update ListView datasource with optional sorting | [example](#method-updatelistviewdatasource) |

> **Code Example for CheckItemsAsync:**
> ```csharp
> // Check specific items
> var itemsToCheck = new List<DataModel> 
> { 
>     new DataModel { Id = "1", Text = "Item 1" },
>     new DataModel { Id = "2", Text = "Item 2" }
> };
> await ListViewRef.CheckItemsAsync(itemsToCheck);
>
> // Check all items
> await ListViewRef.CheckItemsAsync();
> ```

### Method: UncheckItemsAsync

```csharp
// Uncheck specific items
var itemsToUncheck = new List<DataModel> 
{ 
    new DataModel { Id = "1", Text = "Item 1" }
};
await ListViewRef.UncheckItemsAsync(itemsToUncheck);

// Uncheck all items
await ListViewRef.UncheckItemsAsync();
```

### Method: EnableItemAsync

```csharp
// Enable item that was previously disabled
var itemToEnable = new DataModel { Id = "1", Text = "Item 1" };
await ListViewRef.EnableItemAsync(itemToEnable);
```

### Method: DisableItemAsync

```csharp
// Disable a specific item
var itemToDisable = new DataModel { Id = "2", Text = "Item 2" };
await ListViewRef.DisableItemAsync(itemToDisable);
```

### Method: RemoveItems

```csharp
// Remove specific items from ListView
var itemsToRemove = new List<DataModel>
{
    new DataModel { Id = "1", Text = "Item 1" },
    new DataModel { Id = "3", Text = "Item 3" }
};
ListViewRef.RemoveItems(itemsToRemove);
```

### Method: UpdateListViewDataSource

```csharp
// Update datasource with new data
var newData = new List<DataModel>
{
    new DataModel { Id = "1", Text = "Updated Item 1" },
    new DataModel { Id = "2", Text = "Updated Item 2" },
    new DataModel { Id = "3", Text = "New Item 3" }
};

// Update datasource only
ListViewRef.UpdateListViewDataSource(false, newData);

// Update datasource and refresh sorted data
ListViewRef.UpdateListViewDataSource(true, newData);

// Update only sorting (keep current datasource)
ListViewRef.UpdateListViewDataSource(true);

// Refresh current datasource with sorting
ListViewRef.UpdateListViewDataSource(true, ListViewRef.DataSource);
```

---

## Events

| Event | Arguments | Description | Reference |
|-------|-----------|-------------|-----------|
| `Clicked` | ClickEventArgs<TValue> | Fires when a list item is clicked | [view](events-and-navigation.md#click-event) |
| `Created` | object | Fires when the component is created | [example](#event-created) |
| `Destroyed` | object | Fires when the component is destroyed | [example](#event-destroyed) |
| `OnActionBegin` | ActionEventsArgs | Fires at the beginning of each ListView action | [view](events-and-navigation.md#actionbegin-event---data-binding) |
| `OnActionComplete` | ActionEventsArgs | Fires when a ListView action completes | [view](events-and-navigation.md#actioncomplete-event---data-binding-complete) |
| `OnActionFailure` | ActionFailureEventsArgs | Fires when remote data fetch fails | [example](#event-onactionfailure) |
| `OnBack` | BackEventArgs<TValue> | Fires when back icon is clicked in nested list | [example](#event-onback) |

### Event: Created

```csharp
void OnCreated()
{
    Console.WriteLine("ListView component created and rendered");
    // Initialize any dependent logic
}
```

### Event: Destroyed

```csharp
void OnDestroyed()
{
    Console.WriteLine("ListView component destroyed");
    // Cleanup resources if needed
}
```

### Event: OnActionFailure

```csharp
void OnActionFailure(ActionFailureEventsArgs args)
{
    // Triggered when remote datasource fails to fetch data
    Console.WriteLine($"Error fetching data: {args.Error}");
    // Handle error appropriately - show user message, retry logic, etc.
}
```

### Event: OnBack

```csharp
void OnBack(BackEventArgs<TValue> args)
{
    // Triggered when back button is clicked in nested list navigation
    Console.WriteLine($"Going back from list. Current item: {args.Text}");
}
```

---

## Field Settings

Configure field mappings for your data model using `ListViewFieldSettings<TValue>`:

| Property | Type | Default | Description | Reference |
|----------|------|---------|-------------|-----------|
| `Id` | string | String.Empty | Unique identifier field | [view](data-binding-and-source.md#field-mapping) |
| `Text` | string | String.Empty | Display text field | [view](data-binding-and-source.md#field-mapping) |
| `Tooltip` | string | String.Empty | Tooltip text field | [example](#field-tooltip) |
| `IconCss` | string | String.Empty | Icon CSS class field | [example](#field-iconcss) |
| `Child` | string | String.Empty | Nested items field (for hierarchical data) | [view](advanced-layouts.md) |
| `GroupBy` | string | String.Empty | Field to group items by | [view](grouping-and-organization.md) |
| `IsChecked` | string | String.Empty | Checkbox state field | [view](item-selection-and-actions.md#multiple-selection-modes) |
| `Enabled` | string | String.Empty | Item enabled/disabled state field | [example](#field-enabled) |
| `HtmlAttributes` | string | String.Empty | HTML attributes field | [example](#field-htmlattributes) |

### Field: Tooltip

```csharp
<ListViewFieldSettings TValue="Product"
                      Id="Id"
                      Text="Name"
                      Tooltip="Description">
</ListViewFieldSettings>

// Data model
public class Product
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }  // Maps to Tooltip
}
```

### Field: IconCss

```csharp
<ListViewFieldSettings TValue="MenuItem"
                      Id="Id"
                      Text="Label"
                      IconCss="Icon">
</ListViewFieldSettings>

// Data model with FontAwesome icons
public class MenuItem
{
    public string Id { get; set; }
    public string Label { get; set; }
    public string Icon { get; set; } = "fa-home";  // FontAwesome class
}
```

### Field: Enabled

```csharp
<ListViewFieldSettings TValue="MenuItem"
                      Id="Id"
                      Text="Label"
                      Enabled="IsActive">
</ListViewFieldSettings>

// When IsActive is false, the item appears disabled
public class MenuItem
{
    public string Id { get; set; }
    public string Label { get; set; }
    public bool IsActive { get; set; } = true;
}
```

### Field: HtmlAttributes

```csharp
<ListViewFieldSettings TValue="Product"
                      Id="Id"
                      Text="Name"
                      HtmlAttributes="CssClasses">
</ListViewFieldSettings>

// Apply custom CSS classes to items
public class Product
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string CssClasses { get; set; } = "custom-item highlight";
}
```

---

## Templates

Configure custom rendering using `ListViewTemplates<TValue>`:

| Template | Type | Description | Reference |
|----------|------|-------------|-----------|
| `Template` | RenderFragment<TValue> | Custom template for individual list items | [view](templating-and-rendering.md#custom-item-templates) |
| `HeaderTemplate` | RenderFragment | Custom template for the component header | [view](templating-and-rendering.md#header-template) |
| `GroupTemplate` | RenderFragment<ComposedItemModel<TValue>> | Custom template for group headers | [view](templating-and-rendering.md#group-templates) |

---

## Supporting Enums and Classes

### AnimationSettings

Configuration for animation effects:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Effect` | ListViewEffect | SlideLeft | Animation effect type |
| `Duration` | int | 400 | Animation duration in milliseconds |
| `Easing` | string | "ease" | CSS easing function |

**ListViewEffect Values:**
- `None` - No animation
- `SlideLeft` - Slide left animation
- `SlideDown` - Slide down animation
- `Zoom` - Zoom animation
- `Fade` - Fade animation

> **Code Example:**
> ```csharp
> public AnimationSettings ListAnimation { get; set; } = new AnimationSettings() 
> { 
>     Effect = ListViewEffect.Zoom, 
>     Duration = 500, 
>     Easing = "ease-in-out" 
> };
> ```

### SortOrder Enum

| Value | Description |
|-------|-------------|
| `None` | No sorting |
| `Ascending` | Sort in ascending order (A→Z, 0→9) |
| `Descending` | Sort in descending order (Z→A, 9→0) |

### CheckBoxPosition Enum

| Value | Description |
|-------|-------------|
| `Left` | Checkbox appears before item text (default) |
| `Right` | Checkbox appears after item text |

### SelectedItems<TValue> Class

Return type for `GetCheckedItemsAsync()`:

| Property | Type | Description |
|----------|------|-------------|
| `Data` | List<TValue> | Array of selected item objects |
| `Text` | List<string> | Array of selected item text values |
| `Index` | List<int> | Array of selected item indices (virtualization only) |
| `ParentId` | List<string> | Parent IDs (nested list with checkboxes only) |

---

## Complete Usage Example

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView @ref="ListViewRef"
            DataSource="@Items"
            ShowCheckBox="true"
            ShowHeader="true"
            HeaderTitle="Products"
            CheckBoxPosition="CheckBoxPosition.Left"
            SortOrder="SortOrder.Ascending"
            Height="400px"
            CssClass="custom-list"
            Animation="@ListAnimation">
    
    <ListViewFieldSettings TValue="Product" 
                          Id="Id" 
                          Text="Name"
                          IconCss="Icon"
                          Tooltip="Description">
    </ListViewFieldSettings>
    
    <ListViewTemplates TValue="Product">
        <Template>
            @{
                var item = context as Product;
                <div class="e-list-wrapper e-list-multi-line">
                    <span class="e-list-item-header">@item.Name</span>
                    <span class="e-list-content">@item.Description</span>
                </div>
            }
        </Template>
    </ListViewTemplates>
    
    <ListViewEvents TValue="Product"
                    Clicked="@OnItemClicked"
                    OnActionComplete="@OnActionComplete">
    </ListViewEvents>
</SfListView>

<button @onclick="GetSelectedItems">Get Selected Items</button>

@code {
    SfListView<Product> ListViewRef;
    List<Product> Items = new List<Product>
    {
        new Product { Id = "1", Name = "Laptop", Description = "High-performance laptop", Icon = "e-icons e-mobile" },
        new Product { Id = "2", Name = "Mouse", Description = "Wireless mouse", Icon = "e-icons e-hardware" }
    };

    public AnimationSettings ListAnimation { get; set; } = new AnimationSettings() 
    { 
        Effect = ListViewEffect.Zoom, 
        Duration = 500, 
        Easing = "ease" 
    };

    void OnItemClicked(ClickEventArgs<Product> args)
    {
        Console.WriteLine($"Clicked: {args.Text}");
    }

    void OnActionComplete(ActionEventsArgs args)
    {
        Console.WriteLine("Action completed");
    }

    async Task GetSelectedItems()
    {
        var selected = await ListViewRef.GetCheckedItemsAsync();
        // Process selected.Data, selected.Text, etc.
    }

    public class Product
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Description { get; set; }
        public string Icon { get; set; }
    }
}
```

---

## Additional Property Examples

### Property: Enabled

Disable/enable user interaction with the component:

```cshtml
<SfListView DataSource="@Items"
            Enabled="@IsEnabled">
</SfListView>

@code {
    bool IsEnabled = true;
    
    void ToggleEnabled()
    {
        IsEnabled = !IsEnabled;
        // Component becomes read-only when Enabled=false
    }
}
```

### Property: EnablePersistence

Persist checkbox state across page reloads using browser storage:

```cshtml
<SfListView DataSource="@Items"
            ShowCheckBox="true"
            EnablePersistence="true">
</SfListView>

@* When EnablePersistence is true:
   - Checked items are automatically saved to localStorage
   - State is restored when the page is refreshed
   - User selections persist across sessions *@
```

### Property: EnableRtl

Enable right-to-left text direction for languages like Arabic, Hebrew, Urdu:

```cshtml
<SfListView DataSource="@Items"
            EnableRtl="@IsRtlEnabled"
            ShowHeader="true"
            HeaderTitle="قائمة">
</SfListView>

@code {
    bool IsRtlEnabled = false;
    
    void SetRtl()
    {
        IsRtlEnabled = true;
        // Text, icons, and layout flow from right to left
    }
}
```

### Property: HtmlAttributes

Add custom HTML attributes to the component root element:

```cshtml
<SfListView DataSource="@Items"
            HtmlAttributes="@CustomAttributes">
</SfListView>

@code {
    Dictionary<string, object> CustomAttributes = new Dictionary<string, object>
    {
        { "data-testid", "product-list" },
        { "aria-label", "List of Products" },
        { "role", "listbox" }
    };
}
```

### Property: ID

Set the HTML element ID for the component:

```cshtml
<SfListView ID="productList"
            DataSource="@Items">
</SfListView>

@* The rendered element: <div id="productList" ...>...</div>
   Useful for CSS selectors and JavaScript interop *@

@code {
    void SelectListViaJavaScript()
    {
        // Can reference: document.getElementById("productList")
    }
}
```

### Property: ShowIcon

Display icons from data model in list items:

```cshtml
<SfListView DataSource="@Items"
            ShowIcon="true">
    <ListViewFieldSettings TValue="MenuItem"
                          Id="Id"
                          Text="Name"
                          IconCss="Icon">
    </ListViewFieldSettings>
</SfListView>

@code {
    List<MenuItem> Items = new List<MenuItem>
    {
        new MenuItem { Id = "1", Name = "Users", Icon = "e-icons e-people" },
        new MenuItem { Id = "2", Name = "Products", Icon = "e-icons e-shopping-cart" }
    };
    
    public class MenuItem
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Icon { get; set; }
    }
}
```

### Property: Width

Set the component width using CSS length values:

```cshtml
<SfListView DataSource="@Items"
            Height="400px"
            Width="100%">
</SfListView>

@* Supported values: pixels (300px), percentage (100%), em, rem, etc.
   Common patterns:
   - Width="100%" - Full width of container
   - Width="500px" - Fixed width
   - Width="auto" - Auto-size based on content *@
```

---

## Event Arguments Reference

Complete documentation of all properties available in event argument classes:

### ClickEventArgs<T>

Properties available when handling click events:

| Property | Type | Description |
|----------|------|-------------|
| `Cancel` | bool | Gets or sets a value indicating whether to prevent the current click action on the list item |
| `Index` | int | Gets the index position of the clicked element |
| `IsChecked` | bool | Gets or sets a value that indicates whether the element is checked or not |
| `IsInteracted` | bool | Gets or sets whether the event is triggered by user interaction |
| `ItemData` | T | Gets or sets the data source of the clicked item |
| `Level` | int | Gets or sets the nesting level of the list items |
| `Text` | string | Gets or sets the selected item text |

**Example using ClickEventArgs properties:**

```csharp
void OnItemClicked(ClickEventArgs<Product> args)
{
    Console.WriteLine($"Item: {args.Text}");
    Console.WriteLine($"Index: {args.Index}");
    Console.WriteLine($"Checked: {args.IsChecked}");
    Console.WriteLine($"Level: {args.Level}");
    Console.WriteLine($"User Interaction: {args.IsInteracted}");
    
    // Access full item data
    var product = args.ItemData;
    Console.WriteLine($"Product ID: {product.Id}");
    
    // Cancel the click action if needed
    if (product.Id == "restricted")
    {
        args.Cancel = true;
    }
}
```

### ActionEventsArgs

Properties available in action begin/complete events:

| Property | Type | Description |
|----------|------|-------------|
| `Count` | double | Gets or sets the total number of records |
| `Name` | string | Gets whether the event name is 'ActionBegin' or 'ActionComplete' |

**Example:**

```csharp
void OnActionBegin(ActionEventsArgs args)
{
    if (args.Name == "ActionBegin")
    {
        Console.WriteLine("Data binding started");
    }
}

void OnActionComplete(ActionEventsArgs args)
{
    Console.WriteLine($"Total records: {args.Count}");
    Console.WriteLine("Data binding completed");
}
```

### ActionFailureEventsArgs

Properties available when data action fails (inherits from ActionEventsArgs):

| Property | Type | Description |
|----------|------|-------------|
| `Error` | Exception | Gets or sets the exception error associated with the event |
| `Count` | double | Gets or sets the total number of records (inherited) |
| `Name` | string | Gets the event name (inherited) |

**Example:**

```csharp
void OnActionFailure(ActionFailureEventsArgs args)
{
    Console.WriteLine($"Error: {args.Error.Message}");
    Console.WriteLine($"Error Type: {args.Error.GetType().Name}");
    
    // Log and handle specific error types
    if (args.Error is HttpRequestException)
    {
        Console.WriteLine("Network error occurred");
    }
    else
    {
        Console.WriteLine("Server error occurred");
    }
}
```

### BackEventArgs<T>

Properties available when back button is clicked in nested lists:

| Property | Type | Description |
|----------|------|-------------|
| `IsInteracted` | bool | Gets or sets whether the event is triggered by user interaction |
| `Level` | int | Gets or sets the nesting level of the list items |

**Example:**

```csharp
void OnBack(BackEventArgs<Product> args)
{
    Console.WriteLine($"Nesting Level: {args.Level}");
    Console.WriteLine($"User Interaction: {args.IsInteracted}");
    
    // Handle navigation based on nesting level
    if (args.Level == 0)
    {
        Console.WriteLine("Going back to root level");
    }
}
```

---

## Navigation Guide

- **Getting Started** → [view](getting-started.md)
- **Data Binding** → [view](data-binding-and-source.md)
- **Templates** → [view](templating-and-rendering.md)
- **Selection & Checkboxes** → [view](item-selection-and-actions.md)
- **Filtering & Search** → [view](filtering-and-searching.md)
- **Grouping** → [view](grouping-and-organization.md)
- **Events** → [view](events-and-navigation.md)
- **Advanced Layouts** → [view](advanced-layouts.md)
- **Styling** → [view](styling-and-customization.md)
- **Virtualization** → [view](virtualization-and-performance.md)
- **Advanced Patterns** → [view](advanced-patterns.md)
