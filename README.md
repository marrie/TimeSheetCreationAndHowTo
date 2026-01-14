# Introduction

This document explains how the timesheet is set up and used in Google Sheets. It documents the formulas, validation rules, and checkbox workflow used to calculate daily and weekly totals. It also explains how to lock those totals in place using the data > protect ranges menu option. The structure mirrors the worksheet as it is used and is intended to be reused from month to month.

# Prepare the Sheet

Open a new sheet by using the web address 
[sheets.new.](https://sheets.new) 
You can also add a new tab with Shift + F11 if working with an existing sheet.

Note: If your web browser does not support Shift + F11, consult the Google Sheets documentation for your browser. This applies to all keyboard shortcuts listed in this document. Select all rows with Ctrl + A, then press Backspace. Clear formatting just in case with Ctrl + \\   
Go to the View menu → Freeze > 1 row. When freezing the first row, Ensure row 1 is highlighted

# Adding Column labels, optional validation and formulas

In row 1, label the columns as follows:

- Date
- Task
- Time
- Total (day)
- Total (week)
- Total (month)
- Hours remaining (month)
- Hours remaining (week)
- Total (checkboxes)

Name the sheet. For example, “November 2025.”

## Optional validation step

If you’re setting up validation rules, paste the combo box cells into the Time column starting at C2. Extend the range down to C200 ensuring the entire month will be affected.
First time validation setup will be documented in detail later.

# Setting up Formulas

Following are example formulas used for time calculations. Some formulas intentionally reference fixed row numbers to reflect finalized daily or weekly totals that should not shift when new task rows are added. Other formulas use open or adjustable ranges during active work periods. Ranges should be modified as needed to align with the structure of the sheet for the given month.

- **Column: total (day)**
  **Formula:**
```
=SUMIF(i2:I500, true, c2:c500)
```
- **Column: Total hours (week)**
 **Formula**
```
=SUM(D26:D)
```
- **Column: Total hours (month) **
 **Formula**
```
=SUM(C2:C)
```
- **Column: Remaining hours (month): **
 **Formula:**
```
=80-C2
```
- **Column: Remaining hours (week): **
 **Formula:**
```
=20-$C$2
```

Each of these cells may be referenced throughout the month. For the Remaining hours (week) column, the formula is updated at the start of each new week to reference the first finalized daily total for that week. This is done by copying the formula and adjusting the referenced cell as needed. For example, if the new week begins with the daily total in cell C28, the formula would be:
```
=20-C28
```

#  Advanced checkbox setup 

In addition to static range-based totals, this timesheet supports a checkbox-driven workflow that calculates daily and weekly totals incrementally. This method is useful when tasks are added throughout the day and when finalized totals must be preserved for later reference or timesheet submission. This workflow introduces a dedicated Total (day) column, which is calculated first and then used as the source for weekly totals.

## Add a Checkbox Column

Insert an additional column to the right of the “remaining (week) column and label it: “total (check boxes).” Insert checkboxes into this column starting at row 2 and extending down as needed (for example, I2:I200). These checkboxes indicate which task rows should be included in the current calculation.

# Calculating Dailly Total 

Daily totals are calculated from checked task rows and stored in the Total (day) column.
Place the following formula in the appropriate Total (day) cell for the day being calculated:
```
=SUMIF(I2:I500, TRUE, C2:C500)
```
In this formula:

- Column I contains the checkboxes
- Column C contains the time values
- Only rows with a checked box (TRUE) are included

This approach allows tasks to be checked off incrementally without requiring fixed row ranges or consecutive task blocks.

# Locking a Daily Total

Once all tasks for a given day are complete and the Total (day) value is verified to be correct:
- Copy the Total (day) cell
- Use Paste special > Values only (also accessed with the keyboard shortcut control shift V)
- Protect the rows for that day using Data > Protect sheets and ranges. 
This converts the daily total from a live formula into a fixed value and prevents accidental changes. For the naming convention the date is used in the format yhyyy/mm/dd and for protecting the week (discussed later) the format is week of [day of month].

# Calculating Weekly Total from Daily Totals

Weekly totals are calculated from finalized values in the Total (day) column, not from individual task rows. If the daily totals for the week are not in consecutive rows, reference each Total (day) cell explicitly. For example:
```
=SUM(D6, D10, D15, D21, D27, D33)
```
Alternatively, if daily totals for a given week occupy a continuous block and no additional rows will be inserted above that block, a range-based approach may be used during the active work period:
```
=SUM(D2:D500)
```
When starting a new week, the lower bound of the range can be adjusted accordingly:
```
=SUM(D20:D500)
```
Regardless of the approach used, once the weekly total is verified, paste the value using Paste special > Values only and protect the cell. This ensures the weekly figure remains stable and can be reliably referenced later.

##Notes on Record Integrity

Checkboxes and formulas are intended for calculation during the active work period. Finalized totals should always be stored as pasted values in the Total (day) and Total (week) columns and protected to maintain an accurate historical record.

# First Time Validation Rule Setup

- To create validation rules do the following:

- Add a new tab in the sheet ( For example “Validation Rules).”
- Create a column called Hours in column A
Enter values in A2–A5 (with A1 as the header). Adjust your range accordingly, for example A2:A40:
  - 0.25
  - 0.5
  - 0.75
  - 1.0

## Define the Range

Highlight A2, then press Ctrl + Shift + Down Arrow to select the entire column. Note that cells up to the end of the range will be selected. Make note of the range when selecting (for example, A2:A5), then Go back to your main sheet using control shift page up, or control shift page down. Select the cells where you want the validation rules to apply (for example, C2:C200).

## Apply Data Validation

- Open the Data menu (alt plus shift plus D > Data validation. Add a rule
- Under Criteria, choose Dropdown from a range
- In the first box enter your range from your main sheet (for example, november2025). This range is where the validation combo box lives. For example, C2:C200.
- Tab to the range button and press it. 
- Switch to the sheet with your validation rule data set using control shift page up, or control shift page down. This will be the set of hours we entered earlier. Enter the range from your “Rules” sheet (for example, validation!A2:A20)

### Navigation Notes (NVDA Users)

Once in the sheet where you will set up your validation rule, use object navigation in NVDA to focus back into the range dialogue box. Sometimes NVDA may freeze; if that happens, try Shift + Ctrl + F then use object navigation to return to the range dialogue.
Once the range is applied click “done.” The range is now applied as a dropdown list of hours in your timesheet.

Also, for easier navigation, the sheet for the month currently being worked on is moved all the way to the left. This is done by opening the sheet’s context menu with Shift + Control + S and selecting Move left until the option is no longer available. Supporting sheets, such as the validation rules sheet, remain in their original position.

## Validation Rule Setup for Additional Months

At the start of a new month, Copy the validation rule from the prior month’s sheet. Select the new range, Right click (or press the application key) → Paste special → Data validation.

# Conclusion

This document describes the timesheet structure as it is used. Tasks are logged as they are completed, daily totals are calculated using checkboxes and formulas, and those totals are then locked in once the day or week is complete.
Checkboxes and formulas are used only while work is in progress. Once a total is correct, it is pasted as a value, and the relevant rows are protected. This prevents accidental changes later and keeps past days and weeks intact.
The column names, formulas, and workflows in this document intentionally mirror the worksheet itself. This makes it easier to return to the sheet later without having to reinterpret how totals were calculated or finalized.
This approach favors clarity and repeatability over automation. It allows changes while work is active and preserves finalized results.
    