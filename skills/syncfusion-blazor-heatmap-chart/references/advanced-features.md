# Advanced Features - Blazor HeatMap Chart Component

This comprehensive guide covers advanced features and configurations for the Syncfusion Blazor HeatMap Chart component, including dimensions, selection, Content Security Policy (CSP) considerations, and accessibility features.

## Table of Contents

1. [Dimensions and Sizing](#dimensions-and-sizing)
   - [Size Configuration Overview](#size-configuration-overview)
   - [Pixel-Based Sizing](#pixel-based-sizing)
   - [Percentage-Based Sizing](#percentage-based-sizing)
2. [Cell Selection](#cell-selection)
   - [Enabling Selection](#enabling-selection)
   - [Selection Interaction Modes](#selection-interaction-modes)
   - [Single Cell Selection](#single-cell-selection)
   - [Multiple Cell Selection](#multiple-cell-selection)
   - [Clearing Cell Selection](#clearing-cell-selection)
3. [Content Security Policy (CSP)](#content-security-policy-csp)
   - [Features Supported Under Strict CSP](#features-supported-under-strict-csp)
   - [Features Requiring CSP Relaxation](#features-requiring-csp-relaxation)
   - [Recommended CSP Configurations](#recommended-csp-configurations)
4. [Accessibility](#accessibility)
   - [Accessibility Compliance Standards](#accessibility-compliance-standards)
   - [WAI-ARIA Attributes](#wai-aria-attributes)
   - [Screen Reader Support](#screen-reader-support)
   - [Accessibility Testing](#accessibility-testing)
5. [Performance Optimization](#performance-optimization)
6. [Server-Side Rendering Considerations](#server-side-rendering-considerations)

---

## Dimensions and Sizing

### Size Configuration Overview

You can control the size of the HeatMap Chart directly using the `Width` and `Height` properties. The component supports both pixel-based and percentage-based sizing to provide flexibility for different layout requirements.

### Pixel-Based Sizing

Set fixed dimensions for the HeatMap using pixel values. This approach provides precise control over the chart size regardless of the container dimensions.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData" Width="750px" Height="350px">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Best Practices for Pixel-Based Sizing:**
- Use pixel values when you need consistent sizing across different screen sizes
- Ideal for fixed-layout designs or when the HeatMap is part of a dashboard with specific dimensions
- Ensure the pixel values are appropriate for your target display resolutions

### Percentage-Based Sizing

By setting dimensions in percentage values, the HeatMap adapts to its container size, providing responsive behavior. For example, setting height to '50%' renders the HeatMap at half the container's height.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData" Width="75%" Height="75%">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Best Practices for Percentage-Based Sizing:**
- Use percentage values for responsive layouts that adapt to different container sizes
- Ideal for flexible layouts and responsive designs
- Ensure the parent container has a defined height when using percentage-based height
- Combine with CSS media queries for optimal responsive behavior

---

## Cell Selection

### Enabling Selection

The Blazor HeatMap Chart supports cell selection functionality, allowing users to select single or multiple cells at runtime. Enable this feature using the `AllowSelection` property and capture selection events using the `CellSelected` event.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap AllowSelection="true" DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
</SfHeatMap>

@code{
    public int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    public string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    public string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

### Selection Interaction Modes

The HeatMap supports multiple interaction modes for cell selection:

| Interaction Mode | Description |
|------------------|-------------|
| **Mouse** | Cells can be selected by clicking or dragging and dropping over them |
| **Touch** | Cells can be selected by tapping or dragging and dropping over them |
| **Keyboard** | Use the **Ctrl** key with mouse or touch interactions to enable multiple cell selection. The Ctrl key only works when `EnableMultiSelect` is set to **true** |

Users can select cells by:
- **Single click**: Select a single cell
- **Click and drag**: Select multiple cells by dragging the mouse across them
- **Ctrl + Click**: Add multiple individual cells to the selection (when `EnableMultiSelect` is true)

### Single Cell Selection

To restrict selection to only one cell at a time, set the `EnableMultiSelect` property to **false**. This ensures that selecting a new cell will automatically deselect the previously selected cell.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@DataSource" AllowSelection="true" EnableMultiSelect="false">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)"></HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YLabels"></HeatMapYAxis>
</SfHeatMap>

@code{
    public int YIndex = 0;
    public string[] XLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] YLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
    public double[,] DataSource = new double[,]
    {
         { 73, 39, 26, 39, 94, 0 },
         { 93, 58, 53, 38, 26, 68 },
         { 99, 28, 22, 4, 66, 90 },
         { 14, 26, 97, 69, 69, 3 },
         { 7, 46, 47, 47, 88, 6 },
         { 41, 55, 73, 23, 3, 79 },
         { 56, 69, 21, 86, 3, 33 },
         { 45, 7, 53, 81, 95, 79 },
         { 60, 77, 74, 68, 88, 51 },
         { 25, 25, 10, 12, 78, 14 },
         { 25, 56, 55, 58, 12, 82 },
         {74, 33, 88, 23, 86, 59 }
    };
}
```

**Use Cases for Single Cell Selection:**
- Drill-down scenarios where only one data point should be active
- Detail view displays based on a single selected cell
- Simplified user interactions in mobile or touch interfaces

### Multiple Cell Selection

By default, the `EnableMultiSelect` property is set to **true**, allowing users to select multiple cells simultaneously. Users can:
- Click and drag to select a rectangular region of cells
- Hold the Ctrl key and click individual cells to add them to the selection
- Use touch gestures to select multiple cells on touch-enabled devices

**Use Cases for Multiple Cell Selection:**
- Batch operations on multiple data points
- Comparison analysis across multiple cells
- Data export or reporting features
- Complex data filtering scenarios

### Clearing Cell Selection

Use the `ClearSelectionAsync` method to programmatically clear all selected cells. This is useful for reset functionality or when transitioning between different views.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap @ref="Heatmap" AllowSelection="true" DataSource="@dataSource">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
</SfHeatMap>

<button @onclick="ClearSelection">Clear Selection</button>

@code {
    public SfHeatMap<int[,]> Heatmap;
   
    public int[,] dataSource = new int[,]
    {
        {73, 39, 26, 39, 94, 0},
        {93, 58, 53, 38, 26, 68},
        {99, 28, 22, 4, 66, 90},
        {14, 26, 97, 69, 69, 3},
        {7, 46, 47, 47, 88, 6},
        {41, 55, 73, 23, 3, 79}
    };
       
    public string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    public string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    
    public async Task ClearSelection()
    {
        await Heatmap.ClearSelectionAsync();
    }
}
```

**Implementation Tips:**
- Call `ClearSelectionAsync` when switching between data sets
- Use in conjunction with filter or search operations
- Provide a visible "Clear Selection" button for better user experience
- Consider clearing selection automatically after performing batch operations

---

## Content Security Policy (CSP)

Content Security Policy is a security standard that helps prevent cross-site scripting (XSS), clickjacking, and other code injection attacks. Understanding CSP requirements is crucial for deploying secure HeatMap implementations.

### Features Supported Under Strict CSP

The Syncfusion Blazor HeatMap component supports most features under strict Content Security Policy without requiring the `'unsafe-inline'` directive. The following features work fully with strict CSP:

- **Data Binding**: Both one-dimensional and two-dimensional data sources
- **Color Mapping**: Palette-based, gradient, and fixed color configurations
- **Axis Customization**: Labels, ticks, inversed axis, opposed positioning
- **Cell Rendering**: All cell types and spacing configurations
- **Legends**: Legend display with customizable positions and styles
- **Tooltips**: Interactive tooltips with custom templates
- **Title and Subtitle**: Chart title and subtitle text
- **RTL Support**: Right-to-left layout support
- **Accessibility Features**: WAI-ARIA attributes and screen reader support
- **Keyboard Navigation**: Full keyboard interaction support
- **Export Functionality**: Image and PDF export capabilities

### Features Requiring CSP Relaxation

**Cell Selection** (both single and multiple) requires the `style-src 'unsafe-inline'` directive in your Content Security Policy configuration.

#### Why Selection Requires `'unsafe-inline'`

Cell selection applies dynamic inline styles at runtime to provide visual feedback for user interactions. These dynamic styles include:

- **Selection Borders and Outlines**: Visual indicators around selected cells
- **Background Color Changes**: Highlighting selected cells with different colors
- **Opacity and Brightness Adjustments**: Visual emphasis for active selections
- **Focus Rings and Highlight Effects**: Clear indication of currently selected cells

These visual indicators are applied in real-time during user interaction and are blocked under strict CSP policies that prohibit inline styles.

#### Disabling Selection for Strict CSP Compliance

If strict CSP compliance is a priority and cell selection is not required, you can:
- Set `AllowSelection="false"` in your HeatMap configuration
- Simply omit selection-related properties and events
- The rest of the HeatMap functionality will work fully under strict CSP

### Recommended CSP Configurations

#### Strict CSP (Without Cell Selection)

Use this configuration for maximum security when cell selection functionality is not required:

```html
<meta http-equiv="Content-Security-Policy"
      content="base-uri 'self';
               default-src 'self';
               connect-src 'self' https: ws: wss:;
               img-src 'self' data: https:;
               object-src 'none';
               script-src 'self';
               style-src 'self';
               font-src 'self' data:;
               upgrade-insecure-requests;">
```

**Benefits of Strict CSP:**
- Maximum protection against XSS attacks
- Compliance with strict security policies
- Suitable for high-security environments
- Full data visualization capabilities maintained

#### Relaxed CSP (With Cell Selection)

Use this configuration when cell selection functionality is essential for user workflows:

```html
<meta http-equiv="Content-Security-Policy"
      content="base-uri 'self';
               default-src 'self';
               connect-src 'self' https: ws: wss:;
               img-src 'self' data: https:;
               object-src 'none';
               script-src 'self';
               style-src 'self' 'unsafe-inline';
               font-src 'self' data:;
               upgrade-insecure-requests;">
```

**When to Use Relaxed CSP:**
- Cell selection is essential for drill-down functionality
- Users need to select multiple cells for data exploration
- Interactive data analysis requires cell selection
- Export or reporting features depend on cell selection

**Security Considerations:**
- Only relax CSP when selection features are absolutely necessary
- Document the security trade-offs in your project
- Consider implementing additional security measures to compensate
- Regularly review and audit CSP configurations

---

## Accessibility

### Accessibility Compliance Standards

The Blazor HeatMap Chart component follows commonly used accessibility guidelines and standards, ensuring that the component is usable by all users, including those with disabilities.

**Supported Standards:**
- **ADA** (Americans with Disabilities Act)
- **Section 508** (U.S. Federal accessibility requirements)
- **WCAG 2.2** (Web Content Accessibility Guidelines 2.2)
- **WAI-ARIA** (Web Accessibility Initiative - Accessible Rich Internet Applications)

#### Accessibility Compliance Table

| Accessibility Criteria | Compatibility Level |
|------------------------|---------------------|
| WCAG 2.2 Support | AA |
| Section 508 Support | Partial |
| Screen Reader Support | Full |
| Right-To-Left Support | Not Applicable |
| Color Contrast | Full |
| Mobile Device Support | Full |
| Keyboard Navigation Support | Not Applicable |
| Axe-core Accessibility Validation | Full |

**Compatibility Legend:**
- **Full**: All features meet the requirement
- **Partial**: Some features meet the requirement
- **Not Applicable**: The requirement does not apply to this component

### WAI-ARIA Attributes

The Blazor HeatMap Chart implements WAI-ARIA patterns to ensure proper semantic structure and accessibility information for assistive technologies.

| ARIA Attribute | Purpose | Applied To |
|----------------|---------|------------|
| `role=img` | Specifies content presented in a visual manner | Legend and border elements |
| `role=region` | Identifies significant areas that don't support interactive functions | Non-interactive HeatMap areas |
| `aria-label` | Provides accessible names for elements | Title, legend title, legend item labels, axis labels, cell labels, and multilevel labels |

**Implementation Details:**
- The `role=img` attribute ensures screen readers understand visual content as images
- The `role=region` attribute helps users navigate between different sections
- `aria-label` attributes provide context and descriptions for all visible text elements
- Labels are dynamically generated based on data and configuration

### Screen Reader Support

The Blazor HeatMap Chart provides comprehensive screen reader support, ensuring all visual information is accessible through audio descriptions.

#### Elements Read by Screen Readers

| Element | Description |
|---------|-------------|
| **Title** | Reads the main title text of the HeatMap Chart |
| **Axis Labels** | Announces X-axis and Y-axis labels in sequence |
| **Multilevel Labels** | Reads hierarchical labels in both X and Y axes |
| **Cell Labels** | Announces the value and position of each cell |
| **Legend Title** | Reads the legend's title text |
| **Legend Item Labels** | Announces each legend item's label and meaning |

**Screen Reader Best Practices:**
- Provide descriptive titles that explain the chart's purpose
- Use clear, concise axis labels
- Ensure cell values are formatted appropriately
- Include meaningful legend labels
- Test with multiple screen readers (NVDA, JAWS, Narrator)

**Example Screen Reader Flow:**
1. "Sales Revenue per Employee HeatMap Chart" (Title)
2. "X-axis: Nancy, Andrew, Janet, Margaret, Steven, Michael" (X-axis labels)
3. "Y-axis: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday" (Y-axis labels)
4. "Cell: Nancy, Monday, Value: 73" (Cell information)
5. "Legend: High to Low revenue scale" (Legend information)

### Accessibility Testing

#### Automated Testing with Axe-core

The Blazor HeatMap component's accessibility levels are validated using the axe-core software tool during automated testing. This ensures compliance with WCAG standards and identifies potential accessibility issues.

**Testing Process:**
1. Integrate axe-core into your testing pipeline
2. Run automated accessibility scans on HeatMap instances
3. Review and address any flagged issues
4. Verify fixes with both automated and manual testing

#### Manual Testing Recommendations

- **Keyboard Navigation**: Verify all interactive elements are keyboard accessible
- **Screen Reader Testing**: Test with multiple screen readers
- **Color Contrast**: Verify sufficient contrast ratios (minimum 4.5:1 for normal text)
- **Focus Indicators**: Ensure visible focus states for all interactive elements
- **Zoom Testing**: Test at 200% zoom to verify layout integrity
- **Touch Target Size**: Verify touch targets are at least 44x44 pixels

#### Accessibility Evaluation Sample

To evaluate the HeatMap component's accessibility, you can:
1. Test with browser accessibility tools (Chrome DevTools, Firefox Accessibility Inspector)
2. Use the axe-core NuGet package: `Deque.AxeCore.Playwright`
3. Refer to the official Syncfusion accessibility samples
4. Conduct user testing with individuals who use assistive technologies

---

## Performance Optimization

### Data Optimization

**Large Dataset Handling:**
- Use appropriately sized data arrays for your use case
- Consider data aggregation for very large datasets (>1000 cells)
- Implement virtual scrolling if available for extremely large HeatMaps
- Use efficient data structures (arrays over collections)

### Rendering Optimization

**Reduce Re-rendering:**
- Implement `ShouldRender()` logic in parent components to control re-rendering
- Use `@key` directive to optimize Blazor's diff algorithm
- Avoid unnecessary state changes that trigger re-renders
- Cache computed properties in `@code` blocks

**Cell Rendering:**
- Limit cell label visibility for large datasets using `ShowLabel` strategically
- Use appropriate `TileType` (Rect vs Circle) based on data density
- Consider disabling features like selection for read-only scenarios

### Legend and Tooltip Optimization

- Disable tooltips if not needed: `<HeatMapTooltipSettings Enable="false">`
- Minimize legend complexity for faster rendering
- Use appropriate legend position to avoid layout recalculations

### Memory Management

- Properly dispose of HeatMap instances when components are destroyed
- Clear large data references when navigating away
- Monitor memory usage in browser DevTools
- Use `@implements IDisposable` for cleanup logic

---

## Server-Side Rendering Considerations

### Blazor Server vs. WebAssembly

**Blazor Server Considerations:**
- HeatMap rendering occurs on the server with UI updates sent via SignalR
- Be mindful of SignalR payload size with large datasets
- Consider network latency impact on interactive features
- Implement loading indicators for better user experience

**Blazor WebAssembly Considerations:**
- All rendering occurs in the browser, reducing server load
- Initial load time may be higher due to WASM download
- Better performance for highly interactive HeatMaps
- No server connection required after initial load

### Pre-rendering Optimization

**Static Rendering:**
- Enable pre-rendering for faster initial page loads
- HeatMaps are rendered on the server during initial request
- Reduces perceived load time for users
- Configure in `App.razor` or `_Host.cshtml`

**Pre-rendering Challenges:**
- JavaScript interop not available during pre-render phase
- Avoid client-side only APIs during initialization
- Handle `OnAfterRender` appropriately for JS initialization

### State Management

- Use appropriate state management patterns (Cascade Parameters, State Containers)
- Avoid excessive state synchronization between client and server
- Consider local storage for client-side caching in WebAssembly
- Implement efficient data refresh strategies

### Connection Resilience

**Blazor Server Specific:**
- Implement reconnection UI for SignalR disconnections
- Handle circuit timeout scenarios gracefully
- Consider implementing auto-save for user selections
- Provide offline fallback messages

---

## Summary

This guide covered the advanced features of the Syncfusion Blazor HeatMap Chart component:

1. **Dimensions**: Flexible sizing with pixel and percentage-based options for responsive layouts
2. **Selection**: Interactive cell selection with single and multiple modes, programmatic control
3. **CSP Compliance**: Understanding security requirements and appropriate configurations
4. **Accessibility**: Full WCAG 2.2 compliance, screen reader support, and WAI-ARIA implementation
5. **Performance**: Optimization strategies for large datasets and efficient rendering
6. **Server-Side Rendering**: Best practices for both Blazor Server and WebAssembly deployments

These features enable you to create accessible, secure, and performant HeatMap visualizations that meet enterprise requirements while providing excellent user experiences across all platforms and devices.
