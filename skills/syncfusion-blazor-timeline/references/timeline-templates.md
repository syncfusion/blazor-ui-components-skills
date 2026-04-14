# Timeline Templates & Advanced Patterns

## Table of Contents
- [Overview](#overview)
- [Template Directive](#template-directive)
- [Template Context](#template-context)
- [Complete Template Example](#complete-template-example)
- [Advanced Patterns](#advanced-patterns)
- [Progress Bar Pattern](#progress-bar-pattern)

## Overview

The `Template` tag directive allows complete customization of Timeline item appearance. With templates, you can:

- Create custom dot designs
- Build progress indicators
- Add custom connectors
- Implement complex layouts
- Render dynamic content

## Template Directive

The Timeline component's `Template` directive provides access to each item's data and allows you to render a completely custom item structure.

```razor
<SfTimeline>
    <ChildContent>
        <TimelineItems>
            <!-- Define your items -->
        </TimelineItems>
    </ChildContent>
    <Template>
        <!-- Your custom template markup -->
    </Template>
</SfTimeline>
```

## Template Context

The `Template` context provides the following information:

| Property | Type | Purpose |
|----------|------|---------|
| `Item` | TimelineItemData | Current item data |
| `ItemIndex` | int | Zero-based index of the current item |

### Accessing Item Data

```razor
<Template>
    <div>
        Current Index: @context.ItemIndex
        Item Data: @context.Item
    </div>
</Template>
```

### Accessing Item Content

```razor
<Template>
    <div>
        @context.Item.Content(context)
    </div>
</Template>
```

## Complete Template Example

Here's a comprehensive example showing custom template implementation with progress bars and styling:

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 150px; width: 600px;">
    <SfTimeline Orientation=TimelineOrientation.Horizontal CssClass="custom-timeline">
        <ChildContent>
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
        </ChildContent>
        <Template>
            <div class="@("template-container" + " " + "item-" + context.ItemIndex)">
                <div class="content-container">
                    <div class="timeline-content"> @context.Item.Content(context) </div>
                </div>
                <div class="content-connector"></div>
                <div class="progress-line">
                    <span class="indicator"></span>
                </div>
            </div>
        </Template>
    </SfTimeline>
</div>

<style>
    .container * {
        box-sizing: unset;
    }

    .custom-timeline .e-timeline-item.e-item-template {
        align-items: flex-end;
    }

    .custom-timeline .e-timeline-items {
        justify-content: center;
    }

    .template-container .content-connector {
        position: absolute;
        left: 88%;
        width: 3px;
        height: 28px;
    }

    .template-container .content-container {
        padding: 8px;
        border-width: 1px;
        border-style: solid;
    }

    .content-container .timeline-content {
        font-size: 14px;
    }

    /* Color customizations - Progress line, connector line, dot border */
    .item-0 .progress-line,
    .item-0 .content-connector {
        background-color: rgb(233, 93, 93);
    }

    .item-1 .progress-line,
    .item-1 .content-connector {
        background-color: rgba(247, 179, 22, 0.907);
    }

    .item-2 .progress-line,
    .item-2 .content-connector {
        background-color: rgb(60, 184, 60);
    }

    .item-3 .progress-line,
    .item-3 .content-connector {
        background-color: rgb(153, 29, 230);
    }

    .item-0 .progress-line .indicator,
    .item-0 .content-container {
        border-color: rgb(233, 93, 93);
    }

    .item-1 .progress-line .indicator,
    .item-1 .content-container {
        border-color: rgba(247, 179, 22, 0.907);
    }

    .item-2 .progress-line .indicator,
    .item-2 .content-container {
        border-color: rgb(60, 184, 60);
    }

    .item-3 .progress-line .indicator,
    .item-3 .content-container {
        border-color: rgb(153, 29, 230);
    }

    .item-0 .content-container {
        box-shadow: 2px 2px 8px rgb(233, 93, 93);
    }

    .item-1 .content-container {
        box-shadow: 2px 2px 8px rgba(247, 179, 22, 0.907);
    }

    .item-2 .content-container {
        box-shadow: 2px 2px 8px rgb(60, 184, 60);
    }

    .item-3 .content-container {
        box-shadow: 2px 2px 8px rgb(153, 29, 230);
    }

    /* Customizing Dot and progress line */
    .custom-timeline .template-container .indicator {
        position: absolute;
        width: 25px;
        height: 25px;
        border-radius: 50%;
        background-color: #fff;
        border-width: 6px;
        border-style: solid;
        left: 88%;
        transform: translate(-50%, -40%);
        cursor: pointer;
    }

    .progress-line {
        position: absolute;
        height: 10px;
        width: 100%;
        left: 0;
        top: 50%;
    }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Kickoff meeting" },
        new TimelineItemModel() { Content = "Content approved" },
        new TimelineItemModel() { Content = "Design approved" },
        new TimelineItemModel() { Content = "Product delivered" }
    };
}
```

## Advanced Patterns

### Pattern 1: Item Index-Based Styling

Apply different styling based on item index:

```razor
<Template>
    <div class="@("item-step-" + context.ItemIndex)">
        <div class="step-number">@(context.ItemIndex + 1)</div>
        <div class="step-content">@context.Item.Content(context)</div>
    </div>
</Template>

<style>
    .item-step-0 { --step-color: #ff6b6b; }
    .item-step-1 { --step-color: #4ecdc4; }
    .item-step-2 { --step-color: #45b7d1; }
    .item-step-3 { --step-color: #96ceb4; }

    .step-number {
        background: var(--step-color);
        color: white;
        width: 30px;
        height: 30px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
```

### Pattern 2: Conditional Rendering

Render different content based on conditions:

```razor
<Template>
    <div>
        @if (context.ItemIndex == 0)
        {
            <div class="first-item">
                <strong>START: </strong> @context.Item.Content(context)
            </div>
        }
        else if (context.ItemIndex == timelineItems.Count - 1)
        {
            <div class="last-item">
                <strong>END: </strong> @context.Item.Content(context)
            </div>
        }
        else
        {
            <div class="middle-item">
                @context.Item.Content(context)
            </div>
        }
    </div>
</Template>
```

### Pattern 3: Status Indicators

Show status with visual indicators:

```razor
<Template>
    <div class="status-item">
        <div class="status-badge @("badge-" + statusList[context.ItemIndex])">
            @statusList[context.ItemIndex]
        </div>
        <div class="status-content">@context.Item.Content(context)</div>
    </div>
</Template>

<style>
    .badge-completed { background: #4CAF50; color: white; }
    .badge-pending { background: #FFC107; color: black; }
    .badge-failed { background: #F44336; color: white; }
</style>

@code {
    private List<string> statusList = new() { "Completed", "Pending", "Completed", "Pending" };
}
```

## Progress Bar Pattern

Create a progress bar using templates:

```razor
@using Syncfusion.Blazor.Layouts

<div style="padding: 30px;">
    <div>Progress: @Math.Round((completedItems / (double)timelineItems.Count) * 100)%</div>
    <div style="width: 100%; height: 8px; background: #e0e0e0; border-radius: 4px; margin: 10px 0;">
        <div style="width: @($"{(completedItems / (double)timelineItems.Count) * 100}%"); height: 100%; background: #4CAF50; border-radius: 4px; transition: width 0.3s ease;"></div>
    </div>
</div>

<SfTimeline CssClass="progress-timeline">
    <ChildContent>
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content>@item.Content</Content>
                </TimelineItem>
            }
        </TimelineItems>
    </ChildContent>
    <Template>
        <div class="progress-item @("status-" + (context.ItemIndex < completedItems ? "done" : "pending"))">
            <div class="progress-dot">
                @if (context.ItemIndex < completedItems)
                {
                    <span class="checkmark">✓</span>
                }
                else
                {
                    <span class="pending-dot"></span>
                }
            </div>
            <div class="progress-content">@context.Item.Content(context)</div>
        </div>
    </Template>
</SfTimeline>

<style>
    .progress-item.status-done .progress-dot {
        background: #4CAF50;
        color: white;
    }

    .progress-item.status-pending .progress-dot {
        background: #ccc;
        color: #999;
    }

    .progress-dot {
        width: 30px;
        height: 30px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: bold;
    }

    .checkmark { font-size: 18px; }
</style>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private int completedItems = 2;

    private List<TimelineItemModel> timelineItems = new()
    {
        new TimelineItemModel() { Content = "Phase 1" },
        new TimelineItemModel() { Content = "Phase 2" },
        new TimelineItemModel() { Content = "Phase 3" },
        new TimelineItemModel() { Content = "Phase 4" }
    };
}
```

## Template Best Practices

1. **Keep templates simple**: Complex templates can impact performance
2. **Use Index wisely**: Use ItemIndex for conditional styling and logic
3. **Access content correctly**: Use `@context.Item.Content(context)` to render item content
4. **Style appropriately**: Apply CSS for better visual hierarchy
5. **Performance**: Minimize DOM operations in templates
6. **Consistency**: Keep styling consistent across all items unless intentional variation

## Template vs Content Directive

| Feature | Template | Content |
|---------|----------|---------|
| Flexibility | Full control | Basic content |
| Complexity | Can be complex | Simple |
| Performance | More rendering | Less rendering |
| Use case | Custom layouts | Text/basic HTML |
| Styling | Full CSS control | Component defaults |
