# Content Rendering in Blazor Accordion Component

This guide explains how the Accordion component renders content and how to optimize performance using the `LoadOnDemand` property.

## Table of Contents
- [Overview](#overview)
- [LoadOnDemand Property](#loadondemand-property)
- [On-Demand Loading (Default Behavior)](#on-demand-loading-default-behavior)
- [Pre-Rendering All Content](#pre-rendering-all-content)
- [Performance Comparison](#performance-comparison)
- [Best Practices](#best-practices)
- [Advanced Scenarios](#advanced-scenarios)
- [Impact on Initial Expansion](#impact-on-initial-expansion)
- [Troubleshooting](#troubleshooting)
- [Related Topics](#related-topics)

## Overview

The Accordion component provides two content rendering strategies:

1. **On-Demand Loading (Default):** Content is rendered only when an item is expanded
2. **Pre-Rendering:** All content is rendered at initial load and maintained in the DOM

The rendering strategy is controlled by the `LoadOnDemand` property.

## LoadOnDemand Property

The `LoadOnDemand` property determines when accordion item content is rendered.

- **`true` (Default):** Content is loaded only when the item is expanded for the first time
- **`false`:** All content is rendered immediately when the accordion loads

## On-Demand Loading (Default Behavior)

### How It Works

With `LoadOnDemand="true"` (the default):

1. Initially, only accordion headers are rendered
2. When a user expands an item for the first time, its content is rendered
3. Once rendered, the content remains in the DOM (cached)
4. Subsequent expand/collapse actions don't re-render the content

### When to Use On-Demand Loading

**Use LoadOnDemand="true" when:**
- Accordion has many items (5+ items)
- Content is complex (images, charts, nested components)
- Initial page load time is critical
- Not all users will expand every item
- Memory efficiency is important

### Implementation

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion LoadOnDemand="true">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="This content loads only when Item 1 is expanded."></AccordionItem>
        <AccordionItem Header="Item 2" Content="This content loads only when Item 2 is expanded."></AccordionItem>
        <AccordionItem Header="Item 3" Content="This content loads only when Item 3 is expanded."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Note:** Since `LoadOnDemand="true"` is the default, you can omit this property:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Performance Benefits

**Initial Load:**
- Faster page load time
- Reduced initial HTML size
- Lower memory consumption
- Improved perceived performance

**Runtime:**
- Content is rendered only once (cached after first expansion)
- No re-rendering on subsequent expand/collapse
- Smooth expand/collapse animations

## Pre-Rendering All Content

### How It Works

With `LoadOnDemand="false"`:

1. All accordion item content is rendered immediately on page load
2. Content exists in the DOM whether items are expanded or collapsed
3. Expanding/collapsing items only shows/hides existing content
4. No additional rendering occurs during user interaction

### When to Use Pre-Rendering

**Use LoadOnDemand="false" when:**
- Small number of items (2-3 items)
- Content is simple (text-only, minimal HTML)
- All content will likely be viewed
- SEO is important (content indexing)
- Need to access all content via JavaScript immediately
- Client-side search needs access to all content

### Implementation

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion LoadOnDemand="false">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="All content is pre-rendered."></AccordionItem>
        <AccordionItem Header="Item 2" Content="This content is in the DOM even when collapsed."></AccordionItem>
        <AccordionItem Header="Item 3" Content="Expand/collapse only toggles visibility."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Trade-offs

**Advantages:**
- Instant expand/collapse (no rendering delay)
- Content available for search/indexing
- Simpler for screen readers
- Predictable memory usage

**Disadvantages:**
- Slower initial page load
- Higher initial memory consumption
- All content loads even if never viewed
- Not ideal for large accordions

## Performance Comparison

### Scenario: 10 Items with Complex Content

| Metric | LoadOnDemand="true" | LoadOnDemand="false" |
|--------|-------------------|---------------------|
| Initial Load Time | Fast | Slow |
| Initial Memory | Low | High |
| First Expand Delay | Slight | None |
| Total Memory (all expanded) | Same | Same |
| Recommended For | Most cases | SEO/small accordions |

### Scenario: 3 Items with Simple Text

| Metric | LoadOnDemand="true" | LoadOnDemand="false" |
|--------|-------------------|---------------------|
| Initial Load Time | Minimal difference | Minimal difference |
| User Experience | Excellent | Excellent |
| Recommended For | Either | Either |

## Best Practices

### Default Recommendation

For most use cases, keep the default `LoadOnDemand="true"`:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Header 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Header 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### When to Override

Only set `LoadOnDemand="false"` if you have a specific need:

```cshtml
<!-- SEO-critical content that must be indexed -->
<SfAccordion LoadOnDemand="false">
    <AccordionItems>
        <AccordionItem Header="Product Description" Content="Important product details for search engines."></AccordionItem>
        <AccordionItem Header="Specifications" Content="Technical specs that need to be indexed."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Combining with ExpandMode

The `LoadOnDemand` property works with both expand modes:

#### Single Mode + On-Demand

```cshtml
<SfAccordion ExpandMode="ExpandMode.Single" LoadOnDemand="true">
    <AccordionItems>
        <AccordionItem Expanded="true" Header="FAQ 1" Content="Answer 1 is pre-loaded because it's initially expanded."></AccordionItem>
        <AccordionItem Header="FAQ 2" Content="Answer 2 loads when first expanded."></AccordionItem>
        <AccordionItem Header="FAQ 3" Content="Answer 3 loads when first expanded."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Behavior:** Initially expanded item's content is rendered immediately. Other items load on first expansion.

#### Multiple Mode + On-Demand

```cshtml
<SfAccordion ExpandMode="ExpandMode.Multiple" LoadOnDemand="true" @bind-ExpandedIndices="@(new int[]{0, 2})">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Pre-loaded (initially expanded)."></AccordionItem>
        <AccordionItem Header="Item 2" Content="Loads on first expand."></AccordionItem>
        <AccordionItem Header="Item 3" Content="Pre-loaded (initially expanded)."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Behavior:** Initially expanded items (0 and 2) are pre-rendered. Item 1 loads when first expanded.

## Advanced Scenarios

### Heavy Content with LoadOnDemand

For accordion items with complex content (charts, grids, large images):

```cshtml
<SfAccordion LoadOnDemand="true">
    <AccordionItems>
        <AccordionItem Header="Sales Dashboard">
            <ContentTemplate>
                <!-- Complex chart component only renders when expanded -->
                <SfChart>
                    <!-- Chart configuration -->
                </SfChart>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem Header="Data Grid">
            <ContentTemplate>
                <!-- Large data grid only renders when expanded -->
                <SfGrid DataSource="@LargeDataSet">
                    <!-- Grid configuration -->
                </SfGrid>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Benefit:** Heavy components only initialize when users actually need them.

### Lazy Data Loading

Combine `LoadOnDemand` with async data fetching:

```cshtml
<SfAccordion LoadOnDemand="true">
    <AccordionEvents Expanding="OnExpanding"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="User Data">
            <ContentTemplate>
                @if (UserData != null) {
                    <div>@UserData</div>
                } else {
                    <div>Loading...</div>
                }
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    string UserData = null;
    
    async void OnExpanding(ExpandEventArgs args) {
        if (args.Index == 0 && UserData == null) {
            UserData = await FetchUserDataAsync();
            StateHasChanged();
        }
    }
    
    async Task<string> FetchUserDataAsync() {
        await Task.Delay(1000); // Simulate API call
        return "User data loaded from API";
    }
}
```

**Pattern:** Content shell renders on expand, then data is fetched and displayed.

## Impact on Initial Expansion

### With Expanded Property

Items marked as `Expanded="true"` are rendered immediately regardless of `LoadOnDemand` setting:

```cshtml
<SfAccordion LoadOnDemand="true">
    <AccordionItems>
        <AccordionItem Expanded="true" Header="Item 1" Content="This IS rendered on page load."></AccordionItem>
        <AccordionItem Header="Item 2" Content="This is NOT rendered until expanded."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### With ExpandedIndices

Items in the `ExpandedIndices` array are rendered immediately:

```cshtml
<SfAccordion LoadOnDemand="true" @bind-ExpandedIndices="@(new int[]{0, 2})">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Rendered on page load."></AccordionItem>
        <AccordionItem Header="Item 2" Content="NOT rendered until expanded."></AccordionItem>
        <AccordionItem Header="Item 3" Content="Rendered on page load."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

## Troubleshooting

### Content Not Loading

**Issue:** Content doesn't appear when item is expanded

**Solutions:**
- Verify `LoadOnDemand` is set correctly
- Check browser console for errors
- Ensure ContentTemplate has valid markup
- Verify data is available when content renders

### Slow Initial Load

**Issue:** Page takes too long to load with accordion

**Solutions:**
- Ensure `LoadOnDemand="true"` (default)
- Reduce number of initially expanded items
- Optimize content within items
- Consider lazy loading for heavy components

### SEO Issues

**Issue:** Search engines not indexing accordion content

**Solutions:**
- Set `LoadOnDemand="false"` for SEO-critical content
- Use meaningful header text (always visible)
- Consider server-side rendering for important content
- Test with Google Search Console

## Related Topics

- [Getting Started](getting-started.md) - Basic accordion setup
- [Expand Modes](expand-modes.md) - Controlling expansion behavior
- [Data Binding and Events](data-binding-events.md) - Dynamic content and event handling
- [How-To Scenarios](how-to-scenarios.md) - Practical implementation examples
