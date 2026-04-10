# Getting Started with MultiColumn ComboBox

## Table of Contents
- [Installation](#installation)
- [Basic Setup](#basic-setup)
- [First ComboBox](#first-combobox)
- [Adding Columns](#adding-columns)
- [Binding Data](#binding-data)
- [Common Issues](#common-issues)

---

## Installation

The MultiColumn ComboBox is part of the **Syncfusion Blazor package**. Install it via NuGet:

```bash
dotnet add package Syncfusion.Blazor.MultiColumnComboBox
```

### Register in App.razor or _Host.cshtml

Add the Syncfusion styles and scripts:

```html
<!-- In Head -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- In Body (before closing tag) -->
<script src="_framework/blazor.web.js"></script>
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

### Add NuGet Package to .csproj

```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.Blazor.MultiColumnComboBox" Version="latest" />
</ItemGroup>
```

---

## Basic Setup

### Step 1: Add Using Statement

```razor
@using Syncfusion.Blazor.MultiColumnComboBox
@using Syncfusion.Blazor.Data
```

### Step 2: Define Your Data Model

```csharp
public class ProductData
{
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
}
```

### Step 3: Prepare Data in OnInitialized

```csharp
@code {
    private List<ProductData> Products = new();

    protected override void OnInitialized()
    {
        Products = new()
        {
            new ProductData { ProductID = 1, ProductName = "Laptop", Category = "Electronics", Price = 999.99m },
            new ProductData { ProductID = 2, ProductName = "Mouse", Category = "Electronics", Price = 29.99m },
            new ProductData { ProductID = 3, ProductName = "Keyboard", Category = "Electronics", Price = 79.99m }
        };
    }
}
```

---

## First MultiColumn ComboBox

### Minimal Example

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**What's happening:**
- `TValue="int"` - Selected value type (ProductID)
- `TItem="ProductData"` - Data model type
- `DataSource="@Products"` - Binds list of products
- `TextField` - Field to display in input
- `ValueField` - Field to use as value
- `Placeholder` - Helper text before selection
- `MultiColumnComboboxColumn` - Defines visible columns

### Multi-Column Display

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       ValueField="ProductID"
                       TextField="ProductName"
                       Placeholder="Pick a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product Name" Width="200px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Category" Header="Category" Width="150px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Price" Header="Price" Width="100px" Format="C2"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Adding Columns

### Column Syntax

```razor
<MultiColumnComboboxColumn Field="PropertyName" 
                           Header="Display Header" 
                           Width="150px"
                           Format="C2"
                           TextAlign="TextAlign.Right">
</MultiColumnComboboxColumn>
```

| Property | Purpose |
|----------|---------|
| `Field` | Binds to data property |
| `Header` | Column header text |
| `Width` | Column width (px, %, auto) |
| `Format` | Format string (C2=currency, N2=number) |
| `TextAlign` | Text alignment (Left, Right, Center) |

### Multi-Column Example

```razor
<SfMultiColumnComboBox TValue="string" TItem="EmployeeData" 
                       DataSource="@Employees"
                       TextField="Name"
                       ValueField="EmployeeID"
                       Placeholder="Select employee">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="EmployeeID" Header="ID" Width="80px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Name" Header="Name" Width="150px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Department" Header="Dept" Width="120px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Salary" Header="Salary" Width="110px" Format="C0"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Binding Data

### Local Data Binding

Direct bind with C# List:

```csharp
@code {
    private List<ProductData> Products = new();

    protected override void OnInitialized()
    {
        Products = new()
        {
            new ProductData { ProductID = 1, ProductName = "Item A", Category = "Category 1" },
            new ProductData { ProductID = 2, ProductName = "Item B", Category = "Category 2" }
        };
    }
}
```

### Observable Collection Binding

Auto-updates when collection changes:

```csharp
@using System.Collections.ObjectModel

@code {
    private ObservableCollection<ProductData> Products = new();

    protected override void OnInitialized()
    {
        Products = new ObservableCollection<ProductData>
        {
            new ProductData { ProductID = 1, ProductName = "Item A", Category = "Cat 1" },
            new ProductData { ProductID = 2, ProductName = "Item B", Category = "Cat 2" }
        };
    }

    private void AddProduct()
    {
        Products.Add(new ProductData { ProductID = 3, ProductName = "Item C", Category = "Cat 3" });
    }
}
Map data fields using TextField and ValueField properties:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       TextField="ProductName"
                       ValueField="ProductID">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    // TextField = "ProductName" - Display text in input
    // ValueField = "ProductID" - Value to bind
}
```

---

## Common Issues

### Issue: MultiColumnComboboxColumn elements don't show up.

**Solution:** Ensure you're using the correct namespace and component hierarchy:

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox ...>
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="..." Header="..."></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Issue: No Data Showing

**Problem:** DataSource is empty or null.

**Checklist:**
- ✓ Data initialized in `OnInitialized()` or property setter
- ✓ List is not null
- ✓ `Field` property names match your data model exactly (case-sensitive)
- ✓ `TextField` and `ValueField` are set correctly
- ✓ Check browser console for errors
**Example:**
```csharp
protected override void OnInitialized()
{
    // Populate data here, not in component parameters
    Products = LoadProducts();
}
```

<!-- Verify with debug output -->
@if (Products == null)
{
    <p>Products list is null</p>
}
else if (Products.Count == 0)
{
    <p>Products list is empty</p>
}
else
{
    <SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                           DataSource="@Products">
        <MultiColumnComboboxColumns>
            <!-- Columns -->
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
}
```

### Issue: Selected Value Not Binding

**Problem:** `@bind-Value` doesn't update selected item.

**Solution:** Use proper two-way binding:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       @bind-Value="@SelectedProductID">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private int SelectedProductID { get; set; }
}
```

### Issue: Styling Issues

**Problem:** Component looks unstyled or broken layout.

**Solution:** Verify theme CSS is loaded in `_Host.cshtml` or `App.razor`.

---

## Next Steps

- **Filter data:** See [references/filtering-and-search.md](filtering-and-search.md)
- **Customize appearance:** See [references/styling-and-appearance.md](styling-and-appearance.md)
- **Handle selection:** See [references/selection-and-values.md](selection-and-values.md)
