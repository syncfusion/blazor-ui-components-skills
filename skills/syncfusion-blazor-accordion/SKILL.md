---
name: syncfusion-blazor-accordion
description: Guide for implementing the Syncfusion Blazor Accordion component for creating collapsible panels with expandable content. Use this skill when users need to build accordions, implement collapsible sections, create expandable panels, organize content in navigation components, implement FAQs, build collapsible menus, add nested accordions, configure expand modes (single or multiple), handle accordion events, customize accordion styling, implement accessible accordions with WCAG compliance, integrate data binding, or work with any vertically collapsible content containers in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Accordion Component

The Blazor Accordion is a vertically collapsible content container that displays one or more panels. Each panel consists of a header and expandable content section. It supports single or multiple panel expansion, nested accordions, animations, and is fully accessible with WCAG compliance.

## When to Use This Skill

Use this skill when you need to:
- Create collapsible content sections in Blazor applications
- Build FAQ sections with expandable answers
- Implement navigation menus with nested items
- Organize large amounts of content in compact, expandable panels
- Create wizard-like interfaces with sequential steps
- Display hierarchical data with nested accordions
- Implement accessible collapsible components with keyboard navigation
- Configure expand/collapse animations and behavior
- Bind dynamic data to accordion items
- Customize accordion appearance and styling

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Blazor.Navigations NuGet package
- Namespace imports and service registration
- Adding theme and script references
- Creating your first accordion with basic items
- Implementing accordions using templates (HeaderTemplate, ContentTemplate)

### Expand Modes and Behavior
📄 **Read:** [references/expand-modes.md](references/expand-modes.md)
- Single expand mode (only one panel open at a time)
- Multiple expand mode (multiple panels can be open)
- Configuring initial expanded state with ExpandedIndices
- Setting individual item expansion with Expanded property
- Controlling default expand behavior

### Content Rendering
📄 **Read:** [references/content-rendering.md](references/content-rendering.md)
- LoadOnDemand property for performance optimization
- Rendering all content at initial load
- On-demand content loading (default behavior)
- Performance considerations for large accordions

### Data Binding and Events
📄 **Read:** [references/data-binding-events.md](references/data-binding-events.md)
- Binding local data with foreach loops
- Using HeaderTemplate and ContentTemplate for dynamic content
- Handling Created and Destroyed lifecycle events
- Responding to Clicked events
- Managing Expanding/Expanded and Collapsing/Collapsed events
- Preventing default expand/collapse behavior with event args

### Customization and Styling
📄 **Read:** [references/customization-styling.md](references/customization-styling.md)
- Customizing accordion border and appearance
- Styling accordion items with CSS
- Customizing header content and expand/collapse icons
- Applying hover and selected item styles
- Using CssClass property on individual items
- CSS class structure and available classes
- Theme integration and responsive design

### Accessibility and Animations
📄 **Read:** [references/accessibility-animations.md](references/accessibility-animations.md)
- WCAG 2.2 and Section 508 compliance
- ARIA attributes and screen reader support
- Keyboard navigation (Arrow keys, Space/Enter, Home/End)
- RTL support and color contrast
- Configuring expand/collapse animations
- Animation effects (SlideDown, SlideUp, FadeIn, FadeOut, ZoomIn, ZoomOut, None)
- Customizing animation duration and easing

### How-To Scenarios
📄 **Read:** [references/how-to-scenarios.md](references/how-to-scenarios.md)
- Adding custom icons to accordion headers
- Creating nested accordions (accordion within accordion)
- Dynamically adding and removing accordion items
- Creating wizard interfaces with sequential validation
- Enabling or disabling specific accordion items
- Integrating other components (TreeView, etc.) inside accordion content
- Preventing expand/collapse for specific conditions
- Showing or hiding accordion items conditionally

## Quick Start

### Basic Accordion

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="ASP.NET" Content="Microsoft ASP.NET is a set of technologies for building Web applications."></AccordionItem>
        <AccordionItem Header="ASP.NET MVC" Content="The Model-View-Controller pattern separates an application into three components."></AccordionItem>
        <AccordionItem Header="JavaScript" Content="JavaScript is an interpreted programming language for web development."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Accordion with Templates

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem>
            <HeaderTemplate>
                <div>Employee Details</div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>
                    <b>Name:</b> John Doe<br />
                    <b>Position:</b> Software Engineer
                </div>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Data-Bound Accordion

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        @foreach (var item in AccordionData)
        {
            <AccordionItem>
                <HeaderTemplate>
                    <div>@item.Title</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<DataItem> AccordionData = new List<DataItem>() {
        new DataItem { Title = "Item 1", Content = "Content 1" },
        new DataItem { Title = "Item 2", Content = "Content 2" }
    };
    
    public class DataItem {
        public string Title { get; set; }
        public string Content { get; set; }
    }
}
```

## Common Patterns

### Single Expand Mode

Use when only one panel should be open at a time (like FAQs):

```cshtml
<SfAccordion ExpandMode="ExpandMode.Single">
    <AccordionItems>
        <AccordionItem Expanded="true" Header="Question 1" Content="Answer 1"></AccordionItem>
        <AccordionItem Header="Question 2" Content="Answer 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Handling Expand Events

