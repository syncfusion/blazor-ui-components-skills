# Accessibility and Animations in Blazor Accordion Component

## Table of Contents
- [Accessibility Overview](#accessibility-overview)
- [Accessibility Compliance](#accessibility-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Accessibility Best Practices](#accessibility-best-practices)
- [Animation Configuration](#animation-configuration)
- [Animation Effects](#animation-effects)
- [Custom Animation Settings](#custom-animation-settings)

## Accessibility Overview

The Blazor Accordion component is designed with accessibility as a core feature, following WAI-ARIA specifications and providing comprehensive keyboard navigation and screen reader support.

## Accessibility Compliance

The Accordion component meets the following accessibility standards:

| Accessibility Criteria | Compliance Level |
|------------------------|------------------|
| WCAG 2.2 Support | AA |
| Section 508 Support | ✓ Yes |
| Screen Reader Support | ✓ Yes |
| Right-To-Left Support | ✓ Yes |
| Color Contrast | ✓ Yes |
| Mobile Device Support | ✓ Yes |
| Keyboard Navigation Support | ✓ Yes |
| Axe-core Accessibility Validation | ✓ Yes |

**Legend:**
- ✓ Yes: All features meet the requirement
- Partial: Some features meet the requirement
- No: The component does not meet the requirement

### Standards Covered

**WCAG 2.2 (Web Content Accessibility Guidelines):**
- Level AA compliance
- Perceivable, operable, understandable, and robust content
- Keyboard accessible
- Sufficient color contrast

**Section 508:**
- Federal accessibility standards compliance
- Compatible with assistive technologies
- Keyboard-only navigation support

**WAI-ARIA (Web Accessibility Initiative - Accessible Rich Internet Applications):**
- Proper ARIA roles and attributes
- State and property management
- Follows accordion design pattern

## WAI-ARIA Attributes

The Accordion component implements appropriate ARIA attributes to ensure compatibility with assistive technologies.

### Implemented ARIA Attributes

| Attribute | Applied To | Purpose |
|-----------|-----------|---------|
| `role="button"` | Accordion header | Indicates header can toggle content visibility |
| `role="region"` | Accordion panel | Creates landmark region for expanded content |
| `aria-labelledby` | Content panel | Points to corresponding accordion header |
| `aria-controls` | Header | Points to corresponding accordion content |
| `aria-expanded` | Header | Indicates expand state (true/false) |
| `aria-hidden` | Panel | Indicates content visibility state |
| `aria-disabled` | Accordion/Item | Indicates disabled state |

### How ARIA Attributes Work

**Header with role="button":**
```html
<div role="button" aria-expanded="false" aria-controls="content-1">
    Header Text
</div>
```

**Content with role="region":**
```html
<div role="region" aria-labelledby="header-1" aria-hidden="true">
    Content Text
</div>
```

**When expanded:**
- `aria-expanded` changes to `"true"`
- `aria-hidden` changes to `"false"`
- Screen readers announce the state change

## Keyboard Navigation

The Accordion component supports full keyboard navigation, allowing users to interact without a mouse.

### Supported Keyboard Shortcuts

| Key (Windows/Mac) | Action |
|-------------------|--------|
| <kbd>Space</kbd> / <kbd>Enter</kbd> | Expand or collapse the focused accordion item |
| <kbd>↓</kbd> | Move focus to the next accordion header |
| <kbd>↑</kbd> | Move focus to the previous accordion header |
| <kbd>Home</kbd> | Move focus to the first accordion header |
| <kbd>End</kbd> | Move focus to the last accordion header |
| <kbd>Tab</kbd> | Move focus out of the accordion to the next focusable element |
| <kbd>Shift</kbd> + <kbd>Tab</kbd> | Move focus to the previous focusable element |

### Keyboard Navigation Behavior

**Initial Focus:**
- Tabbing into the accordion focuses the first header
- If an item is expanded, focus goes to that header

**Within Accordion:**
- Arrow keys navigate between headers only
- Space/Enter toggles the focused item
- Focus indicators clearly show current position

**Leaving Accordion:**
- Tab moves to next focusable element outside accordion
- Shift+Tab moves to previous focusable element

### Implementation

Keyboard navigation works automatically - no additional configuration required:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
        <AccordionItem Header="Item 3" Content="Content 3"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

## Screen Reader Support

The Accordion component provides comprehensive screen reader support for visually impaired users.

### Screen Reader Announcements

**When focusing a header:**
- Announces header text
- Announces current state (expanded or collapsed)
- Announces position ("Item 1 of 3")

**When expanding:**
- Announces "Expanded"
- Reads associated content

**When collapsing:**
- Announces "Collapsed"

### Supported Screen Readers

- **JAWS** (Job Access With Speech)
- **NVDA** (NonVisual Desktop Access)
- **VoiceOver** (macOS/iOS)
- **TalkBack** (Android)
- **Narrator** (Windows)

### Testing with Screen Readers

The accordion component has been tested with major screen readers and meets accessibility requirements. You can test your implementation using Axe-core:

```bash
# Install Axe-core for testing
dotnet add package Deque.AxeCore.Playwright
```

## Accessibility Best Practices

### Meaningful Header Text

Provide descriptive headers that clearly indicate content:

```cshtml
<!-- ✓ Good: Clear and descriptive -->
<AccordionItem Header="Shipping and Delivery Information" Content="..."></AccordionItem>

<!-- ✗ Bad: Vague -->
<AccordionItem Header="More Info" Content="..."></AccordionItem>
```

### Logical Order

Arrange items in a logical sequence that makes sense when navigated with keyboard:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Step 1: Registration" Content="..."></AccordionItem>
        <AccordionItem Header="Step 2: Verification" Content="..."></AccordionItem>
        <AccordionItem Header="Step 3: Completion" Content="..."></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Color Contrast

Ensure sufficient contrast between text and background:

```css
/* Minimum contrast ratios (WCAG AA): */
/* Normal text: 4.5:1 */
/* Large text (18pt+): 3:1 */

.e-accordion .e-acrdn-header {
    color: #000000; /* Black text */
    background: #FFFFFF; /* White background */
    /* Contrast ratio: 21:1 ✓ */
}
```

### Focus Indicators

Maintain visible focus indicators (default provided):

```css
/* Customize if needed, but keep visible */
.e-accordion .e-acrdn-header:focus {
    outline: 2px solid #2196F3;
    outline-offset: 2px;
}
```

### Right-to-Left (RTL) Support

The component automatically supports RTL languages:

```cshtml
<div dir="rtl">
    <SfAccordion>
        <AccordionItems>
            <AccordionItem Header="العنوان" Content="المحتوى"></AccordionItem>
        </AccordionItems>
    </SfAccordion>
</div>
```

## Animation Configuration

The Accordion component supports customizable animations for expand and collapse actions.

### Default Animations

By default, the accordion uses:
- **Expand:** SlideDown effect
- **Collapse:** SlideUp effect

### AccordionAnimationSettings

Configure animations using the `AccordionAnimationSettings` component:

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <AccordionAnimationExpand Effect="AnimationEffect.SlideDown" Duration="400" Easing="ease"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="AnimationEffect.SlideUp" Duration="400" Easing="ease"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Animation Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Effect` | AnimationEffect | SlideDown/SlideUp | Animation effect type |
| `Duration` | int | 400 | Animation duration in milliseconds |
| `Easing` | string | "ease" | CSS easing function |

## Animation Effects

The following animation effects are available:

### Available Effects

| Effect | Description | Use Case |
|--------|-------------|----------|
| `SlideDown` | Content slides down | Default expand (smooth, natural) |
| `SlideUp` | Content slides up | Default collapse (smooth, natural) |
| `FadeIn` | Content fades in | Subtle, elegant expand |
| `FadeOut` | Content fades out | Subtle, elegant collapse |
| `FadeZoomIn` | Fade in with zoom | Attention-grabbing expand |
| `FadeZoomOut` | Fade out with zoom | Attention-grabbing collapse |
| `ZoomIn` | Content zooms in | Bold, dynamic expand |
| `ZoomOut` | Content zooms out | Bold, dynamic collapse |
| `None` | No animation | Instant toggle, accessibility mode |

### Examples

#### Fade Animation

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <AccordionAnimationExpand Effect="AnimationEffect.FadeIn" Duration="300"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="AnimationEffect.FadeOut" Duration="300"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <AccordionItem Header="Fade Effect" Content="This uses fade animation"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

#### Zoom Animation

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <AccordionAnimationExpand Effect="AnimationEffect.ZoomIn" Duration="400"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="AnimationEffect.ZoomOut" Duration="400"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <AccordionItem Header="Zoom Effect" Content="This uses zoom animation"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

#### No Animation (Accessibility Mode)

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <AccordionAnimationExpand Effect="AnimationEffect.None"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="AnimationEffect.None"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <AccordionItem Header="No Animation" Content="Instant toggle"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**When to disable animations:**
- User prefers reduced motion (prefers-reduced-motion CSS media query)
- Performance issues on low-end devices
- Accessibility requirements
- Testing scenarios

## Custom Animation Settings

### Adjusting Duration

Control animation speed (in milliseconds):

```cshtml
<AccordionAnimationSettings>
    <!-- Fast animation (200ms) -->
    <AccordionAnimationExpand Effect="AnimationEffect.SlideDown" Duration="200"></AccordionAnimationExpand>
    
    <!-- Slow animation (800ms) -->
    <AccordionAnimationCollapse Effect="AnimationEffect.SlideUp" Duration="800"></AccordionAnimationCollapse>
</AccordionAnimationSettings>
```

**Guidelines:**
- **200-300ms:** Fast, snappy feel
- **400-500ms:** Balanced (recommended)
- **600-800ms:** Slow, emphasize motion
- **>800ms:** Potentially frustrating

### Customizing Easing

Control acceleration curve:

```cshtml
<AccordionAnimationSettings>
    <AccordionAnimationExpand Easing="ease-in"></AccordionAnimationExpand>
    <AccordionAnimationCollapse Easing="ease-out"></AccordionAnimationCollapse>
</AccordionAnimationSettings>
```

**Common easing functions:**
- `ease`: Slow start and end (default)
- `linear`: Constant speed
- `ease-in`: Slow start, fast end
- `ease-out`: Fast start, slow end
- `ease-in-out`: Slow start and end (more pronounced)

### Respecting User Preferences

Detect and respect user's motion preferences:

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <AccordionAnimationExpand Effect="@ExpandEffect"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="@CollapseEffect"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    AnimationEffect ExpandEffect = AnimationEffect.SlideDown;
    AnimationEffect CollapseEffect = AnimationEffect.SlideUp;
    
    protected override async Task OnInitializedAsync() {
        // Check user preference (would need JS interop in real implementation)
        bool prefersReducedMotion = await CheckReducedMotionPreference();
        
        if (prefersReducedMotion) {
            ExpandEffect = AnimationEffect.None;
            CollapseEffect = AnimationEffect.None;
        }
    }
    
    async Task<bool> CheckReducedMotionPreference() {
        // Implementation would use JS interop to check:
        // window.matchMedia('(prefers-reduced-motion: reduce)').matches
        return false;
    }
}
```

### CSS-Based Reduced Motion

Alternative approach using CSS:

```css
@media (prefers-reduced-motion: reduce) {
    .e-accordion .e-acrdn-panel {
        transition: none !important;
        animation: none !important;
    }
}
```

## Combining Accessibility and Animations

Balance aesthetics with accessibility:

```cshtml
<SfAccordion>
    <AccordionAnimationSettings>
        <!-- Moderate duration for good UX -->
        <AccordionAnimationExpand Effect="AnimationEffect.SlideDown" Duration="350"></AccordionAnimationExpand>
        <AccordionAnimationCollapse Effect="AnimationEffect.SlideUp" Duration="350"></AccordionAnimationCollapse>
    </AccordionAnimationSettings>
    <AccordionItems>
        <!-- Clear, descriptive headers for screen readers -->
        <AccordionItem Header="Shipping Information" Content="Detailed shipping info..."></AccordionItem>
        <AccordionItem Header="Return Policy" Content="Return policy details..."></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    /* Ensure good contrast */
    .e-accordion .e-acrdn-header {
        color: #000000;
        background: #FFFFFF;
    }
    
    /* Visible focus indicator */
    .e-accordion .e-acrdn-header:focus {
        outline: 2px solid #2196F3;
        outline-offset: 2px;
    }
    
    /* Respect user motion preferences */
    @media (prefers-reduced-motion: reduce) {
        .e-accordion * {
            animation-duration: 0.01ms !important;
            transition-duration: 0.01ms !important;
        }
    }
</style>
```

## Testing Accessibility

### Manual Testing

1. **Keyboard Navigation:** Tab through all interactive elements
2. **Screen Reader:** Test with NVDA, JAWS, or VoiceOver
3. **Color Contrast:** Use browser dev tools or online checkers
4. **Focus Indicators:** Ensure focus is always visible
5. **RTL Languages:** Test with dir="rtl"

### Automated Testing

Use axe-core for automated accessibility validation. The component passes axe-core tests.

## Related Topics

- [Getting Started](getting-started.md) - Basic setup
- [Expand Modes](expand-modes.md) - Single vs multiple expand behavior
- [Customization and Styling](customization-styling.md) - CSS customization with accessibility considerations
- [How-To Scenarios](how-to-scenarios.md) - Practical implementation examples
