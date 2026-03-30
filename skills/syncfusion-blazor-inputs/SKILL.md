---
name: syncfusion-blazor-inputs
description: Implement Syncfusion Blazor FileUpload component with async/sync uploads, file validation, drag-drop functionality, chunked uploads for large files, and comprehensive event handling. Use this when building file upload functionality with progress tracking, customizable file filtering, and seamless integration into Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Inputs"
---

# Implementing Syncfusion Blazor FileUpload

This skill covers the Syncfusion Blazor FileUpload component, a robust solution for file handling in Blazor applications. Learn to implement single and multiple file uploads, configure validation rules, enable drag-drop interactions, handle large files with chunked uploads, and leverage comprehensive events for complete upload control and user feedback.

---

## FileUpload

Learn to implement Syncfusion Blazor File Upload component with async/sync uploads, validation, events, customization, and always get immediate file handling with drag-drop or form integration for web and server applications.

### Documentation

#### Getting Started
📄 **Read:** [references/file-upload-getting-started.md](references/file-upload-getting-started.md)
- Installation and NuGet package setup
- Visual Studio/Visual Studio Code setup steps
- Blazor WebAssembly vs Server configuration
- Basic SfUploader component rendering
- CSS and script imports
- Minimal working example

#### Core Configuration
📄 **Read:** [references/file-upload-configuration.md](references/file-upload-configuration.md)
- ID property for component identification
- AllowedExtensions for file type restriction
- AllowMultiple vs single file uploads
- AutoUpload behavior configuration
- SequentialUpload for ordered processing
- DirectoryUpload capability
- Enabled state and component control

#### Upload Methods & Behavior
📄 **Read:** [references/file-upload-file-upload-methods.md](references/file-upload-file-upload-methods.md)
- Synchronous vs asynchronous uploads
- Save URL and Remove URL configuration
- Upload button click handlers
- Automatic vs manual upload triggering
- Upload progress tracking mechanisms
- Backend API requirements

#### Events & Handlers
📄 **Read:** [references/file-upload-events-and-handlers.md](references/file-upload-events-and-handlers.md)
- ValueChange event for direct file access (Blazor Server only, without AsyncSettings)
- FileSelected event for pre-validation
- Created event for initialization logic
- OnFileListRender for custom file display
- OnUploadStart, Success, OnFailure events (use with AsyncSettings)
- Event handler patterns and best practices
- **Important:** ValueChange and UploaderAsyncSettings are mutually exclusive

#### File Validation
📄 **Read:** [references/file-upload-validation.md](references/file-upload-validation.md)
- File type validation strategies
- File size constraints (MinFileSize, MaxFileSize)
- Custom validation functions
- Preventing invalid file uploads
- Error messages and user feedback
- Validation within EditForm

#### Advanced Features
📄 **Read:** [references/file-upload-advanced-features.md](references/file-upload-advanced-features.md)
- Chunked upload for large files
- Pause and resume functionality
- Async/await patterns in event handlers
- MemoryStream processing without disk I/O
- Batch upload operations
- Retry and recovery mechanisms

#### Customization & Styling
📄 **Read:** [references/file-upload-customization.md](references/file-upload-customization.md)
- Custom file list templates
- CSS class customization
- Theme Studio integration
- Custom button styling
- Dark mode support
- Responsive design patterns

#### File Source Options
📄 **Read:** [references/file-upload-file-source-options.md](references/file-upload-file-source-options.md)
- Drag-and-drop upload implementation
- Form integration patterns
- Direct file picker interaction
- Browser file dialog usage
- Directory selection and upload
- Multiple input method combinations
- Accessibility best practices

#### Localization & Accessibility
📄 **Read:** [references/file-upload-localization-accessibility.md](references/file-upload-localization-accessibility.md)
- Multi-language UI support
- Locale configuration options
- Custom text labels
- WCAG 2.1 compliance
- Keyboard navigation implementation
- Screen reader support
- ARIA attributes

#### Platform-Specific Setup
📄 **Read:** [references/file-upload-platform-specific-setup.md](references/file-upload-platform-specific-setup.md)
- Blazor WebAssembly app setup
- Blazor Server app setup
- Blazor Web App (.NET 8+) setup
- MAUI integration
- Different render modes
- Platform-specific considerations

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="true" AllowedExtensions=".jpg,.jpeg,.png,.pdf">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save" 
        RemoveUrl="api/upload/remove">
    </UploaderAsyncSettings>
</SfUploader>
```

### Common Patterns

#### Pattern 1: Basic File Upload with Validation (Server Upload)
- Use `AutoUpload="true"` for instant uploads
- Configure `UploaderAsyncSettings` with SaveUrl/RemoveUrl
- Set `AllowedExtensions` to restrict file types
- Listen to `FileSelected` event for pre-validation
- Use `Success`/`OnFailure` events for upload feedback

#### Pattern 2: Direct File Access (Blazor Server Only)
- Use `ValueChange` event to access file content directly
- **Do NOT use** `UploaderAsyncSettings` with ValueChange
- Process files in memory or save to directory
- Best for file preview, Base64 conversion, or direct storage

#### Pattern 3: Multiple File Handling
- Set `AllowMultiple="true"` to allow batch uploads
- Use `SequentialUpload` for ordered processing
- Track progress with upload events
- Display file list with individual progress indicators

#### Pattern 4: Large File Upload with Chunking
- Configure `ChunkSize` in `UploaderAsyncSettings`
- Enable pause/resume with chunk upload
- Implement retry logic for failed chunks
- Show chunk-level progress to user

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| AutoUpload | true | Upload files immediately after selection |
| AllowMultiple | true | User needs to upload multiple files |
| SequentialUpload | false | Files must upload one at a time |
| AllowedExtensions | "" | Only specific file types allowed |
| DirectoryUpload | false | User can select entire folders |
| MaxFileSize | 28.4 MB | Limiting maximum upload file size |
| MinFileSize | 0 | Setting minimum file size requirement |
| ChunkSize | 0 (disabled) | Enable chunked upload for large files |
| ShowFileList | true | Control visibility of uploaded file list |
| ShowProgressBar | true | Display upload progress indicator |
| Enabled | true | Enable or disable the uploader |
| DropArea | null | Specify custom drop zone CSS selector |
| CssClass | "" | Apply custom CSS classes |
| TabIndex | 0 | Set tab navigation order |
| EnablePersistence | false | Maintain state across page reloads |
| EnableRtl | false | Enable right-to-left layout |


### Common Use Cases

1. **Document Upload**: Resume, PDF, certification file uploads
2. **Image Gallery**: User profile pictures, photo collections
3. **Data Import**: CSV/Excel file imports for data processing
4. **Media Library**: Video, audio file uploads and management
5. **Backup Uploads**: Database backups, configuration files
6. **Report Generation**: Monthly reports, analytics data
7. **Invoice Processing**: Financial document uploads
8. **User Attachments**: Email attachments, message files

### Quick Decision Tree

**User needs file upload functionality**
  ├─ Single file only? → Set `AllowMultiple="false"` + use basic setup
  ├─ Multiple files? 
  │  ├─ All at once? → `AllowMultiple="true"` + `SequentialUpload="false"`
  │  └─ One at a time? → `AllowMultiple="true"` + `SequentialUpload="true"`
  └─ Large files (>100MB)?
     ├─ Enable chunking → Set `ChunkSize` property
     └─ Add pause/resume → Listen to `Paused` and `OnResume` events