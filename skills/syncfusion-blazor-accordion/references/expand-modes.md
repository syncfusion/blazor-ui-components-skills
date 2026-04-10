# Expand Modes in Blazor Accordion Component

## Table of Contents
- [Overview](#overview)
- [Single Expand Mode](#single-expand-mode)
- [Multiple Expand Mode](#multiple-expand-mode)
- [Configuring Initial Expansion](#configuring-initial-expansion)
- [Controlling Expansion Programmatically](#controlling-expansion-programmatically)
- [Best Practices](#best-practices)

## Overview

The Blazor Accordion component supports two expand modes that control how many panels can be open simultaneously:

- **Single Mode:** Only one panel can be expanded at a time
- **Multiple Mode:** Multiple panels can be expanded simultaneously (default)

The expand mode is controlled by the `ExpandMode` property on the `SfAccordion` component.

## Single Expand Mode

In Single mode, expanding a new panel automatically collapses the previously expanded panel. This ensures only one panel is visible at any time.

### When to Use Single Mode

- **FAQ Sections:** Users typically read one answer at a time
- **Wizard Interfaces:** Sequential steps where only the current step should be visible
- **Form Sections:** Breaking forms into sections where focus should be on one section
- **Mobile Views:** Limited screen space requires focused content display

### Implementation

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion ExpandMode="ExpandMode.Single">
    <AccordionItems>
        <AccordionItem Expanded="true" Header="ASP.NET" Content="Microsoft ASP.NET is a set of technologies in the Microsoft .NET Framework for building Web applications and XML Web services."></AccordionItem>
        <AccordionItem Header="ASP.NET MVC" Content="The Model-View-Controller (MVC) architectural pattern separates an application into three main components: the model, the view, and the controller."></AccordionItem>
        <AccordionItem Header="JavaScript" Content="JavaScript (JS) is an interpreted computer programming language originally implemented as part of web browsers."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Key Points:**
- Set `ExpandMode="ExpandMode.Single"`
- Optionally set `Expanded="true"` on one item to have it open initially
- Clicking any collapsed item will collapse the currently expanded item

### Behavior

1. User clicks on Item 2 → Item 1 collapses, Item 2 expands
2. User clicks on Item 3 → Item 2 collapses, Item 3 expands
3. User clicks on the expanded item → Item collapses (all items collapsed)

## Multiple Expand Mode

Multiple mode is the default behavior. It allows users to expand as many panels as needed, providing maximum flexibility.

### When to Use Multiple Mode

- **Documentation/Help Pages:** Users may want to reference multiple sections
- **Settings Panels:** Users may need to view multiple configuration sections
- **Content Comparison:** Comparing information across different panels
- **General Navigation:** When there's no specific workflow or sequence

### Implementation

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion ExpandMode="ExpandMode.Multiple">
    <AccordionItems>
        <AccordionItem Header="ASP.NET" Content="Microsoft ASP.NET is a set of technologies in the Microsoft .NET Framework for building Web applications and XML Web services."></AccordionItem>
        <AccordionItem Header="ASP.NET MVC" Content="The Model-View-Controller (MVC) architectural pattern separates an application into three main components: the model, the view, and the controller."></AccordionItem>
        <AccordionItem Header="JavaScript" Content="JavaScript (JS) is an interpreted computer programming language originally implemented as part of web browsers."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Key Points:**
- `ExpandMode="ExpandMode.Multiple"` is the default (can be omitted)
- Multiple items can be expanded simultaneously
- Each item can be expanded/collapsed independently
- Clicking an expanded item toggles it closed

### Behavior

- All items can be expanded at the same time
- Expanding one item does not affect others
- Each item acts as an independent toggle

## Configuring Initial Expansion

Control which panels are expanded when the accordion first renders.

### Using the Expanded Property

Set `Expanded="true"` on individual `AccordionItem` elements:

```cshtml
<SfAccordion ExpandMode="ExpandMode.Multiple">
    <AccordionItems>
        <AccordionItem Expanded="true" Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
        <AccordionItem Expanded="true" Header="Item 3" Content="Content 3"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

In this example, Item 1 and Item 3 are expanded when the page loads.

**Note:** In Single mode, if multiple items have `Expanded="true"`, only the last one will be expanded.

### Using the ExpandedIndices Property

Use `ExpandedIndices` to specify which items should be expanded using their zero-based indices:

```cshtml
<SfAccordion ExpandMode="ExpandMode.Multiple" @bind-ExpandedIndices="ExpandItems">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
        <AccordionItem Header="Item 3" Content="Content 3"></AccordionItem>
        <AccordionItem Header="Item 4" Content="Content 4"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    public int[] ExpandItems = new int[] { 0, 2 }; // Expand Item 1 (index 0) and Item 3 (index 2)
}
```

**Benefits of ExpandedIndices:**
- Centralized control over expanded items
- Easy to change programmatically
- Works well with data-bound scenarios
- Supports two-way binding with `@bind-ExpandedIndices`

### Dynamic Expansion with Data Binding

Combine `ExpandedIndices` with data binding for dynamic content:

```cshtml
<SfAccordion @bind-ExpandedIndices="ExpandItems">
    <AccordionItems>
        @foreach (var item in AccordionData)
        {
            <AccordionItem>
                <HeaderTemplate>
                    <div>@item.EmployeeName</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>
                        <div><b>Employee ID:</b> @item.EmployeeId</div>
                        <div><b>Designation:</b> @item.Designation</div>
                    </div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    public int[] ExpandItems = new int[] { 1, 2 }; // Expand second and third employees
    
    List<AccordionData> AccordionData = new List<AccordionData>() {
        new AccordionData { EmployeeId = 1, EmployeeName = "Laura Callahan", Designation = "Product Manager" },
        new AccordionData { EmployeeId = 3, EmployeeName = "Andrew Fuller", Designation = "Team Lead" },
        new AccordionData { EmployeeId = 4, EmployeeName = "Anne Dodsworth", Designation = "Developer" },
        new AccordionData { EmployeeId = 5, EmployeeName = "Nancy Davolio", Designation = "Product Manager" }
    };

    public class AccordionData {
        public string EmployeeName { get; set; }
        public int EmployeeId { get; set; }
        public string Designation { get; set; }
    }
}
```

## Controlling Expansion Programmatically

Change the expanded state of items in response to user actions or application logic.

### Updating ExpandedIndices

Modify the `ExpandedIndices` array to change which items are expanded:

```cshtml
<SfButton @onclick="ExpandFirst">Expand First Item</SfButton>
<SfButton @onclick="ExpandAll">Expand All</SfButton>
<SfButton @onclick="CollapseAll">Collapse All</SfButton>

<SfAccordion @bind-ExpandedIndices="ExpandItems">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
        <AccordionItem Header="Item 3" Content="Content 3"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    public int[] ExpandItems = new int[] { };
    
    void ExpandFirst() {
        ExpandItems = new int[] { 0 };
    }
    
    void ExpandAll() {
        ExpandItems = new int[] { 0, 1, 2 };
    }
    
    void CollapseAll() {
        ExpandItems = new int[] { };
    }
}
```

### Toggling Individual Items

Toggle a specific item's expansion state:

```cshtml
<SfButton @onclick="ToggleSecondItem">Toggle Second Item</SfButton>

<SfAccordion @bind-ExpandedIndices="ExpandItems">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
        <AccordionItem Header="Item 3" Content="Content 3"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    public int[] ExpandItems = new int[] { };
    
    void ToggleSecondItem() {
        var list = ExpandItems.ToList();
        if (list.Contains(1)) {
            list.Remove(1);
        } else {
            list.Add(1);
        }
        ExpandItems = list.ToArray();
    }
}
```

## Best Practices

### Choosing the Right Mode

**Use Single Mode when:**
- Content is mutually exclusive (only one section is relevant at a time)
- Screen space is limited (mobile devices)
- You want to guide users through sequential steps
- Simplifying the UI by reducing visual complexity

**Use Multiple Mode when:**
- Users may need to reference multiple sections simultaneously
- Content is independent and can be viewed in any order
- Maximizing flexibility is more important than guided flow
- Desktop environments with ample screen space

### Initial Expansion Guidelines

- **Don't expand all items by default** - defeats the purpose of an accordion
- **Expand the most important or frequently accessed item** - helps users get started
- **In Single mode, expand one item** - provides immediate context
- **In Multiple mode, expand 0-2 items** - maintains the collapsed benefit
- **Consider user context** - expand items based on user role or previous selections

### Performance Considerations

- Use `LoadOnDemand="true"` (default) with expand modes for better performance
- In Single mode, content is loaded when needed (only for the expanded item)
- In Multiple mode with many initially expanded items, consider load impact
- See [content-rendering.md](content-rendering.md) for more details

### Accessibility

- Both modes support full keyboard navigation
- Screen readers announce expansion state correctly
- Use descriptive headers to help users understand content
- See [accessibility-animations.md](accessibility-animations.md) for complete accessibility guide

## Common Scenarios

### FAQ with Single Mode

```cshtml
<SfAccordion ExpandMode="ExpandMode.Single">
    <AccordionItems>
        <AccordionItem Expanded="true" Header="What is Blazor?" Content="Blazor is a framework for building interactive web UIs using C# instead of JavaScript."></AccordionItem>
        <AccordionItem Header="How do I install Syncfusion components?" Content="Install the NuGet package and register the Syncfusion service in Program.cs."></AccordionItem>
        <AccordionItem Header="Is the Accordion component accessible?" Content="Yes, it supports WCAG 2.2 compliance with keyboard navigation and screen reader support."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Settings Panel with Multiple Mode

```cshtml
<SfAccordion ExpandMode="ExpandMode.Multiple" @bind-ExpandedIndices="@(new int[]{0})">
    <AccordionItems>
        <AccordionItem Header="General Settings" Content="Configure general application settings here."></AccordionItem>
        <AccordionItem Header="Security Settings" Content="Manage security and privacy settings."></AccordionItem>
        <AccordionItem Header="Notification Settings" Content="Control notification preferences."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Wizard with Single Mode and Controlled Expansion

```cshtml
<SfAccordion ExpandMode="ExpandMode.Single" @bind-ExpandedIndices="CurrentStep">
    <AccordionItems>
        <AccordionItem Header="Step 1: Personal Information">
            <ContentTemplate>
                <!-- Form fields for personal info -->
                <SfButton @onclick="() => CurrentStep = new int[]{1}">Next</SfButton>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem Header="Step 2: Contact Details">
            <ContentTemplate>
                <!-- Form fields for contact info -->
                <SfButton @onclick="() => CurrentStep = new int[]{2}">Next</SfButton>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem Header="Step 3: Review and Submit">
            <ContentTemplate>
                <!-- Review and submit -->
                <SfButton @onclick="Submit">Submit</SfButton>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    public int[] CurrentStep = new int[] { 0 };
    
    void Submit() {
        // Handle submission
    }
}
```

## Related Topics

- [Getting Started](getting-started.md) - Initial setup and basic implementation
- [Content Rendering](content-rendering.md) - LoadOnDemand and performance optimization
- [Data Binding and Events](data-binding-events.md) - Handling expansion events
- [How-To Scenarios](how-to-scenarios.md) - Practical examples including wizards
