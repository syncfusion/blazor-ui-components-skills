# Styling and Appearance in Blazor Kanban Component

Customize the Kanban component's appearance using CSS class overrides and built-in styling hooks.

## Table of Contents

- [CSS Class Reference](#css-classes)
- [Customizing the Fixed Header](#customizing-fixed-header)

## CSS Classes

The following CSS classes are available for customization:

| CSS Class | Description |
|-----------|-------------|
| `.e-kanban` | Root element of the Kanban component |
| `.e-kanban-table` | Kanban outer table element |
| `.e-header-row` | Header row element |
| `.e-header-cells` | Header cell elements |
| `.e-header-wrap` | Header wrap element |
| `.e-header-title` | Header title element |
| `.e-header-text` | Header title text element |
| `.e-item-count` | Item count display element in header |
| `.e-limits` | Constraint (min/max) limits display |
| `.e-min-count` | Minimum count constraint element |
| `.e-max-count` | Maximum count constraint element |
| `.e-kanban-content` | Content area (cards container) |
| `.e-content-row` | Content row element |
| `.e-content-cells` | Content cell elements |
| `.e-card-wrapper` | Wrapper containing all cards in a cell |
| `.e-card-container` | Container element for an individual card |
| `.e-card` | Individual card element |
| `.e-card-header` | Card header element |
| `.e-card-header-title` | Card header title text |
| `.e-card-content` | Card body content element |
| `.e-card-tags` | Card tag wrapper element |
| `.e-card-tag-field` | Individual card tag element |
| `.e-card-footer` | Card footer element |
| `.e-card-footer-css` | Individual footer CSS class element |
| `.e-card-left-border` | Card left border (GrabberField color) |
| `.e-swimlane-row` | Swimlane row element |
| `.e-swimlane-header` | Swimlane header row |
| `.e-swimlane-text` | Swimlane header text |
| `.e-swimlane-count` | Card count in swimlane header |
| `.e-frozen-swimlane-row` | Frozen swimlane row header |
| `.e-toggle-column` | Collapsed/toggled column element |
| `.e-collapsed` | Applied when a column is collapsed |
| `.e-empty-card` | Empty placeholder card in an empty column |
| `.e-kanban-dialog` | Built-in dialog element |
| `.e-kanban-form-wrapper` | Dialog form wrapper |
| `.e-tooltip-wrap` | Tooltip wrapper element |

## Customizing Fixed Header

When a Kanban column header is fixed/sticky during scroll, customize it with:

```css
.e-kanban .e-header-cells.e-fixed-header {
    background-color: #e0e0e0;
    font-weight: bold;
}
```

## Common Customization Examples

### Card Background Color by Priority

```css
.e-kanban .e-card[data-priority="Critical"] {
    background-color: #ffe0e0;
    border-left: 4px solid #ff4444;
}

.e-kanban .e-card[data-priority="Low"] {
    background-color: #e0ffe0;
    border-left: 4px solid #44aa44;
}
```

### Column Header Background

```css
.e-kanban .e-header-cells {
    background-color: #1e88e5;
    color: white;
}
```

### Card Hover Effect

```css
.e-kanban .e-card:hover {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transform: translateY(-2px);
    transition: all 0.2s ease;
}
```

### Swimlane Header Styling

```css
.e-kanban .e-swimlane-row .e-swimlane-header {
    background-color: #f5f5f5;
    border-bottom: 2px solid #1e88e5;
    padding: 8px 12px;
}

.e-kanban .e-swimlane-row .e-swimlane-text {
    font-size: 14px;
    font-weight: 600;
    color: #333;
}
```

### Constraint Limit Indicators

```css
/* Warning when approaching maximum */
.e-kanban .e-limits.e-max-count {
    color: #ff6f00;
    font-weight: bold;
}

/* Error when below minimum */
.e-kanban .e-limits.e-min-count {
    color: #d32f2f;
    font-weight: bold;
}
```

## Using CssClass Property

Apply a custom CSS class to the Kanban for scoped styling:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" CssClass="custom-kanban">
    ...
</SfKanban>

<style>
    .custom-kanban .e-header-cells {
        background-color: #673ab7;
        color: white;
    }

    .custom-kanban .e-card {
        border-radius: 8px;
    }
</style>
```
