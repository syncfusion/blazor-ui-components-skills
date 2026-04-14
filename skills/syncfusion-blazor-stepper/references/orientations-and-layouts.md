# Orientations and Layouts

## Table of Contents

- [Horizontal Orientation](#horizontal-orientation)
  - [Basic Horizontal Stepper](#basic-horizontal-stepper)
  - [Responsive Horizontal Layout](#responsive-horizontal-layout)
- [Vertical Orientation](#vertical-orientation)
  - [Basic Vertical Stepper](#basic-vertical-stepper)
  - [Vertical Stepper with Content Panel](#vertical-stepper-with-content-panel)
- [Linear Flow](#linear-flow)
  - [Basic Linear Flow](#basic-linear-flow)
  - [Linear Flow with Validation](#linear-flow-with-validation)
- [Non-Linear Flow](#non-linear-flow)
  - [Basic Non-Linear Flow](#basic-non-linear-flow)
  - [Non-Linear with Skip Navigation](#non-linear-with-skip-navigation)
- [Responsive Layout Strategy](#responsive-layout-strategy)
  - [Mobile-First Responsive Layout](#mobile-first-responsive-layout)
  - [Adaptive Label Display](#adaptive-label-display)
- [Layout Combinations](#layout-combinations)
  - [Horizontal Stepper with Vertical Content](#horizontal-stepper-with-vertical-content)

## Horizontal Orientation

Horizontal orientation is the default layout for the Stepper component, displaying steps from left to right.

### Basic Horizontal Stepper

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper Orientation="StepperOrientation.Horizontal">
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Step 4" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Desktop applications, wide screens, horizontal workflows.

### Responsive Horizontal Layout

```csharp
@using Syncfusion.Blazor.Navigations

<div class="stepper-container">
    <SfStepper Orientation="StepperOrientation.Horizontal">
        <StepperSteps>
            <StepperStep Label="Personal"></StepperStep>
            <StepperStep Label="Address"></StepperStep>
            <StepperStep Label="Payment"></StepperStep>
            <StepperStep Label="Confirm"></StepperStep>
        </StepperSteps>
    </SfStepper>
</div>

<style>
    .stepper-container {
        width: 100%;
        overflow-x: auto;
    }
    
    /* Responsive behavior */
    @media (max-width: 768px) {
        .stepper-container {
            transform: scale(0.9);
            transform-origin: left top;
        }
    }
    
    @media (max-width: 480px) {
        .stepper-container {
            transform: scale(0.75);
            transform-origin: left top;
        }
    }
</style>
```

## Vertical Orientation

Vertical orientation displays steps from top to bottom, ideal for mobile devices or sidebar layouts.

### Basic Vertical Stepper

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper Orientation="StepperOrientation.Vertical">
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Step 4" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Mobile layouts, sidebar navigation, long workflows.

### Vertical Stepper with Content Panel

```csharp
@using Syncfusion.Blazor.Navigations

<div class="vertical-layout">
    <div class="stepper-sidebar">
        <SfStepper @ref="stepper" 
                   Orientation="StepperOrientation.Vertical"
                   ActiveStep="@currentStep"
                   StepChanged="OnStepChanged">
            <StepperSteps>
                <StepperStep Label="Personal Information" IconCss="sf-icon-user"></StepperStep>
                <StepperStep Label="Address Details" IconCss="sf-icon-location"></StepperStep>
                <StepperStep Label="Payment Method" IconCss="sf-icon-payment"></StepperStep>
                <StepperStep Label="Review & Submit" IconCss="sf-icon-success"></StepperStep>
            </StepperSteps>
        </SfStepper>
    </div>
    
    <div class="content-panel">
        @if (currentStep == 0)
        {
            <h3>Personal Information</h3>
            <div class="form-fields">
                <input placeholder="Full Name" />
                <input placeholder="Email Address" />
            </div>
        }
        else if (currentStep == 1)
        {
            <h3>Address Details</h3>
            <div class="form-fields">
                <input placeholder="Street Address" />
                <input placeholder="City" />
            </div>
        }
        else if (currentStep == 2)
        {
            <h3>Payment Method</h3>
            <div class="form-fields">
                <input placeholder="Card Number" />
                <input placeholder="Expiry Date" />
            </div>
        }
        else if (currentStep == 3)
        {
            <h3>Review & Submit</h3>
            <p>Please review your information and click Submit to complete.</p>
        }
    </div>
</div>

@code {
    private SfStepper stepper;
    private int currentStep = 0;
    
    private void OnStepChanged(StepperChangedEventArgs args)
    {
        currentStep = args.ActiveStep;
    }
}

<style>
    .vertical-layout {
        display: flex;
        gap: 20px;
        height: 100vh;
    }
    
    .stepper-sidebar {
        flex: 0 0 250px;
        padding: 20px;
        background-color: #f5f5f5;
        border-right: 1px solid #ddd;
        overflow-y: auto;
    }
    
    .content-panel {
        flex: 1;
        padding: 40px;
        overflow-y: auto;
    }
    
    .content-panel h3 {
        margin-top: 0;
        margin-bottom: 20px;
        color: #333;
    }
    
    .form-fields {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }
    
    .form-fields input {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 4px;
        font-size: 14px;
    }
    
    /* Responsive */
    @media (max-width: 768px) {
        .vertical-layout {
            flex-direction: column;
            height: auto;
        }
        
        .stepper-sidebar {
            flex: none;
            border-right: none;
            border-bottom: 1px solid #ddd;
        }
    }
</style>
```

## Linear Flow

Linear flow enforces sequential step progression. Users can only move forward through completed steps or backward to previous steps, but cannot skip steps.

### Basic Linear Flow

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper Linear="true">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
        <StepperStep Label="Step 4"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Wizards, forms where sequence matters, mandatory workflows.

### Linear Flow with Validation

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" Linear="true" StepChanging="@ValidateStep">
    <StepperSteps>
        <StepperStep Label="Basic Information"></StepperStep>
        <StepperStep Label="Contact Details"></StepperStep>
        <StepperStep Label="Verification"></StepperStep>
        <StepperStep Label="Confirmation"></StepperStep>
    </StepperSteps>
</SfStepper>

<div class="step-content">
    @if (currentStep == 0)
    {
        <div>
            <input @bind="basicInfo" placeholder="Enter basic info" />
        </div>
    }
    else if (currentStep == 1)
    {
        <div>
            <input @bind="contactInfo" placeholder="Enter contact info" />
        </div>
    }
</div>

@code {
    private SfStepper stepper;
    private int currentStep = 0;
    private string basicInfo = "";
    private string contactInfo = "";
    
    private void ValidateStep(StepperChangeEventArgs args)
    {
        // Only allow forward navigation if current step is valid
        if (args.ActiveStep > args.PreviousStep)
        {
            bool isValid = args.PreviousStep switch
            {
                0 => !string.IsNullOrEmpty(basicInfo),
                1 => !string.IsNullOrEmpty(contactInfo),
                _ => true
            };
            
            args.Cancel = !isValid;
        }
    }
}
```

## Non-Linear Flow

Non-linear flow allows users to navigate to any step without restrictions. Users can jump between steps and explore the workflow in any order.

### Basic Non-Linear Flow

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper Linear="false">
    <StepperSteps>
        <StepperStep Label="Overview"></StepperStep>
        <StepperStep Label="Features"></StepperStep>
        <StepperStep Label="Pricing"></StepperStep>
        <StepperStep Label="Support"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Documentation navigation, exploratory workflows, settings pages.

### Non-Linear with Skip Navigation

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" Linear="false">
    <StepperSteps>
        <StepperStep Label="Quick Start"></StepperStep>
        <StepperStep Label="Advanced Configuration"></StepperStep>
        <StepperStep Label="Integration"></StepperStep>
        <StepperStep Label="API Reference"></StepperStep>
    </StepperSteps>
</SfStepper>

<div class="navigation-buttons">
    <button @onclick="@(() => JumpToStep(0))">Quick Start</button>
    <button @onclick="@(() => JumpToStep(1))">Configuration</button>
    <button @onclick="@(() => JumpToStep(2))">Integration</button>
    <button @onclick="@(() => JumpToStep(3))">API Docs</button>
</div>

@code {
    private SfStepper stepper;
    
    private async Task JumpToStep(int stepIndex)
    {
        // Set the active step directly by incrementally navigating
        while (stepper.ActiveStep < stepIndex)
        {
            await stepper.NextStepAsync();
        }
        while (stepper.ActiveStep > stepIndex)
        {
            await stepper.PreviousStepAsync();
        }
    }
}

<style>
    .navigation-buttons {
        display: flex;
        gap: 10px;
        margin-top: 20px;
        flex-wrap: wrap;
    }
    
    .navigation-buttons button {
        padding: 8px 16px;
        background-color: #2196F3;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 600;
    }
    
    .navigation-buttons button:hover {
        background-color: #1976D2;
    }
</style>
```

## Responsive Layout Strategy

### Mobile-First Responsive Layout

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" Orientation="@stepperOrientation">
    <StepperSteps>
        <StepperStep Label="Cart"></StepperStep>
        <StepperStep Label="Shipping"></StepperStep>
        <StepperStep Label="Payment"></StepperStep>
        <StepperStep Label="Review"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private SfStepper stepper;
    private StepperOrientation stepperOrientation = StepperOrientation.Vertical;
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Handle window resize to switch orientation
            await HandleWindowResize();
        }
    }
    
    private async Task HandleWindowResize()
    {
        // In a real scenario, you'd use JavaScript interop to get window size
        // For now, we'll assume vertical on mobile, horizontal on desktop
        stepperOrientation = StepperOrientation.Vertical; // Default to vertical
        await InvokeAsync(StateHasChanged);
    }
}

<style>
    /* Mobile (vertical by default) */
    @media (max-width: 768px) {
        .e-stepper {
            width: 100%;
        }
    }
    
    /* Tablet and above (switch to horizontal) */
    @media (min-width: 769px) {
        /* Stepper will automatically be horizontal */
    }
</style>
```

### Adaptive Label Display

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" 
           Orientation="@stepperOrientation"
           LabelPosition="@labelPosition">
    <StepperSteps>
        <StepperStep Label="Order Details"></StepperStep>
        <StepperStep Label="Shipping Address"></StepperStep>
        <StepperStep Label="Payment Information"></StepperStep>
        <StepperStep Label="Order Summary"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private SfStepper stepper;
    private StepperOrientation stepperOrientation = StepperOrientation.Horizontal;
    private StepperLabelPosition labelPosition = StepperLabelPosition.Bottom;
    
    protected override void OnInitialized()
    {
        // Set initial orientation based on screen size
        UpdateOrientation();
    }
    
    private void UpdateOrientation()
    {
        // In production, use JavaScript interop to get window size
        // For demo, assume desktop
        stepperOrientation = StepperOrientation.Horizontal;
        labelPosition = StepperLabelPosition.Bottom;
    }
}

<style>
    /* Mobile: Vertical stepper, labels on side */
    @media (max-width: 480px) {
        .e-stepper {
            width: 100%;
        }
        
        .e-step {
            margin: 15px 0;
        }
    }
    
    /* Tablet: Horizontal stepper, labels at bottom */
    @media (min-width: 481px) and (max-width: 768px) {
        .e-stepper-progressbar {
            display: none;
        }
    }
    
    /* Desktop: Full horizontal layout */
    @media (min-width: 769px) {
        .e-stepper {
            display: flex;
            justify-content: center;
        }
    }
</style>
```

## Layout Combinations

### Horizontal Stepper with Vertical Content

```csharp
@using Syncfusion.Navigations

<div class="layout-container">
    <SfStepper Orientation="StepperOrientation.Horizontal">
        <StepperSteps>
            <StepperStep Label="Step 1"></StepperStep>
            <StepperStep Label="Step 2"></StepperStep>
            <StepperStep Label="Step 3"></StepperStep>
        </StepperSteps>
    </SfStepper>
    
    <div class="vertical-content">
        <div class="content-section">Content for Step 1</div>
        <div class="content-section">Content for Step 2</div>
        <div class="content-section">Content for Step 3</div>
    </div>
</div>

<style>
    .layout-container {
        display: flex;
        flex-direction: column;
        gap: 30px;
    }
    
    .vertical-content {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }
    
    .content-section {
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 4px;
        background-color: #fafafa;
    }
</style>
```
