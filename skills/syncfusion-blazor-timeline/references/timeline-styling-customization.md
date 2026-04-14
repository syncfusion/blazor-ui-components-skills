# Timeline Styling & Customization

## Table of Contents
- [Overview](#overview)
- [Connector Styling](#connector-styling)
- [Dot Styling](#dot-styling)
- [CSS Variables](#css-variables)
- [Dot Outline Mode](#dot-outline-mode)
- [Common Styling Patterns](#common-styling-patterns)

## Overview

The Timeline component provides extensive customization options through CSS. You can style connectors, dots, colors, sizes, shadows, and more using component-level CSS classes or custom classes applied via `CssClass` property.

## Connector Styling

Connectors are the lines connecting Timeline items. They can be customized with colors, widths, and styles.

### Common Connector Styling

Apply styles to all Timeline item connectors using the CSS selector `.e-timeline-item.e-connector::after`:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline CssClass="custom-connector">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .custom-connector .e-timeline-item.e-connector::after {
        border-color: #f7c867;
        border-width: 1.4px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Eat" },
        new TimelineItemModel() { Content = "Code" },
        new TimelineItemModel() { Content = "Repeat" }
    };
}
```

### Individual Connector Styling

Apply unique styles to individual connectors to differentiate specific items:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline CssClass="custom-connector">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem CssClass=@item.CssClass>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .custom-connector .state-initial.e-connector::after {
        border: 1.5px #f8c050 dashed;
    }

    .custom-connector .state-intermediate.e-connector::after {
        border: 1.5px #4d85f5 dashed;
    }

    .custom-connector .state-completed.e-connector::after {
        border: 1.5px #00c300 solid;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string CssClass { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Planned", CssClass = "state-initial" },
        new TimelineItemModel() { Content = "In Progress", CssClass = "state-intermediate" },
        new TimelineItemModel() { Content = "Completed", CssClass = "state-completed" }
    };
}
```

**Connector properties**:
- `border-color`: Color of the connector line
- `border-width`: Thickness of the line
- `border-style`: Style (solid, dashed, dotted)

## Dot Styling

Dots are the visual indicators for each Timeline item. They can be customized extensively.

### Dot Color

Modify dot colors to highlight specific items. Use the `.e-dot` class selector:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container">
    <SfTimeline CssClass="dot-color">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem CssClass=@item.CssClass>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .dot-color .state-completed .e-dot {
        background: #ff9900;
        outline: 1px dashed #ff9900;
        border-color: #ff9900;
    }

    .dot-color .state-progress .e-dot {
        background: #33cc33;
        outline: 1px dashed #33cc33;
        border-color: #33cc33;
    }

    .container {
        height: 250px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string CssClass { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Ordered", CssClass = "state-completed" },
        new TimelineItemModel() { Content = "Shipped", CssClass = "state-progress" },
        new TimelineItemModel() { Content = "Delivered" }
    };
}
```

### Dot Size

Adjust dot size using the `--dot-size` CSS variable:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container">
    <SfTimeline CssClass="dot-size">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem CssClass=@item.CssClass>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .dot-size .e-dot {
        background: #33cc33;
    }

    .dot-size .x-small .e-dot {
        --dot-size: 12px;
    }

    .dot-size .small .e-dot {
        --dot-size: 18px;
    }

    .dot-size .medium .e-dot {
        --dot-size: 24px;
    }

    .dot-size .large .e-dot {
        --dot-size: 30px;
    }

    .container {
        height: 250px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string CssClass { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Extra Small", CssClass = "x-small" },
        new TimelineItemModel() { Content = "Small", CssClass = "small" },
        new TimelineItemModel() { Content = "Medium", CssClass = "medium" },
        new TimelineItemModel() { Content = "Large", CssClass = "large" }
    };
}
```

### Dot Shadow

Add shadow effects to dots using CSS variables:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container">
    <SfTimeline CssClass="dot-shadow">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .dot-shadow .e-dot {
        --dot-outer-space: 3px;
        --dot-border: 3px;
        --dot-size: 20px;
        outline-color: #dee2e6;
        border-color: #fff;
        box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.5), 2px -2px 4px rgba(255, 255, 255, 0.5) inset;
    }

    .container {
        height: 250px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Ordered" },
        new TimelineItemModel() { Content = "Shipped" },
        new TimelineItemModel() { Content = "Delivered" }
    };
}
```

### Dot Variant

Create dot variants with different styles using pseudo-elements and CSS properties:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container">
    <SfTimeline CssClass="dot-variant">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem CssClass=@item.CssClass>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .dot-variant .dot-filled .e-dot::before {
        content: 'A';
        color: #fff;
    }

    .dot-variant .dot-flat .e-dot::before {
        content: 'B';
        color: #fff;
    }

    .dot-variant .dot-outlined .e-dot::before {
        content: 'C';
    }

    .dot-variant .dot-filled .e-dot {
        background: #33cc33;
        --dot-outer-space: 3px;
        outline-color: #81ff05;
        --dot-size: 25px;
    }

    .dot-variant .dot-flat .e-dot {
        background: #33cc33;
        --dot-size: 25px;
        --dot-radius: 10%;
    }

    .dot-variant .dot-outlined .e-dot {
        outline-color: #33cc33;
        --dot-outer-space: 3px;
        background-color: unset;
        --dot-size: 25px;
    }

    .container {
        height: 250px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string CssClass { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Filled", CssClass = "dot-filled" },
        new TimelineItemModel() { Content = "Flat", CssClass = "dot-flat" },
        new TimelineItemModel() { Content = "Outlined", CssClass = "dot-outlined" }
    };
}
```

## CSS Variables

Timeline provides CSS variables for fine-grained dot customization:

| Variable | Purpose | Default |
|----------|---------|---------|
| `--dot-size` | Controls dot dimensions | 16px |
| `--dot-radius` | Controls dot border-radius | 50% |
| `--dot-outer-space` | Space around dot (outline) | 2px |
| `--dot-border` | Border width of dot | 2px |

## Dot Outline Mode

Enable outline mode using the `e-outline` class on the Timeline's `CssClass` property:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container">
    <SfTimeline CssClass="e-outline">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .container {
        height: 250px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Shipped" },
        new TimelineItemModel() { Content = "Departed" },
        new TimelineItemModel() { Content = "Arrived" },
        new TimelineItemModel() { Content = "Out for Delivery" }
    };
}
```

**Effect**: Dots have a distinct outline style with hollow centers, visually emphasizing each item.

## Common Styling Patterns

### Pattern 1: Status-Based Colors
```razor
.status-pending .e-dot { background: #ffc107; }
.status-active .e-dot { background: #2196F3; }
.status-completed .e-dot { background: #4CAF50; }
```

### Pattern 2: Gradient Dots
```razor
.gradient-dot .e-dot {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

### Pattern 3: Icon Dots
```razor
.icon-dot .e-dot::before {
    content: '✓';
    color: white;
    font-weight: bold;
}
```

### Pattern 4: Animated Connectors
```razor
.animated-connector .e-timeline-item.e-connector::after {
    animation: connectorAnimation 1s ease-in-out infinite;
}

@keyframes connectorAnimation {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
}
```

## Key CSS Selectors

| Selector | Purpose |
|----------|---------|
| `.e-dot` | Targets individual dots |
| `.e-timeline-item.e-connector::after` | Targets connector lines |
| `.e-timeline-item` | Targets entire item |
| `.e-outline` | Enables outline dot style |
| `[CssClass]` | Custom item styling via CssClass property |