Use when you need to perform actions during expand/collapse:

```cshtml
<SfAccordion>
    <AccordionEvents Expanding="OnExpanding" Expanded="OnExpanded"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    public void OnExpanding(ExpandEventArgs args) {
        // Perform validation or load data before expansion
    }
    
    public void OnExpanded(ExpandedEventArgs args) {
        // Handle post-expansion actions
    }
}
```

### Dynamic Item Management

Use when items need to be added or removed at runtime:

```cshtml
<SfButton @onclick="AddItem">Add Item</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem Header="@item.Header" Content="@item.Content"></AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<ItemData> Items = new List<ItemData>();
    
    void AddItem() {
        Items.Add(new ItemData { Header = "New Item", Content = "New Content" });
    }
}
```

### Nested Accordion

Use when you need hierarchical collapsible sections:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Parent Item">
            <ContentTemplate>
                <SfAccordion>
                    <AccordionItems>
                        <AccordionItem Header="Child Item 1" Content="Child Content 1"></AccordionItem>
                        <AccordionItem Header="Child Item 2" Content="Child Content 2"></AccordionItem>
                    </AccordionItems>
                </SfAccordion>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

## Key Properties and Configuration

### Core Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ExpandMode` | ExpandMode | Multiple | Controls whether single or multiple items can be expanded |
| `ExpandedIndices` | int[] | null | Array of indices for initially expanded items |
| `LoadOnDemand` | bool | true | Whether to load content on demand or at initial render |

### AccordionItem Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Header` | string | null | Text content for the accordion header |
| `Content` | string | null | Text content for the accordion panel |
| `HeaderTemplate` | RenderFragment | null | Custom template for the header |
| `ContentTemplate` | RenderFragment | null | Custom template for the content |
| `Expanded` | bool | false | Whether the item is initially expanded |
| `Disabled` | bool | false | Whether the item is disabled |
| `Visible` | bool | true | Whether the item is visible |
| `IconCss` | string | null | CSS class for custom header icon |
| `CssClass` | string | null | Custom CSS class for the item |

### Animation Properties

Configure expand/collapse animations using `AccordionAnimationSettings`:

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <AccordionAnimationExpand Effect="AnimationEffect.SlideDown" Duration="400"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="AnimationEffect.SlideUp" Duration="400"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <!-- Items here -->
    </AccordionItems>
</SfAccordion>
```

Available animation effects: `SlideDown`, `SlideUp`, `FadeIn`, `FadeOut`, `FadeZoomIn`, `FadeZoomOut`, `ZoomIn`, `ZoomOut`, `None`

## Common Use Cases

### FAQ Section

Perfect for FAQs where only one answer should be visible at a time:
- Use `ExpandMode.Single`
- Set first item as `Expanded="true"`
- Style headers to look like questions

### Multi-Step Form/Wizard

Create guided workflows with sequential steps:
- Use `Disabled` property to prevent skipping steps
- Use `Expanded` property to control which step is active
- Handle `Expanding` event to validate before moving to next step

### Navigation Menu

Organize navigation items hierarchically:
- Use nested accordions for sub-menus
- Use `IconCss` for menu icons
- Handle `Clicked` event for navigation actions

### Content Organization

Organize large amounts of content in compact form:
- Use `LoadOnDemand="true"` for performance
- Use `Multiple` expand mode for flexible viewing
- Provide clear, descriptive headers

### Data Display

Display structured data in collapsible sections:
- Use data binding with `foreach`
- Use templates for rich content display
- Handle events for lazy-loading detailed data

## Next Steps

- For installation and setup, read [references/getting-started.md](references/getting-started.md)
- For expand behavior configuration, read [references/expand-modes.md](references/expand-modes.md)
- For event handling and data binding, read [references/data-binding-events.md](references/data-binding-events.md)
- For styling and customization, read [references/customization-styling.md](references/customization-styling.md)
- For practical examples, read [references/how-to-scenarios.md](references/how-to-scenarios.md)
