# OLAP Data Source — Syncfusion Blazor Pivot Table

## Table of Contents
- [Overview](#overview)
- [Getting Started with OLAP](#getting-started-with-olap)
- [OLAP Data Binding](#olap-data-binding)
- [OLAP Cube Elements](#olap-cube-elements)
- [Adding Cube Elements to Axes](#adding-cube-elements-to-axes)
- [Measures in Row Axis](#measures-in-row-axis)
- [Named Sets](#named-sets)
- [Calculated Fields](#calculated-fields)
- [Authentication](#authentication)
- [Roles](#roles)
- [Field List with OLAP](#field-list-with-olap)
- [Grouping Bar with OLAP](#grouping-bar-with-olap)
- [Filter Axis](#filter-axis)
- [Virtual Scrolling with OLAP](#virtual-scrolling-with-olap)
- [OLAP Cube Elements Reference](#olap-cube-elements-reference)

---

## Overview

The Syncfusion Blazor Pivot Table supports OLAP (Online Analytical Processing) data sources, enabling users to analyze multidimensional data from OLAP cubes like Microsoft SQL Server Analysis Services (SSAS). OLAP provides powerful capabilities for complex data analysis, including hierarchies, calculated members, named sets, and MDX expressions.

**When to Use OLAP:**
- Connecting to Microsoft SQL Server Analysis Services (SSAS)
- Working with multidimensional cubes
- Analyzing data with complex hierarchies
- Using pre-defined calculated measures and dimensions
- Leveraging named sets and MDX expressions

---

## Getting Started with OLAP

### Basic OLAP Connection

To connect to an OLAP data source, configure `PivotViewDataSourceSettings` with OLAP-specific properties:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" Width="800" Height="350">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll" 
                                 LocaleIdentifier="1033" 
                                 EnableSorting="true">
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public class ProductDetails
    {
        public int Sold { get; set; }
        public double Amount { get; set; }
        public string Country { get; set; }
        public string Products { get; set; }
        public string Year { get; set; }
        public string Quarter { get; set; }
    }
}
```

---

## OLAP Data Binding

### Required Properties

Configure these properties in `PivotViewDataSourceSettings` to bind OLAP data:

| Property | Type | Description |
|----------|------|-------------|
| `ProviderType` | `ProviderType.SSAS` | Indicates OLAP provider type |
| `Url` | string | OLAP service URL (msmdpump.dll endpoint) |
| `Catalog` | string | Database/catalog name containing the cube |
| `Cube` | string | Name of the OLAP cube to use |
| `LocaleIdentifier` | int | Optional: Locale ID for formatting (e.g., 1033 for en-US) |

### Complete OLAP Binding Example

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" Width="800" Height="350">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll" 
                                 LocaleIdentifier="1033" 
                                 EnableSorting="true">
        <PivotViewColumns>
            <PivotViewColumn Name="[Product].[Product Categories]" Caption="Product Category"></PivotViewColumn>
            <PivotViewColumn Name="[Measures]" Caption="Measure"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="[Customer].[Customer Geography]" Caption="Customer Geography"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="[Measures].[Customer Count]" Caption="Customer Count"></PivotViewValue>
            <PivotViewValue Name="[Measures].[Internet Sales Amount]" Caption="Internet Sales Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="[Measures].[Internet Sales Amount]" Format="C0"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
    <PivotViewGridSettings ColumnWidth="160"></PivotViewGridSettings>
</SfPivotView>
```

---

## OLAP Cube Elements

OLAP cubes contain several types of elements that can be used in the Pivot Table:

### Element Types

1. **Measures**: Numeric values for analysis (e.g., Sales Amount, Customer Count)
2. **Dimensions**: Categorical data (e.g., Product, Customer, Date)
3. **Hierarchies**: Parent-child relationships within dimensions
4. **Levels**: Specific stages within hierarchies
5. **Named Sets**: Predefined groups of members
6. **Calculated Members**: Custom calculations defined in the cube

### OLAP Naming Convention

OLAP cube elements use MDX naming syntax:
- `[Dimension].[Hierarchy]` - Hierarchy reference
- `[Dimension].[Hierarchy].[Level]` - Level reference
- `[Measures].[Measure Name]` - Measure reference

**Example:**
```
[Product].[Product Categories]
[Customer].[Customer Geography]
[Measures].[Internet Sales Amount]
```

---

## Adding Cube Elements to Axes

### Specifying Cube Elements

Each OLAP cube element must be specified using:
- **Name**: Unique name from the cube (exact match required)
- **Caption**: Display label (optional, defaults to Name if not specified)

### Four Axes Configuration

```razor
<PivotViewDataSourceSettings TValue="ProductDetails" 
                             ProviderType="ProviderType.SSAS" 
                             Catalog="Adventure Works DW 2008 SE" 
                             Cube="Adventure Works" 
                             Url="https://bi.syncfusion.com/olap/msmdpump.dll">
    
    <!-- Columns: Dimensions/Hierarchies displayed as columns -->
    <PivotViewColumns>
        <PivotViewColumn Name="[Product].[Product Categories]" Caption="Product Category"></PivotViewColumn>
        <PivotViewColumn Name="[Measures]" Caption="Measure"></PivotViewColumn>
    </PivotViewColumns>
    
    <!-- Rows: Dimensions/Hierarchies displayed as rows -->
    <PivotViewRows>
        <PivotViewRow Name="[Customer].[Customer Geography]" Caption="Customer Geography"></PivotViewRow>
    </PivotViewRows>
    
    <!-- Values: Measures for aggregation -->
    <PivotViewValues>
        <PivotViewValue Name="[Measures].[Customer Count]" Caption="Customer Count"></PivotViewValue>
        <PivotViewValue Name="[Measures].[Internet Sales Amount]" Caption="Internet Sales Amount"></PivotViewValue>
    </PivotViewValues>
    
    <!-- Filters: Master filters for the entire table -->
    <PivotViewFilters>
        <PivotViewFilter Name="[Date].[Fiscal]" Caption="Date Fiscal"></PivotViewFilter>
    </PivotViewFilters>
</PivotViewDataSourceSettings>
```

> **Critical:** If the `Name` does not exactly match the cube element name, the Pivot Table will be empty.

---

## Measures in Row Axis

By default, measures appear in columns. To display measures in rows:

```razor
<PivotViewRows>
    <PivotViewRow Name="[Customer].[Customer Geography]" Caption="Customer Geography"></PivotViewRow>
    <PivotViewRow Name="[Measures]" Caption="Measures"></PivotViewRow>
</PivotViewRows>
```

Users can also drag the "Measures" button between axes using the grouping bar or field list at runtime.

---

## Named Sets

Named sets are predefined MDX expressions that return a group of dimension members.

### Using Named Sets

Set `IsNamedSet="true"` when adding a named set:

```razor
<PivotViewColumns>
    <PivotViewColumn Name="[Core Product Group]" 
                     Caption="Core Product Group" 
                     IsNamedSet="true"></PivotViewColumn>
    <PivotViewColumn Name="[Measures]" Caption="Measures"></PivotViewColumn>
</PivotViewColumns>
```

Named sets can only be added to row or column axes, not to values or filters.

---

## Calculated Fields

Create custom calculations based on existing cube elements.

### Types of Calculated Fields

1. **Calculated Measure**: New measure from an expression
2. **Calculated Dimension**: New dimension from an expression

### Defining Calculated Fields in Code

```razor
<PivotViewDataSourceSettings TValue="ProductDetails" ProviderType="ProviderType.SSAS" 
                             Catalog="Adventure Works DW 2008 SE" 
                             Cube="Adventure Works" 
                             Url="https://bi.syncfusion.com/olap/msmdpump.dll">
    <PivotViewColumns>
        <PivotViewColumn Name="[Product].[Product Categories]" Caption="Product Categories"></PivotViewColumn>
        <PivotViewColumn Name="[Measures]" Caption="Measures"></PivotViewColumn>
    </PivotViewColumns>
    <PivotViewRows>
        <PivotViewRow Name="[Customer].[Customer Geography]" Caption="Customer Geography"></PivotViewRow>
    </PivotViewRows>
    <PivotViewValues>
        <PivotViewValue Name="[Measures].[Customer Count]" Caption="Customer Count"></PivotViewValue>
        <PivotViewValue Name="Order on Discount" IsCalculatedField="true"></PivotViewValue>
    </PivotViewValues>
    
    <PivotViewCalculatedFieldSettings>
        <!-- Calculated Dimension -->
        <PivotViewCalculatedFieldSetting 
            Name="BikeAndComponents" 
            Formula="([Product].[Product Categories].[Category].[Bikes] + [Product].[Product Categories].[Category].[Components])" 
            HierarchyUniqueName="[Product].[Product Categories]" 
            FormatString="Standard">
        </PivotViewCalculatedFieldSetting>
        
        <!-- Calculated Measure -->
        <PivotViewCalculatedFieldSetting 
            Name="Order on Discount" 
            Formula="[Measures].[Order Quantity] + ([Measures].[Order Quantity] * 0.10)" 
            FormatString="Currency">
        </PivotViewCalculatedFieldSetting>
    </PivotViewCalculatedFieldSettings>
</PivotViewDataSourceSettings>
```

### Calculated Field Properties

| Property | Description |
|----------|-------------|
| `Name` | Unique name for the calculated field |
| `Formula` | MDX expression for the calculation |
| `HierarchyUniqueName` | Parent hierarchy (for calculated dimensions only) |
| `FormatString` | Display format: Standard, Currency, Percent, or custom |
| `IsCalculatedField` | Set to `true` when adding to an axis |

### Enable Calculated Field UI

Allow users to create calculated fields at runtime:

```razor
<SfPivotView TValue="ProductDetails" 
             ShowFieldList="true" 
             AllowCalculatedField="true">
```

This adds a "CALCULATED FIELD" button in the field list UI.

### Format String Options

- **Standard**: Display as numbers (e.g., 9584.3)
- **Currency**: Display with currency symbol (e.g., $9,584.30)
- **Percent**: Display as percentage (e.g., 95.84%)
- **Custom**: Define custom format (e.g., "###0.##0#" → 9584.300)

### Supported MDX Operators and Functions

Calculated fields support standard MDX operators and functions. See Microsoft documentation:
- [MDX Operators](https://learn.microsoft.com/en-us/sql/mdx/operators-mdx-syntax?view=sql-server-ver15)
- [MDX Functions](https://learn.microsoft.com/en-us/sql/mdx/functions-mdx-syntax?view=sql-server-ver15)

**Example Formula:**
```
IIF([Measures].[Internet Sales Amount]^0.5 > 100, 
    [Measures].[Internet Sales Amount]*100, 
    [Measures].[Internet Sales Amount]/100)
```

> **Note:** Calculated measures can only be added to the value axis.

---

## Authentication

Provide credentials for secure OLAP connections:

```razor
<PivotViewDataSourceSettings TValue="ProductDetails" 
                             ProviderType="ProviderType.SSAS" 
                             Catalog="Adventure Works DW 2008 SE" 
                             Cube="Adventure Works" 
                             Url="https://bi.syncfusion.com/olap/msmdpump.dll">
    
    <PivotViewAuthentication UserName="YourUsername" Password="YourPassword"></PivotViewAuthentication>
    
    <!-- Other settings... -->
</PivotViewDataSourceSettings>
```

If authentication is not provided in code, the browser displays a default login popup.

---

## Roles

Assign SSAS roles to control data access:

```razor
<PivotViewDataSourceSettings TValue="ExpandoObject" 
                             ProviderType="ProviderType.SSAS" 
                             Catalog="Adventure Works DW 2008 SE" 
                             Cube="Adventure Works" 
                             Url="https://bi.syncfusion.com/olap/msmdpump.dll" 
                             Roles="Role1,Role2">
    
    <PivotViewAuthentication UserName="YourUsername" Password="YourPassword"></PivotViewAuthentication>
    
    <!-- Other settings... -->
</PivotViewDataSourceSettings>
```

**Multiple Roles:** Specify as comma-separated string: `"Role1,Role2,Role3"`

Roles control which cube data users can access based on SSAS security configuration.

---

## Field List with OLAP

Enable the field list to allow runtime OLAP cube exploration:

```razor
<SfPivotView TValue="ProductDetails" ShowFieldList="true" Width="800" Height="350">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll">
        <!-- Axes configuration... -->
    </PivotViewDataSourceSettings>
</SfPivotView>
```

The field list displays:
- Dimensions with their hierarchies
- Measures
- Named sets
- Calculated members
- Display folders (organizational containers)

Users can drag cube elements between axes and apply filters/sorting.

---

## Grouping Bar with OLAP

Enable the grouping bar for quick axis management:

```razor
<SfPivotView TValue="ProductDetails" ShowGroupingBar="true" Width="800" Height="350">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll">
        <!-- Axes configuration... -->
    </PivotViewDataSourceSettings>
</SfPivotView>
```

Users can drag cube elements between row, column, value, and filter axes directly.

---

## Filter Axis

The filter axis acts as a master filter controlling data displayed across all other axes:

```razor
<PivotViewFilters>
    <PivotViewFilter Name="[Date].[Fiscal]" Caption="Date Fiscal"></PivotViewFilter>
</PivotViewFilters>
```

**Use Filter Axis For:**
- Dimensions that should filter the entire table
- Hierarchies that don't need to be displayed as rows/columns
- Calculated members used for filtering

Users can modify filter selections through the field list or grouping bar at runtime.

---

## Virtual Scrolling with OLAP

Enable virtual scrolling for large OLAP datasets:

```razor
<SfPivotView TValue="ProductDetails" 
             Width="800" 
             Height="350" 
             EnableVirtualization="true" 
             ShowGroupingBar="true">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll">
        <PivotViewColumns>
            <PivotViewColumn Name="[Product].[Product Categories]"></PivotViewColumn>
            <PivotViewColumn Name="[Measures]"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="[Customer].[Customer]" Caption="Customer"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="[Measures].[Customer Count]"></PivotViewValue>
            <PivotViewValue Name="[Measures].[Internet Sales Amount]"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
    <PivotViewGridSettings ColumnWidth="160"></PivotViewGridSettings>
</SfPivotView>
```

### Virtual Scrolling Limitations with OLAP

1. **Column Width:** Must be specified in pixels (not percentage) in `PivotViewGridSettings`
2. **Totals Display:** Subtotals and grand totals appear only when measures are at the end of rows or columns
3. **Column Resizing:** Affects scrolling calculations
4. **Performance:** Large width/height values increase data loaded per page

---

## OLAP Cube Elements Reference

### Field List Node Types

The OLAP field list displays different icons for each element type:

| Icon Type | Element | Description | Draggable? |
|-----------|---------|-------------|------------|
| 📁 Folder | Display Folder | Organizational container | No |
| 📊 Chart | Measure | Numeric value for analysis | No |
| 🧊 Cube | Dimension | Categorical grouping | No |
| 📐 Hierarchy | User-Defined Hierarchy | Multi-level structure | Yes |
| 🌳 Tree | Attribute Hierarchy | Single-level hierarchy | Yes |
| ➊➋➌ Numbers | Level | Stage within hierarchy | Yes |
| 🔢 Set | Named Set | Predefined member group | Yes |

### Hierarchies

**User-Defined Hierarchy:**
- Contains 2+ levels
- Example: Date → Year → Quarter → Month
- Allows drill-down navigation

**Attribute Hierarchy:**
- Contains single level
- Each dimension field creates one
- Example: Country (single level)

### Measures

Measures are numeric values from the cube's fact table. Always displayed under the "Measures" dimension in the field list.

**Adding to Values Axis:**
```razor
<PivotViewValues>
    <PivotViewValue Name="[Measures].[Internet Sales Amount]" Caption="Sales"></PivotViewValue>
</PivotViewValues>
```

**Applying Formatting:**
```razor
<PivotViewFormatSettings>
    <PivotViewFormatSetting Name="[Measures].[Internet Sales Amount]" Format="C0"></PivotViewFormatSetting>
</PivotViewFormatSettings>
```

### Dimensions

Dimensions organize data into categories. Each dimension can contain:
- Multiple hierarchies (user-defined and attribute)
- Levels within hierarchies
- Members and child members
- Named sets
- Calculated members

### Levels

Levels represent stages within a hierarchy:

```
Customer Geography Hierarchy:
  Level 1: Country
  Level 2: State
  Level 3: City
```

Each level can be added independently to axes for specific granularity.

---

## Common Patterns

### Pattern 1: Basic OLAP Connection

```razor
<SfPivotView TValue="ProductDetails">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Your Catalog" 
                                 Cube="Your Cube" 
                                 Url="https://your-server/olap/msmdpump.dll">
    </PivotViewDataSourceSettings>
</SfPivotView>
```

### Pattern 2: OLAP with Field List and Grouping Bar

```razor
<SfPivotView TValue="ProductDetails" 
             ShowFieldList="true" 
             ShowGroupingBar="true">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll">
    </PivotViewDataSourceSettings>
</SfPivotView>
```

### Pattern 3: OLAP with Authentication and Roles

```razor
<PivotViewDataSourceSettings TValue="ExpandoObject" 
                             ProviderType="ProviderType.SSAS" 
                             Roles="AnalystRole,ViewerRole">
    <PivotViewAuthentication UserName="analyst1" Password="SecurePass123"></PivotViewAuthentication>
</PivotViewDataSourceSettings>
```

### Pattern 4: OLAP with Calculated Fields

```razor
<SfPivotView TValue="ProductDetails" 
             ShowFieldList="true" 
             AllowCalculatedField="true">
    <PivotViewDataSourceSettings TValue="ProductDetails" 
                                 ProviderType="ProviderType.SSAS" 
                                 Catalog="Adventure Works DW 2008 SE" 
                                 Cube="Adventure Works" 
                                 Url="https://bi.syncfusion.com/olap/msmdpump.dll">
        <PivotViewCalculatedFieldSettings>
            <PivotViewCalculatedFieldSetting 
                Name="DiscountedAmount" 
                Formula="[Measures].[Sales Amount] * 0.9" 
                FormatString="Currency">
            </PivotViewCalculatedFieldSetting>
        </PivotViewCalculatedFieldSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

---

## Troubleshooting

### Empty Pivot Table
- **Cause:** Incorrect cube element names
- **Solution:** Verify exact names from SSAS using SQL Server Management Studio or field list

### Authentication Popup
- **Cause:** Missing authentication in code
- **Solution:** Add `PivotViewAuthentication` with credentials

### Missing Totals with Virtual Scrolling
- **Cause:** Measures not at end of rows/columns
- **Solution:** Place `[Measures]` as last item in row or column axis

### Calculated Field Not Showing
- **Cause:** `IsCalculatedField` not set or wrong axis
- **Solution:** Set `IsCalculatedField="true"` and ensure calculated measures are in values axis

### Role Access Denied
- **Cause:** SSAS role doesn't grant access to cube
- **Solution:** Verify role permissions in SSAS or use different role

---

## Key Properties Summary

| Property | Location | Purpose |
|----------|----------|---------|
| `ProviderType` | DataSourceSettings | Set to `ProviderType.SSAS` for OLAP |
| `Url` | DataSourceSettings | OLAP service endpoint (msmdpump.dll) |
| `Catalog` | DataSourceSettings | Database/catalog name |
| `Cube` | DataSourceSettings | OLAP cube name |
| `Roles` | DataSourceSettings | SSAS role names (comma-separated) |
| `LocaleIdentifier` | DataSourceSettings | Locale ID for formatting |
| `UserName` | Authentication | Login username |
| `Password` | Authentication | Login password |
| `IsNamedSet` | Column/Row | Indicates named set element |
| `IsCalculatedField` | Column/Row/Value | Indicates calculated field |
| `HierarchyUniqueName` | CalculatedFieldSetting | Parent hierarchy for calculated dimension |
| `FormatString` | CalculatedFieldSetting | Display format for calculated result |
