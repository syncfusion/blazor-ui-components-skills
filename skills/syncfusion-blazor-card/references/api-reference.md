# Syncfusion Blazor Card Component - API Reference

## Overview

The Syncfusion Blazor Card component is a flexible container for organizing and displaying content with structured sections including headers, images, content, and action buttons. This API reference documents all properties, methods, and events for the Card component family, with comprehensive examples for each feature.

## Quick Navigation

- **Components**: [SfCard](#sfcard-component) | [CardHeader](#cardheader-component) | [CardContent](#cardcontent-component) | [CardImage](#cardimage-component) | [CardFooter](#cardfooter-component) | [CardFooterContent](#cardfootercontent-component) | [CardStacked](#cardstacked-component)
- **Enumerations**: [CardOrientation](#cardorientation) | [ImagePosition](#imageposition) | [TitlePositions](#titlepositions)
- **Styling**: [CSS Classes](#css-classes-for-styling) | [Examples](#css-styling-examples)
- **Integration**: [Component Hierarchy](#component-hierarchy) | [Syncfusion Components](#integration-with-syncfusion-components)

---

## SfCard Component

The main Card container component that serves as the root element for all card content.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | Any valid Blazor component | Accepts nested card child components (CardHeader, CardContent, CardImage, CardFooter) | [view](#sfcard-childcontent) |
| `CssClass` | `string` | null | CSS class names | Applies custom CSS classes to the card element for styling | [view](#sfcard-cssclass) |
| `EnableRtl` | `bool` | false | `true`, `false` | Enables right-to-left text direction rendering for RTL languages | [view](#sfcard-enablertl) |
| `ID` | `string` | null | Any valid HTML ID | Unique identifier for the card component, used for CSS targeting | [view](#sfcard-id) |
| `Orientation` | `CardOrientation` | `Vertical` | `Vertical`, `Horizontal` | Arranges card elements vertically (default) or horizontally | [view](#sfcard-orientation) |

#### SfCard ChildContent
```razor
@using Syncfusion.Blazor.Cards

<SfCard>
    <ChildContent>
        <CardHeader Title="Header"></CardHeader>
        <CardContent Content="Content"></CardContent>
        <CardFooter>
            <CardFooterContent>
                <!-- Footer content -->
            </CardFooterContent>
        </CardFooter>
    </ChildContent>
</SfCard>
```

#### SfCard CssClass
```razor
<SfCard CssClass="primary-card elevated-shadow">
    <CardHeader Title="Styled Card"></CardHeader>
    <CardContent Content="Card with custom CSS classes"></CardContent>
</SfCard>

<style>
    .primary-card {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
    }
    .elevated-shadow {
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    }
</style>
```

#### SfCard EnableRtl
```razor
<!-- For RTL languages like Arabic, Hebrew -->
<SfCard EnableRtl="true">
    <CardHeader Title="مرحبا بالعالم"></CardHeader>
    <CardContent Content="محتوى البطاقة"></CardContent>
</SfCard>
```

#### SfCard ID
```razor
<SfCard ID="UniqueCardId">
    <CardHeader Title="Card with ID"></CardHeader>
</SfCard>

<style>
    #UniqueCardId {
        border: 2px solid #2196f3;
        border-radius: 8px;
    }
</style>
```

#### SfCard Orientation
```razor
<!-- Vertical (Default) -->
<SfCard Orientation="CardOrientation.Vertical">
    <CardImage Image="image.jpg"/>
    <CardHeader Title="Title"></CardHeader>
    <CardContent Content="Content"></CardContent>
</SfCard>

<!-- Horizontal -->
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Title"></CardHeader>
        <CardContent Content="Content"></CardContent>
    </CardStacked>
    <img src="image.jpg" />
</SfCard>
```

---

## CardHeader Component

Displays header information at the top of the card with optional title, subtitle, and image.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | Any valid Blazor component | Allows custom HTML/component content inside the header | [view](#cardheader-childcontent) |
| `Title` | `string` | null | Any string value | Main title text displayed in the card header | [view](#cardheader-title) |
| `SubTitle` | `string` | null | Any string value | Subtitle text displayed below the main title | [view](#cardheader-subtitle) |
| `ImageUrl` | `string` | null | Valid image path/URL | Path or URL to a header image displayed alongside the title | [view](#cardheader-imageurl) |
| `ImagePosition` | `ImagePosition` | `Left` | `Left`, `Right` | Position of header image (before or after title/subtitle) | [view](#cardheader-imageposition) |

#### CardHeader ChildContent
```razor
<SfCard>
    <CardHeader>
        <ChildContent>
            <div>
                <h3>Custom Header</h3>
                <p>With custom HTML structure</p>
            </div>
        </ChildContent>
    </CardHeader>
</SfCard>
```

#### CardHeader Title
```razor
<SfCard>
    <CardHeader Title="Card Title">
    </CardHeader>
</SfCard>
```

#### CardHeader SubTitle
```razor
<SfCard>
    <CardHeader 
        Title="John Doe"
        SubTitle="Senior Developer">
    </CardHeader>
</SfCard>
```

#### CardHeader ImageUrl
```razor
<SfCard>
    <CardHeader 
        Title="User Profile"
        ImageUrl="images/avatar.png">
    </CardHeader>
</SfCard>
```

#### CardHeader ImagePosition
```razor
<!-- Image at Left -->
<SfCard>
    <CardHeader 
        Title="With Avatar"
        ImageUrl="images/avatar.png"
        ImagePosition="ImagePosition.Left">
    </CardHeader>
</SfCard>

<!-- Image at Right -->
<SfCard>
    <CardHeader 
        Title="With Icon"
        ImageUrl="images/icon.png"
        ImagePosition="ImagePosition.Right">
    </CardHeader>
</SfCard>
```

---

## CardContent Component

Displays text, HTML, and other content sections within the card body.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | Any valid Blazor component or HTML | Allows custom HTML/component content inside the content section | [view](#cardcontent-childcontent) |
| `Content` | `string` | null | Any string value (supports HTML) | Text content displayed in the card content area | [view](#cardcontent-content) |
| `EnableSeparator` | `bool` | false | `true`, `false` | Displays a dividing line below the content section | [view](#cardcontent-enableseparator) |

#### CardContent ChildContent
```razor
<SfCard>
    <CardContent>
        <ChildContent>
            <div class="custom-content">
                <h4>Custom Content Section</h4>
                <ul>
                    <li>Item 1</li>
                    <li>Item 2</li>
                    <li>Item 3</li>
                </ul>
            </div>
        </ChildContent>
    </CardContent>
</SfCard>
```

#### CardContent Content
```razor
<SfCard>
    <CardHeader Title="Description"></CardHeader>
    <CardContent Content="This is simple text content displayed in the card."></CardContent>
</SfCard>
```

#### CardContent EnableSeparator
```razor
<SfCard>
    <CardHeader Title="Information"></CardHeader>
    
    <CardContent EnableSeparator="true">
        <strong>Section 1:</strong> First section with a divider below
    </CardContent>
    
    <CardContent EnableSeparator="true">
        <strong>Section 2:</strong> Second section with a divider below
    </CardContent>
    
    <CardContent EnableSeparator="false">
        <strong>Section 3:</strong> Last section without divider
    </CardContent>
</SfCard>
```

---

## CardImage Component

Displays images within the card as visual elements or overlays with optional titles.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | Any valid Blazor component or HTML | Custom content rendered within the image container | [view](#cardimage-childcontent) |
| `Image` | `string` | null | Valid image path/URL | Source path or URL to the image to display | [view](#cardimage-image) |
| `Title` | `string` | null | Any string value | Optional title overlay displayed on the image | [view](#cardimage-title) |
| `TitlePosition` | `TitlePositions` | `BottomLeft` | `TopLeft`, `TopRight`, `BottomLeft`, `BottomRight` | Position of the title overlay on the image | [view](#cardimage-titleposition) |

#### CardImage ChildContent
```razor
<SfCard>
    <CardImage>
        <ChildContent>
            <div class="image-overlay">
                <span class="badge">Featured</span>
            </div>
        </ChildContent>
    </CardImage>
</SfCard>
```

#### CardImage Image
```razor
<SfCard>
    <CardImage Image="images/photo.jpg"></CardImage>
    <CardHeader Title="Photo Gallery"></CardHeader>
</SfCard>
```

#### CardImage Title
```razor
<SfCard>
    <CardImage 
        Image="images/mountain.jpg"
        Title="Mountain Peak">
    </CardImage>
</SfCard>
```

#### CardImage TitlePosition
```razor
<!-- Top Left -->
<SfCard>
    <CardImage 
        Image="images/photo.jpg"
        Title="Top Left"
        TitlePosition="TitlePositions.TopLeft">
    </CardImage>
</SfCard>

<!-- Top Right -->
<SfCard>
    <CardImage 
        Image="images/photo.jpg"
        Title="Top Right"
        TitlePosition="TitlePositions.TopRight">
    </CardImage>
</SfCard>

<!-- Bottom Left -->
<SfCard>
    <CardImage 
        Image="images/photo.jpg"
        Title="Bottom Left"
        TitlePosition="TitlePositions.BottomLeft">
    </CardImage>
</SfCard>

<!-- Bottom Right (Default) -->
<SfCard>
    <CardImage 
        Image="images/photo.jpg"
        Title="Bottom Right"
        TitlePosition="TitlePositions.BottomRight">
    </CardImage>
</SfCard>
```

---

## CardFooter Component

Footer section for displaying action buttons or footer content.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | Any valid Blazor component | Allows nested CardFooterContent or custom content | [view](#cardfooter-childcontent) |
| `Orientation` | `CardOrientation` | `Horizontal` | `Vertical`, `Horizontal` | Arranges footer buttons vertically or horizontally | [view](#cardfooter-orientation) |

#### CardFooter ChildContent
```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardHeader Title="Actions"></CardHeader>
    <CardContent Content="Select an action below."></CardContent>
    <CardFooter>
        <ChildContent>
            <CardFooterContent>
                <SfButton CssClass="e-btn e-primary">Save</SfButton>
                <SfButton CssClass="e-btn e-outline">Cancel</SfButton>
            </CardFooterContent>
        </ChildContent>
    </CardFooter>
</SfCard>
```

#### CardFooter Orientation
```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<!-- Horizontal Buttons (Default) -->
<SfCard>
    <CardHeader Title="Horizontal Buttons"></CardHeader>
    <CardFooter Orientation="CardOrientation.Horizontal">
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">Button 1</SfButton>
            <SfButton CssClass="e-btn e-outline">Button 2</SfButton>
            <SfButton CssClass="e-btn e-outline">Button 3</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>

<!-- Vertical Buttons -->
<SfCard>
    <CardHeader Title="Vertical Buttons"></CardHeader>
    <CardFooter Orientation="CardOrientation.Vertical">
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary" Style="width: 100%;">Button 1</SfButton>
            <SfButton CssClass="e-btn e-outline" Style="width: 100%;">Button 2</SfButton>
            <SfButton CssClass="e-btn e-outline" Style="width: 100%;">Button 3</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

---

## CardFooterContent Component

Container for footer content, typically used to wrap action buttons.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | `SfButton`, `SfAnchor`, or custom HTML | Accepts buttons and other footer elements (typically action buttons) | [view](#cardfootercontent-childcontent) || `Text` | `string` | null | Any string value | Text content displayed in the card footer content | [view](#cardfootercontent-text) |
#### CardFooterContent ChildContent
```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardHeader Title="With Buttons"></CardHeader>
    <CardContent Content="This card has action buttons."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <ChildContent>
                <SfButton CssClass="e-btn e-primary">SAVE</SfButton>
                <SfButton CssClass="e-btn e-outline">CLOSE</SfButton>
            </ChildContent>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

#### CardFooterContent Text
```razor
@using Syncfusion.Blazor.Cards

<SfCard>
    <CardHeader Title="Footer Text"></CardHeader>
    <CardContent Content="Card with text in footer."></CardContent>
    <CardFooter>
        <CardFooterContent Text="Footer text content here">
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

---

## CardStacked Component

Used within horizontal cards to create vertically stacked sections while maintaining horizontal layout.

### Properties

| Property | Type | Default | Accepted Values | Description | Example |
|----------|------|---------|-----------------|-------------|---------|
| `ChildContent` | `RenderFragment` | null | Any valid card child component | Wraps elements that should remain vertically stacked in horizontal layout | [view](#cardstacked-childcontent) |

#### CardStacked ChildContent
```razor
@using Syncfusion.Blazor.Cards

<!-- Horizontal layout with vertically stacked content -->
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <ChildContent>
            <CardHeader Title="Product Name"></CardHeader>
            <CardContent Content="Product description goes here."></CardContent>
        </ChildContent>
    </CardStacked>
    <img src="product-image.jpg" alt="Product" />
</SfCard>
```

---

## Enumeration Values

### CardOrientation

Defines the layout direction of card elements.

| Value | Description | Usage |
|-------|-------------|-------|
| `Vertical` | Stacks elements from top to bottom (default) | Default card layout for all elements |
| `Horizontal` | Arranges elements side-by-side left to right | Featured cards with images beside content |

### ImagePosition

Defines the position of header images relative to title/subtitle.

| Value | Description | Usage |
|-------|-------------|-------|
| `Left` | Image appears on the left of title/subtitle | Profile cards, user avatars |
| `Right` | Image appears on the right of title/subtitle | Optional header image placement |

### TitlePositions

Defines the position of title overlay on card images.

| Value | Description | Usage |
|-------|-------------|-------|
| `TopLeft` | Title at top-left corner of image | Top-left image title positioning |
| `TopRight` | Title at top-right corner of image | Top-right image title positioning |
| `BottomLeft` | Title at bottom-left corner of image | Bottom-left image title positioning |
| `BottomRight` | Title at bottom-right corner of image | Default image title position |

---

## Component Hierarchy

```
<SfCard>
    ├── <CardImage />
    ├── <CardHeader />
    │   └── Supports Title, SubTitle, ImageUrl
    ├── <CardContent /> (Multiple supported)
    │   └── Supports EnableSeparator for dividers
    ├── <CardFooter>
    │   └── <CardFooterContent>
    │       └── <SfButton /> or other controls
    └── <CardStacked /> (For horizontal layout)
        └── Wraps elements to keep vertically aligned
```

---

## CSS Styling Examples

### Basic Card Styling
```razor
<SfCard CssClass="e-card" ID="BasicCard">
    <CardHeader Title="Styled Card"></CardHeader>
    <CardContent Content="Content with CSS classes applied"></CardContent>
</SfCard>

<style>
    .e-card {
        border: 1px solid #e0e0e0;
        border-radius: 4px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
</style>
```

### Header Styling
```razor
<SfCard>
    <CardHeader Title="Custom Header" SubTitle="With styling">
    </CardHeader>
</SfCard>

<style>
    .e-card-header {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 16px;
    }
    
    .e-card-header-title {
        font-size: 18px;
        font-weight: 600;
    }
    
    .e-card-sub-title {
        font-size: 14px;
        opacity: 0.9;
    }
</style>
```

### Content Section Styling
```razor
<SfCard>
    <CardContent EnableSeparator="true">
        <p>First section</p>
    </CardContent>
    <CardContent EnableSeparator="true">
        <p>Second section</p>
    </CardContent>
</SfCard>

<style>
    .e-card-content {
        padding: 16px;
        font-size: 14px;
        line-height: 1.6;
    }
    
    .e-card-separator {
        border-bottom: 1px solid #e0e0e0;
    }
</style>
```

### Action Buttons Styling
```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardContent Content="Action buttons"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-card-btn e-primary">Save</SfButton>
            <SfButton CssClass="e-card-btn e-outline">Cancel</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>

<style>
    .e-card-actions {
        display: flex;
        gap: 8px;
        padding: 12px 16px;
        justify-content: flex-end;
    }
    
    .e-card-btn {
        padding: 8px 16px;
        border-radius: 4px;
        font-weight: 500;
    }
</style>
```

### Horizontal Layout Styling
```razor
<SfCard Orientation="CardOrientation.Horizontal" CssClass="e-card-horizontal">
    <CardStacked CssClass="e-card-stacked">
        <CardHeader Title="Product"></CardHeader>
        <CardContent Content="Description"></CardContent>
    </CardStacked>
    <img src="image.jpg" />
</SfCard>

<style>
    .e-card-horizontal {
        display: flex;
        align-items: stretch;
    }
    
    .e-card-horizontal img {
        width: 200px;
        object-fit: cover;
    }
    
    .e-card-stacked {
        flex: 1;
        display: flex;
        flex-direction: column;
    }
</style>
```

---

## CSS Classes Reference

### Main Elements

| Class | Element | Purpose | Example |
|-------|---------|---------|---------|
| `.e-card` | Main card container | Root container for all card content | `<SfCard CssClass="e-card">` |
| `.e-card-header` | Header section | Container for card header | Use with `<CardHeader>` |
| `.e-card-content` | Content area | Main content section | Use with `<CardContent>` |
| `.e-card-image` | Image element | Card image container | Use with `<CardImage>` |
| `.e-card-separator` | Divider between sections | Horizontal divider line | Enable with `EnableSeparator="true"` |
| `.e-card-footer` | Footer section | Action button container | Use with `<CardFooter>` |
| `.e-card-actions` | Action buttons container | Wraps footer buttons | Use with `<CardFooterContent>` |
| `.e-card-btn` | Card action button | Individual button styling | Apply to `<SfButton>` elements |

**Usage Example:**
```css
/* Style all cards */
.e-card {
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

/* Style card headers */
.e-card-header {
    background-color: #f5f5f5;
    border-bottom: 1px solid #e0e0e0;
}

/* Style action buttons */
.e-card-btn {
    padding: 8px 16px;
    margin-right: 8px;
}
```

### Header-Specific

| Class | Element | Purpose | Example |
|-------|---------|---------|---------|
| `.e-card-header-caption` | Header text wrapper | Text container in header | Applies to title/subtitle area |
| `.e-card-header-title` | Main title text | Primary header text | Set via `Title` property |
| `.e-card-sub-title` | Subtitle text | Secondary header text | Set via `SubTitle` property |
| `.e-card-header-image` | Header image element | Image in header | Set via `ImageUrl` property |

**Usage Example:**
```css
/* Customize header title */
.e-card-header-title {
    font-size: 18px;
    font-weight: 600;
    color: #333;
}

/* Customize subtitle */
.e-card-sub-title {
    font-size: 14px;
    color: #666;
    margin-top: 4px;
}

/* Style header image */
.e-card-header-image {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    object-fit: cover;
}
```

### Layout

| Class | Element | Purpose | Example |
|-------|---------|---------|---------|
| `.e-card-horizontal` | Horizontal card layout | Side-by-side layout | Set `Orientation="CardOrientation.Horizontal"` |
| `.e-card-stacked` | Vertically stacked section | Keep content vertical in horizontal cards | Use `<CardStacked>` component |

**Usage Example:**
```css
/* Horizontal card styling */
.e-card-horizontal {
    display: flex;
    align-items: stretch;
    gap: 16px;
}

.e-card-horizontal img {
    width: 200px;
    height: 200px;
    object-fit: cover;
}

/* Stacked section in horizontal layout */
.e-card-stacked {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
```

---

## Quick Reference Examples

### Basic Card
```razor
<SfCard>
    <CardHeader Title="Welcome"></CardHeader>
    <CardContent Content="This is a basic card."></CardContent>
</SfCard>
```

### Card with All Elements
```razor
<SfCard>
    <CardImage Image="images/card.jpg"/>
    <CardHeader Title="Title" SubTitle="Subtitle" ImageUrl="images/avatar.png"/>
    <CardContent EnableSeparator="true">First section</CardContent>
    <CardContent EnableSeparator="true">Second section</CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">Action</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Horizontal Card
```razor
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Product"></CardHeader>
        <CardContent Content="Description"></CardContent>
    </CardStacked>
    <img src="image.jpg" />
</SfCard>
```

### Styled Card
```razor
<SfCard CssClass="primary-card" ID="StyledCard">
    <CardHeader Title="Styled Card"></CardHeader>
    <CardContent Content="With custom styling"></CardContent>
</SfCard>
```

---

## Integration with Syncfusion Components

The Card component works seamlessly with other Syncfusion Blazor components:

- **SfButton** - Use in CardFooter and CardFooterContent for action buttons
- **Syncfusion Themes** - Apply bootstrap5, material, fabric, or highcontrast themes via CSS
- **Event Handlers** - Attach OnClick events to buttons within card footers
- **Data Binding** - Use foreach loops to generate multiple cards dynamically

---

## Accessibility

- The Card component supports **RTL** mode via `EnableRtl="true"` on SfCard
- Headers support semantic HTML with proper heading hierarchy
- Images support `alt` text via HTML img attributes
- Buttons within footers maintain keyboard navigation

---

## Related Documentation

**All property examples are included in this API Reference document above.**

For deeper conceptual understanding and advanced patterns, refer to these guides:

- [Getting Started](getting-started.md) - Installation, setup, and first steps
- [Building Card Structure](building-card-structure.md) - Advanced header, content, and image patterns
- [Card Layouts and Orientation](card-layouts-and-orientation.md) - Complex layouts and responsive design
- [Action Buttons and Footers](action-buttons-and-footers.md) - Interactive button patterns and events
- [Styling and Customization](styling-and-customization.md) - Theme customization and advanced CSS

---

## Official Syncfusion References

- [SfCard API](url)
- [CardHeader API](url)
- [CardContent API](url)
- [CardFooter API](url)
- [CardImage API](url)
- [Blazor Card Component](url)
