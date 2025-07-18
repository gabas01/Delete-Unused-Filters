# Dynamo - Delete Unused View Filters from Templates

This Dynamo script is designed to optimize and clean up Revit projects by identifying and allowing the deletion of `View Filters` that are not being used in any **`View Template`**.

## Description

Over time, Revit projects can accumulate dozens of view filters that were created for testing or for workflows that are no longer in use. Keeping these filters clutters the project file and can confuse users. This script automates the cleanup process in a safe manner, focusing only on filters that are not applied to any template, which is the primary method of standardization in well-structured projects.

The workflow is interactive:
1.  The script analyzes all view templates and identifies which filters are in use.
2.  It compares this list with the list of all existing filters in the project to find the unused ones.
3.  It presents an **interactive dialog box** with a list of the unused filters.
4.  The user can uncheck the filters they wish to keep, ensuring that nothing is deleted by mistake.
5.  Only the filters that remain checked are permanently deleted from the project.

## Requirements

* Autodesk Revit 2020 or later.
* Dynamo 2.x or later.
* **Data-Shapes** package installed in Dynamo.

## How to Use

1.  Open the Revit project you want to clean up.
2.  Go to the "Manage" tab and launch Dynamo.
3.  Open the `.dyn` file for this script.
4.  Click the **"Run"** button in the bottom-left corner of Dynamo.
5.  A Data-Shapes window will appear, listing all the filters found that are not in use within templates.
6.  **Uncheck** any filter you want to **keep**. By default, all are checked for deletion.
7.  Click the **"Delete Selected"** button to confirm and delete the checked filters. If you click "Cancel," no action will be taken.

## Credits and Acknowledgements

This script and the interest in automation were inspired by the excellent work and knowledge sharing of the **BIM ONE** team. Their tutorials and solutions are a benchmark for the automation community in the construction industry. I thank them for sparking this passion!
