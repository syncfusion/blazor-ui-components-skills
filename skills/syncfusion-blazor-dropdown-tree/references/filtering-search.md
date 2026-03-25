# Filtering and Search

## Table of Contents
- [Enabling Filtering](#enabling-filtering)
- [Filter Types](#filter-types)
- [Local Data Filtering](#local-data-filtering)
- [Remote Data Filtering](#remote-data-filtering)
- [Case-Sensitive Filtering](#case-sensitive-filtering)
- [Filter Placeholder](#filter-placeholder)
- [Minimum Filter Length](#minimum-filter-length)

## Enabling Filtering

Enable search functionality by setting `AllowFiltering` to `true`:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
        new EmployeeData() { Id = "10", PId = "3", Name = "Lilly", Job = "Developer" },
        new EmployeeData() { Id = "5", PId = "1", Name = "Nancy Davolio", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Michael Suyama", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King", Job = "Developer" },
        new EmployeeData() { Id = "11", PId = "6", Name = "Mary", Job = "Developer" },
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR"}
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Job { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## Filter Types

### Available Filter Types

The `FilterType` property controls how filtering matches search text:

| FilterType | Description |
|-----------|-------------|
| `StartsWith` | Match nodes where text begins with search value (default) |
| `EndsWith` | Match nodes where text ends with search value |
| `Contains` | Match nodes where text contains search value anywhere |

### Using StartsWith (Default)

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    FilterType="FilterType.StartsWith">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    // Data as above
}
```

### Using EndsWith

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    FilterType="FilterType.EndsWith">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    // Data as above
}
```

### Using Contains

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    FilterType="FilterType.Contains">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    // Data as above
}
```

## Local Data Filtering

Filter locally bound data automatically:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth" },
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## Remote Data Filtering

Filter data from remote OData services:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfDropDownTree TValue="int?" TItem="TreeData" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true">
    <DropDownTreeField TItem="TreeData" 
        Query="@Query" 
        ID="EmployeeID" 
        Text="FirstName" 
        HasChildren="EmployeeID">
        <SfDataManager Url="http://services.odata.org/V4/Northwind/Northwind.svc" 
            Adaptor="@Syncfusion.Blazor.Adaptors.ODataV4Adaptor" 
            CrossDomain="true">
        </SfDataManager>
    </DropDownTreeField>
    <DropDownTreeField TItem="TreeData" 
        Query="@SubQuery" 
        ID="OrderID" 
        Text="ShipName" 
        ParentID="EmployeeID">
        <SfDataManager Url="http://services.odata.org/V4/Northwind/Northwind.svc" 
            Adaptor="@Syncfusion.Blazor.Adaptors.ODataV4Adaptor" 
            CrossDomain="true">
        </SfDataManager>
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public Query Query = new Query().From("Employees").Select(new List<string> { "EmployeeID", "FirstName" }).Take(5).RequiresCount();
    public Query SubQuery = new Query().From("Orders").Select(new List<string> { "OrderID", "EmployeeID", "ShipName" }).Take(5).RequiresCount();
    
    public class TreeData
    {
        public int? EmployeeID { get; set; }
        public int OrderID { get; set; }
        public string? ShipName { get; set; }
        public string? FirstName { get; set; }
    }
}
```

## Case-Sensitive Filtering

Control filtering behavior with the `IgnoreCase` property:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    IgnoreCase="false">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", Name = "steven buchanan", HasChild = true },
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

**Note:** By default, `IgnoreCase` is `true` (case-insensitive filtering)

## Filter Placeholder

Customize the filter input placeholder text:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    FilterBarPlaceholder="Search by name...">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    // Data as above
}
```

## Minimum Filter Length

Implement minimum character requirement before filtering occurs:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    FilterBarPlaceholder="Search (min 2 chars)"
    Filtering="OnFiltering">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth" },
        new EmployeeData() { Id = "5", PId = "1", Name = "Nancy Davolio", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Michael Suyama", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King" },
    };

    void OnFiltering(DdtFilteringEventArgs args)
    {
        // Check filter text length
        if (!string.IsNullOrEmpty(args.Text) && args.Text.Length < 2)
        {
            args.PreventDefaultAction = true;
        }
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## Complete Filtering Example

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<div>
    <div style="margin-bottom: 20px">
        <label>Filter Type: </label>
        <select @onchange="OnFilterTypeChange">
            <option value="@FilterType.StartsWith">Starts With</option>
            <option value="@FilterType.EndsWith">Ends With</option>
            <option value="@FilterType.Contains">Contains</option>
        </select>
    </div>
    
    <div style="margin-bottom: 20px">
        <label>
            <input type="checkbox" @onchange="OnIgnoreCaseChange" />
            Ignore Case
        </label>
    </div>
</div>

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true"
    FilterType="@CurrentFilterType"
    IgnoreCase="@CurrentIgnoreCase"
    FilterBarPlaceholder="Search employees..."
    Filtering="OnFiltering">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    FilterType CurrentFilterType = FilterType.StartsWith;
    bool CurrentIgnoreCase = true;

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", HasChild = true },
    };

    void OnFilterTypeChange(ChangeEventArgs e)
    {
        CurrentFilterType = (FilterType)Enum.Parse(typeof(FilterType), e.Value.ToString());
    }

    void OnIgnoreCaseChange(ChangeEventArgs e)
    {
        CurrentIgnoreCase = (bool)e.Value;
    }

    void OnFiltering(DdtFilteringEventArgs args)
    {
        Console.WriteLine($"Filtering text: {args.Text}");
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## Filtering Properties Summary

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowFiltering` | bool | false | Enable/disable search filtering |
| `FilterType` | FilterType | StartsWith | Filter matching strategy |
| `IgnoreCase` | bool | true | Case-insensitive filtering |
| `FilterBarPlaceholder` | string | null | Filter input placeholder text |
| `Filtering` | EventCallback<DdtFilteringEventArgs> | null | Filter event callback |
