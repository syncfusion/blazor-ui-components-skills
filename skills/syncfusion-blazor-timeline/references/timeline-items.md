# Timeline Items

## Table of Contents
- [Overview](#overview)
- [Adding Content](#adding-content)
- [String Content](#string-content)
- [Template Content](#template-content)
- [Opposite Content](#opposite-content)
- [Customizing Dots](#customizing-dots)
- [Disabling Items](#disabling-items)
- [CSS Classes](#css-classes)

## Overview

Timeline items are added using the `TimelineItem` tag directive. Each item represents a single event or milestone in your timeline sequence. Items are configured with properties like `Content`, `OppositeContent`, `DotCss`, `Disabled`, and `CssClass`.

## Adding Content

Define item content using the `Content` tag directive as a child to the `TimelineItem` directive.

## String Content

The simplest way to add content is using plain strings:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline>
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

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

**When to use**: Simple text-only timelines, status tracking, step-by-step processes.

## Template Content

Specify complex template content for items by including HTML and components within the `Content` tag directive:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 400px">
    <SfTimeline>
        <ChildContent>
            <TimelineItems>
                @foreach (var item in timelineItems)
                {
                    <TimelineItem>
                        <Content>
                            <div class="content-container">
                                <div class="title">
                                    @item.Title
                                </div>
                                <div class="description">
                                    @item.Description
                                </div>
                                <div class="info">
                                    @item.Info
                                </div>
                            </div>
                        </Content>
                    </TimelineItem>
                }
                <TimelineItem>
                    <Content> Out for Delivery </Content>
                </TimelineItem>
            </TimelineItems>
        </ChildContent>
    </SfTimeline>
</div>

<style>
    .content-container {
        position: relative;
        width: 180px;
        padding: 10px;
        margin-left: 5px;
        box-shadow: rgba(9, 30, 66, 0.25) 0px 4px 8px -2px, rgba(9, 30, 66, 0.08) 0px 0px 0px 1px;
        background-color: ghostwhite;
    }

    .content-container::before {
        content: '';
        position: absolute;
        left: -8px;
        transform: translateY(-50%);
        width: 0;
        height: 0;
        border-top: 5px solid transparent;
        border-bottom: 5px solid transparent;
        border-right: 8px solid silver;
    }

    .content-container .title {
        font-size: 16px;
        font-weight: bold;
    }

    .content-container .description {
        color: #999999;
        font-size: 12px;
        margin-top: 5px;
    }

    .content-container .info {
        color: #999999;
        font-size: 10px;
        margin-top: 5px;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Title { get; set; }
        public string Description { get; set; }
        public string Info { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Title = "Shipped", Description = "Package details received", Info = "- Awaiting dispatch" },
        new TimelineItemModel() { Title = "Departed", Description = "In-transit", Info = "(International warehouse)" },
        new TimelineItemModel() { Title = "Arrived", Description = "Package arrived at nearest hub", Info = "(New york - US)" },
    };
}
```

**When to use**: Complex layouts, formatted information, multiple data fields per item, styled content boxes.

## Opposite Content

Additional information can be added to each Timeline item using the `OppositeContent` tag directive. This content is positioned opposite to the item's main Content, and works with any alignment mode.

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline>
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content>
                        @item.Content
                    </Content>
                    <OppositeContent>
                        @item.OppositeContent
                    </OppositeContent>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string OppositeContent { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Breakfast", OppositeContent = "8:00 AM" },
        new TimelineItemModel() { Content = "Lunch", OppositeContent = "1:00 PM" },
        new TimelineItemModel() { Content = "Dinner", OppositeContent = "8:00 PM" },
    };
}
```

**When to use**: Pairing events with timestamps, locations with descriptions, primary and secondary information.

## Customizing Dots

The `DotCss` property allows defining a CSS class to set icons, background colors, or images on timeline dots.

### Adding Icons

Define a CSS class with icon styling using `DotCss`:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline>
        <TimelineItems>
            <TimelineItem DotCss="e-icons e-check">
                <Content>Completed</Content>
            </TimelineItem>
            <TimelineItem DotCss="e-icons e-close">
                <Content>Cancelled</Content>
            </TimelineItem>
        </TimelineItems>
    </SfTimeline>
</div>
```

**Available icon classes**: Use `e-icons` class with icon names like `e-check`, `e-close`, `e-download`, `e-upload`, etc.

### Adding Images

Include images for Timeline items using `DotCss` by setting CSS `background-image`:

```razor
<style>
    .e-dot.custom-image {
        background-image: url('./dot-image.png');
        background-size: cover;
    }
</style>

<TimelineItem DotCss="custom-image">
    <Content>Image Dot</Content>
</TimelineItem>
```

### Adding Text

Display text on dots using `DotCss` with CSS `content` property:

```razor
<style>
    .e-dot.custom-text::before {
        content: 'A';
        font-weight: bold;
        color: white;
    }
</style>

<TimelineItem DotCss="custom-text">
    <Content>Text Dot</Content>
</TimelineItem>
```

### Complete Example

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline>
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem DotCss=@item.DotCss>
                    <Content>@item.Content</Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .e-dot.custom-image { background-image: url('./dot-image.png'); }
    .e-dot.custom-text::before { content: 'A'; }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string DotCss { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new()
    {
        new() { Content = "Default" },
        new() { Content = "Check", DotCss = "e-icons e-check" },
        new() { Content = "Image", DotCss = "custom-image" },
        new() { Content = "Text", DotCss = "custom-text" }
    };
}
```

## Disabling Items

Use the `Disabled` property to disable an item. When set to `true`, the item appears disabled with reduced opacity. By default, its value is `false`.

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline>
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem Disabled=@item.Disabled>
                    <Content>
                        @item.Content
                    </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public bool Disabled { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Eat" },
        new TimelineItemModel() { Content = "Code" },
        new TimelineItemModel() { Content = "Repeat", Disabled = true },
    };
}
```

**When to use**: Show pending/incomplete steps, disable future milestones, indicate skipped items.

## CSS Classes

Customize the appearance of a Timeline item by specifying a custom CSS class using the `CssClass` property. This applies styling to the entire item element.

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 250px">
    <SfTimeline>
        <TimelineItems>
            <TimelineItem CssClass="item-pending">
                <Content>Pending Task</Content>
            </TimelineItem>
            <TimelineItem CssClass="item-completed">
                <Content>Completed Task</Content>
            </TimelineItem>
            <TimelineItem CssClass="item-important">
                <Content>Important Task</Content>
            </TimelineItem>
        </TimelineItems>
    </SfTimeline>
</div>

<style>
    .item-pending {
        opacity: 0.6;
    }

    .item-completed {
        opacity: 1;
    }

    .item-important {
        font-weight: bold;
    }
</style>
```

**When to use**: Apply different styling based on status, importance, or category.

## Key Properties Reference

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Content` | RenderFragment | - | Primary content of the item |
| `OppositeContent` | RenderFragment | null | Secondary content opposite to main content |
| `DotCss` | string | "" | CSS class for dot styling |
| `Disabled` | bool | false | Disables the item if true |
| `CssClass` | string | "" | Custom CSS class for item styling |
