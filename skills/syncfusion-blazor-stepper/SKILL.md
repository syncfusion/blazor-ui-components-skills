---
name: syncfusion-blazor-stepper
description: Implement the Syncfusion Blazor Stepper component for multi-step workflows. Use this skill when creating step-by-step navigation UI, wizards, form workflows, or progress tracking. This skill covers step-based navigation, wizard interfaces, step validation, animations, templates, and stepper customization in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Navigation"
---

# Implementing the Syncfusion Blazor Stepper Component

The Blazor Stepper component provides an intuitive way to guide users through multi-step processes. It displays progress across numbered or labeled steps, supporting linear and non-linear workflows with customizable animations, templates, and validation states.

## When to Use This Skill

Use the Stepper component when you need to:
- Create multi-step wizards or workflows
- Display progress through sequential tasks
- Build step-by-step forms with validation
- Implement checklist-style navigation
- Guide users through onboarding processes
- Show optional vs. required steps
- Validate step completion before progression

## Component Overview

The Stepper displays steps in a horizontal or vertical layout with configurable indicators, labels, and templates. Each step can be customized with:
- **Visual states**: Not started, in progress, completed, error
- **Labels and icons**: Custom text and CSS icon classes
- **Validation**: Success or error indicators
- **Templates**: Custom content for advanced layouts
- **Animations**: Smooth transitions between steps

## Documentation and Navigation Guide

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete API documentation for all properties, methods, and events
- Component and property definitions with direct links to implementation guides
- Event arguments reference and enumeration types
- Complete component hierarchy (SfStepper, StepperStep, StepperAnimationSettings)

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup for Blazor WebAssembly and Blazor Web App
- Service registration and namespace imports
- Basic Stepper implementation with icons and labels
- Interactive render modes for modern Web Apps
- Theme configuration and CSS imports

### Step Types and Configuration
📄 **Read:** [references/step-types.md](references/step-types.md)
- Step type variations (Default, Label, Indicator)
- Label positioning (Top, Bottom, Start, End)
- Step properties and attributes
- Optional and disabled steps
- Linear vs. non-linear workflows
- Step status management

### Events and Interactions
📄 **Read:** [references/events.md](references/events.md)
- Event handlers and lifecycle (Created, StepChanged, StepChanging, StepClicked, StepRendered)
- Event arguments and parameters
- Common event patterns
- Handling step progression and validation

### Templates and Customization
📄 **Read:** [references/templates-and-styling.md](references/templates-and-styling.md)
- Custom step templates
- Template context and properties
- CSS styling and customization
- CssClass property for individual steps
- Theme customization and appearance

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Step validation (IsValid property and validation states)
- Validation workflow implementation
- Tooltip support (ShowTooltip property and custom templates)
- Step content templates and custom layouts
- Step completion tracking and error handling
- Conditional step display patterns

### Animations and Globalization
📄 **Read:** [references/animations-and-globalization.md](references/animations-and-globalization.md)
- Animation settings (Duration and Delay)
- Enabling/disabling animations
- Localization support
- RTL (Right-to-Left) support
- Multi-language configuration

### Orientations and Layouts
📄 **Read:** [references/orientations-and-layouts.md](references/orientations-and-layouts.md)
- Horizontal orientation (default)
- Vertical orientation
- Linear flow with sequential progression
- Non-linear flow with free navigation
- Responsive design patterns
- Mobile and desktop layout considerations

## Quick Start Example

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Address" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Payment" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Ordered" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

## Common Patterns

### Linear Workflow
Use the `Linear` property to enforce sequential progression—users can only move to the next step after completing the current one.

```csharp
<SfStepper Linear="true">
    <!-- Steps -->
</SfStepper>
```

### Active Step Control
Set the `ActiveStep` property to control which step is currently active (0-indexed).

```csharp
<SfStepper ActiveStep="1">
    <!-- Steps -->
</SfStepper>
```

### Tracking Step Changes
Handle the `StepChanged` event to respond when users navigate between steps.

```csharp
<SfStepper StepChanged="@OnStepChanged">
    <!-- Steps -->
</SfStepper>

@code {
    private void OnStepChanged(StepperChangedEventArgs args)
    {
        // Handle step change
    }
}
```

### Validation States
Display success or error states using the `IsValid` property on individual steps.

```csharp
<StepperStep Label="Payment" IsValid="true"></StepperStep>
<StepperStep Label="Confirm" IsValid="false"></StepperStep>
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `ActiveStep` | `int` | Sets the currently active step (0-indexed) |
| `LinearFlow` | `bool` | Enforces sequential step progression |
| `ReadOnly` | `bool` | Disables user interaction with the stepper |
| `StepType` | `StepperType` | Display format (Default, Label, Indicator) |
| `Orientation` | `StepperOrientation` | Layout direction (Horizontal, Vertical) |
| `LabelPosition` | `StepperLabelPosition` | Label placement (Top, Bottom, Start, End) |

**Step Properties:**
- `Label`: Display text for the step
- `Text`: Alternative text content
- `IconCss`: CSS class for custom icons
- `Disabled`: Prevents interaction with the step
- `Optional`: Marks step as optional
- `IsValid`: Sets validation state (true/false/null)
- `Status`: Current state (NotStarted, InProgress, Completed)
- `CssClass`: Custom CSS class for styling

## Common Use Cases

**Checkout Flow**: Guide users through purchase steps with validation and error handling.

**Form Wizards**: Break complex forms into manageable steps with optional sections.

**Onboarding**: Create step-by-step tutorials for new users with progress tracking.

**Setup Wizards**: Guide users through configuration processes with success/error states.

**Task Lists**: Display checklist-style progress with completed/pending indicators.
