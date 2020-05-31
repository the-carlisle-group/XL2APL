# XL2APL
Fast, light-weight utility to import Excel data into Dyalog APL.

This utility currently requires .NET for extracting files from zip archives directly into memory, and is thus Windows dependent. It is anticipated that Dyalog will realease a .NET Core bridge in the future, making it platform independent. 

This utility does NOT require or make use of the Microsoft Open XML SDK.

The general idea is to extract and transform the cell data of a sheet in the fastest most efficient way possible,
providing it to the APL programmer as an inverted table (a vector of character matrices). 
No attempt is made to convert columns to data or numeric types; 
This is left up to the consuming package.


See the tests for getting started.
