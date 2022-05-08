::: top_header
[Home](https://automatetheboringstuff.com/) \| [Buy Direct from
Publisher](https://www.nostarch.com/automatestuff2) \| [Buy on
Amazon](https://inventwithpython.com/amazon-automate2) \|
[\@AlSweigart](https://twitter.com/AlSweigart) \| [Support on
Patreon](https://www.patreon.com/AlSweigart) \| [Write a
Review](https://www.amazon.com/review/create-review/?ie=UTF8&channel=glance-detail&asin=1593279922)
\|

![](https://www.paypal.com/en_US/i/scr/pixel.gif){border="0" width="1"
height="1" hidden="" style="display: none !important;"}
:::

::: main
::: {role="main"}
<div>

[![](/images/prev.png)](../chapter12)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter14)

</div>

::: {#calibre_link-1574 .calibre}
## []{#calibre_link-938 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[13]{.big} WORKING WITH EXCEL SPREADSHEETS** {#calibre_link-418 .h2b}

::: image
![Image](../images/000076.jpg){.calibre3}
:::

Although we don't often think of spreadsheets as programming tools,
almost everyone uses them to organize information into two-dimensional
data structures, perform calculations with formulas, and produce output
as charts. In the next two chapters, we'll integrate Python into two
popular spreadsheet applications: Microsoft Excel and Google Sheets.

Excel is a popular and powerful spreadsheet application for Windows. The
[openpyxl]{.literal} module allows your Python programs to read and
modify Excel spreadsheet files. For example, you might have the boring
task of copying certain data from one spreadsheet and pasting it into
another one. Or you might have to go through thousands of rows and pick
out just a handful of them to make small edits based on some criteria.
Or you might have to look through hundreds of spreadsheets of department
budgets, searching for any that are in the red. These are exactly the
sort of boring, mindless spreadsheet tasks that Python can do for you.

[]{#calibre_link-776 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Although Excel is proprietary software from
Microsoft, there are free alternatives that run on Windows, macOS, and
Linux. Both LibreOffice Calc and OpenOffice Calc work with Excel's
*.xlsx* file format for spreadsheets, which means the
[openpyxl]{.literal} module can work on spreadsheets from these
applications as well. You can download the software from
*[https://www.libreoffice.org/](https://www.libreoffice.org/){.calibre6}*
and
*[https://www.openoffice.org/](https://www.openoffice.org/){.calibre6}*,
respectively. Even if you already have Excel installed on your computer,
you may find these programs easier to use. The screenshots in this
chapter, however, are all from Excel 2010 on Windows 10.

### **Excel Documents** {#calibre_link-419 .h1}

First, let's go over some basic definitions: an Excel spreadsheet
document is called a *workbook*. A single workbook is saved in a file
with the *.xlsx* extension. Each workbook can contain multiple *sheets*
(also called *worksheets*). The sheet the user is currently viewing (or
last viewed before closing Excel) is called the *active sheet*.

Each sheet has *columns* (addressed by letters starting at *A*) and
*rows* (addressed by numbers starting at 1). A box at a particular
column and row is called a *cell*. Each cell can contain a number or
text value. The grid of cells with data makes up a sheet.

### **Installing the openpyxl Module** {#calibre_link-420 .h1}

Python does not come with OpenPyXL, so you'll have to install it. Follow
the instructions for installing third-party modules in [Appendix
A](#calibre_link-2){.calibre6}; the name of the module is
[openpyxl]{.literal}.

This book uses version 2.6.2 of OpenPyXL. It's important that you
install this version by running [pip install \--user -U
openpyxl==2.6.2]{.literal} because newer versions of OpenPyXL are
incompatible with the information in this book. To test whether it is
installed correctly, enter the following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}

If the module was correctly installed, this should produce no error
messages. Remember to import the [openpyxl]{.literal} module before
running the interactive shell examples in this chapter, or you'll get a
[NameError: name \'openpyxl\' is not defined]{.literal} error.

You can find the full documentation for OpenPyXL at
*[https://openpyxl.readthedocs.org/](https://openpyxl.readthedocs.org/){.calibre6}*.

### **Reading Excel Documents** {#calibre_link-421 .h1}

The examples in this chapter will use a spreadsheet named *example.xlsx*
stored in the root folder. You can either create the spreadsheet
yourself or download it from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.
[Figure 13-1](#calibre_link-1575){.calibre6} shows the
[]{#calibre_link-943 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}tabs for the three default sheets named *Sheet1*,
*Sheet2*, and *Sheet3* that Excel automatically provides for new
workbooks. (The number of default sheets created may vary between
operating systems and spreadsheet programs.)

::: image1
[]{#calibre_link-1575
.calibre6}![image](../images/000024.jpg){.calibre3}
:::

*Figure 13-1: The tabs for a workbook's sheets are in the lower-left
corner of Excel.*

Sheet 1 in the example file should look like [Table
13-1](#calibre_link-1576){.calibre6}. (If you didn't download
*example.xlsx* from the website, you should enter this data into the
sheet yourself.)

**Table 13-1:** The *example.xlsx* Spreadsheet

  ------- ----------------------- -------------- -------
          **A**                   **B**          **C**
  **1**   4/5/2015  1:34:02 PM    Apples         73
  **2**   4/5/2015  3:41:23 AM    Cherries       85
  **3**   4/6/2015  12:46:51 PM   Pears          14
  **4**   4/8/2015  8:59:43 AM    Oranges        52
  **5**   4/10/2015  2:07:00 AM   Apples         152
  **6**   4/10/2015  6:10:37 PM   Bananas        23
  **7**   4/10/2015  2:40:46 AM   Strawberries   98
  ------- ----------------------- -------------- -------

Now that we have our example spreadsheet, let's see how we can
manipulate it with the [openpyxl]{.literal} module.

#### ***Opening Excel Documents with OpenPyXL*** {#calibre_link-422 .h2}

Once you've imported the [openpyxl]{.literal} module, you'll be able to
use the [openpyxl.load_workbook()]{.literal} function. Enter the
following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [type(wb)]{.codestrong1}\
\<class \'openpyxl.workbook.workbook.Workbook\'\>

The [openpyxl.load_workbook()]{.literal} function takes in the filename
and returns a value of the [workbook]{.literal} data type. This
[Workbook]{.literal} object represents the Excel file, a bit like how a
[File]{.literal} object represents an opened text file.

Remember that *example.xlsx* needs to be in the current working
directory in order for you to work with it. You can find out what the
current working directory is by importing [os]{.literal} and using
[os.getcwd()]{.literal}, and you can change the current working
directory using [os.chdir()]{.literal}.

#### []{#calibre_link-777 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Getting Sheets from the Workbook*** {#calibre_link-423 .h2}

You can get a list of all the sheet names in the workbook by accessing
the [sheetnames]{.literal} attribute. Enter the following into the
interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [wb.sheetnames]{.codestrong1} \# The workbook\'s sheets\' names.\
\[\'Sheet1\', \'Sheet2\', \'Sheet3\'\]\
\>\>\> [sheet = wb\[\'Sheet3\'\]]{.codestrong1} \# Get a sheet from the
workbook.\
\>\>\> [sheet]{.codestrong1}\
\<Worksheet \"Sheet3\"\>\
\>\>\> [type(sheet)]{.codestrong1}\
\<class \'openpyxl.worksheet.worksheet.Worksheet\'\>\
\>\>\> [sheet.title]{.codestrong1} \# Get the sheet\'s title as a
string.\
\'Sheet3\'\
\>\>\> [anotherSheet = wb.active]{.codestrong1} \# Get the active
sheet.\
\>\>\> [anotherSheet]{.codestrong1}\
\<Worksheet \"Sheet1\"\>

Each sheet is represented by a [Worksheet]{.literal} object, which you
can obtain by using the square brackets with the sheet name string like
a dictionary key. Finally, you can use the [active]{.literal} attribute
of a [Workbook]{.literal} object to get the workbook's active sheet. The
active sheet is the sheet that's on top when the workbook is opened in
Excel. Once you have the [Worksheet]{.literal} object, you can get its
name from the [title]{.literal} attribute.

#### ***Getting Cells from the Sheets*** {#calibre_link-424 .h2}

Once you have a [Worksheet]{.literal} object, you can access a
[Cell]{.literal} object by its name. Enter the following into the
interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb\[\'Sheet1\'\]]{.codestrong1} \# Get a sheet from the
workbook.\
\>\>\> [sheet\[\'A1\'\]]{.codestrong1} \# Get a cell from the sheet.\
\<Cell \'Sheet1\'.A1\>\
\>\>\> [sheet\[\'A1\'\].value]{.codestrong1} \# Get the value from the
cell.\
datetime.datetime(2015, 4, 5, 13, 34, 2)\
\>\>\> [c = sheet\[\'B1\'\]]{.codestrong1} \# Get another cell from the
sheet.\
\>\>\> [c.value]{.codestrong1}\
\'Apples\'\
\>\>\> \# Get the row, column, and value from the cell.\
\>\>\> [\'Row %s, Column %s is %s\' % (c.row, c.column,
c.value)]{.codestrong1}\
\'Row 1, Column B is Apples\'\
\>\>\> [\'Cell %s is %s\' % (c.coordinate, c.value)]{.codestrong1}\
\'Cell B1 is Apples\'\
\>\>\> [sheet\[\'C1\'\].value]{.codestrong1}\
73

The [Cell]{.literal} object has a [value]{.literal} attribute that
contains, unsurprisingly, the value stored in that cell.
[Cell]{.literal} objects also have [row]{.literal}, [column]{.literal},
and [coordinate]{.literal} attributes that provide location information
for the cell.

[]{#calibre_link-891 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here, accessing the [value]{.literal} attribute of
our [Cell]{.literal} object for cell B1 gives us the string
[\'Apples\']{.literal}. The [row]{.literal} attribute gives us the
integer [1]{.literal}, the [column]{.literal} attribute gives us
[\'B\']{.literal}, and the [coordinate]{.literal} attribute gives us
[\'B1\']{.literal}.

OpenPyXL will automatically interpret the dates in column A and return
them as [datetime]{.literal} values rather than strings. The
[datetime]{.literal} data type is explained further in [Chapter
17](#calibre_link-530){.calibre6}.

Specifying a column by letter can be tricky to program, especially
because after column Z, the columns start by using two letters: AA, AB,
AC, and so on. As an alternative, you can also get a cell using the
sheet's [cell()]{.literal} method and passing integers for its
[row]{.literal} and [column]{.literal} keyword arguments. The first row
or column integer is [1]{.literal}, not [0]{.literal}. Continue the
interactive shell example by entering the following:

\>\>\> [sheet.cell(row=1, column=2)]{.codestrong1}\
\<Cell \'Sheet1\'.B1\>\
\>\>\> [sheet.cell(row=1, column=2).value]{.codestrong1}\
\'Apples\'\
\>\>\> [for i in range(1, 8, 2):]{.codestrong1} \# Go through every
other row:\
[\...     print(i, sheet.cell(row=i, column=2).value)]{.codestrong1}\
\...\
1 Apples\
3 Pears\
5 Apples\
7 Strawberries

As you can see, using the sheet's [cell()]{.literal} method and passing
it [row=1]{.literal} and [column=2]{.literal} gets you a
[Cell]{.literal} object for cell [B1]{.literal}, just like specifying
[sheet\[\'B1\'\]]{.literal} did. Then, using the [cell()]{.literal}
method and its keyword arguments, you can write a [for]{.literal} loop
to print the values of a series of cells.

Say you want to go down column B and print the value in every cell with
an odd row number. By passing [2]{.literal} for the [range()]{.literal}
function's "step" parameter, you can get cells from every second row (in
this case, all the odd-numbered rows). The [for]{.literal} loop's
[i]{.literal} variable is passed for the [row]{.literal} keyword
argument to the [cell()]{.literal} method, while [2]{.literal} is always
passed for the [column]{.literal} keyword argument. Note that the
integer [2]{.literal}, not the string [\'B\']{.literal}, is passed.

You can determine the size of the sheet with the [Worksheet]{.literal}
object's [max_row]{.literal} and [max_column]{.literal} attributes.
Enter the following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb\[\'Sheet1\'\]]{.codestrong1}\
\>\>\> [sheet.max_row]{.codestrong1} \# Get the highest row number.\
7\
\>\>\> [sheet.max_column]{.codestrong1} \# Get the highest column
number.\
3

Note that the [max_column]{.literal} attribute is an integer rather than
the letter that appears in Excel.

#### []{#calibre_link-850 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Converting Between Column Letters and Numbers*** {#calibre_link-425 .h2}

To convert from letters to numbers, call the
[openpyxl.utils.column_index_from_string()]{.literal} function. To
convert from numbers to letters, call the
[openpyxl.utils.get_column_letter()]{.literal} function. Enter the
following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [from openpyxl.utils import get_column_letter,
column_index_from_string]{.codestrong1}\
\>\>\> [get_column_letter(1)]{.codestrong1} \# Translate column 1 to a
letter.\
\'A\'\
\>\>\> [get_column_letter(2]{.codestrong1})\
\'B\'\
\>\>\> [get_column_letter(27)]{.codestrong1}\
\'AA\'\
\>\>\> [get_column_letter(900)]{.codestrong1}\
\'AHP\'\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb\[\'Sheet1\'\]]{.codestrong1}\
\>\>\> [get_column_letter(sheet.max_column)]{.codestrong1}\
\'C\'\
\>\>\> [column_index_from_string(\'A\')]{.codestrong1} \# Get A\'s
number.\
1\
\>\>\> [column_index_from_string(\'AA\')]{.codestrong1}\
27

After you import these two functions from the [openpyxl.utils]{.literal}
module, you can call [get_column_letter()]{.literal} and pass it an
integer like 27 to figure out what the letter name of the 27th column
is. The function [column_index_string()]{.literal} does the reverse: you
pass it the letter name of a column, and it tells you what number that
column is. You don't need to have a workbook loaded to use these
functions. If you want, you can load a workbook, get a
[Worksheet]{.literal} object, and use a [Worksheet]{.literal} attribute
like [max_column]{.literal} to get an integer. Then, you can pass that
integer to [get_column_letter()]{.literal}.

#### ***Getting Rows and Columns from the Sheets*** {#calibre_link-426 .h2}

You can slice [Worksheet]{.literal} objects to get all the
[Cell]{.literal} objects in a row, column, or rectangular area of the
spreadsheet. Then you can loop over all the cells in the slice. Enter
the following into the interactive shell:

   \>\>\> [import openpyxl]{.codestrong1}\
   \>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
   \>\>\> [sheet = wb\[\'Sheet1\'\]]{.codestrong1}\
   \>\>\> [tuple(sheet\[\'A1\':\'C3\'\])]{.codestrong1} \# Get all cells
from A1 to C3.\
   ((\<Cell \'Sheet1\'.A1\>, \<Cell \'Sheet1\'.B1\>, \<Cell
\'Sheet1\'.C1\>), (\<Cell\
   \'Sheet1\'.A2\>, \<Cell \'Sheet1\'.B2\>, \<Cell \'Sheet1\'.C2\>),
(\<Cell \'Sheet1\'.A3\>,\
   \<Cell \'Sheet1\'.B3\>, \<Cell \'Sheet1\'.C3\>))\
[➊]{.ent} \>\>\> [for rowOfCellObjects in
sheet\[\'A1\':\'C3\'\]:]{.codestrong1}\
[➋]{.ent} [\...     for cellObj in rowOfCellObjects:]{.codestrong1}\
   [\...         print(cellObj.coordinate,
cellObj.value)]{.codestrong1}\
   [\...     print(\'\-\-- END OF ROW \-\--\')]{.codestrong1}\
\
   A1 2015-04-05 13:34:02\
[]{#calibre_link-1810 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}   B1 Apples\
   C1 73\
   [\-\--]{.codestrong1} END OF ROW [\-\--]{.codestrong1}\
   A2 2015-04-05 03:41:23\
   B2 Cherries\
   C2 85\
   [\-\--]{.codestrong1} END OF ROW [\-\--]{.codestrong1}\
   A3 2015-04-06 12:46:51\
   B3 Pears\
   C3 14\
   [\-\--]{.codestrong1} END OF ROW [\-\--]{.codestrong1}

Here, we specify that we want the [Cell]{.literal} objects in the
rectangular area from A1 to C3, and we get a [Generator]{.literal}
object containing the [Cell]{.literal} objects in that area. To help us
visualize this [Generator]{.literal} object, we can use
[tuple()]{.literal} on it to display its [Cell]{.literal} objects in a
tuple.

This tuple contains three tuples: one for each row, from the top of the
desired area to the bottom. Each of these three inner tuples contains
the [Cell]{.literal} objects in one row of our desired area, from the
leftmost cell to the right. So overall, our slice of the sheet contains
all the [Cell]{.literal} objects in the area from A1 to C3, starting
from the top-left cell and ending with the bottom-right cell.

To print the values of each cell in the area, we use two [for]{.literal}
loops. The outer [for]{.literal} loop goes over each row in the slice
[➊]{.ent}. Then, for each row, the nested [for]{.literal} loop goes
through each cell in that row [➋]{.ent}.

To access the values of cells in a particular row or column, you can
also use a [Worksheet]{.literal} object's [rows]{.literal} and
[columns]{.literal} attribute. These attributes must be converted to
lists with the [list()]{.literal} function before you can use the square
brackets and an index with them. Enter the following into the
interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [list(sheet.columns)\[1\]]{.codestrong1} \# Get second column\'s
cells.\
(\<Cell \'Sheet1\'.B1\>, \<Cell \'Sheet1\'.B2\>, \<Cell \'Sheet1\'.B3\>,
\<Cell \'Sheet1\'.\
B4\>, \<Cell \'Sheet1\'.B5\>, \<Cell \'Sheet1\'.B6\>, \<Cell
\'Sheet1\'.B7\>)\
\>\>\> [for cellObj in list(sheet.columns)\[1\]:]{.codestrong1}\
        [print(cellObj.value)]{.codestrong1}\
\
Apples\
Cherries\
Pears\
Oranges\
Apples\
Bananas\
Strawberries

Using the [rows]{.literal} attribute on a [Worksheet]{.literal} object
will give you a tuple of tuples. Each of these inner tuples represents a
row, and contains the [Cell]{.literal} objects in that row. The
[columns]{.literal} attribute also gives you a tuple of tuples, with
each of the inner tuples containing the [Cell]{.literal} objects in a
particular []{#calibre_link-1063 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}column. For *example.xlsx*, since there are 7 rows
and 3 columns, [rows]{.literal} gives us a tuple of 7 tuples (each
containing 3 [Cell]{.literal} objects), and [columns]{.literal} gives us
a tuple of 3 tuples (each containing 7 [Cell]{.literal} objects).

To access one particular tuple, you can refer to it by its index in the
larger tuple. For example, to get the tuple that represents column B,
you use [list(sheet.columns)\[1\]]{.literal}. To get the tuple
containing the [Cell]{.literal} objects in column A, you'd use
[list(sheet.columns)\[0\]]{.literal}. Once you have a tuple representing
one row or column, you can loop through its [Cell]{.literal} objects and
print their values.

#### ***Workbooks, Sheets, Cells*** {#calibre_link-427 .h2}

As a quick review, here's a rundown of all the functions, methods, and
data types involved in reading a cell out of a spreadsheet file:

1.  Import the [openpyxl]{.literal} module.
2.  Call the [openpyxl.load_workbook()]{.literal} function.
3.  Get a [Workbook]{.literal} object.
4.  Use the [active]{.literal} or [sheetnames]{.literal} attributes.
5.  Get a [Worksheet]{.literal} object.
6.  Use indexing or the [cell()]{.literal} sheet method with
    [row]{.literal} and [column]{.literal} keyword arguments.
7.  Get a [Cell]{.literal} object.
8.  Read the [Cell]{.literal} object's [value]{.literal} attribute.

### **Project: Reading Data from a Spreadsheet** {#calibre_link-428 .h1}

Say you have a spreadsheet of data from the 2010 US Census and you have
the boring task of going through its thousands of rows to count both the
total population and the number of census tracts for each county. (A
census tract is simply a geographic area defined for the purposes of the
census.) Each row represents a single census tract. We'll name the
spreadsheet file *censuspopdata.xlsx*, and you can download it from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.
Its contents look like [Figure 13-2](#calibre_link-1577){.calibre6}.

::: image1
[]{#calibre_link-1577
.calibre6}![image](../images/000116.jpg){.calibre3}
:::

*Figure 13-2: The* censuspopdata.xlsx *spreadsheet*

[]{#calibre_link-1811 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Even though Excel can calculate the sum of multiple
selected cells, you'd still have to select the cells for each of the
3,000-plus counties. Even if it takes just a few seconds to calculate a
county's population by hand, this would take hours to do for the whole
spreadsheet.

In this project, you'll write a script that can read from the census
spreadsheet file and calculate statistics for each county in a matter of
seconds.

This is what your program does:

1.  Reads the data from the Excel spreadsheet
2.  Counts the number of census tracts in each county
3.  Counts the total population of each county
4.  Prints the results

This means your code will need to do the following:

1.  Open and read the cells of an Excel document with the
    [openpyxl]{.literal} module.
2.  Calculate all the tract and population data and store it in a data
    structure.
3.  Write the data structure to a text file with the *.py* extension
    using the [pprint]{.literal} module.

#### ***Step 1: Read the Spreadsheet Data*** {#calibre_link-429 .h2}

There is just one sheet in the *censuspopdata.xlsx* spreadsheet, named
[\'Population by Census Tract\']{.literal}, and each row holds the data
for a single census tract. The columns are the tract number (A), the
state abbreviation (B), the county name (C), and the population of the
tract (D).

Open a new file editor tab and enter the following code. Save the file
as *readCensusExcel.py*.

   #! python3\
   # readCensusExcel.py - Tabulates population and number of census
tracts for\
   # each county.\
\
[➊]{.ent} import openpyxl, pprint\
   print(\'Opening workbook\...\')\
[➋]{.ent} wb = openpyxl.load_workbook(\'censuspopdata.xlsx\')\
[➌]{.ent} sheet = wb\[\'Population by Census Tract\'\]\
   countyData = {}\
\
   # TODO: Fill in countyData with each county\'s population and
tracts.\
   print(\'Reading rows\...\')\
[➍]{.ent} for row in range(2, sheet.max_row + 1):\
       # Each row in the spreadsheet has data for one census tract.\
       state  = sheet\[\'B\' + str(row)\].value\
       county = sheet\[\'C\' + str(row)\].value\
       pop    = sheet\[\'D\' + str(row)\].value\
\
\# TODO: Open a new text file and write the contents of countyData to
it.

[]{#calibre_link-1812 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This code imports the [openpyxl]{.literal} module,
as well as the [pprint]{.literal} module that you'll use to print the
final county data [➊]{.ent}. Then it opens the *censuspopdata.xlsx* file
[➋]{.ent}, gets the sheet with the census data [➌]{.ent}, and begins
iterating over its rows [➍]{.ent}.

Note that you've also created a variable named [countyData]{.literal},
which will contain the populations and number of tracts you calculate
for each county. Before you can store anything in it, though, you should
determine exactly how you'll structure the data inside it.

#### ***Step 2: Populate the Data Structure*** {#calibre_link-430 .h2}

The data structure stored in [countyData]{.literal} will be a dictionary
with state abbreviations as its keys. Each state abbreviation will map
to another dictionary, whose keys are strings of the county names in
that state. Each county name will in turn map to a dictionary with just
two keys, [\'tracts\']{.literal} and [\'pop\']{.literal}. These keys map
to the number of census tracts and population for the county. For
example, the dictionary will look similar to this:

{\'AK\': {\'Aleutians East\': {\'pop\': 3141, \'tracts\': 1},\
        \'Aleutians West\': {\'pop\': 5561, \'tracts\': 2},\
        \'Anchorage\': {\'pop\': 291826, \'tracts\': 55},\
        \'Bethel\': {\'pop\': 17013, \'tracts\': 3},\
        \'Bristol Bay\': {\'pop\': 997, \'tracts\': 1},\
        \--[snip]{.codeitalic1}\--

If the previous dictionary were stored in [countyData]{.literal}, the
following expressions would evaluate like this:

\>\>\> [countyData\[\'AK\'\]\[\'Anchorage\'\]\[\'pop\'\]]{.codestrong1}\
291826\
\>\>\>
[countyData\[\'AK\'\]\[\'Anchorage\'\]\[\'tracts\'\]]{.codestrong1}\
55

More generally, the [countyData]{.literal} dictionary's keys will look
like this:

countyData\[[state
abbrev]{.codeitalic1}\]\[[county]{.codeitalic1}\]\[\'tracts\'\]\
countyData\[[state
abbrev]{.codeitalic1}\]\[[county]{.codeitalic1}\]\[\'pop\'\]

Now that you know how [countyData]{.literal} will be structured, you can
write the code that will fill it with the county data. Add the following
code to the bottom of your program:

#! python 3\
\# readCensusExcel.py - Tabulates population and number of census tracts
for\
\# each county.\
\
\--[snip]{.codeitalic1}\--\
\
[]{#calibre_link-1058 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}for row in range(2, sheet.max_row + 1):\
     # Each row in the spreadsheet has data for one census tract.\
     state  = sheet\[\'B\' + str(row)\].value\
     county = sheet\[\'C\' + str(row)\].value\
     pop    = sheet\[\'D\' + str(row)\].value\
\
[     # Make sure the key for this state exists.]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [countyData.setdefault(state,
{})]{.codestrong1}\
[     # Make sure the key for this county in this state
exists.]{.codestrong1}\
[  ]{.codestrong1}[➋]{.ent} [countyData\[state\].setdefault(county,
{\'tracts\': 0, \'pop\': 0})]{.codestrong1}\
\
[     # Each row represents one census tract, so increment by
one.]{.codestrong1}\
[  ]{.codestrong1}[➌]{.ent} [countyData\[state\]\[county\]\[\'tracts\'\]
+= 1]{.codestrong1}\
[     # Increase the county pop by the pop in this census
tract.]{.codestrong1}\
[  ]{.codestrong1}[➍]{.ent} [countyData\[state\]\[county\]\[\'pop\'\] +=
int(pop)]{.codestrong1}\
\
\# TODO: Open a new text file and write the contents of countyData to
it.

The last two lines of code perform the actual calculation work,
incrementing the value for [tracts]{.literal} [➌]{.ent} and increasing
the value for [pop]{.literal} [➍]{.ent} for the current county on each
iteration of the [for]{.literal} loop.

The other code is there because you cannot add a county dictionary as
the value for a state abbreviation key until the key itself exists in
[countyData]{.literal}. (That is,
[countyData\[\'AK\'\]\[\'Anchorage\'\]\[\'tracts\'\] += 1]{.literal}
will cause an error if the \'[AK\']{.literal} key doesn't exist yet.) To
make sure the state abbreviation key exists in your data structure, you
need to call the [setdefault()]{.literal} method to set a value if one
does not already exist for [state]{.literal} [➊]{.ent}.

Just as the [countyData]{.literal} dictionary needs a dictionary as the
value for each state abbreviation key, each of *those* dictionaries will
need its own dictionary as the value for each county key [➋]{.ent}. And
each of *those* dictionaries in turn will need keys
[\'tracts\']{.literal} and [\'pop\']{.literal} that start with the
integer value [0]{.literal}. (If you ever lose track of the dictionary
structure, look back at the example dictionary at the start of this
section.)

Since [setdefault()]{.literal} will do nothing if the key already
exists, you can call it on every iteration of the [for]{.literal} loop
without a problem.

#### ***Step 3: Write the Results to a File*** {#calibre_link-431 .h2}

After the [for]{.literal} loop has finished, the [countyData]{.literal}
dictionary will contain all of the population and tract information
keyed by county and state. At this point, you could program more code to
write this to a text file or another Excel spreadsheet. For now, let's
just use the [pprint.pformat()]{.literal} function to write the
[countyData]{.literal} dictionary value as a massive string to a file
named *census2010.py*. Add the following code to the bottom of your
program (making sure to keep it unindented so that it stays outside the
[for]{.literal} loop):

#! python 3\
\# readCensusExcel.py - Tabulates population and number of census tracts
for\
\# each county.\
\
[]{#calibre_link-1813 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\--[snip]{.codeitalic1}\--\
\
for row in range(2, sheet.max_row + 1):\
\--[snip]{.codeitalic1}\--\
\
[\# Open a new text file and write the contents of countyData to
it.]{.codestrong1}\
[print(\'Writing results\...\')]{.codestrong1}\
[resultFile = open(\'census2010.py\', \'w\')]{.codestrong1}\
[resultFile.write(\'allData = \' +
pprint.pformat(countyData))]{.codestrong1}\
[resultFile.close()]{.codestrong1}\
[print(\'Done.\')]{.codestrong1}

The [pprint.pformat()]{.literal} function produces a string that itself
is formatted as valid Python code. By outputting it to a text file named
*census2010.py*, you've generated a Python program from your Python
program! This may seem complicated, but the advantage is that you can
now import *census2010.py* just like any other Python module. In the
interactive shell, change the current working directory to the folder
with your newly created *census2010.py* file and then import it:

\>\>\> [import os]{.codestrong1}\
\
\>\>\> [import census2010]{.codestrong1}\
\>\>\> [census2010.allData\[\'AK\'\]\[\'Anchorage\'\]]{.codestrong1}\
{\'pop\': 291826, \'tracts\': 55}\
\>\>\> [anchoragePop =
census2010.allData\[\'AK\'\]\[\'Anchorage\'\]\[\'pop\'\]]{.codestrong1}\
\>\>\> [print(\'The 2010 population of Anchorage was \' +
str(anchoragePop))]{.codestrong1}\
The 2010 population of Anchorage was 291826

The *readCensusExcel.py* program was throwaway code: once you have its
results saved to *census2010.py*, you won't need to run the program
again. Whenever you need the county data, you can just run [import
census2010]{.literal}.

Calculating this data by hand would have taken hours; this program did
it in a few seconds. Using OpenPyXL, you will have no trouble extracting
information that is saved to an Excel spreadsheet and performing
calculations on it. You can download the complete program from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

#### ***Ideas for Similar Programs*** {#calibre_link-432 .h2}

Many businesses and offices use Excel to store various types of data,
and it's not uncommon for spreadsheets to become large and unwieldy. Any
program that parses an Excel spreadsheet has a similar structure: it
loads the spreadsheet file, preps some variables or data structures, and
then loops through each of the rows in the spreadsheet. Such a program
could do the following:

-   Compare data across multiple rows in a spreadsheet.
-   Open multiple Excel files and compare data between spreadsheets.
-   []{#calibre_link-939 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Check whether a spreadsheet has blank rows or
    invalid data in any cells and alert the user if it does.
-   Read data from a spreadsheet and use it as the input for your Python
    programs.

### **Writing Excel Documents** {#calibre_link-433 .h1}

OpenPyXL also provides ways of writing data, meaning that your programs
can create and edit spreadsheet files. With Python, it's simple to
create spreadsheets with thousands of rows of data.

#### ***Creating and Saving Excel Documents*** {#calibre_link-434 .h2}

Call the [openpyxl.Workbook()]{.literal} function to create a new, blank
[Workbook]{.literal} object. Enter the following into the interactive
shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1} \# Create a blank
workbook.\
\>\>\> [wb.sheetnames]{.codestrong1} \# It starts with one sheet.\
\[\'Sheet\'\]\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet.title]{.codestrong1}\
\'Sheet\'\
\>\>\> [sheet.title = \'Spam Bacon Eggs Sheet\']{.codestrong1} \# Change
title.\
\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'Spam Bacon Eggs Sheet\'\]

The workbook will start off with a single sheet named *Sheet*. You can
change the name of the sheet by storing a new string in its
[title]{.literal} attribute.

Any time you modify the [Workbook]{.literal} object or its sheets and
cells, the spreadsheet file will not be saved until you call the
[save()]{.literal} workbook method. Enter the following into the
interactive shell (with *example.xlsx* in the current working
directory):

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'example.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet.title = \'Spam Spam Spam\']{.codestrong1}\
\>\>\> [wb.save(\'example_copy.xlsx\')]{.codestrong1} \# Save the
workbook.

Here, we change the name of our sheet. To save our changes, we pass a
filename as a string to the [save()]{.literal} method. Passing a
different filename than the original, such as
[\'example_copy.xlsx\']{.literal}, saves the changes to a copy of the
spreadsheet.

Whenever you edit a spreadsheet you've loaded from a file, you should
always save the new, edited spreadsheet to a different filename than the
original. That way, you'll still have the original spreadsheet file to
work with in case a bug in your code caused the new, saved file to have
incorrect or corrupt data.

#### []{#calibre_link-832 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Creating and Removing Sheets*** {#calibre_link-435 .h2}

Sheets can be added to and removed from a workbook with the
[create_sheet()]{.literal} method and [del]{.literal} operator. Enter
the following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'Sheet\'\]\
\>\>\> [wb.create_sheet()]{.codestrong1} \# Add a new sheet.\
\<Worksheet \"Sheet1\"\>\
\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'Sheet\', \'Sheet1\'\]\
\>\>\> \# Create a new sheet at index 0.\
\>\>\> [wb.create_sheet(index=0, title=\'First Sheet\')]{.codestrong1}\
\<Worksheet \"First Sheet\"\>\
\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'First Sheet\', \'Sheet\', \'Sheet1\'\]\
\>\>\> [wb.create_sheet(index=2, title=\'Middle Sheet\')]{.codestrong1}\
\<Worksheet \"Middle Sheet\"\>\
\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'First Sheet\', \'Sheet\', \'Middle Sheet\', \'Sheet1\'\]

The [create_sheet()]{.literal} method returns a new
[Worksheet]{.literal} object named [Sheet]{.literal}[X]{.codeitalic},
which by default is set to be the last sheet in the workbook.
Optionally, the index and name of the new sheet can be specified with
the [index]{.literal} and [title]{.literal} keyword arguments.

Continue the previous example by entering the following:

\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'First Sheet\', \'Sheet\', \'Middle Sheet\', \'Sheet1\'\]\
\>\>\> [del wb\[\'Middle Sheet\'\]]{.codestrong1}\
\>\>\> [del wb\[\'Sheet1\'\]]{.codestrong1}\
\>\>\> [wb.sheetnames]{.codestrong1}\
\[\'First Sheet\', \'Sheet\'\]

You can use the [del]{.literal} operator to delete a sheet from a
workbook, just like you can use it to delete a key-value pair from a
dictionary.

Remember to call the [save()]{.literal} method to save the changes after
adding sheets to or removing sheets from the workbook.

#### ***Writing Values to Cells*** {#calibre_link-436 .h2}

Writing values to cells is much like writing values to keys in a
dictionary. Enter this into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [sheet = wb\[\'Sheet\'\]]{.codestrong1}\
\>\>\> [sheet\[\'A1\'\] = \'Hello, world!\']{.codestrong1} \# Edit the
cell\'s value.\
\>\>\> [sheet\[\'A1\'\].value]{.codestrong1}\
\'Hello, world!\'

[]{#calibre_link-944 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}If you have the cell's coordinate as a string, you
can use it just like a dictionary key on the [Worksheet]{.literal}
object to specify which cell to write to.

### **Project: Updating a Spreadsheet** {#calibre_link-437 .h1}

In this project, you'll write a program to update cells in a spreadsheet
of produce sales. Your program will look through the spreadsheet, find
specific kinds of produce, and update their prices. Download this
spreadsheet from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.
[Figure 13-3](#calibre_link-1578){.calibre6} shows what the spreadsheet
looks like.

::: image1
[]{#calibre_link-1578
.calibre6}![image](../images/000062.jpg){.calibre3}
:::

*Figure 13-3: A spreadsheet of produce sales*

Each row represents an individual sale. The columns are the type of
produce sold (A), the cost per pound of that produce (B), the number of
pounds sold (C), and the total revenue from the sale (D). The TOTAL
column is set to the Excel formula *=ROUND(B3\*C3, 2)*, which multiplies
the cost per pound by the number of pounds sold and rounds the result to
the nearest cent. With this formula, the cells in the TOTAL column will
automatically update themselves if there is a change in column B or C.

Now imagine that the prices of garlic, celery, and lemons were entered
incorrectly, leaving you with the boring task of going through thousands
of rows in this spreadsheet to update the cost per pound for any garlic,
celery, and lemon rows. You can't do a simple find-and-replace for the
price, because there might be other items with the same price that you
don't want to mistakenly "correct." For thousands of rows, this would
take hours to do by hand. But you can write a program that can
accomplish this in seconds.

Your program does the following:

1.  Loops over all the rows
2.  If the row is for garlic, celery, or lemons, changes the price

[]{#calibre_link-1814 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This means your code will need to do the following:

1.  Open the spreadsheet file.
2.  For each row, check whether the value in column A is
    [Celery]{.literal}, [Garlic]{.literal}, or [Lemon]{.literal}.
3.  If it is, update the price in column B.
4.  Save the spreadsheet to a new file (so that you don't lose the old
    spreadsheet, just in case).

#### ***Step 1: Set Up a Data Structure with the Update Information*** {#calibre_link-438 .h2}

The prices that you need to update are as follows:

Celery         1.19

Garlic          3.07

Lemon         1.27

You could write code like this:

if produceName == \'Celery\':\
    cellObj = 1.19\
if produceName == \'Garlic\':\
    cellObj = 3.07\
if produceName == \'Lemon\':\
    cellObj = 1.27

Having the produce and updated price data hardcoded like this is a bit
inelegant. If you needed to update the spreadsheet again with different
prices or different produce, you would have to change a lot of the code.
Every time you change code, you risk introducing bugs.

A more flexible solution is to store the corrected price information in
a dictionary and write your code to use this data structure. In a new
file editor tab, enter the following code:

#! python3\
\# updateProduce.py - Corrects costs in produce sales spreadsheet.\
\
import openpyxl\
\
wb = openpyxl.load_workbook(\'produceSales.xlsx\')\
sheet = wb\[\'Sheet\'\]\
\
\# The produce types and their updated prices\
PRICE_UPDATES = {\'Garlic\': 3.07,\
                 \'Celery\': 1.19,\
                 \'Lemon\': 1.27}\
\
\# TODO: Loop through the rows and update the prices.

Save this as *updateProduce.py*. If you need to update the spreadsheet
again, you'll need to update only the [PRICE_UPDATES]{.literal}
dictionary, not any other code.

#### []{#calibre_link-1069 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 2: Check All Rows and Update Incorrect Prices*** {#calibre_link-439 .h2}

The next part of the program will loop through all the rows in the
spreadsheet. Add the following code to the bottom of *updateProduce.py*:

   #! python3\
   # updateProduce.py - Corrects costs in produce sales spreadsheet.\
\
   \--[snip]{.codeitalic1}\--\
\
   [\# Loop through the rows and update the prices.]{.codestrong1}\
[➊]{.ent} [for rowNum in range(2, sheet.max_row):    # skip the first
row]{.codestrong1}\
[    ]{.codestrong1}[➋]{.ent} [produceName = sheet.cell(row=rowNum,
column=1).value]{.codestrong1}\
[    ]{.codestrong1}[➌]{.ent} [if produceName in
PRICE_UPDATES:]{.codestrong1}\
 [         sheet.cell(row=rowNum, column=2).value =
PRICE_UPDATES\[produceName\]]{.codestrong1}\
\
[➍]{.ent} [wb.save(\'updatedProduceSales.xlsx\')]{.codestrong1}

We loop through the rows starting at row 2, since row 1 is just the
header [➊]{.ent}. The cell in column 1 (that is, column A) will be
stored in the variable [produceName]{.literal} [➋]{.ent}. If
[produceName]{.literal} exists as a key in the [PRICE_UPDATES]{.literal}
dictionary [➌]{.ent}, then you know this is a row that must have its
price corrected. The correct price will be in
[PRICE_UPDATES\[produceName\]]{.literal}.

Notice how clean using [PRICE_UPDATES]{.literal} makes the code. Only
one [if]{.literal} statement, rather than code like [if produceName ==
\'Garlic\':]{.literal} , is necessary for every type of produce to
update. And since the code uses the [PRICE_UPDATES]{.literal} dictionary
instead of hardcoding the produce names and updated costs into the
[for]{.literal} loop, you modify only the [PRICE_UPDATES]{.literal}
dictionary and not the code if the produce sales spreadsheet needs
additional changes.

After going through the entire spreadsheet and making changes, the code
saves the [Workbook]{.literal} object to *updatedProduceSales.xlsx*
[➍]{.ent}. It doesn't overwrite the old spreadsheet just in case there's
a bug in your program and the updated spreadsheet is wrong. After
checking that the updated spreadsheet looks right, you can delete the
old spreadsheet.

You can download the complete source code for this program from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

#### ***Ideas for Similar Programs*** {#calibre_link-440 .h2}

Since many office workers use Excel spreadsheets all the time, a program
that can automatically edit and write Excel files could be really
useful. Such a program could do the following:

-   Read data from one spreadsheet and write it to parts of other
    spreadsheets.
-   Read data from websites, text files, or the clipboard and write it
    to a spreadsheet.
-   Automatically "clean up" data in spreadsheets. For example, it could
    use regular expressions to read multiple formats of phone numbers
    and edit them to a single, standard format.

### []{#calibre_link-912 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Setting the Font Style of Cells** {#calibre_link-441 .h1}

Styling certain cells, rows, or columns can help you emphasize important
areas in your spreadsheet. In the produce spreadsheet, for example, your
program could apply bold text to the potato, garlic, and parsnip rows.
Or perhaps you want to italicize every row with a cost per pound greater
than \$5. Styling parts of a large spreadsheet by hand would be tedious,
but your programs can do it instantly.

To customize font styles in cells, important, import the
[Font()]{.literal} function from the [openpyxl.styles]{.literal} module.

from openpyxl.styles import Font

This allows you to type [Font()]{.literal} instead of
[openpyxl.styles.Font()]{.literal}. (See "[Importing
Modules](#calibre_link-125){.calibre6}" on [page
47](#calibre_link-827){.calibre6} to review this style of
[import]{.literal} statement.)

Here's an example that creates a new workbook and sets cell A1 to have a
24-point, italicized font. Enter the following into the interactive
shell:

  \>\>\> [import openpyxl]{.codestrong1}\
  \>\>\> [from openpyxl.styles import Font]{.codestrong1}\
  \>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
  \>\>\> [sheet = wb\[\'Sheet\'\]]{.codestrong1}\
[➊]{.ent} \>\>\> [italic24Font = Font(size=24,
italic=True)]{.codestrong1} \# Create a font.\
[➋]{.ent} \>\>\> [sheet\[\'A1\'\].font = italic24Font]{.codestrong1} \#
Apply the font to A1.\
  \>\>\> [sheet\[\'A1\'\] = \'Hello, world!\']{.codestrong1}\
  \>\>\> [wb.save(\'styles.xlsx\')]{.codestrong1}

In this example, [Font(size=24, italic=True)]{.literal} returns a
[Font]{.literal} object, which is stored in [italic24Font]{.literal}
[➊]{.ent}. The keyword arguments to [Font()]{.literal}, [size]{.literal}
and [italic]{.literal}, configure the [Font]{.literal} object's styling
information. And when [sheet\[\'A1\'\].font]{.literal} is assigned the
[italic24Font]{.literal} object [➋]{.ent}, all that font styling
information gets applied to cell A1.

### **Font Objects** {#calibre_link-442 .h1}

To set [font]{.literal} attributes, you pass keyword arguments to
[Font()]{.literal}. [Table 13-2](#calibre_link-1579){.calibre6} shows
the possible keyword arguments for the [Font()]{.literal} function.

**Table 13-2:** Keyword Arguments for [Font]{.literal1} Objects

  **Keyword argument**   **Data type**   **Description**
  ---------------------- --------------- -----------------------------------------------------------------------------------
  [name]{.literal}       String          The font name, such as [\'Calibri\']{.literal} or [\'Times New Roman\']{.literal}
  [size]{.literal}       Integer         The point size
  [bold]{.literal}       Boolean         [True]{.literal}, for bold font
  [italic]{.literal}     Boolean         [True]{.literal}, for italic font

[]{#calibre_link-940 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can call [Font()]{.literal} to create a
[Font]{.literal} object and store that [Font]{.literal} object in a
variable. You then assign that variable to a [Cell]{.literal} object's
[font]{.literal} attribute. For example, this code creates various font
styles:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [from openpyxl.styles import Font]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [sheet = wb\[\'Sheet\'\]]{.codestrong1}\
\
\>\>\> [fontObj1 = Font(name=\'Times New Roman\',
bold=True)]{.codestrong1}\
\>\>\> [sheet\[\'A1\'\].font = fontObj1]{.codestrong1}\
\>\>\> [sheet\[\'A1\'\] = \'Bold Times New Roman\']{.codestrong1}\
\
\>\>\> [fontObj2 = Font(size=24, italic=True)]{.codestrong1}\
\>\>\> [sheet\[\'B3\'\].font = fontObj2]{.codestrong1}\
\>\>\> [sheet\[\'B3\'\] = \'24 pt Italic\']{.codestrong1}\
\
\>\>\> [wb.save(\'styles.xlsx\')]{.codestrong1}

Here, we store a [Font]{.literal} object in [fontObj1]{.literal} and
then set the A1 [Cell]{.literal} object's [font]{.literal} attribute to
[fontObj1]{.literal}. We repeat the process with another
[Font]{.literal} object to set the font of a second cell. After you run
this code, the styles of the A1 and B3 cells in the spreadsheet will be
set to custom font styles, as shown in [Figure
13-4](#calibre_link-1580){.calibre6}.

::: image1
[]{#calibre_link-1580
.calibre6}![image](../images/000007.jpg){.calibre3}
:::

*Figure 13-4: A spreadsheet with custom font styles*

For cell A1, we set the font name to [\'Times New Roman\']{.literal} and
set [bold]{.literal} to [true]{.literal}, so our text appears in bold
Times New Roman. We didn't specify a size, so the [openpyxl]{.literal}
default, 11, is used. In cell B3, our text is italic, with a size of 24;
we didn't specify a font name, so the [openpyxl]{.literal} default,
Calibri, is used.

### **Formulas** {#calibre_link-443 .h1}

Excel formulas, which begin with an equal sign, can configure cells to
contain values calculated from other cells. In this section, you'll use
the [openpyxl]{.literal} module to programmatically add formulas to
cells, just like any normal value. For example:

[\>\>\> sheet\[\'B9\'\] = \'=SUM(B1:B8)\']{.codestrong1}

[]{#calibre_link-1815 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This will store *=SUM(B1:B8)* as the value in cell
B9. This sets the B9 cell to a formula that calculates the sum of values
in cells B1 to B8. You can see this in action in [Figure
13-5](#calibre_link-1581){.calibre6}.

::: image1
[]{#calibre_link-1581
.calibre6}![image](../images/000100.jpg){.calibre3}
:::

*Figure 13-5: Cell B9 contains the formula* =SUM(B1:B8), *which adds the
cells B1 to B8.*

An Excel formula is set just like any other text value in a cell. Enter
the following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet\[\'A1\'\] = 200]{.codestrong1}\
\>\>\> [sheet\[\'A2\'\] = 300]{.codestrong1}\
\>\>\> [sheet\[\'A3\'\] = \'=SUM(A1:A2)\']{.codestrong1} \# Set the
formula.\
\>\>\> [wb.save(\'writeFormula.xlsx\')]{.codestrong1}

The cells in A1 and A2 are set to 200 and 300, respectively. The value
in cell A3 is set to a formula that sums the values in A1 and A2. When
the spreadsheet is opened in Excel, A3 will display its value as 500.

Excel formulas offer a level of programmability for spreadsheets but can
quickly become unmanageable for complicated tasks. For example, even if
you're deeply familiar with Excel formulas, it's a headache to try to
decipher what *=IFERROR(TRIM(IF(LEN(VLOOKUP(F7,
Sheet2!\$A\$1:\$B\$10000, 2, FALSE))\>0,SUBSTITUTE(VLOOKUP(F7,
Sheet2!\$A\$1:\$B\$10000, 2, FALSE), \" \", \"\"),\"\")), \"\")*
actually does. Python code is much more readable.

### **Adjusting Rows and Columns** {#calibre_link-444 .h1}

In Excel, adjusting the sizes of rows and columns is as easy as clicking
and dragging the edges of a row or column header. But if you need to set
a row []{#calibre_link-851 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}or column's size based on its cells' contents or if
you want to set sizes in a large number of spreadsheet files, it will be
much quicker to write a Python program to do it.

Rows and columns can also be hidden entirely from view. Or they can be
"frozen" in place so that they are always visible on the screen and
appear on every page when the spreadsheet is printed (which is handy for
headers).

#### ***Setting Row Height and Column Width*** {#calibre_link-445 .h2}

[Worksheet]{.literal} objects have [row_dimensions]{.literal} and
[column_dimensions]{.literal} attributes that control row heights and
column widths. Enter this into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet\[\'A1\'\] = \'Tall row\']{.codestrong1}\
\>\>\> [sheet\[\'B2\'\] = \'Wide column\']{.codestrong1}\
\>\>\> \# Set the height and width:\
\>\>\> [sheet.row_dimensions\[1\].height = 70]{.codestrong1}\
\>\>\> [sheet.column_dimensions\[\'B\'\].width = 20]{.codestrong1}\
\>\>\> [wb.save(\'dimensions.xlsx\')]{.codestrong1}

A sheet's [row_dimensions]{.literal} and [column_dimensions]{.literal}
are dictionary-like values; [row_dimensions]{.literal} contains
[RowDimension]{.literal} objects and [column_dimensions]{.literal}
contains [ColumnDimension]{.literal} objects. In
[row_dimensions]{.literal}, you can access one of the objects using the
number of the row (in this case, 1 or 2). In
[column_dimensions]{.literal}, you can access one of the objects using
the letter of the column (in this case, A or B).

The *dimensions.xlsx* spreadsheet looks like [Figure
13-6](#calibre_link-1582){.calibre6}.

::: image1
[]{#calibre_link-1582
.calibre6}![image](../images/000046.jpg){.calibre3}
:::

*Figure 13-6: Row 1 and column B set to larger heights and widths*

Once you have the [RowDimension]{.literal} object, you can set its
height. Once you have the [ColumnDimension]{.literal} object, you can
set its width. The row height can be set to an integer or float value
between [0]{.literal} and [409]{.literal}. This value represents the
height measured in *points*, where one point equals 1/72 of an inch. The
default row height is 12.75. The column width can be set to an integer
or float value between [0]{.literal} and [255]{.literal}. This value
represents the number of characters at the default font size (11 point)
that can be displayed in the cell. The default column width is 8.43
characters. Columns with widths of [0]{.literal} or rows with heights of
[0]{.literal} are hidden from the user.

#### []{#calibre_link-941 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Merging and Unmerging Cells*** {#calibre_link-446 .h2}

A rectangular area of cells can be merged into a single cell with the
[merge_cells()]{.literal} sheet method. Enter the following into the
interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet.merge_cells(\'A1:D3\')]{.codestrong1} \# Merge all these
cells.\
\>\>\> [sheet\[\'A1\'\] = \'Twelve cells merged
together.\']{.codestrong1}\
\>\>\> [sheet.merge_cells(\'C5:D5\')]{.codestrong1} \# Merge these two
cells.\
\>\>\> [sheet\[\'C5\'\] = \'Two merged cells.\']{.codestrong1}\
\>\>\> [wb.save(\'merged.xlsx\')]{.codestrong1}

The argument to [merge_cells()]{.literal} is a single string of the
top-left and bottom-right cells of the rectangular area to be merged:
[\'A1:D3\']{.literal} merges 12 cells into a single cell. To set the
value of these merged cells, simply set the value of the top-left cell
of the merged group.

When you run this code, *merged.xlsx* will look like [Figure
13-7](#calibre_link-1583){.calibre6}.

::: image1
[]{#calibre_link-1583
.calibre6}![image](../images/000140.jpg){.calibre3}
:::

*Figure 13-7: Merged cells in a spreadsheet*

To unmerge cells, call the [unmerge_cells()]{.literal} sheet method.
Enter this into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.load_workbook(\'merged.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet.unmerge_cells(\'A1:D3\')]{.codestrong1} \# Split these
cells up.\
\>\>\> [sheet.unmerge_cells(\'C5:D5\')]{.codestrong1}\
\>\>\> [wb.save(\'merged.xlsx\')]{.codestrong1}

If you save your changes and then take a look at the spreadsheet, you'll
see that the merged cells have gone back to being individual cells.

#### ***Freezing Panes*** {#calibre_link-447 .h2}

For spreadsheets too large to be displayed all at once, it's helpful to
"freeze" a few of the top rows or leftmost columns onscreen. Frozen
column or row headers, for example, are always visible to the user even
as []{#calibre_link-942 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}they scroll through the spreadsheet. These are
known as *freeze panes*. In OpenPyXL, each [Worksheet]{.literal} object
has a [freeze_panes]{.literal} attribute that can be set to a
[Cell]{.literal} object or a string of a cell's coordinates. Note that
all rows above and all columns to the left of this cell will be frozen,
but the row and column of the cell itself will not be frozen.

To unfreeze all panes, set [freeze_panes]{.literal} to [None]{.literal}
or [\'A1\']{.literal}. [Table 13-3](#calibre_link-1584){.calibre6} shows
which rows and columns will be frozen for some example settings of
[freeze_panes]{.literal}.

**Table 13-3:** Frozen Pane Examples

  [freeze_panes]{.codestrong} **setting**                                            **Rows and columns frozen**
  ---------------------------------------------------------------------------------- -----------------------------
  [sheet.freeze_panes = \'A2\']{.literal}                                            Row 1
  [sheet.freeze_panes = \'B1\']{.literal}                                            Column A
  [sheet.freeze_panes = \'C1\']{.literal}                                            Columns A and B
  [sheet.freeze_panes = \'C2\']{.literal}                                            Row 1 and columns A and B
  [sheet.freeze_panes = \'A1\']{.literal} or [sheet.freeze_panes = None]{.literal}   No frozen panes

Make sure you have the produce sales spreadsheet from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.
Then enter the following into the interactive shell:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb =
openpyxl.load_workbook(\'produceSales.xlsx\')]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [sheet.freeze_panes = \'A2\']{.codestrong1} \# Freeze the rows
above A2.\
\>\>\> [wb.save(\'freezeExample.xlsx\')]{.codestrong1}

If you set the [freeze_panes]{.literal} attribute to [\'A2\']{.literal},
row 1 will always be viewable, no matter where the user scrolls in the
spreadsheet. You can see this in [Figure
13-8](#calibre_link-1585){.calibre6}.

::: image1
[]{#calibre_link-1585
.calibre6}![image](../images/000083.jpg){.calibre3}
:::

*Figure 13-8: With [freeze_panes]{.literal1} set to [\'A2\']{.literal1},
row 1 is always visible, even as the user scrolls down.*

### []{#calibre_link-838 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Charts** {#calibre_link-448 .h1}

OpenPyXL supports creating bar, line, scatter, and pie charts using the
data in a sheet's cells. To make a chart, you need to do the following:

1.  Create a [Reference]{.literal} object from a rectangular selection
    of cells.
2.  Create a [Series]{.literal} object by passing in the
    [Reference]{.literal} object.
3.  Create a [Chart]{.literal} object.
4.  Append the [Series]{.literal} object to the [Chart]{.literal}
    object.
5.  Add the [Chart]{.literal} object to the [Worksheet]{.literal}
    object, optionally specifying which cell should be the top-left
    corner of the chart.

The [Reference]{.literal} object requires some explaining. You create
[Reference]{.literal} objects by calling the
[openpyxl.chart.Reference()]{.literal} function and passing three
arguments:

1.  The [Worksheet]{.literal} object containing your chart data.
2.  A tuple of two integers, representing the top-left cell of the
    rectangular selection of cells containing your chart data: the first
    integer in the tuple is the row, and the second is the column. Note
    that [1]{.literal} is the first row, not [0]{.literal}.
3.  A tuple of two integers, representing the bottom-right cell of the
    rectangular selection of cells containing your chart data: the first
    integer in the tuple is the row, and the second is the column.

[Figure 13-9](#calibre_link-1586){.calibre6} shows some sample
coordinate arguments.

::: image1
[]{#calibre_link-1586
.calibre6}![image](../images/000099.jpg){.calibre3}
:::

*Figure 13-9: From left to right: [(1, 1), (10, 1)]{.literal1}; [(3, 2),
(6, 4)]{.literal1}; [(5, 3), (5, 3)]{.literal1}*

Enter this interactive shell example to create a bar chart and add it to
the spreadsheet:

\>\>\> [import openpyxl]{.codestrong1}\
\>\>\> [wb = openpyxl.Workbook()]{.codestrong1}\
\>\>\> [sheet = wb.active]{.codestrong1}\
\>\>\> [for i in range(1, 11):]{.codestrong1} \# create some data in
column A\
[\...     sheet\[\'A\' + str(i)\] = i]{.codestrong1}\
\...\
\>\>\> [refObj = openpyxl.chart.Reference(sheet, min_col=1, min_row=1,
max_col=1,\
max_row=10)]{.codestrong1}\
[]{#calibre_link-913 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [seriesObj = openpyxl.chart.Series(refObj,
title=\'First series\')]{.codestrong1}\
\
\>\>\> [chartObj = openpyxl.chart.BarChart()]{.codestrong1}\
\>\>\> [chartObj.title = \'My Chart\']{.codestrong1}\
\>\>\> [chartObj.append(seriesObj)]{.codestrong1}\
\
\>\>\> [sheet.add_chart(chartObj, \'C5\')]{.codestrong1}\
\>\>\> [wb.save(\'sampleChart.xlsx\')]{.codestrong1}

This produces a spreadsheet that looks like [Figure
13-10](#calibre_link-1587){.calibre6}.

::: image1
[]{#calibre_link-1587
.calibre6}![image](../images/000122.jpg){.calibre3}
:::

*Figure 13-10: A spreadsheet with a chart added*

We've created a bar chart by calling
[openpyxl.chart.BarChart()]{.literal}. You can also create line charts,
scatter charts, and pie charts by calling
[openpyxl.charts.LineChart()]{.literal},
[openpyxl.chart.ScatterChart()]{.literal}, and
[openpyxl.chart.PieChart()]{.literal}.

### **Summary** {#calibre_link-449 .h1}

Often the hard part of processing information isn't the processing
itself but simply getting the data in the right format for your program.
But once you have your spreadsheet loaded into Python, you can extract
and manipulate its data much faster than you could by hand.

You can also generate spreadsheets as output from your programs. So if
colleagues need your text file or PDF of thousands of sales contacts
transferred to a spreadsheet file, you won't have to tediously copy and
paste it all into Excel.

Equipped with the [openpyxl]{.literal} module and some programming
knowledge, you'll find processing even the biggest spreadsheets a piece
of cake.

In the next chapter, we'll take a look at using Python to interact with
another spreadsheet program: the popular online Google Sheets
application.

### []{#calibre_link-1816 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Practice Questions** {#calibre_link-450 .h1}

For the following questions, imagine you have a [Workbook]{.literal}
object in the variable [wb]{.literal}, a [Worksheet]{.literal} object in
[sheet]{.literal}, a [Cell]{.literal} object in [cell]{.literal}, a
[Comment]{.literal} object in [comm]{.literal}, and an [Image]{.literal}
object in [img]{.literal}.

[1](#calibre_link-1588){#calibre_link-1397 .calibre6}. What does the
[openpyxl.load_workbook()]{.literal} function return?

[2](#calibre_link-1589){#calibre_link-1398 .calibre6}. What does the
[wb.sheetnames]{.literal} workbook attribute contain?

[3](#calibre_link-1590){#calibre_link-1399 .calibre6}. How would you
retrieve the [Worksheet]{.literal} object for a sheet named
[\'Sheet1\']{.literal}?

[4](#calibre_link-1591){#calibre_link-1400 .calibre6}. How would you
retrieve the [Worksheet]{.literal} object for the workbook's active
sheet?

[5](#calibre_link-1592){#calibre_link-1401 .calibre6}. How would you
retrieve the value in the cell C5?

[6](#calibre_link-1593){#calibre_link-1402 .calibre6}. How would you set
the value in the cell C5 to [\"Hello\"]{.literal}?

[7](#calibre_link-1594){#calibre_link-1403 .calibre6}. How would you
retrieve the cell's row and column as integers?

[8](#calibre_link-1595){#calibre_link-1404 .calibre6}. What do the
[sheet.max_column]{.literal} and [sheet.max_row]{.literal} sheet
attributes hold, and what is the data type of these attributes?

[9](#calibre_link-1596){#calibre_link-1405 .calibre6}. If you needed to
get the integer index for column [\'M\']{.literal}, what function would
you need to call?

[10](#calibre_link-1597){#calibre_link-1406 .calibre6}. If you needed to
get the string name for column [14]{.literal}, what function would you
need to call?

[11](#calibre_link-1598){#calibre_link-1407 .calibre6}. How can you
retrieve a tuple of all the [Cell]{.literal} objects from A1 to F1?

[12](#calibre_link-1599){#calibre_link-1408 .calibre6}. How would you
save the workbook to the filename *example.xlsx*?

[13](#calibre_link-1600){#calibre_link-1409 .calibre6}. How do you set a
formula in a cell?

[14](#calibre_link-1601){#calibre_link-1410 .calibre6}. If you want to
retrieve the result of a cell's formula instead of the cell's formula
itself, what must you do first?

[15](#calibre_link-1602){#calibre_link-1411 .calibre6}. How would you
set the height of row 5 to 100?

[16](#calibre_link-1603){#calibre_link-1412 .calibre6}. How would you
hide column C?

[17](#calibre_link-1604){#calibre_link-1413 .calibre6}. What is a freeze
pane?

[18](#calibre_link-1605){#calibre_link-1414 .calibre6}. What five
functions and methods do you have to call to create a bar chart?

### **Practice Projects** {#calibre_link-451 .h1}

For practice, write programs that perform the following tasks.

#### ***Multiplication Table Maker*** {#calibre_link-452 .h2}

Create a program *multiplicationTable.py* that takes a number *N* from
the command line and creates an *N*×*N* multiplication table in an Excel
spreadsheet. For example, when the program is run like this:

py multiplicationTable.py 6

. . . it should create a spreadsheet that looks like [Figure
13-11](#calibre_link-1606){.calibre6}.

::: image1
[]{#calibre_link-1817 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1606
.calibre6}![image](../images/000067.jpg){.calibre3}
:::

*Figure 13-11: A multiplication table generated in a spreadsheet*

Row 1 and column A should be used for labels and should be in bold.

#### ***Blank Row Inserter*** {#calibre_link-453 .h2}

Create a program *blankRowInserter.py* that takes two integers and a
filename string as command line arguments. Let's call the first integer
*N* and the second integer *M*. Starting at row *N*, the program should
insert *M* blank rows into the spreadsheet. For example, when the
program is run like this:

python blankRowInserter.py 3 2 myProduce.xlsx

. . . the "before" and "after" spreadsheets should look like [Figure
13-12](#calibre_link-1607){.calibre6}.

::: image1
[]{#calibre_link-1607
.calibre6}![image](../images/000013.jpg){.calibre3}
:::

*Figure 13-12: Before (left) and after (right) the two blank rows are
inserted at row 3*

You can write this program by reading in the contents of the
spreadsheet. Then, when writing out the new spreadsheet, use a
[for]{.literal} loop to copy the first *N* lines. For the remaining
lines, add *M* to the row number in the output spreadsheet.

#### ***Spreadsheet Cell Inverter*** {#calibre_link-454 .h2}

Write a program to invert the row and column of the cells in the
spreadsheet. For example, the value at row 5, column 3 will be at row 3,
column 5 (and vice versa). This should be done for all cells in the
spreadsheet. For example, the "before" and "after" spreadsheets would
look something like [Figure 13-13](#calibre_link-1608){.calibre6}.

::: image1
[]{#calibre_link-1818 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1608
.calibre6}![image](../images/000108.jpg){.calibre3}
:::

*Figure 13-13: The spreadsheet before (top) and after (bottom)
inversion*

You can write this program by using nested [for]{.literal} loops to read
the spreadsheet's data into a list of lists data structure. This data
structure could have [sheetData\[x\]\[y\]]{.literal} for the cell at
column [x]{.literal} and row [y]{.literal}. Then, when writing out the
new spreadsheet, use [sheetData\[y\]\[x\]]{.literal} for the cell at
column [x]{.literal} and row [y]{.literal}.

#### ***Text Files to Spreadsheet*** {#calibre_link-455 .h2}

Write a program to read in the contents of several text files (you can
make the text files yourself) and insert those contents into a
spreadsheet, with one line of text per row. The lines of the first text
file will be in the cells of column A, the lines of the second text file
will be in the cells of column B, and so on.

Use the [readlines()]{.literal} [File]{.literal} object method to return
a list of strings, one string per line in the file. For the first file,
output the first line to column 1, row 1. The second line should be
written to column 1, row 2, and so on. The next file that is read with
[readlines()]{.literal} will be written to column 2, the next file to
column 3, and so on.

#### ***Spreadsheet to Text Files*** {#calibre_link-456 .h2}

Write a program that performs the tasks of the previous program in
reverse order: the program should open a spreadsheet and write the cells
of column A into one text file, the cells of column B into another text
file, and so on.
:::

<div>

[![](/images/prev.png)](../chapter12)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter14)

</div>
:::

<div>

\
Read the author\'s other free programming books on
[InventWithPython.com](https://inventwithpython.com). Support the author
with a purchase: [Buy Direct from Publisher (Free
Ebook!)](https://www.nostarch.com/automatestuff2) \| [Buy on
Amazon](https://inventwithpython.com/amazon-automate2)

</div>

<div>

[![Automate the Boring Stuff with Python book cover
thumbnail](/images/cover_automate2_thumb.jpg){style="width: 120px"}](https://automatetheboringstuff.com)
[![Big Book of Small Python Projects book cover
thumbnail](/images/cover_bigbookpython_thumb.jpg){style="width: 120px"}](https://inventwithpython.com/bigbookpython)
[![Beyond the Basic Stuff with Python book cover
thumbnail](/images/cover_beyond_thumb.jpg){style="width: 120px"}](https://inventwithpython.com/beyond)
[![Coding with Minecraft book cover
thumbnail](/images/cover_codingwithminecraft_thumb.png){style="width: 120px"}](https://turtleappstore.com/book)
[![Cracking Codes with Python book cover
thumbnail](/images/cover_crackingcodes_thumb.png){style="width: 120px"}](https://inventwithpython.com/cracking/)
[![Invent with Python book cover
thumbnail](/images/cover_invent4th_thumb.png){style="width: 120px"}](https://inventwithpython.com/invent4thed)
[![Scratch 3 Programming Playground book cover
thumbnail](/images/cover_scratch3programmingplayground_thumb.png){style="width: 120px"}](https://inventwithscratch.com/book3/)
[![Making Games with Python and Pygame book cover
thumbnail](/images/cover_makinggames_thumb.png){style="width: 120px"}](https://inventwithpython.com/pygame/)

</div>
:::
