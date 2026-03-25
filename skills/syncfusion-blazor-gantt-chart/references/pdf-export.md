# PDF Export

## Table of Contents
- [Overview](#overview)
- [When to Read](#when-to-read) - [Key Properties and Features](#key-properties-and-features) - [Basic PDF Export](#basic-pdf-export) - [Customize File Name](#customize-file-name) - [Page Orientation](#page-orientation) - [Page Size](#page-size) - [Multi-Page PDF Export](#multi-page-pdf-export) - [Themes](#themes) - [Headers and Footers](#headers-and-footers) - [Export Selected Rows](#export-selected-rows) - [Export Specific Columns](#export-specific-columns) - [Include/Exclude Hidden Columns](#includeexclude-hidden-columns) - [Export Events](#export-events) - [Customize Cell Appearance](#customize-cell-appearance) - [Export with Blob Object](#export-with-blob-object) - [Export Multiple Gantt Charts](#export-multiple-gantt-charts) - [Best Practices](#best-practices) - [Common Scenarios](#common-scenarios) - [Troubleshooting](#troubleshooting) - [Limitations](#limitations)

---

## Overview

PDF export in the Syncfusion Blazor Gantt Chart component enables exporting project schedules to PDF documents for sharing, reporting, archiving, and stakeholder communication. The feature provides comprehensive customization options for file names, page layouts, themes, headers, footers, and multi-page configurations.

## When to Read
- You need to export Gantt charts to PDF format
- You want to customize PDF export settings (orientation, page size, themes)
- You need multi-page PDF export for large timelines
- You require header and footer customization
- You want to control which columns or rows to export
- You need to customize taskbar appearance in PDFs

## Key Properties and Features

### Basic Export Properties
- `AllowPdfExport` - Enables PDF export functionality
- `ExportToPdfAsync(properties, enableMultiPage)` - Method to trigger PDF export
- `GanttPdfExportProperties` - Configuration class for export settings

### Export Customization Properties
- `FileName` - Set custom PDF file name
- `PageOrientation` - Portrait or Landscape orientation
- `PageSize` - A4, Letter, Legal, etc.
- `Theme` - Material, Bootstrap, Fabric, etc.
- `IncludeHiddenColumn` - Include/exclude hidden columns
- `Header` - Custom header content
- `Footer` - Custom footer content

### Multi-Page Properties
- `ScaleMode` - FitToPages or Percentage scaling
- `PageWide` - Number of pages horizontally
- `PageTall` - Number of pages vertically
- `ScalePercentage` - Percentage-based scaling factor

### Events
- `PdfExporting` - Triggered before PDF export (cancellable)
- `PdfExported` - Triggered after PDF export completes
- `PdfQueryTimelineCellInfo` - Customize timeline cell appearance
- `PdfColumnHeaderQueryCellInfo` - Customize column headers
- `PdfQueryCellInfo` - Customize grid cell appearance
- `PdfQueryTaskbarInfo` - Customize taskbar appearance in PDF

## Basic PDF Export

To enable PDF export, set `AllowPdfExport` to true and call `ExportToPdfAsync()`:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Navigations

<SfGantt @ref="Gantt" 
         DataSource="@TaskCollection" 
         Height="450px" 
         Width="900px" 
         AllowPdfExport="true" 
         Toolbar="@ToolbarItems">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress" 
                     Dependency="Predecessor" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData" />
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }
    private List<object> ToolbarItems = new List<object>() 
    { 
        new ToolbarItem() { Text = "PDF Export", TooltipText = "PDF Export", 
                           Id = "PdfExport", PrefixIcon = "e-pdfexport" } 
    };

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    public async void ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "PdfExport")
        {
            await Gantt.ExportToPdfAsync();
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
        public string Predecessor { get; set; }
    }

    public static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", 
                            StartDate = new DateTime(2022, 04, 05), 
                            EndDate = new DateTime(2022, 04, 08) },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location", 
                            StartDate = new DateTime(2022, 04, 05), Duration = "0", 
                            Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", 
                            StartDate = new DateTime(2022, 04, 05), Duration = "4", 
                            Progress = 40, ParentID = 1, Predecessor = "2" },
            new TaskData() { TaskID = 4, TaskName = "Soil test approval", 
                            StartDate = new DateTime(2022, 04, 05), Duration = "0", 
                            Progress = 30, ParentID = 1, Predecessor = "3" }
        };
    }
}
```

## Customize File Name

Set a custom file name for the exported PDF:

```csharp
public async void ToolbarClickHandler(ClickEventArgs args)
{
    if (args.Item.Id == "PdfExport")
    {
        GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
        exportProperties.FileName = "ProjectSchedule.pdf";
        await Gantt.ExportToPdfAsync(exportProperties);
    }
}
```

## Page Orientation

Change page orientation between Portrait (default) and Landscape:

```csharp
public async void ToolbarClickHandler(ClickEventArgs args)
{
    if (args.Item.Id == "PdfExport")
    {
        GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
        exportProperties.PageOrientation = PageOrientation.Landscape;
        await Gantt.ExportToPdfAsync(exportProperties);
    }
}
```

**When to use:**
- **Portrait**: More vertical space, fewer columns, vertical task lists
- **Landscape**: More horizontal space, wider timelines, many columns

## Page Size

Customize the PDF page size:

```csharp
GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
exportProperties.PageSize = PdfPageSize.A4;
await Gantt.ExportToPdfAsync(exportProperties);
```

**Supported page sizes:**
- Letter (default)
- Note
- Legal
- A0 to A9
- B0 to B5
- Archa, Archb, Archc, Archd, Arche
- Flsa
- HalfLetter
- Letter11x17
- Ledger

## Multi-Page PDF Export

For large projects, enable multi-page export to split content across multiple pages:

```csharp
public async Task ToolbarClickHandler(ClickEventArgs args)
{
    if (args.Item.Id == "PdfExport")
    {
        GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
        // Enable multi-page export
        await Gantt.ExportToPdfAsync(exportProperties, true);
    }
}
```

### Scale Modes

#### FitToPages Mode

Compress content to fit within a specified number of pages:

```razor
<SfGantt @ref="Gantt" DataSource="@TaskCollection" AllowPdfExport="true">
    <GanttEvents PdfExporting="PdfExportingHandler" TValue="TaskData" />
