# Selection and Value Binding

## Table of Contents
- [Basic Value Binding](#basic-value-binding)
- [Two-Way Binding](#two-way-binding)
- [Selection Change Event](#selection-change-event)
- [Complex Object Binding](#complex-object-binding)
- [Tuple Binding](#tuple-binding)
- [Custom Values](#custom-values)
- [Enum Binding](#enum-binding)
- [Examples](#examples)

---

## Basic Value Binding

### One-Way Binding (Read-Only)

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Value="@SelectedProductID"
                       TextField="ProductName"
                       ValueField="ProductID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private int SelectedProductID = 1; // Initial value
    private List<ProductData> Products = new();
}
```

**Use case:** Display selected value but prevent user changes

---

## Two-Way Binding

### @bind-Value Syntax

Automatically sync selected value with property:

```razor
<p>Selected Product ID: @SelectedProductID</p>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       @bind-Value="@SelectedProductID"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<button @onclick="ShowSelected">Show Selected Value</button>

@code {
    private int SelectedProductID { get; set; }
    private List<ProductData> Products = new();

    private void ShowSelected()
    {
        Console.WriteLine($"Selected: {SelectedProductID}");
    }

    protected override void OnInitialized()
    {
        Products = new()
        {
            new ProductData { ProductID = 1, ProductName = "Laptop" },
            new ProductData { ProductID = 2, ProductName = "Mouse" }
        };
    }

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
    }
}
```

**Benefits:**
- ✓ Value updates in real-time
- ✓ Component reflects model state
- ✓ Easy to sync with forms

### Programmatic Updates

```csharp
private int SelectedProductID { get; set; }

private async Task SelectFirstProduct()
{
    SelectedProductID = Products.First().ProductID;
    await Task.CompletedTask;
}

private void ClearSelection()
{
    SelectedProductID = 0;
}
```

---

## Selection Change Event

### ValueChange Event

Fires when user selects an item:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       ValueChange="@OnValueChange"
                       TextField="CustomerName"
                       ValueField="OrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<div>@ResultMessage</div>

@code {
    private List<OrderData> Orders = new();
    private string ResultMessage = "";

    private async Task OnValueChange(ValueChangeEventArgs<int, OrderData> args)
    {
        // args.Value = selected value (OrderID)
        // args.ItemData = full selected object
        // args.IsInteracted = user action or programmatic
        
        if (args.ItemData != null)
        {
            ResultMessage = $"Selected: Order #{args.ItemData.OrderID} - {args.ItemData.CustomerName}";
        }
        
        await Task.CompletedTask;
    }
}
```
MultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       ValueChanged="@OnValueChanged">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnomboBox TValue="int" TItem="ProductData" 
            DataSource="@Products"
            ValueChanged="@OnValueChanged">
    <!-- Columns -->
</SfComboBox>

@code {
    private async Task OnValueChanged(int value)
    {
        // Fire after selection complete
        Console.WriteLine($"New value: {value}");
        await Task.CompletedTask;
    }
}
```

---

## Complex Object Binding

### Bind Full Object (Not Just ID)
MultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       @bind-Value="@SelectedOrderID"
                       ValueChange="@OnOrderSelected">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="TotalAmount" Header="Amount"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<div>
    @if (SelectedOrder != null)
    {
        <p>Selected Order: @SelectedOrder.OrderID - @SelectedOrder.CustomerName ($@SelectedOrder.TotalAmount)</p>
    }
</div>

@code {
    private List<OrderData> Orders = new();
    private int SelectedOrderID { get; set; }
    private OrderData SelectedOrder { get; set; }

    private void OnOrderSelected(ValueChangeEventArgs<int, OrderData> args)
    {
        SelectedOrder = args.ItemData;
   
    private List<OrderData> Orders = new();
    private OrderData SelectedOrder { get; set; }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public decimal TotalAmount { get; set; }
    }
}
```
Custom Value Types

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductInfo" 
                       DataSource="@Products"
                       TextField="Name"
                       ValueField="Id"
                       @bind-Value="@SelectedProductId">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="Id" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Name" Header="Name"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Price" Header="Price" Format="C2"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private int SelectedProductId { get; set; }
    private List<ProductInfo> Products = new();
    
    public class ProductInfo
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
   ecimal Price)>
        {
   MultiColumnComboBox TValue="string" TItem="TagData" 
                       DataSource="@Tags"
                       AllowCustom="true"
                       @bind-Value="@SelectedTag"
                       TextField="TagName"
                       ValueField="TagName"
                       Placeholder="Select or enter a tag">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="TagName" Header="Tag"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumn
@code {
    private (int, string) SelectedTuple { get; set; }
}
```

---

## Custom Values

### Allow User to Enter Custom Values

```razor
<SfMultiColumnComboBox TValue="string" TItem="TagData" 
                       DataSource="@Tags"
                       AllowCustom="true"
                       @bind-Value="@SelectedTag"
                       TextField="TagName"
                       ValueField="TagName"
                       Placeholder="Select or enter a tag">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="TagName" Header="Tag"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<p>Selected: @SelectedTag</p>

@code {
    private List<TagData> Tags = new();
    private string SelectedTag { get; set; }

    protected override void OnInitialized()
    {
        Tags = new()
        {
            new TagData { TagName = "Urgent" },
            new TagData { TagName = "Important" },
            new TagData { TagName = "Follow-up" }
        };
    }

    public class TagData
    {
        public string TagName { get; set; }
    }
}
```

---

## Enum Binding

### Bind to Enum Values

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       TextField="Status"
                       ValueField="OrderID"
                       @bind-Value="@SelectedOrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Status" Header="Status"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    public enum OrderStatus
    {
        Pending,
        Processing,
        Shipped,
        Delivered,
        Cancelled
    }

    private int SelectedOrderID { get; set; }

    public class OrderData
    {
        public int OrderID { get; set; }
        public OrderStatus Status { get; set; }
    }

    private OrderStatus SelectedStatus { get; set; }
    private List<OrderData> Orders = new();

    protected override void OnInitialized()
    {
        Orders = new()
        {
            new OrderData { OrderID = 1001, Status = OrderStatus.Pending },
            new OrderData { OrderID = 1002, Status = OrderStatus.Shipped }
        };
    }
}
```

---

## Examples

### Complete Selection Example

```razor
@page "/employee-selection"
@using Syncfusion.Blazor.MultiColumnComboBox

<h3>Employee Selection with Details</h3>

<div class="form-group">
    <label>Select an Employee:</label>
    <SfMultiColumnComboBox TValue="int" TItem="EmployeeData" 
                           DataSource="@Employees"
                           TextField="FullName"
                           ValueField="EmployeeID"
                           @bind-Value="@SelectedEmployeeID"
                           ValueChange="@OnEmployeeSelected">
        <MultiColumnComboboxColumns>
            <MultiColumnComboboxColumn Field="EmployeeID" Header="ID" Width="70px"></MultiColumnComboboxColumn>
            <MultiColumnComboboxColumn Field="FullName" Header="Name" Width="150px"></MultiColumnComboboxColumn>
            <MultiColumnComboboxColumn Field="Department" Header="Department" Width="130px"></MultiColumnComboboxColumn>
            <MultiColumnComboboxColumn Field="Salary" Header="Salary" Width="100px" Format="C0" TextAlign="TextAlign.Right"></MultiColumnComboboxColumn>
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
</div>

@if (SelectedEmployee != null)
{
    <div class="alert alert-info mt-3">
        <h5>Selected Employee Details</h5>
        <p><strong>Name:</strong> @SelectedEmployee.FullName</p>
        <p><strong>ID:</strong> @SelectedEmployee.EmployeeID</p>
        <p><strong>Department:</strong> @SelectedEmployee.Department</p>
        <p><strong>Salary:</strong> @SelectedEmployee.Salary.ToString("C")</p>
        <p><strong>Email:</strong> @SelectedEmployee.Email</p>
    </div>
}

@code {
    private List<EmployeeData> Employees = new();
    private int SelectedEmployeeID { get; set; }
    private EmployeeData SelectedEmployee { get; set; }

    private async Task OnEmployeeSelected(ValueChangeEventArgs<int, EmployeeData> args)
    {
        SelectedEmployee = args.ItemData;
        Console.WriteLine($"Employee selected: {SelectedEmployee?.FullName}");
        // Trigger any additional actions (API calls, etc.)
        await Task.CompletedTask;
    }

    protected override void OnInitialized()
    {
        Employees = new()
        {
            new EmployeeData 
            { 
                EmployeeID = 1, 
                FullName = "John Doe", 
                Department = "Engineering", 
                Salary = 75000m,
                Email = "john.doe@company.com"
            },
            new EmployeeData 
            { 
                EmployeeID = 2, 
                FullName = "Jane Smith", 
                Department = "Sales", 
                Salary = 65000m,
                Email = "jane.smith@company.com"
            },
            new EmployeeData 
            { 
                EmployeeID = 3, 
                FullName = "Bob Johnson", 
                Department = "HR", 
                Salary = 60000m,
                Email = "bob.johnson@company.com"
            }
        };
    }

    public class EmployeeData
    {
    Custom Value Handling

```razor
<SfMultiColumnComboBox TValue="string" TItem="CategoryData" 
                       DataSource="@Categories"
                       AllowCustom="true"
                       TextField="CategoryName"
                       ValueField="CategoryName"
        public int EmployeeID { get; set; }
        public string FullName { get; set; }
        public string Department { get; set; }
        public decimal Salary { get; set; }
        public string Email { get; set; }
    }
}
```

### Custom Value Handling

```razor
<SfMultiColumnComboBox TValue="string" TItem="CategoryData" 
                       DataSource="@Categories"
                       AllowCustom="true"
                       TextField="CategoryName"
                       ValueField="CategoryName"
                       @bind-Value="@SelectedCategory">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="CategoryName" Header="Category"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private string SelectedCategory { get; set; }

---

## Next Steps

- **Validate selection:** See [references/features-and-settings.md](features-and-settings.md)
- **Handle form submission:** Integrate with EditForm in [references/features-and-settings.md](features-and-settings.md)
- **Customize display:** See [references/styling-and-appearance.md](styling-and-appearance.md)
