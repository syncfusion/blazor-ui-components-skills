# Events and Interactions

## Table of Contents

- [Created Event](#created-event)
- [StepChanged Event](#stepchanged-event)
- [StepChanging Event](#stepchanging-event)
- [StepClicked Event](#stepclicked-event)
- [StepRendered Event](#steprendered-event)
- [Event Handling Patterns](#event-handling-patterns)

## Created Event

The `Created` event fires when the Stepper component has finished rendering.

**Usage**: Initialize data or perform setup operations after the component is ready.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper Created="OnStepperCreated">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private void OnStepperCreated()
    {
        Console.WriteLine("Stepper component has been created and rendered.");
        // Perform initialization tasks here
    }
}
```

**Event Args**: None (void callback)

## StepChanged Event

The `StepChanged` event fires after the active step has been changed successfully.

**Usage**: Respond to step navigation, update UI, or trigger data loading for the new step.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" StepChanged="OnStepChanged">
    <StepperSteps>
        <StepperStep Label="Cart"></StepperStep>
        <StepperStep Label="Address"></StepperStep>
        <StepperStep Label="Payment"></StepperStep>
    </StepperSteps>
</SfStepper>

<div>Current step: @currentStepIndex</div>

@code {
    private SfStepper stepper;
    private int currentStepIndex = 0;
    
    private void OnStepChanged(StepperChangedEventArgs args)
    {
        currentStepIndex = args.ActiveStep;
        Console.WriteLine($"Active step changed to: {currentStepIndex}");
        // Load data for the new step
        LoadStepData(currentStepIndex);
    }
    
    private void LoadStepData(int stepIndex)
    {
        // Load step-specific data here
    }
}
```

**Event Args (`StepperChangedEventArgs`):**
- `ActiveStep` (int): Current active step index
- `PreviousStep` (int): Previous step index
- `IsInteracted` (bool): True if change was user-initiated

## StepChanging Event

The `StepChanging` event fires before the active step is about to change. You can cancel the navigation by setting `Cancel` to `true`.

**Usage**: Validate step completion or prevent navigation based on conditions.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepChanging="OnStepChanging">
    <StepperSteps>
        <StepperStep Label="Form Step"></StepperStep>
        <StepperStep Label="Review Step"></StepperStep>
        <StepperStep Label="Submit Step"></StepperStep>
    </StepperSteps>
</SfStepper>

<div>@validationMessage</div>

@code {
    private string validationMessage = "";
    
    private void OnStepChanging(StepperChangeEventArgs args)
    {
        // Prevent moving forward if validation fails
        if (args.ActiveStep > args.PreviousStep && !IsCurrentStepValid())
        {
            args.Cancel = true;
            validationMessage = "Please complete all required fields before proceeding.";
            Console.WriteLine("Navigation cancelled due to validation failure.");
        }
        else
        {
            validationMessage = "";
            Console.WriteLine($"Moving from step {args.PreviousStep} to step {args.ActiveStep}");
        }
    }
    
    private bool IsCurrentStepValid()
    {
        // Implement validation logic
        return true;
    }
}
```

**Event Args (`StepperChangeEventArgs`):**
- `ActiveStep` (int): Step being navigated to
- `PreviousStep` (int): Current step
- `IsInteracted` (bool): True if change was user-initiated
- `Cancel` (bool): Set to true to cancel navigation

## StepClicked Event

The `StepClicked` event fires when a step is clicked.

**Usage**: Track user interactions or provide feedback on step selection attempts.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepClicked="OnStepClicked">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

<div>Last clicked step: @lastClickedStep</div>

@code {
    private int lastClickedStep = -1;
    
    private void OnStepClicked(StepperClickedEventArgs args)
    {
        lastClickedStep = args.ActiveStep;
        Console.WriteLine($"Step {args.Step} was clicked.");
    }
}
```

**Event Args (`StepperClickedEventArgs`):**
- `ActiveStep` (int): Clicked step index
- `PreviousStep` (int): Previously active step index

## StepRendered Event

The `StepRendered` event fires after each step is rendered.

**Usage**: Apply custom styling or perform operations on specific steps after rendering.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepRendered="OnStepRendered">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

<div>Steps rendered: @stepsRendered</div>

@code {
    private int stepsRendered = 0;
    
    private void OnStepRendered(StepperRenderedEventArgs args)
    {
        stepsRendered++;
        Console.WriteLine($"Step {args.Step} has been rendered.");
    }
}
```

**Event Args (`StepperRenderedEventArgs`):**
- `Index` (int): The index of the rendered step.
- `Step` (int): The step element that is being rendered.

## Event Handling Patterns

### Complete Workflow Example

```csharp
@using Syncfusion.Blazor.Navigations

<div class="checkout-wizard">
    <SfStepper @ref="stepper" 
               Linear="true"
               ActiveStep="@currentStep"
               StepChanging="@OnStepChanging"
               StepChanged="@OnStepChanged"
               StepClicked="@OnStepClicked">
        <StepperSteps>
            <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
            <StepperStep Label="Shipping" IconCss="sf-icon-transport"></StepperStep>
            <StepperStep Label="Payment" IconCss="sf-icon-payment"></StepperStep>
            <StepperStep Label="Review" IconCss="sf-icon-success"></StepperStep>
        </StepperSteps>
    </SfStepper>
    
    <div class="step-content">
        @if (currentStep == 0)
        {
            <div>
                <h3>Shopping Cart</h3>
                <p>Review your items</p>
            </div>
        }
        else if (currentStep == 1)
        {
            <div>
                <h3>Shipping Address</h3>
                <p>Enter shipping details</p>
            </div>
        }
        else if (currentStep == 2)
        {
            <div>
                <h3>Payment Information</h3>
                <p>Enter payment details</p>
            </div>
        }
        else if (currentStep == 3)
        {
            <div>
                <h3>Order Review</h3>
                <p>Confirm your order</p>
            </div>
        }
    </div>
    
    <div class="validation-message" style="@(string.IsNullOrEmpty(validationMessage) ? "display:none;" : "")">
        @validationMessage
    </div>
</div>

@code {
    private SfStepper stepper;
    private int currentStep = 0;
    private string validationMessage = "";
    private bool[] stepValidation = new bool[] { true, false, false, true };
    
    private void OnStepChanging(StepperChangeEventArgs args)
    {
        validationMessage = "";
        
        // Validate only when moving forward
        if (args.ActiveStep > args.PreviousStep)
        {
            if (!stepValidation[args.PreviousStep])
            {
                args.Cancel = true;
                validationMessage = $"Please complete Step {args.PreviousStep + 1} before proceeding.";
            }
        }
    }
    
    private void OnStepChanged(StepperChangedEventArgs args)
    {
        currentStep = args.ActiveStep;
        Console.WriteLine($"Successfully moved to step {currentStep + 1}");
    }
    
    private void OnStepClicked(StepperClickedEventArgs args)
    {
        Console.WriteLine($"User clicked on step {args.Step + 1}");
    }
}

<style>
    .checkout-wizard {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
    }
    
    .step-content {
        margin-top: 40px;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 4px;
        min-height: 150px;
    }
    
    .validation-message {
        margin-top: 20px;
        padding: 15px;
        background-color: #fee;
        border: 1px solid #fcc;
        border-radius: 4px;
        color: #c33;
    }
</style>
```

### Async Event Handling

```csharp
private async Task OnStepChangedAsync(StepperChangedEventArgs args)
{
    // Load data asynchronously for the new step
    await LoadStepDataAsync(args.ActiveStep);
}

private async Task LoadStepDataAsync(int stepIndex)
{
    // Simulate API call
    await Task.Delay(500);
    Console.WriteLine($"Data loaded for step {stepIndex}");
}
```

### Common Patterns Summary

| Pattern | Event | Use Case |
|---------|-------|----------|
| **Validation** | `StepChanging` | Prevent navigation until conditions are met |
| **Data Loading** | `StepChanged` | Load step-specific data after navigation |
| **UI Updates** | `StepChanged` | Update UI elements based on active step |
| **Tracking** | `StepClicked` | Log user interactions and navigation attempts |
| **Initialization** | `Created` | Initialize Stepper-dependent resources |
| **Custom Rendering** | `StepRendered` | Apply custom styles or behaviors to steps |