</SfGantt>

@code {
    public void PdfExportingHandler(PdfExportEventArgs args)
    {
        args.PdfDoc.PdfMultiPage = new PdfMultiPageSettings
        {
            ScaleMode = GanttPdfExportScaleMode.FitToPages,
            PageWide = 2,  // Fit horizontally in 2 pages
            PageTall = 3   // Fit vertically in 3 pages
        };
    }
}
```

**Properties:**
- `PageWide` - Number of pages horizontally
- `PageTall` - Number of pages vertically
- Content automatically scales to fit within specified pages

**Use when:**
- Fixed page count is required
- Document must fit specific page limits
- Printing budget restricts page count

#### Percentage Mode

Apply uniform percentage-based scaling:

```csharp
public void PdfExportingHandler(PdfExportEventArgs args)
{
    args.PdfDoc.PdfMultiPage = new PdfMultiPageSettings
    {
        ScaleMode = GanttPdfExportScaleMode.Percentage,
        ScalePercentage = 75  // Scale to 75% of original size
    };
}
```

**Use when:**
- Predictable downscaling is needed
- Page count doesn't matter
- Maintaining consistent scale factor across exports

## Themes

Apply predefined themes to PDF exports:

```csharp
GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
exportProperties.Theme = PdfTheme.Material;
await Gantt.ExportToPdfAsync(exportProperties);
```

**Available themes:**
- Material
- Bootstrap4
- Bootstrap5
- Fabric
- Fluent
- Tailwind
- HighContrast

## Headers and Footers

Add custom headers and footers to PDF pages:

```csharp
public async void ToolbarClickHandler(ClickEventArgs args)
{
    if (args.Item.Id == "PdfExport")
    {
        GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
        
        // Header configuration
        exportProperties.Header = new PdfHeader
        {
            FromTop = 0,
            Height = 130,
            Contents = new List<PdfHeaderFooterContent>
            {
                new PdfHeaderFooterContent
                {
                    Type = ContentType.Text,
                    Value = "Project Schedule Report",
                    Position = new PdfPosition { X = 0, Y = 50 },
                    Style = new PdfContentStyle 
                    { 
                        TextBrushColor = "#000000", 
                        FontSize = 13 
                    }
                }
            }
        };
        
        // Footer configuration
        exportProperties.Footer = new PdfFooter
        {
            FromBottom = 160,
            Height = 150,
            Contents = new List<PdfHeaderFooterContent>
            {
                new PdfHeaderFooterContent
                {
                    Type = ContentType.PageNumber,
                    PageNumberType = PdfPageNumberType.Arabic,
                    Format = "Page {$current} of {$total}",
                    Position = new PdfPosition { X = 250, Y = 0 },
                    Style = new PdfContentStyle 
                    { 
                        TextBrushColor = "#02007a", 
                        FontSize = 12 
                    }
                }
            }
        };
        
        await Gantt.ExportToPdfAsync(exportProperties);
    }
}
```

### Header/Footer Content Types
- `Text` - Static text
- `PageNumber` - Page numbering
- `Image` - Logo or image (base64 encoded)
- `Line` - Horizontal/vertical lines

## Export Selected Rows

Export only selected rows:

```csharp
public async Task ExportSelectedRows()
{
    var selectedRecords = await Gantt.GetSelectedRecords();
    
    GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
    exportProperties.DataSource = selectedRecords;
    
    await Gantt.ExportToPdfAsync(exportProperties);
}
```

## Export Specific Columns

Control which columns to export:

```csharp
public async void ToolbarClickHandler(ClickEventArgs args)
{
    if (args.Item.Id == "PdfExport")
    {
        GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
        exportProperties.Columns = new List<GanttColumn>
        {
            new GanttColumn { Field = "TaskID", HeaderText = "ID", Width = "50" },
            new GanttColumn { Field = "TaskName", HeaderText = "Task Name", Width = "200" },
            new GanttColumn { Field = "Duration", HeaderText = "Duration", Width = "80" }
        };
        
        await Gantt.ExportToPdfAsync(exportProperties);
    }
}
```

## Include/Exclude Hidden Columns

```csharp
GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
exportProperties.IncludeHiddenColumn = true;  // Include hidden columns
await Gantt.ExportToPdfAsync(exportProperties);
```

## Export Events

### PdfExporting Event

Triggered before export starts (cancellable):

```razor
<SfGantt DataSource="@TaskCollection" AllowPdfExport="true">
    <GanttEvents PdfExporting="OnPdfExporting" TValue="TaskData" />
