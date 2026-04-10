# Getting Started with Blazor Accordion Component

This guide covers the complete setup process for implementing the Syncfusion Blazor Accordion component in your Blazor WebAssembly application.

## Table of Contents
- [Installing Syncfusion Blazor Packages](#installing-syncfusion-blazor-packages)
- [Importing Namespaces](#importing-namespaces)
- [Registering Syncfusion Blazor Service](#registering-syncfusion-blazor-service)
- [Adding Stylesheet and Script References](#adding-stylesheet-and-script-references)
- [Creating Your First Accordion](#creating-your-first-accordion)
- [Using Templates for Custom Content](#using-templates-for-custom-content)
- [Initial Expansion State](#initial-expansion-state)
- [Complete Working Example](#complete-working-example)
- [Next Steps](#next-steps)
- [Troubleshooting](#troubleshooting)

## Installing Syncfusion Blazor Packages

The Accordion component requires two NuGet packages:

1. **Syncfusion.Blazor.Navigations** - Contains the Accordion component
2. **Syncfusion.Blazor.Themes** - Provides theme stylesheets

### Installation Methods

**Using NuGet Package Manager (Visual Studio):**
- Tools → NuGet Package Manager → Manage NuGet Packages for Solution
- Search for and install both packages

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
```

All Syncfusion Blazor packages are available on [nuget.org](https://www.nuget.org/packages?q=syncfusion.blazor).

## Importing Namespaces

After installing the packages, import the required namespaces in your **~/_Imports.razor** file:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

This makes the Accordion component and related classes available throughout your application.

## Registering Syncfusion Blazor Service

Register the Syncfusion Blazor Service in your **Program.cs** file:

```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
```

This registration enables Syncfusion components to work properly in your application.

## Adding Stylesheet and Script References

Add theme stylesheet and script references to your **~/index.html** file (Blazor WebAssembly):

```html
<head>
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
</head>

<body>
    <!-- Syncfusion Blazor Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

### Available Themes

You can use any of these theme files:
- `bootstrap5.css`
- `material.css`
- `fabric.css`
- `fluent.css`
- `fluent2.css`
- `tailwind.css`
- `material3.css`

**Note:** Theme stylesheets can be referenced via:
- Static Web Assets (recommended for production)
- CDN (Content Delivery Network)
- CRG (Custom Resource Generator) for optimized loading

## Creating Your First Accordion

Add the Accordion component to any Blazor page (e.g., **~/Pages/Index.razor**):

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="ASP.NET" Content="Microsoft ASP.NET is a set of technologies in the Microsoft .NET Framework for building Web applications and XML Web services."></AccordionItem>
        <AccordionItem Header="ASP.NET MVC" Content="The Model-View-Controller (MVC) architectural pattern separates an application into three main components: the model, the view, and the controller."></AccordionItem>
        <AccordionItem Header="JavaScript" Content="JavaScript (JS) is an interpreted computer programming language originally implemented as part of web browsers."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to run the application. The Accordion component will render with three collapsible items.

## Using Templates for Custom Content

For more control over header and content presentation, use templates:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem>
            <HeaderTemplate>
                <div>Employee Information</div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <div><b>Name:</b> Margaret Peacock</div>
                    <div><b>Position:</b> Sales Coordinator</div>
                    <div><b>Department:</b> Seattle, WA</div>
                </div>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem>
            <HeaderTemplate>
                <div>Contact Details</div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <div><b>Email:</b> margaret@example.com</div>
                    <div><b>Phone:</b> (206) 555-0100</div>
                </div>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Template Benefits

**HeaderTemplate:**
- Add custom HTML structure to headers
- Include icons, badges, or buttons
- Apply custom styling and layout

**ContentTemplate:**
- Render complex content with rich formatting
- Include images, tables, or other components
- Implement dynamic content with data binding

## Initial Expansion State

Control which items are expanded when the accordion loads:

### Using Expanded Property

Set `Expanded="true"` on individual items:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Expanded="true" Header="Item 1" Content="This item is expanded by default"></AccordionItem>
        <AccordionItem Header="Item 2" Content="This item is collapsed by default"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Using ExpandedIndices Property

Specify multiple items to expand using their zero-based indices:

```cshtml
<SfAccordion @bind-ExpandedIndices="ExpandItems">
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
        <AccordionItem Header="Item 3" Content="Content 3"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    public int[] ExpandItems = new int[] { 0, 2 }; // Expand first and third items
}
```

## Complete Working Example

Here's a complete example combining all concepts:

```cshtml
@page "/accordion-demo"
@using Syncfusion.Blazor.Navigations

<h3>Accordion Component Demo</h3>

<SfAccordion>
    <AccordionItems>
        <AccordionItem Expanded="true">
            <HeaderTemplate>
                <div><strong>Getting Started</strong></div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 15px;">
                    <p>Welcome to the Syncfusion Blazor Accordion component.</p>
                    <ul>
                        <li>Easy to integrate</li>
                        <li>Highly customizable</li>
                        <li>Accessible and responsive</li>
                    </ul>
                </div>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem>
            <HeaderTemplate>
                <div><strong>Features</strong></div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 15px;">
                    <p>Key features of the component:</p>
                    <ul>
                        <li>Single and multiple expand modes</li>
                        <li>Customizable animations</li>
                        <li>Event handling support</li>
                        <li>Template-based rendering</li>
                    </ul>
                </div>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem>
            <HeaderTemplate>
                <div><strong>Documentation</strong></div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 15px;">
                    <p>Explore comprehensive documentation and examples to learn more about implementing the Accordion component in your applications.</p>
                </div>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

## Next Steps

Now that you've successfully created your first Accordion component, explore these topics:

- **Expand Modes:** Learn about single vs. multiple expand modes in [expand-modes.md](expand-modes.md)
- **Events:** Handle user interactions in [data-binding-events.md](data-binding-events.md)
- **Customization:** Apply custom styles in [customization-styling.md](customization-styling.md)
- **Accessibility:** Implement keyboard navigation and ARIA support in [accessibility-animations.md](accessibility-animations.md)

## Troubleshooting

**Component not rendering:**
- Verify NuGet packages are installed
- Check namespace imports in _Imports.razor
- Ensure Syncfusion service is registered in Program.cs
- Confirm stylesheet and script references are added

**Styling issues:**
- Verify theme CSS is loaded correctly
- Check browser console for CSS loading errors
- Ensure the correct theme file is referenced

**Build errors:**
- Clean and rebuild the solution
- Verify package versions are compatible
- Check for namespace conflicts
