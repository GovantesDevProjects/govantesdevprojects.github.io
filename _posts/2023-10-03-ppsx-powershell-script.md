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

# Function to close any running PowerPoint instances
function ClosePowerPointInstances {
    Get-Process | Where-Object { $_.ProcessName -like "*POWERPNT*" } | ForEach-Object { $_.CloseMainWindow() }
}

# Function to open a PowerPoint slideshow
function OpenPowerPointSlideshow($filePath) {
    try {
        if ($filePath -ne $null -and $filePath -ne "") {
            # Close any running PowerPoint instances
            ClosePowerPointInstances
            
            # Add a delay before opening the PowerPoint slideshow
            Start-Sleep -Seconds 2
            
            # Open the PowerPoint slideshow using Invoke-Item
            Invoke-Item $filePath
        } else {
            Write-Host "File path is null or empty."
        }
    } catch {
        Write-Host "Error: $($_.Exception.Message)"
    }
}

# Initialize an empty hashtable to keep track of processed files
$processedFiles = @{}

# Create an infinite loop to periodically scan the folder
try {
    while ($true) {
        $files = Get-ChildItem -Path $folderPath -File -Filter "*.ppsx"
        
        foreach ($file in $files) {
            $filePath = $file.FullName
            
            # Check if the file has been processed before
            if (-not $processedFiles.ContainsKey($filePath)) {
                Write-Host "New .ppsx file created: $filePath"
                
                # Open the PowerPoint slideshow
                OpenPowerPointSlideshow $filePath
                
                # Add the file to the processed files hashtable
                $processedFiles[$filePath] = $true
            }
        }
        
        Start-Sleep -Seconds 2  # Adjust the sleep interval as needed
    }
}
finally {
    # Close any running PowerPoint instances and clean up
    ClosePowerPointInstances
}
```

In the script, replace `$folderPath` with the actual path to the folder you want to monitor. Also, ensure that the `Get-Process` command points to the correct location (or a rough guestimate) of PowerPoint on your system.

## The errors that were encountered (and how they were handled)

* ```ps1
    "Get-Process : Cannot find a process with the name "POWERPNT". Verify the process name and call the cmdlet again.
    At line:7 char:5
    +     Get-Process "POWERPNT" | ForEach-Object { $_.CloseMainWindow() }
    +     ~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (POWERPNT:String) [Get-Process], ProcessCommandException
        + FullyQualifiedErrorId : NoProcessFoundForGivenName,Microsoft.PowerShell.Commands.GetProcessCommand"
    ```

  This was solved by changing `"POWERPNT"` to `"*POWERPNT*"`

## Conclusion

Automating the process of opening and closing PowerPoint slideshows with PowerShell can be a time-saver, especially in scenarios where you need to display new slides frequently. By following the steps outlined in this guide, you can streamline your presentation or display setup and ensure a seamless experience for your audience.
