
## **READING AND WRITING FILES**


![Image](../images/000133.jpg){.calibre3}


Variables are a fine way to store data while your program is running,
but if you want your data to persist even after your program has
finished, you need to save it to a file. You can think of a file's
contents as a single string value, potentially gigabytes in size. In
this chapter, you will learn how to use Python to create, read, and save
files on the hard drive.

### **Files and File Paths** {#calibre_link-290 .h1}

A file has two key properties: a *filename* (usually written as one
word) and a *path*. The path specifies the location of a file on the
computer. For example, there is a file on my Windows laptop with the
filename *project.docx* in the path *C:\\Users\\Al\\Documents*. The part
of the filename after the last period is called the file's *extension*
and tells you a file's type. The filename *project.docx*
[]{#calibre_link-760 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}is a Word document, and *Users*, *Al*, and
*Documents* all refer to *folders* (also called *directories*). Folders
can contain files and other folders. For example, *project.docx* is in
the *Documents* folder, which is inside the *Al* folder, which is inside
the *Users* folder. [Figure 9-1](#calibre_link-1173){.calibre6} shows
this folder organization.


[]{#calibre_link-1173
.calibre6}![image](../images/000009.jpg){.calibre3}


*Figure 9-1: A file in a hierarchy of folders*

The *C:\\* part of the path is the *root folder*, which contains all
other folders. On Windows, the root folder is named *C:\\* and is also
called the *C: drive*. On macOS and Linux, the root folder is */*. In
this book, I'll use the Windows-style root folder, *C:\\*. If you are
entering the interactive shell examples on macOS or Linux, enter
[/]{.literal} instead.

Additional *volumes*, such as a DVD drive or USB flash drive, will
appear differently on different operating systems. On Windows, they
appear as new, lettered root drives, such as *D:\\* or *E:\\*. On macOS,
they appear as new folders under the */Volumes* folder. On Linux, they
appear as new folders under the */mnt* ("mount") folder. Also note that
while folder names and filenames are not case-sensitive on Windows and
macOS, they are case-sensitive on Linux.


**[NOTE]{.notes}**

*Since your system probably has different files and folders on it than
mine, you won't be able to follow every example in this chapter exactly.
Still, try to follow along using folders that exist on your computer.*


#### ***Backslash on Windows and Forward Slash on macOS and Linux*** {#calibre_link-291 .h2}

On Windows, paths are written using backslashes ([\\]{.literal}) as the
separator between folder names. The macOS and Linux operating systems,
however, use the forward slash (*/*) as their path separator. If you
want your programs to work on all operating systems, you will have to
write your Python scripts to handle both cases.

Fortunately, this is simple to do with the [Path()]{.literal} function
in the [pathlib]{.literal} module. If you pass it the string values of
individual file and folder names in your path, [Path()]{.literal} will
return a string with a file path using the correct path separators.
Enter the following into the interactive shell:

\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [Path(\'spam\', \'bacon\', \'eggs\')]{.codestrong1}\
\
WindowsPath(\'spam/bacon/eggs\')\
[]{#calibre_link-761 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [str(Path(\'spam\', \'bacon\',
\'eggs\'))]{.codestrong1}\
\'spam\\\\bacon\\\\eggs\'

Note that the convention for importing [pathlib]{.literal} is to run
[from pathlib import Path]{.literal}, since otherwise we'd have to enter
[pathlib.Path]{.literal} everywhere [Path]{.literal} shows up in our
code. Not only is this extra typing redundant, but it's also redundant.

I'm running this chapter's interactive shell examples on Windows, so
[Path(\'spam\', \'bacon\', \'eggs\')]{.literal} returned a
[WindowsPath]{.literal} object for the joined path, represented as
[WindowsPath(\'spam/bacon/eggs\')]{.literal}. Even though Windows uses
backslashes, the [WindowsPath]{.literal} representation in the
interactive shell displays them using forward slashes, since open source
software developers have historically favored the Linux operating
system.

If you want to get a simple text string of this path, you can pass it to
the [str()]{.literal} function, which in our example returns
[\'spam\\\\bacon\\\\eggs\']{.literal}. (Notice that the backslashes are
doubled because each backslash needs to be escaped by another backslash
character.) If I had called this function on, say, Linux,
[Path()]{.literal} would have returned a [PosixPath]{.literal} object
that, when passed to [str()]{.literal}, would have returned
[\'spam/bacon/eggs\']{.literal}. (*POSIX* is a set of standards for
Unix-like operating systems such as Linux.)

These [Path]{.literal} objects (really, [WindowsPath]{.literal} or
[PosixPath]{.literal} objects, depending on your operating system) will
be passed to several of the file-related functions introduced in this
chapter. For example, the following code joins names from a list of
filenames to the end of a folder's name:

\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [myFiles = \[\'accounts.txt\', \'details.csv\',
\'invite.docx\'\]]{.codestrong1}\
\>\>\> [for filename in myFiles:]{.codestrong1}\
        [print(Path(r\'C:\\Users\\Al\', filename))]{.codestrong1}\
C:\\Users\\Al\\accounts.txt\
C:\\Users\\Al\\details.csv\
C:\\Users\\Al\\invite.docx

On Windows, the backslash separates directories, so you can't use it in
filenames. However, you can use backslashes in filenames on macOS and
Linux. So while [Path(r\'spam\\eggs\')]{.literal} refers to two separate
folders (or a file *eggs* in a folder *spam*) on Windows, the same
command would refer to a single folder (or file) named *spam\\eggs* on
macOS and Linux. For this reason, it's usually a good idea to always use
forward slashes in your Python code (and I'll be doing so for the rest
of this chapter). The [pathlib]{.literal} module will ensure that it
always works on all operating systems.

Note that [pathlib]{.literal} was introduced in Python 3.4 to replace
older [os.path]{.literal} functions. The Python Standard Library modules
support it as of Python 3.6, but if you are working with legacy Python 2
versions, I recommend using [pathlib2]{.literal}, which gives you
[pathlib]{.literal}'s features on Python 2.7. [Appendix
A](#calibre_link-2){.calibre6} has instructions for installing
[pathlib2]{.literal} using pip. Whenever I've replaced an older
[os.path]{.literal} function with [pathlib]{.literal}, I've made a short
note. You can look up the older functions at
*[https://docs.python.org/3/library/os.path.html](https://docs.python.org/3/library/os.path.html){.calibre6}*.

#### []{#calibre_link-1780 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Using the / Operator to Join Paths*** {#calibre_link-292 .h2}

We normally use the [+]{.literal} operator to add two integer or
floating-point numbers, such as in the expression [2 + 2]{.literal},
which evaluates to the integer value [4]{.literal}. But we can also use
the [+]{.literal} operator to concatenate two string values, like the
expression [\'Hello\' + \'World\']{.literal}, which evaluates to the
string value [\'HelloWorld\']{.literal}. Similarly, the [/]{.literal}
operator that we normally use for division can also combine
[Path]{.literal} objects and strings. This is helpful for modifying a
[Path]{.literal} object after you've already created it with the
[Path()]{.literal} function.

For example, enter the following into the interactive shell:

\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [Path(\'spam\') / \'bacon\' / \'eggs\']{.codestrong1}\
WindowsPath(\'spam/bacon/eggs\')\
\>\>\> [Path(\'spam\') / Path(\'bacon/eggs\')]{.codestrong1}\
WindowsPath(\'spam/bacon/eggs\')\
\>\>\> [Path(\'spam\') / Path(\'bacon\', \'eggs\')]{.codestrong1}\
WindowsPath(\'spam/bacon/eggs\')

Using the [/]{.literal} operator with [Path]{.literal} objects makes
joining paths just as easy as string concatenation. It's also safer than
using string concatenation or the [join()]{.literal} method, like we do
in this example:

\>\>\> [homeFolder = r\'C:\\Users\\Al\']{.codestrong1}\
\>\>\> [subFolder = \'spam\']{.codestrong1}\
\>\>\> [homeFolder + \'\\\\\' + subFolder]{.codestrong1}\
\'C:\\\\Users\\\\Al\\\\spam\'\
\>\>\> [\'\\\\\'.join(\[homeFolder, subFolder\])]{.codestrong1}\
\'C:\\\\Users\\\\Al\\\\spam\'

A script that uses this code isn't safe, because its backslashes would
only work on Windows. You could add an [if]{.literal} statement that
checks [sys.platform]{.literal} (which contains a string describing the
computer's operating system) to decide what kind of slash to use, but
applying this custom code everywhere it's needed can be inconsistent and
bug-prone.

The [pathlib]{.literal} module solves these problems by reusing the
[/]{.literal} math division operator to join paths correctly, no matter
what operating system your code is running on. The following example
uses this strategy to join the same paths as in the previous example:

\>\>\> [homeFolder = Path(\'C:/Users/Al\')]{.codestrong1}\
\>\>\> [subFolder = Path(\'spam\')]{.codestrong1}\
\>\>\> [homeFolder / subFolder]{.codestrong1}\
WindowsPath(\'C:/Users/Al/spam\')\
\>\>\> [str(homeFolder / subFolder)]{.codestrong1}\
\'C:\\\\Users\\\\Al\\\\spam\'

The only thing you need to keep in mind when using the [/]{.literal}
operator for joining paths is that one of the first two values must be a
[Path]{.literal} object. []{#calibre_link-840 {http:="" www.idpf.org=""
2007="" ops}type="pagebreak"}Python will give you an error if you try
entering the following into the interactive shell:

\>\>\> [\'spam\' / \'bacon\' / \'eggs\']{.codestrong1}\
Traceback (most recent call last):\
  File \"\<stdin\>\", line 1, in \<module\>\
TypeError: unsupported operand type(s) for /: \'str\' and \'str\'

Python evaluates the [/]{.literal} operator from left to right and
evaluates to a [Path]{.literal} object, so either the first or second
leftmost value must be a [Path]{.literal} object for the entire
expression to evaluate to a [Path]{.literal} object. Here's how the
[/]{.literal} operator and a [Path]{.literal} object evaluate to the
final [Path]{.literal} object.


![image](../images/000091.jpg){.calibre3}


If you see the [TypeError: unsupported operand type(s) for /: \'str\'
and \'str\']{.literal} error message shown previously, you need to put a
[Path]{.literal} object on the left side of the expression.

The [/]{.literal} operator replaces the older [os.path.join()]{.literal}
function, which you can learn more about from
*[https://docs.python.org/3/library/os.path.html#os.path.join](https://docs.python.org/3/library/os.path.html#os.path.join){.calibre6}*.

#### ***The Current Working Directory*** {#calibre_link-293 .h2}

Every program that runs on your computer has a *current working
directory*, or *cwd*. Any filenames or paths that do not begin with the
root folder are assumed to be under the current working directory.


**[NOTE]{.notes}**

*While* folder *is the more modern name for directory, note that*
current working directory *(or just* working directory*) is the standard
term, not "current working folder."*


You can get the current working directory as a string value with the
[Path.cwd()]{.literal} function and change it using
[os.chdir()]{.literal}. Enter the following into the interactive shell:

\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [import os]{.codestrong1}\
\>\>\> [Path.cwd()]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local/Programs/Python/Python37\')\'\
\>\>\> [os.chdir(\'C:\\\\Windows\\\\System32\')]{.codestrong1}\
\>\>\> [Path.cwd()]{.codestrong1}\
WindowsPath(\'C:/Windows/System32\')

[]{#calibre_link-756 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here, the current working directory is set to
*C:\\Users\\Al\\AppData\\Local\\Programs\\Python\\Python37*, so the
filename *project.docx* refers to
*C:\\Users\\Al\\AppData\\Local\\Programs\\Python\\Python37\\project.docx*.
When we change the current working directory to *C:\\Windows\\System32*,
the filename *project.docx* is interpreted as
*C:\\Windows\\System32\\project.docx*.

Python will display an error if you try to change to a directory that
does not exist.

\>\>\> [os.chdir(\'C:/ThisFolderDoesNotExist\')]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<stdin\>\", line 1, in \<module\>\
FileNotFoundError: \[WinError 2\] The system cannot find the file
specified:\
\'C:/ThisFolderDoesNotExist\'

There is no [pathlib]{.literal} function for changing the working
directory, because changing the current working directory while a
program is running can often lead to subtle bugs.

The [os.getcwd()]{.literal} function is the older way of getting the
current working directory as a string.

#### ***The Home Directory*** {#calibre_link-294 .h2}

All users have a folder for their own files on the computer called the
*home directory* or *home folder*. You can get a [Path]{.literal} object
of the home folder by calling [Path.home()]{.literal}:

\>\>\> [Path.home()]{.codestrong1}\
WindowsPath(\'C:/Users/Al\')

The home directories are located in a set place depending on your
operating system:

-   On Windows, home directories are under *C:\\Users*.
-   On Mac, home directories are under */Users*.
-   On Linux, home directories are often under */home*.

Your scripts will almost certainly have permissions to read and write
the files under your home directory, so it's an ideal place to put the
files that your Python programs will work with.

#### ***Absolute vs. Relative Paths*** {#calibre_link-295 .h2}

There are two ways to specify a file path:

-   An *absolute path*, which always begins with the root folder
-   A *relative path*, which is relative to the program's current
    working directory

There are also the *dot* ([.]{.literal}) and *dot-dot* ([..]{.literal})
folders. These are not real folders but special names that can be used
in a path. A single period []{#calibre_link-757 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}("dot") for a folder name
is shorthand for "this directory." Two periods ("dot-dot") means "the
parent folder."

[Figure 9-2](#calibre_link-1174){.calibre6} is an example of some
folders and files. When the current working directory is set to
*C:\\bacon*, the relative paths for the other folders and files are set
as they are in the figure.


[]{#calibre_link-1174
.calibre6}![image](../images/000057.jpg){.calibre3}


*Figure 9-2: The relative paths for folders and files in the working
directory* C:\\bacon

The *.\\* at the start of a relative path is optional. For example,
*.\\spam.txt* and *spam.txt* refer to the same file.

#### ***Creating New Folders Using the os.makedirs() Function*** {#calibre_link-296 .h2}

Your programs can create new folders (directories) with the
[os.makedirs()]{.literal} function. Enter the following into the
interactive shell:

\>\>\> [import os]{.codestrong1}\
\>\>\>
[os.makedirs(\'C:\\\\delicious\\\\walnut\\\\waffles\')]{.codestrong1}

This will create not just the *C:\\delicious* folder but also a *walnut*
folder inside *C:\\delicious* and a *waffles* folder inside
*C:\\delicious\\walnut*. That is, [os.makedirs()]{.literal} will create
any necessary intermediate folders in order to ensure that the full path
exists. [Figure 9-3](#calibre_link-1175){.calibre6} shows this hierarchy
of folders.


[]{#calibre_link-1175
.calibre6}![image](../images/000129.jpg){.calibre3}


*Figure 9-3: The result of
[os.makedirs(\'C:\\\\delicious\\\\walnut\\\\waffles\')]{.literal1}*

[]{#calibre_link-976 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}To make a directory from a [Path]{.literal} object,
call the [mkdir()]{.literal} method. For example, this code will create
a *spam* folder under the home folder on my computer:

\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [Path(r\'C:\\Users\\Al\\spam\').mkdir()]{.codestrong1}

Note that [mkdir()]{.literal} can only make one directory at a time; it
won't make several subdirectories at once like
[os.makedirs()]{.literal}.

#### ***Handling Absolute and Relative Paths*** {#calibre_link-297 .h2}

The [pathlib]{.literal} module provides methods for checking whether a
given path is an absolute path and returning the absolute path of a
relative path.

Calling the [is_absolute()]{.literal} method on a [Path]{.literal}
object will return [True]{.literal} if it represents an absolute path or
[False]{.literal} if it represents a relative path. For example, enter
the following into the interactive shell, using your own files and
folders instead of the exact ones listed here:

\>\>\> [Path.cwd()]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local/Programs/Python/Python37\')\
\>\>\> [Path.cwd().is_absolute()]{.codestrong1}\
True\
\>\>\> [Path(\'spam/bacon/eggs\').is_absolute()]{.codestrong1}\
False

To get an absolute path from a relative path, you can put [Path.cwd()
/]{.literal} in front of the relative [Path]{.literal} object. After
all, when we say "relative path," we almost always mean a path that is
relative to the current working directory. Enter the following into the
interactive shell:

\>\>\> [Path(\'my/relative/path\')]{.codestrong1}\
WindowsPath(\'my/relative/path\')\
\>\>\> [Path.cwd() / Path(\'my/relative/path\')]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local/Programs/Python/Python37/my/relative/\
path\')

If your relative path is relative to another path besides the current
working directory, just replace [Path.cwd()]{.literal} with that other
path instead. The following example gets an absolute path using the home
directory instead of the current working directory:

\>\>\> [Path(\'my/relative/path\')]{.codestrong1}\
WindowsPath(\'my/relative/path\')\
\>\>\> [Path.home() / Path(\'my/relative/path\')]{.codestrong1}\
WindowsPath(\'C:/Users/Al/my/relative/path\')

[]{#calibre_link-774 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [os.path]{.literal} module also has some useful
functions related to absolute and relative paths:

-   Calling [os.path.abspath(]{.literal}[path]{.codeitalic}[)]{.literal}
    will return a string of the absolute path of the argument. This is
    an easy way to convert a relative path into an absolute one.
-   Calling [os.path.isabs(]{.literal}[path]{.codeitalic}[)]{.literal}
    will return [True]{.literal} if the argument is an absolute path and
    [False]{.literal} if it is a relative path.
-   Calling [os.path.relpath(]{.literal}[path]{.codeitalic}[,]{.literal}
    [start]{.codeitalic}[)]{.literal} will return a string of a relative
    path from the [start]{.codeitalic} path to [path]{.codeitalic}. If
    [start]{.codeitalic} is not provided, the current working directory
    is used as the start path.

Try these functions in the interactive shell:

\>\>\> [os.path.abspath(\'.\')]{.codestrong1}\
\
\'C:\\\\Users\\\\Al\\\\AppData\\\\Local\\\\Programs\\\\Python\\\\Python37\'\
\>\>\> [os.path.abspath(\'.\\\\Scripts\')]{.codestrong1}\
\'C:\\\\Users\\\\Al\\\\AppData\\\\Local\\\\Programs\\\\Python\\\\Python37\\\\Scripts\'\
\>\>\> [os.path.isabs(\'.\')]{.codestrong1}\
False\
\>\>\> [os.path.isabs(os.path.abspath(\'.\'))]{.codestrong1}\
True

Since *C:\\Users\\Al\\AppData\\Local\\Programs\\Python\\Python37* was
the working directory when [os.path.abspath()]{.literal} was called, the
"single-dot" folder represents the absolute path
[\'C:\\\\Users\\\\Al\\\\AppData\\\\Local\\\\Programs\\\\Python\\\\Python37\']{.literal}.

Enter the following calls to [os.path.relpath()]{.literal} into the
interactive shell:

\>\>\> [os.path.relpath(\'C:\\\\Windows\', \'C:\\\\\')]{.codestrong1}\
\'Windows\'\
\>\>\> [os.path.relpath(\'C:\\\\Windows\',
\'C:\\\\spam\\\\eggs\')]{.codestrong1}\
\'..\\\\..\\\\Windows\'

When the relative path is within the same parent folder as the path, but
is within subfolders of a different path, such as
[\'C:\\\\Windows\']{.literal} and [\'C:\\\\spam\\\\eggs\']{.literal},
you can use the "dot-dot" notation to return to the parent folder.

#### ***Getting the Parts of a File Path*** {#calibre_link-298 .h2}

Given a [Path]{.literal} object, you can extract the file path's
different parts as strings using several [Path]{.literal} object
attributes. These can be useful for constructing new file paths based on
existing ones. The attributes are diagrammed in [Figure
9-4](#calibre_link-1176){.calibre6}.


[]{#calibre_link-1781 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1176
.calibre6}![image](../images/000033.jpg){.calibre3}


*Figure 9-4: The parts of a Windows (top) and macOS/Linux (bottom) file
path*

The parts of a file path include the following:

-   The *anchor*, which is the root folder of the filesystem
-   On Windows, the *drive*, which is the single letter that often
    denotes a physical hard drive or other storage device
-   The *parent*, which is the folder that contains the file
-   The *name* of the file, made up of the *stem* (or *base name*) and
    the *suffix* (or *extension*)

Note that Windows [Path]{.literal} objects have a [drive]{.literal}
attribute, but macOS and Linux [Path]{.literal} objects don't. The
[drive]{.literal} attribute doesn't include the first backslash.

To extract each attribute from the file path, enter the following into
the interactive shell:

\>\>\> [p = Path(\'C:/Users/Al/spam.txt\')]{.codestrong1}\
\>\>\> [p.anchor]{.codestrong1}\
\'C:\\\\\'\
\>\>\> [p.parent]{.codestrong1} \# This is a Path object, not a string.\
WindowsPath(\'C:/Users/Al\')\
\>\>\> [p.name]{.codestrong1}\
\'spam.txt\'\
\>\>\> [p.stem]{.codestrong1}\
\'spam\'\
\>\>\> [p.suffix]{.codestrong1}\
\'.txt\'\
\>\>\> [p.drive]{.codestrong1}\
\'C:\'

These attributes evaluate to simple string values, except for
[parent]{.literal}, which evaluates to another [Path]{.literal} object.

[]{#calibre_link-809 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [parents]{.literal} attribute (which is
different from the [parent]{.literal} attribute) evaluates to the
ancestor folders of a [Path]{.literal} object with an integer index:

\>\>\> [Path.cwd()]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local/Programs/Python/Python37\')\
\>\>\> [Path.cwd().parents\[0\]]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local/Programs/Python\')\
\>\>\> [Path.cwd().parents\[1\]]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local/Programs\')\
\>\>\> [Path.cwd().parents\[2\]]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData/Local\')\
\>\>\> [Path.cwd().parents\[3\]]{.codestrong1}\
WindowsPath(\'C:/Users/Al/AppData\')\
\>\>\> [Path.cwd().parents\[4\]]{.codestrong1}\
WindowsPath(\'C:/Users/Al\')\
\>\>\> [Path.cwd().parents\[5\]]{.codestrong1}\
WindowsPath(\'C:/Users\')\
\>\>\> [Path.cwd().parents\[6\]]{.codestrong1}\
WindowsPath(\'C:/\')

The older [os.path]{.literal} module also has similar functions for
getting the different parts of a path written in a string value. Calling
[os.path.dirname(]{.literal}[path]{.codeitalic}[)]{.literal} will return
a string of everything that comes before the last slash in the
[path]{.literal} argument. Calling
[os.path.basename(]{.literal}[path]{.codeitalic}[)]{.literal} will
return a string of everything that comes after the last slash in the
[path]{.literal} argument. The directory (or dir) name and base name of
a path are outlined in [Figure 9-5](#calibre_link-1177){.calibre6}.


[]{#calibre_link-1177
.calibre6}![image](../images/000022.jpg){.calibre3}


*Figure 9-5: The base name follows the last slash in a path and is the
same as the filename. The dir name is everything before the last slash.*

For example, enter the following into the interactive shell:

\>\>\> [calcFilePath =
\'C:\\\\Windows\\\\System32\\\\calc.exe\']{.codestrong1}\
\>\>\> [os.path.basename(calcFilePath)]{.codestrong1}\
\'calc.exe\'\
\>\>\> [os.path.dirname(calcFilePath)]{.codestrong1}\
\'C:\\\\Windows\\\\System32\'

If you need a path's dir name and base name together, you can just call
[os.path.split()]{.literal} to get a tuple value with these two strings,
like so:

\>\>\> [calcFilePath =
\'C:\\\\Windows\\\\System32\\\\calc.exe\']{.codestrong1}\
\>\>\> [os.path.split(calcFilePath)]{.codestrong1}\
(\'C:\\\\Windows\\\\System32\', \'calc.exe\')

[]{#calibre_link-810 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Notice that you could create the same tuple by
calling [os.path.dirname()]{.literal} and [os.path.basename()]{.literal}
and placing their return values in a tuple:

\>\>\> [(os.path.dirname(calcFilePath),
os.path.basename(calcFilePath))]{.codestrong1}\
(\'C:\\\\Windows\\\\System32\', \'calc.exe\')

But [os.path.split()]{.literal} is a nice shortcut if you need both
values.

Also, note that [os.path.split()]{.literal} does *not* take a file path
and return a list of strings of each folder. For that, use the
[split()]{.literal} string method and split on the string in
[os.sep]{.literal}. (Note that [sep]{.literal} is in [os]{.literal}, not
[os.path]{.literal}.) The [os.sep]{.literal} variable is set to the
correct folder-separating slash for the computer running the program,
[\'\\\\\']{.literal} on Windows and [\'/\']{.literal} on macOS and
Linux, and splitting on it will return a list of the individual folders.

For example, enter the following into the interactive shell:

\>\>\> [calcFilePath.split(os.sep)]{.codestrong1}\
\[\'C:\', \'Windows\', \'System32\', \'calc.exe\'\]

This returns all the parts of the path as strings.

On macOS and Linux systems, the returned list of folders will begin with
a blank string, like this:

\>\>\> [\'/usr/bin\'.split(os. sep)]{.codestrong1}\
\[\'\', \'usr\', \'bin\'\]

The [split()]{.literal} string method will work to return a list of each
part of the path.

#### ***Finding File Sizes and Folder Contents*** {#calibre_link-299 .h2}

Once you have ways of handling file paths, you can then start gathering
information about specific files and folders. The [os.path]{.literal}
module provides functions for finding the size of a file in bytes and
the files and folders inside a given folder.

-   Calling [os.path.getsize(]{.literal}[path]{.codeitalic}[)]{.literal}
    will return the size in bytes of the file in the [path]{.codeitalic}
    argument.
-   Calling [os.listdir(]{.literal}[path]{.codeitalic}[)]{.literal} will
    return a list of filename strings for each file in the
    [path]{.codeitalic} argument. (Note that this function is in the
    [os]{.literal} module, not [os.path]{.literal}.)

Here's what I get when I try these functions in the interactive shell:

\>\>\>
[os.path.getsize(\'C:\\\\Windows\\\\System32\\\\calc.exe\')]{.codestrong1}\
27648\
\>\>\> [os.listdir(\'C:\\\\Windows\\\\System32\')]{.codestrong1}\
\[\'0409\', \'12520437.cpx\', \'12520850.cpx\', \'5U877.ax\',
\'aaclient.dll\',\
\--[snip]{.codeitalic1}\--\
\'xwtpdui.dll\', \'xwtpw32.dll\', \'zh-CN\', \'zh-HK\', \'zh-TW\',
\'zipfldr.dll\'\]

[]{#calibre_link-1782 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}As you can see, the *calc.exe* program on my
computer is 27,648 bytes in size, and I have a lot of files in
*C:\\Windows\\system32*. If I want to find the total size of all the
files in this directory, I can use [os.path.getsize()]{.literal} and
[os.listdir()]{.literal} together.

\>\>\> [totalSize = 0]{.codestrong1}\
\>\>\> [for filename in
os.listdir(\'C:\\\\Windows\\\\System32\'):]{.codestrong1}\
      [totalSize = totalSize +
os.path.getsize(os.path.join(\'C:\\\\Windows\\\\System32\',
filename))]{.codestrong1}\
\>\>\> [print(totalSize)]{.codestrong1}\
2559970473

As I loop over each filename in the *C:\\Windows\\System32* folder, the
[totalSize]{.literal} variable is incremented by the size of each file.
Notice how when I call [os.path.getsize()]{.literal}, I use
[os.path.join()]{.literal} to join the folder name with the current
filename. The integer that [os.path.getsize()]{.literal} returns is
added to the value of [totalSize]{.literal}. After looping through all
the files, I print [totalSize]{.literal} to see the total size of the
*C:\\Windows\\System32* folder.

#### ***Modifying a List of Files Using Glob Patterns*** {#calibre_link-300 .h2}

If you want to work on specific files, the [glob()]{.literal} method is
simpler to use than [listdir()]{.literal}. Path objects have a
[glob()]{.literal} method for listing the contents of a folder according
to a *glob pattern*. Glob patterns are like a simplified form of regular
expressions often used in command line commands. The [glob()]{.literal}
method returns a generator object (which are beyond the scope of this
book) that you'll need to pass to [list()]{.literal} to easily view in
the interactive shell:

\>\>\> [p = Path(\'C:/Users/Al/Desktop\')]{.codestrong1}\
\>\>\> [p.glob(\'\*\')]{.codestrong1}\
\<generator object Path.glob at 0x000002A6E389DED0\>\
\>\>\> [list(p.glob(\'\*\'))]{.codestrong1} \# Make a list from the
generator.\
\[WindowsPath(\'C:/Users/Al/Desktop/1.png\'),
WindowsPath(\'C:/Users/Al/\
Desktop/22-ap.pdf\'), WindowsPath(\'C:/Users/Al/Desktop/cat.jpg\'),\
  [\--snip\--]{.codeitalic1}\
WindowsPath(\'C:/Users/Al/Desktop/zzz.txt\')\]

The asterisk ([\*]{.literal}) stands for "multiple of any characters,"
so [p.glob(\'\*\')]{.literal} returns a generator of all files in the
path stored in [p]{.literal}.

Like with regexes, you can create complex expressions:

\>\>\> [list(p.glob(\'\*.txt\')]{.codestrong1} \# Lists all text files.\
\[WindowsPath(\'C:/Users/Al/Desktop/foo.txt\'),\
[  \--snip\--]{.codeitalic1}\
WindowsPath(\'C:/Users/Al/Desktop/zzz.txt\')\]

The glob pattern [\'\*.txt\']{.literal} will return files that start
with any combination of characters as long as it ends with the string
[\'.txt\']{.literal}, which is the text file extension.

[]{#calibre_link-949 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}In contrast with the asterisk, the question mark
([?]{.literal}) stands for any single character:

\>\>\> [list(p.glob(\'project?.docx\')]{.codestrong1}\
\[WindowsPath(\'C:/Users/Al/Desktop/project1.docx\'),
WindowsPath(\'C:/Users/Al/\
Desktop/project2.docx\'),\
[  \--snip\--]{.codeitalic1}\
WindowsPath(\'C:/Users/Al/Desktop/project9.docx\')\]

The glob expression [\'project?.docx\']{.literal} will return
[\'project1.docx\']{.literal} or [\'project5.docx\']{.literal}, but it
will not return [\'project10.docx\']{.literal}, because [?]{.literal}
only matches to one character---so it will not match to the
two-character string [\'10\']{.literal}.

Finally, you can also combine the asterisk and question mark to create
even more complex glob expressions, like this:

\>\>\> [list(p.glob(\'\*.?x?\')]{.codestrong1}\
\[WindowsPath(\'C:/Users/Al/Desktop/calc.exe\'),
WindowsPath(\'C:/Users/Al/\
Desktop/foo.txt\'),\
[  \--snip\--]{.codeitalic1}\
WindowsPath(\'C:/Users/Al/Desktop/zzz.txt\')\]

The glob expression [\'\*.?x?\']{.literal} will return files with any
name and any three-character extension where the middle character is an
[\'x\']{.literal}.

By picking out files with specific attributes, the [glob()]{.literal}
method lets you easily specify the files in a directory you want to
perform some operation on. You can use a [for]{.literal} loop to iterate
over the generator that [glob()]{.literal} returns:

\>\>\> [p = Path(\'C:/Users/Al/Desktop\')]{.codestrong1}\
\>\>\> [for textFilePathObj in p.glob(\'\*.txt\'):]{.codestrong1}\
\...     [print(textFilePathObj)]{.codestrong1} [[\# Prints the Path
object as a string.]{.codestrong1}]{.codeitalic1}\
\...     [[\# Do something with the text
file.]{.codestrong1}]{.codeitalic1}\
\...\
C:\\Users\\Al\\Desktop\\foo.txt\
C:\\Users\\Al\\Desktop\\spam.txt\
C:\\Users\\Al\\Desktop\\zzz.txt

If you want to perform some operation on every file in a directory, you
can use either [os.listdir(p)]{.literal} or [p.glob(\'\*\')]{.literal}.

#### ***Checking Path Validity*** {#calibre_link-301 .h2}

Many Python functions will crash with an error if you supply them with a
path that does not exist. Luckily, [Path]{.literal} objects have methods
to check whether a given path exists and whether it is a file or folder.
Assuming that a variable [p]{.literal} holds a [Path]{.literal} object,
you could expect the following:

-   Calling [p.exists()]{.literal} returns [True]{.literal} if the path
    exists or returns [False]{.literal} if it doesn't exist.
-   Calling [p.is_file()]{.literal} returns [True]{.literal} if the path
    exists and is a file, or returns [False]{.literal} otherwise.
-   []{#calibre_link-814 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Calling [p.is_dir()]{.literal} returns
    [True]{.literal} if the path exists and is a directory, or returns
    [False]{.literal} otherwise.

On my computer, here's what I get when I try these methods in the
interactive shell:

\>\>\> [winDir = Path(\'C:/Windows\')]{.codestrong1}\
\>\>\> [notExistsDir =
Path(\'C:/This/Folder/Does/Not/Exist\')]{.codestrong1}\
\>\>\> [calcFile = Path(\'C:/Windows]{.codestrong1}\
[/System32/calc.exe\')]{.codestrong1}\
\>\>\> [winDir.exists()]{.codestrong1}\
True\
\>\>\> [winDir.is_dir()]{.codestrong1}\
True\
\>\>\> [notExistsDir.exists()]{.codestrong1}\
False\
\>\>\> [calcFile.is_file()]{.codestrong1}\
True\
\>\>\> [calcFile.is_dir()]{.codestrong1}\
False

You can determine whether there is a DVD or flash drive currently
attached to the computer by checking for it with the
[exists()]{.literal} method. For instance, if I wanted to check for a
flash drive with the volume named *D:\\* on my Windows computer, I could
do that with the following:

\>\>\> [dDrive = Path(\'D:/\')]{.codestrong1}\
\>\>\> [dDrive.exists()]{.codestrong1}\
False

Oops! It looks like I forgot to plug in my flash drive.

The older [os.path]{.literal} module can accomplish the same task with
the [os.path.exists(]{.literal}[path]{.codeitalic}[)]{.literal},
[os.path.isfile(]{.literal}[path]{.codeitalic}[)]{.literal}, and
[os.path.isdir(]{.literal}[path]{.codeitalic}[)]{.literal} functions,
which act just like their [Path]{.literal} function counterparts. As of
Python 3.6, these functions can accept [Path]{.literal} objects as well
as strings of the file paths.

### **The File Reading/Writing Process** {#calibre_link-302 .h1}

Once you are comfortable working with folders and relative paths, you'll
be able to specify the location of files to read and write. The
functions covered in the next few sections will apply to plaintext
files. *Plaintext files* contain only basic text characters and do not
include font, size, or color information. Text files with the *.txt*
extension or Python script files with the *.py* extension are examples
of plaintext files. These can be opened with Windows's Notepad or
macOS's TextEdit application. Your programs can easily read the contents
of plaintext files and treat them as an ordinary string value.

*Binary files* are all other file types, such as word processing
documents, PDFs, images, spreadsheets, and executable programs. If you
open a binary []{#calibre_link-848 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}file in Notepad or TextEdit, it will look like
scrambled nonsense, like in [Figure 9-6](#calibre_link-1178){.calibre6}.


[]{#calibre_link-1178
.calibre6}![image](../images/000150.jpg){.calibre3}


*Figure 9-6: The Windows [calc.exe]{.literal1} program opened in
Notepad*

Since every different type of binary file must be handled in its own
way, this book will not go into reading and writing raw binary files
directly. Fortunately, many modules make working with binary files
easier---you will explore one of them, the [shelve]{.literal} module,
later in this chapter. The [pathlib]{.literal} module's
[read_text()]{.literal} method returns a string of the full contents of
a text file. Its [write_text()]{.literal} method creates a new text file
(or overwrites an existing one) with the string passed to it. Enter the
following into the interactive shell:

\>\>\> [from pathlib import Path]{.codestrong1}\
\>\>\> [p = Path(\'spam.txt\')]{.codestrong1}\
\>\>\> [p.write_text(\'Hello, world!\')]{.codestrong1}\
13\
\>\>\> [p.read_text()]{.codestrong1}\
\'Hello, world!\'

These method calls create a *spam.txt* file with the content [\'Hello,
world!\']{.literal}. The [13]{.literal} that [write_text()]{.literal}
returns indicates that 13 characters were written to the file. (You can
often disregard this information.) The [read_text()]{.literal} call
reads and returns the contents of our new file as a string: [\'Hello,
world!\']{.literal}.

Keep in mind that these [Path]{.literal} object methods only provide
basic interactions with files. The more common way of writing to a file
involves using the [open()]{.literal} function and file objects. There
are three steps to reading or writing files in Python:

1.  Call the [open()]{.literal} function to return a [File]{.literal}
    object.
2.  Call the [read()]{.literal} or [write()]{.literal} method on the
    [File]{.literal} object.
3.  Close the file by calling the [close()]{.literal} method on the
    [File]{.literal} object.

We'll go over these steps in the following sections.

#### []{#calibre_link-965 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Opening Files with the open() Function*** {#calibre_link-303 .h2}

To open a file with the [open()]{.literal} function, you pass it a
string path indicating the file you want to open; it can be either an
absolute or relative path. The [open()]{.literal} function returns a
[File]{.literal} object.

Try it by creating a text file named *hello.txt* using Notepad or
TextEdit. Type **Hello, world!** as the content of this text file and
save it in your user home folder. Then enter the following into the
interactive shell:

\>\>\> [helloFile = open(Path.home() / \'hello.txt\')]{.codestrong1}

The [open()]{.literal} function can also accept strings. If you're using
Windows, enter the following into the interactive shell:

\>\>\> [helloFile =
open(\'C:\\\\Users\\\\]{.codestrong1}[[your_home_folder]{.codestrong1}]{.codeitalic1}[\\\\hello.txt\')]{.codestrong1}

If you're using macOS, enter the following into the interactive shell
instead:

\>\>\> [helloFile =
open(\'/Users/]{.codestrong1}[[your_home_folder]{.codestrong1}]{.codeitalic1}[/hello.txt\')]{.codestrong1}

Make sure to replace [your_home_folder]{.codeitalic} with your computer
username. For example, my username is *Al*, so I'd enter
[\'C:\\\\Users\\\\Al\\\\hello.txt\']{.literal} on Windows. Note that the
[open()]{.literal} function only accepts [Path]{.literal} objects as of
Python 3.6. In previous versions, you always need to pass a string to
[open()]{.literal}.

Both these commands will open the file in "reading plaintext" mode, or
*read mode* for short. When a file is opened in read mode, Python lets
you only read data from the file; you can't write or modify it in any
way. Read mode is the default mode for files you open in Python. But if
you don't want to rely on Python's defaults, you can explicitly specify
the mode by passing the string value [\'r\']{.literal} as a second
argument to [open()]{.literal}. So [open(\'/Users/Al/hello.txt\',
\'r\')]{.literal} and [open(\'/Users/Al/hello.txt\')]{.literal} do the
same thing.

The call to [open()]{.literal} returns a [File]{.literal} object. A
[File]{.literal} object represents a file on your computer; it is simply
another type of value in Python, much like the lists and dictionaries
you're already familiar with. In the previous example, you stored the
[File]{.literal} object in the variable [helloFile]{.literal}. Now,
whenever you want to read from or write to the file, you can do so by
calling methods on the [File]{.literal} object in [helloFile]{.literal}.

#### ***Reading the Contents of Files*** {#calibre_link-304 .h2}

Now that you have a [File]{.literal} object, you can start reading from
it. If you want to read the entire contents of a file as a string value,
use the [File]{.literal} object's [read()]{.literal} method. Let's
continue with the *hello.txt* [File]{.literal} object you stored in
[helloFile]{.literal}. Enter the following into the interactive shell:

\>\>\> [helloContent = helloFile.read()]{.codestrong1}\
\>\>\> [helloContent]{.codestrong1}\
\'Hello, world!\'

[]{#calibre_link-970 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}If you think of the contents of a file as a single
large string value, the [read()]{.literal} method returns the string
that is stored in the file.

Alternatively, you can use the [readlines()]{.literal} method to get a
*list* of string values from the file, one string for each line of text.
For example, create a file named *sonnet29.txt* in the same directory as
*hello.txt* and write the following text in it:

When, in disgrace with fortune and men\'s eyes,\
I all alone beweep my outcast state,\
And trouble deaf heaven with my bootless cries,\
And look upon myself and curse my fate,

Make sure to separate the four lines with line breaks. Then enter the
following into the interactive shell:

\>\>\> [sonnetFile = open(Path.home() /
\'sonnet29.txt\')]{.codestrong1}\
\>\>\> [sonnetFile.readlines()]{.codestrong1}\
\[When, in disgrace with fortune and men\'s eyes,\\n\', \' I all alone
beweep my\
outcast state,\\n\', And trouble deaf heaven with my bootless
cries,\\n\', And\
look upon myself and curse my fate,\'\]

Note that, except for the last line of the file, each of the string
values ends with a newline character [\\n]{.literal}. A list of strings
is often easier to work with than a single large string value.

#### ***Writing to Files*** {#calibre_link-305 .h2}

Python allows you to write content to a file in a way similar to how the
[print()]{.literal} function "writes" strings to the screen. You can't
write to a file you've opened in read mode, though. Instead, you need to
open it in "write plaintext" mode or "append plaintext" mode, or *write
mode* and *append mode* for short.

Write mode will overwrite the existing file and start from scratch, just
like when you overwrite a variable's value with a new value. Pass
[\'w\']{.literal} as the second argument to [open()]{.literal} to open
the file in write mode. Append mode, on the other hand, will append text
to the end of the existing file. You can think of this as appending to a
list in a variable, rather than overwriting the variable altogether.
Pass [\'a\']{.literal} as the second argument to [open()]{.literal} to
open the file in append mode.

If the filename passed to [open()]{.literal} does not exist, both write
and append mode will create a new, blank file. After reading or writing
a file, call the [close()]{.literal} method before opening the file
again.

Let's put these concepts together. Enter the following into the
interactive shell:

\>\>\> [baconFile = open(\'bacon.txt\', \'w\')   ]{.codestrong1}\
\>\>\> [baconFile.write(\'Hello, world!\\n\')]{.codestrong1}\
13\
\>\>\> [baconFile.close()]{.codestrong1}\
\>\>\> [baconFile = open(\'bacon.txt\', \'a\')]{.codestrong1}\
\>\>\> [baconFile.write(\'Bacon is not a vegetable.\')]{.codestrong1}\
[]{#calibre_link-968 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}25\
\>\>\> [baconFile.close()]{.codestrong1}\
\>\>\> [baconFile = open(\'bacon.txt\')]{.codestrong1}\
\>\>\> [content = baconFile.read()]{.codestrong1}\
\>\>\> [baconFile.close()]{.codestrong1}\
\>\>\> [print(content)]{.codestrong1}\
Hello, world!\
Bacon is not a vegetable.

First, we open *bacon.txt* in write mode. Since there isn't a
*bacon.txt* yet, Python creates one. Calling [write()]{.literal} on the
opened file and passing [write()]{.literal} the string argument
[\'Hello, world! /n\']{.literal} writes the string to the file and
returns the number of characters written, including the newline. Then we
close the file.

To add text to the existing contents of the file instead of replacing
the string we just wrote, we open the file in append mode. We write
[\'Bacon is not a vegetable.\']{.literal} to the file and close it.
Finally, to print the file contents to the screen, we open the file in
its default read mode, call [read()]{.literal}, store the resulting
[File]{.literal} object in [content]{.literal}, close the file, and
print [content]{.literal}.

Note that the [write()]{.literal} method does not automatically add a
newline character to the end of the string like the [print()]{.literal}
function does. You will have to add this character yourself.

As of Python 3.6, you can also pass a [Path]{.literal} object to the
[open()]{.literal} function instead of a string for the filename.

### **Saving Variables with the shelve Module** {#calibre_link-306 .h1}

You can save variables in your Python programs to binary shelf files
using the [shelve]{.literal} module. This way, your program can restore
data to variables from the hard drive. The [shelve]{.literal} module
will let you add Save and Open features to your program. For example, if
you ran a program and entered some configuration settings, you could
save those settings to a shelf file and then have the program load them
the next time it is run.

Enter the following into the interactive shell:

\>\>\> [import shelve]{.codestrong1}\
\>\>\> [shelfFile = shelve.open(\'mydata\')]{.codestrong1}\
\>\>\> [cats = \[\'Zophie\', \'Pooka\', \'Simon\'\]]{.codestrong1}\
\>\>\> [shelfFile\[\'cats\'\] = cats]{.codestrong1}\
\>\>\> [shelfFile.close()]{.codestrong1}

To read and write data using the [shelve]{.literal} module, you first
import [shelve]{.literal}. Call [shelve.open()]{.literal} and pass it a
filename, and then store the returned shelf value in a variable. You can
make changes to the shelf value as if it were a dictionary. When you're
done, call [close()]{.literal} on the shelf value. Here, our shelf value
is stored in [shelfFile]{.literal}. We create a list [cats]{.literal}
and write [shelfFile\[\'cats\'\] = cats]{.literal} to store the list in
[shelfFile]{.literal} as a value associated with the key
[\'cats\']{.literal} (like in a dictionary). Then we call
[close()]{.literal} on [shelfFile]{.literal}. Note that as of Python
3.7, []{#calibre_link-1034 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}you have to pass the [open()]{.literal} shelf
method filenames as strings. You can't pass it [Path]{.literal} object.

After running the previous code on Windows, you will see three new files
in the current working directory: *mydata.bak*, *mydata.dat*, and
*mydata.dir*. On macOS, only a single *mydata.db* file will be created.

These binary files contain the data you stored in your shelf. The format
of these binary files is not important; you only need to know what the
[shelve]{.literal} module does, not how it does it. The module frees you
from worrying about how to store your program's data to a file.

Your programs can use the [shelve]{.literal} module to later reopen and
retrieve the data from these shelf files. Shelf values don't have to be
opened in read or write mode---they can do both once opened. Enter the
following into the interactive shell:

\>\>\> [shelfFile = shelve.open(\'mydata\')]{.codestrong1}\
\>\>\> [type(shelfFile)]{.codestrong1}\
\<class \'shelve.DbfilenameShelf\'\>\
\>\>\> [shelfFile\[\'cats\'\]]{.codestrong1}\
\[\'Zophie\', \'Pooka\', \'Simon\'\]\
\>\>\> [shelfFile.close()]{.codestrong1}

Here, we open the shelf files to check that our data was stored
correctly. Entering [shelfFile\[\'cats\'\]]{.literal} returns the same
list that we stored earlier, so we know that the list is correctly
stored, and we call [close()]{.literal}.

Just like dictionaries, shelf values have [keys()]{.literal} and
[values()]{.literal} methods that will return list-like values of the
keys and values in the shelf. Since these methods return list-like
values instead of true lists, you should pass them to the
[list()]{.literal} function to get them in list form. Enter the
following into the interactive shell:

\>\>\> [shelfFile = shelve.open(\'mydata\')]{.codestrong1}\
\>\>\> [list(shelfFile.keys())]{.codestrong1}\
\[\'cats\'\]\
\>\>\> [list(shelfFile.values())]{.codestrong1}\
\[\[\'Zophie\', \'Pooka\', \'Simon\'\]\]\
\>\>\> [shelfFile.close()]{.codestrong1}

Plaintext is useful for creating files that you'll read in a text editor
such as Notepad or TextEdit, but if you want to save data from your
Python programs, use the [shelve]{.literal} module.

### **Saving Variables with the pprint.pformat() Function** {#calibre_link-307 .h1}

Recall from "[Pretty Printing](#calibre_link-197){.calibre6}" on [page
118](#calibre_link-1050){.calibre6} that the [pprint.pprint()]{.literal}
function will "pretty print" the contents of a list or dictionary to the
screen, while the [pprint.pformat()]{.literal} function will return this
same text as a string instead of printing it. Not only is this string
formatted to be easy to read, but it is also syntactically correct
Python code. Say you have a dictionary stored in a variable and you want
to save this variable and its contents for future use. Using
[]{#calibre_link-979 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[pprint.pformat()]{.literal} will give you a string
that you can write to a *.py* file. This file will be your very own
module that you can import whenever you want to use the variable stored
in it.

For example, enter the following into the interactive shell:

\>\>\> [import pprint]{.codestrong1}\
\>\>\> [cats = \[{\'name\': \'Zophie\', \'desc\': \'chubby\'},
{\'name\': \'Pooka\', \'desc\': \'fluffy\'}\]]{.codestrong1}\
\>\>\> [pprint.pformat(cats)]{.codestrong1}\
\"\[{\'desc\': \'chubby\', \'name\': \'Zophie\'}, {\'desc\': \'fluffy\',
\'name\': \'Pooka\'}\]\"\
\>\>\> [fileObj = open(\'myCats.py\', \'w\')]{.codestrong1}\
\>\>\> [fileObj.write(\'cats = \' + pprint.pformat(cats) +
\'\\n\')]{.codestrong1}\
83\
\>\>\> [fileObj.close()]{.codestrong1}

Here, we import [pprint]{.literal} to let us use
[pprint.pformat()]{.literal}. We have a list of dictionaries, stored in
a variable [cats]{.literal}. To keep the list in [cats]{.literal}
available even after we close the shell, we use
[pprint.pformat()]{.literal} to return it as a string. Once we have the
data in [cats]{.literal} as a string, it's easy to write the string to a
file, which we'll call *myCats.py*.

The modules that an [import]{.literal} statement imports are themselves
just Python scripts. When the string from [pprint.pformat()]{.literal}
is saved to a *.py* file, the file is a module that can be imported just
like any other.

And since Python scripts are themselves just text files with the *.py*
file extension, your Python programs can even generate other Python
programs. You can then import these files into scripts.

\>\>\> [import myCats]{.codestrong1}\
\>\>\> [myCats.cats]{.codestrong1}\
\[{\'name\': \'Zophie\', \'desc\': \'chubby\'}, {\'name\': \'Pooka\',
\'desc\': \'fluffy\'}\]\
\>\>\> [myCats.cats\[0\]]{.codestrong1}\
{\'name\': \'Zophie\', \'desc\': \'chubby\'}\
\>\>\> [myCats.cats\[0\]\[\'name\'\]]{.codestrong1}\
\'Zophie\'

The benefit of creating a *.py* file (as opposed to saving variables
with the [shelve]{.literal} module) is that because it is a text file,
the contents of the file can be read and modified by anyone with a
simple text editor. For most applications, however, saving data using
the [shelve]{.literal} module is the preferred way to save variables to
a file. Only basic data types such as integers, floats, strings, lists,
and dictionaries can be written to a file as simple text.
[File]{.literal} objects, for example, cannot be encoded as text.

### **Project: Generating Random Quiz Files** {#calibre_link-308 .h1}

Say you're a geography teacher with 35 students in your class and you
want to give a pop quiz on US state capitals. Alas, your class has a few
bad eggs in it, and you can't trust the students not to cheat. You'd
like to randomize the []{#calibre_link-1783 {http:="" www.idpf.org=""
2007="" ops}type="pagebreak"}order of questions so that each quiz is
unique, making it impossible for anyone to crib answers from anyone
else. Of course, doing this by hand would be a lengthy and boring
affair. Fortunately, you know some Python.

Here is what the program does:

1.  Creates 35 different quizzes
2.  Creates 50 multiple-choice questions for each quiz, in random order
3.  Provides the correct answer and three random wrong answers for each
    question, in random order
4.  Writes the quizzes to 35 text files
5.  Writes the answer keys to 35 text files

This means the code will need to do the following:

1.  Store the states and their capitals in a dictionary
2.  Call [open()]{.literal}, [write()]{.literal}, and
    [close()]{.literal} for the quiz and answer key text files
3.  Use [random.shuffle()]{.literal} to randomize the order of the
    questions and multiple-choice options

#### ***Step 1: Store the Quiz Data in a Dictionary*** {#calibre_link-309 .h2}

The first step is to create a skeleton script and fill it with your quiz
data. Create a file named *randomQuizGenerator.py*, and make it look
like the following:

   #! python3\
   # randomQuizGenerator.py - Creates quizzes with questions and answers
in\
   # random order, along with the answer key.\
[➊]{.ent} import random\
   # The quiz data. Keys are states and values are their capitals.\
[➋]{.ent} capitals = {\'Alabama\': \'Montgomery\', \'Alaska\':
\'Juneau\', \'Arizona\': \'Phoenix\',\
   \'Arkansas\': \'Little Rock\', \'California\': \'Sacramento\',
\'Colorado\': \'Denver\',\
   \'Connecticut\': \'Hartford\', \'Delaware\': \'Dover\', \'Florida\':
\'Tallahassee\',\
   \'Georgia\': \'Atlanta\', \'Hawaii\': \'Honolulu\', \'Idaho\':
\'Boise\', \'Illinois\':\
   \'Springfield\', \'Indiana\': \'Indianapolis\', \'Iowa\': \'Des
Moines\', \'Kansas\':\
   \'Topeka\', \'Kentucky\': \'Frankfort\', \'Louisiana\': \'Baton
Rouge\', \'Maine\':\
   \'Augusta\', \'Maryland\': \'Annapolis\', \'Massachusetts\':
\'Boston\', \'Michigan\':\
   \'Lansing\', \'Minnesota\': \'Saint Paul\', \'Mississippi\':
\'Jackson\', \'Missouri\':\
   \'Jefferson City\', \'Montana\': \'Helena\', \'Nebraska\':
\'Lincoln\', \'Nevada\':\
   \'Carson City\', \'New Hampshire\': \'Concord\', \'New Jersey\':
\'Trenton\', \'New\
   Mexico\': \'Santa Fe\', \'New York\': \'Albany\',\
   \'North Carolina\': \'Raleigh\', \'North Dakota\': \'Bismarck\',
\'Ohio\': \'Columbus\', \'Oklahoma\': \'Oklahoma City\',\
   \'Oregon\': \'Salem\', \'Pennsylvania\': \'Harrisburg\', \'Rhode
Island\': \'Providence\',\
   \'South Carolina\': \'Columbia\', \'South Dakota\': \'Pierre\',
\'Tennessee\':\
   \'Nashville\', \'Texas\': \'Austin\', \'Utah\': \'Salt Lake City\',
\'Vermont\':\
   \'Montpelier\', \'Virginia\': \'Richmond\', \'Washington\':
\'Olympia\', \'West\
   Virginia\': \'Charleston\', \'Wisconsin\': \'Madison\', \'Wyoming\':
\'Cheyenne\'}\
\
   # Generate 35 quiz files.\
[]{#calibre_link-1784 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[➌]{.ent} for quizNum in range(35):\
       # TODO: Create the quiz and answer key files.\
\
       # TODO: Write out the header for the quiz.\
\
       # TODO: Shuffle the order of the states.\
\
       # TODO: Loop through all 50 states, making a question for each.

Since this program will be randomly ordering the questions and answers,
you'll need to import the [random]{.literal} module [➊]{.ent} to make
use of its functions. The [capitals]{.literal} variable [➋]{.ent}
contains a dictionary with US states as keys and their capitals as
values. And since you want to create 35 quizzes, the code that actually
generates the quiz and answer key files (marked with [TODO]{.literal}
comments for now) will go inside a [for]{.literal} loop that loops 35
times [➌]{.ent}. (This number can be changed to generate any number of
quiz files.)

#### ***Step 2: Create the Quiz File and Shuffle the Question Order*** {#calibre_link-310 .h2}

Now it's time to start filling in those [TODO]{.literal}s.

The code in the loop will be repeated 35 times---once for each quiz---so
you have to worry about only one quiz at a time within the loop. First
you'll create the actual quiz file. It needs to have a unique filename
and should also have some kind of standard header in it, with places for
the student to fill in a name, date, and class period. Then you'll need
to get a list of states in randomized order, which can be used later to
create the questions and answers for the quiz.

Add the following lines of code to *randomQuizGenerator.py*:

#! python3\
\# randomQuizGenerator.py - Creates quizzes with questions and answers
in\
\# random order, along with the answer key.\
\
\--[snip]{.codeitalic1}\--\
\
\# Generate 35 quiz files.\
for quizNum in range(35):\
    [\# Create the quiz and answer key files.]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [quizFile = open(f\'capitalsquiz{quizNum +
1}.txt\', \'w\')]{.codestrong1}\
[  ]{.codestrong1}[➋]{.ent} [answerKeyFile =
open(f\'capitalsquiz_answers{quizNum + 1}.txt\', \'w\')]{.codestrong1}\
     [\# Write out the header for the quiz.]{.codestrong1}\
[  ]{.codestrong1}[➌]{.ent}
[quizFile.write(\'Name:\\n\\nDate:\\n\\nPeriod:\\n\\n\')]{.codestrong1}\
[     quizFile.write((\' \' \* 20) + f\'State Capitals Quiz
(Form{quizNum + 1})\')]{.codestrong1}\
[     quizFile.write(\'\\n\\n\')]{.codestrong1}\
\
     [\# Shuffle the order of the states.]{.codestrong1}\
[     states = list(capitals.keys())]{.codestrong1}\
[  ]{.codestrong1}[➍]{.ent} [random.shuffle(states)]{.codestrong1}\
\
     # TODO: Loop through all 50 states, making a question for each.

[]{#calibre_link-1053 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The filenames for the quizzes will be
*capitalsquiz\<N\>.txt*, where *\<N\>* is a unique number for the quiz
that comes from [quizNum]{.literal}, the [for]{.literal} loop's counter.
The answer key for *capitalsquiz\<N\>.txt* will be stored in a text file
named *capitalsquiz_answers\<N\>.txt*. Each time through the loop, the
[{quizNum + 1}]{.literal} placeholder in [f\'capitalsquiz{quizNum +
1}.txt\']{.literal} and [f\'capitalsquiz_answers{quizNum +
1}.txt\']{.literal} will be replaced by the unique number, so the first
quiz and answer key created will be *capitalsquiz1.txt* and
*capitalsquiz_answers1.txt*. These files will be created with calls to
the [open()]{.literal} function at [➊]{.ent} and [➋]{.ent}, with
[\'w\']{.literal} as the second argument to open them in write mode.

The [write()]{.literal} statements at [➌]{.ent} create a quiz header for
the student to fill out. Finally, a randomized list of US states is
created with the help of the [random.shuffle()]{.literal} function
[➍]{.ent}, which randomly reorders the values in any list that is passed
to it.

#### ***Step 3: Create the Answer Options*** {#calibre_link-311 .h2}

Now you need to generate the answer options for each question, which
will be multiple choice from A to D. You'll need to create another
[for]{.literal} loop---this one to generate the content for each of the
50 questions on the quiz. Then there will be a third [for]{.literal}
loop nested inside to generate the multiple-choice options for each
question. Make your code look like the following:

#! python3\
\# randomQuizGenerator.py - Creates quizzes with questions and answers
in\
\# random order, along with the answer key.\
\
\--[snip]{.codeitalic1}\--\
\
    [\# Loop through all 50 states, making a question for
each.]{.codestrong1}\
[    for questionNum in range(50):]{.codestrong1}\
\
[         # Get right and wrong answers.]{.codestrong1}\
[      ]{.codestrong1}[➊]{.ent} [correctAnswer =
capitals\[states\[questionNum\]\]]{.codestrong1}\
[      ]{.codestrong1}[➋]{.ent} [wrongAnswers =
list(capitals.values())]{.codestrong1}\
[      ]{.codestrong1}[➌]{.ent} [del
wrongAnswers\[wrongAnswers.index(correctAnswer)\]]{.codestrong1}\
[      ]{.codestrong1}[➍]{.ent} [wrongAnswers =
random.sample(wrongAnswers, 3)]{.codestrong1}\
[      ]{.codestrong1}[➎]{.ent} [answerOptions = wrongAnswers +
\[correctAnswer\]]{.codestrong1}\
[      ]{.codestrong1}[➏]{.ent}
[random.shuffle(answerOptions)]{.codestrong1}\
\
[         # TODO: Write the question and answer options to the quiz
file.]{.codestrong1}\
\
[         # TODO: Write the answer key to a file.]{.codestrong1}

The correct answer is easy to get---it's stored as a value in the
[capitals]{.literal} dictionary [➊]{.ent}. This loop will loop through
the states in the shuffled [states]{.literal} list, from
[states\[0\]]{.literal} to [states\[49\]]{.literal}, find each state in
[capitals]{.literal}, and store that state's corresponding capital in
[correctAnswer]{.literal}.

The list of possible wrong answers is trickier. You can get it by
duplicating *all* the values in the [capitals]{.literal} dictionary
[➋]{.ent}, deleting the correct answer [➌]{.ent}, and selecting three
random values from this list [➍]{.ent}. The [random.sample()]{.literal}
function makes it easy to do this selection. Its first argument is the
list you want []{#calibre_link-1785 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}to select from; the second argument is the number
of values you want to select. The full list of answer options is the
combination of these three wrong answers with the correct answers
[➎]{.ent}. Finally, the answers need to be randomized [➏]{.ent} so that
the correct response isn't always choice D.

#### ***Step 4: Write Content to the Quiz and Answer Key Files*** {#calibre_link-312 .h2}

All that is left is to write the question to the quiz file and the
answer to the answer key file. Make your code look like the following:

#! python3\
\# randomQuizGenerator.py - Creates quizzes with questions and answers
in\
\# random order, along with the answer key.\
\
\--[snip]{.codeitalic1}\--\
\
    # Loop through all 50 states, making a question for each.\
    for questionNum in range(50):\
        \--[snip]{.codeitalic1}\--\
\
        [\# Write the question and the answer options to the quiz
file.]{.codestrong1}\
[        quizFile.write(f\'{questionNum + 1}. What is the capital
of]{.codestrong1}\
[{states\[questionNum\]}?\\n\')]{.codestrong1}\
[      ]{.codestrong1}[➊]{.ent} [for i in range(4):]{.codestrong1}\
[          ]{.codestrong1}[➋]{.ent}
[quizFile.write(f\"    {\'ABCD\'\[i\]}. {
answerOptions\[i\]}\\n\")]{.codestrong1}\
[         quizFile.write(\'\\n\')]{.codestrong1}\
\
[         # Write the answer key to a file.]{.codestrong1}\
[      ]{.codestrong1}[➌]{.ent} [answerKeyFile.write(f\"{questionNum +
1}.]{.codestrong1}\
[{\'ABCD\'\[answerOptions.index(correctAnswer)\]}\")]{.codestrong1}\
[     quizFile.close()]{.codestrong1}\
[     answerKeyFile.close()]{.codestrong1}

A [for]{.literal} loop that goes through integers [0]{.literal} to
[3]{.literal} will write the answer options in the
[answerOptions]{.literal} list [➊]{.ent}. The expression
[\'ABCD\'\[i\]]{.literal} at [➋]{.ent} treats the string
[\'ABCD\']{.literal} as an array and will evaluate to
[\'A\']{.literal},[\'B\']{.literal}, [\'C\']{.literal}, and then
[\'D\']{.literal} on each respective iteration through the loop.

In the final line [➌]{.ent}, the expression
[answerOptions.index(correctAnswer)]{.literal} will find the integer
index of the correct answer in the randomly ordered answer options, and
[\'ABCD\'\[answerOptions.index(correctAnswer)\]]{.literal} will evaluate
to the correct answer's letter to be written to the answer key file.

After you run the program, this is how your *capitalsquiz1.txt* file
will look, though of course your questions and answer options may be
different from those shown here, depending on the outcome of your
[random.shuffle()]{.literal} calls:

Name:\
\
Date:\
\
Period:\
\
                    State Capitals Quiz (Form 1)\
\
[]{#calibre_link-980 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}1. What is the capital of West Virginia?\
    A. Hartford\
    B. Santa Fe\
    C. Harrisburg\
    D. Charleston\
\
2. What is the capital of Colorado?\
    A. Raleigh\
    B. Harrisburg\
    C. Denver\
    D. Lincoln\
\
\--[snip]{.codeitalic1}\--

The corresponding *capitalsquiz_answers1.txt* text file will look like
this:

1\. D\
2. C\
3. A\
4. C\
\--[snip]{.codeitalic1}\--

### **Project: Updatable Multi-Clipboard** {#calibre_link-313 .h1}

Let's rewrite the "multi-clipboard" program from [Chapter
6](#calibre_link-207){.calibre6} so that it uses the [shelve]{.literal}
module. The user will now be able to save new strings to load to the
clipboard without having to modify the source code. We'll name this new
program *mcb.pyw* (since "mcb" is shorter to type than
"multi-clipboard"). The *.pyw* extension means that Python won't show a
Terminal window when it runs this program. (See [Appendix
B](#calibre_link-35){.calibre6} for more details.)

The program will save each piece of clipboard text under a keyword. For
example, when you run [py mcb.pyw save spam]{.literal}, the current
contents of the clipboard will be saved with the keyword *spam*. This
text can later be loaded to the clipboard again by running [py mcb.pyw
spam]{.literal}. And if the user forgets what keywords they have, they
can run [py mcb.pyw list]{.literal} to copy a list of all keywords to
the clipboard.

Here's what the program does:

1.  The command line argument for the keyword is checked.
2.  If the argument is [save]{.literal}, then the clipboard contents are
    saved to the keyword.
3.  If the argument is [list]{.literal}, then all the keywords are
    copied to the clipboard.
4.  Otherwise, the text for the keyword is copied to the clipboard.

This means the code will need to do the following:

1.  Read the command line arguments from [sys.argv]{.literal}.
2.  Read and write to the clipboard.
3.  Save and load to a shelf file.

[]{#calibre_link-1786 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}If you use Windows, you can easily run this script
from the Run\... window by creating a batch file named *mcb.bat* with
the following content:

\@pyw.exe C:\\Python34\\mcb.pyw %\*

#### ***Step 1: Comments and Shelf Setup*** {#calibre_link-314 .h2}

Let's start by making a skeleton script with some comments and basic
setup. Make your code look like the following:

   #! python3\
   # mcb.pyw - Saves and loads pieces of text to the clipboard.\
[➊]{.ent} \# Usage: py.exe mcb.pyw save \<keyword\> - Saves clipboard to
keyword.\
   #        py.exe mcb.pyw \<keyword\> - Loads keyword to clipboard.\
   #        py.exe mcb.pyw list - Loads all keywords to clipboard.\
\
[➋]{.ent} import shelve, pyperclip, sys\
\
[➌]{.ent} mcbShelf = shelve.open(\'mcb\')\
\
   # TODO: Save clipboard content.\
\
   # TODO: List keywords and load content.\
\
   mcbShelf.close()

It's common practice to put general usage information in comments at the
top of the file [➊]{.ent}. If you ever forget how to run your script,
you can always look at these comments for a reminder. Then you import
your modules [➋]{.ent}. Copying and pasting will require the
[pyperclip]{.literal} module, and reading the command line arguments
will require the [sys]{.literal} module. The [shelve]{.literal} module
will also come in handy: Whenever the user wants to save a new piece of
clipboard text, you'll save it to a shelf file. Then, when the user
wants to paste the text back to their clipboard, you'll open the shelf
file and load it back into your program. The shelf file will be named
with the prefix *mcb* [➌]{.ent}.

#### ***Step 2: Save Clipboard Content with a Keyword*** {#calibre_link-315 .h2}

The program does different things depending on whether the user wants to
save text to a keyword, load text into the clipboard, or list all the
existing keywords. Let's deal with that first case. Make your code look
like the following:

   #! python3\
   # mcb.pyw - Saves and loads pieces of text to the clipboard.\
   \--[snip]{.codeitalic1}\--\
\
   [\# Save clipboard content.]{.codestrong1}\
[➊]{.ent} [if len(sys.argv) == 3 and sys.argv\[1\].lower() ==
\'save\':]{.codestrong1}\
[         ]{.codestrong1}[➋]{.ent} [mcbShelf\[sys.argv\[2\]\] =
pyperclip.paste()]{.codestrong1}\
   [elif len(sys.argv) == 2:]{.codestrong1}\
[]{#calibre_link-1046 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}   [➌]{.ent} \# TODO: List keywords and load
content.\
\
mcbShelf.close()

If the first command line argument (which will always be at index
[1]{.literal} of the [sys.argv]{.literal} list) is [\'save\']{.literal}
[➊]{.ent}, the second command line argument is the keyword for the
current content of the clipboard. The keyword will be used as the key
for [mcbShelf]{.literal}, and the value will be the text currently on
the clipboard [➋]{.ent}.

If there is only one command line argument, you will assume it is either
[\'list\']{.literal} or a keyword to load content onto the clipboard.
You will implement that code later. For now, just put a [TODO]{.literal}
comment there [➌]{.ent}.

#### ***Step 3: List Keywords and Load a Keyword's Content*** {#calibre_link-316 .h2}

Finally, let's implement the two remaining cases: the user wants to load
clipboard text in from a keyword, or they want a list of all available
keywords. Make your code look like the following:

#! python3\
\# mcb.pyw - Saves and loads pieces of text to the clipboard.\
\--[snip]{.codeitalic1}\--\
\
\# Save clipboard content.\
if len(sys.argv) == 3 and sys.argv\[1\].lower() == \'save\':\
        mcbShelf\[sys.argv\[2\]\] = pyperclip.paste()\
elif len(sys.argv) == 2:\
[     # List keywords and load content.]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [if sys.argv\[1\].lower() ==
\'list\':]{.codestrong1}\
[      ]{.codestrong1}[➋]{.ent}
[pyperclip.copy(str(list(mcbShelf.keys())))]{.codestrong1}\
[     elif sys.argv\[1\] in mcbShelf:]{.codestrong1}\
[      ]{.codestrong1}[➌]{.ent}
[pyperclip.copy(mcbShelf\[sys.argv\[1\]\])]{.codestrong1}\
\
mcbShelf.close()

If there is only one command line argument, first let's check whether
it's [\'list\']{.literal} [➊]{.ent}. If so, a string representation of
the list of shelf keys will be copied to the clipboard [➋]{.ent}. The
user can paste this list into an open text editor to read it.

Otherwise, you can assume the command line argument is a keyword. If
this keyword exists in the [mcbShelf]{.literal} shelf as a key, you can
load the value onto the clipboard [➌]{.ent}.

And that's it! Launching this program has different steps depending on
what operating system your computer uses. See [Appendix
B](#calibre_link-35){.calibre6} for details.

Recall the password locker program you created in [Chapter
6](#calibre_link-207){.calibre6} that stored the passwords in a
dictionary. Updating the passwords required changing the source code of
the program. This isn't ideal, because average users don't feel
comfortable changing source code to update their software. Also, every
time you modify the source code to a program, you run the risk of
accidentally introducing new bugs. By storing the data for a program in
[]{#calibre_link-1787 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}a different place than the code, you can make your
programs easier for others to use and more resistant to bugs.

### **Summary** {#calibre_link-317 .h1}

Files are organized into folders (also called directories), and a path
describes the location of a file. Every program running on your computer
has a current working directory, which allows you to specify file paths
relative to the current location instead of always typing the full (or
absolute) path. The [pathlib]{.literal} and [os.path]{.literal} modules
have many functions for manipulating file paths.

Your programs can also directly interact with the contents of text
files. The [open()]{.literal} function can open these files to read in
their contents as one large string (with the [read()]{.literal} method)
or as a list of strings (with the [readlines()]{.literal} method). The
[open()]{.literal} function can open files in write or append mode to
create new text files or add to existing text files, respectively.

In previous chapters, you used the clipboard as a way of getting large
amounts of text into a program, rather than typing it all in. Now you
can have your programs read files directly from the hard drive, which is
a big improvement, since files are much less volatile than the
clipboard.

In the next chapter, you will learn how to handle the files themselves,
by copying them, deleting them, renaming them, moving them, and more.

### **Practice Questions** {#calibre_link-318 .h1}

[1](#calibre_link-1179){#calibre_link-1352 .calibre6}. What is a
relative path relative to?

[2](#calibre_link-1180){#calibre_link-1353 .calibre6}. What does an
absolute path start with?

[3](#calibre_link-1181){#calibre_link-1354 .calibre6}. What does
[Path(\'C:/Users\') / \'Al\']{.literal} evaluate to on Windows?

[4](#calibre_link-1182){#calibre_link-1355 .calibre6}. What does
[\'C:/Users\' / \'Al\']{.literal} evaluate to on Windows?

[5](#calibre_link-1183){#calibre_link-1356 .calibre6}. What do the
[os.getcwd()]{.literal} and [os.chdir()]{.literal} functions do?

[6](#calibre_link-1184){#calibre_link-1357 .calibre6}. What are the
[.]{.literal} and [..]{.literal} folders?

[7](#calibre_link-1185){#calibre_link-1358 .calibre6}. In
*C:\\bacon\\eggs\\spam.txt*, which part is the dir name, and which part
is the base name?

[8](#calibre_link-1186){#calibre_link-1359 .calibre6}. What are the
three "mode" arguments that can be passed to the [open()]{.literal}
function?

[9](#calibre_link-1187){#calibre_link-1360 .calibre6}. What happens if
an existing file is opened in write mode?

[10](#calibre_link-1188){#calibre_link-1361 .calibre6}. What is the
difference between the [read()]{.literal} and [readlines()]{.literal}
methods?

[11](#calibre_link-1189){#calibre_link-1362 .calibre6}. What data
structure does a shelf value resemble?

### **Practice Projects** {#calibre_link-319 .h1}

For practice, design and write the following programs.

#### []{#calibre_link-1788 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Extending the Multi-Clipboard*** {#calibre_link-320 .h2}

Extend the multi-clipboard program in this chapter so that it has a
[delete \<keyword\>]{.literal} command line argument that will delete a
keyword from the shelf. Then add a [delete]{.literal} command line
argument that will delete *all* keywords.

#### ***Mad Libs*** {#calibre_link-321 .h2}

Create a Mad Libs program that reads in text files and lets the user add
their own text anywhere the word *ADJECTIVE*, *NOUN*, *ADVERB*, or
*VERB* appears in the text file. For example, a text file may look like
this:

The ADJECTIVE panda walked to the NOUN and then VERB. A nearby NOUN was\
unaffected by these events.

The program would find these occurrences and prompt the user to replace
them.

Enter an adjective:\
[silly]{.codestrong1}\
Enter a noun:\
[chandelier]{.codestrong1}\
Enter a verb:\
[screamed]{.codestrong1}\
Enter a noun:\
[pickup truck]{.codestrong1}

The following text file would then be created:

The silly panda walked to the chandelier and then screamed. A nearby
pickup\
truck was unaffected by these events.

The results should be printed to the screen and saved to a new text
file.

#### ***Regex Search*** {#calibre_link-322 .h2}

Write a program that opens all .*txt* files in a folder and searches for
any line that matches a user-supplied regular expression. The results
should be printed to the screen.

