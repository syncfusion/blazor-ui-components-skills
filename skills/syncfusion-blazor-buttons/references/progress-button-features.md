# ProgressButton Component - Features

## Installation

```bash
dotnet add package Syncfusion.Blazor.SplitButtons
```

## Basic Progress Button

```razor
<SfProgressButton Content="Submit" Duration="4000"></SfProgressButton>
```

## Spinner Types

```razor
<!-- Left Spinner -->
<SfProgressButton Content="Left" SpinSettings="@(new SpinSettings { Position = SpinPosition.Left })"></SfProgressButton>

<!-- Right Spinner -->
<SfProgressButton Content="Right" SpinSettings="@(new SpinSettings { Position = SpinPosition.Right })"></SfProgressButton>

<!-- Top Spinner -->
<SfProgressButton Content="Top" SpinSettings="@(new SpinSettings { Position = SpinPosition.Top })"></SfProgressButton>

<!-- Bottom Spinner -->
<SfProgressButton Content="Bottom" SpinSettings="@(new SpinSettings { Position = SpinPosition.Bottom })"></SfProgressButton>

<!-- Center Spinner -->
<SfProgressButton Content="Center" SpinSettings="@(new SpinSettings { Position = SpinPosition.Center })"></SfProgressButton>
```

## Progress Events

```razor
<SfProgressButton Content="Submit" Duration="3000">
    <ProgressButtonEvents 
        OnBegin="OnBegin"
        Progress="OnProgress"
        End="OnEnd"
        Fail="OnFail">
    </ProgressButtonEvents>
</SfProgressButton>

@code {
    private void OnBegin(ProgressEventArgs args)
    {
        Console.WriteLine("Progress started");
    }

    private void OnProgress(ProgressEventArgs args)
    {
        Console.WriteLine($"Progress: {args.Percent}%");
    }

    private void OnEnd(ProgressEventArgs args)
    {
        Console.WriteLine("Progress completed");
    }

    private void OnFail(ProgressEventArgs args)
    {
        Console.WriteLine("Progress failed");
    }
}
```

## Control Progress

```razor
<SfProgressButton @ref="progressBtn" Content="Process" Duration="5000" EnableProgress="@enableProgress"></SfProgressButton>

<button @onclick="StartProgress">Start</button>
<button @onclick="StopProgress">Stop</button>

@code {
    private SfProgressButton progressBtn;
    private bool enableProgress = false;

    private void StartProgress()
    {
        enableProgress = true;
    }

    private void StopProgress()
    {
        enableProgress = false;
    }
}
```

## Custom Duration

```razor
<SfProgressButton Content="Quick (2s)" Duration="2000"></SfProgressButton>
<SfProgressButton Content="Medium (5s)" Duration="5000"></SfProgressButton>
<SfProgressButton Content="Slow (10s)" Duration="10000"></SfProgressButton>
```

## Hide Spinner

```razor
<SfProgressButton Content="Submit" SpinSettings="@(new SpinSettings { Position = SpinPosition.Center, Width = "0" })"></SfProgressButton>
```

## With Icon

```razor
<SfProgressButton Content="Upload" IconCss="e-icons e-upload"></SfProgressButton>
```

## Content During Progress

```razor
<SfProgressButton @ref="progressBtn" Content="@buttonContent" Duration="3000">
    <ProgressButtonEvents OnBegin="OnProgressBegin" End="OnProgressEnd"></ProgressButtonEvents>
</SfProgressButton>

@code {
    private SfProgressButton progressBtn;
    private string buttonContent = "Submit";

    private void OnProgressBegin(ProgressEventArgs args)
    {
        buttonContent = "Processing...";
    }

    private void OnProgressEnd(ProgressEventArgs args)
    {
        buttonContent = "Completed";
        Task.Delay(1000).ContinueWith(_ => 
        {
            InvokeAsync(() => 
            {
                buttonContent = "Submit";
                StateHasChanged();
            });
        });
    }
}
```

---

See also: [progress-button-styling.md](progress-button-styling.md) | [progress-button-advanced-features.md](progress-button-advanced-features.md)
