---
name: syncfusion-blazor-card
description: Build responsive card layouts with Syncfusion Blazor Card component. Use this when creating reusable card containers with headers, images, content, action buttons, and custom styling. This skill covers setup, structure, layouts, interactivity, and advanced customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Components"
---

# Implementing Syncfusion Blazor Card Component

The Syncfusion Blazor Card component is a lightweight, flexible container for organizing content with headers, images, text, action buttons, and custom styling. Cards are ideal for displaying grouped information in a visually appealing format.

## When to Use This Skill

Use this skill when you need to:
- Create reusable card containers for displaying organized content
- Display content with headers, subtitles, and images
- Add action buttons and footers to cards
- Arrange cards in horizontal or vertical layouts
- Apply custom styling and theming to cards
- Build responsive card-based UI layouts
- Organize related content into discrete, visually distinct containers
- Implement dividers to separate card sections

## Component Overview

The **SfCard** component provides:
- **Flexible Structure**: Compose cards with headers, images, content, and footers
- **Multiple Layouts**: Vertical (default) and horizontal orientations
- **Rich Content Support**: Text, images, buttons, and HTML elements
- **Styling**: Built-in CSS classes and custom styling options
- **Component-Based Architecture**: Use Blazor components like `CardHeader`, `CardContent`, `CardImage`, `CardFooter`

## Documentation and Navigation Guide

### Getting Started with Card Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet packages
- Project setup (WASM, Web App, Server App variations)
- Import namespaces and register services
- Basic card component creation
- Minimal working example with first render

### Building Card Structure
📄 **Read:** [references/building-card-structure.md](references/building-card-structure.md)
- Header elements (Title, SubTitle, ImageUrl)
- Content sections and text placement
- CardImage component for visual elements
- Dividers to separate sections
- Multiple content sections in single card
- Complex card layouts with nested elements

### Card Layouts and Orientation
📄 **Read:** [references/card-layouts-and-orientation.md](references/card-layouts-and-orientation.md)
- Default vertical card layout
- Horizontal orientation (CardOrientation.Horizontal)
- CardStacked component for vertical sections
- Combining multiple orientations
- Responsive layouts and alignment

### Action Buttons and Footers
📄 **Read:** [references/action-buttons-and-footers.md](references/action-buttons-and-footers.md)
- CardFooter and CardFooterContent components
- Adding buttons to card footers
- Vertical button orientation
- Button styling and integration
- Click handlers and interactivity

### Styling and Customization
📄 **Read:** [references/styling-and-customization.md](references/styling-and-customization.md)
- CSS class customization (.e-card, .e-card-header, etc.)
- Styling header elements and titles
- Content and divider styling
- Image styling and backgrounds
- Button footer styling
- Theme customization and advanced styling

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete property reference for all components
- Enumeration values and accepted parameters
- CSS class listings for styling
- Component hierarchy and integration
- Quick reference examples

## Quick Start Example

### Basic Card with Header and Content

```razor
@using Syncfusion.Blazor.Cards

<SfCard>
    <CardHeader Title="Welcome" SubTitle="Getting Started"></CardHeader>
    <CardContent Content="This is a basic Syncfusion Blazor Card component with header and content."></CardContent>
</SfCard>
```

### Card with Image and Action Buttons

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardImage Image="images/card.png"/>
    <CardHeader Title="Product Title" SubTitle="by Company"></CardHeader>
    <CardContent Content="Description of the product goes here with key features and benefits."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-outline e-primary">BUY NOW</SfButton>
            <SfButton CssClass="e-btn e-outline">DETAILS</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Horizontal Card with Stacked Section

```razor
@using Syncfusion.Blazor.Cards

<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Syncfusion Card" />
        <CardContent Content="The Syncfusion Blazor Card component is used to display content in organized containers." />
    </CardStacked>
    <img src="images/card-image.png" alt="Card" />
</SfCard>
```

## Common Patterns

### Pattern 1: Product Card
Combine image, header, description, and action buttons for product listings.

```razor
<SfCard>
    <CardImage Image="product.png"/>
    <CardHeader Title="Product Name" SubTitle="$49.99"/>
    <CardContent Content="High-quality product description with key features."/>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">ADD TO CART</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Pattern 2: Profile Card
Display user information with avatar and action buttons.

```razor
<SfCard>
    <CardHeader Title="John Doe" SubTitle="UI Designer" ImageUrl="avatar.png"/>
    <CardContent Content="Creative designer with 5+ years experience in web and mobile design."/>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-outline">FOLLOW</SfButton>
            <SfButton CssClass="e-btn e-outline">MESSAGE</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Pattern 3: Multi-Section Card
Use multiple CardContent elements with dividers to organize information.

```razor
<SfCard>
    <CardHeader Title="City Information" />
    <CardContent EnableSeparator="true">
        <strong>Sydney</strong><br/>
        Capital of New South Wales, Australia
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Location</strong><br/>
        East coast of Australia
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Population</strong><br/>
        Approximately 4 million residents
    </CardContent>
</SfCard>
```

## Key Components

| Component | Purpose |
|-----------|---------|
| `SfCard` | Root card container with optional Orientation property |
| `CardHeader` | Displays Title, SubTitle, and optional ImageUrl |
| `CardImage` | Renders image elements within card |
| `CardContent` | Contains text and HTML content with optional EnableSeparator |
| `CardFooter` | Footer section for action buttons |
| `CardFooterContent` | Container for footer content (buttons) |
| `CardStacked` | Creates vertical section in horizontal cards |

## Key Properties

| Property | Values | Purpose |
|----------|--------|---------|
| `Orientation` | `Vertical` (default), `Horizontal` | Arranges card elements |
| `ID` | string | Unique identifier for card |
| `Title` | string | Card header main title |
| `SubTitle` | string | Card header subtitle |
| `ImageUrl` | string | Path to header image |
| `Content` | string | Card content text |
| `EnableSeparator` | true/false | Adds divider between sections |
| `Image` | string | CardImage source path |

## Quick Decision Tree

**When choosing card layout:**
- **Vertical cards**: Default, use for product listings, profiles, information cards
- **Horizontal cards**: Use for featured items with large images
- **Multiple sections**: Use CardContent with EnableSeparator for organized data
- **Action buttons**: Always use CardFooter + CardFooterContent for interactive cards

## Integration Points

- **With SfButton**: Use for interactive action buttons in card footers
- **With CSS Classes**: Apply .e-card, .e-card-header, .e-card-content for styling
- **With HTML**: Embed custom HTML directly in CardContent
- **With Images**: Use CardImage or img tags for visual content

---

## References

- [Syncfusion Blazor Card Documentation](url)
- [System Requirements](url)
- [NuGet Packages](url)
