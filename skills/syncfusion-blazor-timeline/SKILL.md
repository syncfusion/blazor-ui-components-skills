---
name: syncfusion-blazor-timeline
description: Build interactive timeline components that display events and milestones sequentially. Use SfTimeline with alignment controls, custom items, and event handlers. This skill covers timeline configuration and customization for creating visual timelines in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "layouts"
---

# Implementing Blazor Timeline Component

The **Syncfusion Blazor Timeline** component enables you to display a series of events or milestones in a chronological sequence. It's ideal for visualizing project progress, historical timelines, delivery tracking, or process workflows.

## When to Use This Skill

Use the Timeline component when you need to:

- Display events in chronological order (project phases, shipping tracking, workflow steps)
- Show before/after information with opposite content (dates paired with descriptions)
- Create vertical or horizontal timeline layouts
- Customize visual appearance (colors, dots, connectors)
- Render complex content with templates
- Handle component lifecycle events

## Component Overview

The Timeline component renders a series of items sequentially, with each item containing:
- **Content**: Main information or description
- **OppositeContent**: Secondary information displayed opposite to content
- **Dot**: Visual indicator (customizable with icons, images, or text)
- **Connector**: Line connecting items visually

Key capabilities:
- Multiple alignment modes (Before, After, Alternate, AlternateReverse)
- Horizontal and vertical orientations
- Custom item styling and disabled states
- Event callbacks for component lifecycle
- Template-based rendering for advanced layouts
- Reverse order rendering

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Install Syncfusion.Blazor.Layouts and Themes NuGet packages
- Configure imports and register Blazor service
- Add stylesheet and script resources
- Create your first basic timeline
- Run and test the application

### Timeline Alignment
📄 **Read:** [references/timeline-alignment.md](references/timeline-alignment.md)
- Control content display with Alignment property
- **Before**: Content placed before/left, OppositeContent after/right
- **After**: Content placed after/right, OppositeContent before/left
- **Alternate**: Items arranged alternately regardless of orientation
- **AlternateReverse**: Reverse alternate arrangement
- Using OppositeContent with alignments

### Timeline Items
📄 **Read:** [references/timeline-items.md](references/timeline-items.md)
- Add TimelineItem components with Content directive
- String content vs template content approaches
- OppositeContent for dual information display
- Customize dots with DotCss (icons, images, text)
- Disable items with Disabled property
- Apply custom CSS classes with CssClass

### Timeline Orientation
📄 **Read:** [references/timeline-orientation.md](references/timeline-orientation.md)
- **Vertical orientation**: Display items sequentially top-to-bottom (default)
- **Horizontal orientation**: Display items side-by-side
- Combine orientation with alignment for different layouts
- Responsive considerations

### Timeline Events
📄 **Read:** [references/timeline-events.md](references/timeline-events.md)
- **Created**: Fires when component rendering is completed
- **ItemRendered**: Fires after each item is rendered
- Access event arguments and item information
- Common event handling patterns

### Styling & Customization
📄 **Read:** [references/timeline-styling-customization.md](references/timeline-styling-customization.md)
- Connector styling (common and individual)
- Dot color, size, shadow, and variant customization
- CSS variables for dot appearance (--dot-size, --dot-outer-space, --dot-border)
- Dot outline mode (e-outline class)
- Component-level CSS classes

### Template & Advanced Patterns
📄 **Read:** [references/timeline-templates.md](references/timeline-templates.md)
- Template directive for complete item customization
- Access Item and ItemIndex from template context
- Create progress bars, custom connectors, and indicators
- Advanced layout patterns

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete SfTimeline component properties
- TimelineItem properties and configuration
- TimelineAlignment enumeration values
- TimelineOrientation enumeration values
- TimelineRenderedEventArgs event arguments
- CSS classes and CSS variables
- Code examples for all APIs
- Common implementation patterns

## Quick Start Example

```razor
@using Syncfusion.Blazor.Layouts

<div style="height: 350px;">
    <SfTimeline>
        <TimelineItems>
            <TimelineItem>
                <Content>Event 1</Content>
                <OppositeContent>2024-01-01</OppositeContent>
            </TimelineItem>
            <TimelineItem>
                <Content>Event 2</Content>
                <OppositeContent>2024-01-15</OppositeContent>
            </TimelineItem>
            <TimelineItem>
                <Content>Event 3</Content>
                <OppositeContent>2024-02-01</OppositeContent>
            </TimelineItem>
        </TimelineItems>
    </SfTimeline>
</div>
```

## Common Use Cases

### 1. Order/Shipping Timeline
Display order status with timestamps and locations using alignment with opposite content.

### 2. Project Milestone Timeline
Show project phases with start/end dates and descriptions using horizontal orientation.

### 3. Historical Timeline
Present historical events vertically with dates on one side and descriptions on the other using Alternate alignment.

### 4. Progress Tracking
Visualize workflow steps with custom dot colors and disabled states for completed vs. pending items.

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Alignment` | TimelineAlignment enum | Positions content (Before, After, Alternate, AlternateReverse) |
| `Orientation` | TimelineOrientation enum | Layout direction (Vertical, Horizontal) |
| `Reverse` | bool | Reverses item order |
| `CssClass` | string | Custom CSS class for styling |
| `Created` | event | Fires when component rendering completes |
| `ItemRendered` | event | Fires after each item renders |

**Item Properties:**
| Property | Type | Purpose |
|----------|------|---------|
| `Content` | RenderFragment | Primary information |
| `OppositeContent` | RenderFragment | Secondary information |
| `DotCss` | string | CSS class for dot styling (icons, images) |
| `Disabled` | bool | Disables the item |
| `CssClass` | string | Custom CSS class for item styling |

## Key Setup Steps

1. **Install NuGet packages**: Syncfusion.Blazor.Layouts, Syncfusion.Blazor.Themes
2. **Add imports**: `@using Syncfusion.Blazor.Layouts` in _Imports.razor
3. **Register service**: `builder.Services.AddSyncfusionBlazor()` in Program.cs
4. **Include stylesheet**: Add material3.css to index.html
5. **Create timeline**: Use `<SfTimeline>` with `<TimelineItems>` collection

