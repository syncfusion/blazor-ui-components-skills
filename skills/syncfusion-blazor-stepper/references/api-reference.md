# Syncfusion Blazor Stepper API Reference

## Table of Contents

- [Overview](#overview)
- [Main Component: SfStepper](#main-component-sfstepper)
  - [Properties](#properties)
  - [Methods](#methods)
  - [Events](#events)
- [StepperStep Component](#stepperstep-component)
- [StepperAnimationSettings Component](#stepperanimationsettings-component)
- [Enumeration Types](#enumeration-types)
- [Event Arguments Reference](#event-arguments-reference)
- [Complete Implementation Example](#complete-implementation-example)
- [Namespace and Usage](#namespace-and-usage)
- [Related References](#related-references)

---

## Overview

The **SfStepper** component from Syncfusion Blazor suite provides a visual representation of multi-step processes. This API reference documents all properties, methods, and events available in the Stepper component and its related classes.

---

## Main Component: SfStepper

### Properties

| Property | Type | Default | Description | Reference |
|----------|------|---------|-------------|-----------|
| `ActiveStep` | int | 0 | Gets or sets the current step index (0-based) | [step-types.md#active-step-control](step-types.md#active-step-control) |
| `CssClass` | string | Empty | Custom CSS class to customize the SfStepper appearance | [View Details](#css-class-property) |
| `ID` | string | null | Sets id attribute for the stepper element | [View Details](#id-property) |
| `LabelPosition` | `StepperLabelPosition` | Bottom | Position of step labels (Bottom, Top, Start, End) | [step-types.md#label-positioning](step-types.md#label-positioning) |
| `Linear` | bool | false | Restricts navigation to linear path when true | [orientations-and-layouts.md#linear-flow](orientations-and-layouts.md#linear-flow) |
| `Orientation` | `StepperOrientation` | Horizontal | Component orientation (Horizontal, Vertical) | [orientations-and-layouts.md](orientations-and-layouts.md) |
| `ReadOnly` | bool | false | Makes the SfStepper read-only when true | [step-types.md#read-only-mode](step-types.md#read-only-mode) |
| `ShowTooltip` | bool | false | Displays tooltips for steps when true | [advanced-features.md#tooltip-support](advanced-features.md#tooltip-support) |
| `StepType` | `StepperType` | Default | Display style (Default, Indicator, Label) | [step-types.md](step-types.md) |
| `Template` | `RenderFragment<StepperStep>` | null | Custom template for rendering individual steps | [templates-and-styling.md#custom-step-templates](templates-and-styling.md#custom-step-templates) |
| `TooltipTemplate` | `RenderFragment<StepperStep>` | null | Custom template for rendering tooltips | [View Details](#tooltip-template-property) |

> **Note:** For detailed examples of properties like `ActiveStep`, `LabelPosition`, `Linear`, `Orientation`, `ReadOnly`, `ShowTooltip`, `StepType`, and `Template`, refer to their respective documentation files linked above.

#### CssClass Example

```csharp
<SfStepper CssClass="custom-stepper dark-theme">
    <StepperSteps>
        <StepperStep Label="Styled Step"></StepperStep>
    </StepperSteps>
</SfStepper>

<style>
    .custom-stepper.dark-theme {
        background-color: #333;
        color: #fff;
    }
</style>
```

#### ID Example

```csharp
<SfStepper ID="checkoutStepper">
    <StepperSteps>
        <StepperStep Label="Checkout"></StepperStep>
    </StepperSteps>
</SfStepper>
```

#### TooltipTemplate Example

```csharp
<SfStepper ShowTooltip="true">
    <StepperSteps>
        <StepperStep Label="Info" Text="Step information"></StepperStep>
    </StepperSteps>
    <TooltipTemplate>
        <div class="custom-tooltip">
            <strong>@context.Label</strong>
            <span>@context.Text</span>
        </div>
    </TooltipTemplate>
</SfStepper>
```

---

### Methods

| Method | Parameters | Return Type | Description | Reference |
|--------|------------|-------------|-------------|-----------|
| `NextStepAsync()` | None | Task | Moves to the next step from current | [View Details](#next-step-async) |
| `PreviousStepAsync()` | None | Task | Moves to the previous step from current | [View Details](#previous-step-async) |
| `ResetAsync()` | None | Task | Resets stepper to first step | [View Details](#reset-async) |
| `RefreshProgressbarAsync()` | None | Task | Refreshes progress bar position after container resize | [View Details](#refresh-progressbar-async) |

#### NextStepAsync

Programmatically advance to the next step in the stepper.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

<button @onclick="GoNext">Next Step</button>

@code {
    private SfStepper stepper;
    
    private async Task GoNext()
    {
        await stepper.NextStepAsync();
    }
}
```

#### PreviousStepAsync

Move back to the previous step.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

<button @onclick="GoPrevious">Previous Step</button>

@code {
    private SfStepper stepper;
    
    private async Task GoPrevious()
    {
        await stepper.PreviousStepAsync();
    }
}
```

#### ResetAsync

Reset the stepper to its initial state and first step.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper">
    <StepperSteps>
        <StepperStep Label="Start"></StepperStep>
        <StepperStep Label="Middle"></StepperStep>
        <StepperStep Label="End"></StepperStep>
    </StepperSteps>
</SfStepper>

<button @onclick="ResetProcess">Reset</button>

@code {
    private SfStepper stepper;
    
    private async Task ResetProcess()
    {
        await stepper.ResetAsync();
    }
}
```

#### RefreshProgressbarAsync

Adjusts the progress bar when the container is resized.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
    </StepperSteps>
</SfStepper>

<button @onclick="HandleResize">Refresh Layout</button>

@code {
    private SfStepper stepper;
    
    private async Task HandleResize()
    {
        // After container size changes
        await stepper.RefreshProgressbarAsync();
    }
}
```

---

### Events

| Event | Arguments | Description | Reference |
|-------|-----------|-------------|-----------|
| `Created` | None | Fires when component rendering is complete | [events.md#created-event](events.md#created-event) |
| `StepChanged` | `StepperChangedEventArgs` | Fires after active step has changed | [events.md#stepchanged-event](events.md#stepchanged-event) |
| `StepChanging` | `StepperChangeEventArgs` | Fires before step change (can be cancelled) | [events.md#stepchanging-event](events.md#stepchanging-event) |
| `StepClicked` | `StepperClickedEventArgs` | Fires when a step is clicked | [events.md#stepclicked-event](events.md#stepclicked-event) |
| `StepRendered` | `StepperRenderedEventArgs` | Fires after a step is rendered | [events.md#steprendered-event](events.md#steprendered-event) |

> **Note:** For detailed examples of all events listed above, refer to [events.md](events.md).

---

## StepperStep Component

Individual step within the SfStepper container.

### StepperStep Properties

| Property | Type | Default | Description | Reference |
|----------|------|---------|-------------|-----------|
| `CssClass` | string | Empty | Custom CSS class for the step | [templates-and-styling.md#using-cssclass-property](templates-and-styling.md#using-cssclass-property) |
| `Disabled` | bool | false | Disables the step when true | [step-types.md#disabled-steps](step-types.md#disabled-steps) |
| `IconCss` | string | Empty | CSS class for step icon | [View Details](#stepper-step-iconcss) |
| `IsValid` | bool? | null | Validation state (true/false/null) | [advanced-features.md#validation-states](advanced-features.md#validation-states) |
| `Label` | string | Empty | Display label for the step | [View Details](#stepper-step-label) |
| `Optional` | bool | false | Marks step as optional when true | [step-types.md#optional-steps](step-types.md#optional-steps) |
| `Status` | `StepperStatus` | NotStarted | Current status of the step | [step-types.md#step-status](step-types.md#step-status) |
| `Text` | string | Empty | Additional text description | [View Details](#stepper-step-text) |

> **Note:** For detailed examples of StepperStep properties like `CssClass`, `Disabled`, `IsValid`, `Optional`, and `Status`, refer to their linked documentation files.

#### StepperStep IconCss

Display an icon for each step.

```csharp
<StepperStep Label="Personal" IconCss="sf-icon-user"></StepperStep>
<StepperStep Label="Address" IconCss="sf-icon-location"></StepperStep>
<StepperStep Label="Payment" IconCss="sf-icon-credit"></StepperStep>
```

#### StepperStep Label

Set the primary label text for the step.

```csharp
<StepperStep Label="Checkout"></StepperStep>
```

#### StepperStep Text

Add supplementary text description below the label.

```csharp
<StepperStep Label="Shipping" Text="Select delivery address"></StepperStep>
```

---

## StepperAnimationSettings Component

Configure animation behavior for the stepper.

### Properties

| Property | Type | Default | Description | Reference |
|----------|------|---------|-------------|-----------|
| `Enable` | bool | true | Enables/disables animations | [animations-and-globalization.md#animation-settings](animations-and-globalization.md#animation-settings) |
| `Duration` | double | 2000 | Animation duration in milliseconds | [animations-and-globalization.md#animation-properties](animations-and-globalization.md#animation-properties) |
| `Delay` | double | 0 | Delay before animation starts in milliseconds | [animations-and-globalization.md#animation-properties](animations-and-globalization.md#animation-properties) |

---

## Enumeration Types

### StepperOrientation

```csharp
public enum StepperOrientation
{
    Horizontal,  // Steps displayed in horizontal layout
    Vertical     // Steps displayed in vertical layout
}
```

### StepperStatus

```csharp
public enum StepperStatus
{
    NotStarted,  // Step has not been started
    InProgress,  // Step is currently active
    Completed    // Step has been completed
}
```

### StepperLabelPosition

```csharp
public enum StepperLabelPosition
{
    Bottom,  // Labels appear below step indicators
    Top,     // Labels appear above step indicators
    Start,   // Labels appear at the start (left for LTR)
    End      // Labels appear at the end (right for LTR)
}
```

### StepperType

```csharp
public enum StepperType
{
    Default,    // Display both indicators and labels
    Indicator,  // Display only step indicators
    Label       // Display only step labels
}
```

---

## Event Arguments Reference

### StepperChangedEventArgs

Passed to `StepChanged` and as base for `StepperChangeEventArgs`.

**Properties:**
- `ActiveStep` (int): Current active step index
- `PreviousStep` (int): Previous step index
- `IsInteracted` (bool): True if change was user-initiated

### StepperChangeEventArgs

Passed to `StepChanging` event.

**Properties:**
- `ActiveStep` (int): Step being navigated to
- `PreviousStep` (int): Current step
- `IsInteracted` (bool): True if change was user-initiated
- `Cancel` (bool): Set to true to cancel navigation

### StepperClickedEventArgs

Passed to `StepClicked` event.

**Properties:**
- `ActiveStep` (int): Clicked step index
- `PreviousStep` (int): Previously active step index

### StepperRenderedEventArgs

Passed to `StepRendered` event.

**Properties:**
- `Index` (int): The index of the rendered step.
- `Step` (int): The step element that is being rendered.

---

## Complete Implementation Example

```csharp
@using Syncfusion.Blazor.Navigations

<div class="stepper-container">
    <SfStepper @ref="stepper"
               ActiveStep="activeStep"
               Linear="isLinear"
               Orientation="StepperOrientation.Horizontal"
               LabelPosition="StepperLabelPosition.Bottom"
               ShowTooltip="true"
               StepChanging="OnStepChanging"
               StepChanged="OnStepChanged"
               StepClicked="OnStepClicked">
        
        <StepperSteps>
            <StepperStep Label="Cart" IconCss="sf-icon-cart" Text="Review items"></StepperStep>
            <StepperStep Label="Shipping" IconCss="sf-icon-truck" Text="Enter address"></StepperStep>
            <StepperStep Label="Payment" IconCss="sf-icon-card" Text="Payment details"></StepperStep>
            <StepperStep Label="Confirmation" IconCss="sf-icon-check" Text="Order placed"></StepperStep>
        </StepperSteps>
        
        <StepperAnimationSettings Enable="true" Duration="1000" Delay="100"></StepperAnimationSettings>
        
        <TooltipTemplate>
            <div class="tooltip-content">
                <strong>@context.Label</strong>
                <p>@context.Text</p>
            </div>
        </TooltipTemplate>
    </SfStepper>
</div>

<div class="button-group">
    <button @onclick="PreviousStep" disabled="@(activeStep == 0)">Previous</button>
    <button @onclick="NextStep" disabled="@(activeStep >= 3)">Next</button>
    <button @onclick="ResetStepper">Reset</button>
</div>

<div class="status-info">
    <p>Current Step: @activeStep</p>
    <p>@statusMessage</p>
</div>

@code {
    private SfStepper stepper;
    private int activeStep = 0;
    private bool isLinear = false;
    private string statusMessage = "";

    private async Task NextStep()
    {
        if (activeStep < 3)
        {
            await stepper.NextStepAsync();
        }
    }

    private async Task PreviousStep()
    {
        if (activeStep > 0)
        {
            await stepper.PreviousStepAsync();
        }
    }

    private async Task ResetStepper()
    {
        await stepper.ResetAsync();
        activeStep = 0;
        statusMessage = "Stepper has been reset";
    }

    private void OnStepChanging(StepperChangeEventArgs args)
    {
        statusMessage = $"Attempting to move from step {args.PreviousStep} to step {args.ActiveStep}";
        
        // Validate before allowing step change
        if (args.ActiveStep > args.PreviousStep && !ValidateCurrentStep(args.PreviousStep))
        {
            args.Cancel = true;
            statusMessage = "Cannot proceed - validation failed!";
        }
    }

    private void OnStepChanged(StepperChangedEventArgs args)
    {
        activeStep = args.ActiveStep;
        statusMessage = $"Successfully moved to step {args.ActiveStep}";
    }

    private void OnStepClicked(StepperClickedEventArgs args)
    {
        statusMessage = $"Clicked on step {args.ActiveStep}";
    }

    private bool ValidateCurrentStep(int stepIndex)
    {
        return true; // Implement actual validation logic
    }
}

<style>
    .stepper-container {
        padding: 20px;
        background: #f5f5f5;
        border-radius: 4px;
    }

    .button-group {
        margin-top: 20px;
        display: flex;
        gap: 10px;
    }

    .button-group button {
        padding: 8px 16px;
        background: #007bff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    .button-group button:disabled {
        background: #ccc;
        cursor: not-allowed;
    }

    .status-info {
        margin-top: 20px;
        padding: 10px;
        background: white;
        border-radius: 4px;
    }

    .tooltip-content {
        padding: 10px;
        background: #333;
        color: white;
        border-radius: 4px;
    }
</style>
```

---

## Namespace and Usage

To use the Stepper component, include the following namespace in your Blazor component:

```csharp
@using Syncfusion.Blazor.Navigations
```

In your `Program.cs`, register Syncfusion services:

```csharp
builder.Services.AddSyncfusionBlazor();
```

---

## Related References

- **[Getting Started Guide](getting-started.md)** — Installation, setup, namespaces, and basic examples
- **[Step Types Guide](step-types.md)** — Step type variations, label positioning, optional/disabled steps, active step control, status, and read-only mode
- **[Orientations and Layouts](orientations-and-layouts.md)** — Horizontal/vertical orientations, linear/non-linear flows, responsive layouts, and layout combinations
- **[Events Reference](events.md)** — All event documentation (Created, StepChanged, StepChanging, StepClicked, StepRendered) with event args and patterns
- **[Templates and Styling](templates-and-styling.md)** — Custom templates, CSS customization, CssClass property, theme support, and advanced examples
- **[Advanced Features](advanced-features.md)** — Step validation, tooltips, custom templates, completion tracking, conditional display, and error handling
- **[Animations and Globalization](animations-and-globalization.md)** — Animation settings, localization support, RTL implementation, and performance tips
