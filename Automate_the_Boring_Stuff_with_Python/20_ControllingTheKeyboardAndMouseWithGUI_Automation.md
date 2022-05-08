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

[![](/images/prev.png)](../chapter19)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../appendixa)

</div>

::: {#calibre_link-1627 .calibre}
## []{#calibre_link-995 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[20]{.big} CONTROLLING THE KEYBOARD AND MOUSE WITH GUI AUTOMATION** {#calibre_link-634 .h2b}

::: image
![Image](../images/000071.jpg){.calibre3}
:::

Knowing various Python modules for editing spreadsheets, downloading
files, and launching programs is useful, but sometimes there just aren't
any modules for the applications you need to work with. The ultimate
tools for automating tasks on your computer are pro­grams you write that
directly control the keyboard and mouse. These programs can control
other applications by sending them virtual keystrokes and mouse clicks,
just as if you were sitting at your computer and interacting with the
applications yourself.

This technique is known as *graphical user interface automation*, or
*GUI automation* for short. With GUI automation, your programs can do
anything that a human user sitting at the computer can do, except spill
coffee on the keyboard. Think of GUI automation as programming a robotic
arm. You can program the robotic arm to type at your keyboard and move
your mouse for you. This technique is particularly useful for tasks that
involve a lot of mindless clicking or filling out of forms.

[]{#calibre_link-994 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Some companies sell innovative (and pricey)
"automation solutions," usually marketed as *robotic process automation*
*(RPA)*. These products are effectively no different than the Python
scripts you can make yourself with the [pyautogui]{.literal} module,
which has functions for simulating mouse movements, button clicks, and
mouse wheel scrolls. This chapter covers only a subset of PyAutoGUI's
features; you can find the full documentation at
*[https://pyautogui.readthedocs.io/](https://pyautogui.readthedocs.io/){.calibre6}*.

### **Installing the pyautogui Module** {#calibre_link-635 .h1}

The [pyautogui]{.literal} module can send virtual keypresses and mouse
clicks to Windows, macOS, and Linux. Windows and macOS users can simply
use pip to install PyAutoGUI. However, Linux users will first have to
install some software that PyAutoGUI depends on. Open a terminal window
and enter the following commands:

-   [sudo apt-get install scrot]{.literal}
-   [sudo apt-get install python3-tk]{.literal}
-   [sudo apt-get install python3-dev]{.literal}

To install PyAutoGUI, run [pip install \--user pyautogui]{.literal}.
Don't use [sudo]{.literal} with [pip]{.literal}; you may install modules
to the Python installation that the operating system uses, causing
conflicts with any scripts that rely on its original configuration.
However, you should use the [sudo]{.literal} command when installing
applications with [apt-get]{.literal}.

[Appendix A](#calibre_link-2){.calibre6} has complete information on
installing third-party modules. To test whether PyAutoGUI has been
installed correctly, run [import pyautogui]{.codestrong} from the
interactive shell and check for any error messages.

::: note
**[WARNING]{.notes}**

*Don't save your program as* pyautogui.py*. When you run [import
pyautogui]{.codeitalic}, Python will import your program instead of the
PyAutoGUI and you'll get error messages like [AttributeError: module
\'pyautogui\' has no attribute \'click\']{.codeitalic}.*
:::

### **Setting Up Accessibility Apps on macOS** {#calibre_link-636 .h1}

As a security measure, macOS doesn't normally let programs control the
mouse or keyboard. To make PyAutoGUI work on macOS, you must set the
program running your Python script to be an accessibility application.
Without this step, your PyAutoGUI function calls will have no effect.

Whether you run your Python programs from Mu, IDLE, or the Terminal,
have that application open. Then open the System Preferences and go to
the Accessibility tab. The currently open applications will appear under
the "Allow the apps below to control your computer" label. Check
[]{#calibre_link-963 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Mu, IDLE, Terminal, or whichever app you use to run
your Python scripts. You'll be prompted to enter your password to
confirm these changes.

### **Staying on Track** {#calibre_link-637 .h1}

Before you jump into a GUI automation, you should know how to escape
problems that may arise. Python can move your mouse and type keystrokes
at an incredible speed. In fact, it might be too fast for other programs
to keep up with. Also, if something goes wrong but your program keeps
moving the mouse around, it will be hard to tell what exactly the
program is doing or how to recover from the problem. Like the enchanted
brooms from Disney's *The Sorcerer's Apprentice*, which kept
filling---and then overfilling---Mickey's tub with water, your program
could get out of control even though it's following your instructions
perfectly. Stopping the program can be difficult if the mouse is moving
around on its own, preventing you from clicking the Mu Editor window to
close it. Fortunately, there are several ways to prevent or recover from
GUI automation problems.

#### ***Pauses and Fail-Safes*** {#calibre_link-638 .h2}

If your program has a bug and you're unable to use the keyboard and
mouse to shut it down, you can use PyAutoGUI's fail-safe feature.
Quickly slide the mouse to one of the four corners of the screen. Every
PyAutoGUI function call has a 10th-of-a-second delay after performing
its action to give you enough time to move the mouse to a corner. If
PyAutoGUI then finds that the mouse cursor is in a corner, it raises the
[pyautogui.FailSafeException]{.literal} exception. Non-PyAutoGUI
instructions will not have this 10th-of-a-second delay.

If you find yourself in a situation where you need to stop your
PyAutoGUI program, just slam the mouse toward a corner to stop it.

#### ***Shutting Down Everything by Logging Out*** {#calibre_link-639 .h2}

Perhaps the simplest way to stop an out-of-control GUI automation
program is to log out, which will shut down all running programs. On
Windows and Linux, the logout hotkey is
[CTRL]{.small}-[ALT]{.small}-[DEL]{.small}. On macOS, it is
![image](../images/000064.jpg){.calibre3}-[SHIFT]{.small}-[OPTION]{.small}-Q.
By logging out, you'll lose any unsaved work, but at least you won't
have to wait for a full reboot of the computer.

### **Controlling Mouse Movement** {#calibre_link-640 .h1}

In this section, you'll learn how to move the mouse and track its
position on the screen using PyAutoGUI, but first you need to understand
how PyAutoGUI works with coordinates.

The mouse functions of PyAutoGUI use x- and y-coordinates. [Figure
20-1](#calibre_link-1628){.calibre6} shows the coordinate system for the
computer screen; it's similar to the coordinate system used for images,
discussed in [Chapter 19](#calibre_link-608){.calibre6}. The *origin*,
where *x* []{#calibre_link-861 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}and *y* are both zero, is at the upper-left corner
of the screen. The x-coordinates increase going to the right, and the
y-coordinates increase going down. All coordinates are positive
integers; there are no negative coordinates.

::: image1
[]{#calibre_link-1628
.calibre6}![image](../images/000125.jpg){.calibre3}
:::

*Figure 20-1: The coordinates of a computer screen with 1920×1080
resolution*

Your *resolution* is how many pixels wide and tall your screen is. If
your screen's resolution is set to 1920×1080, then the coordinate for
the upper-left corner will be (0, 0), and the coordinate for the
bottom-right corner will be (1919, 1079).

The [pyautogui.size()]{.literal} function returns a two-integer tuple of
the screen's width and height in pixels. Enter the following into the
interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [wh =]{.codestrong1} [pyautogui.size()]{.codestrong1} \# Obtain
the screen resolution.\
\>\>\> [wh]{.codestrong1}\
Size(width=1920, height=1080)\
\>\>\> [wh\[0\]]{.codestrong1}\
1920\
\>\>\> [wh.width]{.codestrong1}\
1920

The [pyautogui.size()]{.literal} function returns [(1920,
1080)]{.literal} on a computer with a 1920×1080 resolution; depending on
your screen's resolution, your return value may be different. The
[Size]{.literal} object returned by [size()]{.literal} is a named tuple.
*Named tuples* have numeric indexes, like regular tuples, and attribute
names, like objects: both [wh\[0\]]{.literal} and [wh.width]{.literal}
evaluate to the width of the screen. (Named tuples are beyond the scope
of this book. Just remember that you can use them the same way you use
tuples.)

#### []{#calibre_link-991 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Moving the Mouse*** {#calibre_link-641 .h2}

Now that you understand screen coordinates, let's move the mouse. The
[pyautogui.moveTo()]{.literal} function will instantly move the mouse
cursor to a specified position on the screen. Integer values for the x-
and y-coordinates make up the function's first and second arguments,
respectively. An optional [duration]{.literal} integer or float keyword
argument specifies the number of seconds it should take to move the
mouse to the destination. If you leave it out, the default is
[0]{.literal} for instantaneous movement. (All of the
[duration]{.literal} keyword arguments in PyAutoGUI functions are
optional.) Enter the following into the interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [for i in range(10):]{.codestrong1} \# Move mouse in a square.\
[\...       pyautogui.moveTo(100, 100, duration=0.25)]{.codestrong1}\
[\...       pyautogui.moveTo(200, 100, duration=0.25)]{.codestrong1}\
[\...       pyautogui.moveTo(200, 200, duration=0.25)]{.codestrong1}\
[\...       pyautogui.moveTo(100, 200, duration=0.25)]{.codestrong1}

This example moves the mouse cursor clockwise in a square pattern among
the four coordinates provided a total of 10 times. Each movement takes a
quarter of a second, as specified by the [duration=0.25]{.literal}
keyword argument. If you hadn't passed a third argument to any of the
[pyautogui.moveTo()]{.literal} calls, the mouse cursor would have
instantly teleported from point to point.

The [pyautogui.move()]{.literal} function moves the mouse cursor
*relative to its current position*. The following example moves the
mouse in the same square pattern, except it begins the square from
wherever the mouse happens to be on the screen when the code starts
running:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [for i in range(10):]{.codestrong1}\
[\...       pyautogui.move(100, 0, duration=0.25)   ]{.codestrong1}\#
right\
[\...       pyautogui.move(0, 100, duration=0.25)   ]{.codestrong1}\#
down\
[\...       pyautogui.move(-100, 0, duration=0.25)  ]{.codestrong1}\#
left\
[\...       pyautogui.move(0, -100, duration=0.25)  ]{.codestrong1}\# up

The [pyautogui.move()]{.literal} function also takes three arguments:
how many pixels to move horizontally to the right, how many pixels to
move vertically downward, and (optionally) how long it should take to
complete the movement. A negative integer for the first or second
argument will cause the mouse to move left or upward, respectively.

#### ***Getting the Mouse Position*** {#calibre_link-642 .h2}

You can determine the mouse's current position by calling the
[pyautogui.position()]{.literal} function, which will return a
[Point]{.literal} named tuple of the mouse cursor's *x* and *y*
positions at the time of the function call. Enter the following into the
interactive shell, moving the mouse around after each call:

\>\>\> [pyautogui.position()]{.codestrong1} \# Get current mouse
position.\
Point(x=311, y=622)\
[]{#calibre_link-844 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [pyautogui.position()]{.codestrong1} \# Get
current mouse position again.\
Point(x=377, y=481)\
\>\>\> [p =]{.codestrong1} [pyautogui.position()]{.codestrong1} \# And
again.\
\>\>\> [p]{.codestrong1}\
Point(x=1536, y=637)\
\>\>\> [p\[0\]]{.codestrong1} \# The x-coordinate is at index 0.\
1536\
\>\>\> [p.x]{.codestrong1} \# The x-coordinate is also in the x
attribute.\
1536

Of course, your return values will vary depending on where your mouse
cursor is.

### **Controlling Mouse Interaction** {#calibre_link-643 .h1}

Now that you know how to move the mouse and figure out where it is on
the screen, you're ready to start clicking, dragging, and scrolling.

#### ***Clicking the Mouse*** {#calibre_link-644 .h2}

To send a virtual mouse click to your computer, call the
[pyautogui.click()]{.literal} method. By default, this click uses the
left mouse button and takes place wherever the mouse cursor is currently
located. You can pass x- and y-coordinates of the click as optional
first and second arguments if you want it to take place somewhere other
than the mouse's current position.

If you want to specify which mouse button to use, include the
[button]{.literal} keyword argument, with a value of
[\'left\']{.literal}, [\'middle\']{.literal}, or [\'right\']{.literal}.
For example, [pyautogui.click(100, 150, button=\'left\')]{.literal} will
click the left mouse button at the coordinates (100, 150), while
[pyautogui.click(200, 250, button=\'right\')]{.literal} will perform a
right-click at (200, 250).

Enter the following into the interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [pyautogui.click(10, 5)]{.codestrong1} \# Move mouse to (10, 5)
and click.

You should see the mouse pointer move to near the top-left corner of
your screen and click once. A full "click" is defined as pushing a mouse
button down and then releasing it back up without moving the cursor. You
can also perform a click by calling [pyautogui.mouseDown()]{.literal},
which only pushes the mouse button down, and
[pyautogui.mouseUp()]{.literal}, which only releases the button. These
functions have the same arguments as [click()]{.literal}, and in fact,
the [click()]{.literal} function is just a convenient wrapper around
these two function calls.

As a further convenience, the [pyautogui.doubleClick()]{.literal}
function will perform two clicks with the left mouse button, while the
[pyautogui.rightClick()]{.literal} and
[pyautogui.middleClick()]{.literal} functions will perform a click with
the right and middle mouse buttons, respectively.

#### []{#calibre_link-920 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Dragging the Mouse*** {#calibre_link-645 .h2}

*Dragging* means moving the mouse while holding down one of the mouse
buttons. For example, you can move files between folders by dragging the
folder icons, or you can move appointments around in a calendar app.

PyAutoGUI provides the [pyautogui.dragTo()]{.literal} and
[pyautogui.drag()]{.literal} functions to drag the mouse cursor to a new
location or a location relative to its current one. The arguments for
[dragTo()]{.literal} and [drag()]{.literal} are the same as
[moveTo()]{.literal} and [move()]{.literal}: the x-coordinate/horizontal
movement, the y-coordinate/vertical movement, and an optional duration
of time. (macOS does not drag correctly when the mouse moves too
quickly, so passing a [duration]{.literal} keyword argument is
recommended.)

To try these functions, open a graphics-drawing application such as MS
Paint on Windows, Paintbrush on macOS, or GNU Paint on Linux. (If you
don't have a drawing application, you can use the online one at
*[https://sumopaint.com/](https://sumopaint.com/){.calibre6}*.) I will
use PyAutoGUI to draw in these applications.

With the mouse cursor over the drawing application's canvas and the
Pencil or Brush tool selected, enter the following into a new file
editor window and save it as *spiralDraw.py*:

   import pyautogui, time\
[?]{.ent} time.sleep(5)\
[?]{.ent} pyautogui.click()    # Click to make the window active.\
   distance = 300\
   change = 20\
   while distance \> 0:\
    [?]{.ent} pyautogui.drag(distance, 0, duration=0.2)   # Move right.\
    [?]{.ent} distance = distance -- change\
    [?]{.ent} pyautogui.drag(0, distance, duration=0.2)   # Move down.\
    [?]{.ent} pyautogui.drag(-distance, 0, duration=0.2)  # Move left.\
       distance = distance -- change\
       pyautogui.drag(0, -distance, duration=0.2)  # Move up.

When you run this program, there will be a five-second delay [?]{.ent}
for you to move the mouse cursor over the drawing program's window with
the Pencil or Brush tool selected. Then *spiralDraw.py* will take
control of the mouse and click to make the drawing program's window
active [?]{.ent}. The *active window* is the window that currently
accepts keyboard input, and the actions you take---like typing or, in
this case, dragging the mouse---will affect that window. The active
window is also known as the *focused* or *foreground window*. Once the
drawing program is active, *spiralDraw.py* draws a square spiral pattern
like the one on the left of [Figure
20-2](#calibre_link-1629){.calibre6}. While you can also create a square
spiral image by using the Pillow module discussed in [Chapter
19](#calibre_link-608){.calibre6}, creating the image by controlling the
mouse to draw it in MS Paint lets you make use of this program's various
brush styles, like in [Figure 20-2](#calibre_link-1629){.calibre6} on
the right, as well as other advanced features, like gradients or the
fill bucket. You can preselect the brush settings yourself (or have your
Python code select these settings) and then run the spiral-drawing
program.

::: image1
[]{#calibre_link-990 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1629
.calibre6}![image](../images/000073.jpg){.calibre3}
:::

*Figure 20-2: The results from the [pyautogui.drag()]{.literal1}
example, drawn with MS Paint's different brushes*

The [distance]{.literal} variable starts at [200]{.literal}, so on the
first iteration of the [while]{.literal} loop, the first
[drag()]{.literal} call drags the cursor 200 pixels to the right, taking
0.2 seconds [?]{.ent}. [distance]{.literal} is then decreased to 195
[?]{.ent}, and the second [drag()]{.literal} call drags the cursor 195
pixels down [?]{.ent}. The third [drag()]{.literal} call drags the
cursor --195 horizontally (195 to the left) [?]{.ent},
[distance]{.literal} is decreased to 190, and the last
[drag()]{.literal} call drags the cursor 190 pixels up. On each
iteration, the mouse is dragged right, down, left, and up, and
[distance]{.literal} is slightly smaller than it was in the previous
iteration. By looping over this code, you can move the mouse cursor to
draw a square spiral.

You could draw this spiral by hand (or rather, by mouse), but you'd have
to work slowly to be so precise. PyAutoGUI can do it in a few seconds!

::: note
**[NOTE]{.notes}**

*At the time of this writing, PyAutoGUI can't send mouse clicks or
keystrokes to certain programs, such as antivirus software (to prevent
viruses from disabling the software) or video games on Windows (which
use a different method of receiving mouse and keyboard input). You can
check the online documentation at*
[https://pyautogui.readthedocs.io/](https://pyautogui.readthedocs.io/){.calibre6}
*to see if these features have been added.*
:::

#### ***Scrolling the Mouse*** {#calibre_link-646 .h2}

The final PyAutoGUI mouse function is [scroll()]{.literal}, which you
pass an integer argument for how many units you want to scroll the mouse
up or down. The size of a unit varies for each operating system and
application, so you'll have to experiment to see exactly how far it
scrolls in your particular situation. []{#calibre_link-992 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}The scrolling takes place
at the mouse cursor's current position. Passing a positive integer
scrolls up, and passing a negative integer scrolls down. Run the
following in Mu Editor's interactive shell while the mouse cursor is
over the Mu Editor window:

\>\>\> [pyautogui.scroll(200)]{.codestrong1}

You'll see Mu scroll upward if the mouse cursor is over a text field
that can be scrolled up.

### **Planning Your Mouse Movements** {#calibre_link-647 .h1}

One of the difficulties of writing a program that will automate clicking
the screen is finding the x- and y-coordinates of the things you'd like
to click. The [pyautogui.mouseInfo()]{.literal} function can help you
with this.

The [pyautogui.mouseInfo()]{.literal} function is meant to be called
from the interactive shell, rather than as part of your program. It
launches a small application named MouseInfo that's included with
PyAutoGUI. The window for the application looks like [Figure
20-3](#calibre_link-1630){.calibre6}.

::: image1
[]{#calibre_link-1630
.calibre6}![image](../images/000019.jpg){.calibre3}
:::

*Figure 20-3: The MouseInfo application's window*

Enter the following into the interactive shell:

\>\>\> import pyautogui\
\>\>\> pyautogui.mouseInfo()

This makes the MouseInfo window appear. This window gives you
information about the mouse's cursor current position, as well the color
of the pixel underneath the mouse cursor, as a three-integer RGB tuple
and as a hex value. The color itself appears in the color box in the
window.

[]{#calibre_link-996 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}To help you record this coordinate or pixel
information, you can click one of the eight Copy or Log buttons. The
Copy All, Copy XY, Copy RGB, and Copy RGB Hex buttons will copy their
respective information to the clipboard. The Log All, Log XY, Log RGB,
and Log RGB Hex buttons will write their respective information to the
large text field in the window. You can save the text in this log text
field by clicking the Save Log button.

By default, the 3 Sec. Button Delay checkbox is checked, causing a
three-second delay between clicking a Copy or Log button and the copying
or logging taking place. This gives you a short amount of time in which
to click the button and then move the mouse into your desired position.
It may be easier to uncheck this box, move the mouse into position, and
press the F1 to F8 keys to copy or log the mouse position. You can look
at the Copy and Log menus at the top of the MouseInfo window to find out
which key maps to which buttons.

For example, uncheck the 3 Sec. Button Delay, then move the mouse around
the screen while pressing the F6 button, and notice how the x- and
y-coordinates of the mouse are recorded in the large text field in the
middle of the window. You can later use these coordinates in your
PyAutoGUI scripts.

For more information on MouseInfo, review the complete documentation at
*[https://mouseinfo.readthedocs.io/](https://mouseinfo.readthedocs.io/){.calibre6}*.

### **Working with the Screen** {#calibre_link-648 .h1}

Your GUI automation programs don't have to click and type blindly.
PyAutoGUI has screenshot features that can create an image file based on
the current contents of the screen. These functions can also return a
Pillow [Image]{.literal} object of the current screen's appearance. If
you've been skipping around in this book, you'll want to read [Chapter
17](#calibre_link-530){.calibre6} and install the [pillow]{.literal}
module before continuing with this section.

On Linux computers, the [scrot]{.literal} program needs to be installed
to use the screenshot functions in PyAutoGUI. In a Terminal window, run
**sudo apt-get install scrot** to install this program. If you're on
Windows or macOS, skip this step and continue with the section.

#### ***Getting a Screenshot*** {#calibre_link-649 .h2}

To take screenshots in Python, call the
[pyautogui.screenshot()]{.literal} function. Enter the following into
the interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [im = pyautogui.screenshot()]{.codestrong1}

The [im]{.literal} variable will contain the [Image]{.literal} object of
the screenshot. You can now call methods on the [Image]{.literal} object
in the [im]{.literal} variable, just like any other [Image]{.literal}
object. [Chapter 19](#calibre_link-608){.calibre6} has more information
about [Image]{.literal} objects.

#### []{#calibre_link-1062 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Analyzing the Screenshot*** {#calibre_link-650 .h2}

Say that one of the steps in your GUI automation program is to click a
gray button. Before calling the [click()]{.literal} method, you could
take a screenshot and look at the pixel where the script is about to
click. If it's not the same gray as the gray button, then your program
knows something is wrong. Maybe the window moved unexpectedly, or maybe
a pop-up dialog has blocked the button. At this point, instead of
continuing---and possibly wreaking havoc by clicking the wrong
thing---your program can "see" that it isn't clicking the right thing
and stop itself.

You can obtain the RGB color value of a particular pixel on the screen
with the [pixel()]{.literal} function. Enter the following into the
interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [pyautogui.pixel((0, 0))]{.codestrong1}\
(176, 176, 175)\
\>\>\> [pyautogui.pixel((50, 200))]{.codestrong1}\
(130, 135, 144)

Pass [pixel()]{.literal} a tuple of coordinates, like (0, 0) or (50,
200), and it'll tell you the color of the pixel at those coordinates in
your image. The return value from [pixel()]{.literal} is an RGB tuple of
three integers for the amount of red, green, and blue in the pixel.
(There is no fourth value for alpha, because screenshot images are fully
opaque.)

PyAutoGUI's [pixelMatchesColor()]{.literal} function will return
[True]{.literal} if the pixel at the given x- and y-coordinates on the
screen matches the given color. The first and second arguments are
integers for the x- and y-coordinates, and the third argument is a tuple
of three integers for the RGB color the screen pixel must match. Enter
the following into the interactive shell:

   \>\>\> [import pyautogui]{.codestrong1}\
[?]{.ent} \>\>\> [pyautogui.pixel((50, 200))]{.codestrong1}\
   (130, 135, 144)\
[?]{.ent} \>\>\> [pyautogui.pixelMatchesColor(50, 200, (130, 135,
144))]{.codestrong1}\
   True\
[?]{.ent} \>\>\> [pyautogui.pixelMatchesColor(50, 200, (255, 135,
144))]{.codestrong1}\
   False

After using [pixel()]{.literal} to get an RGB tuple for the color of a
pixel at specific coordinates [?]{.ent}, pass the same coordinates and
RGB tuple to [pixelMatchesColor()]{.literal} [?]{.ent}, which should
return [True]{.literal}. Then change a value in the RGB tuple and call
[pixelMatchesColor()]{.literal} again for the same coordinates
[?]{.ent}. This should return [false]{.literal}. This method can be
useful to call whenever your GUI automation programs are about to call
[click()]{.literal}. Note that the color at the given coordinates must
*exactly* match. If it is even slightly different---for example, [(255,
255, 254)]{.literal} instead of [(255, 255, 255)]{.literal}---then
[pixelMatchesColor()]{.literal} will return [False]{.literal}.

### []{#calibre_link-993 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Image Recognition** {#calibre_link-651 .h1}

But what if you do not know beforehand where PyAutoGUI should click? You
can use image recognition instead. Give PyAutoGUI an image of what you
want to click, and let it figure out the coordinates.

For example, if you have previously taken a screenshot to capture the
image of a Submit button in *submit.png*, the
[locateOnScreen()]{.literal} function will return the coordinates where
that image is found. To see how [locateOnScreen()]{.literal} works, try
taking a screenshot of a small area on your screen; then save the image
and enter the following into the interactive shell, replacing
[\'submit.png\']{.literal} with the filename of your screenshot:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [b = pyautogui.locateOnScreen(\'submit.png\')]{.codestrong1}\
\>\>\> [b]{.codestrong1}\
Box(left=643, top=745, width=70, height=29)\
\>\>\> [b\[0\]]{.codestrong1}\
643\
\>\>\> [b.left]{.codestrong1}\
643

The [Box]{.literal} object is a named tuple that
[locateOnScreen()]{.literal} returns and has the x-coordinate of the
left edge, the y-coordinate of the top edge, the width, and the height
for the first place on the screen the image was found. If you're trying
this on your computer with your own screenshot, your return value will
be different from the one shown here.

If the image cannot be found on the screen, [locateOnScreen()]{.literal}
returns [None]{.literal}. Note that the image on the screen must match
the provided image perfectly in order to be recognized. If the image is
even a pixel off, [locateOnScreen()]{.literal} raises an
[ImageNotFoundException]{.literal} exception. If you've changed your
screen resolution, images from previous screenshots might not match the
images on your current screen. You can change the scaling in the display
settings of your operating system, as shown in [Figure
20-4](#calibre_link-1631){.calibre6}.

::: image1
[]{#calibre_link-1631
.calibre6}![image](../images/000113.jpg){.calibre3}
:::

*Figure 20-4: The scale display settings in Windows 10 (left) and macOS
(right)*

If the image can be found in several places on the screen,
[locateAllOnScreen()]{.literal} will return a [Generator]{.literal}
object. Generators are beyond the scope of this book,
[]{#calibre_link-1874 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}but you can pass them to [list()]{.literal} to
return a list of four-integer tuples. There will be one four-integer
tuple for each location where the image is found on the screen. Continue
the interactive shell example by entering the following (and replacing
[\'submit.png\']{.literal} with your own image filename):

\>\>\>
[list(pyautogui.locateAllOnScreen(\'submit.png\'))]{.codestrong1}\
\[(643, 745, 70, 29), (1007, 801, 70, 29)\]

Each of the four-integer tuples represents an area on the screen. In the
example above, the image appears in two locations. If your image is only
found in one area, then using [list()]{.literal} and
[locateAllOnScreen()]{.literal} returns a list containing just one
tuple.

Once you have the four-integer tuple for the specific image you want to
select, you can click the center of this area by passing the tuple to
[click()]{.literal}. Enter the following into the interactive shell:

\>\>\> [pyautogui.click((643, 745, 70, 29))]{.codestrong1}

As a shortcut, you can also pass the image filename directly to the
[click()]{.literal} function:

\>\>\> [pyautogui.click(\'submit.png\')]{.codestrong1}

The [moveTo()]{.literal} and [dragTo()]{.literal} functions also accept
image filename arguments. Remember [locateOnScreen()]{.literal} raises
an exception if it can't find the image on the screen, so you should
call it from inside a [try]{.literal} statement:

try:\
    location = pyautogui.locateOnScreen(\'submit.png\')\
except:\
    print(\'Image could not be found.\')

Without the [try]{.literal} and [except]{.literal} statements, the
uncaught exception would crash your program. Since you can't be sure
that your program will always find the image, it's a good idea to use
the [try]{.literal} and [except]{.literal} statements when calling
[locateOnScreen()]{.literal}.

### **Getting Window Information** {#calibre_link-652 .h1}

Image recognition is a fragile way to find things on the screen; if a
single pixel is a different color, then
[pyautogui.locateOnScreen()]{.literal} won't find the image. If you need
to find where a particular window is on the screen, it's faster and more
reliable to use PyAutoGUI's window features.

::: note
**[NOTE]{.notes}**

*As of version 0.9.46, PyAutoGUI's window features work only on Windows,
not on macOS or Linux. These features come from PyAutoGUI's inclusion of
the PyGetWindow module.*
:::

#### []{#calibre_link-1875 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Obtaining the Active Window*** {#calibre_link-653 .h2}

The active window on your screen is the window currently in the
foreground and accepting keyboard input. If you're currently writing
code in the Mu Editor, the Mu Editor's window is the active window. Of
all the windows on your screen, only one will be active at a time.

In the interactive shell, call the
[pyautogui.getActiveWindow()]{.literal} function to get a
[Window]{.literal} object (technically a [Win32Window]{.literal} object
when run on Windows).

Once you have that [Window]{.literal} object, you can retrieve any of
the object's attributes, which describe its size, position, and title:

[left, right, top, bottom]{.codestrong} A single integer for the x- or
y-coordinate of the window's side

[topleft, topright, bottomleft, bottomright]{.codestrong} A named tuple
of two integers for the (x, y) coordinates of the window's corner

[midleft, midright, midleft, midright]{.codestrong} A named tuple of two
integers for the (x, y) coordinate of the middle of the window's side

[width, height]{.codestrong} A single integer for one of the window's
dimensions, in pixels

[size]{.codestrong} A named tuple of two integers for the (width,
height) of the window

[area]{.codestrong} A single integer representing the area of the
window, in pixels

[center]{.codestrong} A named tuple of two integers for the (x, y)
coordinate of the window's center

[centerx, centery]{.codestrong} A single integer for the x- or
y-coordinate of the window's center

[box]{.codestrong} A named tuple of four integers for the (left, top,
width, height) measurements of the window

[title]{.codestrong} A string of the text in the title bar at the top of
the window

To get the window's position, size, and title information from the
[window]{.literal} object, for example, enter the following into the
interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [fw = pyautogui.getActiveWindow()]{.codestrong1}\
\>\>\> [fw]{.codestrong1}\
Win32Window(hWnd=2034368)\
\>\>\> [str(fw)]{.codestrong1}\
\'\<Win32Window left=\"500\", top=\"300\", width=\"2070\",
height=\"1208\", title=\"Mu 1.0.1 -- test1.py\"\>\'\
\>\>\> [fw.title]{.codestrong1}\
\'Mu 1.0.1 -- test1.py\'\
\>\>\> [fw.size]{.codestrong1}\
(2070, 1208)\
\>\>\> [fw.left, fw.top, fw.right, fw.bottom]{.codestrong1}\
(500, 300, 2070, 1208)\
\>\>\> [fw.topleft]{.codestrong1}\
(256, 144)\
[]{#calibre_link-981 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [fw.area]{.codestrong1}\
2500560\
\>\>\> [pyautogui.click(fw.left + 10, fw.top + 20)]{.codestrong1}

You can now use these attributes to calculate precise coordinates within
a window. If you know that a button you want to click is always 10
pixels to the right of and 20 pixels down from the window's top-left
corner, and the window's top-left corner is at screen coordinates (300,
500), then calling [pyautogui.click(310, 520)]{.literal} (or
[pyautogui.click(fw.left + 10, fw.top + 20)]{.literal} if [fw]{.literal}
contains the [Window]{.literal} object for the window) will click the
button. This way, you won't have to rely on the slower, less reliable
[locateOnScreen()]{.literal} function to find the button for you.

#### ***Other Ways of Obtaining Windows*** {#calibre_link-654 .h2}

While [getActiveWindow()]{.literal} is useful for obtaining the window
that is active at the time of the function call, you'll need to use some
other function to obtain [Window]{.literal} objects for the other
windows on the screen.

The following four functions return a list of [Window]{.literal}
objects. If they're unable to find any windows, they return an empty
list:

[pyautogui.getAllWindows()]{.codestrong} Returns a list of
[Window]{.literal} objects for every visible window on the screen.

[pyautogui.getWindowsAt(x, y)]{.codestrong} Returns a list of
[Window]{.literal} objects for every visible window that includes the
point (x, y).

[pyautogui.getWindowsWithTitle(title)]{.codestrong} Returns a list of
[Window]{.literal} objects for every visible window that includes the
string [title]{.literal} in its title bar.

[pyautogui.getActiveWindow()]{.codestrong} Returns the
[Window]{.literal} object for the window that is currently receiving
keyboard focus.

PyAutoGUI also has a [pyautogui.getAllTitles()]{.literal} function,
which returns a list of strings of every visible window.

#### ***Manipulating Windows*** {#calibre_link-655 .h2}

Windows attributes can do more than just tell you the size and position
of the window. You can also set their values in order to resize or move
the window. For example, enter the following into the interactive shell:

   \>\>\> [import pyautogui]{.codestrong1}\
   \>\>\> [fw = pyautogui.getActiveWindow()]{.codestrong1}\
[?]{.ent} \>\>\> [fw.width]{.codestrong1} \# Gets the current width of
the window.\
   1669\
[?]{.ent} \>\>\> [fw.topleft]{.codestrong1} \# Gets the current position
of the window.\
   (174, 153)\
[?]{.ent} \>\>\> [fw.width = 1000]{.codestrong1} \# Resizes the width.\
[?]{.ent} \>\>\> [fw.topleft = (800, 400)]{.codestrong1} \# Moves the
window.

[]{#calibre_link-1876 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}First, we use the [Window]{.literal} object's
attributes to find out information about the window's size [?]{.ent} and
position [?]{.ent}. After calling these functions in Mu Editor, the
window should move [?]{.ent} and become narrower [?]{.ent}, as in
[Figure 20-5](#calibre_link-1632){.calibre6}.

::: image1
[]{#calibre_link-1632
.calibre6}![image](../images/000058.jpg){.calibre3}
:::

*Figure 20-5: The Mu Editor window before (top) and after (bottom) using
the [Window]{.literal1} object attributes to move and resize it*

You can also find out and change the window's minimized, maximized, and
activated states. Try entering the following into the interactive shell:

   \>\>\> [import pyautogui]{.codestrong1}\
   \>\>\> [fw = pyautogui.getActiveWindow()]{.codestrong1}\
[?]{.ent} \>\>\> [fw.isMaximized]{.codestrong1} \# Returns True if
window is maximized.\
   False\
[?]{.ent} \>\>\> [fw.isMinimized]{.codestrong1} \# Returns True if
window is minimized.\
   []{#calibre_link-775 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}False\
[?]{.ent} \>\>\> [fw.isActive]{.codestrong1} \# Returns True if window
is the active window.\
   True\
[?]{.ent} \>\>\> [fw.maximize()]{.codestrong1} \# Maximizes the window.\
   \>\>\> [fw.isMaximized]{.codestrong1}\
   True\
[?]{.ent} \>\>\> [fw.restore()]{.codestrong1} \# Undoes a
minimize/maximize action.\
[?]{.ent} \>\>\> [fw.minimize()]{.codestrong1} \# Minimizes the window.\
   \>\>\> [import time]{.codestrong1}\
   \>\>\> \# Wait 5 seconds while you activate a different window:\
[?]{.ent} \>\>\> [time.sleep(5); fw.activate()]{.codestrong1}\
[?]{.ent} \>\>\> [fw.close()]{.codestrong1} \# This will close the
window you\'re typing in.

The [isMaximized]{.literal} [?]{.ent}, [isMinimized]{.literal}
[?]{.ent}, and [isActive]{.literal} [?]{.ent} attributes contain Boolean
values that indicate whether the window is currently in that state. The
[maximize()]{.literal} [?]{.ent}, [minimize()]{.literal} [?]{.ent},
[activate()]{.literal} [?]{.ent}, and [restore()]{.literal} [?]{.ent}
methods change the window's state. After you maximize or minimize the
window with [maximize()]{.literal} or [minimize()]{.literal}, the
[restore()]{.literal} method will restore the window to its former size
and position.

The [close()]{.literal} method [?]{.ent} will close a window. Be careful
with this method, as it may bypass any message dialogs asking you to
save your work before quitting the application.

The complete documentation for PyAutoGUI's window-controlling feature
can be found at
*[https://pyautogui.readthedocs.io/](https://pyautogui.readthedocs.io/){.calibre6}*.
You can also use these features separately from PyAutoGUI with the
PyGetWindow module, documented at
*[https://pygetwindow.readthedocs.io/](https://pygetwindow.readthedocs.io/){.calibre6}*.

### **Controlling the Keyboard** {#calibre_link-656 .h1}

PyAutoGUI also has functions for sending virtual keypresses to your
computer, which enables you to fill out forms or enter text into
applications.

#### ***Sending a String from the Keyboard*** {#calibre_link-657 .h2}

The [pyautogui.write()]{.literal} function sends virtual keypresses to
the computer. What these keypresses do depends on what window is active
and what text field has focus. You may want to first send a mouse click
to the text field you want in order to ensure that it has focus.

As a simple example, let's use Python to automatically type the words
*Hello, world!* into a file editor window. First, open a new file editor
window and position it in the upper-left corner of your screen so that
PyAutoGUI will click in the right place to bring it into focus. Next,
enter the following into the interactive shell:

\>\>\> [pyautogui.click(100, 200); pyautogui.write(\'Hello,
world!\')]{.codestrong1}

Notice how placing two commands on the same line, separated by a
semicolon, keeps the interactive shell from prompting you for input
between running the two instructions. This prevents you from
[]{#calibre_link-988 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}accidentally bringing a new window into focus
between the [click()]{.literal} and [write()]{.literal} calls, which
would mess up the example.

Python will first send a virtual mouse click to the coordinates (100,
200), which should click the file editor window and put it in focus. The
[write()]{.literal} call will send the text *Hello, world!* to the
window, making it look like [Figure
20-6](#calibre_link-1633){.calibre6}. You now have code that can type
for you!

::: image1
[]{#calibre_link-1633
.calibre6}![image](../images/000001.jpg){.calibre3}
:::

*Figure 20-6: Using PyAutogGUI to click the file editor window and type*
Hello, world! *into it*

By default, the [write()]{.literal} function will type the full string
instantly. However, you can pass an optional second argument to add a
short pause between each character. This second argument is an integer
or float value of the number of seconds to pause. For example,
[pyautogui.write(\'Hello, world!\', 0.25)]{.literal} will wait a
quarter-second after typing *H*, another quarter-second after *e*, and
so on. This gradual typewriter effect may be useful for slower
applications that can't process keystrokes fast enough to keep up with
PyAutoGUI.

For characters such as *A* or *!*, PyAutoGUI will automatically simulate
holding down the [SHIFT]{.small} key as well.

#### ***Key Names*** {#calibre_link-658 .h2}

Not all keys are easy to represent with single text characters. For
example, how do you represent [SHIFT]{.small} or the left arrow key as a
single character? In PyAutoGUI, these keyboard keys are represented by
short string values instead: [\'esc\']{.literal} for the [ESC]{.small}
key or [\'enter\']{.literal} for the [ENTER]{.small} key.

Instead of a single string argument, a list of these keyboard key
strings can be passed to [write()]{.literal}. For example, the following
call presses the A key, then the B key, then the left arrow key twice,
and finally the X and Y keys:

\>\>\> [pyautogui.write(\[\'a\', \'b\', \'left\', \'left\', \'X\',
\'Y\'\])]{.codestrong1}

[]{#calibre_link-989 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Because pressing the left arrow key moves the
keyboard cursor, this will output *XYab*. [Table
20-1](#calibre_link-1634){.calibre6} lists the PyAutoGUI keyboard key
strings that you can pass to [write()]{.literal} to simulate pressing
any combination of keys.

You can also examine the [pyautogui.KEYBOARD_KEYS]{.literal} list to see
all possible keyboard key strings that PyAutoGUI will accept. The
[\'shift\']{.literal} string refers to the left [SHIFT]{.small} key and
is equivalent to [\'shiftleft\']{.literal}. The same applies for
[\'ctrl\']{.literal}, [\'alt\']{.literal}, and [\'win\']{.literal}
strings; they all refer to the left-side key.

**Table 20-1:** [PyKeyboard]{.literal1} Attributes

  **Keyboard key string**                                                                                                                                                                                                                         **Meaning**
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [\'a\']{.literal}, [\'b\']{.literal}, [\'c\']{.literal}, [\'A\']{.literal}, [\'B\']{.literal}, [\'C\']{.literal}, [\'1\']{.literal}, [\'2\']{.literal}, [\'3\']{.literal}, [\'!\']{.literal}, [\'@\']{.literal}, [\'#\']{.literal}, and so on   The keys for single characters
  [\'enter\']{.literal} (or [\'return\']{.literal} or [\'\\n\']{.literal})                                                                                                                                                                        The [ENTER]{.small} key
  [\'esc\']{.literal}                                                                                                                                                                                                                             The [ESC]{.small} key
  [\'shiftleft\']{.literal}, [\'shiftright\']{.literal}                                                                                                                                                                                           The left and right [SHIFT]{.small} keys
  [\'altleft\']{.literal}, [\'altright\']{.literal}                                                                                                                                                                                               The left and right [ALT]{.small} keys
  [\'ctrlleft\']{.literal}, [\'ctrlright\']{.literal}                                                                                                                                                                                             The left and right [CTRL]{.small} keys
  [\'tab\']{.literal} (or [\'\\t\']{.literal})                                                                                                                                                                                                    The [TAB]{.small} key
  [\'backspace\']{.literal}, [\'delete\']{.literal}                                                                                                                                                                                               The [BACKSPACE]{.small} and [DELETE]{.small} keys
  [\'pageup\']{.literal}, [\'pagedown\']{.literal}                                                                                                                                                                                                The [PAGE UP]{.small} and [PAGE DOWN]{.small} keys
  [\'home\']{.literal}, [\'end\']{.literal}                                                                                                                                                                                                       The [HOME]{.small} and [END]{.small} keys
  [\'up\']{.literal}, [\'down\']{.literal}, [\'left\']{.literal}, [\'right\']{.literal}                                                                                                                                                           The up, down, left, and right arrow keys
  [\'f1\']{.literal}, [\'f2\']{.literal}, [\'f3\']{.literal}, and so on                                                                                                                                                                           The F1 to F12 keys
  [\'volumemute\']{.literal}, [\'volumedown\']{.literal}, [\'volumeup\']{.literal}                                                                                                                                                                The mute, volume down, and volume up keys (some keyboards do not have these keys, but your operating system will still be able to understand these simulated keypresses)
  [\'pause\']{.literal}                                                                                                                                                                                                                           The [PAUSE]{.small} key
  [\'capslock\']{.literal}, [\'numlock\']{.literal}, [\'scrolllock\']{.literal}                                                                                                                                                                   The [CAPS LOCK]{.small}, [NUM LOCK]{.small}, and [SCROLL LOCK]{.small} keys
  [\'insert\']{.literal}                                                                                                                                                                                                                          The [INS]{.small} or [INSERT]{.small} key
  [\'printscreen\']{.literal}                                                                                                                                                                                                                     The [PRTSC]{.small} or [PRINT SCREEN]{.small} key
  [\'winleft\']{.literal}, [\'winright\']{.literal}                                                                                                                                                                                               The left and right [WIN]{.small} keys (on Windows)
  [\'command\']{.literal}                                                                                                                                                                                                                         The Command (![image](../images/000064.jpg){.calibre3}) key (on macOS)
  [\'option\']{.literal}                                                                                                                                                                                                                          The [OPTION]{.small} key (on macOS)

#### ***Pressing and Releasing the Keyboard*** {#calibre_link-659 .h2}

Much like the [mouseDown()]{.literal} and [mouseUp()]{.literal}
functions, [pyautogui.keyDown()]{.literal} and
[pyautogui.keyUp()]{.literal} will send virtual keypresses and releases
to the computer. They are passed a keyboard key string (see [Table
20-1](#calibre_link-1634){.calibre6}) for their argument. For
convenience, PyAutoGUI provides the [pyautogui.press()]{.literal}
function, which calls both of these functions to simulate a complete
keypress.

[]{#calibre_link-987 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Run the following code, which will type a dollar
sign character (obtained by holding the [SHIFT]{.small} key and pressing
4):

\>\>\> [pyautogui.keyDown(\'shift\'); pyautogui.press(\'4\');
pyautogui.keyUp(\'shift\')]{.codestrong1}

This line presses down [SHIFT]{.small}, presses (and releases) 4, and
then releases [SHIFT]{.small}. If you need to type a string into a text
field, the [write()]{.literal} function is more suitable. But for
applications that take single-key commands, the [press()]{.literal}
function is the simpler approach.

#### ***Hotkey Combinations*** {#calibre_link-660 .h2}

A *hotkey* or *shortcut* is a combination of keypresses to invoke some
application function. The common hotkey for copying a selection is
[CTRL]{.small}-C (on Windows and Linux) or
![image](../images/000064.jpg){.calibre3}-C (on macOS). The user presses
and holds the [CTRL]{.small} key, then presses the C key, and then
releases the C and [CTRL]{.small} keys. To do this with PyAutoGUI's
[keyDown()]{.literal} and [keyUp()]{.literal} functions, you would have
to enter the following:

pyautogui.keyDown(\'ctrl\')\
pyautogui.keyDown(\'c\')\
pyautogui.keyUp(\'c\')\
pyautogui.keyUp(\'ctrl\')

This is rather complicated. Instead, use the
[pyautogui.hotkey()]{.literal} function, which takes multiple keyboard
key string arguments, presses them in order, and releases them in the
reverse order. For the [CTRL]{.small}-C example, the code would simply
be as follows:

pyautogui.hotkey(\'ctrl\', \'c\')

This function is especially useful for larger hotkey combinations. In
Word, the [CTRL]{.small}-[ALT]{.small}-[SHIFT]{.small}-S hotkey
combination displays the Style pane. Instead of making eight different
function calls (four [keyDown()]{.literal} calls and four
[keyUp()]{.literal} calls), you can just call [hotkey(\'ctrl\', \'alt\',
\'shift\', \'s\')]{.literal}.

### **Setting Up Your GUI Automation Scripts** {#calibre_link-661 .h1}

GUI automation scripts are a great way to automate the boring stuff, but
your scripts can also be finicky. If a window is in the wrong place on a
desktop or some pop-up appears unexpectedly, your script could be
clicking on the wrong things on the screen. Here are some tips for
setting up your GUI automation scripts:

-   Use the same screen resolution each time you run the script so that
    the position of windows doesn't change.
-   The application window that your script clicks should be maximized
    so that its buttons and menus are in the same place each time you
    run the script.
-   []{#calibre_link-872 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Add generous pauses while waiting for content
    to load; you don't want your script to begin clicking before the
    application is ready.
-   Use [locateOnScreen()]{.literal} to find buttons and menus to click,
    rather than relying on XY coordinates. If your script can't find the
    thing it needs to click, stop the program rather than let it
    continue blindly clicking.
-   Use [getWindowsWithTitle()]{.literal} to ensure that the application
    window you think your script is clicking on exists, and use the
    [activate()]{.literal} method to put that window in the foreground.
-   Use the [logging]{.literal} module from [Chapter
    11](#calibre_link-349){.calibre6} to keep a log file of what your
    script has done. This way, if you have to stop your script halfway
    through a process, you can change it to pick up from where it left
    off.
-   Add as many checks as you can to your script. Think about how it
    could fail if an unexpected pop-up window appears or if your
    computer loses its internet connection.
-   You may want to supervise the script when it first begins to ensure
    that it's working correctly.

You might also want to put a pause at the start of your script so the
user can set up the window the script will click on. PyAutoGUI has a
[sleep()]{.literal} function that acts identically to
[time.sleep()]{.literal} (it just frees you from having to also add
[import time]{.literal} to your scripts). There is also a
[countdown()]{.literal} function that prints numbers counting down to
give the user a visual indication that the script will continue soon.
Enter the following into the interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [pyautogui.sleep(3)]{.codestrong1} \# Pauses the program for 3
seconds.\
\>\>\> [pyautogui.countdown(10)]{.codestrong1} \# Counts down over 10
seconds.\
10 9 8 7 6 5 4 3 2 1\
\>\>\> [print(\'Starting in \', end=\'\');
pyautogui.countdown(3)]{.codestrong1}\
Starting in 3 2 1

These tips can help make your GUI automation scripts easier to use and
more able to recover from unforeseen circumstances.

### **Review of the PyAutoGUI Functions** {#calibre_link-662 .h1}

Since this chapter covered many different functions, here is a quick
summary reference:

[moveTo(]{.codestrong}[x]{.codestrongitalic}[,]{.codestrong}
[y]{.codestrongitalic}[)]{.codestrong} Moves the mouse cursor to the
given [x]{.codeitalic} and [y]{.codeitalic} coordinates.

[move(]{.codestrong}[xOffset]{.codestrongitalic}[,]{.codestrong}
[yOffset]{.codestrongitalic}[)]{.codestrong} Moves the mouse cursor
relative to its current position.

[dragTo(]{.codestrong}[x]{.codestrongitalic}[,]{.codestrong}
[y]{.codestrongitalic}[)]{.codestrong} Moves the mouse cursor while the
left button is held down.

[drag(]{.codestrong}[xOffset]{.codestrongitalic}[,]{.codestrong}
[yOffset]{.codestrongitalic}[)]{.codestrong} Moves the mouse cursor
relative to its current position while the left button is held down.

[click(]{.codestrong}[x]{.codestrongitalic}[,]{.codestrong}
[y]{.codestrongitalic}[,]{.codestrong}
[button]{.codestrongitalic}[)]{.codestrong} Simulates a click (left
button by default).

[]{#calibre_link-1071 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[rightClick()]{.codestrong} Simulates a
right-button click.

[middleClick()]{.codestrong} Simulates a middle-button click.

[doubleClick()]{.codestrong} Simulates a double left-button click.

[mouseDown(]{.codestrong}[x]{.codestrongitalic}[,]{.codestrong}
[y]{.codestrongitalic}[,]{.codestrong}
[button]{.codestrongitalic}[)]{.codestrong} Simulates pressing down the
given button at the position [x]{.codeitalic}, [y]{.codeitalic}.

[mouseUp(]{.codestrong}[x]{.codestrongitalic}[,]{.codestrong}
[y]{.codestrongitalic}[,]{.codestrong}
[button]{.codestrongitalic}[)]{.codestrong} Simulates releasing the
given button at the position [x]{.codeitalic}, [y]{.codeitalic}.

[scroll(]{.codestrong}[units]{.codestrongitalic}[)]{.codestrong}
Simulates the scroll wheel. A positive argument scrolls up; a negative
argument scrolls down.

[write(]{.codestrong}[message]{.codestrongitalic}[)]{.codestrong} Types
the characters in the given message string.

[write(\[]{.codestrong}[key1]{.codestrongitalic}[,]{.codestrong}
[key2]{.codestrongitalic}[,]{.codestrong}
[key3]{.codestrongitalic}[\])]{.codestrong} Types the given keyboard key
strings.

[press(]{.codestrong}[key]{.codestrongitalic}[)]{.codestrong} Presses
the given keyboard key string.

[keyDown(]{.codestrong}[key]{.codestrongitalic}[)]{.codestrong}
Simulates pressing down the given keyboard key.

[keyUp(]{.codestrong}[key]{.codestrongitalic}[)]{.codestrong} Simulates
releasing the given keyboard key.

[hotkey(\[]{.codestrong}[key1]{.codestrongitalic}[,]{.codestrong}
[key2]{.codestrongitalic}[,]{.codestrong}
[key3]{.codestrongitalic}[\])]{.codestrong} Simulates pressing the given
keyboard key strings down in order and then releasing them in reverse
order.

[screenshot()]{.codestrong} Returns a screenshot as an [Image]{.literal}
object. (See [Chapter 19](#calibre_link-608){.calibre6} for information
on [Image]{.literal} objects.)

[getActiveWindow(), getAllWindows(), getWindowsAt()]{.codestrong},
**and** [getWindowsWithTitle()]{.codestrong} These functions return
Window objects that can resize and reposition application windows on the
desktop.

[getAllTitles()]{.codestrong} Returns a list of strings of the title bar
text of every window on the desktop.

::: sidebar
**CAPTCHAS AND COMPUTER ETHICS**

"Completely Automated Public Turing test to tell Computers and Humans
Apart" or "captchas" are those small tests that ask you to type the
letters in a distorted picture or click on photos of fire hydrants.
These are tests that are easy, if annoying, for humans to pass but
nearly impossible for software to solve. After reading this chapter, you
can see how easy it is to write a script that could, say, sign up for
billions of free email accounts or flood users with harassing messages.
Captchas mitigate this by requiring a step that only a human can pass.

However not all websites implement captchas, and these can be vulnerable
to abuse by unethical programmers. Learning to code is a powerful and
exciting skill, and you may be tempted to misuse this power for personal
gain or even just to show off. But just as an unlocked door isn't
justification for trespass, the responsibility for your programs falls
upon you, the programmer. There is nothing clever about circumventing
systems to cause harm, invade privacy, or gain unfair advantage. I hope
that my efforts in writing this book enable you to become your most
productive self, rather than a mercenary one.
:::

### []{#calibre_link-804 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Project: Automatic Form Filler** {#calibre_link-663 .h1}

Of all the boring tasks, filling out forms is the most dreaded of
chores. It's only fitting that now, in the final chapter project, you
will slay it. Say you have a huge amount of data in a spreadsheet, and
you have to tediously retype it into some other application's form
interface---with no intern to do it for you. Although some applications
will have an Import feature that will allow you to upload a spreadsheet
with the information, sometimes it seems that there is no other way than
mindlessly clicking and typing for hours on end. You've come this far in
this book; you know that *of course* must be a way to automate this
boring task.

The form for this project is a Google Docs form that you can find at
*[https://autbor.com/form](https://autbor.com/form){.calibre6}*. It
looks like [Figure 20-7](#calibre_link-1635){.calibre6}.

::: image1
[]{#calibre_link-1635
.calibre6}![image](../images/000096.jpg){.calibre3}
:::

*Figure 20-7: The form used for this project*

At a high level, here's what your program should do:

1.  []{#calibre_link-1877 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Click the first text field of the form.
2.  Move through the form, typing information into each field.
3.  Click the Submit button.
4.  Repeat the process with the next set of data.

This means your code will need to do the following:

1.  Call [pyautogui.click()]{.literal} to click the form and Submit
    button.
2.  Call [pyautogui.write()]{.literal} to enter text into the fields.
3.  Handle the [KeyboardInterrupt]{.literal} exception so the user can
    press [CTRL]{.small}-C to quit.

Open a new file editor window and save it as *formFiller.py*.

#### ***Step 1: Figure Out the Steps*** {#calibre_link-664 .h2}

Before writing code, you need to figure out the exact keystrokes and
mouse clicks that will fill out the form once. The application launched
by calling [pyautogui.mouseInfo()]{.literal} can help you figure out
specific mouse coordinates. You need to know only the coordinates of the
first text field. After clicking the first field, you can just press
[TAB]{.small} to move focus to the next field. This will save you from
having to figure out the x- and y-coordinates to click for every field.

Here are the steps for entering data into the form:

1.  Put the keyboard focus on the Name field so that pressing keys types
    text into the field.
2.  Type a name and then press [TAB]{.small}.
3.  Type a greatest fear and then press [TAB]{.small}.
4.  Press the down arrow key the correct number of times to select the
    wizard power source: once for *wand*, twice for *amulet*, three
    times for *crystal ball*, and four times for *money*. Then press
    [TAB]{.small}. (Note that on macOS, you will have to press the down
    arrow key one more time for each option. For some browsers, you may
    need to press [ENTER]{.small} as well.)
5.  Press the right arrow key to select the answer to the RoboCop
    question. Press it once for *2*, twice for *3*, three times for *4*,
    or four times for *5* or just press the spacebar to select *1*
    (which is highlighted by default). Then press [TAB]{.small}.
6.  Type an additional comment and then press [TAB]{.small}.
7.  Press [ENTER]{.small} to "click" the Submit button.
8.  After submitting the form, the browser will take you to a page where
    you will need to follow a link to return to the form page.

[]{#calibre_link-1878 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Different browsers on different operating systems
might work slightly differently from the steps given here, so check that
these keystroke combinations work for your computer before running your
program.

#### ***Step 2: Set Up Coordinates*** {#calibre_link-665 .h2}

Load the example form you downloaded ([Figure
20-7](#calibre_link-1635){.calibre6}) in a browser by going to
*[https://autbor.com/form](https://autbor.com/form){.calibre6}*.

Make your source code look like the following:

#! python3\
\# formFiller.py - Automatically fills in the form.\
\
import pyautogui, time\
\
\# TODO: Give the user a chance to kill the script.\
\
\# TODO: Wait until the form page has loaded.\
\
\# TODO: Fill out the Name Field.\
\
\# TODO: Fill out the Greatest Fear(s) field.\
\
\# TODO: Fill out the Source of Wizard Powers field.\
\
\# TODO: Fill out the RoboCop field.\
\
\# TODO: Fill out the Additional Comments field.\
\
\# TODO: Click Submit.\
\
\# TODO: Wait until form page has loaded.\
\
\# TODO: Click the Submit another response link.

Now you need the data you actually want to enter into this form. In the
real world, this data might come from a spreadsheet, a plaintext file,
or a website, and it would require additional code to load into the
program. But for this project, you'll just hardcode all this data in a
variable. Add the following to your program:

#! python3\
\# formFiller.py - Automatically fills in the form.\
\
\--[snip]{.codeitalic1}\--\
\
[formData = \[{\'name\': \'Alice\', \'fear\': \'eavesdroppers\',
\'source\': \'wand\',]{.codestrong1}\
[            \'robocop\': 4, \'comments\': \'Tell Bob I said
hi.\'},]{.codestrong1}\
[            {\'name\': \'Bob\', \'fear\': \'bees\', \'source\':
\'amulet\', \'robocop\': 4,]{.codestrong1}\
[            \'comments\': \'n/a\'},]{.codestrong1}\
          [  {\'name\': \'Carol\', \'fear\': \'puppets\', \'source\':
\'crystal ball\',]{.codestrong1}\
[            \'robocop\': 1, \'comments\': \'Please take the puppets out
of the]{.codestrong1}\
[            break room.\'},]{.codestrong1}\
[]{#calibre_link-1054 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[            {\'name\': \'Alex Murphy\', \'fear\':
\'ED-209\', \'source\': \'money\',]{.codestrong1}\
[            \'robocop\': 5, \'comments\': \'Protect the innocent. Serve
the public]{.codestrong1}\
[            trust. Uphold the law.\'},]{.codestrong1}\
[           \]]{.codestrong1}\
\
\--[snip]{.codeitalic1}\--

The [formData]{.literal} list contains four dictionaries for four
different names. Each dictionary has names of text fields as keys and
responses as values. The last bit of setup is to set PyAutoGUI's
[PAUSE]{.literal} variable to wait half a second after each function
call. Also, remind the user to click on the browser to make it the
active window. Add the following to your program after the
[formData]{.literal} assignment statement:

pyautogui.PAUSE = 0.5\
print(\'Ensure that the browser window is active and the form is
loaded!\')

#### ***Step 3: Start Typing Data*** {#calibre_link-666 .h2}

A [for]{.literal} loop will iterate over each of the dictionaries in the
[formData]{.literal} list, passing the values in the dictionary to the
PyAutoGUI functions that will virtually type in the text fields.

Add the following code to your program:

#! python3\
\# formFiller.py - Automatically fills in the form.\
\
\--[snip]{.codeitalic1}\--\
\
[for person in formData:]{.codestrong1}\
[    # Give the user a chance to kill the script.]{.codestrong1}\
[    print(\'\>\>\> 5-SECOND PAUSE TO LET USER PRESS CTRL-C
\<\<\<\')]{.codestrong1}\
[  ]{.codestrong1}[?]{.ent} [time.sleep(5)]{.codestrong1}\
\
\--[snip]{.codeitalic1}\--

As a small safety feature, the script has a five-second pause [?]{.ent}
that gives the user a chance to hit [CTRL]{.small}-C (or move the mouse
cursor to the upper-left corner of the screen to raise the
[FailSafeException]{.literal} exception) to shut the program down in
case it's doing something unexpected. After the code that waits to give
the page time to load, add the following:

#! python3\
\# formFiller.py - Automatically fills in the form.\
\
\--[snip]{.codeitalic1}\--\
\
[  ]{.codestrong1}[?]{.ent} [print(\'Entering %s info\...\' %
(person\[\'name\'\]))]{.codestrong1}\
[  ]{.codestrong1}[?]{.ent} [pyautogui.write(\[\'\\t\',
\'\\t\'\])]{.codestrong1}\
\
[     # Fill out the Name field.]{.codestrong1}\
[]{#calibre_link-1879 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[  ]{.codestrong1}[?]{.ent}
[pyautogui.write(person\[\'name\'\] + \'\\t\')]{.codestrong1}\
\
[     # Fill out the Greatest Fear(s) field.]{.codestrong1}\
[  ]{.codestrong1}[?]{.ent} [pyautogui.write(person\[\'fear\'\] +
\'\\t\')]{.codestrong1}\
\
\--[snip]{.codeitalic1}\--

We add an occasional [print()]{.literal} call to display the program's
status in its Terminal window to let the user know what's going on
[?]{.ent}.

Since the form has had time to load, call [pyautogui.write(\[\'\\t\',
\'\\t\'\])]{.literal} to press [TAB]{.small} twice and put the Name
field into focus [?]{.ent}. Then call [write()]{.literal} again to enter
the string in [person\[\'name\'\]]{.literal} [?]{.ent}. The
[\'\\t\']{.literal} character is added to the end of the string passed
to [write()]{.literal} to simulate pressing [TAB]{.small}, which moves
the keyboard focus to the next field, Greatest Fear(s). Another call to
[write()]{.literal} will type the string in
[person\[\'fear\'\]]{.literal} into this field and then tab to the next
field in the form [?]{.ent}.

#### ***Step 4: Handle Select Lists and Radio Buttons*** {#calibre_link-667 .h2}

The drop-down menu for the "wizard powers" question and the radio
buttons for the RoboCop field are trickier to handle than the text
fields. To click these options with the mouse, you would have to figure
out the x- and y-coordinates of each possible option. It's easier to use
the keyboard arrow keys to make a selection instead.

Add the following to your program:

#! python3\
\# formFiller.py - Automatically fills in the form.\
\
\--[snip]{.codeitalic1}\--\
\
     [\# Fill out the Source of Wizard Powers field.]{.codestrong1}\
[  ]{.codestrong1}[?]{.ent} [if person\[\'source\'\] ==
\'wand\':]{.codestrong1}\
[      ]{.codestrong1}[?]{.ent} [pyautogui.write(\[\'down\',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
     [elif person\[\'source\'\] == \'amulet\':]{.codestrong1}\
[         pyautogui.write(\[\'down\', \'down\', \'\\t\'\]]{.codestrong1}
[, 0.5)]{.codestrong1}\
[     elif person\[\'source\'\] == \'crystal ball\':]{.codestrong1}\
[         pyautogui.write(\[\'down\', \'down\', \'down\',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
[     elif person\[\'source\'\] == \'money\':]{.codestrong1}\
[         pyautogui.write(\[\'down\', \'down\', \'down\', \'down\',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
\
[     # Fill out the RoboCop field.]{.codestrong1}\
[  ]{.codestrong1}[?]{.ent} [if person\[\'robocop\'\] ==
1:]{.codestrong1}\
[      ]{.codestrong1}[?]{.ent} [pyautogui.write(\[\' \',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
[     elif person\[\'robocop\'\] == 2:]{.codestrong1}\
[         pyautogui.write(\[\'right\', \'\\t\'\]]{.codestrong1} [,
0.5)]{.codestrong1}\
[     elif person\[\'robocop\'\] == 3:]{.codestrong1}\
[         pyautogui.write(\[\'right\', \'right\',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
[     elif person\[\'robocop\'\] == 4:]{.codestrong1}\
[         pyautogui.write(\[\'right\', \'right\', \'right\',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
[     elif person\[\'robocop\'\] == 5:]{.codestrong1}\
[         pyautogui.write(\[\'right\', \'right\', \'right\', \'right\',
\'\\t\'\]]{.codestrong1} [, 0.5)]{.codestrong1}\
\
\--[snip]{.codeitalic1}\--

Once the drop-down menu has focus (remember that you wrote code to
simulate pressing [TAB]{.small} after filling out the Greatest Fear(s)
field), pressing the down arrow key will move to the next item in the
selection list. Depending on the value in
[person\[\'source\'\]]{.literal}, your program should send a
[]{#calibre_link-1880 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}number of down arrow keypresses before tabbing to
the next field. If the value at the [\'source\']{.literal} key in this
user's dictionary is [\'wand\']{.literal} [?]{.ent}, we simulate
pressing the down arrow key once (to select *Wand*) and pressing
[TAB]{.small} [?]{.ent}. If the value at the [\'source\']{.literal} key
is [\'amulet\']{.literal}, we simulate pressing the down arrow key twice
and pressing [TAB]{.small}, and so on for the other possible answers.
The [0.5]{.literal} argument in these [write()]{.literal} calls add a
half-second pause in between each key so that our program doesn't move
too fast for the form.

The radio buttons for the RoboCop question can be selected with the
right arrow keys---or, if you want to select the first choice [?]{.ent},
by just pressing the spacebar [?]{.ent}.

#### ***Step 5: Submit the Form and Wait*** {#calibre_link-668 .h2}

You can fill out the Additional Comments field with the
[write()]{.literal} function by passing
[person\[\'comments\'\]]{.literal} as an argument. You can type an
additional [\'\\t\']{.literal} to move the keyboard focus to the next
field or the Submit button. Once the Submit button is in focus, calling
[pyautogui.press(\'enter\')]{.literal} will simulate pressing the
[ENTER]{.small} key and submit the form. After submitting the form, your
program will wait five seconds for the next page to load.

Once the new page has loaded, it will have a *Submit another response*
link that will direct the browser to a new, empty form page. You stored
the coordinates of this link as a tuple in [submitAnotherLink]{.literal}
in step 2, so pass these coordinates to [pyautogui.click()]{.literal} to
click this link.

With the new form ready to go, the script's outer [for]{.literal} loop
can continue to the next iteration and enter the next person's
information into the form.

Complete your program by adding the following code:

#! python3\
\# formFiller.py - Automatically fills in the form.\
\
\--[snip]{.codeitalic1}\--\
\
    [\# Fill out the Additional Comments field.]{.codestrong1}\
[    pyautogui.write(person\[\'comments\'\] + \'\\t\')]{.codestrong1}\
\
[    # \"Click\" Submit button by pressing Enter.]{.codestrong1}\
[    time.sleep(0.5) \# Wait for the button to activate.]{.codestrong1}\
[    pyautogui.press(\'enter\')]{.codestrong1}\
\
[    # Wait until form page has loaded.]{.codestrong1}\
[    print(\'Submitted form.\')]{.codestrong1}\
[    time.sleep(5)]{.codestrong1}\
\
[    # Click the Submit another response link.]{.codestrong1}\
[    pyautogui.click(submitAnotherLink\[0\],
submitAnotherLink\[1\])]{.codestrong1}

[]{#calibre_link-805 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Once the main [for]{.literal} loop has finished,
the program will have plugged in the information for each person. In
this example, there are only four people to enter. But if you had
*4,000* people, then writing a program to do this would save you a lot
of time and typing!

### **Displaying Message Boxes** {#calibre_link-669 .h1}

The programs you've been writing so far all tend to use plaintext output
(with the [print()]{.literal} function) and input (with the
[input()]{.literal} function). However, PyAutoGUI programs will use your
entire desktop as its playground. The text-based window that your
program runs in, whether it's Mu or a Terminal window, will probably be
lost as your PyAutoGUI program clicks and interacts with other windows.
This can make getting input and output from the user hard if the Mu or
Terminal windows get hidden under other windows.

To solve this, PyAutoGUI offers pop-up message boxes to provide
notifications to the user and receive input from them. There are four
message box functions:

[pyautogui.alert(text)]{.codestrong} Displays [text]{.literal} and has a
single OK button.

[pyautogui.confirm(text)]{.literal} Displays [text]{.literal} and has OK
and Cancel buttons, returning either [\'OK\']{.literal} or
[\'Cancel\']{.literal} depending on the button clicked.

[pyautogui.prompt(text)]{.codestrong} Displays [text]{.literal} and has
a text field for the user to type in, which it returns as a string.

[pyautogui.password(text)]{.codestrong} Is the same as
[prompt()]{.literal}, but displays asterisks so the user can enter
sensitive information such as a password.

These functions also have an optional second parameter that accepts a
string value to use as the title in the title bar of the message box.
The functions won't return until the user has clicked a button on them,
so they can also be used to introduce pauses into your PyAutoGUI
programs. Enter the following into the interactive shell:

\>\>\> [import pyautogui]{.codestrong1}\
\>\>\> [pyautogui.alert(\'This is a message.\',
\'Important\')]{.codestrong1}\
\'OK\'\
\>\>\> [pyautogui.confirm(\'Do you want to continue?\')]{.codestrong1}
\# Click Cancel\
\'Cancel\'\
\>\>\> [pyautogui.prompt(\"What is your cat\'s name?\")]{.codestrong1}\
\'Zophie\'\
\>\>\> [pyautogui.password(\'What is the password?\')]{.codestrong1}\
\'hunter2\'

The pop-up message boxes that these lines produce look like [Figure
20-8](#calibre_link-1636){.calibre6}.

::: image1
[]{#calibre_link-1881 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1636
.calibre6}![image](../images/000043.jpg){.calibre3}
:::

*Figure 20-8: From top left to bottom right, the windows created by
[alert()]{.literal1}, [confirm()]{.literal1}, [prompt()]{.literal1}, and
[password()]{.literal1}*

These functions can be used to provide notifications or ask the user
questions while the rest of the program interacts with the computer
through the mouse and keyboard. The full online documentation can be
found at
*[https://pymsgbox.readthedocs.io](https://pymsgbox.readthedocs.io){.calibre6}*.

### **Summary** {#calibre_link-670 .h1}

GUI automation with the [pyautogui]{.literal} module allows you to
interact with applications on your computer by controlling the mouse and
keyboard. While this approach is flexible enough to do anything that a
human user can do, the downside is that these programs are fairly blind
to what they are clicking or typing. When writing GUI automation
programs, try to ensure that they will crash quickly if they're given
bad instructions. Crashing is annoying, but it's much better than the
program continuing in error.

You can move the mouse cursor around the screen and simulate mouse
clicks, keystrokes, and keyboard shortcuts with PyAutoGUI. The
[pyautogui]{.literal} module can also check the colors on the screen,
which can provide your GUI automation program with enough of an idea of
the screen contents to know whether it has gotten offtrack. You can even
give PyAutoGUI a screenshot and let it figure out the coordinates of the
area you want to click.

You can combine all of these PyAutoGUI features to automate any
mindlessly repetitive task on your computer. In fact, it can be
downright hypnotic to watch the mouse cursor move on its own and to see
text appear on the screen automatically. Why not spend the time you
saved by sitting back and watching your program do all your work for
you? There's a certain satisfaction that comes from seeing how your
cleverness has saved you from the boring stuff.

### []{#calibre_link-1882 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Practice Questions** {#calibre_link-671 .h1}

[1](#calibre_link-1637){#calibre_link-1473 .calibre6}. How can you
trigger PyAutoGUI's fail-safe to stop a program?

[2](#calibre_link-1638){#calibre_link-1474 .calibre6}. What function
returns the current [resolution()]{.literal}?

[3](#calibre_link-1639){#calibre_link-1475 .calibre6}. What function
returns the coordinates for the mouse cursor's current position?

[4](#calibre_link-1640){#calibre_link-1476 .calibre6}. What is the
difference between [pyautogui.moveTo()]{.literal} and
[pyautogui.move()]{.literal}?

[5](#calibre_link-1641){#calibre_link-1477 .calibre6}. What functions
can be used to drag the mouse?

[6](#calibre_link-1642){#calibre_link-1478 .calibre6}. What function
call will type out the characters of [\"Hello, world!\"]{.literal}?

[7](#calibre_link-1643){#calibre_link-1479 .calibre6}. How can you do
keypresses for special keys such as the keyboard's left arrow key?

[8](#calibre_link-1644){#calibre_link-1480 .calibre6}. How can you save
the current contents of the screen to an image file named
*screenshot.png*?

[9](#calibre_link-1645){#calibre_link-1481 .calibre6}. What code would
set a two-second pause after every PyAutoGUI function call?

[10](#calibre_link-1646){#calibre_link-1482 .calibre6}. If you want to
automate clicks and keystrokes inside a web browser, should you use
PyAutoGUI or Selenium?

[11](#calibre_link-1647){#calibre_link-1483 .calibre6}. What makes
PyAutoGUI error-prone?

[12](#calibre_link-1648){#calibre_link-1484 .calibre6}. How can you find
the size of every window on the screen that includes the text
[Notepad]{.literal} in its title?

[13](#calibre_link-1649){#calibre_link-1485 .calibre6}. How can you
make, say, the Firefox browser active and in front of every other window
on the screen?

### **Practice Projects** {#calibre_link-672 .h1}

For practice, write programs that do the following.

#### ***Looking Busy*** {#calibre_link-673 .h2}

Many instant messaging programs determine whether you are idle, or away
from your computer, by detecting a lack of mouse movement over some
period of time---say, 10 minutes. Maybe you're away from your computer
but don't want others to see your instant messenger status go into idle
mode. Write a script to nudge your mouse cursor slightly every 10
seconds. The nudge should be small and infrequent enough so that it
won't get in the way if you do happen to need to use your computer while
the script is running.

#### ***Using the Clipboard to Read a Text Field*** {#calibre_link-674 .h2}

While you can send keystrokes to an application's text fields with
[pyautogui.write()]{.literal}, you can't use PyAutoGUI alone to read the
text already inside a text field. This is where the Pyperclip module can
help. You can use PyAutoGUI to obtain the window for a text editor such
as Mu or Notepad, []{#calibre_link-1883 {http:="" www.idpf.org=""
2007="" ops}type="pagebreak"}bring it to the front of the screen by
clicking on it, click inside the text field, and then send the
[CTRL]{.small}-A or ![image](../images/000064.jpg){.calibre3}-A hotkey
to "select all" and [CTRL]{.small}-C or
![image](../images/000064.jpg){.calibre3}-C hotkey to "copy to
clipboard." Your Python script can then read the clipboard text by
running [import pyperclip]{.literal} and [pyperclip.paste()]{.literal}.

Write a program that follows this procedure for copying the text from a
window's text fields. Use
[pyautogui.getWindowsWithTitle(\'Notepad\')]{.literal} (or whichever
text editor you choose) to obtain a Window object. The [top]{.literal}
and [left]{.literal} attributes of this Window object can tell you where
this window is, while the [activate()]{.literal} method will ensure it
is at the front of the screen. You can then click the main text field of
the text editor by adding, say, [100]{.literal} or [200]{.literal}
pixels to the [top]{.literal} and [left]{.literal} attribute values with
[pyautogui.click()]{.literal} to put the keyboard focus there. Call
[pyautogui.hotkey(\'ctrl\', \'a\')]{.literal} and
[pyautogui.hotkey(\'ctrl\', \'c\')]{.literal} to select all the text and
copy it to the clipboard. Finally, call [pyperclip.paste()]{.literal} to
retrieve the text from the clipboard and paste it into your Python
program. From there, you can use this string however you want, but just
pass it to [print()]{.literal} for now.

Note that the window functions of PyAutoGUI only work on Windows as of
PyAutoGUI version 1.0.0, and not on macOS or Linux.

#### ***Instant Messenger Bot*** {#calibre_link-675 .h2}

Google Talk, Skype, Yahoo Messenger, AIM, and other instant messaging
applications often use proprietary protocols that make it difficult for
others to write Python modules that can interact with these programs.
But even these proprietary protocols can't stop you from writing a GUI
automation tool.

The Google Talk application has a search bar that lets you enter a
username on your friend list and open a messaging window when you press
[ENTER]{.small}. The keyboard focus automatically moves to the new
window. Other instant messenger applications have similar ways to open
new message windows. Write a program that will automatically send out a
notification message to a select group of people on your friend list.
Your program may have to deal with exceptional cases, such as friends
being offline, the chat window appearing at different coordinates on the
screen, or confirmation boxes that interrupt your messaging. Your
program will have to take screenshots to guide its GUI interaction and
adopt ways of detecting when its virtual keystrokes aren't being sent.

::: note
**[NOTE]{.notes}**

*You may want to set up some fake test accounts so that you don't
accidentally spam your real friends while writing this program.*
:::

#### []{#calibre_link-1884 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Game-Playing Bot Tutorial*** {#calibre_link-676 .h2}

There is a great tutorial titled "How to Build a Python Bot That Can
Play Web Games" that you can find a link to at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.
This tutorial explains how to create a GUI automation program in Python
that plays a Flash game called Sushi Go Round. The game involves
clicking the correct ingredient buttons to fill customers' sushi orders.
The faster you fill orders without mistakes, the more points you get.
This is a perfectly suited task for a GUI automation program---and a way
to cheat to a high score! The tutorial covers many of the same topics
that this chapter covers but also includes descriptions of PyAutoGUI's
basic image recognition features. The source code for this bot is at
*[https://github.com/asweigart/sushigoroundbot/](https://github.com/asweigart/sushigoroundbot/){.calibre6}*
and a video of the bot playing the game is at
*[https://youtu.be/lfk_T6VKhTE](https://youtu.be/lfk_T6VKhTE){.calibre6}*.[]{#calibre_link-1885
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}
:::

<div>

[![](/images/prev.png)](../chapter19)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../appendixa)

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
