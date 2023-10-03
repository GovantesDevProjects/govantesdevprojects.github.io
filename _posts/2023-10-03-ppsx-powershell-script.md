---
title: How to Automatically Open PowerPoint (.ppsx) Files and Close Slideshows with PowerShell
date: 2023-10-03 12:00:00 -500
categories: [scripts, powershell]
tags: [windows,powershell,scripts,automation]
---

Do you frequently use PowerPoint slideshows (.ppsx files) and want to automate the process of opening them when they are added to a specific folder? In this guide, we'll walk you through creating a PowerShell script that automatically opens `.ppsx` files from a folder and closes the current slideshow being presented. This can be especially helpful for scenarios like presentations and displays where you need to display new slides quickly.

## Prerequisites

Before we begin, make sure you have the following:

1. A Windows computer with PowerShell installed.
2. Microsoft PowerPoint installed on your system.
3. A folder where you want to monitor for `.ppsx` files.

## Step 1: Create a PowerShell Script

First, let's create a PowerShell script to handle the automation. Open a text editor and paste the following code:

```powershell
# Define the folder path to monitor
$folderPath = "C:\Path\To\Your\Folder"

# Register a FileSystemWatcher to monitor the folder for changes
$watcher = New-Object System.IO.FileSystemWatcher
$watcher.Path = $folderPath
$watcher.Filter = "*.ppsx"
$watcher.IncludeSubdirectories = $false
$watcher.EnableRaisingEvents = $true

# Function to open a PowerPoint slideshow
function Open-Ppsx {
    param (
        [string]$ppsxPath
    )

    # Close any running PowerPoint slideshow
    Stop-Process -Name "POWERPNT" -Force -ErrorAction SilentlyContinue

    # Start the PowerPoint slideshow
    Start-Process -FilePath "C:\Program Files\Microsoft Office\root\Office16\POWERPNT.EXE" -ArgumentList "/S `"$ppsxPath`"" -Wait
}

# Event handler when a new .ppsx file is created
$createdHandler = Register-ObjectEvent -InputObject $watcher -EventName Created -Action {
    $filePath = $Event.SourceEventArgs.FullPath
    Write-Host "New .ppsx file created: $filePath"
    Open-Ppsx -ppsxPath $filePath
}

# Keep the script running
try {
    while ($true) {
        Start-Sleep -Seconds 60
    }
}
finally {
    # Clean up and unregister the event handler when done
    Unregister-Event -SourceIdentifier $createdHandler.Name
    $watcher.Dispose()
}
```

In the script, replace `$folderPath` with the actual path to the folder you want to monitor. Also, ensure that the `Start-Process` command points to the correct location of PowerPoint on your system.

## Step 2: Set Up a Scheduled Task

Now that we have our PowerShell script ready, let's set up a scheduled task to trigger it whenever a new `.ppsx` file is added to the folder.

1. Open Task Scheduler from the Start menu.
2. In the right-hand pane, click on "Create Basic Task..."
3. Follow the wizard to create a new basic task. Provide a name and description for the task.
4. Choose the "When a specific event is logged" trigger.
5. Configure the trigger to monitor the folder where your `.ppsx` files are located. Set the Source to "PowerShell" and the Event ID to "1000" (this corresponds to the creation event in PowerShell).
6. Select "Start a Program" as the action and browse to the location of your PowerShell script (`OpenPpsx.ps1`).
7. Complete the wizard, review your settings, and click "Finish."

Now, whenever a new `.ppsx` file is added to the specified folder, the PowerShell script will be triggered to open it as a slideshow and close any currently running PowerPoint slideshow.

Remember to adapt the script and Task Scheduler settings to your specific needs and environment.

## Conclusion

Automating the process of opening and closing PowerPoint slideshows with PowerShell can be a time-saver, especially in scenarios where you need to display new slides frequently. By following the steps outlined in this guide, you can streamline your presentation or display setup and ensure a seamless experience for your audience.
