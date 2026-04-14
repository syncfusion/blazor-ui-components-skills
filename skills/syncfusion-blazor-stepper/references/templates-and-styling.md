# Templates and Styling

## Table of Contents

- [Custom Step Templates](#custom-step-templates)
  - [Template Basics](#template-basics)
  - [Custom Styling with Templates](#custom-styling-with-templates)
  - [Icon and Label Customization](#icon-and-label-customization)
- [CSS Styling](#css-styling)
  - [Built-in CSS Classes](#built-in-css-classes)
  - [Custom CSS Classes](#custom-css-classes)
- [Theme Customization](#theme-customization)
- [Custom CSS Styling](#custom-css-styling)

## Custom Step Templates

The Stepper component supports custom templates for advanced layouts. Use the `Template` tag directive to customize how steps are rendered.

### Template Basics

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper ActiveStep="1">
    <ChildContent>
        <StepperSteps>
            <StepperStep Label="PowerPoint" IconCss="sf-icon-powerpoint"></StepperStep>
            <StepperStep Label="Presentation" IconCss="sf-icon-projector"></StepperStep>
            <StepperStep Label="Backup" IconCss="sf-icon-onedrive"></StepperStep>
        </StepperSteps>
    </ChildContent>
    <Template>
        <div class="template-content">
            <span class="@context.IconCss"></span><br>
            <span class="e-label">@context.Label</span>
        </div>
    </Template>
</SfStepper>

<style>
    .template-content {
        background: #fff;
        width: 65px;
        text-align: center;
        padding: 10px;
    }
</style>
```

### Template Context Properties

The template context provides access to the current step's properties:

- `@context.Label` - Step label text
- `@context.Text` - Step text content
- `@context.IconCss` - Icon CSS class
- `@context.Optional` - Is step optional
- `@context.Disabled` - Is step disabled
- `@context.Status` - Current step status

## CSS Styling and Customization

### Using CssClass Property

Apply custom CSS to individual steps using the `CssClass` property:

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Required" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Optional" IconCss="sf-icon-transport" Optional="true" CssClass="optional-step"></StepperStep>
        <StepperStep Label="Important" IconCss="sf-icon-payment" CssClass="important-step"></StepperStep>
        <StepperStep Label="Final" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>

<style>
    .optional-step .e-step-label-optional {
        font-style: italic;
        font-weight: 900;
        color: #299100;
    }
    
    .important-step .e-step-indicator {
        background-color: #ff6b6b !important;
        color: white;
    }
</style>
```

### Stepper-Level CSS Classes

Customize the entire stepper appearance using CSS selectors:

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3" IconCss="sf-icon-payment"></StepperStep>
    </StepperSteps>
</SfStepper>

<style>
    /* Stepper progress bar customization */
    .e-stepper .e-stepper-progressbar {
        height: 3px;
        top: 25px;
    }
    
    .e-stepper .e-stepper-progressbar .e-progressbar-value {
        background-color: #27d96d;
    }
    
    /* Stepper step status customization */
    .e-stepper .e-step-completed * {
        color: #19cd60;
    }
    
    .e-stepper .e-step-inprogress * {
        color: #3479f3;
    }
    
    .e-stepper .e-step-notstarted * {
        color: #bdbdbd;
    }
    
    /* Individual step styling */
    .e-stepper .e-step-indicator {
        width: 40px;
        height: 40px;
        font-size: 18px;
    }
    
    .e-stepper .e-step-label {
        font-weight: 600;
        font-size: 14px;
    }
</style>
```

### Progressive Styling

Create visual hierarchy with progressive styling:

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper ID="progressiveStepper">
    <StepperSteps>
        <StepperStep Label="Getting Started"></StepperStep>
        <StepperStep Label="Configuration"></StepperStep>
        <StepperStep Label="Integration"></StepperStep>
        <StepperStep Label="Deployment"></StepperStep>
    </StepperSteps>
</SfStepper>

<style>
    #progressiveStepper .e-step {
        margin: 0 10px;
        transition: all 0.3s ease;
    }
    
    #progressiveStepper .e-step-completed .e-step-indicator {
        background-color: #4CAF50;
        box-shadow: 0 0 8px rgba(76, 175, 80, 0.4);
        transform: scale(1.1);
    }
    
    #progressiveStepper .e-step-inprogress .e-step-indicator {
        background-color: #2196F3;
        box-shadow: 0 0 12px rgba(33, 150, 243, 0.6);
        animation: pulse 1s infinite;
    }
    
    #progressiveStepper .e-step-notstarted .e-step-indicator {
        background-color: #bdbdbd;
        opacity: 0.6;
    }
    
    @keyframes pulse {
        0%, 100% { transform: scale(1); }
        50% { transform: scale(1.05); }
    }
</style>
```

## Theme Customization

### Theme Variables

Override Syncfusion theme CSS variables for custom styling:

```csharp
<style>
    :root {
        /* Primary color */
        --e-primary: #1976D2;
        /* Text color */
        --e-text-color: #333333;
        /* Border color */
        --e-border-color: #CCCCCC;
    }
    
    .e-stepper .e-step-completed .e-step-indicator {
        background-color: var(--e-primary);
    }
</style>
```

### Multiple Theme Support

Switch between themes dynamically:

```csharp
@using Syncfusion.Blazor.Navigations

<div>
    <button @onclick="@(() => ApplyTheme("material"))">Material</button>
    <button @onclick="@(() => ApplyTheme("bootstrap"))">Bootstrap</button>
    <button @onclick="@(() => ApplyTheme("fabric"))">Fabric</button>
</div>

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private void ApplyTheme(string themeName)
    {
        // Theme switching logic
        Console.WriteLine($"Applied {themeName} theme");
    }
}
```

## Advanced Customization Examples

### Stepper with Icons and Status

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" ActiveStep="@currentStep" StepChanged="OnStepChanged">
    <StepperSteps>
        <StepperStep Label="Cart" IconCss="sf-icon-cart" CssClass="@(currentStep >= 0 ? "completed" : "pending")"></StepperStep>
        <StepperStep Label="Address" IconCss="sf-icon-user" CssClass="@(currentStep >= 1 ? "completed" : currentStep == 1 ? "current" : "pending")"></StepperStep>
        <StepperStep Label="Payment" IconCss="sf-icon-payment" CssClass="@(currentStep == 2 ? "current" : "pending")"></StepperStep>
        <StepperStep Label="Confirmation" IconCss="sf-icon-success" CssClass="@(currentStep == 3 ? "current" : "pending")"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private SfStepper stepper;
    private int currentStep = 0;
    
    private void OnStepChanged(StepperChangedEventArgs args)
    {
        currentStep = args.ActiveStep;
    }
}

<style>
    .e-step.completed .e-step-indicator {
        background-color: #4CAF50 !important;
        color: white;
    }
    
    .e-step.current .e-step-indicator {
        background-color: #2196F3 !important;
        color: white;
        box-shadow: 0 0 10px rgba(33, 150, 243, 0.5);
    }
    
    .e-step.pending .e-step-indicator {
        background-color: #E0E0E0;
        color: #999;
    }
    
    .e-step.completed .e-step-label {
        color: #4CAF50;
        font-weight: 600;
    }
    
    .e-step.current .e-step-label {
        color: #2196F3;
        font-weight: 700;
    }
</style>
```

### Vertical Stepper with Custom Layout

```csharp
@using Syncfusion.Blazor.Navigations

<div class="custom-stepper-wrapper">
    <SfStepper Orientation="StepperOrientation.Vertical" ActiveStep="@currentStep">
        <StepperSteps>
            <StepperStep Label="Personal Information" IconCss="sf-icon-user"></StepperStep>
            <StepperStep Label="Address Details" IconCss="sf-icon-location"></StepperStep>
            <StepperStep Label="Payment Method" IconCss="sf-icon-payment"></StepperStep>
            <StepperStep Label="Review & Submit" IconCss="sf-icon-success"></StepperStep>
        </StepperSteps>
    </SfStepper>
    
    <div class="stepper-content">
        <!-- Step content here -->
    </div>
</div>

@code {
    private int currentStep = 0;
}

<style>
    .custom-stepper-wrapper {
        display: flex;
        gap: 30px;
        padding: 20px;
    }
    
    .e-stepper {
        flex: 0 0 250px;
    }
    
    .stepper-content {
        flex: 1;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 4px;
        background-color: #fafafa;
    }
</style>
```

## Common CSS Selectors

| Selector | Description |
|----------|-------------|
| `.e-stepper` | Main stepper container |
| `.e-stepper-steps` | Steps wrapper |
| `.e-step` | Individual step |
| `.e-step-indicator` | Step number/icon |
| `.e-step-label` | Step label text |
| `.e-step-completed` | Completed step |
| `.e-step-inprogress` | In-progress step |
| `.e-step-notstarted` | Not started step |
| `.e-stepper-progressbar` | Progress bar |
| `.e-progressbar-value` | Progress bar fill |
