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

[![](/images/prev.png)](../chapter9)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter11)

</div>

::: {#calibre_link-1513 .calibre}
## []{#calibre_link-1789 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[10]{.big} ORGANIZING FILES** {#calibre_link-30 .h2b}

::: image
![Image](../images/000060.jpg){.calibre3}
:::

In the previous chapter, you learned how to create and write to new
files in Python. Your programs can also organize preexisting files on
the hard drive. Maybe you've had the experience of going through a
folder full of dozens, hundreds, or even thousands of files and copying,
renaming, moving, or compressing them all by hand. Or consider tasks
such as these:

-   Making copies of all PDF files (and *only* the PDF files) in every
    subfolder of a folder
-   Removing the leading zeros in the filenames for every file in a
    folder of hundreds of files named *spam001.txt*, *spam002.txt*,
    *spam003.txt*, and so on
-   Compressing the contents of several folders into one ZIP file (which
    could be a simple backup system)

[]{#calibre_link-871 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}All this boring stuff is just begging to be
automated in Python. By programming your computer to do these tasks, you
can transform it into a quick-working file clerk who never makes
mistakes.

As you begin working with files, you may find it helpful to be able to
quickly see what the extension (.*txt*, .*pdf*, .*jpg*, and so on) of a
file is. With macOS and Linux, your file browser most likely shows
extensions automatically. With Windows, file extensions may be hidden by
default. To show extensions, go to **Start** ▸ **Control Panel** ▸
**Appearance and Personalization** ▸ **Folder Options**. On the View
tab, under Advanced Settings, uncheck the **Hide extensions for known
file types** checkbox.

### **The shutil Module** {#calibre_link-323 .h1}

The [shutil]{.literal} (or shell utilities) module has functions to let
you copy, move, rename, and delete files in your Python programs. To use
the [shutil]{.literal} functions, you will first need to use [import
shutil]{.literal}.

#### ***Copying Files and Folders*** {#calibre_link-324 .h2}

The [shutil]{.literal} module provides functions for copying files, as
well as entire folders.

Calling [shutil.copy(]{.literal}[source]{.codeitalic}[,]{.literal}
[destination]{.codeitalic}[)]{.literal} will copy the file at the path
[source]{.codeitalic} to the folder at the path
[destination]{.codeitalic}. (Both [source]{.codeitalic} and
[destination]{.codeitalic} can be strings or [Path]{.literal} objects.)
If [destination]{.codeitalic} is a filename, it will be used as the new
name of the copied file. This function returns a string or
[Path]{.literal} object of the copied file.

Enter the following into the interactive shell to see how
[shutil.copy()]{.literal} works:

   \>\>\> [import shutil, os]{.codestrong1}\
   \>\>\> [from pathlib import Path]{.codestrong1}\
   \>\>\> [p = Path.home()]{.codestrong1}\
[➊]{.ent} \>\>\> [shutil.copy(p / \'spam.txt\', p /
\'some_folder\')]{.codestrong1}\
   \'C:\\\\Users\\\\Al\\\\some_folder\\\\spam.txt\'\
[➋]{.ent} \>\>\> [shutil.copy(p / \'eggs.txt\', p /
\'some_folder/eggs2.txt\')]{.codestrong1}\
   WindowsPath(\'C:/Users/Al/some_folder/eggs2.txt\')

The first [shutil.copy()]{.literal} call copies the file at
*C:\\Users\\Al\\spam.txt* to the folder *C:\\Users\\Al\\some_folder*.
The return value is the path of the newly copied file. Note that since a
folder was specified as the destination [➊]{.ent}, the original
*spam.txt* filename is used for the new, copied file's filename. The
second [shutil.copy()]{.literal} call [➋]{.ent} also copies the file at
*C:\\Users\\Al\\eggs.txt* to the folder *C:\\Users\\Al\\some_folder* but
gives the copied file the name *eggs2.txt*.

While [shutil.copy()]{.literal} will copy a single file,
[shutil.copytree()]{.literal} will copy an entire folder and every
folder and file contained in it. Calling
[shutil.copytree(]{.literal}[source]{.codeitalic}[,]{.literal}
[destination]{.codeitalic}[)]{.literal} will copy the folder at the path
[source]{.codeitalic}, along with all of its files and subfolders, to
the folder at the path [destination]{.codeitalic}. The
[source]{.codeitalic} and [destination]{.codeitalic} parameters are both
strings. The function returns a string of the path of the copied folder.

[]{#calibre_link-966 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Enter the following into the interactive shell:

\>\>\> [import shutil, os]{.codestrong1}\
\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [p = Path.home()]{.codestrong1}\
\>\>\> [shutil.copytree(p / \'spam\', p /
\'spam_backup\')]{.codestrong1}\
WindowsPath(\'C:/Users/Al/spam_backup\')

The [shutil.copytree()]{.literal} call creates a new folder named
*spam_backup* with the same content as the original *spam* folder. You
have now safely backed up your precious, precious spam.

#### ***Moving and Renaming Files and Folders*** {#calibre_link-325 .h2}

Calling [shutil.move(]{.literal}[source]{.codeitalic}[,]{.literal}
[destination]{.codeitalic}[)]{.literal} will move the file or folder at
the path [source]{.codeitalic} to the path [destination]{.codeitalic}
and will return a string of the absolute path of the new location.

If [destination]{.codeitalic} points to a folder, the
[source]{.codeitalic} file gets moved into [destination]{.codeitalic}
and keeps its current filename. For example, enter the following into
the interactive shell:

\>\>\> [import shutil]{.codestrong1}\
\>\>\> [shutil.move(\'C:\\\\bacon.txt\', \'C:\\\\eggs\')]{.codestrong1}\
\'C:\\\\eggs\\\\bacon.txt\'

Assuming a folder named *eggs* already exists in the *C:\\* directory,
this [shutil.move()]{.literal} call says, "Move *C:\\bacon.txt* into the
folder *C:\\eggs*."

If there had been a *bacon.txt* file already in *C:\\eggs*, it would
have been overwritten. Since it's easy to accidentally overwrite files
in this way, you should take some care when using [move()]{.literal}.

The [destination]{.codeitalic} path can also specify a filename. In the
following example, the [source]{.codeitalic} file is moved *and*
renamed.

\>\>\> [shutil.move(\'C:\\\\bacon.txt\',
\'C:\\\\eggs\\\\new_bacon.txt\')]{.codestrong1}\
\'C:\\\\eggs\\\\new_bacon.txt\'

This line says, "Move *C:\\bacon.txt* into the folder *C:\\eggs*, and
while you're at it, rename that *bacon.txt* file to *new_bacon.txt*."

Both of the previous examples worked under the assumption that there was
a folder *eggs* in the *C:\\* directory. But if there is no *eggs*
folder, then [move()]{.literal} will rename *bacon.txt* to a file named
*eggs*.

\>\>\> [shutil.move(\'C:\\\\bacon.txt\', \'C:\\\\eggs\')]{.codestrong1}\
\'C:\\\\eggs\'

Here, [move()]{.literal} can't find a folder named *eggs* in the *C:\\*
directory and so assumes that [destination]{.codeitalic} must be
specifying a filename, not a folder. So the *bacon.txt* text file is
renamed to *eggs* (a text file without the *.txt* file
extension)---probably not what you wanted! This can be a tough-to-spot
bug in your programs since the [move()]{.literal} call can happily do
something that might be []{#calibre_link-905 {http:="" www.idpf.org=""
2007="" ops}type="pagebreak"}quite different from what you were
expecting. This is yet another reason to be careful when using
[move()]{.literal}.

Finally, the folders that make up the destination must already exist, or
else Python will throw an exception. Enter the following into the
interactive shell:

\>\>\> [shutil.move(\'spam.txt\',
\'c:\\\\does_not_exist\\\\eggs\\\\ham\')]{.codestrong1}\
Traceback (most recent call last):\
  \--[snip]{.codeitalic1}\--\
FileNotFoundError: \[Errno 2\] No such file or directory:
\'c:\\\\does_not_exist\\\\\
eggs\\\\ham\'

Python looks for *eggs* and *ham* inside the directory *does_not_exist*.
It doesn't find the nonexistent directory, so it can't move *spam.txt*
to the path you specified.

#### ***Permanently Deleting Files and Folders*** {#calibre_link-326 .h2}

You can delete a single file or a single empty folder with functions in
the [os]{.literal} module, whereas to delete a folder and all of its
contents, you use the [shutil]{.literal} module.

-   Calling [os.unlink(]{.literal}[path]{.codeitalic}[)]{.literal} will
    delete the file at [path]{.codeitalic}.
-   Calling [os.rmdir(]{.literal}[path]{.codeitalic}[)]{.literal} will
    delete the folder at [path]{.codeitalic}. This folder must be empty
    of any files or folders.
-   Calling [shutil.rmtree(]{.literal}[path]{.codeitalic}[)]{.literal}
    will remove the folder at [path]{.codeitalic}, and all files and
    folders it contains will also be deleted.

Be careful when using these functions in your programs! It's often a
good idea to first run your program with these calls commented out and
with [print()]{.literal} calls added to show the files that would be
deleted. Here is a Python program that was intended to delete files that
have the *.txt* file extension but has a typo (highlighted in bold) that
causes it to delete *.rxt* files instead:

import os\
from pathlib import Path\
for filename in Path.home().glob(\'\*.[r]{.codestrong1}xt\'):\
    os.unlink(filename)

If you had any important files ending with *.rxt*, they would have been
accidentally, permanently deleted. Instead, you should have first run
the program like this:

import os\
from pathlib import Path\
for filename in Path.home().glob(\'\*.rxt\'):\
    #os.unlink(filename)\
    print(filename)

[]{#calibre_link-906 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Now the [os.unlink()]{.literal} call is commented,
so Python ignores it. Instead, you will print the filename of the file
that would have been deleted. Running this version of the program first
will show you that you've accidentally told the program to delete *.rxt*
files instead of *.txt* files.

Once you are certain the program works as intended, delete the
[print(filename)]{.literal} line and uncomment the
[os.unlink(filename)]{.literal} line. Then run the program again to
actually delete the files.

#### ***Safe Deletes with the send2trash Module*** {#calibre_link-327 .h2}

Since Python's built-in [shutil.rmtree()]{.literal} function
irreversibly deletes files and folders, it can be dangerous to use. A
much better way to delete files and folders is with the third-party
[send2trash]{.literal} module. You can install this module by running
[pip install \--user send2trash]{.literal} from a Terminal window. (See
[Appendix A](#calibre_link-2){.calibre6} for a more in-depth explanation
of how to install third-party modules.)

Using [send2trash]{.literal} is much safer than Python's regular delete
functions, because it will send folders and files to your computer's
trash or recycle bin instead of permanently deleting them. If a bug in
your program deletes something with [send2trash]{.literal} you didn't
intend to delete, you can later restore it from the recycle bin.

After you have installed [send2trash]{.literal}, enter the following
into the interactive shell:

\>\>\> [import send2trash]{.codestrong1}\
\>\>\> [baconFile = open(\'bacon.txt\', \'a\')   # creates the
file]{.codestrong1}\
\>\>\> [baconFile.write(\'Bacon is not a vegetable.\')]{.codestrong1}\
25\
\>\>\> [baconFile.close()]{.codestrong1}\
\>\>\> [send2trash.send2trash(\'bacon.txt\')]{.codestrong1}

In general, you should always use the
[send2trash.send2trash()]{.literal} function to delete files and
folders. But while sending files to the recycle bin lets you recover
them later, it will not free up disk space like permanently deleting
them does. If you want your program to free up disk space, use the
[os]{.literal} and [shutil]{.literal} functions for deleting files and
folders. Note that the [send2trash()]{.literal} function can only send
files to the recycle bin; it cannot pull files out of it.

### **Walking a Directory Tree** {#calibre_link-328 .h1}

Say you want to rename every file in some folder and also every file in
every subfolder of that folder. That is, you want to walk through the
directory tree, touching each file as you go. Writing a program to do
this could get tricky; fortunately, Python provides a function to handle
this process for you.

Let's look at the *C:\\delicious* folder with its contents, shown in
[Figure 10-1](#calibre_link-1195){.calibre6}.

::: image1
[]{#calibre_link-969 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1195
.calibre6}![image](../images/000047.jpg){.calibre3}
:::

*Figure 10-1: An example folder that contains three folders and four
files*

Here is an example program that uses the [os.walk()]{.literal} function
on the directory tree from [Figure 10-1](#calibre_link-1195){.calibre6}:

import os\
\
for folderName, subfolders, filenames in os.walk(\'C:\\\\delicious\'):\
    print(\'The current folder is \' + folderName)\
\
    for subfolder in subfolders:\
        print(\'SUBFOLDER OF \' + folderName + \': \' + subfolder)\
\
    for filename in filenames:\
        print(\'FILE INSIDE \' + folderName + \': \'+ filename)\
\
    print(\'\')

The [os.walk()]{.literal} function is passed a single string value: the
path of a folder. You can use [os.walk()]{.literal} in a [for]{.literal}
loop statement to walk a directory tree, much like how you can use the
[range()]{.literal} function to walk over a range of numbers. Unlike
[range()]{.literal}, the [os.walk()]{.literal} function will return
three values on each iteration through the loop:

-   A string of the current folder's name
-   A list of strings of the folders in the current folder
-   A list of strings of the files in the current folder

(By current folder, I mean the folder for the current iteration of the
[for]{.literal} loop. The current working directory of the program is
*not* changed by [os.walk()]{.literal}.)

[]{#calibre_link-860 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Just like you can choose the variable name
[i]{.literal} in the code [for i in range(10):]{.literal}, you can also
choose the variable names for the three values listed earlier. I usually
use the names [foldername]{.literal}, [subfolders]{.literal}, and
[filenames]{.literal}.

When you run this program, it will output the following:

The current folder is C:\\delicious\
SUBFOLDER OF C:\\delicious: cats\
SUBFOLDER OF C:\\delicious: walnut\
FILE INSIDE C:\\delicious: spam.txt\
\
The current folder is C:\\delicious\\cats\
FILE INSIDE C:\\delicious\\cats: catnames.txt\
FILE INSIDE C:\\delicious\\cats: zophie.jpg\
\
The current folder is C:\\delicious\\walnut\
SUBFOLDER OF C:\\delicious\\walnut: waffles\
\
The current folder is C:\\delicious\\walnut\\waffles\
FILE INSIDE C:\\delicious\\walnut\\waffles: butter.txt.

Since [os.walk()]{.literal} returns lists of strings for the
[subfolder]{.literal} and [filename]{.literal} variables, you can use
these lists in their own [for]{.literal} loops. Replace the
[print()]{.literal} function calls with your own custom code. (Or if you
don't need one or both of them, remove the [for]{.literal} loops.)

### **Compressing Files with the zipfile Module** {#calibre_link-329 .h1}

You may be familiar with ZIP files (with the *.zip* file extension),
which can hold the compressed contents of many other files. Compressing
a file reduces its size, which is useful when transferring it over the
internet. And since a ZIP file can also contain multiple files and
subfolders, it's a handy way to package several files into one. This
single file, called an *archive file*, can then be, say, attached to an
email.

Your Python programs can create and open (or *extract*) ZIP files using
functions in the [zipfile]{.literal} module. Say you have a ZIP file
named *example.zip* that has the contents shown in [Figure
10-2](#calibre_link-1514){.calibre6}.

::: image1
[]{#calibre_link-1514
.calibre6}![image](../images/000008.jpg){.calibre3}
:::

*Figure 10-2: The contents of* example.zip

You can download this ZIP file from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
or just follow along using a ZIP file already on your computer.

#### []{#calibre_link-859 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Reading ZIP Files*** {#calibre_link-330 .h2}

To read the contents of a ZIP file, first you must create a
[ZipFile]{.literal} object (note the capital letters *Z* and *F*).
[ZipFile]{.literal} objects are conceptually similar to the
[File]{.literal} objects you saw returned by the [open()]{.literal}
function in the previous chapter: they are values through which the
program interacts with the file. To create a [ZipFile]{.literal} object,
call the [zipfile.ZipFile()]{.literal} function, passing it a string of
the *.ZIP* file's filename. Note that [zipfile]{.literal} is the name of
the Python module, and [ZipFile()]{.literal} is the name of the
function.

For example, enter the following into the interactive shell:

   \>\>\> [import zipfile, os]{.codestrong1}\
\
   \>\>\> [from pathlib import Path]{.codestrong1}\
   \>\>\> [p = Path.home()]{.codestrong1}\
   \>\>\> [exampleZip = zipfile.ZipFile(p /
\'example.zip\')]{.codestrong1}\
   \>\>\> [exampleZip.namelist()]{.codestrong1}\
   \[\'spam.txt\', \'cats/\', \'cats/catnames.txt\',
\'cats/zophie.jpg\'\]\
   \>\>\> [spamInfo = exampleZip.getinfo(\'spam.txt\')]{.codestrong1}\
   \>\>\> [spamInfo.file_size]{.codestrong1}\
   13908\
   \>\>\> [spamInfo.compress_size]{.codestrong1}\
   3828\
[➊]{.ent} \>\>\> [f\'Compressed file is {round(spamInfo.file_size /
spamInfo]{.codestrong1}\
   [.compress_size, 2)}x smaller!\']{.codestrong1}\
   [)]{.codestrong1}\
   \'Compressed file is 3.63x smaller!\'\
   \>\>\> [exampleZip.close()]{.codestrong1}

A [ZipFile]{.literal} object has a [namelist()]{.literal} method that
returns a list of strings for all the files and folders contained in the
ZIP file. These strings can be passed to the [getinfo()]{.literal}
[ZipFile]{.literal} method to return a [ZipInfo]{.literal} object about
that particular file. [ZipInfo]{.literal} objects have their own
attributes, such as [file_size]{.literal} and [compress_size]{.literal}
in bytes, which hold integers of the original file size and compressed
file size, respectively. While a [ZipFile]{.literal} object represents
an entire archive file, a [ZipInfo]{.literal} object holds useful
information about a *single file* in the archive.

The command at [➊]{.ent} calculates how efficiently *example.zip* is
compressed by dividing the original file size by the compressed file
size and prints this information.

#### ***Extracting from ZIP Files*** {#calibre_link-331 .h2}

The [extractall()]{.literal} method for [ZipFile]{.literal} objects
extracts all the files and folders from a ZIP file into the current
working directory.

   \>\>\> [import zipfile, os]{.codestrong1}\
   \>\>\> [from pathlib import Path]{.codestrong1}\
   \>\>\> [p = Path.home()]{.codestrong1}\
   \>\>\> [exampleZip = zipfile.ZipFile(p /
\'example.zip\')]{.codestrong1}\
[➊]{.ent} \>\>\> [exampleZip.extractall()]{.codestrong1}\
   \>\>\> [exampleZip.close()]{.codestrong1}

[]{#calibre_link-858 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}After running this code, the contents of
*example.zip* will be extracted to *C:\\*. Optionally, you can pass a
folder name to [extractall()]{.literal} to have it extract the files
into a folder other than the current working directory. If the folder
passed to the [extractall()]{.literal} method does not exist, it will be
created. For instance, if you replaced the call at [➊]{.ent} with
[exampleZip.extractall(\'C:\\\\delicious\')]{.literal}, the code would
extract the files from *example.zip* into a newly created
*C:\\delicious* folder.

The [extract()]{.literal} method for [ZipFile]{.literal} objects will
extract a single file from the ZIP file. Continue the interactive shell
example:

\>\>\> [exampleZip.extract(\'spam.txt\')]{.codestrong1}\
\'C:\\\\spam.txt\'\
\>\>\> [exampleZip.extract(\'spam.txt\',
\'C:\\\\some\\\\new\\\\folders\')]{.codestrong1}\
\'C:\\\\some\\\\new\\\\folders\\\\spam.txt\'\
\>\>\> [exampleZip.close()]{.codestrong1}

The string you pass to [extract()]{.literal} must match one of the
strings in the list returned by [namelist()]{.literal}. Optionally, you
can pass a second argument to [extract()]{.literal} to extract the file
into a folder other than the current working directory. If this second
argument is a folder that doesn't yet exist, Python will create the
folder. The value that [extract()]{.literal} returns is the absolute
path to which the file was extracted.

#### ***Creating and Adding to ZIP Files*** {#calibre_link-332 .h2}

To create your own compressed ZIP files, you must open the
[ZipFile]{.literal} object in *write mode* by passing [\'w\']{.literal}
as the second argument. (This is similar to opening a text file in write
mode by passing [\'w\']{.literal} to the [open()]{.literal} function.)

When you pass a path to the [write()]{.literal} method of a
[ZipFile]{.literal} object, Python will compress the file at that path
and add it into the ZIP file. The [write()]{.literal} method's first
argument is a string of the filename to add. The second argument is the
*compression type* parameter, which tells the computer what algorithm it
should use to compress the files; you can always just set this value to
[zipfile.ZIP_DEFLATED]{.literal}. (This specifies the *deflate*
compression algorithm, which works well on all types of data.) Enter the
following into the interactive shell:

\>\>\> [import zipfile]{.codestrong1}\
\>\>\> [newZip = zipfile.ZipFile(\'new.zip\', \'w\')]{.codestrong1}\
\>\>\> [newZip.write(\'spam.txt\',
compress_type=zipfile.ZIP_DEFLATED)]{.codestrong1}\
\>\>\> [newZip.close()]{.codestrong1}

This code will create a new ZIP file named *new.zip* that has the
compressed contents of *spam.txt*.

Keep in mind that, just as with writing to files, write mode will erase
all existing contents of a ZIP file. If you want to simply add files to
an existing ZIP file, pass [\'a\']{.literal} as the second argument to
[zipfile.ZipFile()]{.literal} to open the ZIP file in *append mode*.

### []{#calibre_link-967 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Project: Renaming Files with American-Style Dates to European-Style Dates** {#calibre_link-333 .h1}

Say your boss emails you thousands of files with American-style dates
(MM-DD-YYYY) in their names and needs them renamed to European-style
dates (DD-MM-YYYY). This boring task could take all day to do by hand!
Let's write a program to do it instead.

Here's what the program does:

1.  It searches all the filenames in the current working directory for
    American-style dates.
2.  When one is found, it renames the file with the month and day
    swapped to make it European-style.

This means the code will need to do the following:

1.  Create a regex that can identify the text pattern of American-style
    dates.
2.  Call [os.listdir()]{.literal} to find all the files in the working
    directory.
3.  Loop over each filename, using the regex to check whether it has a
    date.
4.  If it has a date, rename the file with [shutil.move()]{.literal}.

For this project, open a new file editor window and save your code as
*renameDates.py*.

#### ***Step 1: Create a Regex for American-Style Dates*** {#calibre_link-334 .h2}

The first part of the program will need to import the necessary modules
and create a regex that can identify MM-DD-YYYY dates. The to-do
comments will remind you what's left to write in this program. Typing
them as [TODO]{.literal} makes them easy to find using Mu editor's
[CTRL-F]{.small} find feature. Make your code look like the following:

   #! python3\
   # renameDates.py - Renames filenames with American MM-DD-YYYY date
format\
   # to European DD-MM-YYYY.\
\
[➊]{.ent} import shutil, os, re\
\
   # Create a regex that matches files with the American date format.\
[➋]{.ent} datePattern = re.compile(r\"\"\"\^(.\*?) \# all text before
the date\
       ((0\|1)?\\d)-                     # one or two digits for the
month\
       ((0\|1\|2\|3)?\\d)-                 # one or two digits for the
day\
       ((19\|20)\\d\\d)                   # four digits for the year\
       (.\*?)\$                          # all text after the date\
       \"\"\", re.VERBOSE[➌]{.ent})\
\
   # TODO: Loop over the files in the working directory.\
\
   # TODO: Skip files without a date.\
\
[]{#calibre_link-1790 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}   # TODO: Get the different parts of the
filename.\
\
   # TODO: Form the European-style filename.\
\
   # TODO: Get the full, absolute file paths.\
\
   # TODO: Rename the files.

From this chapter, you know the [shutil.move()]{.literal} function can
be used to rename files: its arguments are the name of the file to
rename and the new filename. Because this function exists in the
[shutil]{.literal} module, you must import that module [➊]{.ent}.

But before renaming the files, you need to identify which files you want
to rename. Filenames with dates such as *spam4-4-1984.txt* and
*01-03-2014eggs.zip* should be renamed, while filenames without dates
such as *littlebrother.epub* can be ignored.

You can use a regular expression to identify this pattern. After
importing the [re]{.literal} module at the top, call
[re.compile()]{.literal} to create a [Regex]{.literal} object [➋]{.ent}.
Passing [re.VERBOSE]{.literal} for the second argument [➌]{.ent} will
allow whitespace and comments in the regex string to make it more
readable.

The regular expression string begins with [\^(.\*?)]{.literal} to match
any text at the beginning of the filename that might come before the
date. The [((0\|1)?\\d)]{.literal} group matches the month. The first
digit can be either [0]{.literal} or [1]{.literal}, so the regex matches
[12]{.literal} for December but also [02]{.literal} for February. This
digit is also optional so that the month can be [04]{.literal} or
[4]{.literal} for April. The group for the day is
[((0\|1\|2\|3)?\\d)]{.literal} and follows similar logic; [3]{.literal},
[03]{.literal}, and [31]{.literal} are all valid numbers for days. (Yes,
this regex will accept some invalid dates such as [4-31-2014]{.literal},
[2-29-2013]{.literal}, and [0-15-2014]{.literal}. Dates have a lot of
thorny special cases that can be easy to miss. But for simplicity, the
regex in this program works well enough.)

While 1885 is a valid year, you can just look for years in the 20th or
21st century. This will keep your program from accidentally matching
nondate filenames with a date-like format, such as *10-10-1000.txt*.

The [(.\*?)\$]{.literal} part of the regex will match any text that
comes after the date.

#### ***Step 2: Identify the Date Parts from the Filenames*** {#calibre_link-335 .h2}

Next, the program will have to loop over the list of filename strings
returned from [os.listdir()]{.literal} and match them against the regex.
Any files that do not have a date in them should be skipped. For
filenames that have a date, the matched text will be stored in several
variables. Fill in the first three [TODO]{.literal}s in your program
with the following code:

#! python3\
\# renameDates.py - Renames filenames with American MM-DD-YYYY date
format\
\# to European DD-MM-YYYY.\
\
\--[snip]{.codeitalic1}\--\
\
[\# Loop over the files in the working directory.]{.codestrong1}\
[for amerFilename in os.listdir(\'.\'):]{.codestrong1}\
[]{#calibre_link-1791 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[    mo =
datePattern.search(amerFilename)]{.codestrong1}\
\
[    # Skip files without a date.]{.codestrong1}\
  [➊]{.ent} [if mo == None:]{.codestrong1}\
      [➋]{.ent} [continue]{.codestrong1}\
\
[  ]{.codestrong1}[➌]{.ent} [\# Get the different parts of the
filename.]{.codestrong1}\
[    beforePart = mo.group(1)]{.codestrong1}\
[    monthPart  = mo.group(2)]{.codestrong1}\
[    dayPart    = mo.group(4)]{.codestrong1}\
[    yearPart   = mo.group(6)]{.codestrong1}\
[    afterPart  = mo.group(8)]{.codestrong1}\
\
\--[snip]{.codeitalic1}\--

If the [Match]{.literal} object returned from the [search()]{.literal}
method is [None]{.literal} [➊]{.ent}, then the filename in
[amerFilename]{.literal} does not match the regular expression. The
[continue]{.literal} statement [➋]{.ent} will skip the rest of the loop
and move on to the next filename.

Otherwise, the various strings matched in the regular expression groups
are stored in variables named [beforePart]{.literal},
[monthPart]{.literal}, [dayPart]{.literal}, [yearPart]{.literal}, and
[afterPart]{.literal} [➌]{.ent}. The strings in these variables will be
used to form the European-style filename in the next step.

To keep the group numbers straight, try reading the regex from the
beginning, and count up each time you encounter an opening parenthesis.
Without thinking about the code, just write an outline of the regular
expression. This can help you visualize the groups. Here's an example:

datePattern = re.compile(r\"\"\"\^([1]{.codestrong1}) \# all text before
the date\
    ([2]{.codestrong1} ([3]{.codestrong1}) )-                     # one
or two digits for the month\
    ([4]{.codestrong1} ([5]{.codestrong1}) )-                     # one
or two digits for the day\
    ([6]{.codestrong1} ([7]{.codestrong1}) )                      # four
digits for the year\
    ([8]{.codestrong1})\$                          # all text after the
date\
    \"\"\", re.VERBOSE)

Here, the numbers **1** through **8** represent the groups in the
regular expression you wrote. Making an outline of the regular
expression, with just the parentheses and group numbers, can give you a
clearer understanding of your regex before you move on with the rest of
the program.

#### ***Step 3: Form the New Filename and Rename the Files*** {#calibre_link-336 .h2}

As the final step, concatenate the strings in the variables made in the
previous step with the European-style date: the date comes before the
month. Fill in the three remaining [TODO]{.literal}s in your program
with the following code:

#! python3\
\# renameDates.py - Renames filenames with American MM-DD-YYYY date
format \# to European DD-MM-YYYY.\
\
\--[snip]{.codeitalic1}\--\
\
[]{#calibre_link-806 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}   [  # Form the European-style
filename.]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [euroFilename = beforePart + dayPart +
\'-\' + monthPart + \'-\' + yearPart +]{.codestrong1}\
[                    afterPart]{.codestrong1}\
\
[     # Get the full, absolute file paths.]{.codestrong1}\
[     absWorkingDir = os.path.abspath(\'.\')]{.codestrong1}\
[     amerFilename = os.path.join(absWorkingDir,
amerFilename)]{.codestrong1}\
[     euroFilename = os.path.join(absWorkingDir,
euroFilename)]{.codestrong1}\
\
[     # Rename the files.]{.codestrong1}\
  [➋]{.ent} [print(f\'Renaming \"{amerFilename}\" to
\"{euroFilename}\"\...\')]{.codestrong1}\
  [➌]{.ent} [#shutil.move(amerFilename, euroFilename)   # uncomment
after testing]{.codestrong1}

Store the concatenated string in a variable named
[euroFilename]{.literal} [➊]{.ent}. Then, pass the original filename in
[amerFilename]{.literal} and the new [euroFilename]{.literal} variable
to the [shutil.move()]{.literal} function to rename the file [➌]{.ent}.

This program has the [shutil.move()]{.literal} call commented out and
instead prints the filenames that will be renamed [➋]{.ent}. Running the
program like this first can let you double-check that the files are
renamed correctly. Then you can uncomment the [shutil.move()]{.literal}
call and run the program again to actually rename the files.

#### ***Ideas for Similar Programs*** {#calibre_link-337 .h2}

There are many other reasons you might want to rename a large number of
files.

-   To add a prefix to the start of the filename, such as adding
    *spam\_* to rename *eggs.txt* to *spam_eggs.txt*
-   To change filenames with European-style dates to American-style
    dates
-   To remove the zeros from files such as *spam0042.txt*

### **Project: Backing Up a Folder into a ZIP File** {#calibre_link-338 .h1}

Say you're working on a project whose files you keep in a folder named
*C:\\AlsPythonBook*. You're worried about losing your work, so you'd
like to create ZIP file "snapshots" of the entire folder. You'd like to
keep different versions, so you want the ZIP file's filename to
increment each time it is made; for example, *AlsPythonBook_1.zip*,
*AlsPythonBook_2.zip*, *AlsPythonBook_3.zip*, and so on. You could do
this by hand, but it is rather annoying, and you might accidentally
misnumber the ZIP files' names. It would be much simpler to run a
program that does this boring task for you.

For this project, open a new file editor window and save it as
*backupToZip.py*.

#### ***Step 1: Figure Out the ZIP File's Name*** {#calibre_link-339 .h2}

The code for this program will be placed into a function named
[backupToZip()]{.literal}. This will make it easy to copy and paste the
function []{#calibre_link-1792 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}into other Python programs that need this
functionality. At the end of the program, the function will be called to
perform the backup. Make your program look like this:

   #! python3\
   # backupToZip.py - Copies an entire folder and its contents into\
   # a ZIP file whose filename increments.\
\
[➊]{.ent} import zipfile, os\
\
   def backupToZip(folder):\
       # Back up the entire contents of \"folder\" into a ZIP file.\
\
       folder = os.path.abspath(folder)   # make sure folder is
absolute\
\
       # Figure out the filename this code should use based on\
       # what files already exist.\
    [➋]{.ent} number = 1\
    [➌]{.ent} while True:\
           zipFilename = os.path.basename(folder) + \'\_\' +
str(number) + \'.zip\'\
           if not os.path.exists(zipFilename):\
               break\
           number = number + 1\
\
    [➍]{.ent} \# TODO: Create the ZIP file.\
\
       # TODO: Walk the entire folder tree and compress the files in
each folder.\
       print(\'Done.\')\
\
backupToZip(\'C:\\\\delicious\')

Do the basics first: add the shebang ([#!]{.literal}) line, describe
what the program does, and import the [zipfile]{.literal} and
[os]{.literal} modules [➊]{.ent}.

Define a [backupToZip()]{.literal} function that takes just one
parameter, [folder]{.literal}. This parameter is a string path to the
folder whose contents should be backed up. The function will determine
what filename to use for the ZIP file it will create; then the function
will create the file, walk the [folder]{.literal} folder, and add each
of the subfolders and files to the ZIP file. Write [TODO]{.literal}
comments for these steps in the source code to remind yourself to do
them later [➍]{.ent}.

The first part, naming the ZIP file, uses the base name of the absolute
path of [folder]{.literal}. If the folder being backed up is
*C:\\delicious*, the ZIP file's name should be *delicious_N.zip*, where
*N* = 1 is the first time you run the program, *N* = 2 is the second
time, and so on.

You can determine what *N* should be by checking whether
*delicious_1.zip* already exists, then checking whether
*delicious_2.zip* already exists, and so on. Use a variable named
[number]{.literal} for *N* [➋]{.ent}, and keep incrementing it inside
the loop that calls [os.path.exists()]{.literal} to check whether the
file exists [➌]{.ent}. The first nonexistent filename found will cause
the loop to [break]{.literal}, since it will have found the filename of
the new zip.

#### []{#calibre_link-1793 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 2: Create the New ZIP File*** {#calibre_link-340 .h2}

Next let's create the ZIP file. Make your program look like the
following:

#! python3\
\# backupToZip.py - Copies an entire folder and its contents into\
\# a ZIP file whose filename increments.\
\
\--[snip]{.codeitalic1}\--\
    while True:\
        zipFilename = os.path.basename(folder) + \'\_\' + str(number) +
\'.zip\'\
        if not os.path.exists(zipFilename):\
            break\
        number = number + 1\
\
    [\# Create the ZIP file.]{.codestrong1}\
[    print(f\'Creating {zipFilename}\...\')]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [backupZip = zipfile.ZipFile(zipFilename,
\'w\')]{.codestrong1}\
\
     # TODO: Walk the entire folder tree and compress the files in each
folder.\
     print(\'Done.\')\
\
backupToZip(\'C:\\\\delicious\')

Now that the new ZIP file's name is stored in the
[zipFilename]{.literal} variable, you can call
[zipfile.ZipFile()]{.literal} to actually create the ZIP file [➊]{.ent}.
Be sure to pass [\'w\']{.literal} as the second argument so that the ZIP
file is opened in write mode.

#### ***Step 3: Walk the Directory Tree and Add to the ZIP File*** {#calibre_link-341 .h2}

Now you need to use the [os.walk()]{.literal} function to do the work of
listing every file in the folder and its subfolders. Make your program
look like the following:

#! python3\
\# backupToZip.py - Copies an entire folder and its contents into\
\# a ZIP file whose filename increments.\
\
\--[snip]{.codeitalic1}\--\
\
[     # Walk the entire folder tree and compress the files in each
folder.]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [for foldername, subfolders, filenames in
os.walk(folder):]{.codestrong1}\
[         print(f\'Adding files in {foldername}\...\')]{.codestrong1}\
[         # Add the current folder to the ZIP file.]{.codestrong1}\
[      ]{.codestrong1}[➋]{.ent}
[backupZip.write(foldername)]{.codestrong1}\
\
         [\# Add all the files in this folder to the ZIP
file.]{.codestrong1}\
[      ]{.codestrong1}[➌]{.ent} [for filename in
filenames:]{.codestrong1}\
[            newBase = os.path.basename(folder) + \'\_\']{.codestrong1}\
[            if filename.startswith(newBase) and
filename.endswith(\'.zip\'):]{.codestrong1}\
[                continue   # don\'t back up the backup ZIP
files]{.codestrong1}\
[             backupZip.write(os.path.join(foldername,
filename))]{.codestrong1}\
[     backupZip.close()]{.codestrong1}\
[]{#calibre_link-807 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}     print(\'Done.\')\
\
backupToZip(\'C:\\\\delicious\')

You can use [os.walk()]{.literal} in a [for]{.literal} loop [➊]{.ent},
and on each iteration it will return the iteration's current folder
name, the subfolders in that folder, and the filenames in that folder.

In the [for]{.literal} loop, the folder is added to the ZIP file
[➋]{.ent}. The nested [for]{.literal} loop can go through each filename
in the [filenames]{.literal} list [➌]{.ent}. Each of these is added to
the ZIP file, except for previously made backup ZIPs.

When you run this program, it will produce output that will look
something like this:

Creating delicious_1.zip\...\
Adding files in C:\\delicious\...\
Adding files in C:\\delicious\\cats\...\
Adding files in C:\\delicious\\waffles\...\
Adding files in C:\\delicious\\walnut\...\
Adding files in C:\\delicious\\walnut\\waffles\...\
Done.

The second time you run it, it will put all the files in *C:\\delicious*
into a ZIP file named *delicious_2.zip*, and so on.

#### ***Ideas for Similar Programs*** {#calibre_link-342 .h2}

You can walk a directory tree and add files to compressed ZIP archives
in several other programs. For example, you can write programs that do
the following:

-   Walk a directory tree and archive just files with certain
    extensions, such as *.txt* or *.py*, and nothing else.
-   Walk a directory tree and archive every file except the *.txt* and
    *.py* ones.
-   Find the folder in a directory tree that has the greatest number of
    files or the folder that uses the most disk space.

### **Summary** {#calibre_link-343 .h1}

Even if you are an experienced computer user, you probably handle files
manually with the mouse and keyboard. Modern file explorers make it easy
to work with a few files. But sometimes you'll need to perform a task
that would take hours using your computer's file explorer.

The [os]{.literal} and [shutil]{.literal} modules offer functions for
copying, moving, renaming, and deleting files. When deleting files, you
might want to use the [send2trash]{.literal} module to move files to the
recycle bin or trash rather than permanently deleting them. And when
writing programs that handle files, []{#calibre_link-1794 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}it's a good idea to comment
out the code that does the actual copy/move/rename/delete and add a
[print()]{.literal} call instead so you can run the program and verify
exactly what it will do.

Often you will need to perform these operations not only on files in one
folder but also on every folder in that folder, every folder in those
folders, and so on. The [os.walk()]{.literal} function handles this trek
across the folders for you so that you can concentrate on what your
program needs to do with the files in them.

The [zipfile]{.literal} module gives you a way of compressing and
extracting files in *.ZIP* archives through Python. Combined with the
file-handling functions of [os]{.literal} and [shutil]{.literal},
[zipfile]{.literal} makes it easy to package up several files from
anywhere on your hard drive. These *.ZIP* files are much easier to
upload to websites or send as email attachments than many separate
files.

Previous chapters of this book have provided source code for you to
copy. But when you write your own programs, they probably won't come out
perfectly the first time. The next chapter focuses on some Python
modules that will help you analyze and debug your programs so that you
can quickly get them working correctly.

### **Practice Questions** {#calibre_link-344 .h1}

[1](#calibre_link-1515){#calibre_link-1363 .calibre6}. What is the
difference between [shutil.copy()]{.literal} and
[shutil.copytree()]{.literal}?

[2](#calibre_link-1516){#calibre_link-1364 .calibre6}. What function is
used to rename files?

[3](#calibre_link-1517){#calibre_link-1365 .calibre6}. What is the
difference between the delete functions in the [send2trash]{.literal}
and [shutil]{.literal} modules?

[4](#calibre_link-1518){#calibre_link-1366 .calibre6}.
[ZipFile]{.literal} objects have a [close()]{.literal} method just like
[File]{.literal} objects' [close()]{.literal} method. What
[ZipFile]{.literal} method is equivalent to [File]{.literal} objects'
[open()]{.literal} method?

### **Practice Projects** {#calibre_link-345 .h1}

For practice, write programs to do the following tasks.

#### ***Selective Copy*** {#calibre_link-346 .h2}

Write a program that walks through a folder tree and searches for files
with a certain file extension (such as *.pdf* or *.jpg*). Copy these
files from whatever location they are in to a new folder.

#### ***Deleting Unneeded Files*** {#calibre_link-347 .h2}

It's not uncommon for a few unneeded but humongous files or folders to
take up the bulk of the space on your hard drive. If you're trying to
free up room on your computer, you'll get the most bang for your buck by
deleting the most massive of the unwanted files. But first you have to
find them.

[]{#calibre_link-1795 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Write a program that walks through a folder tree
and searches for exceptionally large files or folders---say, ones that
have a file size of more than 100MB. (Remember that to get a file's
size, you can use [os.path.getsize()]{.literal} from the [os]{.literal}
module.) Print these files with their absolute path to the screen.

#### ***Filling in the Gaps*** {#calibre_link-348 .h2}

Write a program that finds all files with a given prefix, such as
*spam001.txt*, *spam002.txt*, and so on, in a single folder and locates
any gaps in the numbering (such as if there is a *spam001.txt* and
*spam003.txt* but no *spam002.txt*). Have the program rename all the
later files to close this gap.

As an added challenge, write another program that can insert gaps into
numbered files so that a new file can be added.
:::

<div>

[![](/images/prev.png)](../chapter9)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter11)

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
