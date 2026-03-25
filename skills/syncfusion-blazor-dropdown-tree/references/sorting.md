# Sorting

## Table of Contents
- [Overview](#overview)
- [SortOrder Property](#sortorder-property)
- [Dynamic Sorting Example](#dynamic-sorting-example)

## Overview
The Dropdown Tree component allows you to sort nodes in `Ascending` or `Descending` order. The sorting is controlled by the `SortOrder` property.

## SortOrder Property
The `SortOrder` property supports the following values:
* **None**: Nodes are not sorted (default).
* **Ascending**: Nodes are sorted in ascending order.
* **Descending**: Nodes are sorted in descending order.

## Dynamic Sorting Example
You can dynamically update the sorting order by binding the `SortOrder` property to a component variable and updating it via user interaction.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="Ascending">Ascending Order</SfButton>
<SfButton OnClick="Descending">Descending Order</SfButton>

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    SortOrder="sortOrder">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    public Syncfusion.Blazor.Navigations.SortOrder sortOrder;
    
    // ... Data source definition ...
    
    public void Ascending()
    {
        sortOrder = Syncfusion.Blazor.Navigations.SortOrder.Ascending;
    }
    
    public void Descending()
    {
        sortOrder = Syncfusion.Blazor.Navigations.SortOrder.Descending;
    }
}
```
