# XL2APL
Fast, light-weight, cross-platform package to import Excel data into Dyalog APL.
This package does NOT require or make use of the Microsoft Open XML SDK.

The general idea is to extract and transform the cell data of a sheet in the fastest most efficient way possible,
providing it to the APL programmer as an inverted table, a vector of character matrices, one item for each column. 
Because any Excel cell can contain any type, or no type at all, it is left up to the consuming package to potentially 
convert columns to numeric or date types. However, likely column types are provided based on analyis of the first
data row and its Excel formatting instructions.

## Getting Started
The API consists of two primary functions. The first, `GetWorkbookInfo`, takes a file name as its right argument and returns
a namespace of useful information about the workbook. The result may be thought of as an "instance" of XL2APL, representing a workbook: 

~~~
      w←GetWorkbookInfo 'C:\APLProjects\XL2APL\Assets\Development\TestFiles\Baseball.xlsx'
      w.SheetNames
 People  Pitching  Batting 
~~~

The second function, `GetSheetData`, takes the result of GetWorkbookInfo as
its left argument and a sheet name as its right argument and returns a namespace containing the Excel data and other useful
properties: 

~~~
      r←w GetSheetData 'People'
      5↑,r.Header
 playerID  birthYear  birthMonth  birthDay  birthCountry 
      ⍴¨5↑r.Data
 19370 9  19370 4  19370 2  19370 2  19370 14 
      ]display 4↑¨5↑r.Data
┌→──────────────────────────────────────────────┐
│ ┌→────────┐ ┌→───┐ ┌→─┐ ┌→─┐ ┌→─────────────┐ │
│ ↓aardsda01│ ↓1981│ ↓12│ ↓27│ ↓USA           │ │
│ │aaronha01│ │1934│ │2 │ │5 │ │USA           │ │
│ │aaronto01│ │1939│ │8 │ │5 │ │USA           │ │
│ │aasedo01 │ │1954│ │9 │ │8 │ │USA           │ │
│ └─────────┘ └────┘ └──┘ └──┘ └──────────────┘ │
└∊──────────────────────────────────────────────┘
      5↑r.ColumnType
 Char  Numeric  Numeric  Numeric  Char 
~~~

## Properties 
Various properties may be specified in the left argument namespace of `GetSheetData` to control
how XL2APL reads and organizes the sheet data:

`HeaderRows` The number of rows that comprise the header. Defaults to 1.

`SkipBeforeHeader`  The number or rows to skip before reading the header. Defaults to 0.

`SkipBeforeData` The number of rows to skip before reading the main data. Defaults to 0. 

`RowLimit` The maximum number of data rows to read. Defaults to 0 indicating no maximum, or
read all rows. Note the actual number of rows read may be more than the value of the property. 

`OmitEmptyRows`  Set to 1 to omit empty rows. Defaults to 0. Note that setting this to 1 may
affect how the above 4 properties are processed. For example, SkipBeforeHeader will skip that many
non-empty rows.

`OmitEmptyColumns` Set to 1 to omit empty columns. Defaults to 0.

**BlockSize** The number of bytes to read in each block. Defaults to `2*23` (8 MB).
