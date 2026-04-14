# Step Types and Configuration

## Table of Contents

- [Default Type](#default-type)
- [Label Type](#label-type)
- [Indicator Type](#indicator-type)
- [Label Positioning](#label-positioning)
- [Optional and Disabled Steps](#optional-and-disabled-steps)
- [Active Step Control](#active-step-control)
- [Step Status](#step-status)
- [Read-Only Mode](#read-only-mode)
- [Linear Flow](#linear-flow)

## Default Type

The default type displays steps with both indicators and labels, providing maximum clarity about step content and progress.

**Usage**: Set `StepType` to `StepperType.Default` (or omit it, as this is the default)

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepType="StepperType.Default">
    <StepperSteps>
        <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Delivery" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Payment" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Ordered" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Workflows where both visual indicators and text labels are important for clarity.

## Label Type

The label type displays only the step labels, without icons or indicators. When both label and text are defined, the label takes priority.

**Usage**: Set `StepType` to `StepperType.Label`

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepType="StepperType.Label">
    <StepperSteps>
        <StepperStep Label="Cart"></StepperStep>
        <StepperStep Label="Delivery"></StepperStep>
        <StepperStep Label="Payment"></StepperStep>
        <StepperStep Label="Ordered"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Text-focused workflows with limited horizontal space or minimalist designs.

## Indicator Type

The indicator type displays only the step indicators (icons or numbers), without labels. This is useful for compact layouts or when step context is clear from the surrounding UI.

**Usage**: Set `StepType` to `StepperType.Indicator`

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepType="StepperType.Indicator">
    <StepperSteps>
        <StepperStep IconCss="sf-icon-cart"></StepperStep>
        <StepperStep IconCss="sf-icon-transport"></StepperStep>
        <StepperStep IconCss="sf-icon-payment"></StepperStep>
        <StepperStep IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**With Numeric Indicators:**

```csharp
<SfStepper StepType="StepperType.Indicator">
    <StepperSteps>
        <StepperStep Text="1"></StepperStep>
        <StepperStep Text="2"></StepperStep>
        <StepperStep Text="3"></StepperStep>
        <StepperStep Text="4"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Space-constrained layouts, mobile interfaces, or icon-driven designs.

## Label Positioning

Control where step labels appear relative to indicators using the `LabelPosition` property.

**Available Positions:**
- `StepperLabelPosition.Top` - Label above the indicator (default)
- `StepperLabelPosition.Bottom` - Label below the indicator
- `StepperLabelPosition.Start` - Label to the left of the indicator
- `StepperLabelPosition.End` - Label to the right of the indicator

```csharp
@using Syncfusion.Blazor.Navigations

<div id="container">
    <div class="e-btn-group">
        <input type="radio" id="start" name="position" value="start" 
               @oninput="@(() => updateLabelPosition("Start"))" checked />
        <label class="e-btn" for="start">Start</label>
        <input type="radio" id="end" name="position" value="end" 
               @oninput="@(() => updateLabelPosition("End"))" />
        <label class="e-btn" for="end">End</label>
        <input type="radio" id="top" name="position" value="top" 
               @oninput="@(() => updateLabelPosition("Top"))" />
        <label class="e-btn" for="top">Top</label>
        <input type="radio" id="bottom" name="position" value="bottom" 
               @oninput="@(() => updateLabelPosition("Bottom"))" />
        <label class="e-btn" for="bottom">Bottom</label>
    </div>
    <SfStepper LabelPosition="@labelPosition">
        <StepperSteps>
            <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
            <StepperStep Label="Delivery" IconCss="sf-icon-transport"></StepperStep>
            <StepperStep Label="Payment" IconCss="sf-icon-payment"></StepperStep>
            <StepperStep Label="Confirmation" IconCss="sf-icon-success"></StepperStep>
        </StepperSteps>
    </SfStepper>
</div>

@code {
    private StepperLabelPosition labelPosition = StepperLabelPosition.Start;
    
    private void updateLabelPosition(string val)
    {
        labelPosition = val == "Start" ? StepperLabelPosition.Start : 
                       val == "End" ? StepperLabelPosition.End : 
                       val == "Top" ? StepperLabelPosition.Top : 
                       StepperLabelPosition.Bottom;
    }
}

<style>
    #container {
        text-align: center;
    }
</style>
```

**Best For**: Adapting to different screen sizes or design requirements (e.g., vertical stepper with labels on the side).

## Optional and Disabled Steps

### Optional Steps

Mark steps as optional using the `Optional` property. Optional steps can be skipped in linear workflows.

```csharp
<SfStepper>
    <StepperSteps>
        <StepperStep Label="Required Step" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Optional Step" IconCss="sf-icon-transport" Optional="true"></StepperStep>
        <StepperStep Label="Required Step" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Required Step" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**When to Use**: Multi-step forms where certain steps are conditional (e.g., gift wrapping, special requests).

### Disabled Steps

Prevent interaction with steps using the `Disabled` property. Disabled steps cannot be clicked or activated by users.

```csharp
<SfStepper>
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3 (Disabled)" IconCss="sf-icon-payment" Disabled="true"></StepperStep>
        <StepperStep Label="Step 4" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**When to Use**: Steps that are not yet available or require prior completion (e.g., payment step until shipping is selected).

## Active Step Control

Set the currently active step using the `ActiveStep` property (0-indexed). The first step (index 0) is active by default.

```csharp
<SfStepper ActiveStep="1">
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2 (Active)" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Step 4" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Programmatic Control:**

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper">
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
</SfStepper>

<button @onclick="NextStep">Next Step</button>

@code {
    private SfStepper stepper;
    
    private async Task NextStep()
    {
        // Programmatically change the active step
        if (stepper.ActiveStep < 2) // Assuming 3 steps (0-2)
        {
            await stepper.NextStepAsync();
        }
    }
}
```

## Step Status

Each step can display a completion status using the `Status` property. This provides visual feedback on the state of each step.

**Available Status Values:**
- `StepperStatus.NotStarted` - Step not yet started (default)
- `StepperStatus.InProgress` - Step currently in progress
- `StepperStatus.Completed` - Step completed successfully

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper ID="stepper" StepChanged="StepChanged">
    <StepperSteps>
        <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep @ref="paymentStep" Label="Payment" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Ordered" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>

<div id="statusDisplay">@statusMessage</div>

@code {
    private StepperStep paymentStep;
    private string statusMessage = "";
    
    public void StepChanged()
    {
        // Update status message based on payment step status
        statusMessage = paymentStep.Status switch
        {
            StepperStatus.NotStarted => "Payment has not started",
            StepperStatus.InProgress => "Processing payment...",
            StepperStatus.Completed => "Payment successful",
            _ => "Unknown status"
        };
    }
}

<style>
    #statusDisplay {
        margin-top: 20px;
        padding: 10px;
        background-color: #f0f0f0;
        border-radius: 4px;
        text-align: center;
        font-weight: bold;
    }
</style>
```

**Best For**: Providing real-time feedback on workflow progress or indicating error states for validation.

## Read-Only Mode

Disable all user interactions with the Stepper using the `ReadOnly` property:

```csharp
<SfStepper ReadOnly="true">
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Step 4" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**When to Use**: Displaying a read-only progress view or preview of a completed workflow.

## Linear Flow

Enforce sequential step progression using the `Linear` property. Users can only move to the next step after completing the current one.

```csharp
<SfStepper Linear="true">
    <StepperSteps>
        <StepperStep Label="Step 1" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Step 2" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Step 3" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Step 4" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

**Best For**: Wizard-style forms where step sequence is critical to the workflow.
