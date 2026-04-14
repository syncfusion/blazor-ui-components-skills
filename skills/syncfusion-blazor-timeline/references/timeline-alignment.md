# Timeline Alignment

## Table of Contents
- [Overview](#overview)
- [Before Alignment](#before-alignment)
- [After Alignment](#after-alignment)
- [Alternate Alignment](#alternate-alignment)
- [Alternate Reverse Alignment](#alternate-reverse-alignment)
- [Using Opposite Content](#using-opposite-content)

## Overview

The `Alignment` property controls how timeline content is displayed relative to the timeline axis. You can position content and opposite content in different arrangements to suit your layout needs.

The Timeline component supports four alignment options:
- **Before**: Content before/left, OppositeContent after/right
- **After**: Content after/right, OppositeContent before/left
- **Alternate**: Content alternates sides
- **AlternateReverse**: Content alternates in reverse order

## Before Alignment

In `Before` alignment:
- For **horizontal** orientation: Item content is placed at the top, OppositeContent at the bottom
- For **vertical** orientation: Content is placed to the left, OppositeContent to the right

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline Alignment="TimelineAlignment.Before">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                    <OppositeContent> @item.OppositeContent </OppositeContent>
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
        new TimelineItemModel() { Content = "ReactJs", OppositeContent = "Owned by Facebook" },
        new TimelineItemModel() { Content = "Angular", OppositeContent = "Owned by Google" },
        new TimelineItemModel() { Content = "VueJs", OppositeContent = "Owned by Evan you" },
        new TimelineItemModel() { Content = "Svelte", OppositeContent = "Owned by Rich Harris" }
    };
}
```

**Use case**: Ideal for displaying frameworks with their creators or events with their locations on opposite sides.

## After Alignment

In `After` alignment:
- For **horizontal** orientation: Item content is placed at the bottom, OppositeContent at the top
- For **vertical** orientation: Content is placed to the right, OppositeContent to the left

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline Alignment="TimelineAlignment.After">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                    <OppositeContent> @item.OppositeContent </OppositeContent>
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
        new TimelineItemModel() { Content = "ReactJs", OppositeContent = "Owned by Facebook" },
        new TimelineItemModel() { Content = "Angular", OppositeContent = "Owned by Google" },
        new TimelineItemModel() { Content = "VueJs", OppositeContent = "Owned by Evan you" },
        new TimelineItemModel() { Content = "Svelte", OppositeContent = "Owned by Rich Harris" }
    };
}
```

**Use case**: Reverse of Before alignment; useful for alternative visual flows.

## Alternate Alignment

In `Alternate` alignment, item content is arranged alternatively on both sides, regardless of the Timeline orientation. This creates a balanced, zigzag pattern.

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline Alignment="TimelineAlignment.Alternate">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                    <OppositeContent> @item.OppositeContent </OppositeContent>
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
        new TimelineItemModel() { Content = "ReactJs", OppositeContent = "Owned by Facebook" },
        new TimelineItemModel() { Content = "Angular", OppositeContent = "Owned by Google" },
        new TimelineItemModel() { Content = "VueJs", OppositeContent = "Owned by Evan you" },
        new TimelineItemModel() { Content = "Svelte", OppositeContent = "Owned by Rich Harris" }
    };
}
```

**Use case**: Perfect for historical timelines, employee career progressions, or project milestones where you want balanced, alternating content display.

## Alternate Reverse Alignment

In `AlternateReverse` alignment, item content is arranged in reverse alternate, regardless of the Timeline orientation. The items that would appear on the left in Alternate now appear on the right, and vice versa.

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline Alignment="TimelineAlignment.AlternateReverse">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                    <OppositeContent> @item.OppositeContent </OppositeContent>
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
        new TimelineItemModel() { Content = "ReactJs", OppositeContent = "Owned by Facebook" },
        new TimelineItemModel() { Content = "Angular", OppositeContent = "Owned by Google" },
        new TimelineItemModel() { Content = "VueJs", OppositeContent = "Owned by Evan you" },
        new TimelineItemModel() { Content = "Svelte", OppositeContent = "Owned by Rich Harris" }
    };
}
```

**Use case**: Alternative reverse alternation; useful when you want the opposite direction from standard Alternate.

## Using Opposite Content

The `OppositeContent` is always displayed parallel to the main `Content` when configured within the `TimelineItem` directive. It allows you to display complementary information.

```razor
<TimelineItem>
    <Content>
        Main Information or Event
    </Content>
    <OppositeContent>
        Additional Information (Date, Location, Status, etc.)
    </OppositeContent>
</TimelineItem>
```

**OppositeContent Examples**:
- **Shipping Timeline**: Content = "Shipped", OppositeContent = "2024-01-15 10:00 AM"
- **Project Timeline**: Content = "Design Phase", OppositeContent = "Week 1-2"
- **Career Timeline**: Content = "Software Engineer", OppositeContent = "Jan 2023 - Present"
- **Historical Timeline**: Content = "Event Name", OppositeContent = "Date & Location"

### Example with Different Alignments

```razor
@using Syncfusion.Blazor.Layouts

<div style="margin: 20px; padding: 20px; border: 1px solid #ddd;">
    <h3>Before Alignment</h3>
    <div style="height: 200px;">
        <SfTimeline Alignment="TimelineAlignment.Before">
            <TimelineItems>
                <TimelineItem>
                    <Content>Breakfast</Content>
                    <OppositeContent>8:00 AM</OppositeContent>
                </TimelineItem>
                <TimelineItem>
                    <Content>Lunch</Content>
                    <OppositeContent>1:00 PM</OppositeContent>
                </TimelineItem>
                <TimelineItem>
                    <Content>Dinner</Content>
                    <OppositeContent>8:00 PM</OppositeContent>
                </TimelineItem>
            </TimelineItems>
        </SfTimeline>
    </div>
</div>

<div style="margin: 20px; padding: 20px; border: 1px solid #ddd;">
    <h3>Alternate Alignment</h3>
    <div style="height: 200px;">
        <SfTimeline Alignment="TimelineAlignment.Alternate">
            <TimelineItems>
                <TimelineItem>
                    <Content>Breakfast</Content>
                    <OppositeContent>8:00 AM</OppositeContent>
                </TimelineItem>
                <TimelineItem>
                    <Content>Lunch</Content>
                    <OppositeContent>1:00 PM</OppositeContent>
                </TimelineItem>
                <TimelineItem>
                    <Content>Dinner</Content>
                    <OppositeContent>8:00 PM</OppositeContent>
                </TimelineItem>
            </TimelineItems>
        </SfTimeline>
    </div>
</div>
```

## Choosing the Right Alignment

| Alignment | Visual Pattern | Best Use Case |
|-----------|---|---|
| **Before** | Content on one side consistently | Status updates, notifications in order |
| **After** | Content on opposite side from Before | Alternative flow from Before |
| **Alternate** | Content alternates sides | Balanced timelines, project phases, historical events |
| **AlternateReverse** | Content alternates in reverse | Alternative to Alternate |

## Key Properties Used

- `Alignment="TimelineAlignment.Before|After|Alternate|AlternateReverse"` - Sets the alignment mode
- `<Content>` - Primary content tag directive
- `<OppositeContent>` - Secondary content tag directive
