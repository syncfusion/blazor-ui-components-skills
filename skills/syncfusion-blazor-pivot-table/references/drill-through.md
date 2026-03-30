# Drill Through — Syncfusion Blazor Pivot Table

Drill-through displays the raw underlying records behind an aggregated value cell in a popup data grid, allowing users to inspect detailed transactions that contribute to a summary figure.

## Enable Drill Through

Double-clicking the value cell opens the drill-through dialog listing all rows that contribute to that aggregated value.

**Enable drill-through:**
```razor
<SfPivotView TValue="ProductDetails" AllowDrillThrough="true">
    ...
</SfPivotView>
```

## Configure Drill Through Grid

**Configure the detail grid columns** (optional):
```razor
<SfPivotView TValue="ProductDetails" AllowDrillThrough="true">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
    <PivotViewDrillThroughSettings
        EnableColumnVirtualization="false">
    </PivotViewDrillThroughSettings>
</SfPivotView>
```

## Use Cases

Drill-through is ideal for financial reports where analysts need to see the transactions behind a summary figure. It surfaces data without navigating away from the pivot report.
