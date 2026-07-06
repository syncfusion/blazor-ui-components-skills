---
name: syncfusion-blazor-listview
description: Guide for implementing Syncfusion Blazor ListView component. Use this when building list-based interfaces with data binding, filtering, grouping, virtual scrolling, selection modes, and custom templates. Covers installation, templating, event handling, and advanced patterns.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Navigation Components"
---

# Implementing Syncfusion Blazor ListView Component

The Syncfusion Blazor ListView component is a feature-rich control for displaying and interacting with lists of data. It supports data binding, custom templates, grouping, filtering, virtualization, and advanced selection modes.

## When to Use This Skill

- **Setting up ListView** - Installation, namespaces, service registration, theme setup
- **Binding data** - Local data arrays, remote DataManager sources, Entity Framework
- **Creating templates** - Custom item templates, headers, footers, group headers, device-specific rendering
- **Selection & interaction** - Checkbox selection, retrieving selected items, click handlers
- **Filtering & searching** - Real-time search, text filtering, custom filter logic
- **Organizing data** - Grouping by category, group headers, hierarchical grouping
- **Performance** - Virtual scrolling for large datasets, container scroll vs window scroll
- **Advanced layouts** - Grid layouts, dual-list patterns, chat UI, mobile-responsive designs
- **Styling & customization** - CSS classes, themes, HTML attributes, responsive design
- **Event handling** - Item selection, navigation, hyperlink integration, list events

## Quick Start Example

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@ListData">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

@code {
    List<DataModel> ListData = new List<DataModel>();

    protected override void OnInitialized()
    {
        ListData.Add(new DataModel { Text = "Item 1", Id = "1" });
        ListData.Add(new DataModel { Text = "Item 2", Id = "2" });
        ListData.Add(new DataModel { Text = "Item 3", Id = "3" });
    }

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

## Common Properties & Events

### Essential Properties

- **DataSource** - Array of items or DataManager for binding
- **HeaderTitle** - Optional header text
- **ShowHeader** - Display/hide header (boolean)
- **EnableCheckbox** - Enable checkbox selection (boolean)
- **EnableVirtualization** - Virtual scrolling for large lists (boolean)
- **Height** - Container height (required for virtual scrolling)
- **ShowCheckBox** - Display checkboxes for selection
- **SortOrder** - Sort items (Ascending/Descending)

### Template Properties

- **HeaderTemplate** - Customize header appearance
- **Template** - Custom item rendering
- **GroupTemplate** - Group header customization
- **FooterTemplate** - Footer customization

### Selection & Data

- **ListViewFieldSettings** - Map data properties:
  - `Id` - Item identifier
  - `Text` - Display text
  - `GroupBy` - Grouping field
  - `Child` - Nested items
  - `IconCss` - Icon class
  - `Tooltip` - Hover tooltip

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation in WebAssembly, Web App, and Server App projects
- Package installation (NuGet)
- Namespace imports and service registration
- Theme stylesheet and script references
- First ListView component setup
- Minimal working example

### Data Binding and Sources
📄 **Read:** [references/data-binding-and-source.md](references/data-binding-and-source.md)
- Local data arrays and JSON binding
- Remote data with DataManager
- OData, OData V4, Web API integration
- Entity Framework binding
- Field mapping with ListViewFieldSettings
- Dynamic data updates and refresh

### Templating and Rendering
📄 **Read:** [references/templating-and-rendering.md](references/templating-and-rendering.md)
- Item template customization
- Header and footer templates
- Group header templates
- Device-specific templates (responsive rendering)
- Template element classes (e-list-template, e-list-wrapper, etc.)
- Avatar and badge templates
- Multi-line items

### Item Selection and Actions
📄 **Read:** [references/item-selection-and-actions.md](references/item-selection-and-actions.md)
- GetCheckedItemsAsync method
- Selection with checkboxes
- Single and multiple selection modes
- Retrieving selected item data and indices
- Custom template selection handling
- Click event handlers

### Filtering and Searching
📄 **Read:** [references/filtering-and-searching.md](references/filtering-and-searching.md)
- Real-time search implementation
- Text input filtering
- Filter logic and predicates
- String matching strategies
- Case-insensitive filtering
- Custom filter conditions

### Grouping and Organization
📄 **Read:** [references/grouping-and-organization.md](references/grouping-and-organization.md)
- GroupBy field configuration
- Group header styling
- Collapsible groups
- Category-based organization
- Hierarchical grouping patterns

### Virtualization and Performance
📄 **Read:** [references/virtualization-and-performance.md](references/virtualization-and-performance.md)
- EnableVirtualization property
- Window scroll vs container scroll
- Height configuration
- Large dataset handling (1000+ items)
- Performance optimization techniques
- Virtual scrolling limitations and constraints

### Advanced Layouts
📄 **Read:** [references/advanced-layouts.md](references/advanced-layouts.md)
- Grid layout customization
- Dual-list/multi-select patterns
- Mobile-responsive contact layouts
- Chat window UI implementation
- Layout-specific CSS classes
- Responsive design patterns

### Styling and Customization
📄 **Read:** [references/styling-and-customization.md](references/styling-and-customization.md)
- CSS class customization (e-listview, e-list-item, e-list-header)
- Theme integration and colors
- Hover state styling
- Selected item styling
- Group header styling
- HTML attribute support
- CSS structure and selectors

### Events and Navigation
📄 **Read:** [references/events-and-navigation.md](references/events-and-navigation.md)
- ListViewItem event handling
- Click and selection events
- Navigation patterns
- Hyperlink integration and routing
- Event method signatures
- Event context and data

### Advanced Patterns
📄 **Read:** [references/advanced-patterns.md](references/advanced-patterns.md)
- Nested list implementation
- Add/remove items dynamically
- CRUD operations on ListData
- Complex data scenarios
- Multiple nested levels
- Best practices for hierarchical data

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete properties documentation with types and defaults
- All component methods with parameters and return types
- Event signatures and event arguments
- Field settings and field mapping configuration
- Template definitions and usage
- Supporting enums (SortOrder, CheckBoxPosition, ListViewEffect)
- Complete working example with all features

## Key Props Reference

| Property | Type | Purpose |
|----------|------|---------|
| `DataSource` | Array/DataManager | Items to display |
| `HeaderTitle` | string | Header text |
| `ShowHeader` | bool | Show/hide header |
| `ShowCheckBox` | bool | Enable checkboxes |
| `EnableVirtualization` | bool | Virtual scrolling |
| `Height` | string | Component height (px) |
| `SortOrder` | SortOrder | Ascending/Descending |
| `CssClass` | string | Custom CSS class |
| `Template` | RenderFragment | Item template |
| `HeaderTemplate` | RenderFragment | Header template |

## Common Use Cases

1. **Simple list display** - Use basic DataSource binding with ListViewFieldSettings
2. **Selectable lists** - Add ShowCheckBox="true" with GetCheckedItemsAsync()
3. **Searchable list** - Bind TextBox input to filter DataSource in OnInput events
4. **Grouped lists** - Map GroupBy field in ListViewFieldSettings
5. **Large datasets** - Enable virtualization with EnableVirtualization="true" and Height
6. **Custom UI** - Use Template property with multi-line layouts, avatars, badges
7. **Nested lists** - Map Child field and use recursive templates
8. **Responsive design** - Device-specific templates or dynamic templates based on viewport

---

*This skill provides architecture and patterns for all ListView scenarios. Reference the specific guide files for detailed implementations and code examples.*
