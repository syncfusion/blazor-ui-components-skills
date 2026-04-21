# Selection and Highlight

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Highlight on Hover](#highlight-on-hover)
- [Programmatic Selection](#programmatic-selection)
- [Combining with Drill-Down](#combining-with-drill-down)

## Overview
How to configure `TreeMapSelectionSettings` and `TreeMapHighlightSettings` for TreeMap items. The `TreeMapSelectionSettings` class provides properties like `Enable`, `Mode`, `Fill`, and `Opacity` to manage item selection, while `TreeMapHighlightSettings` class provides the same properties to manage hover highlight behavior.

## Selection Modes
- Use `Enable` property to activate or deactivate selection functionality in `TreeMapSelectionSettings`.
- Use `Mode` property to specify the selection mode (SelectionMode.All, SelectionMode.Child, SelectionMode.Item, SelectionMode.Parent) in `TreeMapSelectionSettings`.
- Use `Fill` property to set the fill color of selected TreeMap items in `TreeMapSelectionSettings`.
- Use `Opacity` property to set the opacity level of selected TreeMap items in `TreeMapSelectionSettings`.

## Highlight on Hover
- Use `Enable` property to activate or deactivate highlight functionality on hover in `TreeMapHighlightSettings`.
- Use `Mode` property to specify the highlight mode (HighLightMode.All, HighLightMode.Child, HighLightMode.Item, HighLightMode.Parent) for hovered items in `TreeMapHighlightSettings`.
- Use `Fill` property to set the fill color of highlighted TreeMap items in `TreeMapHighlightSettings`.
- Use `Opacity` property to set the opacity level of highlighted TreeMap items in `TreeMapHighlightSettings`.

## Programmatic Selection
- Use `SelectItemAsync()` method on `SfTreeMap<TValue>` to programmatically select TreeMap items.
- Use `SelectItemAsync()` method with appropriate parameters (string[] levelOrder, bool isSelected = true) to clear or modify item selection state.

## Combining with Drill-Down
- Use selection state from `TreeMapSelectionSettings` to persist user context across drill-down transitions.
- Maintain selection context when navigating between drill-down levels in TreeMap.

## Code Sample

### Example 1: Enable Selection with Custom Fill Color

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@Data" TValue="DataModel" WeightValuePath="Weight" ColorValuePath="Color">
    <TreeMapSelectionSettings Enable="true" Mode="SelectionMode.Item" Fill="#FF5733" Opacity="0.8"></TreeMapSelectionSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Continent"></TreeMapLevel>
        <TreeMapLevel GroupPath="Country"></TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>

@code {
    private List<DataModel> Data = new List<DataModel>
    {
        new DataModel { Continent = "Asia", Country = "India", Weight = 50, Color = "#FF5733" },
        new DataModel { Continent = "Asia", Country = "China", Weight = 60, Color = "#33FF57" },
        new DataModel { Continent = "Europe", Country = "Germany", Weight = 45, Color = "#3366FF" }
    };

    public class DataModel
    {
        public string Continent { get; set; }
        public string Country { get; set; }
        public int Weight { get; set; }
        public string Color { get; set; }
    }
}
```

### Example 2: Enable Highlight on Hover with Custom Fill Color

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@Data" TValue="DataModel" WeightValuePath="Weight" ColorValuePath="Color">
    <TreeMapHighlightSettings Enable="true" Mode="HighLightMode.Item" Fill="#00FF00" Opacity="0.5"></TreeMapHighlightSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Continent"></TreeMapLevel>
        <TreeMapLevel GroupPath="Country"></TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>

@code {
    private List<DataModel> Data = new List<DataModel>
    {
        new DataModel { Continent = "Asia", Country = "India", Weight = 50, Color = "#FF5733" },
        new DataModel { Continent = "Asia", Country = "China", Weight = 60, Color = "#33FF57" },
        new DataModel { Continent = "Europe", Country = "Germany", Weight = 45, Color = "#3366FF" }
    };

    public class DataModel
    {
        public string Continent { get; set; }
        public string Country { get; set; }
        public int Weight { get; set; }
        public string Color { get; set; }
    }
}
```

### Example 3: Combining Selection and Highlight

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@Data" TValue="DataModel" WeightValuePath="Weight" ColorValuePath="Color">
    <TreeMapSelectionSettings Enable="true" Mode="SelectionMode.Item" Fill="#FF5733" Opacity="0.8"></TreeMapSelectionSettings>
    <TreeMapHighlightSettings Enable="true" Mode="HighLightMode.Item" Fill="#FFC300" Opacity="0.6"></TreeMapHighlightSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Continent"></TreeMapLevel>
        <TreeMapLevel GroupPath="Country"></TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>

@code {
    private List<DataModel> Data = new List<DataModel>
    {
        new DataModel { Continent = "Asia", Country = "India", Weight = 50, Color = "#FF5733" },
        new DataModel { Continent = "Europe", Country = "Germany", Weight = 45, Color = "#33FF57" }
    };
    public class DataModel
    {
        public string Continent { get; set; }
        public string Country { get; set; }
        public int Weight { get; set; }
        public string Color { get; set; }
    }
}
```