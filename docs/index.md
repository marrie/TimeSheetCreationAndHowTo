# Introduction

This document explains how a timesheet can be set up and used in Google
Sheets. It documents the formulas, validation rules, and checkbox
workflow used to calculate daily and weekly totals. It also explains how
to lock those totals in place using the data \> protect ranges menu
option.

The structure mirrors the worksheet as it is used and is intended to be
reused from month to month.

# Prepare the Sheet

Open a new sheet by using the web address
[sheets.new](https://sheets.new)

Or add a new tab with Shift + F11 if working with an existing sheet.

Note: If your web browser does not support Shift + F11, consult the
Google Sheets documentation for your browser. This applies to all
keyboard shortcuts listed in this document. Select all rows with
Ctrl + A, then press Backspace. Clear formatting just in case
with Ctrl + \\

Go to the View menu → Freeze \> 1 row. When freezing the
first row, ensure row 1 is highlighted

## Add Column Labels

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

- Control block

Name the sheet. For example, “November 2025.”

Note. Column J is designated as a control column. It stores the active
work date and related helper values used in daily and weekly
calculations. This structure prevents repeated manual formula
adjustments during the active work period.

## Optional Validation Step

If you’re setting up validation rules, paste the combo box cells into
the Time column starting at C2. Extend the range down to
C200 ensuring the entire month will be affected.  
First time validation setup will be documented in detail later.

## Setting Up Formulas

Following are example formulas used for time calculations. Some formulas
intentionally reference fixed row numbers to reflect finalized daily or
weekly totals that should not shift when new task rows are added. Other
formulas use open or adjustable ranges during active work periods.

- Column: total (day)  
  Formula:


```
=j5
```

- Column: Total hours (week)  
  Formula

```
=j9
```

- Column: Total hours (month)  
  Formula

```
=SUM(D2:D)
```

- Column: Remaining hours (month):  
  Formula:

```
=80-F2
```

- Column: Remaining hours (week):  
  Formula:

```
=20-j9
```

Each of these cells may be referenced throughout the month. For the
Remaining hours (week) column, the value in cell J9 is updated at the
start of each new week to reference the first finalized daily total for
that week.

# Calculating Daily Total

This timesheet supports a checkbox-driven workflow that calculates daily
and weekly totals incrementally. This method is useful when tasks are
added throughout the day and when finalized totals must be preserved for
later reference or timesheet submission.

This workflow introduces a dedicated Total (day) column, which is
calculated first and then used as the source for weekly totals. 


## Add a Checkbox Column

Insert an additional column to the right of the “remaining (week) column
and label it: “total (check boxes).” Insert checkboxes into this column
starting at row 2 and extending down as needed (for example, I2:I200).
These checkboxes indicate which task rows should be included in the
current calculation.

## Daily Total Using SUMIFS (Total (day))

Daily totals are calculated using a fixed SUMIFS formula stored in the
control block in column J.

Cell J3 contains the active work date in YYYY/MM/DD format. This date is
entered manually at the start of each work day.

Cell J5 contains the live daily calculation:

```
=SUMIFS(C2:C500, I2:I500, TRUE, A2:A500, \$J\$3)
```

In this formula:

- Column C contains time values.

- Column I contains checkboxes.

- Column A contains the manually entered date.

- Only rows where the checkbox is TRUE and the date matches J3 are
  included.

- This formula remains unchanged throughout the month. It is not
  adjusted by row number.

## Locking a Daily Total 

When all tasks for the day are complete and the live total in J5 is
verified:

- In the Total (day) cell for that date, enter =J5.

- Press Enter.

- Copy that cell.

- Use Paste special → Values only.

The formula in J5 remains intact and is reused the next day by changing
only the date in J3.

## Calculate Weekly Total from Daily Totals

Weekly totals are calculated from finalized values in the Total (day)
column, not from individual task rows. At the beginning of the week,
enter =J9 in the total (week) cell. Cell J9 contains the following
formula.

```
=SUMIFS(D2:D499, A2:A499, "\>="&\$J\$7)
```

- Column D equals Total (day).

- Column A equals Date

- Cell J7 stores the week’s start date.

Once the weekly total is verified, paste the value using Paste special
\> Values only and protect the cell. This ensures the weekly figure
remains stable and can be reliably referenced later.

Cell J11 is reserved for month-boundary handling. If a new month begins on a day other than Monday, the number of hours worked earlier in the week for the previous month is manually entered into J11. This value is then temporarily referenced by weekly calculations until the week is finalized. Once the week is complete and totals have been pasted as values, J11 may be cleared and reused when needed.

## Notes on Record Integrity

Checkboxes and formulas are intended for calculation during the active
work period. Finalized totals should always be stored as pasted values
in the Total (day) and Total (week) columns. At the end of the month,
the tab should always be protected to ensure values are not accidentally
changed.

# First Time Validation Rule Setup

To create validation rules:

Add a new tab in the sheet ( For example “Validation Rules).”

Create a column called Hours. This is in column A

Enter values in A2–A5 (with A1 as the header). Adjust your range
accordingly, for example A2:A40:

   - 0.25
   
   - 0.5
   
   - 0.75
   
   - 1.0

## Define the Range

Highlight A2, then press Ctrl + Shift + Down Arrow to select the
entire column. Note that cells up to the end of the range will be
selected. Make note of the range when selecting (for example, A2:A5),
then Go back to your main sheet using control shift page up, or control
shift page down.

Select the cells where you want the validation rules to apply (for
example, C2:C200).

## Apply Data Validation

- Open Data (alt plus shift plus D \> Data validation. Add a rule

- Under Criteria, choose Dropdown from a range

- In the first box enter your range from your main sheet (for example,
  november2025). This range is where the validation combo box lives. For
  example, C2:C200.

- Next, tab to the range button and press it.

- Switch to the sheet with your validation rule data set using control
  shift page up, or control shift page down. This will be the set of
  hours we entered earlier. Enter the range from your “Rules” sheet (for
  example, validation!A2:A20)

## Navigation Notes (NVDA Users)

Once in the sheet where you will set up your validation rule, use object
navigation in NVDA to focus back into the range dialogue box. Sometimes
NVDA may freeze; if that happens, try Shift + Ctrl + F then use
object navigation to return to the range dialogue.

Once the range is applied click “done.” The range is now applied as a
dropdown list of hours in your timesheet.

Also, for easier navigation, the sheet for the month currently being
worked on is moved all the way to the left. This is done by opening the
sheet’s context menu with Shift + Control + S and selecting Move left
until the option is no longer available.

Supporting sheets, such as the validation rules sheet, remain in their
original position. 

Note: A personal script (not included in this
documentation) as well as a custom menu and option have been created to
move the selected month’s tab all the way to the beginning of the
workbook.

Note: If using bookmarks in Google Chrome or another web browser, update
the bookmark when creating a new month’s sheet. Opening an outdated
bookmark will return to the prior month and may cause incorrect
calculations.

## Validation Rule Setup for Additional Months

At the start of a new month, Copy the validation rule from the prior
month’s sheet. Select the new range, Right‑click (or press the
application key) → Paste special → Data validation.

# Conclusion

This document describes the timesheet structure as it is used. Tasks are
logged as they are completed, daily totals are calculated using
checkboxes and formulas, and those totals are then locked in once the
day or week is complete.

Checkboxes and formulas are used only while work is in progress. Once a
total is correct, it is pasted as a value, and the relevant tab is
protected at the end of the month. This prevents accidental changes
later and keeps data integrity intact.

The column names, formulas, and workflows in this document intentionally
mirror the worksheet itself. This makes it easier to return to the sheet
later without having to reinterpret how totals were calculated or
finalized.

This approach favors clarity and repeatability over automation. It
allows changes while work is active and preserves finalized results.
