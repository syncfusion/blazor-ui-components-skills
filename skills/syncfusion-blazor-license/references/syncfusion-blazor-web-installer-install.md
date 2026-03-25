# Installing the Syncfusion Blazor Web Installer

This document describes how to install, configure, and uninstall Syncfusion Blazor components using the **Syncfusion Blazor Web Installer**. The installer supports both **trial** and **licensed** accounts, with product access determined after sign-in.

---

## Installation Prerequisites

- Download the Syncfusion Blazor Web Installer (`.exe`) from your Syncfusion account.
- Ensure you have internet access for authentication and package downloads.
- Sign in with a valid Syncfusion account (trial or licensed).

---

## Install Syncfusion Blazor

### Step 1: Launch the Installer

1. Locate the downloaded Blazor web installer executable.
2. Double-click the file to start the installer.
3. The installer extracts the required setup files and opens the welcome wizard.
4. Click **Next** to continue.

---

### Step 2: Select Products

1. The **Platform Selection** screen appears.
2. In the **Available** tab, select the Blazor products you want to install.
3. To install everything, choose **Install All**.
4. If products from the same version are already installed, they appear under the **Installed** tab, where you can also choose products to uninstall.
5. Click **Next**.

> **Note:** If any required third-party software is missing, the installer shows a warning. You may proceed and install the prerequisites later.

---

### Step 3: Handle Previous Versions

1. If earlier versions of selected products are detected, the installer prompts you to remove them.
2. Review the list of older versions.
3. Choose **Uninstall All** to remove previous versions, or modify the selection as needed.
4. Confirm when prompted and click **Next**.

---

### Step 4: Review and Configure

1. The **Confirmation** screen summarizes products to be installed or removed.
2. Review the list carefully.
3. Use **Download Size** and **Installation Size** links to view approximate disk usage.
4. Click **Next**.

---

### Step 5: Configuration Settings

You can customize installation settings on this screen:

- **Download Location** - where setup packages are stored
- **Install Location** - where Syncfusion components are installed
- **Demos Location** - where sample projects are placed

#### Additional Options

- Install demos (Syncfusion samples)
- Configure Syncfusion extensions in Visual Studio
- Create a desktop shortcut for the Syncfusion Control Panel
- Create a Start Menu shortcut for the Syncfusion Control Panel

Select **I agree to the License Terms and Privacy Policy**, then click **Next**.

---

### Step 6: Sign In and Install

1. Enter your Syncfusion account email and password.
2. Use **Create an account** or **Forgot password** if needed.
3. Click **Install**.

> **Important:** Products are installed based on your Syncfusion license (trial or licensed).

The installer shows download, installation, and uninstallation progress.

---

### Step 7: Complete Installation

1. When installation finishes, the **Summary** screen appears.
2. Review which products were installed successfully and which failed.
3. Click **Finish** to exit the installer.
4. Optionally select **Launch Control Panel** to manage installed products.

After installation, two Control Panel entries may be visible:

- **Essential Studio** - manages all Syncfusion products for the same version
- **Product-specific entry** - manages only Blazor products

---

## Uninstall Syncfusion Blazor

Syncfusion Blazor components can be removed in two ways.

---

### Option 1: Uninstall Using the Web Installer

1. Open the Syncfusion Blazor Web Installer executable.
2. Go to the **Installed** tab.
3. Select the products to uninstall, or choose **Uninstall All**.
4. Follow the wizard steps to complete uninstallation.

---

### Option 2: Uninstall from Windows Control Panel

1. Open **Windows Control Panel** → **Programs and Features**.
2. Choose one of the following:
   - **Syncfusion Essential Studio {version}** - removes all Syncfusion products of that version
   - **Syncfusion Essential Studio for Blazor {version}** - removes only Blazor components
3. Follow the on-screen uninstallation steps.

---
