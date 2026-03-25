# Getting Started with Blazor Tabs Component

## Table of Contents
- [Prerequisites](#prerequisites)
- [NuGet Installation](#nuget-installation)
- [Namespace Setup](#namespace-setup)
- [Service Registration](#service-registration)
- [Theme Stylesheet](#theme-stylesheet)
- [Basic Component Setup](#basic-component-setup)
- [Two-Way Binding](#two-way-binding)
- [Content Templates](#content-templates)

## Prerequisites

Ensure you have the following requirements:
- .NET 6.0 or higher
- Visual Studio, Visual Studio Code, or .NET CLI
- Blazor WebAssembly or Blazor Server application
- System requirements for Syncfusion Blazor components

## NuGet Installation

Install the required NuGet packages for the Tabs component:

### Using Package Manager
```
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

### Using .NET CLI
```
dotnet add package Syncfusion.Blazor.Navigations -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
dotnet restore
```

The `Syncfusion.Blazor.Navigations` package contains the Tab component. The `Syncfusion.Blazor.Themes` package provides theme resources.

## Namespace Setup

Open the **~/_Imports.razor** file and add the required namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

This imports the necessary types and components for using Syncfusion Blazor components.

## Service Registration

Register the Syncfusion Blazor service in your application. Open the **~/Program.cs** file and add the following:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Theme Stylesheet

Add the Syncfusion theme stylesheet to your **~/index.html** file within the `<head>` section:

```html
<head>
    <!-- Other head elements -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

Available themes include:
- bootstrap5.css (default)
- material.css
- fluent.css
- tailwind.css

Choose the theme that matches your application design.

## Basic Component Setup

Create a basic Tabs component in your Blazor page (**~/Pages/Index.razor**):

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Twitter is an online social networking service that enables users to send and read short 140-character messages called tweets. Registered users can read and post tweets, but those who are unregistered can only read them.">
            <ChildContent>
                <TabHeader Text="Twitter"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Facebook is an online social networking service headquartered in Menlo Park, California. Its website was launched on February 4, 2004, by Mark Zuckerberg with his Harvard College roommates.">
            <ChildContent>
                <TabHeader Text="Facebook"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="WhatsApp Messenger is a proprietary cross-platform instant messaging client for smartphones that operates under a subscription business model.">
            <ChildContent>
                <TabHeader Text="WhatsApp"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

This creates a simple three-tab component with headers and content.

### Key Elements
- **`<SfTab>`**: Main component container
- **`<TabItems>`**: Container for all tab items
- **`<TabItem>`**: Individual tab with `Content` attribute for simple text
- **`<ChildContent>`**: Defines the header template
- **`<TabHeader>`**: Tab header with `Text` property

## Two-Way Binding

Bind the selected tab to a component property using `@bind-SelectedItem`. This allows synchronization between the selected tab and other controls:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Inputs

<!-- Numeric input to control selected tab -->
<SfNumericTextBox TValue="int" @bind-Value="@SelectedTab" Min="0" Max="2" Width="200px"></SfNumericTextBox>

<SfTab @bind-SelectedItem="SelectedTab">
    <TabItems>
        <TabItem Content="HTML content here">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# content here">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java content here">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private int SelectedTab = 0;
}
```

When you change the value in the numeric textbox, the selected tab changes. When you click a different tab, the textbox value updates. This bi-directional binding simplifies managing tab state.

## Content Templates

For complex content beyond simple text, use `ContentTemplate` instead of the `Content` attribute:

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>
                    <p>HyperText Markup Language, commonly referred to as HTML, is the standard markup language used to create web pages.</p>
                    <p>Along with CSS and JavaScript, HTML is a cornerstone technology for building web applications.</p>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>
                    <p>Java is a set of computer software and specifications developed by Sun Microsystems.</p>
                    <p>It provides a system for developing application software and deploying it in a cross-platform computing environment.</p>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="JavaScript"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>
                    <p>JavaScript is an interpreted computer programming language.</p>
                    <p>It was originally implemented as part of web browsers for client-side scripting.</p>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

`ContentTemplate` accepts any Blazor markup including:
- HTML elements
- Blazor components
- Conditional rendering
- Loops
- Event handlers

This enables rich, interactive content within tabs.

## Complete Example

Here's a complete working example:

```razor
@page "/"
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations

<h3>Syncfusion Blazor Tabs Example</h3>

<SfTab Width="500px">
    <TabItems>
        <TabItem Content="Twitter enables users to send and read short 140-character messages called tweets.">
            <ChildContent>
                <TabHeader Text="Twitter"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Facebook is a social networking service headquartered in Menlo Park, California.">
            <ChildContent>
                <TabHeader Text="Facebook"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="WhatsApp is a cross-platform instant messaging application for smartphones.">
            <ChildContent>
                <TabHeader Text="WhatsApp"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

Run your Blazor application with `dotnet run` or press F5 in Visual Studio. The Tabs component should render with three tabs and switching between them displays different content.

## Next Steps

- **Tab Selection**: Learn to control which tab is selected programmatically (see tab-selection-and-navigation.md)
- **Tab Management**: Dynamically add and remove tabs (see tab-item-management.md)
- **Styling**: Customize tab appearance and layout (see styling-and-theming.md)
- **Responsive Design**: Handle tabs on different screen sizes (see orientation-and-header-placement.md)
