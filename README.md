# Dynamo Script: Delete Unused View Filters

## Description

This Dynamo script is designed to clean and optimize Revit projects by identifying and deleting view filters that are not currently in use. It specifically targets filters that are not applied to any **View Template**, helping to reduce project clutter and improve performance.

## Features

-   **Automatic Detection:** Scans the entire Revit project to find all view filters.
-   **Usage Analysis:** Intelligently checks every view template to see which filters are applied.
-   **Interactive UI:** Displays a user-friendly window (powered by the Data-Shapes package) listing only the unused filters.
-   **Selective Deletion:** Allows the user to select exactly which of the unused filters they wish to delete.
-   **Safe Cancellation:** Includes a "Cancel" button to close the script without making any changes to the project.
-   **Confirmation:** Provides a final message confirming the number of filters that were successfully deleted.

## Requirements

-   Revit 2021 or later
-   Dynamo 2.6 or later
-   **Required Dynamo Packages:**
    -   `Data-Shapes`
    -   `Clockwork for Dynamo 2.x` (Used for the `Element.Name+` node)

## How to Use

1.  **Open Project:** Open the Revit project you wish to clean.
2.  **Launch Dynamo:** Go to the "Manage" tab in Revit and open Dynamo.
3.  **Open Script:** Open this `.dyn` file.
4.  **Run Script:** Click the "Run" button at the bottom left.
5.  **Select Filters:** An interactive window will appear, listing all view filters that are not applied to any view template.
    -   *If the list is empty, it means all filters in your project are currently in use.*
6.  **Confirm Deletion:** Check the boxes next to the filters you want to permanently delete.
7.  **Execute:** Click the "Delete Selected" button to remove the filters. Alternatively, click "Cancel" to exit without making changes.

## Graph Structure

The script is organized into three logical groups for clarity:

-   **Group 1: Collect Unused Filters (Blue):** This group contains the main Python script. It reads the Revit model, gets a list of all view filters, checks their usage across all view templates, and outputs a final list of only the unused ones.
-   **Group 2: Create Selection Interface (Orange):** This group uses the Data-Shapes package to build the pop-up window. It takes the list of unused filters, displays their names, and configures all UI elements like buttons, text, and window size.
-   **Group 3: Execute Deletion (Red):** This final group takes the user's selection from the UI, processes it, and runs the command to permanently delete the selected filters from the Revit project.
<img width="3526" height="1038" alt="Delete Unused Filters" src="https://github.com/user-attachments/assets/f60a83ec-2c24-4522-808c-bc0a70abad2b" />


## Core Python Scripts

For reference, here are the key scripts used in this workflow.

<details>
<summary><strong>1. Python Script to Find Unused Filters</strong></summary>

```python
# Imports the necessary libraries from Revit and Dynamo
import clr

clr.AddReference('RevitAPI')
from Autodesk.Revit.DB import *

clr.AddReference('RevitServices')
from RevitServices.Persistence import DocumentManager

# Gets the current Revit document
doc = DocumentManager.Instance.CurrentDBDocument

# --- 1. Get ALL view filters from the project ---
all_filters = FilteredElementCollector(doc).OfClass(ParameterFilterElement).ToElements()
all_filter_ids = {f.Id: f for f in all_filters}

# --- 2. Get the IDs of all filters USED in View Templates ---
views = FilteredElementCollector(doc).
