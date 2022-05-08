
## **WEB SCRAPING**


![Image](../images/000070.jpg){.calibre3}


In those rare, terrifying moments when I'm without Wi-Fi, I realize just
how much of what I do on the computer is really what I do on the
internet. Out of sheer habit I'll find myself trying to check email,
read friends' Twitter feeds, or answer the question, "Did Kurtwood Smith
have any major roles before he was in the original 1987
*RoboCop*?"^[1](#calibre_link-34){#calibre_link-1170 .calibre6}^

Since so much work on a computer involves going on the internet, it'd be
great if your programs could get online. *Web scraping* is the term for
using a program to download and process content from the web. For
example, Google runs many web scraping programs to index web pages for
[]{#calibre_link-824 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}its search engine. In this chapter, you will learn
about several modules that make it easy to scrape web pages in Python.

[webbrowser]{.codestrong} Comes with Python and opens a browser to a
specific page.

[requests]{.codestrong} Downloads files and web pages from the internet.

[bs4]{.codestrong} Parses HTML, the format that web pages are written
in.

[selenium]{.codestrong} Launches and controls a web browser. The
[selenium]{.literal} module is able to fill in forms and simulate mouse
clicks in this browser.

### **Project: mapIt.py with the webbrowser Module** {#calibre_link-373 .h1}

The [webbrowser]{.literal} module's [open()]{.literal} function can
launch a new browser to a specified URL. Enter the following into the
interactive shell:

\>\>\> [import webbrowser]{.codestrong1}\
\>\>\>
[webbrowser.open(\'https://inventwithpython.com/\')]{.codestrong1}

A web browser tab will open to the URL
*[https://inventwithpython.com/](https://inventwithpython.com/){.calibre6}*.
This is about the only thing the [webbrowser]{.literal} module can do.
Even so, the [open()]{.literal} function does make some interesting
things possible. For example, it's tedious to copy a street address to
the clipboard and bring up a map of it on Google Maps. You could take a
few steps out of this task by writing a simple script to automatically
launch the map in your browser using the contents of your clipboard.
This way, you only have to copy the address to a clipboard and run the
script, and the map will be loaded for you.

This is what your program does:

1.  Gets a street address from the command line arguments or clipboard
2.  Opens the web browser to the Google Maps page for the address

This means your code will need to do the following:

1.  Read the command line arguments from [sys.argv]{.literal}.
2.  Read the clipboard contents.
3.  Call the [webbrowser.open()]{.literal} function to open the web
    browser.

Open a new file editor tab and save it as *mapIt.py*.

#### ***Step 1: Figure Out the URL*** {#calibre_link-374 .h2}

Based on the instructions in [Appendix B](#calibre_link-35){.calibre6},
set up *mapIt.py* so that when you run it from the command line, like so
. . .

C:\\\> mapit 870 Valencia St, San Francisco, CA 94110

. . . the script will use the command line arguments instead of the
clipboard. If there are no command line arguments, then the program will
know to use the contents of the clipboard.

[]{#calibre_link-854 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}First you need to figure out what URL to use for a
given street address. When you load
*[https://maps.google.com/](https://maps.google.com/){.calibre6}* in the
browser and search for an address, the URL in the address bar looks
something like this:
*[https://www.google.com/maps/place/870+Valencia+St/@37.7590311,-122.4215096,17z/data=!3m1!4b1!4m2!3m1!1s0x808f7e3dadc07a37:0xc86b0b2bb93b73d8](https://www.google.com/maps/place/870+Valencia+St/@37.7590311,-122.4215096,17z/data=!3m1!4b1!4m2!3m1!1s0x808f7e3dadc07a37:0xc86b0b2bb93b73d8){.calibre6}*.

The address is in the URL, but there's a lot of additional text there as
well. Websites often add extra data to URLs to help track visitors or
customize sites. But if you try just going to
*[https://www.google.com/maps/place/870+Valencia+St+San+Francisco+CA/](https://www.google.com/maps/place/870+Valencia+St+San+Francisco+CA/){.calibre6}*,
you'll find that it still brings up the correct page. So your program
can be set to open a web browser to
[\'https://www.google.com/maps/place/]{.literal}[your_address_string]{.codeitalic}[\']{.literal}
(where [your_address_string]{.codeitalic} is the address you want to
map).

#### ***Step 2: Handle the Command Line Arguments*** {#calibre_link-375 .h2}

Make your code look like this:

#! python3\
\# mapIt.py - Launches a map in the browser using an address from the\
\# command line or clipboard.\
\
import webbrowser, sys\
if len(sys.argv) \> 1:\
    # Get address from command line.\
    address = \' \'.join(sys.argv\[1:\])\
\
\# TODO: Get address from clipboard.

After the program's [#!]{.literal} shebang line, you need to import the
[webbrowser]{.literal} module for launching the browser and import the
[sys]{.literal} module for reading the potential command line arguments.
The [sys.argv]{.literal} variable stores a list of the program's
filename and command line arguments. If this list has more than just the
filename in it, then [len(sys.argv)]{.literal} evaluates to an integer
greater than [1]{.literal}, meaning that command line arguments have
indeed been provided.

Command line arguments are usually separated by spaces, but in this
case, you want to interpret all of the arguments as a single string.
Since [sys.argv]{.literal} is a list of strings, you can pass it to the
[join()]{.literal} method, which returns a single string value. You
don't want the program name in this string, so instead of
[sys.argv]{.literal}, you should pass [sys.argv\[1:\]]{.literal} to chop
off the first element of the array. The final string that this
expression evaluates to is stored in the [address]{.literal} variable.

If you run the program by entering this into the command line . . .

mapit 870 Valencia St, San Francisco, CA 94110

. . . the [sys.argv]{.literal} variable will contain this list value:

\[\'mapIt.py\', \'870\', \'Valencia\', \'St, \', \'San\', \'Francisco,
\', \'CA\', \'94110\'\]

The [address]{.literal} variable will contain the string [\'870 Valencia
St, San Francisco, CA 94110\']{.literal}.

#### []{#calibre_link-1040 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 3: Handle the Clipboard Content and Launch the Browser*** {#calibre_link-376 .h2}

Make your code look like the following:

#! python3\
\# mapIt.py - Launches a map in the browser using an address from the\
\# command line or clipboard.\
import webbrowser, sys[,]{.codestrong1} [pyperclip]{.codestrong1}\
if len(sys.argv) \> 1:\
    # Get address from command line.\
    address = \' \'.join(sys.argv\[1:\])\
[else:]{.codestrong1}\
[    # Get address from clipboard.]{.codestrong1}\
[    address = pyperclip.paste()]{.codestrong1}\
\
[webbrowser.open(\'https://www.google.com/maps/place/\' +
address)]{.codestrong1}

If there are no command line arguments, the program will assume the
address is stored on the clipboard. You can get the clipboard content
with [pyperclip.paste()]{.literal} and store it in a variable named
[address]{.literal}. Finally, to launch a web browser with the Google
Maps URL, call [webbrowser.open()]{.literal}.

While some of the programs you write will perform huge tasks that save
you hours, it can be just as satisfying to use a program that
conveniently saves you a few seconds each time you perform a common
task, such as getting a map of an address. [Table
12-1](#calibre_link-36){.calibre6} compares the steps needed to display
a map with and without *mapIt.py*.

**Table 12-1:** Getting a Map with and Without *mapIt.py*

+----------------------------------------+----------------------------+
| **Manually getting a map**             | **Using mapIt.py**         |
+========================================+============================+
| 1.  Highlight the address.             | 1.  Highlight the address. |
| 2.  Copy the address.                  | 2.  Copy the address.      |
| 3.  Open the web browser.              | 3.  Run *mapIt.py*.        |
| 4.  Go to                              |                            |
|     *[https://maps.google.com/](       |                            |
| https://maps.google.com/){.calibre6}*. |                            |
| 5.  Click the address text field.      |                            |
| 6.  Paste the address.                 |                            |
| 7.  Press enter.                       |                            |
+----------------------------------------+----------------------------+

See how *mapIt.py* makes this task less tedious?

#### ***Ideas for Similar Programs*** {#calibre_link-377 .h2}

As long as you have a URL, the [webbrowser]{.literal} module lets users
cut out the step of opening the browser and directing themselves to a
website. Other programs could use this functionality to do the
following:

-   Open all links on a page in separate browser tabs.
-   Open the browser to the URL for your local weather.
-   Open several social network sites that you regularly check.

### []{#calibre_link-917 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Downloading Files from the Web with the requests Module** {#calibre_link-378 .h1}

The [requests]{.literal} module lets you easily download files from the
web without having to worry about complicated issues such as network
errors, connection problems, and data compression. The
[requests]{.literal} module doesn't come with Python, so you'll have to
install it first. From the command line, run [pip install \--user
requests]{.codestrong}. ([Appendix A](#calibre_link-2){.calibre6} has
additional details on how to install third-party modules.)

The [requests]{.literal} module was written because Python's
[urllib2]{.literal} module is too complicated to use. In fact, take a
permanent marker and black out this entire paragraph. Forget I ever
mentioned [urllib2]{.literal}. If you need to download things from the
web, just use the [requests]{.literal} module.

Next, do a simple test to make sure the [requests]{.literal} module
installed itself correctly. Enter the following into the interactive
shell:

\>\>\> [import requests]{.codestrong1}

If no error messages show up, then the [requests]{.literal} module has
been successfully installed.

#### ***Downloading a Web Page with the requests.get() Function*** {#calibre_link-379 .h2}

The [requests.get()]{.literal} function takes a string of a URL to
download. By calling [type()]{.literal} on [requests.get()]{.literal}'s
return value, you can see that it returns a [Response]{.literal} object,
which contains the response that the web server gave for your request.
I'll explain the [Response]{.literal} object in more detail later, but
for now, enter the following into the interactive shell while your
computer is connected to the internet:

   \>\>\> [import requests]{.codestrong1}\
[➊]{.ent} \>\>\> [res =
requests.get(\'https://automatetheboringstuff.com/files/rj.txt\')]{.codestrong1}\
   \>\>\> [type(res)]{.codestrong1}\
   \<class \'requests.models.Response\'\>\
[➋]{.ent} \>\>\> [res.status_code == requests.codes.ok]{.codestrong1}\
   True\
   \>\>\> [len(res.text)]{.codestrong1}\
   178981\
   \>\>\> [print(res.text\[:250\])]{.codestrong1}\
   The Project Gutenberg EBook of Romeo and Juliet, by William
Shakespeare\
\
   This eBook is for the use of anyone anywhere at no cost and with\
   almost no restrictions whatsoever.  You may copy it, give it away or\
   re-use it under the terms of the Proje

The URL goes to a text web page for the entire play of *Romeo and
Juliet*, provided on this book's site [➊]{.ent}. You can tell that the
request for this web page succeeded by checking the
[status_code]{.literal} attribute of the [Response]{.literal} object. If
it is equal to the value of [requests.codes.ok]{.literal}, then
everything went fine [➋]{.ent}. (Incidentally, the status code for "OK"
in the HTTP protocol is 200. You may already be familiar with the 404
status code for "Not Found.") []{#calibre_link-1073 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}You can find a complete
list of HTTP status codes and their meanings at
*[https://en.wikipedia.org/wiki/List_of_HTTP_status_codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes){.calibre6}*.

If the request succeeded, the downloaded web page is stored as a string
in the [Response]{.literal} object's [text]{.literal} variable. This
variable holds a large string of the entire play; the call to
[len(res.text)]{.literal} shows you that it is more than 178,000
characters long. Finally, calling [print(res.text\[:250\])]{.literal}
displays only the first 250 characters.

If the request failed and displayed an error message, like "Failed to
establish a new connection" or "Max retries exceeded," then check your
internet connection. Connecting to servers can be quite complicated, and
I can't give a full list of possible problems here. You can find common
causes of your error by doing a web search of the error message in
quotes.

#### ***Checking for Errors*** {#calibre_link-380 .h2}

As you've seen, the [Response]{.literal} object has a
[status_code]{.literal} attribute that can be checked against
[requests.codes.ok]{.literal} (a variable that has the integer value
[200]{.literal}) to see whether the download succeeded. A simpler way to
check for success is to call the [raise_for_status()]{.literal} method
on the [Response]{.literal} object. This will raise an exception if
there was an error downloading the file and will do nothing if the
download succeeded. Enter the following into the interactive shell:

\>\>\> [res =
requests.get(\'https://inventwithpython.com/page_that_does_not_exist\')]{.codestrong1}\
\>\>\> [res.raise_for_status()]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<stdin\>\", line 1, in \<module\>\
\
  File
\"C:\\Users\\Al\\AppData\\Local\\Programs\\Python\\Python37\\lib\\site-packages\\requests\\models\
.py\", line 940, in raise_for_status\
    raise HTTPError(http_error_msg, response=self)\
requests.exceptions.HTTPError: 404 Client Error: Not Found for url:
https://inventwithpython\
.com/page_that_does_not_exist.html

The [raise_for_status()]{.literal} method is a good way to ensure that a
program halts if a bad download occurs. This is a good thing: You want
your program to stop as soon as some unexpected error happens. If a
failed download *isn't* a deal breaker for your program, you can wrap
the [raise_for_status()]{.literal} line with [try]{.literal} and
[except]{.literal} statements to handle this error case without
crashing.

import requests\
res =
requests.get(\'https://inventwithpython.com/page_that_does_not_exist\')\
try:\
    res.raise_for_status()\
except Exception as exc:\
    print(\'There was a problem: %s\' % (exc))

This [raise_for_status()]{.literal} method call causes the program to
output the following:

There was a problem: 404 Client Error: Not Found for url: https://\
inventwithpython.com/page_that_does_not_exist.html

[]{#calibre_link-916 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Always call [raise_for_status()]{.literal} after
calling [requests.get()]{.literal}. You want to be sure that the
download has actually worked before your program continues.

### **Saving Downloaded Files to the Hard Drive** {#calibre_link-381 .h1}

From here, you can save the web page to a file on your hard drive with
the standard [open()]{.literal} function and [write()]{.literal} method.
There are some slight differences, though. First, you must open the file
in *write binary* mode by passing the string [\'wb\']{.literal} as the
second argument to [open()]{.literal}. Even if the page is in plaintext
(such as the *Romeo and Juliet* text you downloaded earlier), you need
to write binary data instead of text data in order to maintain the
*Unicode encoding* of the text.

To write the web page to a file, you can use a [for]{.literal} loop with
the [Response]{.literal} object's [iter_content()]{.literal} method.

\>\>\> [import requests]{.codestrong1}\
\>\>\> [res =
requests.get(\'https://automatetheboringstuff.com/files/rj.txt\')]{.codestrong1}\
\>\>\> [res.raise_for_status()]{.codestrong1}\
\>\>\> [playFile = open(\'RomeoAndJuliet.txt\', \'wb\')]{.codestrong1}\
\>\>\> [for chunk in res.iter_content(100000):]{.codestrong1}\
[        playFile.write(chunk)]{.codestrong1}\
\
100000\
78981\
\>\>\> [playFile.close()]{.codestrong1}

The [iter_content()]{.literal} method returns "chunks" of the content on
each iteration through the loop. Each chunk is of the *bytes* data type,
and you get to specify how many bytes each chunk will contain. One
hundred thousand bytes is generally a good size, so pass
[100000]{.literal} as the argument to [iter_content()]{.literal}.

The file *RomeoAndJuliet.txt* will now exist in the current working
directory. Note that while the filename on the website was *rj.txt*, the
file on your hard drive has a different filename. The
[requests]{.literal} module simply handles downloading the contents of
web pages. Once the page is downloaded, it is simply data in your
program. Even if you were to lose your internet []{#calibre_link-1003
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}connection after
downloading the web page, all the page data would still be on your
computer.


**UNICODE ENCODINGS**

Unicode encodings are beyond the scope of this book, but you can learn
more about them from these web pages:

-   Joel on Software: The Absolute Minimum Every Software Developer
    Absolutely, Positively Must Know About Unicode and Character Sets
    (No Excuses!):
    *[https://www.joelonsoftware.com/articles/Unicode.html](https://www.joelonsoftware.com/articles/Unicode.html){.calibre6}*
-   Pragmatic Unicode:
    *[https://nedbatchelder.com/text/unipain.html](https://nedbatchelder.com/text/unipain.html){.calibre6}*


The [write()]{.literal} method returns the number of bytes written to
the file. In the previous example, there were 100,000 bytes in the first
chunk, and the remaining part of the file needed only 78,981 bytes.

To review, here's the complete process for downloading and saving a
file:

1.  Call [requests.get()]{.literal} to download the file.
2.  Call [open()]{.literal} with [\'wb\']{.literal} to create a new file
    in write binary mode.
3.  Loop over the [Response]{.literal} object's
    [iter_content()]{.literal} method.
4.  Call [write()]{.literal} on each iteration to write the content to
    the file.
5.  Call [close()]{.literal} to close the file.

That's all there is to the [requests]{.literal} module! The
[for]{.literal} loop and [iter_content()]{.literal} stuff may seem
complicated compared to the
[open()]{.literal}/[write()]{.literal}/[close()]{.literal} workflow
you've been using to write text files, but it's to ensure that the
[requests]{.literal} module doesn't eat up too much memory even if you
download massive files. You can learn about the [requests]{.literal}
module's other features from
*[https://requests.readthedocs.org/](https://requests.readthedocs.org/){.calibre6}*.

### **HTML** {#calibre_link-382 .h1}

Before you pick apart web pages, you'll learn some HTML basics. You'll
also see how to access your web browser's powerful developer tools,
which will make scraping information from the web much easier.

#### ***Resources for Learning HTML*** {#calibre_link-383 .h2}

*Hypertext Markup Language (HTML)* is the format that web pages are
written in. This chapter assumes you have some basic experience with
HTML, but if you need a beginner tutorial, I suggest one of the
following sites:

-   *[https://developer.mozilla.org/en-US/learn/html/](https://developer.mozilla.org/en-US/learn/html/){.calibre6}*
-   *[https://htmldog.com/guides/html/beginner/](https://htmldog.com/guides/html/beginner/){.calibre6}*
-   *[https://www.codecademy.com/learn/learn-html](https://www.codecademy.com/learn/learn-html){.calibre6}*

#### ***A Quick Refresher*** {#calibre_link-384 .h2}

In case it's been a while since you've looked at any HTML, here's a
quick overview of the basics. An HTML file is a plaintext file with the
*.html* file extension. The text in these files is surrounded by *tags*,
which are words enclosed in angle brackets. The tags tell the browser
how to format the web page. A starting tag and closing tag can enclose
some text to form an *element*. The *text* (or *inner HTML*) is the
content between the starting and closing tags. For example, the
following HTML will display *Hello, world!* in the browser, with *Hello*
in bold:

\<strong\>Hello\</strong\>, world!

[]{#calibre_link-803 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This HTML will look like [Figure
12-1](#calibre_link-37){.calibre6} in a browser.


[]{#calibre_link-37 .calibre6}![image](../images/000018.jpg){.calibre3}


*Figure 12-1:* Hello, world! *rendered in the browser*

The opening [\<strong\>]{.literal} tag says that the enclosed text will
appear in bold. The closing [\</strong\>]{.literal} tags tells the
browser where the end of the bold text is.

There are many different tags in HTML. Some of these tags have extra
properties in the form of *attributes* within the angle brackets. For
example, the [\<a\>]{.literal} tag encloses text that should be a link.
The URL that the text links to is determined by the [href]{.literal}
attribute. Here's an example:

Al\'s free \<a href=\"https://inventwithpython.com\"\>Python
books\</a\>.

This HTML will look like [Figure 12-2](#calibre_link-38){.calibre6} in a
browser.


[]{#calibre_link-38 .calibre6}![image](../images/000111.jpg){.calibre3}


*Figure 12-2: The link rendered in the browser*

Some elements have an [id]{.literal} attribute that is used to uniquely
identify the element in the page. You will often instruct your programs
to seek out an element by its [id]{.literal} attribute, so figuring out
an element's [id]{.literal} attribute using the browser's developer
tools is a common task in writing web scraping programs.

#### ***Viewing the Source HTML of a Web Page*** {#calibre_link-385 .h2}

You'll need to look at the HTML source of the web pages that your
programs will work with. To do this, right-click (or
[CTRL]{.small}-click on macOS) any web page in your web browser, and
select **View Source** or **View page source** to see the HTML text of
the page (see [Figure 12-3](#calibre_link-39){.calibre6}). This is the
text your browser actually receives. The browser knows how to display,
or *render*, the web page from this HTML.


[]{#calibre_link-841 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-39
.calibre6}![image](../images/000056.jpg){.calibre3}


*Figure 12-3: Viewing the source of a web page*

I highly recommend viewing the source HTML of some of your favorite
sites. It's fine if you don't fully understand what you are seeing when
you look at the source. You won't need HTML mastery to write simple web
scraping programs---after all, you won't be writing your own websites.
You just need enough knowledge to pick out data from an existing site.

#### ***Opening Your Browser's Developer Tools*** {#calibre_link-386 .h2}

In addition to viewing a web page's source, you can look through a
page's HTML using your browser's developer tools. In Chrome and Internet
Explorer for Windows, the developer tools are already installed, and you
can press F12 to make them appear (see [Figure
12-4](#calibre_link-40){.calibre6}). Pressing F12 again will make the
developer tools disappear. In Chrome, you can also bring up the
developer tools by selecting **View** ▸ **Developer** ▸ **Developer
Tools**. In macOS, pressing
![image](../images/000064.jpg){.calibre3}-[OPTION]{.small}-I will open
Chrome's Developer Tools.


[]{#calibre_link-971 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-40
.calibre6}![image](../images/000152.jpg){.calibre3}


*Figure 12-4: The Developer Tools window in the Chrome browser*

In Firefox, you can bring up the Web Developer Tools Inspector by
pressing [CTRL-SHIFT-C]{.small} on Windows and Linux or by pressing
![image](../images/000064.jpg){.calibre3}[-OPTION-C]{.small} on macOS.
The layout is almost identical to Chrome's developer tools.

In Safari, open the Preferences window, and on the Advanced pane check
the **Show Develop menu in the menu bar** option. After it has been
enabled, you can bring up the developer tools by pressing
![image](../images/000064.jpg){.calibre3}[-OPTION-I]{.small}.

After enabling or installing the developer tools in your browser, you
can right-click any part of the web page and select **Inspect Element**
from the context menu to bring up the HTML responsible for that part of
the page. This will be helpful when you begin to parse HTML for your web
scraping programs.


**DON'T USE REGULAR EXPRESSIONS TO PARSE HTML**

Locating a specific piece of HTML in a string seems like a perfect case
for regular expressions. However, I advise you against it. There are
many different ways that HTML can be formatted and still be considered
valid HTML, but trying to capture all these possible variations in a
regular expression can be tedious and error prone. A module developed
specifically for parsing HTML, such as [bs4]{.literal1}, will be less
likely to result in bugs.

You can find an extended argument for why you shouldn't parse HTML with
regular expressions at
*[https://stackoverflow.com/a/1732454/1893164/](https://stackoverflow.com/a/1732454/1893164/){.calibre6}*.


#### []{#calibre_link-927 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Using the Developer Tools to Find HTML Elements*** {#calibre_link-387 .h2}

Once your program has downloaded a web page using the
[requests]{.literal} module, you will have the page's HTML content as a
single string value. Now you need to figure out which part of the HTML
corresponds to the information on the web page you're interested in.

This is where the browser's developer tools can help. Say you want to
write a program to pull weather forecast data from
*[https://weather.gov/](https://weather.gov/){.calibre6}*. Before
writing any code, do a little research. If you visit the site and search
for the 94105 ZIP code, the site will take you to a page showing the
forecast for that area.

What if you're interested in scraping the weather information for that
ZIP code? Right-click where it is on the page (or
[CONTROL]{.small}-click on macOS) and select **Inspect Element** from
the context menu that appears. This will bring up the Developer Tools
window, which shows you the HTML that produces this particular part of
the web page. [Figure 12-5](#calibre_link-41){.calibre6} shows the
developer tools open to the HTML of the nearest forecast. Note that if
the *[https://weather.gov/](https://weather.gov/){.calibre6}* site
changes the design of its web pages, you'll need to repeat this process
to inspect the new elements.


[]{#calibre_link-41 .calibre6}![image](../images/000094.jpg){.calibre3}


*Figure 12-5: Inspecting the element that holds forecast text with the
developer tools*

[]{#calibre_link-812 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}From the developer tools, you can see that the HTML
responsible for the forecast part of the web page is [\<div
class=\"col-sm-10 forecast-text\"\>Sunny, with a high near 64. West wind
11 to 16 mph, with gusts as high as 21 mph.\</div\>]{.literal}. This is
exactly what you were looking for! It seems that the forecast
information is contained inside a [\<div\>]{.literal} element with the
[forecast-text]{.literal} CSS class. Right-click on this element in the
browser's developer console, and from the context menu that appears,
select **Copy** ▸ **CSS Selector**. This will copy a string such as
[\'div.row-odd:nth-child(1) \> div:nth-child(2)\']{.literal} to the
clipboard. You can use this string for Beautiful Soup's
[select()]{.literal} or Selenium's
[find_element_by_css_selector()]{.literal} methods, as explained later
in this chapter. Now that you know what you're looking for, the
Beautiful Soup module will help you find it in the string.

### **Parsing HTML with the bs4 Module** {#calibre_link-388 .h1}

Beautiful Soup is a module for extracting information from an HTML page
(and is much better for this purpose than regular expressions). The
Beautiful Soup module's name is [bs4]{.literal} (for Beautiful Soup,
version 4). To install it, you will need to run [pip install \--user
beautifulsoup4]{.literal} from the command line. (Check out [Appendix
A](#calibre_link-2){.calibre6} for instructions on installing
third-party modules.) While [beautifulsoup4]{.literal} is the name used
for installation, to import Beautiful Soup you run [import
bs4]{.literal}.

For this chapter, the Beautiful Soup examples will *parse* (that is,
analyze and identify the parts of) an HTML file on the hard drive. Open
a new file editor tab in Mu, enter the following, and save it as
*example.html*. Alternatively, download it from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

\<!\-- This is the example.html example file. \--\>\
\
\<html\>\<head\>\<title\>The Website Title\</title\>\</head\>\
\<body\>\
\<p\>Download my \<strong\>Python\</strong\> book from \<a
href=\"https://\
inventwithpython.com\"\>my website\</a\>.\</p\>\
\<p class=\"slogan\"\>Learn Python the easy way!\</p\>\
\<p\>By \<span id=\"author\"\>Al Sweigart\</span\>\</p\>\
\</body\>\</html\>

As you can see, even a simple HTML file involves many different tags and
attributes, and matters quickly get confusing with complex websites.
Thankfully, Beautiful Soup makes working with HTML much easier.

#### []{#calibre_link-825 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Creating a BeautifulSoup Object from HTML*** {#calibre_link-389 .h2}

The [bs4.BeautifulSoup()]{.literal} function needs to be called with a
string containing the HTML it will parse. The
[bs4.BeautifulSoup()]{.literal} function returns a
[BeautifulSoup]{.literal} object. Enter the following into the
interactive shell while your computer is connected to the internet:

\>\>\> [import requests, bs4]{.codestrong1}\
\>\>\> [res = requests.get(\'https://nostarch.com\')]{.codestrong1}\
\>\>\> [res.raise_for_status()]{.codestrong1}\
\>\>\> [noStarchSoup = bs4.BeautifulSoup(res.text,
\'html.parser\')]{.codestrong1}\
\>\>\> [type(noStarchSoup)]{.codestrong1}\
\<class \'bs4.BeautifulSoup\'\>

This code uses [requests.get()]{.literal} to download the main page from
the No Starch Press website and then passes the [text]{.literal}
attribute of the response to [bs4.BeautifulSoup()]{.literal}. The
[BeautifulSoup]{.literal} object that it returns is stored in a variable
named [noStarchSoup]{.literal}.

You can also load an HTML file from your hard drive by passing a
[File]{.literal} object to [bs4.BeautifulSoup()]{.literal} along with a
second argument that tells Beautiful Soup which parser to use to analyze
the HTML.

Enter the following into the interactive shell (after making sure the
*example.html* file is in the working directory):

\>\>\> [exampleFile = open(\'example.html\')]{.codestrong1}\
\>\>\> [exampleSoup = bs4.BeautifulSoup(exampleFile,
\'html.parser\')]{.codestrong1}\
\>\>\> [type(exampleSoup)]{.codestrong1}\
\<class \'bs4.BeautifulSoup\'\>

The [\'html.parser\']{.literal} parser used here comes with Python.
However, you can use the faster [\'lxml\']{.literal} parser if you
install the third-party [lxml]{.literal} module. Follow the instructions
in [Appendix A](#calibre_link-2){.calibre6} to install this module by
running [pip install \--user lxml]{.literal}. Forgetting to include this
second argument will result in a [UserWarning: No parser was explicitly
specified]{.literal} warning.

Once you have a [BeautifulSoup]{.literal} object, you can use its
methods to locate specific parts of an HTML document.

#### ***Finding an Element with the select() Method*** {#calibre_link-390 .h2}

You can retrieve a web page element from a [BeautifulSoup]{.literal}
object by calling the [select()]{.literal}method and passing a string of
a CSS *selector* for the element you are looking for. Selectors are like
regular expressions: they specify a pattern to look for---in this case,
in HTML pages instead of general text strings.

A full discussion of CSS selector syntax is beyond the scope of this
book (there's a good selector tutorial in the resources at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*),
but here's a short introduction to selectors. [Table
12-2](#calibre_link-42){.calibre6} shows examples of the most common CSS
selector patterns.

[]{#calibre_link-1084 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}**Table 12-2:** Examples of CSS Selectors

  **Selector passed to the [select()]{.codestrong} method**   **Will match . . .**
  ----------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------
  [soup.select(\'div\')]{.literal}                            All elements named [\<div\>]{.literal}
  [soup.select(\'#author\')]{.literal}                        The element with an [id]{.literal} attribute of [author]{.literal}
  [soup.select(\'.notice\')]{.literal}                        All elements that use a CSS [class]{.literal} attribute named [notice]{.literal}
  [soup.select(\'div span\')]{.literal}                       All elements named [\<span\>]{.literal} that are within an element named [\<div\>]{.literal}
  [soup.select(\'div \> span\')]{.literal}                    All elements named [\<span\>]{.literal} that are *directly* within an element named [\<div\>]{.literal}, with no other element in between
  [soup.select(\'input\[name\]\')]{.literal}                  All elements named [\<input\>]{.literal} that have a [name]{.literal} attribute with any value
  [soup.select(\'input\[type=\"button\"\]\')]{.literal}       All elements named [\<input\>]{.literal} that have an attribute named [type]{.literal} with value [button]{.literal}

The various selector patterns can be combined to make sophisticated
matches. For example, [soup.select(\'p #author\')]{.literal} will match
any element that has an [id]{.literal} attribute of [author]{.literal},
as long as it is also inside a [\<p\>]{.literal} element. Instead of
writing the selector yourself, you can also right-click on the element
in your browser and select **Inspect Element**. When the browser's
developer console opens, right-click on the element's HTML and select
**Copy** ▸ **CSS Selector** to copy the selector string to the clipboard
and paste it into your source code.

The [select()]{.literal} method will return a list of [Tag]{.literal}
objects, which is how Beautiful Soup represents an HTML element. The
list will contain one [Tag]{.literal} object for every match in the
[BeautifulSoup]{.literal} object's HTML. Tag values can be passed to the
[str()]{.literal} function to show the HTML tags they represent. Tag
values also have an [attrs]{.literal} attribute that shows all the HTML
attributes of the tag as a dictionary. Using the *example.html* file
from earlier, enter the following into the interactive shell:

\>\>\> [import bs4]{.codestrong1}\
\>\>\> [exampleFile = open(\'example.html\')]{.codestrong1}\
\>\>\> [exampleSoup = bs4.BeautifulSoup(exampleFile.read(),
\'html.parser\')]{.codestrong1}\
\>\>\> [elems = exampleSoup.select(\'#author\')]{.codestrong1}\
\>\>\> [type(elems)]{.codestrong1} \# elems is a list of Tag objects.\
\<class \'list\'\>\
\>\>\> [len(elems)]{.codestrong1}\
1\
\>\>\> [type(elems\[0\])]{.codestrong1}\
\<class \'bs4.element.Tag\'\>\
\>\>\> [str(elems\[0\])]{.codestrong1} \# The Tag object as a string.\
\'\<span id=\"author\"\>Al Sweigart\</span\>\'\
\>\>\> [elems\[0\].getText()]{.codestrong1}\
\'Al Sweigart\'\
\>\>\> [elems\[0\].attrs]{.codestrong1}\
{\'id\': \'author\'}

[]{#calibre_link-826 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This code will pull the element with
[id=\"author\"]{.literal} out of our example HTML. We use
[select(\'#author\')]{.literal} to return a list of all the elements
with [id=\"author\"]{.literal}. We store this list of [Tag]{.codestrong}
objects in the variable [elems]{.literal}, and [len(elems)]{.literal}
tells us there is one [Tag]{.codestrong} object in the list; there was
one match. Calling [getText()]{.literal} on the element returns the
element's text, or inner HTML. The text of an element is the content
between the opening and closing tags: in this case, [\'Al
Sweigart\']{.literal}.

Passing the element to [str()]{.literal} returns a string with the
starting and closing tags and the element's text. Finally,
[attrs]{.literal} gives us a dictionary with the element's attribute,
[\'id\']{.literal}, and the value of the [id]{.literal} attribute,
[\'author\']{.literal}.

You can also pull all the [\<p\>]{.literal} elements from the
[BeautifulSoup]{.literal} object. Enter this into the interactive shell:

\>\>\> [pElems = exampleSoup.select(\'p\')]{.codestrong1}\
\>\>\> [str(pElems\[0\])]{.codestrong1}\
\'\<p\>Download my \<strong\>Python\</strong\> book from \<a
href=\"https://\
inventwithpython.com\"\>my website\</a\>.\</p\>\'\
\>\>\> [pElems\[0\].getText()]{.codestrong1}\
\'Download my Python book from my website.\'\
\>\>\> [str(pElems\[1\])]{.codestrong1}\
\'\<p class=\"slogan\"\>Learn Python the easy way!\</p\>\'\
\>\>\> [pElems\[1\].getText()]{.codestrong1}\
\'Learn Python the easy way!\'\
\>\>\> [str(pElems\[2\])]{.codestrong1}\
\'\<p\>By \<span id=\"author\"\>Al Sweigart\</span\>\</p\>\'\
\>\>\> [pElems\[2\].getText()]{.codestrong1}\
\'By Al Sweigart\'

This time, [select()]{.literal} gives us a list of three matches, which
we store in [pElems]{.literal}. Using [str()]{.literal} on
[pElems\[0\]]{.literal}, [pElems\[1\]]{.literal}, and
[pElems\[2\]]{.literal} shows you each element as a string, and using
[getText()]{.literal} on each element shows you its text.

#### ***Getting Data from an Element's Attributes*** {#calibre_link-391 .h2}

The [get()]{.literal} method for [Tag]{.literal} objects makes it simple
to access attribute values from an element. The method is passed a
string of an attribute name and returns that attribute's value. Using
*example.html*, enter the following into the interactive shell:

\>\>\> [import bs4]{.codestrong1}\
\>\>\> [soup = bs4.BeautifulSoup(open(\'example.html\'),
\'html.parser\')]{.codestrong1}\
\>\>\> [spanElem = soup.select(\'span\')\[0\]]{.codestrong1}\
\>\>\> [str(spanElem)]{.codestrong1}\
\'\<span id=\"author\"\>Al Sweigart\</span\>\'\
\>\>\> [spanElem.get(\'id\')]{.codestrong1}\
\'author\'\
\>\>\> [spanElem.get(\'some_nonexistent_addr\') == None]{.codestrong1}\
True\
\>\>\> [spanElem.attrs]{.codestrong1}\
{\'id\': \'author\'}

[]{#calibre_link-1091 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here we use [select()]{.literal} to find any
[\<span\>]{.literal} elements and then store the first matched element
in [spanElem]{.literal}. Passing the attribute name [\'id\']{.literal}
to [get()]{.literal} returns the attribute's value,
[\'author\']{.literal}.

### **Project: Opening All Search Results** {#calibre_link-392 .h1}

Whenever I search a topic on Google, I don't look at just one search
result at a time. By middle-clicking a search result link (or clicking
while holding [CTRL]{.small}), I open the first several links in a bunch
of new tabs to read later. I search Google often enough that this
workflow---opening my browser, searching for a topic, and
middle-clicking several links one by one---is tedious. It would be nice
if I could simply type a search term on the command line and have my
computer automatically open a browser with all the top search results in
new tabs. Let's write a script to do this with the search results page
for the Python Package Index at
*[https://pypi.org/](https://pypi.org/){.calibre6}*. A program like this
can be adapted to many other websites, although the Google and
DuckDuckGo often employ measures that make scraping their search results
pages difficult.

This is what your program does:

1.  Gets search keywords from the command line arguments
2.  Retrieves the search results page
3.  Opens a browser tab for each result

This means your code will need to do the following:

1.  Read the command line arguments from [sys.argv]{.literal}.
2.  Fetch the search result page with the [requests]{.literal} module.
3.  Find the links to each search result.
4.  Call the [webbrowser.open()]{.literal} function to open the web
    browser.

Open a new file editor tab and save it as *searchpypi.py*.

#### ***Step 1: Get the Command Line Arguments and Request the Search Page*** {#calibre_link-393 .h2}

Before coding anything, you first need to know the URL of the search
result page. By looking at the browser's address bar after doing a
search, you can see that the result page has a URL like
*https://pypi.org/search/?q=\<SEARCH_TERM_HERE\>*. The
[requests]{.literal} module can download this page and then you can use
Beautiful Soup to find the search result links in the HTML. Finally,
you'll use the [webbrowser]{.literal} module to open those links in
browser tabs.

Make your code look like the following:

#! python3\
\# searchpypi.py  - Opens several search results.\
\
import requests, sys, webbrowser, bs4\
[]{#calibre_link-1803 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}print(\'Searching\...\')    # display text while
downloading the search result page\
res = requests.get(\'https://google.com/search?q=\'
\'https://pypi.org/search/?q=\'\
+ \' \'.join(sys.argv\[1:\]))\
res.raise_for_status()\
\
\# TODO: Retrieve top search result links.\
\
\# TODO: Open a browser tab for each result.

The user will specify the search terms using command line arguments when
they launch the program. These arguments will be stored as strings in a
list in [sys.argv]{.literal}.

#### ***Step 2: Find All the Results*** {#calibre_link-394 .h2}

Now you need to use Beautiful Soup to extract the top search result
links from your downloaded HTML. But how do you figure out the right
selector for the job? For example, you can't just search for all
[\<a\>]{.literal} tags, because there are lots of links you don't care
about in the HTML. Instead, you must inspect the search result page with
the browser's developer tools to try to find a selector that will pick
out only the links you want.

After doing a search for *Beautiful Soup*, you can open the browser's
developer tools and inspect some of the link elements on the page. They
can look complicated, something like pages of this: [\<a
class=\"package-snippet\" href=\"HYPERLINK
\"view-source:https://pypi.org/project/xml-parser/\"/project/xml-parser/\"\>]{.literal}.

It doesn't matter that the element looks incredibly complicated. You
just need to find the pattern that all the search result links have.

Make your code look like the following:

#! python3\
\# searchpypi.py - Opens several google results.\
import requests, sys, webbrowser, bs4\
\--[snip]{.codeitalic1}\--\
[\# Retrieve top search result links.]{.codestrong1}\
[soup = bs4.BeautifulSoup(res.text, \'html.parser\')]{.codestrong1}\
[\# Open a browser tab for each result.]{.codestrong1}\
[linkElems = soup.select(\'.package-snippet\')]{.codestrong1}

If you look at the [\<a\>]{.literal} elements, though, the search result
links all have [class=\"package-snippet\"]{.literal}. Looking through
the rest of the HTML source, it looks like the
[package-snippet]{.literal} class is used only for search result links.
You don't have to know what the CSS class [package-snippet]{.literal} is
or what it does. You're just going to use it as a marker for the
[\<a\>]{.literal} element you are looking for. You can create a
[BeautifulSoup]{.literal} object from the downloaded page's HTML text
and then use the selector [\'.package-snippet\']{.literal} to find all
[\<a\>]{.literal} elements that are within an element that has the
[package-snippet]{.literal} CSS class. Note that if the PyPI website
changes its layout, you may need to update this program with a new CSS
selector string to pass to [soup.select()]{.literal}. The rest of the
program will still be up to date.

#### []{#calibre_link-1804 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 3: Open Web Browsers for Each Result*** {#calibre_link-395 .h2}

Finally, we'll tell the program to open web browser tabs for our
results. Add the following to the end of your program:

#! python3\
\# searchpypi.py - Opens several search results.\
import requests, sys, webbrowser, bs4\
\--[snip]{.codeitalic1}\--\
\# Open a browser tab for each result.\
linkElems = soup.select(\'.package-snippet\')\
[numOpen = min(5, len(linkElems))]{.codestrong1}\
[for i in range(numOpen):]{.codestrong1}\
[    urlToOpen = \'https://pypi.org\' +
linkElems\[i\].get(\'href\')]{.codestrong1}\
[    print(\'Opening\', urlToOpen)]{.codestrong1}\
[    webbrowser.open(urlToOpen)]{.codestrong1}

By default, you open the first five search results in new tabs using the
[webbrowser]{.literal} module. However, the user may have searched for
something that turned up fewer than five results. The
[soup.select()]{.literal} call returns a list of all the elements that
matched your [\'.package-snippet\']{.literal} selector, so the number of
tabs you want to open is either [5]{.literal} or the length of this list
(whichever is smaller).

The built-in Python function [min()]{.literal} returns the smallest of
the integer or float arguments it is passed. (There is also a built-in
[max()]{.literal} function that returns the largest argument it is
passed.) You can use [min()]{.literal} to find out whether there are
fewer than five links in the list and store the number of links to open
in a variable named [numOpen]{.literal}. Then you can run through a
[for]{.literal} loop by calling [range(numOpen)]{.literal}.

On each iteration of the loop, you use [webbrowser.open()]{.literal} to
open a new tab in the web browser. Note that the [href]{.literal}
attribute's value in the returned [\<a\>]{.literal} elements do not have
the initial [https://pypi.org]{.literal} part, so you have to
concatenate that to the [href]{.literal} attribute's string value.

Now you can instantly open the first five PyPI search results for, say,
*boring stuff* by running [searchpypi boring stuff]{.literal} on the
command line! (See [Appendix B](#calibre_link-35){.calibre6} for how to
easily run programs on your operating system.)

#### ***Ideas for Similar Programs*** {#calibre_link-396 .h2}

The benefit of tabbed browsing is that you can easily open links in new
tabs to peruse later. A program that automatically opens several links
at once can be a nice shortcut to do the following:

-   Open all the product pages after searching a shopping site such as
    Amazon.
-   Open all the links to reviews for a single product.
-   Open the result links to photos after performing a search on a photo
    site such as Flickr or Imgur.

### []{#calibre_link-918 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Project: Downloading All XKCD Comics** {#calibre_link-397 .h1}

Blogs and other regularly updating websites usually have a front page
with the most recent post as well as a Previous button on the page that
takes you to the previous post. Then that post will also have a Previous
button, and so on, creating a trail from the most recent page to the
first post on the site. If you wanted a copy of the site's content to
read when you're not online, you could manually navigate over every page
and save each one. But this is pretty boring work, so let's write a
program to do it instead.

XKCD is a popular geek webcomic with a website that fits this structure
(see [Figure 12-6](#calibre_link-43){.calibre6}). The front page at
*[https://xkcd.com/](https://xkcd.com/){.calibre6}* has a Prev button
that guides the user back through prior comics. Downloading each comic
by hand would take forever, but you can write a script to do this in a
couple of minutes.


[]{#calibre_link-43 .calibre6}![image](../images/000040.jpg){.calibre3}


*Figure 12-6: XKCD, "a webcomic of romance, sarcasm, math, and
language"*

Here's what your program does:

1.  Loads the XKCD home page
2.  Saves the comic image on that page
3.  Follows the Previous Comic link
4.  Repeats until it reaches the first comic

This means your code will need to do the following:

1.  Download pages with the [requests]{.literal} module.
2.  Find the URL of the comic image for a page using Beautiful Soup.
3.  []{#calibre_link-1805 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Download and save the comic image to the hard
    drive with [iter_content()]{.literal}.
4.  Find the URL of the Previous Comic link, and repeat.

Open a new file editor tab and save it as *downloadXkcd.py*.

#### ***Step 1: Design the Program*** {#calibre_link-398 .h2}

If you open the browser's developer tools and inspect the elements on
the page, you'll find the following:

-   The URL of the comic's image file is given by the [href]{.literal}
    attribute of an [\<img\>]{.literal} element.
-   The [\<img\>]{.literal} element is inside a [\<div
    id=\"comic\"\>]{.literal} element.
-   The Prev button has a [rel]{.literal} HTML attribute with the value
    [prev]{.literal}.
-   The first comic's Prev button links to the
    *[https://xkcd.com/#](https://xkcd.com/#){.calibre6}* URL,
    indicating that there are no more previous pages.

Make your code look like the following:

#! python3\
\# downloadXkcd.py - Downloads every single XKCD comic.\
\
import requests, os, bs4\
\
url = \'https://xkcd.com\'               # starting url\
os.makedirs(\'xkcd\', exist_ok=True)    # store comics in ./xkcd\
while not url.endswith(\'#\'):\
    # TODO: Download the page.\
\
    # TODO: Find the URL of the comic image.\
\
    # TODO: Download the image.\
\
    # TODO: Save the image to ./xkcd.\
\
    # TODO: Get the Prev button\'s url.\
\
print(\'Done.\')

You'll have a [url]{.literal} variable that starts with the value
[\'https://xkcd.com\']{.literal} and repeatedly update it (in a
[for]{.literal} loop) with the URL of the current page's Prev link. At
every step in the loop, you'll download the comic at [url]{.literal}.
You'll know to end the loop when [url]{.literal} ends with
[\'#\']{.literal}.

You will download the image files to a folder in the current working
directory named *xkcd*. The call [os.makedirs()]{.literal} ensures that
this folder exists, and the [exist_ok=True]{.literal} keyword argument
prevents the function from []{#calibre_link-1092 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}throwing an exception if
this folder already exists. The remaining code is just comments that
outline the rest of your program.

#### ***Step 2: Download the Web Page*** {#calibre_link-399 .h2}

Let's implement the code for downloading the page. Make your code look
like the following:

#! python3\
\# downloadXkcd.py - Downloads every single XKCD comic.\
\
import requests, os, bs4\
\
url = \'https://xkcd.com\'               # starting url\
os.makedirs(\'xkcd\', exist_ok=True)    # store comics in ./xkcd\
while not url.endswith(\'#\'):\
    [\# Download the page.]{.codestrong1}\
[    print(\'Downloading page %s\...\' % url)]{.codestrong1}\
[    res = requests.get(url)]{.codestrong1}\
[    res.raise_for_status()]{.codestrong1}\
\
 [   soup = bs4.BeautifulSoup(res.text, \'html.parser\')]{.codestrong1}\
\
    # TODO: Find the URL of the comic image.\
\
    # TODO: Download the image.\
\
    # TODO: Save the image to ./xkcd.\
\
    # TODO: Get the Prev button\'s url.\
\
print(\'Done.\')

First, print [url]{.literal} so that the user knows which URL the
program is about to download; then use the [requests]{.literal} module's
[request.get()]{.literal} function to download it. As always, you
immediately call the [Response]{.literal} object's
[raise_for_status()]{.literal} method to throw an exception and end the
program if something went wrong with the download. Otherwise, you create
a [BeautifulSoup]{.literal} object from the text of the downloaded page.

#### ***Step 3: Find and Download the Comic Image*** {#calibre_link-400 .h2}

Make your code look like the following:

#! python3\
\# downloadXkcd.py - Downloads every single XKCD comic.\
\
import requests, os, bs4\
\
\--[snip]{.codeitalic1}\--\
\
[    # Find the URL of the comic image.]{.codestrong1}\
[    comicElem = soup.select(\'#comic img\')]{.codestrong1}\
[]{#calibre_link-1093 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[    if comicElem == \[\]:]{.codestrong1}\
[        print(\'Could not find comic image.\')]{.codestrong1}\
[    else:]{.codestrong1}\
[        comicUrl = \'https:\' +
comicElem\[0\].get(\'src\')]{.codestrong1}\
[        # Download the image.]{.codestrong1}\
[        print(\'Downloading image %s\...\' %
(comicUrl))]{.codestrong1}\
[        res = requests.get(comicUrl)]{.codestrong1}\
[        res.raise_for_status()]{.codestrong1}\
\
    # TODO: Save the image to ./xkcd.\
\
    # TODO: Get the Prev button\'s url.\
\
print(\'Done.\')

From inspecting the XKCD home page with your developer tools, you know
that the [\<img\>]{.literal} element for the comic image is inside a
[\<div\>]{.literal} element with the [id]{.literal} attribute set to
[comic]{.literal}, so the selector [\'#comic img\']{.literal} will get
you the correct [\<img\>]{.literal} element from the
[BeautifulSoup]{.literal} object.

A few XKCD pages have special content that isn't a simple image file.
That's fine; you'll just skip those. If your selector doesn't find any
elements, then [soup.select(\'#comic img\')]{.literal} will return a
blank list. When that happens, the program can just print an error
message and move on without downloading the image.

Otherwise, the selector will return a list containing one
[\<img\>]{.literal} element. You can get the [src]{.literal} attribute
from this [\<img\>]{.literal} element and pass it to
[requests.get()]{.literal} to download the comic's image file.

#### ***Step 4: Save the Image and Find the Previous Comic*** {#calibre_link-401 .h2}

Make your code look like the following:

#! python3\
\# downloadXkcd.py - Downloads every single XKCD comic.\
\
import requests, os, bs4\
\
\--[snip]{.codeitalic1}\--\
\
 [       # Save the image to ./xkcd.]{.codestrong1}\
[        imageFile = open(os.path.join(\'xkcd\',
os.path.basename(comicUrl)),]{.codestrong1}\
[\'wb\')]{.codestrong1}\
[        for chunk in res.iter_content(100000):]{.codestrong1}\
[            imageFile.write(chunk)]{.codestrong1}\
[        imageFile.close()]{.codestrong1}\
\
[    # Get the Prev button\'s url.]{.codestrong1}\
[    prevLink = soup.select(\'a\[rel=\"prev\"\]\')\[0\]]{.codestrong1}\
[    url = \'https://xkcd.com\' + prevLink.get(\'href\')]{.codestrong1}\
\
print(\'Done.\')

[]{#calibre_link-1806 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}At this point, the image file of the comic is
stored in the [res]{.literal} variable. You need to write this image
data to a file on the hard drive.

You'll need a filename for the local image file to pass to
[open()]{.literal}. The [comicUrl]{.literal} will have a value like
[\'https://imgs.xkcd.com/comics/heartbleed_explanation.png\']{.literal}---which
you might have noticed looks a lot like a file path. And in fact, you
can call [os.path.basename()]{.literal} with [comicUrl]{.literal}, and
it will return just the last part of the URL,
[\'heartbleed_explanation.png\']{.literal}. You can use this as the
filename when saving the image to your hard drive. You join this name
with the name of your [xkcd]{.literal} folder using
[os.path.join()]{.literal} so that your program uses backslashes
([\\]{.literal}) on Windows and forward slashes ([/]{.literal}) on macOS
and Linux. Now that you finally have the filename, you can call
[open()]{.literal} to open a new file in [\'wb\']{.literal} "write
binary" mode.

Remember from earlier in this chapter that to save files you've
downloaded using [requests]{.literal}, you need to loop over the return
value of the [iter_content()]{.literal} method. The code in the
[for]{.literal} loop writes out chunks of the image data (at most
100,000 bytes each) to the file and then you close the file. The image
is now saved to your hard drive.

Afterward, the selector [\'a\[rel=\"prev\"\]\']{.literal} identifies the
[\<a\>]{.literal} element with the [rel]{.literal} attribute set to
[prev]{.literal}, and you can use this [\<a\>]{.literal} element's
[href]{.literal} attribute to get the previous comic's URL, which gets
stored in [url]{.literal}. Then the [while]{.literal} loop begins the
entire download process again for this comic.

The output of this program will look like this:

Downloading page https://xkcd.com\...\
Downloading image https://imgs.xkcd.com/comics/phone_alarm.png\...\
Downloading page https://xkcd.com/1358/\...\
Downloading image https://imgs.xkcd.com/comics/nro.png\...\
Downloading page https://xkcd.com/1357/\...\
Downloading image https://imgs.xkcd.com/comics/free_speech.png\...\
Downloading page https://xkcd.com/1356/\...\
Downloading image
https://imgs.xkcd.com/comics/orbital_mechanics.png\...\
Downloading page https://xkcd.com/1355/\...\
Downloading image https://imgs.xkcd.com/comics/airplane_message.png\...\
Downloading page https://xkcd.com/1354/\...\
Downloading image
https://imgs.xkcd.com/comics/heartbleed_explanation.png\...\
\--[snip]{.codeitalic1}\--

This project is a good example of a program that can automatically
follow links in order to scrape large amounts of data from the web. You
can learn about Beautiful Soup's other features from its documentation
at
*[https://www.crummy.com/software/BeautifulSoup/bs4/doc/](https://www.crummy.com/software/BeautifulSoup/bs4/doc/){.calibre6}.*

#### ***Ideas for Similar Programs*** {#calibre_link-402 .h2}

Downloading pages and following links are the basis of many web crawling
programs. Similar programs could also do the following:

-   Back up an entire site by following all of its links.
-   Copy all the messages off a web forum.
-   Duplicate the catalog of items for sale on an online store.

[]{#calibre_link-919 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [requests]{.literal} and [bs4]{.literal}
modules are great as long as you can figure out the URL you need to pass
to [requests.get()]{.literal}. However, sometimes this isn't so easy to
find. Or perhaps the website you want your program to navigate requires
you to log in first. The [selenium]{.literal} module will give your
programs the power to perform such sophisticated tasks.

### **Controlling the Browser with the selenium Module** {#calibre_link-403 .h1}

The [selenium]{.literal} module lets Python directly control the browser
by programmatically clicking links and filling in login information,
almost as though there were a human user interacting with the page.
Using [selenium]{.literal}, you can interact with web pages in a much
more advanced way than with [requests]{.literal} and [bs4]{.literal};
but because it launches a web browser, it is a bit slower and hard to
run in the background if, say, you just need to download some files from
the web.

Still, if you need to interact with a web page in a way that, say,
depends on the JavaScript code that updates the page, you'll need to use
[selenium]{.literal} instead of [requests]{.literal}. That's because
major ecommerce websites such as Amazon almost certainly have software
systems to recognize traffic that they suspect is a script harvesting
their info or signing up for multiple free accounts. These sites may
refuse to serve pages to you after a while, breaking any scripts you've
made. The [selenium]{.literal} module is much more likely to function on
these sites long-term than [requests]{.literal}.

A major "tell" to websites that you're using a script is the
*user-agent* string, which identifies the web browser and is included in
all HTTP requests. For example, the user-agent string for the
[requests]{.literal} module is something like
[\'python-requests/2.21.0\']{.literal}. You can visit a site such as
*[https://www.whatsmyua.info/](https://www.whatsmyua.info/){.calibre6}*
to see your user-agent string. Using [selenium]{.literal}, you're much
more likely to "pass for human" because not only is Selenium's
user-agent is the same as a regular browser (for instance,
[\'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:65.0) Gecko/20100101
Firefox/65.0\']{.literal}), but it has the same traffic patterns: a
[selenium]{.literal}-controlled browser will download images,
advertisements, cookies, and privacy-invading trackers just like a
regular browser. However, [selenium]{.literal} can still be detected by
websites, and major ticketing and ecommerce websites often block
browsers controlled by [selenium]{.literal} to prevent web scraping of
their pages.

#### ***Starting a selenium-Controlled Browser*** {#calibre_link-404 .h2}

The following examples will show you how to control Firefox's web
browser. If you don't already have Firefox, you can download it for free
from *[https://getfirefox.com/](https://getfirefox.com/){.calibre6}*.
You can install [selenium]{.literal} by running [pip install \--user
selenium]{.literal} from a command line terminal. More information is
available in [Appendix A](#calibre_link-2){.calibre6}.

Importing the modules for [selenium]{.literal} is slightly tricky.
Instead of [import selenium]{.literal}, you need to run [from selenium
import webdriver]{.literal}. (The exact reason why the
[selenium]{.literal} module is set up this way is beyond the scope of
this book.) []{#calibre_link-972 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}After that, you can launch the Firefox browser with
[selenium]{.literal}. Enter the following into the interactive shell:

\>\>\> [from selenium import webdriver]{.codestrong1}\
\>\>\> [browser = webdriver.Firefox()]{.codestrong1}\
\>\>\> [type(browser)]{.codestrong1}\
\<class \'selenium.webdriver.firefox.webdriver.WebDriver\'\>\
\>\>\> [browser.get(\'https://inventwithpython.com\')]{.codestrong1}

You'll notice when [webdriver.Firefox()]{.literal} is called, the
Firefox web browser starts up. Calling [type()]{.literal} on the value
[webdriver.Firefox()]{.literal} reveals it's of the
[WebDriver]{.literal} data type. And calling
[browser.get(\'https://inventwithpython.com\')]{.literal} directs the
browser to
*[https://inventwithpython.com/](https://inventwithpython.com/){.calibre6}*.
Your browser should look something like [Figure
12-7](#calibre_link-44){.calibre6}.


[]{#calibre_link-44 .calibre6}![image](../images/000131.jpg){.calibre3}


*Figure 12-7: After we call [webdriver.Firefox()]{.literal1} and
[get()]{.literal1} in Mu, the Firefox browser appears.*

If you encounter the error message "\'geckodriver\' executable needs to
be in [PATH]{.literal}.", then you need to manually download the
webdriver for Firefox before you can use [selenium]{.literal} to control
it. You can also control browsers other than Firefox if you install the
webdriver for them.

For Firefox, go to
*[https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases){.calibre6}*
and download the geckodriver for your operating system. ("Gecko" is the
name of the browser engine used in Firefox.) For example, on Windows
you'll want to download the *geckodriver-v0.24.0-win64.zip* link, and on
macOS, you'll want the *geckodriver-v0.24.0-macos.tar.gz* link. Newer
versions will have slightly different links. The downloaded ZIP file
will contain a *geckodriver.exe* (on Windows) or *geckodriver* (on macOS
and Linux) file that you can put on your system [PATH]{.literal}.
[Appendix B](#calibre_link-35){.calibre6} has information about the
system [PATH]{.literal}, or you can learn more at
*[https://stackoverflow.com/q/40208051/1893164](https://stackoverflow.com/q/40208051/1893164){.calibre6}*.

[]{#calibre_link-1096 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}For Chrome, go to
*[https://sites.google.com/a/chromium.org/chromedriver/downloads](https://sites.google.com/a/chromium.org/chromedriver/downloads){.calibre6}*
and download the ZIP file for your operating system. This ZIP file will
contain a *chromedriver.exe* (on Windows) or *chromedriver* (on macOS or
Linux) file that you can put on your system [PATH]{.literal}.

Other major web browsers also have webdrivers available, and you can
often find these by performing an internet search for "\<browser name\>
webdriver".

If you still have problems opening up a new browser under the control of
[selenium]{.literal}, it may be because the current version of the
browser is incompatible with the [selenium]{.literal} module. One
workaround is to install an older version of the web browser---or, more
simply, an older version of the [selenium]{.literal} module. You can
find the list of [selenium]{.literal} version numbers at
*[https://pypi.org/project/selenium/#history](https://pypi.org/project/selenium/#history){.calibre6}*.
Unfortunately, the compatibility between versions of
[selenium]{.literal} and a browser sometimes breaks, and you may need to
search the web for possible solutions. [Appendix
A](#calibre_link-2){.calibre6} has more information about running pip to
install a specific version of [selenium]{.literal}. (For example, you
might run [pip install \--user -U selenium==3.14.1]{.literal}.)

#### ***Finding Elements on the Page*** {#calibre_link-405 .h2}

[WebDriver]{.literal} objects have quite a few methods for finding
elements on a page. They are divided into the
[find_element\_\*]{.literal} and [find_elements\_\*]{.literal} methods.
The [find_element\_\*]{.literal} methods return a single
[WebElement]{.literal} object, representing the first element on the
page that matches your query. The [find_elements\_\*]{.literal} methods
return a list of [WebElement\_\*]{.literal} objects for *every* matching
element on the page.

[Table 12-3](#calibre_link-45){.calibre6} shows several examples of
[find_element\_\*]{.literal} and [find_elements\_\*]{.literal} methods
being called on a [WebDriver]{.literal} object that's stored in the
variable [browser]{.literal}.

**Table 12-3:** Selenium's [WebDriver]{.literal1} Methods for Finding
Elements

+----------------------------------+----------------------------------+
| **Method name**                  | [WebElement]{.codestrong}        |
|                                  | object/list returned             |
+==================================+==================================+
| [browser.find_e                  | Elements that use the CSS\       |
| lement_by_class_name(]{.literal} | class [name]{.codeitalic}        |
| [name]{.codeitalic}[)]{.literal} |                                  |
|                                  |                                  |
| [browser.find_el                 |                                  |
| ements_by_class_name(]{.literal} |                                  |
| [name]{.codeitalic}[)]{.literal} |                                  |
+----------------------------------+----------------------------------+
| [browser.find_element_           | Elements that match the CSS\     |
| by_css_selector(]{.literal}[sele | [selector]{.codeitalic}          |
| ctor]{.codeitalic}[)]{.literal}\ |                                  |
| [browser.find_elements           |                                  |
| _by_css_selector(]{.literal}[sel |                                  |
| ector]{.codeitalic}[)]{.literal} |                                  |
+----------------------------------+----------------------------------+
| [brow                            | Elements with a matching         |
| ser.find_element_by_id(]{.litera | [id]{.codeitalic}\               |
| l}[id]{.codeitalic}[)]{.literal} | attribute value                  |
|                                  |                                  |
| [brows                           |                                  |
| er.find_elements_by_id(]{.litera |                                  |
| l}[id]{.codeitalic}[)]{.literal} |                                  |
+----------------------------------+----------------------------------+
| [browser.find_                   | [\<a\>]{.literal} elements that  |
| element_by_link_text(]{.literal} | completely\                      |
| [text]{.codeitalic}[)]{.literal} | match the [text]{.codeitalic}    |
|                                  | provided                         |
| [browser.find_e                  |                                  |
| lements_by_link_text(]{.literal} |                                  |
| [text]{.codeitalic}[)]{.literal} |                                  |
+----------------------------------+----------------------------------+
| [browser.find_element_           | [\<a\>]{.literal} elements that  |
| by_partial_link_text(]{.literal} | contain the\                     |
| [text]{.codeitalic}[)]{.literal} | [text]{.codeitalic} provided     |
|                                  |                                  |
| [browser.find_elements_          |                                  |
| by_partial_link_text(]{.literal} |                                  |
| [text]{.codeitalic}[)]{.literal} |                                  |
+----------------------------------+----------------------------------+
| [browser.                        | Elements with a matching         |
| find_element_by_name(]{.literal} | [name]{.codeitalic}\             |
| [name]{.codeitalic}[)]{.literal} | attribute value                  |
|                                  |                                  |
| [browser.f                       |                                  |
| ind_elements_by_name(]{.literal} |                                  |
| [name]{.codeitalic}[)]{.literal} |                                  |
+----------------------------------+----------------------------------+
| [browser.find_                   | Elements with a matching tag     |
| element_by_tag_name(]{.literal}[ | [name]{.codeitalic}\             |
| name]{.codeitalic}[)]{.literal}\ | (case-insensitive; an            |
| [browser.find_                   | [\<a\>]{.literal} element is\    |
| elements_by_tag_name(]{.literal} | matched by [\'a\']{.literal} and |
| [name]{.codeitalic}[)]{.literal} | [\'A\']{.literal})               |
+----------------------------------+----------------------------------+

[]{#calibre_link-843 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Except for the [\*\_by_tag_name()]{.literal}
methods, the arguments to all the methods are case sensitive. If no
elements exist on the page that match what the method is looking for,
the [selenium]{.literal} module raises a [NoSuchElement]{.literal}
exception. If you do not want this exception to crash your program, add
[try]{.literal} and [except]{.literal} statements to your code.

Once you have the [WebElement]{.literal} object, you can find out more
about it by reading the attributes or calling the methods in [Table
12-4](#calibre_link-46){.calibre6}.

**Table 12-4:** [WebElement]{.literal1} Attributes and Methods

  **Attribute or method**                                      **Description**
  ------------------------------------------------------------ ---------------------------------------------------------------------------------------------------------------------------------
  [tag_name]{.literal}                                         The tag name, such as [\'a\']{.literal} for an [\<a\>]{.literal} element
  [get_attribute(]{.literal}[name]{.codeitalic}[)]{.literal}   The value for the element's [name]{.literal} attribute
  [text]{.literal}                                             The text within the element, such as [\'hello\']{.literal} in [\<span\>hello \</span\>]{.literal}
  [clear()]{.literal}                                          For text field or text area elements, clears the text typed into it
  [is_displayed()]{.literal}                                   Returns [True]{.literal} if the element is visible; otherwise returns [False]{.literal}
  [is_enabled()]{.literal}                                     For input elements, returns [True]{.literal} if the element is enabled; otherwise returns [False]{.literal}
  [is_selected()]{.literal}                                    For checkbox or radio button elements, returns [True]{.literal} if the element is selected; otherwise returns [False]{.literal}
  [location]{.literal}                                         A dictionary with keys [\'x\']{.literal} and [\'y\']{.literal} for the position of the element in the page

For example, open a new file editor tab and enter the following program:

from selenium import webdriver\
browser = webdriver.Firefox()\
browser.get(\'https://inventwithpython.com\')\
try:\
    elem = browser.find_element_by_class_name(\' cover-thumb\')\
    print(\'Found \<%s\> element with that class name!\' %
(elem.tag_name))\
except:\
    print(\'Was not able to find an element with that name.\')

Here we open Firefox and direct it to a URL. On this page, we try to
find elements with the class name [\'bookcover\']{.literal}, and if such
an element is found, we print its tag name using the
[tag_name]{.literal} attribute. If no such element was found, we print a
different message.

This program will output the following:

Found \<img\> element with that class name!

We found an element with the class name [\'bookcover\']{.literal} and
the tag name [\'img\']{.literal}.

#### []{#calibre_link-845 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Clicking the Page*** {#calibre_link-406 .h2}

[WebElement]{.literal} objects returned from the
[find_element\_\*]{.literal} and [find_elements\_\*]{.literal} methods
have a [click()]{.literal} method that simulates a mouse click on that
element. This method can be used to follow a link, make a selection on a
radio button, click a Submit button, or trigger whatever else might
happen when the element is clicked by the mouse. For example, enter the
following into the interactive shell:

\>\>\> [from selenium import webdriver]{.codestrong1}\
\>\>\> [browser = webdriver.Firefox()]{.codestrong1}\
\>\>\> [browser.get(\'https://inventwithpython.com\')]{.codestrong1}\
\>\>\> [linkElem = browser.find_element_by_link_text(\'Read Online for
Free\')]{.codestrong1}\
\>\>\> [type(linkElem)]{.codestrong1}\
\<class \'selenium.webdriver.remote.webelement.FirefoxWebElement\'\>\
\>\>\> [linkElem.click()]{.codestrong1} \# follows the \"Read Online for
Free\" link

This opens Firefox to
*[https://inventwithpython.com/](https://inventwithpython.com/){.calibre6}*,
gets the [WebElement]{.literal} object for the [\<a\>]{.literal} element
with the text *Read It Online*, and then simulates clicking that
[\<a\>]{.literal} element. It's just like if you clicked the link
yourself; the browser then follows that link.

#### ***Filling Out and Submitting Forms*** {#calibre_link-407 .h2}

Sending keystrokes to text fields on a web page is a matter of finding
the [\<input\>]{.literal} or [\<textarea\>]{.literal} element for that
text field and then calling the [send_keys()]{.literal}method. For
example, enter the following into the interactive shell:

\>\>\> [from selenium import webdriver]{.codestrong1}\
\>\>\> [browser = webdriver.Firefox()]{.codestrong1}\
\>\>\> [browser.get(\'https://login.metafilter.com\')]{.codestrong1}\
\>\>\> [userElem =
browser.find_element_by_id(\'user_name)]{.codestrong1}\
\>\>\>
[userElem.send_keys(\']{.codestrong1}[[your_real_username_here]{.codestrong1}]{.codeitalic1}[\')]{.codestrong1}\
\
\
\>\>\> [passwordElem =
browser.find_element_by_id(\'user_pass\')]{.codestrong1}\
\>\>\>
[passwordElem.send_keys(\']{.codestrong1}[[your_real_password_here]{.codestrong1}]{.codeitalic1}[\')]{.codestrong1}\
\>\>\> [passwordElem.submit()]{.codestrong1}

As long as login page for MetaFilter hasn't changed the [id]{.literal}
of the Username and Password text fields since this book was published,
the previous code will fill in those text fields with the provided text.
(You can always use the browser's inspector to verify the
[id]{.literal}.) Calling the [submit()]{.literal} method on any element
will have the same result as clicking the Submit button for the form
that element is in. (You could have just as easily called
[emailElem.submit()]{.literal}, and the code would have done the same
thing.)


**[WARNING]{.notes}**

*Avoid putting your passwords in source code whenever possible. It's
easy to accidentally leak your passwords to others when they are left
unencrypted on your hard drive. If possible, have your program prompt
users to enter their passwords from the keyboard using the*
[pyinputplus.inputPassword()]{.literal} *function described in [Chapter
8](#calibre_link-47){.calibre6}.*


#### []{#calibre_link-1095 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Sending Special Keys*** {#calibre_link-408 .h2}

The [selenium]{.literal} module has a module for keyboard keys that are
impossible to type into a string value, which function much like escape
characters. These values are stored in attributes in the
[selenium.webdriver.common.keys]{.literal} module. Since that is such a
long module name, it's much easier to run [from
selenium.webdriver.common.keys import Keys]{.literal} at the top of your
program; if you do, then you can simply write [Keys]{.literal} anywhere
you'd normally have to write [selenium.webdriver.common.keys]{.literal}.
[Table 12-5](#calibre_link-48){.calibre6} lists the commonly used
[Keys]{.literal} variables.

**Table 12-5:** Commonly Used Variables in the
[selenium.webdriver.common.keys]{.literal1} Module

  **Attributes**                                                                                      **Meanings**
  --------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------
  [Keys.DOWN]{.literal}, [Keys.UP]{.literal}, [Keys.LEFT]{.literal}, [Keys.RIGHT]{.literal}           The keyboard arrow keys
  [Keys.ENTER]{.literal}, [Keys.RETURN]{.literal}                                                     The [ENTER]{.small} and [RETURN]{.small} keys
  [Keys.HOME]{.literal}, [Keys.END]{.literal}, [Keys.PAGE_DOWN]{.literal}, [Keys.PAGE_UP]{.literal}   The [HOME]{.small}, [END]{.small}, [PAGEDOWN]{.small}, and [PAGEUP]{.small} keys
  [Keys.ESCAPE]{.literal}, [Keys.BACK_SPACE]{.literal}, [Keys.DELETE]{.literal}                       The [ESC]{.small}, [BACKSPACE]{.small}, and [DELETE]{.small} keys
  [Keys.F1]{.literal}, [Keys.F2]{.literal}, . . . , [Keys.F12]{.literal}                              The F1 to F12 keys at the top of the keyboard
  [Keys.TAB]{.literal}                                                                                The [TAB]{.small} key

For example, if the cursor is not currently in a text field, pressing
the [HOME]{.small} and [END]{.small} keys will scroll the browser to the
top and bottom of the page, respectively. Enter the following into the
interactive shell, and notice how the [send_keys()]{.literal} calls
scroll the page:

\>\>\> [from selenium import webdriver]{.codestrong1}\
\>\>\> [from selenium.webdriver.common.keys import Keys]{.codestrong1}\
\>\>\> [browser = webdriver.Firefox()]{.codestrong1}\
\>\>\> [browser.get(\'https://nostarch.com\')]{.codestrong1}\
\>\>\> [htmlElem =
browser.find_element_by_tag_name(\'html\')]{.codestrong1}\
\>\>\> [htmlElem.send_keys(Keys.END)]{.codestrong1}     # scrolls to
bottom\
\>\>\> [htmlElem.send_keys(Keys.HOME)]{.codestrong1}    # scrolls to top

The [\<html]{.literal}\> tag is the base tag in HTML files: the full
content of the HTML file is enclosed within the [\<html\>]{.literal} and
[\</html\>]{.literal} tags. Calling
[browser.find_element_by_tag_name(\'html\')]{.literal} is a good place
to send keys to the general web page. This would be useful if, for
example, new content is loaded once you've scrolled to the bottom of the
page.

#### []{#calibre_link-808 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Clicking Browser Buttons*** {#calibre_link-409 .h2}

The [selenium]{.literal} module can simulate clicks on various browser
buttons as well through the following methods:

[browser.back()]{.codestrong} Clicks the Back button.

[browser.forward()]{.codestrong} Clicks the Forward button.

[browser.refresh()]{.codestrong} Clicks the Refresh/Reload button.

[browser.quit()]{.codestrong} Clicks the Close Window button.

#### ***More Information on Selenium*** {#calibre_link-410 .h2}

Selenium can do much more beyond the functions described here. It can
modify your browser's cookies, take screenshots of web pages, and run
custom JavaScript. To learn more about these features, you can visit the
[selenium]{.literal} documentation at
*[https://selenium-python.readthedocs.org/](https://selenium-python.readthedocs.org/){.calibre6}*.

### **Summary** {#calibre_link-411 .h1}

Most boring tasks aren't limited to the files on your computer. Being
able to programmatically download web pages will extend your programs to
the internet. The [requests]{.literal} module makes downloading
straightforward, and with some basic knowledge of HTML concepts and
selectors, you can utilize the [BeautifulSoup]{.literal} module to parse
the pages you download.

But to fully automate any web-based tasks, you need direct control of
your web browser through the [selenium]{.literal} module. The
[selenium]{.literal} module will allow you to log in to websites and
fill out forms automatically. Since a web browser is the most common way
to send and receive information over the internet, this is a great
ability to have in your programmer toolkit.

### **Practice Questions** {#calibre_link-412 .h1}

[1](#calibre_link-49){#calibre_link-1379 .calibre6}. Briefly describe
the differences between the [webbrowser]{.literal},
[requests]{.literal}, [bs4]{.literal}, and [selenium]{.literal} modules.

[2](#calibre_link-50){#calibre_link-1380 .calibre6}. What type of object
is returned by [requests.get()]{.literal}? How can you access the
downloaded content as a string value?

[3](#calibre_link-51){#calibre_link-1381 .calibre6}. What
[requests]{.literal} method checks that the download worked?

[4](#calibre_link-52){#calibre_link-1382 .calibre6}. How can you get the
HTTP status code of a [requests]{.literal} response?

[5](#calibre_link-53){#calibre_link-1383 .calibre6}. How do you save a
[requests]{.literal} response to a file?

[6](#calibre_link-54){#calibre_link-1384 .calibre6}. What is the
keyboard shortcut for opening a browser's developer tools?

[7](#calibre_link-55){#calibre_link-1385 .calibre6}. How can you view
(in the developer tools) the HTML of a specific element on a web page?

[8](#calibre_link-56){#calibre_link-1386 .calibre6}. What is the CSS
selector string that would find the element with an [id]{.literal}
attribute of [main]{.literal}?

[9](#calibre_link-57){#calibre_link-1387 .calibre6}.
[]{#calibre_link-1807 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}What is the CSS selector string that would find the
elements with a CSS class of [highlight]{.literal}?

[10](#calibre_link-58){#calibre_link-1388 .calibre6}. What is the CSS
selector string that would find all the [\<div\>]{.literal} elements
inside another [\<div\>]{.literal} element?

[11](#calibre_link-59){#calibre_link-1389 .calibre6}. What is the CSS
selector string that would find the [\<button\>]{.literal} element with
a [value]{.literal} attribute set to [favorite]{.literal}?

[12](#calibre_link-60){#calibre_link-1390 .calibre6}. Say you have a
Beautiful Soup [Tag]{.literal} object stored in the variable
[spam]{.literal} for the element [\<div\>Hello,
world!\</div\>]{.literal}. How could you get a string [\'Hello,
world!\']{.literal} from the [Tag]{.literal} object?

[13](#calibre_link-61){#calibre_link-1391 .calibre6}. How would you
store all the attributes of a Beautiful Soup [Tag]{.literal} object in a
variable named [linkElem]{.literal}?

[14](#calibre_link-62){#calibre_link-1392 .calibre6}. Running [import
selenium]{.literal} doesn't work. How do you properly import the
[selenium]{.literal} module?

[15](#calibre_link-63){#calibre_link-1393 .calibre6}. What's the
difference between the [find_element\_\*]{.literal} and
[find_elements\_\*]{.literal} methods?

[16](#calibre_link-64){#calibre_link-1394 .calibre6}. What methods do
Selenium's [WebElement]{.literal} objects have for simulating mouse
clicks and keyboard keys?

[17](#calibre_link-65){#calibre_link-1395 .calibre6}. You could call
[send_keys(Keys.ENTER)]{.literal} on the Submit button's
[WebElement]{.literal} object, but what is an easier way to submit a
form with [selenium]{.literal}?

[18](#calibre_link-66){#calibre_link-1396 .calibre6}. How can you
simulate clicking a browser's Forward, Back, and Refresh buttons with
[selenium]{.literal}?

### **Practice Projects** {#calibre_link-413 .h1}

For practice, write programs to do the following tasks.

#### ***Command Line Emailer*** {#calibre_link-414 .h2}

Write a program that takes an email address and string of text on the
command line and then, using [selenium]{.literal}, logs in to your email
account and sends an email of the string to the provided address. (You
might want to set up a separate email account for this program.)

This would be a nice way to add a notification feature to your programs.
You could also write a similar program to send messages from a Facebook
or Twitter account.

#### ***Image Site Downloader*** {#calibre_link-415 .h2}

Write a program that goes to a photo-sharing site like Flickr or Imgur,
searches for a category of photos, and then downloads all the resulting
images. You could write a program that works with any photo site that
has a search feature.

#### []{#calibre_link-1808 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***2048*** {#calibre_link-416 .h2}

*2048* is a simple game where you combine tiles by sliding them up,
down, left, or right with the arrow keys. You can actually get a fairly
high score by repeatedly sliding in an up, right, down, and left pattern
over and over again. Write a program that will open the game at
*[https://gabrielecirulli.github.io/2048/](https://gabrielecirulli.github.io/2048/){.calibre6}*
and keep sending up, right, down, and left keystrokes to automatically
play the game.

#### ***Link Verification*** {#calibre_link-417 .h2}

Write a program that, given the URL of a web page, will attempt to
download every linked page on the page. The program should flag any
pages that have a 404 "Not Found" status code and print them out as
broken links.[]{#calibre_link-1809 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}

