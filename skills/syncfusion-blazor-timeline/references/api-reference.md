# Syncfusion Blazor Timeline API Reference

## Table of Contents
- [Overview](#overview)
- [SfTimeline Component](#sftimeline-component)
- [TimelineItem Component](#timelineitem-component)
- [Enumerations](#enumerations)
- [Event Arguments](#event-arguments)
- [Events](#events)
- [TimelineItemContext](#timelineitemcontext)
- [Methods](#methods)
- [Styling and CSS Classes](#styling-and-css-classes)
- [CSS Variables](#css-variables)
- [Complete Working Example](#complete-working-example)
- [Common Use Cases](#common-use-cases)
- [Performance Considerations](#performance-considerations)
- [Browser Support](#browser-support)
- [See Also](#see-also)

## Overview

The **SfTimeline** component is a layout control that presents a series of events or activities in chronological order. It supports both vertical and horizontal orientations with customizable alignment, styling, and templating capabilities.

**Namespace:** `Syncfusion.Blazor.Layouts`  
**Assembly:** `Syncfusion.Blazor.dll`

---

## SfTimeline Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Alignment` | TimelineAlignment | After | Before, After, Alternate, AlternateReverse | Controls the display of timeline content relative to the component | [Read More](timeline-alignment.md) |
| `Orientation` | TimelineOrientation | Vertical | Vertical, Horizontal | Sets the layout direction of timeline items | [Read More](timeline-orientation.md) |
| `Reverse` | bool | false | true, false | Reverses the order of timeline items | [Read More](#reverse-property) |
| `CssClass` | string | "" | Any valid CSS class name | Custom CSS class for styling the Timeline component | [Read More](timeline-styling-customization.md) |
| `ID` | string | null | Any string value | Sets the id attribute for the timeline element | [Read More](#id-property) |
| `Template` | RenderFragment<TimelineItemContext> | null | Custom template | Defines a custom template for rendering items | [Read More](timeline-templates.md) |
| `Created` | EventCallback<object> | - | Event callback | Event raised when Timeline rendering is completed | [Read More](timeline-events.md#created-event) |
| `ItemRendered` | EventCallback<TimelineRenderedEventArgs> | - | Event callback | Event raised when each Timeline item is rendered | [Read More](timeline-events.md#itemrendered-event) |

#### Reverse Property

```razor
<SfTimeline Reverse="true">
    <TimelineItems>
        <TimelineItem><Content>Item 1</Content></TimelineItem>
        <TimelineItem><Content>Item 2</Content></TimelineItem>
        <TimelineItem><Content>Item 3</Content></TimelineItem>
    </TimelineItems>
</SfTimeline>
```

When `Reverse="true"`, items render in reverse order from the source collection.

#### ID Property

```razor
<SfTimeline ID="myTimeline">
    <TimelineItems>
        <TimelineItem><Content>Event</Content></TimelineItem>
    </TimelineItems>
</SfTimeline>
```

Sets the HTML id attribute for the Timeline container element, useful for CSS targeting and DOM queries.

---

## TimelineItem Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Content` | RenderFragment<TimelineItemContext> | null | Custom template | Defines the main content of the timeline item | [Read More](timeline-items.md#adding-content) |
| `OppositeContent` | RenderFragment<TimelineItemContext> | null | Custom template | Defines secondary content opposite to main content | [Read More](timeline-items.md#opposite-content) |
| `DotCss` | string | "" | CSS class names | CSS class for customizing the dot indicator | [Read More](timeline-items.md#customizing-dots) |
| `Disabled` | bool | false | true, false | Disables the timeline item | [Read More](timeline-items.md#disabling-items) |
| `CssClass` | string | "" | Any valid CSS class name | Custom CSS class for styling individual items | [Read More](timeline-items.md#css-classes) |

### Example: Complete TimelineItem Usage

```razor
@using Syncfusion.Blazor.Layouts

<SfTimeline Alignment="TimelineAlignment.Alternate">
    <TimelineItems>
        <TimelineItem DotCss="e-icons e-check" CssClass="completed-item">
            <Content>Order Confirmed</Content>
            <OppositeContent>2024-01-15</OppositeContent>
        </TimelineItem>
        <TimelineItem DotCss="e-icons e-box">
            <Content>Shipped</Content>
            <OppositeContent>2024-01-16</OppositeContent>
        </TimelineItem>
        <TimelineItem DotCss="e-icons e-location" Disabled="false">
            <Content>In Transit</Content>
            <OppositeContent>2024-01-17</OppositeContent>
        </TimelineItem>
    </TimelineItems>
</SfTimeline>
```

---

## Enumerations

### TimelineAlignment Enum

Specifies the alignment of timeline items relative to the timeline axis.

| Value | Description |
|-------|-------------|
| `Before` | Content placed before/left, OppositeContent after/right |
| `After` | Content placed after/right, OppositeContent before/left |
| `Alternate` | Content alternates between sides regardless of orientation |
| `AlternateReverse` | Content alternates in reverse regardless of orientation |

**Usage:**
```razor
<SfTimeline Alignment="TimelineAlignment.Alternate">
    <!-- items -->
</SfTimeline>
```

---

### TimelineOrientation Enum

Specifies the layout direction of the Timeline component.

| Value | Description |
|-------|-------------|
| `Vertical` | Items displayed top-to-bottom (default) |
| `Horizontal` | Items displayed side-by-side |

**Usage:**
```razor
<SfTimeline Orientation="TimelineOrientation.Horizontal">
    <!-- items -->
</SfTimeline>
```

---

## Event Arguments

### TimelineRenderedEventArgs Class

Provides event data when a timeline item is rendered.

| Property | Type | Description |
|----------|------|-------------|
| `Index` | int | Zero-based index of the rendered item |
| `Item` | TimelineItem | Reference to the rendered TimelineItem component |

**Usage in Event Handler:**
```razor
@using Syncfusion.Blazor.Layouts

<SfTimeline ItemRendered="OnItemRendered">
    <TimelineItems>
        <TimelineItem><Content>Item 1</Content></TimelineItem>
        <TimelineItem><Content>Item 2</Content></TimelineItem>
    </TimelineItems>
</SfTimeline>

@code {
    private void OnItemRendered(TimelineRenderedEventArgs args)
    {
        Console.WriteLine($"Item at index {args.Index} rendered");
        Console.WriteLine($"Item reference: {args.Item}");
    }
}
```

---

### Created Event

Fires when the Timeline component rendering is completed.

**Usage:**
```razor
<SfTimeline Created="OnTimelineCreated">
    <TimelineItems>
        <TimelineItem><Content>Item</Content></TimelineItem>
    </TimelineItems>
</SfTimeline>

@code {
    private void OnTimelineCreated()
    {
        Console.WriteLine("Timeline component created");
    }
}
```

---

## Events

| Event | Arguments | Description | Reference |
|-------|-----------|-------------|-----------|
| `Created` | EventCallback<object> | Fires when component rendering completes | [Read More](timeline-events.md#created-event) |
| `ItemRendered` | EventCallback<TimelineRenderedEventArgs> | Fires when each item renders | [Read More](timeline-events.md#itemrendered-event) |

---

## TimelineItemContext

Context object passed to Content, OppositeContent, and Template render fragments.

| Property | Type | Description |
|----------|------|-------------|
| `Item` | TimelineItem | Reference to the current TimelineItem component |
| `ItemIndex` | int | Zero-based index of the current item |

**Usage in Template:**
```razor
<SfTimeline>
    <ChildContent>
        <TimelineItems>
            <TimelineItem>
                <Content>
                    Item @context.ItemIndex: Main Content
                </Content>
            </TimelineItem>
        </TimelineItems>
    </ChildContent>
    <Template>
        <div class="custom-item-@context.ItemIndex">
            <span>Step @(context.ItemIndex + 1)</span>
        </div>
    </Template>
</SfTimeline>
```

---

## Methods

The SfTimeline component inherits lifecycle methods from Blazor's `ComponentBase` class. Component-specific methods are limited as the Timeline is primarily a declarative UI component.

> **Note:** The Timeline component does not expose custom public methods beyond standard Blazor component lifecycle methods (`OnInitializedAsync`, `OnParametersSetAsync`, `OnAfterRenderAsync`). All functionality is controlled through properties and events.

---

## Styling and CSS Classes

### Component-Level Classes

| CSS Class | Purpose |
|-----------|---------|
| `.e-timeline` | Main timeline container |
| `.e-timeline-items` | Container for all items |
| `.e-timeline-item` | Individual timeline item |
| `.e-connector` | Connector line between items |
| `.e-dot` | Dot indicator for each item |
| `.e-outline` | Outline style for dots (add to component CssClass) |

### Custom Styling Example

```razor
<SfTimeline CssClass="custom-timeline">
    <TimelineItems>
        <TimelineItem CssClass="item-status-active">
            <Content>Active Item</Content>
        </TimelineItem>
    </TimelineItems>
</SfTimeline>

<style>
    .custom-timeline .e-connector::after {
        border-color: #2196F3;
        border-width: 2px;
    }
    
    .item-status-active .e-dot {
        background-color: #4CAF50;
        border-color: #4CAF50;
    }
</style>
```

---

## CSS Variables

The Timeline component supports CSS variables for advanced customization:

| Variable | Default | Purpose |
|----------|---------|---------|
| `--dot-size` | 16px | Size of timeline dots |
| `--dot-radius` | 50% | Border radius of dots |
| `--dot-outer-space` | 2px | Space around dot outline |
| `--dot-border` | 2px | Border width of dots |

**Usage:**
```razor
<style>
    .custom-dots .e-dot {
        --dot-size: 24px;
        --dot-radius: 50%;
        --dot-outer-space: 3px;
        --dot-border: 3px;
    }
</style>
```

---

## Complete Working Example

```razor
@using Syncfusion.Blazor.Layouts

<div style="padding: 20px;">
    <h2>Timeline API Demo</h2>
    
    <SfTimeline 
        Alignment="TimelineAlignment.Alternate" 
        Orientation="TimelineOrientation.Vertical"
        Reverse="false"
        CssClass="demo-timeline"
        Created="OnTimelineCreated"
        ItemRendered="OnItemRendered"
        ID="demoTimeline">
        
        <TimelineItems>
            <TimelineItem DotCss="e-icons e-circle-add">
                <Content>Project Started</Content>
                <OppositeContent>Jan 1, 2024</OppositeContent>
            </TimelineItem>
            <TimelineItem DotCss="e-icons e-circle-check">
                <Content>Design Approved</Content>
                <OppositeContent>Jan 15, 2024</OppositeContent>
            </TimelineItem>
            <TimelineItem DotCss="e-icons e-circle-info">
                <Content>Development Phase</Content>
                <OppositeContent>Feb 1, 2024</OppositeContent>
            </TimelineItem>
            <TimelineItem DotCss="e-icons e-circle-check" Disabled="false">
                <Content>Testing Complete</Content>
                <OppositeContent>Mar 1, 2024</OppositeContent>
            </TimelineItem>
            <TimelineItem DotCss="e-icons e-circle-close" Disabled="true">
                <Content>Deployment (Pending)</Content>
                <OppositeContent>Mar 15, 2024</OppositeContent>
            </TimelineItem>
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .demo-timeline .e-connector::after {
        border-color: #2196F3;
        border-width: 1.5px;
    }
    
    .demo-timeline .e-dot {
        background-color: #2196F3;
        border-color: #2196F3;
    }
</style>

@code {
    private void OnTimelineCreated()
    {
        Console.WriteLine("Timeline component initialized");
    }

    private void OnItemRendered(TimelineRenderedEventArgs args)
    {
        Console.WriteLine($"Rendered item at index: {args.Index}");
    }
}
```

---

## Common Use Cases

### 1. Order/Shipping Tracking
```razor
<SfTimeline Alignment="TimelineAlignment.Before">
    <TimelineItems>
        <TimelineItem>
            <Content>Order Placed</Content>
            <OppositeContent>2024-01-10 10:00 AM</OppositeContent>
        </TimelineItem>
        <TimelineItem>
            <Content>Confirmed</Content>
            <OppositeContent>2024-01-10 11:30 AM</OppositeContent>
        </TimelineItem>
        <TimelineItem>
            <Content>Shipped</Content>
            <OppositeContent>2024-01-11 9:00 AM</OppositeContent>
        </TimelineItem>
        <TimelineItem>
            <Content>Delivered</Content>
            <OppositeContent>2024-01-13 3:00 PM</OppositeContent>
        </TimelineItem>
    </TimelineItems>
</SfTimeline>
```

### 2. Project Phases
```razor
<SfTimeline Orientation="TimelineOrientation.Horizontal">
    <TimelineItems>
        <TimelineItem><Content>Planning</Content></TimelineItem>
        <TimelineItem><Content>Design</Content></TimelineItem>
        <TimelineItem><Content>Development</Content></TimelineItem>
        <TimelineItem><Content>Testing</Content></TimelineItem>
        <TimelineItem><Content>Launch</Content></TimelineItem>
    </TimelineItems>
</SfTimeline>
```

### 3. Historical Timeline
```razor
<SfTimeline Alignment="TimelineAlignment.Alternate">
    <TimelineItems>
        <TimelineItem>
            <Content>Event 1</Content>
            <OppositeContent>1995</OppositeContent>
        </TimelineItem>
        <TimelineItem>
            <Content>Event 2</Content>
            <OppositeContent>2005</OppositeContent>
        </TimelineItem>
        <TimelineItem>
            <Content>Event 3</Content>
            <OppositeContent>2015</OppositeContent>
        </TimelineItem>
    </TimelineItems>
</SfTimeline>
```

---

## Performance Considerations

- **Large Timelines:** For timelines with 100+ items, consider virtual scrolling or pagination
- **Custom Templates:** Complex templates may impact rendering performance
- **Event Handlers:** Keep event handlers lightweight to avoid blocking UI updates
- **CSS Classes:** Use CSS variables for styling to minimize DOM manipulation

---

## Browser Support

The Timeline component supports all modern browsers:
- Chrome (Latest)
- Firefox (Latest)
- Safari (Latest)
- Edge (Latest)

---

## See Also

- [Getting Started](getting-started.md)
- [Timeline Alignment](timeline-alignment.md)
- [Timeline Items](timeline-items.md)
- [Timeline Orientation](timeline-orientation.md)
- [Timeline Events](timeline-events.md)
- [Styling & Customization](timeline-styling-customization.md)
- [Templates & Advanced Patterns](timeline-templates.md)