</SfGantt>

@code {
    public void OnPdfExporting(PdfExportEventArgs args)
    {
        // Modify export properties
        args.PdfDoc.PageSettings.Orientation = PdfPageOrientation.Landscape;
        
        // Cancel export conditionally
        if (TaskCollection.Count > 1000)
        {
            args.Cancel = true;
            // Show warning: "Too many tasks to export"
        }
    }
}
```

### PdfExported Event

Triggered after export completes:

```csharp
public void OnPdfExported(PdfExportedEventArgs args)
{
    Console.WriteLine("PDF export completed successfully");
    // Show success notification
}
```

## Customize Cell Appearance

### Customize Grid Cells

```csharp
public void PdfQueryCellInfoHandler(PdfQueryCellInfoEventArgs<TaskData> args)
{
    if (args.Column.Field == "Progress")
    {
        if (args.Data.Progress < 50)
        {
            args.Style = new PdfQueryCellInfoStyle
            {
                BackgroundColor = "#ffcccc",  // Light red background
                TextBrushColor = "#cc0000"     // Dark red text
            };
        }
    }
}
```

### Customize Column Headers

```csharp
public void PdfColumnHeaderQueryCellInfoHandler(PdfHeaderQueryCellInfoEventArgs args)
{
    args.Style = new PdfQueryCellInfoStyle
    {
        BackgroundColor = "#0078d4",
        TextBrushColor = "#ffffff",
        Bold = true
    };
}
```

### Customize Timeline Cells

```csharp
public void PdfQueryTimelineCellInfoHandler(PdfQueryTimelineCellInfoEventArgs args)
{
    if (args.TimelineCell.ToString().Contains("Weekend"))
    {
        args.Style = new PdfQueryCellInfoStyle
        {
            BackgroundColor = "#f0f0f0"
        };
    }
}
```

### Customize Taskbars

```csharp
public void PdfQueryTaskbarInfoHandler(PdfQueryTaskbarInfoEventArgs<TaskData> args)
{
    if (args.Data.Progress == 100)
    {
        args.Taskbar.ProgressColor = "#00cc00";  // Green for completed
        args.Taskbar.TaskColor = "#00cc00";
    }
    else if (args.Data.Progress < 50)
    {
        args.Taskbar.ProgressColor = "#cc0000";  // Red for behind schedule
    }
}
```

## Export with Blob Object

Export to Blob for preview or custom handling:

```csharp
public async Task ExportToBlob()
{
    GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
    exportProperties.ReturnType = ExportType.Blob;
    
    await Gantt.ExportToPdfAsync(exportProperties);
}
```

## Export Multiple Gantt Charts

Export multiple Gantt instances into a single PDF:

```csharp
public async Task ExportMultipleGantts()
{
    // Export first Gantt to PdfDocument
    GanttPdfExportProperties props1 = new GanttPdfExportProperties();
    var pdfDoc = await Gantt1.ExportToPdfAsync(props1);
    
    // Append second Gantt to same document
    GanttPdfExportProperties props2 = new GanttPdfExportProperties();
    props2.PdfDocument = pdfDoc;
    await Gantt2.ExportToPdfAsync(props2);
}
```

## Best Practices

### File Naming
- Use descriptive names with date/project identifiers
- Include version numbers for tracking
- Avoid special characters that may cause file system issues

```csharp
var fileName = $"Project_{ProjectName}_{DateTime.Now:yyyy-MM-dd}.pdf";
exportProperties.FileName = fileName;
```

### Performance Optimization
- Export only necessary columns
- Use `IncludeHiddenColumn = false` to exclude hidden columns
- For large datasets, consider exporting filtered/selected rows
- Enable virtual scrolling for better rendering performance

### Layout Optimization
- **Wide timelines**: Use Landscape orientation
- **Many rows**: Use Portrait orientation with multi-page export
- **Large projects**: Enable multi-page with FitToPages mode
- **Print-friendly**: Test with PageWide and PageTall settings

### Content Curation
- Hide auxiliary columns before export
- Apply filters to show relevant tasks only
- Zoom to appropriate timeline level
- Simplify templates for better PDF rendering

### Styling Guidelines
- Use high-contrast colors for better print quality
- Avoid complex gradients and transparency
- Keep font sizes readable (minimum 8pt)
- Test print output before finalizing styles

## Common Scenarios

### Export Current View

```csharp
public async Task ExportCurrentView()
{
    // Export with current filter, sort, and zoom settings
    await Gantt.ExportToPdfAsync();
}
```

### Export with Date Range

```csharp
public async Task ExportDateRange(DateTime startDate, DateTime endDate)
{
    GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
    exportProperties.ProjectStartDate = startDate;
    exportProperties.ProjectEndDate = endDate;
    
    await Gantt.ExportToPdfAsync(exportProperties);
}
```

### Export for Stakeholder Review

```csharp
public async Task ExportStakeholderReport()
{
    GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
    exportProperties.FileName = $"Weekly_Report_{DateTime.Now:yyyy-MM-dd}.pdf";
    exportProperties.PageOrientation = PageOrientation.Landscape;
    exportProperties.Theme = PdfTheme.Material;
    
    // Add company header
    exportProperties.Header = new PdfHeader
    {
        Contents = new List<PdfHeaderFooterContent>
        {
            new PdfHeaderFooterContent
            {
                Type = ContentType.Text,
                Value = "Weekly Project Status Report",
                Style = new PdfContentStyle { FontSize = 16, Bold = true }
            }
        }
    };
    
    // Add page numbers
    exportProperties.Footer = new PdfFooter
    {
        Contents = new List<PdfHeaderFooterContent>
        {
            new PdfHeaderFooterContent
            {
                Type = ContentType.PageNumber,
                Format = "Page {$current} of {$total}"
            }
        }
    };
    
    await Gantt.ExportToPdfAsync(exportProperties);
}
```

### Export Summary View

```csharp
public async Task ExportSummary()
{
    // Filter to show only parent tasks
    var summaryTasks = TaskCollection.Where(t => t.ParentID == null).ToList();
    
    GanttPdfExportProperties exportProperties = new GanttPdfExportProperties();
    exportProperties.DataSource = summaryTasks;
    exportProperties.Columns = new List<GanttColumn>
    {
        new GanttColumn { Field = "TaskName", HeaderText = "Phase", Width = "250" },
        new GanttColumn { Field = "StartDate", HeaderText = "Start", Width = "100" },
        new GanttColumn { Field = "EndDate", HeaderText = "End", Width = "100" },
        new GanttColumn { Field = "Progress", HeaderText = "Progress", Width = "80" }
    };
    
    await Gantt.ExportToPdfAsync(exportProperties);
}
```

## Troubleshooting

**Issue**: Export is too crowded  
**Solution**: 
- Reduce visible columns
- Enable multi-page export
- Use Landscape orientation
- Increase page size (e.g., A3 instead of A4)

**Issue**: Timeline text is hard to read  
**Solution**:
- Use coarser zoom level before export
- Enable multi-page export with appropriate PageWide setting
- Increase page size
- Use Landscape orientation

**Issue**: Styled templates don't look good in PDF  
**Solution**:
- Simplify templates for export
- Use `PdfQueryTaskbarInfo` event for PDF-specific styling
- Avoid complex CSS that may not translate to PDF
- Test with different themes

**Issue**: Export takes too long  
**Solution**:
- Export filtered/selected rows instead of all data
- Reduce number of columns
- Disable complex cell templates during export
- Consider exporting date ranges instead of entire project

**Issue**: Page breaks in wrong places  
**Solution**:
- Adjust `PageTall` and `PageWide` settings
- Use FitToPages scale mode
- Manually control page breaks using `PdfExporting` event

**Issue**: Headers/footers not showing  
**Solution**:
- Check `FromTop` and `FromBottom` values
- Ensure `Height` is sufficient
- Verify `Position` coordinates are within page bounds
- Check if content is outside visible area

**Issue**: Colors look different in PDF  
**Solution**:
- Use hex color codes instead of named colors
- Test with print preview
- Avoid transparency and gradients
- Use PDF-safe color schemes

## Limitations

- **Manual scheduling**: Not fully supported in PDF export
- **Complex templates**: May not render identically in PDF
- **Interactive elements**: Buttons, links don't work in static PDF
- **Dynamic content**: Real-time updates not reflected after export
- **File size**: Large projects may produce large PDF files
- **Browser support**: Blob export may have browser compatibility issues

---
