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

[![](/images/prev.png)](../chapter14)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter16)

</div>

::: {#calibre_link-1 .calibre}
## []{#calibre_link-813 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[15]{.big} WORKING WITH PDF AND WORD DOCUMENTS** {#calibre_link-477 .h2b}

::: image
![Image](../images/000059.jpg){.calibre3}
:::

PDF and Word documents are binary files, which makes them much more
complex than plaintext files. In addition to text, they store lots of
font, color, and layout information. If you want your programs to read
or write to PDFs or Word documents, you'll need to do more than simply
pass their filenames to [open()]{.literal}.

Fortunately, there are Python modules that make it easy for you to
interact with PDFs and Word documents. This chapter will cover two such
modules: PyPDF2 and Python-Docx.

### **PDF Documents** {#calibre_link-478 .h1}

*PDF* stands for *Portable Document Format* and uses the *.pdf* file
extension. Although PDFs support many features, this chapter will focus
on the two things you'll be doing most often with them: reading text
content from PDFs and crafting new PDFs from existing documents.

[]{#calibre_link-1055 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The module you'll use to work with PDFs is PyPDF2
version 1.26.0. It's important that you install this version because
future versions of PyPDF2 may be incompatible with the code. To install
it, run [pip install \--user PyPDF2==1.26.0]{.literal} from the command
line. This module name is case sensitive, so make sure the *y* is
lowercase and everything else is uppercase. (Check out [Appendix
A](#calibre_link-2){.calibre6} for full details about installing
third-party modules.) If the module was installed correctly, running
[import PyPDF2]{.literal} in the interactive shell shouldn't display any
errors.

::: sidebar
**THE PROBLEMATIC PDF FORMAT**

While PDF files are great for laying out text in a way that's easy for
people to print and read, they're not straightforward for software to
parse into plaintext. As a result, PyPDF2 might make mistakes when
extracting text from a PDF and may even be unable to open some PDFs at
all. There isn't much you can do about this, unfortunately. PyPDF2 may
simply be unable to work with some of your particular PDF files. That
said, I haven't found any PDF files so far that can't be opened with
PyPDF2.
:::

#### ***Extracting Text from PDFs*** {#calibre_link-479 .h2}

PyPDF2 does not have a way to extract images, charts, or other media
from PDF documents, but it can extract text and return it as a Python
string. To start learning how PyPDF2 works, we'll use it on the example
PDF shown in [Figure 15-1](#calibre_link-3){.calibre6}.

::: image1
[]{#calibre_link-3 .calibre6}![image](../images/000002.jpg){.calibre3}
:::

*Figure 15-1: The PDF page that we will be extracting text from*

[]{#calibre_link-914 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Download this PDF from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
and enter the following into the interactive shell:

   \>\>\> [import PyPDF2]{.codestrong1}\
   \>\>\> [pdfFileObj = open(\'meetingminutes.pdf\',
\'rb\')]{.codestrong1}\
   \>\>\> [pdfReader = PyPDF2.PdfFileReader(pdfFileObj)]{.codestrong1}\
[➊]{.ent} \>\>\> [pdfReader.numPages]{.codestrong1}\
   19\
[➋]{.ent} \>\>\> [pageObj = pdfReader.getPage(0)]{.codestrong1}\
[➌]{.ent} \>\>\> [pageObj.extractText()]{.codestrong1}\
   \'OOFFFFIICCIIAALL  BBOOAARRDD  MMIINNUUTTEESS   Meeting of March 7,\
   2015        \\n     The Board of Elementary and Secondary Education
shall\
   provide leadership and create policies for education that expand
opportunities\
   for children, empower families and communities, and advance Louisiana
in an\
   increasingly competitive global market. BOARD  of ELEMENTARY
and  SECONDARY\
   EDUCATION  \'\
   \>\>\> [pdfFileObj.close()]{.codestrong1}

First, import the [PyPDF2]{.literal} module. Then open
*meetingminutes.pdf* in read binary mode and store it in
[pdfFileObj]{.literal}. To get a [PdfFileReader]{.literal} object that
represents this PDF, call [PyPDF2.PdfFileReader()]{.literal} and pass it
[pdfFileObj]{.literal}. Store this [PdfFileReader]{.literal} object in
[pdfReader]{.literal}.

The total number of pages in the document is stored in the
[numPages]{.literal} attribute of a [PdfFileReader]{.literal} object
[➊]{.ent}. The example PDF has 19 pages, but let's extract text from
only the first page.

To extract text from a page, you need to get a [Page]{.literal} object,
which represents a single page of a PDF, from a
[PdfFileReader]{.literal} object. You can get a [Page]{.literal} object
by calling the [getPage()]{.literal} method [➋]{.ent} on a
[PdfFileReader]{.literal} object and passing it the page number of the
page you're interested in---in our case, 0.

PyPDF2 uses a *zero-based index* for getting pages: The first page is
page 0, the second is [page 1](#calibre_link-4){.calibre6}, and so on.
This is always the case, even if pages are numbered differently within
the document. For example, say your PDF is a three-page excerpt from a
longer report, and its pages are numbered 42, 43, and 44. To get the
first page of this document, you would want to call
[pdfReader.getPage(0)]{.literal}, not [getPage(42)]{.literal} or
[getPage(1)]{.literal}.

Once you have your [Page]{.literal} object, call its
[extractText()]{.literal} method to return a string of the page's text
[➌]{.ent}. The text extraction isn't perfect: The text *Charles E.
"Chas" Roemer, President* from the PDF is absent from the string
returned by [extractText()]{.literal}, and the spacing is sometimes off.
Still, this approximation of the PDF text content may be good enough for
your program.

#### ***Decrypting PDFs*** {#calibre_link-480 .h2}

Some PDF documents have an encryption feature that will keep them from
being read until whoever is opening the document provides a password.
Enter the following into the interactive shell with the PDF you
downloaded, which has been encrypted with the password *rosebud*:

   \>\>\> [import PyPDF2]{.codestrong1}\
   \>\>\> [pdfReader = PyPDF2.PdfFileReader(open(\'encrypted.pdf\',
\'rb\'))]{.codestrong1}\
[]{#calibre_link-902 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[➊]{.ent} \>\>\>
[pdfReader.isEncrypted]{.codestrong1}\
   True\
   \>\>\> [pdfReader.getPage(0)]{.codestrong1}\
[➋]{.ent} Traceback (most recent call last):\
     File \"\<pyshell#173\>\", line 1, in \<module\>\
       pdfReader.getPage()\
     \--[snip]{.codeitalic1}\--\
     File \"C:\\Python34\\lib\\site-packages\\PyPDF2\\pdf.py\", line
1173, in getObject\
       raise utils.PdfReadError(\"file has not been decrypted\")\
   PyPDF2.utils.PdfReadError: file has not been decrypted\
\>\>\> [pdfReader = PyPDF2.PdfFileReader(open(\'encrypted.pdf\',
\'rb\'))]{.codestrong1}\
[➌]{.ent} \>\>\> [pdfReader.decrypt(\'rosebud\')]{.codestrong1}\
   1\
   \>\>\> [pageObj = pdfReader.getPage(0)]{.codestrong1}

All [PdfFileReader]{.literal} objects have an [isEncrypted]{.literal}
attribute that is [True]{.literal} if the PDF is encrypted and
[False]{.literal} if it isn't [➊]{.ent}. Any attempt to call a function
that reads the file before it has been decrypted with the correct
password will result in an error [➋]{.ent}.

::: note
**[NOTE]{.notes}**

*Due to a bug in PyPDF2 version 1.26.0, calling [getPage()]{.codeitalic}
on an encrypted PDF before calling [decrypt()]{.codeitalic} on it causes
future [getPage()]{.codeitalic} calls to fail with the following error:
[IndexError: list index out of range]{.codeitalic}. This is why our
example reopened the file with a new [PdfFileReader]{.codeitalic}
object.*
:::

To read an encrypted PDF, call the [decrypt()]{.literal} function and
pass the password as a string [➌]{.ent}. After you call
[decrypt()]{.literal} with the correct password, you'll see that calling
[getPage()]{.literal} no longer causes an error. If given the wrong
password, the [decrypt()]{.literal} function will return [0]{.literal}
and [getPage()]{.literal} will continue to fail. Note that the
[decrypt()]{.literal} method decrypts only the [PdfFileReader]{.literal}
object, not the actual PDF file. After your program terminates, the file
on your hard drive remains encrypted. Your program will have to call
[decrypt()]{.literal} again the next time it is run.

#### ***Creating PDFs*** {#calibre_link-481 .h2}

PyPDF2's counterpart to [PdfFileReader]{.literal} is
[PdfFileWriter]{.literal}, which can create new PDF files. But PyPDF2
cannot write arbitrary text to a PDF like Python can do with plaintext
files. Instead, PyPDF2's PDF-writing capabilities are limited to copying
pages from other PDFs, rotating pages, overlaying pages, and encrypting
files.

PyPDF2 doesn't allow you to directly edit a PDF. Instead, you have to
create a new PDF and then copy content over from an existing document.
The examples in this section will follow this general approach:

1.  Open one or more existing PDFs (the source PDFs) into
    [PdfFileReader]{.literal} objects.
2.  Create a new [PdfFileWriter]{.literal} object.
3.  []{#calibre_link-786 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Copy pages from the [PdfFileReader]{.literal}
    objects into the [PdfFileWriter]{.literal} object.
4.  Finally, use the [PdfFileWriter]{.literal} object to write the
    output PDF.

Creating a [PdfFileWriter]{.literal} object creates only a value that
represents a PDF document in Python. It doesn't create the actual PDF
file. For that, you must call the [PdfFileWriter]{.literal}'s
[write()]{.literal} method.

The [write()]{.literal} method takes a regular [File]{.literal} object
that has been opened in *write-binary* mode. You can get such a
[File]{.literal} object by calling Python's [open()]{.literal} function
with two arguments: the string of what you want the PDF's filename to be
and [\'wb\']{.literal} to indicate the file should be opened in
write-binary mode.

If this sounds a little confusing, don't worry---you'll see how this
works in the following code examples.

##### **Copying Pages** {#calibre_link-1828 .h2}

You can use PyPDF2 to copy pages from one PDF document to another. This
allows you to combine multiple PDF files, cut unwanted pages, or reorder
pages.

Download *meetingminutes.pdf* and *meetingminutes2.pdf* from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
and place the PDFs in the current working directory. Enter the following
into the interactive shell:

   \>\>\> [import PyPDF2]{.codestrong1}\
   \>\>\> [pdf1File = open(\'meetingminutes.pdf\',
\'rb\')]{.codestrong1}\
   \>\>\> [pdf2File = open(\'meetingminutes2.pdf\',
\'rb\')]{.codestrong1}\
[➊]{.ent} \>\>\> [pdf1Reader =
PyPDF2.PdfFileReader(pdf1File)]{.codestrong1}\
[➋]{.ent} \>\>\> [pdf2Reader =
PyPDF2.PdfFileReader(pdf2File)]{.codestrong1}\
[➌]{.ent} \>\>\> [pdfWriter = PyPDF2.PdfFileWriter()]{.codestrong1}\
\
   \>\>\> [for pageNum in range(pdf1Reader.numPages):]{.codestrong1}\
         [➍]{.ent} [pageObj =
pdf1Reader.getPage(pageNum)]{.codestrong1}\
         [➎]{.ent} [pdfWriter.addPage(pageObj)]{.codestrong1}\
\
   \>\>\> [for pageNum in range(pdf2Reader.numPages):]{.codestrong1}\
         [➍]{.ent} [pageObj =
pdf2Reader.getPage(pageNum)]{.codestrong1}\
         [➎]{.ent} [pdfWriter.addPage(pageObj)]{.codestrong1}\
\
[➏]{.ent} \>\>\> [pdfOutputFile = open(\'combinedminutes.pdf\',
\'wb\')]{.codestrong1}\
   \>\>\> [pdfWriter.write(pdfOutputFile)]{.codestrong1}\
   \>\>\> [pdfOutputFile.close()]{.codestrong1}\
   \>\>\> [pdf1File.close()]{.codestrong1}\
   \>\>\> [pdf2File.close()]{.codestrong1}

Open both PDF files in read binary mode and store the two resulting
[File]{.literal} objects in [pdf1File]{.literal} and
[pdf2File]{.literal}. Call [PyPDF2.PdfFileReader()]{.literal} and pass
it [pdf1File]{.literal} to get a [PdfFileReader]{.literal} object for
*meetingminutes.pdf* [➊]{.ent}. Call it again and pass it
[pdf2File]{.literal} to get a [PdfFileReader]{.literal} object for
*meetingminutes2.pdf* [➋]{.ent}. Then create a new
[PdfFileWriter]{.literal} object, which represents a blank PDF document
[➌]{.ent}.

[]{#calibre_link-787 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Next, copy all the pages from the two source PDFs
and add them to the [PdfFileWriter]{.literal} object. Get the
[Page]{.literal} object by calling [getPage()]{.literal} on a
[PdfFileReader]{.literal} object [➍]{.ent}. Then pass that
[Page]{.literal} object to your PdfFileWriter's [addPage()]{.literal}
method [➎]{.ent}. These steps are done first for [pdf1Reader]{.literal}
and then again for [pdf2Reader]{.literal}. When you're done copying
pages, write a new PDF called *combinedminutes.pdf* by passing a
[File]{.literal} object to the PdfFileWriter's [write()]{.literal}
method [➏]{.ent}.

::: note
**[NOTE]{.notes}**

*PyPDF2 cannot insert pages in the middle of a
[PdfFileWriter]{.codeitalic} object; the [addPage()]{.codeitalic} method
will only add pages to the end.*
:::

You have now created a new PDF file that combines the pages from
*meetingminutes.pdf* and *meetingminutes2.pdf* into a single document.
Remember that the [File]{.literal} object passed to
[PyPDF2.PdfFileReader()]{.literal} needs to be opened in read-binary
mode by passing [\'rb\']{.literal} as the second argument to
[open()]{.literal}. Likewise, the [File]{.literal} object passed to
[PyPDF2.PdfFileWriter()]{.literal} needs to be opened in write-binary
mode with [\'wb\']{.literal}.

##### **Rotating Pages** {#calibre_link-1829 .h2}

The pages of a PDF can also be rotated in 90-degree increments with the
[rotateClockwise()]{.literal} and [rotateCounterClockwise()]{.literal}
methods. Pass one of the integers [90]{.literal}, [180]{.literal}, or
[270]{.literal} to these methods. Enter the following into the
interactive shell, with the *meetingminutes.pdf* file in the current
working directory:

   \>\>\> [import PyPDF2]{.codestrong1}\
   \>\>\> [minutesFile =]{.codestrong1} [open(\'meetingminutes.pdf\',
\'rb\')]{.codestrong1}\
   \>\>\> [pdfReader = PyPDF2.PdfFileReader(minutesFile)]{.codestrong1}\
[➊]{.ent} \>\>\> [page = pdfReader.getPage(0)]{.codestrong1}\
[➋]{.ent} \>\>\> [page.rotateClockwise(90)]{.codestrong1}\
   {\'/Contents\': \[IndirectObject(961, 0), IndirectObject(962, 0),\
   \--[snip]{.codeitalic1}\--\
   }\
   \>\>\> [pdfWriter = PyPDF2.PdfFileWriter()]{.codestrong1}\
   \>\>\> [pdfWriter.addPage(page)]{.codestrong1}\
[➌]{.ent} \>\>\> [resultPdfFile = open(\'rotatedPage.pdf\',
\'wb\')]{.codestrong1}\
   \>\>\> [pdfWriter.write(resultPdfFile)]{.codestrong1}\
   \>\>\> [resultPdfFile.close()]{.codestrong1}\
   \>\>\> [minutesFile.close()]{.codestrong1}

Here we use [getPage(0)]{.literal} to select the first page of the PDF
[➊]{.ent}, and then we call [rotateClockwise(90)]{.literal} on that page
[➋]{.ent}. We write a new PDF with the rotated page and save it as
*rotatedPage.pdf* [➌]{.ent}.

The resulting PDF will have one page, rotated 90 degrees clockwise, as
shown in [Figure 15-2](#calibre_link-5){.calibre6}. The return values
from [rotateClockwise()]{.literal} and
[rotateCounterClockwise()]{.literal} contain a lot of information that
you can ignore.

::: image1
[]{#calibre_link-1057 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-5
.calibre6}![image](../images/000098.jpg){.calibre3}
:::

*Figure 15-2: The* rotatedPage.pdf *file with the page rotated 90
degrees clockwise*

##### **Overlaying Pages** {#calibre_link-1830 .h2}

PyPDF2 can also overlay the contents of one page over another, which is
useful for adding a logo, timestamp, or watermark to a page. With
Python, it's easy to add watermarks to multiple files and only to pages
your program specifies.

Download *watermark.pdf* from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
and place the PDF in the current working directory along with
*meetingminutes.pdf*. Then enter the following into the interactive
shell:

   \>\>\> [import PyPDF2]{.codestrong1}\
   \>\>\> [minutesFile = open(\'meetingminutes.pdf\',
\'rb\')]{.codestrong1}\
[➊]{.ent} \>\>\> [pdfReader =
PyPDF2.PdfFileReader(minutesFile)]{.codestrong1}\
[➋]{.ent} \>\>\> [minutesFirstPage =
pdfReader.getPage(0)]{.codestrong1}\
[➌]{.ent} \>\>\> [pdfWatermarkReader =
PyPDF2.PdfFileReader(open(\'watermark.pdf\', \'rb\'))]{.codestrong1}\
[➍]{.ent} \>\>\>
[minutesFirstPage.mergePage(pdfWatermarkReader.getPage(0))]{.codestrong1}\
[➎]{.ent} \>\>\> [pdfWriter = PyPDF2.PdfFileWriter()]{.codestrong1}\
[➏]{.ent} \>\>\> [pdfWriter.addPage(minutesFirstPage)]{.codestrong1}\
\
[➐]{.ent} \>\>\> [for pageNum in range(1,
pdfReader.numPages):]{.codestrong1}\
           [pageObj = pdfReader.getPage(pageNum)]{.codestrong1}\
           [pdfWriter.addPage(pageObj)]{.codestrong1}\
\
   \>\>\> [resultPdfFile = open(\'watermarkedCover.pdf\',
\'wb\')]{.codestrong1}\
   \>\>\> [pdfWriter.write(resultPdfFile)]{.codestrong1}\
   \>\>\> [minutesFile.close()]{.codestrong1}\
   \>\>\> [resultPdfFile.close()]{.codestrong1}

[]{#calibre_link-1056 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here we make a [PdfFileReader]{.literal} object of
*meetingminutes.pdf* [➊]{.ent}. We call [getPage(0)]{.literal} to get a
[Page]{.literal} object for the first page and store this object in
[minutesFirstPage]{.literal} [➋]{.ent}. We then make a
[PdfFileReader]{.literal} object for *watermark.pdf* [➌]{.ent} and call
[mergePage()]{.literal} on [minutesFirstPage]{.literal} [➍]{.ent}. The
argument we pass to [mergePage()]{.literal} is a [Page]{.literal} object
for the first page of *watermark.pdf*.

Now that we've called [mergePage()]{.literal} on
[minutesFirstPage]{.literal}, [minutesFirstPage]{.literal} represents
the watermarked first page. We make a [PdfFileWriter]{.literal} object
[➎]{.ent} and add the watermarked first page [➏]{.ent}. Then we loop
through the rest of the pages in *meetingminutes.pdf* and add them to
the [PdfFileWriter]{.literal} object [➐]{.ent}. Finally, we open a new
PDF called *watermarkedCover.pdf* and write the contents of the
PdfFileWriter to the new PDF.

[Figure 15-3](#calibre_link-6){.calibre6} shows the results. Our new
PDF, *watermarkedCover.pdf*, has all the contents of the
*meetingminutes.pdf*, and the first page is watermarked.

::: image1
[]{#calibre_link-6 .calibre6}![image](../images/000044.jpg){.calibre3}
:::

*Figure 15-3: The original PDF (left), the watermark PDF (center), and
the merged PDF (right)*

##### **Encrypting PDFs** {#calibre_link-1831 .h2}

A [PdfFileWriter]{.literal} object can also add encryption to a PDF
document. Enter the following into the interactive shell:

   \>\>\> [import PyPDF2]{.codestrong1}\
   \>\>\> [pdfFile = open(\'meetingminutes.pdf\',
\'rb\')]{.codestrong1}\
   \>\>\> [pdfReader = PyPDF2.PdfFileReader(pdfFile)]{.codestrong1}\
   \>\>\> [pdfWriter = PyPDF2.PdfFileWriter()]{.codestrong1}\
   \>\>\> [for pageNum in range(pdfReader.numPages):]{.codestrong1}\
           [pdfWriter.addPage(pdfReader.getPage(pageNum))]{.codestrong1}\
\
[➊]{.ent} \>\>\> [pdfWriter.encrypt(\'swordfish\')]{.codestrong1}\
   \>\>\> [resultPdf = open(\'encryptedminutes.pdf\',
\'wb\')]{.codestrong1}\
   \>\>\> [pdfWriter.write(resultPdf)]{.codestrong1}\
   \>\>\> [resultPdf.close()]{.codestrong1}

Before calling the [write()]{.literal} method to save to a file, call
the [encrypt()]{.literal} method and pass it a password string
[➊]{.ent}. PDFs can have a *user password* (allowing you to view the
PDF) and an *owner password* (allowing you to set permissions for
printing, commenting, extracting text, and other features). The user
password and owner password are the first and second arguments
[]{#calibre_link-852 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}to [encrypt()]{.literal}, respectively. If only one
string argument is passed to [encrypt()]{.literal}, it will be used for
both passwords.

In this example, we copied the pages of *meetingminutes.pdf* to a
[PdfFileWriter]{.literal} object. We encrypted the
[PdfFileWriter]{.literal} with the password *swordfish*, opened a new
PDF called *encryptedminutes.pdf*, and wrote the contents of the
[PdfFileWriter]{.literal} to the new PDF. Before anyone can view
*encryptedminutes.pdf*, they'll have to enter this password. You may
want to delete the original, unencrypted *meetingminutes.pdf* file after
ensuring its copy was correctly encrypted.

### **Project: Combining Select Pages from Many PDFs** {#calibre_link-482 .h1}

Say you have the boring job of merging several dozen PDF documents into
a single PDF file. Each of them has a cover sheet as the first page, but
you don't want the cover sheet repeated in the final result. Even though
there are lots of free programs for combining PDFs, many of them simply
merge entire files together. Let's write a Python program to customize
which pages you want in the combined PDF.

At a high level, here's what the program will do:

1.  Find all PDF files in the current working directory.
2.  Sort the filenames so the PDFs are added in order.
3.  Write each page, excluding the first page, of each PDF to the output
    file.

In terms of implementation, your code will need to do the following:

1.  Call [os.listdir()]{.literal} to find all the files in the working
    directory and remove any non-PDF files.
2.  Call Python's [sort()]{.literal} list method to alphabetize the
    filenames.
3.  Create a [PdfFileWriter]{.literal} object for the output PDF.
4.  Loop over each PDF file, creating a [PdfFileReader]{.literal} object
    for it.
5.  Loop over each page (except the first) in each PDF file.
6.  Add the pages to the output PDF.
7.  Write the output PDF to a file named *allminutes.pdf*.

For this project, open a new file editor tab and save it as
*combinePdfs.py*.

#### ***Step 1: Find All PDF Files*** {#calibre_link-483 .h2}

First, your program needs to get a list of all files with the *.pdf*
extension in the current working directory and sort them. Make your code
look like the following:

   #! python3\
   # combinePdfs.py - Combines all the PDFs in the current working
directory into\
   # into a single PDF.\
\
[➊]{.ent} import PyPDF2, os\
[]{#calibre_link-1832 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}   # Get all the PDF filenames.\
   pdfFiles = \[\]\
   for filename in os.listdir(\'.\'):\
       if filename.endswith(\'.pdf\'):\
         [➋]{.ent} pdfFiles.append(filename)\
[➌]{.ent} pdfFiles.sort(key = str.lower)\
\
[➍]{.ent} pdfWriter = PyPDF2.PdfFileWriter()\
\
   # TODO: Loop through all the PDF files.\
\
   # TODO: Loop through all the pages (except the first) and add them.\
\
   # TODO: Save the resulting PDF to a file.

After the shebang line and the descriptive comment about what the
program does, this code imports the [os]{.literal} and
[PyPDF2]{.literal} modules [➊]{.ent}. The [os.listdir(\'.\')]{.literal}
call will return a list of every file in the current working directory.
The code loops over this list and adds only those files with the *.pdf*
extension to [pdfFiles]{.literal} [➋]{.ent}. Afterward, this list is
sorted in alphabetical order with the [key = str.lower]{.literal}
keyword argument to [sort()]{.literal} [➌]{.ent}.

A [PdfFileWriter]{.literal} object is created to hold the combined PDF
pages [➍]{.ent}. Finally, a few comments outline the rest of the
program.

#### ***Step 2: Open Each PDF*** {#calibre_link-484 .h2}

Now the program must read each PDF file in [pdfFiles]{.literal}. Add the
following to your program:

#! python3\
\# combinePdfs.py - Combines all the PDFs in the current working
directory into\
\# a single PDF.\
\
import PyPDF2, os\
\
\# Get all the PDF filenames.\
pdfFiles = \[\]\
\--[snip]{.codeitalic1}\--\
\
[\# Loop through all the PDF files.]{.codestrong1}\
[for filename in pdfFiles:]{.codestrong1}\
    [pdfFileObj = open(filename, \'rb\')]{.codestrong1}\
    [pdfReader = PyPDF2.PdfFileReader(pdfFileObj)]{.codestrong1}\
    # TODO: Loop through all the pages (except the first) and add them.\
\
\# TODO: Save the resulting PDF to a file.

For each PDF, the loop opens a filename in read-binary mode by calling
[open()]{.literal} with [\'rb\']{.literal} as the second argument. The
[open()]{.literal} call returns a [File]{.literal} object, which gets
passed to [PyPDF2.PdfFileReader()]{.literal} to create a
[PdfFileReader]{.literal} object for that PDF file.

#### []{#calibre_link-1051 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 3: Add Each Page*** {#calibre_link-485 .h2}

For each PDF, you'll want to loop over every page except the first. Add
this code to your program:

#! python3\
\# combinePdfs.py - Combines all the PDFs in the current working
directory into\
\# a single PDF.\
\
import PyPDF2, os\
\
\--[snip]{.codeitalic1}\--\
\
\# Loop through all the PDF files.\
for filename in pdfFiles:\
\--[snip]{.codeitalic1}\--\
     [\# Loop through all the pages (except the first) and add
them.]{.codestrong1}\
  [➊]{.ent} [for pageNum in range(1,
pdfReader.numPages):]{.codestrong1}\
         [pageObj = pdfReader.getPage(pageNum)]{.codestrong1}\
         [pdfWriter.addPage(pageObj)]{.codestrong1}\
\
\# TODO: Save the resulting PDF to a file.

The code inside the [for]{.literal} loop copies each [Page]{.literal}
object individually to the [PdfFileWriter]{.literal} object. Remember,
you want to skip the first page. Since PyPDF2 considers [0]{.literal} to
be the first page, your loop should start at [1]{.literal} [➊]{.ent} and
then go up to, but not include, the integer in
[pdfReader.numPages]{.literal}.

#### ***Step 4: Save the Results*** {#calibre_link-486 .h2}

After these nested [for]{.literal} loops are done looping, the
[pdfWriter]{.literal} variable will contain a [PdfFileWriter]{.literal}
object with the pages for all the PDFs combined. The last step is to
write this content to a file on the hard drive. Add this code to your
program:

#! python3\
\# combinePdfs.py - Combines all the PDFs in the current working
directory into\
\# a single PDF.\
\
import PyPDF2, os\
\
\--[snip]{.codeitalic1}\--\
\
\# Loop through all the PDF files.\
for filename in pdfFiles:\
\--[snip]{.codeitalic1}\--\
    # Loop through all the pages (except the first) and add them.\
    for pageNum in range(1, pdfReader.numPages):\
    \--[snip]{.codeitalic1}\--\
\
[\# Save the resulting PDF to a file.]{.codestrong1}\
[pdfOutput = open(\'allminutes.pdf\', \'wb\')]{.codestrong1}\
[]{#calibre_link-853 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[pdfWriter.write(pdfOutput)]{.codestrong1}\
[pdfOutput.close()]{.codestrong1}

Passing [\'wb\']{.literal} to [open()]{.literal} opens the output PDF
file, *allminutes.pdf*, in write-binary mode. Then, passing the
resulting [File]{.literal} object to the [write()]{.literal} method
creates the actual PDF file. A call to the [close()]{.literal} method
finishes the program.

#### ***Ideas for Similar Programs*** {#calibre_link-487 .h2}

Being able to create PDFs from the pages of other PDFs will let you make
programs that can do the following:

-   Cut out specific pages from PDFs.
-   Reorder pages in a PDF.
-   Create a PDF from only those pages that have some specific text,
    identified by [extractText()]{.literal}.

### **Word Documents** {#calibre_link-488 .h1}

Python can create and modify Word documents, which have the *.docx* file
extension, with the [docx]{.literal} module. You can install the module
by running [pip install \--user -U python-docx==0.8.10]{.literal}.
([Appendix A](#calibre_link-2){.calibre6} has full details on installing
third-party modules.)

::: note
**[NOTE]{.notes}**

*When using pip to first install Python-Docx, be sure to install
[python-docx]{.codeitalic}, not [docx]{.codeitalic}. The package name
[docx]{.codeitalic} is for a different module that this book does not
cover. However, when you are going to import the module from the
[python-docx]{.codeitalic} package, you'll need to run [import
docx]{.codeitalic}, not [import python-docx]{.codeitalic}.*
:::

If you don't have Word, LibreOffice Writer and OpenOffice Writer are
free alternative applications for Windows, macOS, and Linux that can be
used to open *.docx* files. You can download them from
*[https://www.libreoffice.org/](https://www.libreoffice.org/){.calibre6}*
and *[https://openoffice.org/](https://openoffice.org/){.calibre6}*,
respectively. The full documentation for Python-Docx is available at
*[https://python-docx.readthedocs.io/](https://python-docx.readthedocs.io/){.calibre6}*.
Although there is a version of Word for macOS, this chapter will focus
on Word for Windows.

Compared to plaintext, *.docx* files have a lot of structure. This
structure is represented by three different data types in Python-Docx.
At the highest level, a [Document]{.literal} object represents the
entire document. The [Document]{.literal} object contains a list of
[Paragraph]{.literal} objects for the paragraphs in the document. (A new
paragraph begins whenever the user presses [ENTER]{.small} or
[RETURN]{.small} while typing in a Word document.) Each of these
[Paragraph]{.literal} objects contains a list of one or more
[Run]{.literal} objects. The single-sentence paragraph in [Figure
15-4](#calibre_link-7){.calibre6} has four runs.

::: image1
[]{#calibre_link-7 .calibre6}![image](../images/000138.jpg){.calibre3}
:::

*Figure 15-4: The [Run]{.literal1} objects identified in a
[Paragraph]{.literal1} object*

[]{#calibre_link-1833 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The text in a Word document is more than just a
string. It has font, size, color, and other styling information
associated with it. A *style* in Word is a collection of these
attributes. A [Run]{.literal} object is a contiguous run of text with
the same style. A new [Run]{.literal} object is needed whenever the text
style changes.

#### ***Reading Word Documents*** {#calibre_link-489 .h2}

Let's experiment with the [docx]{.literal} module. Download *demo.docx*
from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
and save the document to the working directory. Then enter the following
into the interactive shell:

   \>\>\> [import docx]{.codestrong1}\
[➊]{.ent} \>\>\> [doc = docx.Document(\'demo.docx\')]{.codestrong1}\
[➋]{.ent} \>\>\> [len(doc.paragraphs)]{.codestrong1}\
   7\
[➌]{.ent} \>\>\> [doc.paragraphs\[0\].text]{.codestrong1}\
   \'Document Title\'\
[➍]{.ent} \>\>\> [doc.paragraphs\[1\].text]{.codestrong1}\
   \'A plain paragraph with some bold and some italic\'\
[➎]{.ent} \>\>\> [len(doc.paragraphs\[1\].runs)]{.codestrong1}\
   4\
[➏]{.ent} \>\>\> [doc.paragraphs\[1\].runs\[0\].text]{.codestrong1}\
   \'A plain paragraph with some \'\
[➐]{.ent} \>\>\> [doc.paragraphs\[1\].runs\[1\].text]{.codestrong1}\
   \'bold\'\
[➑]{.ent} \>\>\> [doc.paragraphs\[1\].runs\[2\].text]{.codestrong1}\
   \' and some \'\
[➒]{.ent} \>\>\> [doc.paragraphs\[1\].runs\[3\].text]{.codestrong1}\
   \'italic\'

At [➊]{.ent}, we open a *.docx* file in Python, call
[docx.Document()]{.literal}, and pass the filename *demo.docx*. This
will return a [Document]{.literal} object, which has a
[paragraphs]{.literal} attribute that is a list of [Paragraph]{.literal}
objects. When we call [len()]{.literal} on [doc.paragraphs]{.literal},
it returns [7]{.literal}, which tells us that there are seven
[Paragraph]{.literal} objects in this document [➋]{.ent}. Each of these
[Paragraph]{.literal} objects has a [text]{.literal} attribute that
contains a string of the text in that paragraph (without the style
information). Here, the first [text]{.literal} attribute contains
[\'DocumentTitle\']{.literal} [➌]{.ent}, and the second contains [\'A
plain paragraph with some bold and some italic\']{.literal} [➍]{.ent}.

Each [Paragraph]{.literal} object also has a [runs]{.literal} attribute
that is a list of [Run]{.literal} objects. [Run]{.literal} objects also
have a [text]{.literal} attribute, containing just the text in that
particular run. Let's look at the [text]{.literal} attributes in the
second [Paragraph]{.literal} object, [\'A plain paragraph with some bold
and some italic\']{.literal}. Calling [len()]{.literal} on this
[Paragraph]{.literal} object tells us that there are four
[Run]{.literal} objects [➎]{.ent}. The first run object contains [\'A
plain paragraph with some \']{.literal} [➏]{.ent}. Then, the text
changes to a bold style, so [\'bold\']{.literal} starts a new
[Run]{.literal} object [➐]{.ent}. The text returns to an unbolded style
after that, which results in a third [Run]{.literal} object, [\' and
some \']{.literal} [➑]{.ent}. Finally, the fourth and last
[Run]{.literal} object contains [\'italic\']{.literal} in an italic
style [➒]{.ent}.

With Python-Docx, your Python programs will now be able to read the text
from a *.docx* file and use it just like any other string value.

#### []{#calibre_link-1834 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Getting the Full Text from a .docx File*** {#calibre_link-490 .h2}

If you care only about the text, not the styling information, in the
Word document, you can use the [getText()]{.literal} function. It
accepts a filename of a *.docx* file and returns a single string value
of its text. Open a new file editor tab and enter the following code,
saving it as *readDocx.py*:

#! python3\
\
import docx\
\
def getText(filename):\
    doc = docx.Document(filename)\
    fullText = \[\]\
    for para in doc.paragraphs:\
        fullText.append(para.text)\
    return \'\\n\'.join(fullText)

The [getText()]{.literal} function opens the Word document, loops over
all the [Paragraph]{.literal} objects in the [paragraphs]{.literal}
list, and then appends their text to the list in [fullText]{.literal}.
After the loop, the strings in [fullText]{.literal} are joined together
with newline characters.

The *readDocx.py* program can be imported like any other module. Now if
you just need the text from a Word document, you can enter the
following:

\>\>\> [import readDocx]{.codestrong1}\
\>\>\> [print(readDocx.getText(\'demo.docx\'))]{.codestrong1}\
Document Title\
A plain paragraph with some bold and some italic\
Heading, level 1\
Intense quote\
first item in unordered list\
first item in ordered list

You can also adjust [getText()]{.literal} to modify the string before
returning it. For example, to indent each paragraph, replace the
[append()]{.literal} call in *readDocx.py* with this:

fullText.append([\'  \' +]{.codestrong1} para.text)

To add a double space between paragraphs, change the [join()]{.literal}
call code to this:

return \'\\n[\\n]{.codestrong1}\'.join(fullText)

As you can see, it takes only a few lines of code to write functions
that will read a *.docx* file and return a string of its content to your
liking.

#### []{#calibre_link-837 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Styling Paragraph and Run Objects*** {#calibre_link-491 .h2}

In Word for Windows, you can see the styles by pressing
[CTRL]{.small}-[ALT]{.small}-[SHIFT]{.small}-S to display the Styles
pane, which looks like [Figure 15-5](#calibre_link-8){.calibre6}. On
macOS, you can view the Styles pane by clicking the **View** ▸
**Styles** menu item.

::: image1
[]{#calibre_link-8 .calibre6}![image](../images/000081.jpg){.calibre3}
:::

*Figure 15-5: Display the Styles pane by pressing
[CTRL-ALT-SHIFT]{.small}-S on Windows.*

Word and other word processors use styles to keep the visual
presentation of similar types of text consistent and easy to change. For
example, perhaps you want to set body paragraphs in 11-point, Times New
Roman, left-justified, ragged-right text. You can create a style with
these settings and assign it to all body paragraphs. Then, if you later
want to change the presentation of all body paragraphs in the document,
you can just change the style, and all those paragraphs will be
automatically updated.

For Word documents, there are three types of styles: *paragraph styles*
can be applied to [Paragraph]{.literal} objects, *character styles* can
be applied to [Run]{.literal} objects, and *linked styles* can be
applied to both kinds of objects. You can give both
[Paragraph]{.literal} and [Run]{.literal} objects styles by setting
their [style]{.literal} attribute to a string. This string should be the
name of a style. If [style]{.literal} is set to [None]{.literal}, then
there will be no style associated with the [Paragraph]{.literal} or
[Run]{.literal} object.

The string values for the default Word styles are as follows:

+----------------+----------------+----------------+----------------+
| [\'Norma       | [\'Heading     | [\'List        | [\'List        |
| l\']{.literal} | 5\']{.literal} | Bulle          | Paragrap       |
|                |                | t\']{.literal} | h\']{.literal} |
| [\'Body        | [\'Heading     |                |                |
| Tex            | 6\']{.literal} | [\'List Bullet | [\'MacroTex    |
| t\']{.literal} |                | 2\']{.literal} | t\']{.literal} |
|                | [\'Heading     |                |                |
| [\'Body Text   | 7\']{.literal} | [\'List Bullet | [\'No          |
| 2\']{.literal} |                | 3\']{.literal} | Spacin         |
|                | [\'Heading     |                | g\']{.literal} |
| [\'Body Text   | 8\']{.literal} | [\'List        |                |
| 3\']{.literal} |                | Continu        | [\'Quot        |
|                | [\'Heading     | e\']{.literal} | e\']{.literal} |
| [\'Captio      | 9\']{.literal} |                |                |
| n\']{.literal} |                | [\'List        | [\'Subtitl     |
|                | [\'Intense     | Continue       | e\']{.literal} |
| [\'Heading     | Quot           | 2\']{.literal} |                |
| 1\']{.literal} | e\']{.literal} |                | [\'TOC         |
|                |                | [\'List        | Headin         |
| [\'Heading     | [\'Lis         | Continue       | g\']{.literal} |
| 2\']{.literal} | t\']{.literal} | 3\']{.literal} |                |
|                |                |                | [\'Titl        |
| [\'Heading     | [\'List        | [\'List Number | e\']{.literal} |
| 3\']{.literal} | 2\']{.literal} | \']{.literal}  |                |
|                |                |                |                |
| [\'Heading     | [\'List        | [\'List Number |                |
| 4\']{.literal} | 3\']{.literal} | 2\']{.literal} |                |
|                |                |                |                |
|                |                | [\'List Number |                |
|                |                | 3\']{.literal} |                |
+----------------+----------------+----------------+----------------+

[]{#calibre_link-1079 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}When using a linked style for a [Run]{.literal}
object, you will need to add [\' Char\']{.literal} to the end of its
name. For example, to set the Quote linked style for a
[Paragraph]{.literal} object, you would use [paragraphObj.style =
\'Quote\']{.literal}, but for a [Run]{.literal} object, you would use
[runObj.style = \'Quote Char\']{.literal}.

In the current version of Python-Docx (0.8.10), the only styles that can
be used are the default Word styles and the styles in the opened
*.docx*. New styles cannot be created---though this may change in future
versions of Python-Docx.

#### ***Creating Word Documents with Nondefault Styles*** {#calibre_link-492 .h2}

If you want to create Word documents that use styles beyond the default
ones, you will need to open Word to a blank Word document and create the
styles yourself by clicking the **New Style** button at the bottom of
the Styles pane ([Figure 15-6](#calibre_link-9){.calibre6} shows this on
Windows).

This will open the Create New Style from Formatting dialog, where you
can enter the new style. Then, go back into the interactive shell and
open this blank Word document with [docx.Document()]{.literal}, using it
as the base for your Word document. The name you gave this style will
now be available to use with Python-Docx.

::: image1
[]{#calibre_link-9 .calibre6}![image](../images/000029.jpg){.calibre3}
:::

*Figure 15-6: The New Style button (left) and the Create New Style from
Formatting dialog (right)*

#### ***Run Attributes*** {#calibre_link-493 .h2}

Runs can be further styled using [text]{.literal} attributes. Each
attribute can be set to one of three values: [True]{.literal} (the
attribute is always enabled, no matter what other styles are applied to
the run), [False]{.literal} (the attribute is always disabled), or
[None]{.literal} (defaults to whatever the run's style is set to).

[Table 15-1](#calibre_link-10){.calibre6} lists the [text]{.literal}
attributes that can be set on [Run]{.literal} objects.

[]{#calibre_link-1835 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}**Table 15-1:** [Run]{.literal1} Object
[text]{.literal1} Attributes

  **Attribute**               **Description**
  --------------------------- ---------------------------------------------------------------------------------
  [bold]{.literal}            The text appears in bold.
  [italic]{.literal}          The text appears in italic.
  [underline]{.literal}       The text is underlined.
  [strike]{.literal}          The text appears with strikethrough.
  [double_strike]{.literal}   The text appears with double strikethrough.
  [all_caps]{.literal}        The text appears in capital letters.
  [small_caps]{.literal}      The text appears in capital letters, with lowercase letters two points smaller.
  [shadow]{.literal}          The text appears with a shadow.
  [outline]{.literal}         The text appears outlined rather than solid.
  [rtl]{.literal}             The text is written right-to-left.
  [imprint]{.literal}         The text appears pressed into the page.
  [emboss]{.literal}          The text appears raised off the page in relief.

For example, to change the styles of *demo.docx*, enter the following
into the interactive shell:

\>\>\> [import docx]{.codestrong1}\
\>\>\> [doc = docx.Document(\'demo.docx\')]{.codestrong1}\
\>\>\> [doc.paragraphs\[0\].text]{.codestrong1}\
\'Document Title\'\
\>\>\> [doc.paragraphs\[0\].style \# The exact id may be
different:]{.codestrong1}\
\_ParagraphStyle(\'Title\') id: 3095631007984\
\>\>\> [doc.paragraphs\[0\].style = \'Normal\']{.codestrong1}\
\>\>\> [doc.paragraphs\[1\].text]{.codestrong1}\
\'A plain paragraph with some bold and some italic\'\
\>\>\> [(doc.paragraphs\[1\].runs\[0\].text,
doc.paragraphs\[1\].runs\[1\].text, doc.\
paragraphs\[1\].runs\[2\].text,
doc.paragraphs\[1\].runs\[3\].text)]{.codestrong1}\
(\'A plain paragraph with some \', \'bold\', \' and some \',
\'italic\')\
\>\>\> [doc.paragraphs\[1\].runs\[0\].style =
\'QuoteChar\']{.codestrong1}\
\>\>\> [doc.paragraphs\[1\].runs\[1\].underline = True]{.codestrong1}\
\>\>\> [doc.paragraphs\[1\].runs\[3\].underline = True]{.codestrong1}\
\>\>\> [doc.save(\'restyled.docx\')]{.codestrong1}

Here, we use the [text]{.literal} and [style]{.literal} attributes to
easily see what's in the paragraphs in our document. We can see that
it's simple to divide a paragraph into runs and access each run
individually. So we get the first, second, and fourth runs in the second
paragraph; style each run; and save the results to a new document.

The words *Document Title* at the top of *restyled.docx* will have the
Normal style instead of the Title style, the [Run]{.literal} object for
the text *A plain paragraph with some* will have the QuoteChar style,
and the two [Run]{.literal} objects for the words *bold*
[]{#calibre_link-779 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}and *italic* will have their [underline]{.literal}
attributes set to [True]{.literal}. [Figure
15-7](#calibre_link-11){.calibre6} shows how the styles of paragraphs
and runs look in *restyled.docx*.

::: image1
[]{#calibre_link-11 .calibre6}![image](../images/000120.jpg){.calibre3}
:::

*Figure 15-7: The* restyled.docx *file*

You can find more complete documentation on Python-Docx's use of styles
at *https://python-docx.readthedocs.io/en/latest/user/styles.html*.

#### ***Writing Word Documents*** {#calibre_link-494 .h2}

Enter the following into the interactive shell:

\>\>\> [import docx]{.codestrong1}\
\>\>\> [doc = docx.Document()]{.codestrong1}\
\>\>\> [doc.add_paragraph(\'Hello, world!\')]{.codestrong1}\
\<docx.text.Paragraph object at 0x0000000003B56F60\>\
\>\>\> [doc.save(\'helloworld.docx\')]{.codestrong1}

To create your own *.docx* file, call [docx.Document()]{.literal} to
return a new, blank Word [Document]{.literal} object. The
[add_paragraph()]{.literal} document method adds a new paragraph of text
to the document and returns a reference to the [Paragraph]{.literal}
object that was added. When you're done adding text, pass a filename
string to the [save()]{.literal} document method to save the
[Document]{.literal} object to a file.

This will create a file named *helloworld.docx* in the current working
directory that, when opened, looks like [Figure
15-8](#calibre_link-12){.calibre6}.

::: image1
[]{#calibre_link-12 .calibre6}![image](../images/000065.jpg){.calibre3}
:::

*Figure 15-8: The Word document created using [add_paragraph(\'Hello,
world!\')]{.literal1}*

[]{#calibre_link-780 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can add paragraphs by calling the
[add_paragraph()]{.literal} method again with the new paragraph's text.
Or to add text to the end of an existing paragraph, you can call the
paragraph's [add_run()]{.literal} method and pass it a string. Enter the
following into the interactive shell:

\>\>\> [import docx]{.codestrong1}\
\>\>\> [doc = docx.Document()]{.codestrong1}\
\>\>\> [doc.add_paragraph(\'Hello world!\')]{.codestrong1}\
\<docx.text.Paragraph object at 0x000000000366AD30\>\
\>\>\> [paraObj1 = doc.add_paragraph(\'This is a second
paragraph.\')]{.codestrong1}\
\>\>\> [paraObj2 = doc.add_paragraph(\'This is a yet another
paragraph.\')]{.codestrong1}\
\>\>\> [paraObj1.add_run(\' This text is being added to the second
paragraph.\')]{.codestrong1}\
\<docx.text.Run object at 0x0000000003A2C860\>\
\>\>\> [doc.save(\'multipleParagraphs.docx\')]{.codestrong1}

The resulting document will look like [Figure
15-9](#calibre_link-13){.calibre6}. Note that the text *This text is
being added to the second paragraph.* was added to the
[Paragraph]{.literal} object in [paraObj1]{.literal}, which was the
second paragraph added to [doc]{.literal}. The
[add_paragraph()]{.literal} and [add_run()]{.literal} functions return
paragraph and [Run]{.literal} objects, respectively, to save you the
trouble of extracting them as a separate step.

Keep in mind that as of Python-Docx version 0.8.10, new
[Paragraph]{.literal} objects can be added only to the end of the
document, and new [Run]{.literal} objects can be added only to the end
of a [Paragraph]{.literal} object.

The [save()]{.literal} method can be [called]{.literal} again to save
the additional changes you've made.

::: image1
[]{#calibre_link-13 .calibre6}![image](../images/000103.jpg){.calibre3}
:::

*Figure 15-9: The document with multiple [Paragraph]{.literal1} and
[Run]{.literal1} objects added*

Both [add_paragraph()]{.literal} and [add_run()]{.literal} accept an
optional second argument that is a string of the [Paragraph]{.literal}
or [Run]{.literal} object's style. Here's an example:

\>\>\> [doc.add_paragraph(\'Hello, world!\', \'Title\')]{.codestrong1}

This line adds a paragraph with the text *Hello, world!* in the Title
style.

#### []{#calibre_link-778 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Adding Headings*** {#calibre_link-495 .h2}

Calling [add_heading()]{.literal} adds a paragraph with one of the
heading styles. Enter the following into the interactive shell:

\>\>\> [doc = docx.Document()]{.codestrong1}\
\>\>\> [doc.add_heading(\'Header 0\', 0)]{.codestrong1}\
\<docx.text.Paragraph object at 0x00000000036CB3C8\>\
\>\>\> [doc.add_heading(\'Header 1\', 1)]{.codestrong1}\
\<docx.text.Paragraph object at 0x00000000036CB630\>\
\>\>\> [doc.add_heading(\'Header 2\', 2)]{.codestrong1}\
\<docx.text.Paragraph object at 0x00000000036CB828\>\
\>\>\> [doc.add_heading(\'Header 3\', 3)]{.codestrong1}\
\<docx.text.Paragraph object at 0x00000000036CB2E8\>\
\>\>\> [doc.add_heading(\'Header 4\', 4)]{.codestrong1}\
\<docx.text.Paragraph object at 0x00000000036CB3C8\>\
\>\>\> [doc.save(\'headings.docx\')]{.codestrong1}

The arguments to [add_heading()]{.literal} are a string of the heading
text and an integer from [0]{.literal} to [4]{.literal}. The integer
[0]{.literal} makes the heading the Title style, which is used for the
top of the document. Integers [1]{.literal} to [4]{.literal} are for
various heading levels, with [1]{.literal} being the main heading and
[4]{.literal} the lowest subheading. The [add_heading()]{.literal}
function returns a [Paragraph]{.literal} object to save you the step of
extracting it from the [Document]{.literal} object as a separate step.

The resulting *headings.docx* file will look like [Figure
15-10](#calibre_link-14){.calibre6}.

::: image1
[]{#calibre_link-14 .calibre6}![image](../images/000105.jpg){.calibre3}
:::

*Figure 15-10: The* headings.docx *document with headings 0 to 4*

#### ***Adding Line and Page Breaks*** {#calibre_link-496 .h2}

To add a line break (rather than starting a whole new paragraph), you
can call the [add_break()]{.literal} method on the [Run]{.literal}
object you want to have the break appear after. If you want to add a
page break instead, you need to pass the value
[docx.enum.text.WD_BREAK.PAGE]{.literal} as a lone argument to
[add_break()]{.literal}, as is done in the middle of the following
example:

   \>\>\> [doc = docx.Document()]{.codestrong1}\
   \>\>\> [doc.add_paragraph(\'This is on the first
page!\')]{.codestrong1}\
   \<docx.text.Paragraph object at 0x0000000003785518\>\
[➊]{.ent} \>\>\>
[doc.paragraphs\[0\].runs\[0\].add_break(docx.enum.text.WD_BREAK.PAGE)]{.codestrong1}\
   \>\>\> [doc.add_paragraph(\'This is on the second
page!\')]{.codestrong1}\
   \<docx.text.Paragraph object at 0x00000000037855F8\>\
   \>\>\> [doc.save(\'twoPage.docx\')]{.codestrong1}

[]{#calibre_link-781 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This creates a two-page Word document with *This is
on the first page!* on the first page and *This is on the second page!*
on the second. Even though there was still plenty of space on the first
page after the text *This is on the first page!*, we forced the next
paragraph to begin on a new page by inserting a page break after the
first run of the first paragraph [➊]{.ent}.

#### ***Adding Pictures*** {#calibre_link-497 .h2}

[Document]{.literal} objects have an [add_picture()]{.literal} method
that will let you add an image to the end of the document. Say you have
a file *zophie.png* in the current working directory. You can add
*zophie.png* to the end of your document with a width of 1 inch and
height of 4 centimeters (Word can use both imperial and metric units) by
entering the following:

\>\>\> [doc.add_picture(\'zophie.png\',
width=docx.shared.Inches(1),]{.codestrong1}\
[height=docx.shared.Cm(4))]{.codestrong1}\
\<docx.shape.InlineShape object at 0x00000000036C7D30\>

The first argument is a string of the image's filename. The optional
[width]{.literal} and [height]{.literal} keyword arguments will set the
width and height of the image in the document. If left out, the width
and height will default to the normal size of the image.

You'll probably prefer to specify an image's height and width in
familiar units such as inches and centimeters, so you can use the
[docx.shared.Inches()]{.literal} and [docx.shared.Cm()]{.literal}
functions when you're specifying the [width]{.literal} and
[height]{.literal} keyword arguments.

### **Creating PDFs from Word Documents** {#calibre_link-498 .h1}

The PyPDF2 module doesn't allow you to create PDF documents directly,
but there's a way to generate PDF files with Python if you're on Windows
and have Microsoft Word installed. You'll need to install the Pywin32
package by running [pip install \--user -U pywin32==224]{.literal}. With
this and the [docx]{.literal} module, you can create Word documents and
then convert them to PDFs with the following script.

Open a new file editor tab, enter the following code, and save it as
*convertWordToPDF.py*:

\# This script runs on Windows only, and you must have Word installed.\
import win32com.client \# install with \"pip install pywin32==224\"\
import docx\
wordFilename = \'[your_word_document]{.codeitalic1}.docx\'\
pdfFilename = \'[your_pdf_filename]{.codeitalic1}.pdf\'\
\
doc = docx.Document()\
\# Code to create Word document goes here.\
doc.save(wordFilename)\
\
wdFormatPDF = 17 \# Word\'s numeric code for PDFs.\
wordObj = win32com.client.Dispatch(\'Word.Application\')\
\
[]{#calibre_link-1836 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}docObj = wordObj.Documents.Open(wordFilename)\
docObj.SaveAs(pdfFilename, FileFormat=wdFormatPDF)\
docObj.Close()\
wordObj.Quit()

To write a program that produces PDFs with your own content, you must
use the [docx]{.literal} module to create a Word document, then use the
Pywin32 package's [win32com.client]{.literal} module to convert it to a
PDF. Replace the [\# Code to create Word document goes here.]{.literal}
comment with [docx]{.literal} function calls to create your own content
for the PDF in a Word document.

This may seem like a convoluted way to produce PDFs, but as it turns
out, professional software solutions are often just as complicated.

### **Summary** {#calibre_link-499 .h1}

Text information isn't just for plaintext files; in fact, it's pretty
likely that you deal with PDFs and Word documents much more often. You
can use the [PyPDF2]{.literal} module to read and write PDF documents.
Unfortunately, reading text from PDF documents might not always result
in a perfect translation to a string because of the complicated PDF file
format, and some PDFs might not be readable at all. In these cases,
you're out of luck unless future updates to PyPDF2 support additional
PDF features.

Word documents are more reliable, and you can read them with the
[python-docx]{.literal} package's [docx]{.literal} module. You can
manipulate text in Word documents via [Paragraph]{.literal} and
[Run]{.literal} objects. These objects can also be given styles, though
they must be from the default set of styles or styles already in the
document. You can add new paragraphs, headings, breaks, and pictures to
the document, though only to the end.

Many of the limitations that come with working with PDFs and Word
documents are because these formats are meant to be nicely displayed for
human readers, rather than easy to parse by software. The next chapter
takes a look at two other common formats for storing information: JSON
and CSV files. These formats are designed to be used by computers, and
you'll see that Python can work with these formats much more easily.

### **Practice Questions** {#calibre_link-500 .h1}

[1](#calibre_link-15){#calibre_link-1425 .calibre6}. A string value of
the PDF filename is *not* passed to the
[PyPDF2.PdfFileReader()]{.literal} function. What do you pass to the
function instead?

[2](#calibre_link-16){#calibre_link-1426 .calibre6}. What modes do the
[File]{.literal} objects for [PdfFileReader()]{.literal} and
[PdfFileWriter()]{.literal} need to be opened in?

[3](#calibre_link-17){#calibre_link-1427 .calibre6}. How do you acquire
a [Page]{.literal} object for [page 5](#calibre_link-18){.calibre6} from
a [PdfFileReader]{.literal} object?

[4](#calibre_link-19){#calibre_link-1428 .calibre6}. What
[PdfFileReader]{.literal} variable stores the number of pages in the PDF
document?

[5](#calibre_link-20){#calibre_link-1429 .calibre6}. If a
[PdfFileReader]{.literal} object's PDF is encrypted with the password
[swordfish]{.literal}, what must you do before you can obtain
[Page]{.literal} objects from it?

[6](#calibre_link-21){#calibre_link-1430 .calibre6}.
[]{#calibre_link-1837 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}What methods do you use to rotate a page?

[7](#calibre_link-22){#calibre_link-1431 .calibre6}. What method returns
a [Document]{.literal} object for a file named *demo.docx*?

[8](#calibre_link-23){#calibre_link-1432 .calibre6}. What is the
difference between a [Paragraph]{.literal} object and a [Run]{.literal}
object?

[9](#calibre_link-24){#calibre_link-1433 .calibre6}. How do you obtain a
list of [Paragraph]{.literal} objects for a [Document]{.literal} object
that's stored in a variable named [doc]{.literal}?

[10](#calibre_link-25){#calibre_link-1434 .calibre6}. What type of
object has [bold]{.literal}, [underline]{.literal}, [italic]{.literal},
[strike]{.literal}, and [outline]{.literal} variables?

[11](#calibre_link-26){#calibre_link-1435 .calibre6}. What is the
difference between setting the [bold]{.literal} variable to
[True]{.literal}, [False]{.literal}, or [None]{.literal}?

[12](#calibre_link-27){#calibre_link-1436 .calibre6}. How do you create
a [Document]{.literal} object for a new Word document?

[13](#calibre_link-28){#calibre_link-1437 .calibre6}. How do you add a
paragraph with the text [\'Hello, there!\']{.literal} to a
[Document]{.literal} object stored in a variable named [doc]{.literal}?

[14](#calibre_link-29){#calibre_link-1438 .calibre6}. What integers
represent the levels of headings available in Word documents?

### **Practice Projects** {#calibre_link-501 .h1}

For practice, write programs that do the following.

#### ***PDF Paranoia*** {#calibre_link-502 .h2}

Using the [os.walk()]{.literal} function from [Chapter
10](#calibre_link-30){.calibre6}, write a script that will go through
every PDF in a folder (and its subfolders) and encrypt the PDFs using a
password provided on the command line. Save each encrypted PDF with an
*\_encrypted.pdf* suffix added to the original filename. Before deleting
the original file, have the program attempt to read and decrypt the file
to ensure that it was encrypted correctly.

Then, write a program that finds all encrypted PDFs in a folder (and its
subfolders) and creates a decrypted copy of the PDF using a provided
password. If the password is incorrect, the program should print a
message to the user and continue to the next PDF.

#### ***Custom Invitations as Word Documents*** {#calibre_link-503 .h2}

Say you have a text file of guest names. This *guests.txt* file has one
name per line, as follows:

Prof. Plum\
Miss Scarlet\
Col. Mustard\
Al Sweigart\
RoboCop

Write a program that would generate a Word document with custom
invitations that look like [Figure 15-11](#calibre_link-31){.calibre6}.

Since Python-Docx can use only those styles that already exist in the
Word document, you will have to first add these styles to a blank Word
file and then open that file with Python-Docx. There should be one
invitation []{#calibre_link-1838 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}per page in the resulting Word document, so call
[add_break()]{.literal} to add a page break after the last paragraph of
each invitation. This way, you will need to open only one Word document
to print all of the invitations at once.

::: image1
[]{#calibre_link-31 .calibre6}![image](../images/000069.jpg){.calibre3}
:::

*Figure 15-11: The Word document generated by your custom invite script*

You can download a sample *guests.txt* file from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

#### ***Brute-Force PDF Password Breaker*** {#calibre_link-504 .h2}

Say you have an encrypted PDF that you have forgotten the password to,
but you remember it was a single English word. Trying to guess your
forgotten password is quite a boring task. Instead you can write a
program that will decrypt the PDF by trying every possible English word
until it finds one that works. This is called a *brute-force password
attack.* Download the text file *dictionary.txt* from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.
This *dictionary file* contains over 44,000 English words with one word
per line.

Using the file-reading skills you learned in [Chapter
9](#calibre_link-32){.calibre6}, create a list of word strings by
reading this file. Then loop over each word in this list, passing it to
the [decrypt()]{.literal} method. If this method returns the integer
[0]{.literal}, the password was wrong and your program should continue
to the next password. If [decrypt()]{.literal} returns [1]{.literal},
then your program should break out of the loop and print the hacked
password. You should try both the uppercase and lowercase form of each
word. (On my laptop, going through all 88,000 uppercase and lowercase
words from the dictionary file takes a couple of minutes. This is why
you shouldn't use a simple English word for your passwords.)
:::

<div>

[![](/images/prev.png)](../chapter14)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter16)

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
