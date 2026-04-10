# Columns and Data Display

## Table of Contents
- [Column Configuration](#column-configuration)
- [Column Properties](#column-properties)
- [Text Alignment and Formatting](#text-alignment-and-formatting)
- [Column Templates](#column-templates)
- [Header Templates](#header-templates)
- [Grid Settings](#grid-settings)
- [Text Wrapping](#text-wrapping)
- [Examples](#examples)

---

## Column Configuration

### Basic Column Definition

Define columns using `<MultiColumnComboboxColumn>` elements inside `<MultiColumnComboboxColumns>`:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" DataSource="@Products">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product Name"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Category" Header="Category"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Key Points:**
- `Field` must match property name in data model (case-sensitive)
- `Header` displays as column title
- Order in markup determines column order
- Each column shows in the popup grid

### Multiple Columns

```razor
<SfMultiColumnComboBox TValue="string" TItem="OrderData" DataSource="@Orders">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order" Width="100px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer" Width="180px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="OrderDate" Header="Date" Width="120px" Format="MM/dd/yyyy"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="TotalAmount" Header="Total" Width="110px" Format="C2"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Status" Header="Status" Width="100px"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Column Properties

### Width Management

```razor
<!-- Fixed width in pixels -->
<MultiColumnComboboxColumn Field="Name" Header="Name" Width="200px"></MultiColumnComboboxColumn>

<!-- Percentage width -->
<MultiColumnComboboxColumn Field="Email" Header="Email" Width="30%"></MultiColumnComboboxColumn>

<!-- Auto width (fills remaining space) -->
<MultiColumnComboboxColumn Field="Description" Header="Desc" Width="auto"></MultiColumnComboboxColumn>
```

### Display Formatting

```razor
<!-- Currency format -->
<MultiColumnComboboxColumn Field="Price" Header="Price" Format="C2"></MultiColumnComboboxColumn>

<!-- Number format with decimals -->
<MultiColumnComboboxColumn Field="Quantity" Header="Qty" Format="N0"></MultiColumnComboboxColumn>

<!-- Date format -->
<MultiColumnComboboxColumn Field="OrderDate" Header="Date" Format="MM/dd/yyyy"></MultiColumnComboboxColumn>

<!-- Percentage -->
<MultiColumnComboboxColumn Field="Discount" Header="Discount" Format="P1"></MultiColumnComboboxColumn>
```

**Common Format Strings:**
- `C2` - Currency with 2 decimals
- `N0` - Number with no decimals
- `N2` - Number with 2 decimals
- `P1` - Percentage with 1 decimal
- `MM/dd/yyyy` - Date format
- `HH:mm:ss` - Time format

### Column Customization

```razor
<!-- Custom attributes for column -->
<MultiColumnComboboxColumn Field="ProductID" Header="ID" Width="80px"></MultiColumnComboboxColumn>

<!-- Display boolean as checkbox -->
<MultiColumnComboboxColumn Field="IsActive" Header="Active" DisplayAsCheckBox="true"></MultiColumnComboboxColumn>
```

---

## Text Alignment and Formatting

### Alignment Options

```razor
<!-- Left-aligned (default) -->
<MultiColumnComboboxColumn Field="Name" Header="Name" TextAlign="TextAlign.Left"></MultiColumnComboboxColumn>

<!-- Right-aligned (good for numbers) -->
<MultiColumnComboboxColumn Field="Price" Header="Price" TextAlign="TextAlign.Right"></MultiColumnComboboxColumn>

<!-- Center-aligned -->
<MultiColumnComboboxColumn Field="Status" Header="Status" TextAlign="TextAlign.Center"></MultiColumnComboboxColumn>
```

### Best Practices

```razor
<!-- Text columns: Left -->
<MultiColumnComboboxColumn Field="ProductName" Header="Product" TextAlign="TextAlign.Left"></MultiColumnComboboxColumn>

<!-- Number columns: Right -->
<MultiColumnComboboxColumn Field="Quantity" Header="Qty" TextAlign="TextAlign.Right" Format="N0"></MultiColumnComboboxColumn>

<!-- Date columns: Center -->
<MultiColumnComboboxColumn Field="OrderDate" Header="Date" TextAlign="TextAlign.Center" Format="MM/dd/yyyy"></MultiColumnComboboxColumn>

<!-- Status/Boolean: Center -->
<MultiColumnComboboxColumn Field="IsActive" Header="Active" TextAlign="TextAlign.Center" DisplayAsCheckBox="true"></MultiColumnComboboxColumn>
```

---

## Column Templates

### Item Template

Customize how column cells display:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       TextField="ProductName"
                       ValueField="ProductID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product">
            <Template>
                <div class="product-item">
                    <strong>@((context as ProductData).ProductName)</strong>
                </div>
            </Template>
        </MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Price" Header="Price" Format="C2"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Footer Template

Display custom footer in dropdown:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       @bind-Value="@SelectedOrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
    <FooterTemplate>
        <div style="padding: 10px;">
            Total Orders: @context.Count
        </div>
    </FooterTemplate>
</SfMultiColumnComboBox>

@code {
    private int SelectedOrderID { get; set; }
}
```

### Group Template

When grouping is enabled, customize group headers:

```razor
<SfMultiColumnComboBox TValue="string" TItem="ProductData" 
                       DataSource="@Products"
                       GroupByField="Category">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Category" Header="Category"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
    <GroupTemplate>
        <div class="group-header">
            <strong>📦 Category: @context</strong>
        </div>
    </GroupTemplate>
</SfMultiColumnComboBox>
```

---

## Header Templates

### Custom Column Header

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" DataSource="@Products">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product">
            <HeaderTemplate>
                <div class="header-template">
                    <strong>📦 Product Name</strong>
                </div>
            </HeaderTemplate>
        </MultiColumnComboboxColumn>
        
        <MultiColumnComboboxColumn Field="Price" Header="Price">
            <HeaderTemplate>
                <div class="header-template">
                    <strong>💰 Price</strong>
                </div>
            </HeaderTemplate>
        </MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Grid Settings

### Row Height
MultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       RowHeight="40">
    <MultiColumnComboboxColumns>

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       RowHeight="40">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Alternating Rows

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       EnableAltRow="true">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Grid Lines

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       GridLines="GridLine.Both">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnridLine.None` - No grid lines
- `GridLine.Horizontal` - Only horizontal
- `GridLine.Vertical` - Only vertical
- `GridLine.Both` - Both horizontal and vertical

---

## Text Wrapping

### Enable Text Wrapping

Text wrapping is controlled at the component level using `EnableTextWrap` property:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       EnableTextWrap="true"
                       TextWrapElement="TextWrapElement.Both">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="Description" Header="Description" Width="250px"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**When to use:**
- Long text content
- Product descriptions
- Comments or notes
- Flexible height columns

---

## Examples

### Complete Multi-Column Example

```razor
@page "/employee-selector"
@using Syncfusion.Blazor.MultiColumnComboBox

<h3>Employee Selection</h3>

<SfMultiColumnComboBox TValue="int" TItem="EmployeeData" 
                       DataSource="@Employees"
                       TextField="FirstName"
                       ValueField="EmployeeID"
                       Placeholder="Select an employee"
                       EnableAltRow="true"
                       GridLines="GridLine.Both">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="EmployeeID" Header="ID" Width="80px" TextAlign="TextAlign.Center"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="FirstName" Header="First Name" Width="120px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="LastName" Header="Last Name" Width="120px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="DepartultiColumnComboBox>

@code {
    private List<EmployeeData> Employees = new()rivate ComboBoxFieldSettings ComboFields = new ComboBoxFieldSettings
    {
        Text = "FirstName",
        Value = "EmployeeID"
    };

    protected override void OnInitialized()
    {
        Employees = new()
        {
            new EmployeeData { EmployeeID = 1, FirstName = "John", LastName = "Doe", Department = "Sales", Salary = 50000m, HireDate = new DateTime(2020, 1, 15) },
            new EmployeeData { EmployeeID = 2, FirstName = "Jane", LastName = "Smith", Department = "IT", Salary = 60000m, HireDate = new DateTime(2019, 6, 20) },
            new EmployeeData { EmployeeID = 3, FirstName = "Bob", LastName = "Johnson", Department = "HR", Salary = 55000m, HireDate = new DateTime(2021, 3, 10) }
        };
    }

    public class EmployeeData
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Department { get; set; }
        public decimal Salary { get; set; }
        public DateTime HireDate { get; set; }
    }
}
```

---

## Next Steps

- **Format numbers and dates:** Use Format strings as shown above
- **Handle selection:** See [references/selection-and-values.md](selection-and-values.md)
- **Style columns:** See [references/styling-and-appearance.md](styling-and-appearance.md)
