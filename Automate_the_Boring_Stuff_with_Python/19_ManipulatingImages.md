
## **MANIPULATING IMAGES**


![Image](../images/000075.jpg){.calibre3}


If you have a digital camera or even if you just upload photos from your
phone to Facebook, you probably cross paths with digital image files all
the time. You may know how to use basic graphics software, such as
Microsoft Paint or Paintbrush, or even more advanced applications such
as Adobe Photoshop. But if you need to edit a massive number of images,
editing them by hand can be a lengthy, boring job.

Enter Python. Pillow is a third-party Python module for interacting with
image files. The module has several functions that make it easy to crop,
resize, and edit the content of an image. With the power to manipulate
images the same way you would with software such as Microsoft Paint or
Adobe Photoshop, Python can automatically edit hundreds or thousands of
images with ease. You can install Pillow by running [pip install \--user
-U pillow==6.0.0]{.literal}. [Appendix A](#calibre_link-2){.calibre6}
has more details on installing modules.

### []{#calibre_link-789 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Computer Image Fundamentals** {#calibre_link-609 .h1}

In order to manipulate an image, you need to understand the basics of
how computers deal with colors and coordinates in images and how you can
work with colors and coordinates in Pillow. But before you continue,
install the [pillow]{.literal} module. See [Appendix
A](#calibre_link-2){.calibre6} for help installing third-party modules.

#### ***Colors and RGBA Values*** {#calibre_link-610 .h2}

Computer programs often represent a color in an image as an *RGBA
value*. An RGBA value is a group of numbers that specify the amount of
red, green, blue, and *alpha* (or transparency) in a color. Each of
these component values is an integer from 0 (none at all) to 255 (the
maximum). These RGBA values are assigned to individual *pixels*; a pixel
is the smallest dot of a single color the computer screen can show (as
you can imagine, there are millions of pixels on a screen). A pixel's
RGB setting tells it precisely what shade of color it should display.
Images also have an alpha value to create RGBA values. If an image is
displayed on the screen over a background image or desktop wallpaper,
the alpha value determines how much of the background you can "see
through" the image's pixel.

In Pillow, RGBA values are represented by a tuple of four integer
values. For example, the color red is represented by [(255, 0, 0,
255)]{.literal}. This color has the maximum amount of red, no green or
blue, and the maximum alpha value, meaning it is fully opaque. Green is
represented by [(0, 255, 0, 255)]{.literal}, and blue is [(0, 0, 255,
255)]{.literal}. White, the combination of all colors, is [(255, 255,
255, 255)]{.literal}, while black, which has no color at all, is [(0, 0,
0, 255)]{.literal}.

If a color has an alpha value of 0, it is invisible, and it doesn't
really matter what the RGB values are. After all, invisible red looks
the same as invisible black.

Pillow uses the standard color names that HTML uses. [Table
19-1](#calibre_link-1487){.calibre6} lists a selection of standard color
names and their values.

**Table 19-1:** Standard Color Names and Their RGBA Values

  **Name**   **RGBA value**                     **Name**   **RGBA value**
  ---------- ---------------------------------- ---------- --------------------------------
  White      [(255, 255, 255, 255)]{.literal}   Red        [(255, 0, 0, 255)]{.literal}
  Green      [(0, 128, 0, 255)]{.literal}       Blue       [(0, 0, 255, 255)]{.literal}
  Gray       [(128, 128, 128, 255)]{.literal}   Yellow     [(255, 255, 0, 255)]{.literal}
  Black      [(0, 0, 0, 255)]{.literal}         Purple     [(128, 0, 128, 255)]{.literal}

Pillow offers the [ImageColor.getcolor()]{.literal} function so you
don't have to memorize RGBA values for the colors you want to use. This
function takes a color name string as its first argument, and the string
[\'RGBA\']{.literal} as its second argument, and it returns an RGBA
tuple.

[]{#calibre_link-849 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}To see how this function works, enter the following
into the interactive shell:

[➊]{.ent} \>\>\> [from PIL import ImageColor]{.codestrong1}\
[➋]{.ent} \>\>\> [ImageColor.getcolor(\'red\', \'RGBA\')]{.codestrong1}\
   (255, 0, 0, 255)\
[➌]{.ent} \>\>\> [ImageColor.getcolor(\'RED\', \'RGBA\')]{.codestrong1}\
   (255, 0, 0, 255)\
   \>\>\> [ImageColor.getcolor(\'Black\', \'RGBA\')]{.codestrong1}\
   (0, 0, 0, 255)\
   \>\>\> [ImageColor.getcolor(\'chocolate\', \'RGBA\')]{.codestrong1}\
   (210, 105, 30, 255)\
   \>\>\> [ImageColor.getcolor(\'CornflowerBlue\',
\'RGBA\')]{.codestrong1}\
   (100, 149, 237, 255)

First, you need to import the [ImageColor]{.literal} module from PIL
[➊]{.ent} (not from Pillow; you'll see why in a moment). The color name
string you pass to [ImageColor.getcolor()]{.literal} is
case-insensitive, so passing [\'red\']{.literal} [➋]{.ent} and passing
[\'RED\']{.literal} [➌]{.ent} give you the same RGBA tuple. You can also
pass more unusual color names, like [\'chocolate\']{.literal} and
[\'Cornflower Blue\']{.literal}.

Pillow supports a huge number of color names, from
[\'aliceblue\']{.literal} to [\'whitesmoke\']{.literal}. You can find
the full list of more than 100 standard color names in the resources at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

#### ***Coordinates and Box Tuples*** {#calibre_link-611 .h2}

Image pixels are addressed with x- and y-coordinates, which respectively
specify a pixel's horizontal and vertical locations in an image. The
*origin* is the pixel at the top-left corner of the image and is
specified with the notation (0, 0). The first zero represents the
x-coordinate, which starts at zero at the origin and increases going
from left to right. The second zero represents the y-coordinate, which
starts at zero at the origin and increases going down the image. This
bears repeating: y-coordinates increase going downward, which is the
opposite of how you may remember y-coordinates being used in math class.
[Figure 19-1](#calibre_link-1488){.calibre6} demonstrates how this
coordinate system works.


[]{#calibre_link-1488
.calibre6}![image](../images/000023.jpg){.calibre3}


*Figure 19-1: The x- and y-coordinates of a 28×27 image of some sort of
ancient data storage device*

[]{#calibre_link-822 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Many of Pillow's functions and methods take a *box
tuple* argument. This means Pillow is expecting a tuple of four integer
coordinates that represent a rectangular region in an image. The four
integers are, in order, as follows:

**Left** The x-coordinate of the leftmost edge of the box.

**Top** The y-coordinate of the top edge of the box.

**Right** The x-coordinate of one pixel to the right of the rightmost
edge of the box. This integer must be greater than the left integer.

**Bottom** The y-coordinate of one pixel lower than the bottom edge of
the box. This integer must be greater than the top integer.

Note that the box includes the left and top coordinates and goes up to
but does not include the right and bottom coordinates. For example, the
box tuple [(3, 1, 9, 6)]{.literal} represents all the pixels in the
black box in [Figure 19-2](#calibre_link-1489){.calibre6}.


[]{#calibre_link-1489
.calibre6}![image](../images/000115.jpg){.calibre3}


*Figure 19-2: The area represented by the box tuple [(3, 1, 9,
6)]{.literal1}*

### **Manipulating Images with Pillow** {#calibre_link-612 .h1}

Now that you know how colors and coordinates work in Pillow, let's use
Pillow to manipulate an image. [Figure
19-3](#calibre_link-1490){.calibre6} is the image that will be used for
all the interactive shell examples in this chapter. You can download it
from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

Once you have the image file *zophie.png* in your current working
directory, you'll be ready to load the image of Zophie into Python, like
so:

\>\>\> [from PIL import Image]{.codestrong1}\
\>\>\> [catIm = Image.open(\'zophie.png\')]{.codestrong1}

To load the image, import the [Image]{.literal} module from Pillow and
call [Image.open()]{.literal}, passing it the image's filename. You can
then store the loaded image in a variable like [CatIm]{.literal}.
Pillow's module name is [PIL]{.literal} to make it backward compatible
with an older module called Python Imaging Library; this is why you must
run [from PIL import Image]{.literal} instead of [from Pillow import
Image]{.literal}. Because of the way Pillow's creators set up the
[pillow]{.literal} module, you must use the [import]{.literal} statement
[from PIL import Image]{.literal}, rather than simply [import
PIL]{.literal}.


[]{#calibre_link-839 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1490
.calibre6}![image](../images/000061.jpg){.calibre3}


*Figure 19-3: My cat, Zophie. The camera adds 10 pounds (which is a lot
for a cat).*

If the image file isn't in the current working directory, change the
working directory to the folder that contains the image file by calling
the [os.chdir()]{.literal} function.

\>\>\> [import os]{.codestrong1}\
\>\>\> [os.chdir(\'C:\\\\folder_with_image_file\')]{.codestrong1}

The [Image.open()]{.literal} function returns a value of the
[Image]{.literal} object data type, which is how Pillow represents an
image as a Python value. You can load an [Image]{.literal} object from
an image file (of any format) by passing the [Image.open()]{.literal}
function a string of the filename. Any changes you make to the
[Image]{.literal} object can be saved to an image file (also of any
format) with the [save()]{.literal} method. All the rotations, resizing,
cropping, drawing, and other image manipulations will be done through
method calls on this [Image]{.literal} object.

To shorten the examples in this chapter, I'll assume you've imported
Pillow's [Image]{.literal} module and that you have the Zophie image
stored in a variable named [catIm]{.literal}. Be sure that the
*zophie.png* file is in the current working directory so that the
[Image.open()]{.literal} function can find it. Otherwise, you will also
have to specify the full absolute path in the string argument to
[Image.open()]{.literal}.

#### ***Working with the Image Data Type*** {#calibre_link-613 .h2}

An [Image]{.literal} object has several useful attributes that give you
basic information about the image file it was loaded from: its width and
height, the filename, and the graphics format (such as JPEG, GIF, or
PNG).

For example, enter the following into the interactive shell:

   \>\>\> [from PIL import Image]{.codestrong1}\
   \>\>\> [catIm = Image.open(\'zophie.png\')]{.codestrong1}\
   []{#calibre_link-1052 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [catIm.size]{.codestrong1}\
[➊]{.ent} (816, 1088)\
[➋]{.ent} \>\>\> [width, height = catIm.size]{.codestrong1}\
[➌]{.ent} \>\>\> [width]{.codestrong1}\
   816\
[➍]{.ent} \>\>\> [height]{.codestrong1}\
   1088\
   \>\>\> [catIm.filename]{.codestrong1}\
   \'zophie.png\'\
   \>\>\> [catIm.format]{.codestrong1}\
   \'PNG\'\
   \>\>\> [catIm.format_description]{.codestrong1}\
   \'Portable network graphics\'\
[➎]{.ent} \>\>\> [catIm.save(\'zophie.jpg\')]{.codestrong1}

After making an [Image]{.literal} object from *zophie.png* and storing
the [Image]{.literal} object in [catIm]{.literal}, we can see that the
object's [size]{.literal} attribute contains a tuple of the image's
width and height in pixels [➊]{.ent}. We can assign the values in the
tuple to [width]{.literal} and [height]{.literal} variables [➋]{.ent} in
order to access with width [➌]{.ent} and height [➍]{.ent} individually.
The [filename]{.literal} attribute describes the original file's name.
The [format]{.literal} and [format_description]{.literal} attributes are
strings that describe the image format of the original file (with
[format_description]{.literal} being a bit more verbose).

Finally, calling the [save()]{.literal} method and passing it
[\'zophie.jpg\']{.literal} saves a new image with the filename
*zophie.jpg* to your hard drive [➎]{.ent}. Pillow sees that the file
extension is *.jpg* and automatically saves the image using the JPEG
image format. Now you should have two images, *zophie.png* and
*zophie.jpg*, on your hard drive. While these files are based on the
same image, they are not identical because of their different formats.

Pillow also provides the [Image.new()]{.literal} function, which returns
an [Image]{.literal} object---much like [Image.open()]{.literal}, except
the image represented by [Image.new()]{.literal}'s object will be blank.
The arguments to [Image.new()]{.literal} are as follows:

-   The string [\'RGBA\']{.literal}, which sets the color mode to RGBA.
    (There are other modes that this book doesn't go into.)
-   The size, as a two-integer tuple of the new image's width and
    height.
-   The background color that the image should start with, as a
    four-integer tuple of an RGBA value. You can use the return value of
    the [ImageColor.getcolor()]{.literal} function for this argument.
    Alternatively, [Image.new()]{.literal} also supports just passing
    the string of the standard color name.

For example, enter the following into the interactive shell:

   \>\>\> [from PIL import Image]{.codestrong1}\
[➊]{.ent} \>\>\> [im = Image.new(\'RGBA\', (100, 200),
\'purple\')]{.codestrong1}\
   \>\>\> [im.save(\'purpleImage.png\')]{.codestrong1}\
[➋]{.ent} \>\>\> [im2 = Image.new(\'RGBA\', (20, 20))]{.codestrong1}\
   \>\>\> [im2.save(\'transparentImage.png\')]{.codestrong1}

[]{#calibre_link-880 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here we create an [Image]{.literal} object for an
image that's 100 pixels wide and 200 pixels tall, with a purple
background [➊]{.ent}. This image is then saved to the file
*purpleImage.png*. We call [Image.new()]{.literal} again to create
another [Image]{.literal} object, this time passing (20, 20) for the
dimensions and nothing for the background color [➋]{.ent}. Invisible
black, [(0, 0, 0, 0)]{.literal}, is the default color used if no color
argument is specified, so the second image has a transparent background;
we save this 20×20 transparent square in *transparentImage.png*.

#### ***Cropping Images*** {#calibre_link-614 .h2}

*Cropping* an image means selecting a rectangular region inside an image
and removing everything outside the rectangle. The [crop()]{.literal}
method on [Image]{.literal} objects takes a box tuple and returns an
[Image]{.literal} object representing the cropped image. The cropping
does not happen in place---that is, the original [Image]{.literal}
object is left untouched, and the [crop()]{.literal} method returns a
new [Image]{.literal} object. Remember that a boxed tuple---in this
case, the cropped section---includes the left column and top row of
pixels but only goes up to and does *not* include the right column and
bottom row of pixels.

Enter the following into the interactive shell:

\>\>\> [from PIL import Image]{.codestrong1}\
\>\>\> [catIm = Image.open(\'zophie.png\')]{.codestrong1}\
\>\>\> [croppedIm = catIm.crop((335, 345, 565, 560))]{.codestrong1}\
\>\>\> [croppedIm.save(\'cropped.png\')]{.codestrong1}

This makes a new [Image]{.literal} object for the cropped image, stores
the object in [croppedIm]{.literal}, and then calls [save()]{.literal}
on [croppedIm]{.literal} to save the cropped image in *cropped.png*. The
new file *cropped.png* will be created from the original image, like in
[Figure 19-4](#calibre_link-1491){.calibre6}.


[]{#calibre_link-1491
.calibre6}![image](../images/000003.jpg){.calibre3}


*Figure 19-4: The new image will be just the cropped section of the
original image.*

#### []{#calibre_link-869 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Copying and Pasting Images onto Other Images*** {#calibre_link-615 .h2}

The [copy()]{.literal} method will return a new [Image]{.literal} object
with the same image as the [Image]{.literal} object it was called on.
This is useful if you need to make changes to an image but also want to
keep an untouched version of the original. For example, enter the
following into the interactive shell:

\>\>\> [from PIL import Image]{.codestrong1}\
\>\>\> [catIm = Image.open(\'zophie.png\')]{.codestrong1}\
\>\>\> [catCopyIm = catIm.copy()]{.codestrong1}

The [catIm]{.literal} and [catCopyIm]{.literal} variables contain two
separate [Image]{.literal} objects, which both have the same image on
them. Now that you have an [Image]{.literal} object stored in
[catCopyIm]{.literal}, you can modify [catCopyIm]{.literal} as you like
and save it to a new filename, leaving *zophie.png* untouched. For
example, let's try modifying [catCopyIm]{.literal} with the
[paste()]{.literal} method.

The [paste()]{.literal} method is called on an [Image]{.literal} object
and pastes another image on top of it. Let's continue the shell example
by pasting a smaller image onto [catCopyIm]{.literal}.

\>\>\> [faceIm = catIm.crop((335, 345, 565, 560))]{.codestrong1}\
\>\>\> [faceIm.size]{.codestrong1}\
(230, 215)\
\>\>\> [catCopyIm.paste(faceIm, (0, 0))]{.codestrong1}\
\>\>\> [catCopyIm.paste(faceIm, (400, 500))]{.codestrong1}\
\>\>\> [catCopyIm.save(\'pasted.png\')]{.codestrong1}

First we pass [crop()]{.literal} a box tuple for the rectangular area in
*zophie.png* that contains Zophie's face. This creates an
[Image]{.literal} object representing a 230×215 crop, which we store in
[faceIm]{.literal}. Now we can paste [faceIm]{.literal} onto
[catCopyIm]{.literal}. The [paste()]{.literal} method takes two
arguments: a "source" [Image]{.literal} object and a tuple of the x- and
y-coordinates where you want to paste the top-left corner of the source
[Image]{.literal} object onto the main [Image]{.literal} object. Here we
call [paste()]{.literal} twice on [catCopyIm]{.literal}, passing (0, 0)
the first time and (400, 500) the second time. This pastes
[faceIm]{.literal} onto [catCopyIm]{.literal} twice: once with the
top-left corner of [faceIm]{.literal} at (0, 0) on
[catCopyIm]{.literal}, and once with the top-left corner of
[faceIm]{.literal} at (400, 500). Finally, we save the modified
[catCopyIm]{.literal} to *pasted.png*. The *pasted.png* image looks like
[Figure 19-5](#calibre_link-1492){.calibre6}.


**[NOTE]{.notes}**

*Despite their names, the [copy()]{.codeitalic} and
[paste()]{.codeitalic} methods in Pillow do not use your computer's
clipboard.*


Note that the [paste()]{.literal} method modifies its [Image]{.literal}
object *in place*; it does not return an [Image]{.literal} object with
the pasted image. If you want to call [paste()]{.literal} but also keep
an untouched version of the original image around, you'll need to first
copy the image and then call [paste()]{.literal} on that copy.


[]{#calibre_link-1860 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1492
.calibre6}![image](../images/000093.jpg){.calibre3}


*Figure 19-5: Zophie the cat, with her face pasted twice*

Say you want to tile Zophie's head across the entire image, as in
[Figure 19-6](#calibre_link-1493){.calibre6}. You can achieve this
effect with just a couple [for]{.literal} loops. Continue the
interactive shell example by entering the following:

   \>\>\> [catImWidth, catImHeight = catIm.size]{.codestrong1}\
   \>\>\> [faceImWidth, faceImHeight = faceIm.size]{.codestrong1}\
[➊]{.ent} \>\>\> [catCopyTwo = catIm.copy()]{.codestrong1}\
[➋]{.ent} \>\>\> [for left in range(0, catImWidth,
faceImWidth):]{.codestrong1}\
        [➌]{.ent} [for top in range(0, catImHeight,
faceImHeight):]{.codestrong1}\
               [print(left, top)]{.codestrong1}\
               [catCopyTwo.paste(faceIm, (left, top))]{.codestrong1}\
   0 0\
   0 215\
   0 430\
   0 645\
   0 860\
   0 1075\
   230 0\
   230 215\
   \--[snip]{.codeitalic1}\--\
   690 860\
   690 1075\
   \>\>\> [catCopyTwo.save(\'tiled.png\')]{.codestrong1}


[]{#calibre_link-1005 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1493
.calibre6}![image](../images/000045.jpg){.calibre3}


*Figure 19-6: Nested [for]{.literal1} loops used with
[paste()]{.literal1} to duplicate the cat's face (a dupli-cat, if you
will)*

Here we store the width of height of [catIm]{.literal} in
[catImWidth]{.literal} and [catImHeight]{.literal}. At [➊]{.ent} we make
a copy of [catIm]{.literal} and store it in [catCopyTwo]{.literal}. Now
that we have a copy that we can paste onto, we start looping to paste
[faceIm]{.literal} onto [catCopyTwo]{.literal}. The outer
[for]{.literal} loop's [left]{.literal} variable starts at 0 and
increases by [faceImWidth(230)]{.literal} [➋]{.ent}. The inner
[for]{.literal} loop's [top]{.literal} variable start at 0 and increases
by [faceImHeight(215)]{.literal} [➌]{.ent}. These nested [for]{.literal}
loops produce values for [left]{.literal} and [top]{.literal} to paste a
grid of [faceIm]{.literal} images over the [catCopyTwo]{.literal}
[Image]{.literal} object, as in [Figure
19-6](#calibre_link-1493){.calibre6}. To see our nested loops working,
we print [left]{.literal} and [top]{.literal}. After the pasting is
complete, we save the modified [catCopyTwo]{.literal} to *tiled.png*.

#### ***Resizing an Image*** {#calibre_link-616 .h2}

The [resize()]{.literal} method is called on an [Image]{.literal} object
and returns a new [Image]{.literal} object of the specified width and
height. It accepts a two-integer tuple argument, representing the new
width and height of the returned image. Enter the following into the
interactive shell:

   \>\>\> [from PIL import Image]{.codestrong1}\
   \>\>\> [catIm = Image.open(\'zophie.png\')]{.codestrong1}\
[➊]{.ent} \>\>\> [width, height = catIm.size]{.codestrong1}\
[➋]{.ent} \>\>\> [quartersizedIm = catIm.resize((int(width / 2),
int(height / 2)))]{.codestrong1}\
   \>\>\> [quartersizedIm.save(\'quartersized.png\')]{.codestrong1}\
[➌]{.ent} \>\>\> [svelteIm = catIm.resize((width, height +
300))]{.codestrong1}\
   \>\>\> [svelteIm.save(\'svelte.png\')]{.codestrong1}

Here we assign the two values in the [catIm.size]{.literal} tuple to the
variables [width]{.literal} and [height]{.literal} [➊]{.ent}. Using
[width]{.literal} and [height]{.literal} instead of
[catIm.size\[0\]]{.literal} and [catIm.size\[1\]]{.literal} makes the
rest of the code more readable.

[]{#calibre_link-835 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The first [resize()]{.literal} call passes
[int(width / 2)]{.literal} for the new width and [int(height /
2)]{.literal} for the new height [➋]{.ent}, so the [Image]{.literal}
object returned from [resize()]{.literal} will be half the length and
width of the original image, or one-quarter of the original image size
overall. The [resize()]{.literal} method accepts only integers in its
tuple argument, which is why you needed to wrap both divisions by
[2]{.literal} in an [int()]{.literal} call.

This resizing keeps the same proportions for the width and height. But
the new width and height passed to [resize()]{.literal} do not have to
be proportional to the original image. The [svelteIm]{.literal} variable
contains an [Image]{.literal} object that has the original width but a
height that is 300 pixels taller [➌]{.ent}, giving Zophie a more slender
look.

Note that the [resize()]{.literal} method does not edit the
[Image]{.literal} object in place but instead returns a new
[Image]{.literal} object.

#### ***Rotating and Flipping Images*** {#calibre_link-617 .h2}

Images can be rotated with the [rotate()]{.literal} method, which
returns a new [Image]{.literal} object of the rotated image and leaves
the original [Image]{.literal} object unchanged. The argument to
[rotate()]{.literal} is a single integer or float representing the
number of degrees to rotate the image counterclockwise. Enter the
following into the interactive shell:

\>\>\> [from PIL import Image]{.codestrong1}\
\>\>\> [catIm = Image.open(\'zophie.png\')]{.codestrong1}\
\>\>\> [catIm.rotate(90).save(\'rotated90.png\')]{.codestrong1}\
\>\>\> [catIm.rotate(180).save(\'rotated180.png\')]{.codestrong1}\
\>\>\> [catIm.rotate(270).save(\'rotated270.png\')]{.codestrong1}

Note how you can *chain* method calls by calling [save()]{.literal}
directly on the [Image]{.literal} object returned from
[rotate()]{.literal}. The first [rotate()]{.literal} and
[save()]{.literal} call makes a new [Image]{.literal} object
representing the image rotated counterclockwise by 90 degrees and saves
the rotated image to *rotated90.png*. The second and third calls do the
same, but with 180 degrees and 270 degrees. The results look like
[Figure 19-7](#calibre_link-1494){.calibre6}.


[]{#calibre_link-1494
.calibre6}![image](../images/000139.jpg){.calibre3}


*Figure 19-7: The original image (left) and the image rotated
counterclockwise by 90, 180, and 270 degrees*

Notice that the width and height of the image change when the image is
rotated 90 or 270 degrees. If you rotate an image by some other amount,
the original dimensions of the image are maintained. On Windows, a
[]{#calibre_link-1061 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}black background is used to fill in any gaps made
by the rotation, like in [Figure 19-8](#calibre_link-1495){.calibre6}.
On macOS, transparent pixels are used for the gaps instead.

The [rotate()]{.literal} method has an optional [expand]{.literal}
keyword argument that can be set to [True]{.literal} to enlarge the
dimensions of the image to fit the entire rotated new image. For
example, enter the following into the interactive shell:

\>\>\> [catIm.rotate(6).save(\'rotated6.png\')]{.codestrong1}\
\>\>\> [catIm.rotate(6,
expand=True).save(\'rotated6_expanded.png\')]{.codestrong1}

The first call rotates the image 6 degrees and saves it to *rotate6.png*
(see the image on the left of [Figure
19-8](#calibre_link-1495){.calibre6}). The second call rotates the image
6 degrees with [expand]{.literal} set to [True]{.literal} and saves it
to *rotate6_expanded.png* (see the image on the right of [Figure
19-8](#calibre_link-1495){.calibre6}).


[]{#calibre_link-1495
.calibre6}![image](../images/000082.jpg){.calibre3}


*Figure 19-8: The image rotated 6 degrees normally (left) and with
[expand=True]{.literal1} (right)*

You can also get a "mirror flip" of an image with the
[transpose()]{.literal} method. You must pass either
[Image.FLIP_LEFT_RIGHT]{.literal} or [Image.FLIP_TOP_BOTTOM]{.literal}
to the [transpose()]{.literal} method. Enter the following into the
interactive shell:

\>\>\>
[catIm.transpose(Image.FLIP_LEFT_RIGHT).save(\'horizontal_flip.png\')]{.codestrong1}\
\>\>\>
[catIm.transpose(Image.FLIP_TOP_BOTTOM).save(\'vertical_flip.png\')]{.codestrong1}

Like [rotate()]{.literal}, [transpose()]{.literal} creates a new
[Image]{.literal} object. Here we pass [Image.FLIP_LEFT_RIGHT]{.literal}
to flip the image horizontally and then save the result to
*horizontal_flip.png*. To flip the image vertically, we pass
[Image.FLIP_TOP_BOTTOM]{.literal} and save to *vertical_flip.png*. The
results look like [Figure 19-9](#calibre_link-1496){.calibre6}.


[]{#calibre_link-983 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1496
.calibre6}![image](../images/000030.jpg){.calibre3}


*Figure 19-9: The original image (left), horizontal flip (center), and
vertical flip (right)*

#### ***Changing Individual Pixels*** {#calibre_link-618 .h2}

The color of an individual pixel can be retrieved or set with the
[getpixel()]{.literal} and [putpixel()]{.literal} methods. These methods
both take a tuple representing the x- and y-coordinates of the pixel.
The [putpixel()]{.literal} method also takes an additional tuple
argument for the color of the pixel. This color argument is a
four-integer RGBA tuple or a three-integer RGB tuple. Enter the
following into the interactive shell:

   \>\>\> [from PIL import Image]{.codestrong1}\
[➊]{.ent} \>\>\> [im = Image.new(\'RGBA\', (100, 100))]{.codestrong1}\
[➋]{.ent} \>\>\> [im.getpixel((0, 0))]{.codestrong1}\
   (0, 0, 0, 0)\
[➌]{.ent} \>\>\> [for x in range(100):]{.codestrong1}\
           [for y in range(50):]{.codestrong1}\
            [➍]{.ent} [im.putpixel((x, y), (210, 210,
210))]{.codestrong1}\
\
   \>\>\> [from PIL import ImageColor]{.codestrong1}\
[➎]{.ent} \>\>\> [for x in range(100):]{.codestrong1}\
           [for y in range(50, 100):]{.codestrong1}\
            [➏]{.ent} [im.putpixel((x, y),
ImageColor.getcolor(\'darkgray\', \'RGBA\'))]{.codestrong1}\
   \>\>\> [im.getpixel((0, 0))]{.codestrong1}\
   (210, 210, 210, 255)\
   \>\>\> [im.getpixel((0, 50))]{.codestrong1}\
   (169, 169, 169, 255)\
   \>\>\> [im.save(\'putPixel.png\')]{.codestrong1}

At [➊]{.ent} we make a new image that is a 100×100 transparent square.
Calling [getpixel()]{.literal} on some coordinates in this image returns
[(0, 0, 0, 0)]{.literal} because the image is transparent [➋]{.ent}. To
color pixels in this image, we can use nested [for]{.literal} loops to
go through all the pixels in the top half of the image [➌]{.ent} and
color each pixel using [putpixel()]{.literal} [➍]{.ent}. Here we pass
[putpixel()]{.literal} the RGB tuple [(210, 210, 210)]{.literal}, a
light gray.

[]{#calibre_link-782 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Say we want to color the bottom half of the image
dark gray but don't know the RGB tuple for dark gray. The
[putpixel()]{.literal} method doesn't accept a standard color name like
[\'darkgray\']{.literal}, so you have to use
[ImageColor.getcolor()]{.literal} to get a color tuple from
[\'darkgray\']{.literal}. Loop through the pixels in the bottom half of
the image [➎]{.ent} and pass [putpixel()]{.literal} the return value of
[ImageColor.getcolor()]{.literal} [➏]{.ent}, and you should now have an
image that is light gray in its top half and dark gray in the bottom
half, as shown in [Figure 19-10](#calibre_link-1497){.calibre6}. You can
call [getpixel()]{.literal} on some coordinates to confirm that the
color at any given pixel is what you expect. Finally, save the image to
*putPixel.png*.


[]{#calibre_link-1497
.calibre6}![image](../images/000121.jpg){.calibre3}


*Figure 19-10: The* putPixel.png *image*

Of course, drawing one pixel at a time onto an image isn't very
convenient. If you need to draw shapes, use the [ImageDraw]{.literal}
functions explained later in this chapter.

### **Project: Adding a Logo** {#calibre_link-619 .h1}

Say you have the boring job of resizing thousands of images and adding a
small logo watermark to the corner of each. Doing this with a basic
graphics program such as Paintbrush or Paint would take forever. A
fancier graphics application such as Photoshop can do batch processing,
but that software costs hundreds of dollars. Let's write a script to do
it instead.

Say that [Figure 19-11](#calibre_link-1498){.calibre6} is the logo you
want to add to the bottom-right corner of each image: a black cat icon
with a white border, with the rest of the image transparent.


[]{#calibre_link-1498
.calibre6}![image](../images/000000.jpg){.calibre3}


*Figure 19-11: The logo to be added to the image*

[]{#calibre_link-1861 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}At a high level, here's what the program should do:

1.  Load the logo image.
2.  Loop over all *.png* and*.jpg* files in the working directory.
3.  Check whether the image is wider or taller than 300 pixels.
4.  If so, reduce the width or height (whichever is larger) to 300
    pixels and scale down the other dimension proportionally.
5.  Paste the logo image into the corner.
6.  Save the altered images to another folder.

This means the code will need to do the following:

1.  Open the *catlogo.png* file as an [Image]{.literal} object.
2.  Loop over the strings returned from [os.listdir(\'.\')]{.literal}.
3.  Get the width and height of the image from the [size]{.literal}
    attribute.
4.  Calculate the new width and height of the resized image.
5.  Call the [resize()]{.literal} method to resize the image.
6.  Call the [paste()]{.literal} method to paste the logo.
7.  Call the [save()]{.literal} method to save the changes, using the
    original filename.

#### ***Step 1: Open the Logo Image*** {#calibre_link-620 .h2}

For this project, open a new file editor tab, enter the following code,
and save it as *resizeAndAddLogo.py*:

   #! python3\
   # resizeAndAddLogo.py - Resizes all images in current working
directory to fit\
   # in a 300x300 square, and adds catlogo.png to the lower-right
corner.\
\
   import os\
   from PIL import Image\
\
[➊]{.ent} SQUARE_FIT_SIZE = 300\
[➋]{.ent} LOGO_FILENAME = \'catlogo.png\'\
\
[➌]{.ent} logoIm = Image.open(LOGO_FILENAME)\
[➍]{.ent} logoWidth, logoHeight = logoIm.size\
\
   # TODO: Loop over all files in the working directory.\
\
   # TODO: Check if image needs to be resized.\
\
   # TODO: Calculate the new width and height to resize to.\
\
   # TODO: Resize the image.\
\
   # TODO: Add the logo.\
\
   # TODO: Save changes.

[]{#calibre_link-1862 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}By setting up the [SQUARE_FIT_SIZE]{.literal}
[➊]{.ent} and [LOGO_FILENAME]{.literal} [➋]{.ent} constants at the start
of the program, we've made it easy to change the program later. Say the
logo that you're adding isn't the cat icon, or say you're reducing the
output images' largest dimension to something other than 300 pixels.
With these constants at the start of the program, you can just open the
code, change those values once, and you're done. (Or you can make it so
that the values for these constants are taken from the command line
arguments.) Without these constants, you'd instead have to search the
code for all instances of [300]{.literal} and
[\'catlogo.png\']{.literal} and replace them with the values for your
new project. In short, using constants makes your program more
generalized.

The logo [Image]{.literal} object is returned from
[Image.open()]{.literal} [➌]{.ent}. For readability,
[logoWidth]{.literal} and [logoHeight]{.literal} are assigned to the
values from [logoIm.size]{.literal} [➍]{.ent}.

The rest of the program is a skeleton of [TODO]{.literal} comments for
now.

#### ***Step 2: Loop Over All Files and Open Images*** {#calibre_link-621 .h2}

Now you need to find every *.png* file and *.jpg* file in the current
working directory. You don't want to add the logo image to the logo
image itself, so the program should skip any image with a filename
that's the same as [LOGO_FILENAME]{.literal}. Add the following to your
code:

   #! python3\
   # resizeAndAddLogo.py - Resizes all images in current working
directory to fit\
   # in a 300x300 square, and adds catlogo.png to the lower-right
corner.\
\
   import os\
   from PIL import Image\
\
   \--[snip]{.codeitalic1}\--\
\
   os.makedirs(\'withLogo\', exist_ok=True)\
   [\# Loop over all files in the working directory.]{.codestrong1}\
[➊]{.ent} [for filename in os.listdir(\'.\'):]{.codestrong1}\
[    ]{.codestrong1}[➋]{.ent} [if not (filename.endswith(\'.png\') or
filename.endswith(\'.jpg\')) \\]{.codestrong1}\
   [       or filename == LOGO_FILENAME:]{.codestrong1}\
[        ]{.codestrong1}[➌]{.ent} [continue    # skip non-image files
and the logo file itself]{.codestrong1}\
\
[    ]{.codestrong1}[➍]{.ent} [im = Image.open(filename)]{.codestrong1}\
   [    width, height = im.size]{.codestrong1}\
\
   \--[snip]{.codeitalic1}\--

First, the [os.makedirs()]{.literal} call creates a *withLogo* folder to
store the finished images with logos, instead of overwriting the
original image files. The [exist_ok=True]{.literal} keyword argument
will keep [os.makedirs()]{.literal} from raising an exception if
*withLogo* already exists. While looping through all the files in the
working directory with [os.listdir(\'.\')]{.literal} [➊]{.ent}, the long
[if]{.literal} statement [➋]{.ent} checks whether each filename doesn't
end with *.png* or *.jpg*. If so---or if the file is the logo image
itself---then the loop should skip it and use [continue]{.literal}
[➌]{.ent} to go to the next file. If [filename]{.literal} *does* end
with [\'.png\']{.literal} or [\'.jpg\']{.literal} (and isn't the logo
file), you can open it as an [Image]{.literal} object [➍]{.ent} and set
[width]{.literal} and [height]{.literal}.

#### []{#calibre_link-1863 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 3: Resize the Images*** {#calibre_link-622 .h2}

The program should resize the image only if the width or height is
larger than [SQUARE_FIT_SIZE]{.literal} (300 pixels, in this case), so
put all of the resizing code inside an [if]{.literal} statement that
checks the [width]{.literal} and [height]{.literal} variables. Add the
following code to your program:

#! python3\
\# resizeAndAddLogo.py - Resizes all images in current working directory
to fit\
\# in a 300x300 square, and adds catlogo.png to the lower-right corner.\
\
import os\
from PIL import Image\
\
\--[snip]{.codeitalic1}\--\
\
[     # Check if image needs to be resized.]{.codestrong1}\
[     if width \> SQUARE_FIT_SIZE and height \>
SQUARE_FIT_SIZE:]{.codestrong1}\
[         # Calculate the new width and height to resize
to.]{.codestrong1}\
[         if width \> height:]{.codestrong1}\
[            ]{.codestrong1}[➊]{.ent} [height = int((SQUARE_FIT_SIZE /
width) \* height)]{.codestrong1}\
   [            width = SQUARE_FIT_SIZE]{.codestrong1}\
[         else:]{.codestrong1}\
[            ]{.codestrong1}[➋]{.ent} [width = int((SQUARE_FIT_SIZE /
height) \* width)]{.codestrong1}\
  [             height = SQUARE_FIT_SIZE]{.codestrong1}\
\
   [        # Resize the image.]{.codestrong1}\
   [        print(\'Resizing %s\...\' % (filename))]{.codestrong1}\
[        ]{.codestrong1}[➌]{.ent} [im = im.resize((width,
height))]{.codestrong1}\
\
\--[snip]{.codeitalic1}\--

If the image does need to be resized, you need to find out whether it is
a wide or tall image. If [width]{.literal} is greater than
[height]{.literal}, then the height should be reduced by the same
proportion that the width would be reduced [➊]{.ent}. This proportion is
the [SQUARE_FIT_SIZE]{.literal} value divided by the current width. The
new [height]{.literal} value is this proportion multiplied by the
current [height]{.literal} value. Since the division operator returns a
float value and [resize()]{.literal} requires the dimensions to be
integers, remember to convert the result to an integer with the
[int()]{.literal} function. Finally, the new [width]{.literal} value
will simply be set to [SQUARE_FIT_SIZE]{.literal}.

If the [height]{.literal} is greater than or equal to the
[width]{.literal} (both cases are handled in the [else]{.literal}
clause), then the same calculation is done, except with the
[height]{.literal} and [width]{.literal} variables swapped [➋]{.ent}.

Once [width]{.literal} and [height]{.literal} contain the new image
dimensions, pass them to the [resize()]{.literal} method and store the
returned [Image]{.literal} object in [im]{.literal} [➌]{.ent}.

#### ***Step 4: Add the Logo and Save the Changes*** {#calibre_link-623 .h2}

Whether or not the image was resized, the logo should still be pasted to
the bottom-right corner. Where exactly the logo should be pasted depends
on both the size of the image and the size of the logo. [Figure
19-12](#calibre_link-1499){.calibre6} shows how []{#calibre_link-1864
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}to calculate the
pasting position. The left coordinate for where to paste the logo will
be the image width minus the logo width; the top coordinate for where to
paste the logo will be the image height minus the logo height.


[]{#calibre_link-1499
.calibre6}![image](../images/000011.jpg){.calibre3}


*Figure 19-12: The left and top coordinates for placing the logo in the
bottom-right corner should be the image width/height minus the logo
width/height.*

After your code pastes the logo into the image, it should save the
modified [Image]{.literal} object. Add the following to your program:

#! python3\
\# resizeAndAddLogo.py - Resizes all images in current working directory
to fit\
\# in a 300x300 square, and adds catlogo.png to the lower-right corner.\
\
import os\
from PIL import Image\
\
\--[snip]{.codeitalic1}\--\
\
     # Check if image needs to be resized.\
     \--[snip]{.codeitalic1}\--\
\
[     # Add the logo.]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [print(\'Adding logo to %s\...\' %
(filename))]{.codestrong1}\
[  ]{.codestrong1}[➋]{.ent} [im.paste(logoIm, (width - logoWidth,
height - logoHeight), logoIm)]{.codestrong1}\
\
[     # Save changes.]{.codestrong1}\
[  ]{.codestrong1}[➌]{.ent} [im.save(os.path.join(\'withLogo\',
filename))]{.codestrong1}

The new code prints a message telling the user that the logo is being
added [➊]{.ent}, pastes [logoIm]{.literal} onto [im]{.literal} at the
calculated coordinates [➋]{.ent}, and saves the changes to a filename in
the *withLogo* directory [➌]{.ent}. When you run this program with the
*zophie.png* file as the only image in the working directory, the output
will look like this:

Resizing zophie.png\...\
Adding logo to zophie.png\...

[]{#calibre_link-783 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The image *zophie.png* will be changed to a
225×300-pixel image that looks like [Figure
19-13](#calibre_link-1500){.calibre6}. Remember that the
[paste()]{.literal} method will not paste the transparency pixels if you
do not pass the [logoIm]{.literal} for the third argument as well. This
program can automatically resize and "logo-ify" hundreds of images in
just a couple minutes.


[]{#calibre_link-1500
.calibre6}![image](../images/000106.jpg){.calibre3}


*Figure 19-13: The image* zophie.png *resized and the logo added (left).
If you forget the third argument, the transparent pixels in the logo
will be copied as solid white pixels (right).*

#### ***Ideas for Similar Programs*** {#calibre_link-624 .h2}

Being able to composite images or modify image sizes in a batch can be
useful in many applications. You could write similar programs to do the
following:

-   Add text or a website URL to images.
-   Add timestamps to images.
-   Copy or move images into different folders based on their sizes.
-   Add a mostly transparent watermark to an image to prevent others
    from copying it.

### **Drawing on Images** {#calibre_link-625 .h1}

If you need to draw lines, rectangles, circles, or other simple shapes
on an image, use Pillow's [ImageDraw]{.literal} module. Enter the
following into the interactive shell:

\>\>\> [from PIL import Image, ImageDraw]{.codestrong1}\
\>\>\> [im = Image.new(\'RGBA\', (200, 200), \'white\')]{.codestrong1}\
\>\>\> [draw = ImageDraw.Draw(im)]{.codestrong1}

[]{#calibre_link-922 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}First, we import [Image]{.literal} and
[ImageDraw]{.literal}. Then we create a new image, in this case, a
200×200 white image, and store the [Image]{.literal} object in
[im]{.literal}. We pass the [Image]{.literal} object to the
[ImageDraw.Draw()]{.literal} function to receive an
[ImageDraw]{.literal} object. This object has several methods for
drawing shapes and text onto an [Image]{.literal} object. Store the
[ImageDraw]{.literal} object in a variable like [draw]{.literal} so you
can use it easily in the following example.

#### ***Drawing Shapes*** {#calibre_link-626 .h2}

The following ImageDraw methods draw various kinds of shapes on the
image. The [fill]{.literal} and [outline]{.literal} parameters for these
methods are optional and will default to white if left unspecified.

##### **Points** {#calibre_link-1865 .h2}

The [point(]{.literal}[xy]{.codeitalic}[,]{.literal}
[fill]{.codeitalic}[)]{.literal} method draws individual pixels. The
[xy]{.codeitalic} argument represents a list of the points you want to
draw. The list can be a list of x- and y-coordinate tuples, such as
[\[(x, y), (x, y), \...\]]{.literal}, or a list of x- and y-coordinates
without tuples, such as [\[x1, y1, x2, y2, \...\]]{.literal}. The
[fill]{.codeitalic} argument is the color of the points and is either an
RGBA tuple or a string of a color name, such as [\'red\']{.literal}. The
[fill]{.codeitalic} argument is optional.

##### **Lines** {#calibre_link-1866 .h2}

The [line(]{.literal}[xy]{.codeitalic}[,]{.literal}
[fill]{.codeitalic}[,]{.literal} [width]{.codeitalic}[)]{.literal}
method draws a line or series of lines. [xy]{.codeitalic} is either a
list of tuples, such as [\[(x, y), (x, y), \...\]]{.literal}, or a list
of integers, such as [\[x1, y1, x2, y2, \...\]]{.literal}. Each point is
one of the connecting points on the lines you're drawing. The optional
[fill]{.codeitalic} argument is the color of the lines, as an RGBA tuple
or color name. The optional [width]{.codeitalic} argument is the width
of the lines and defaults to 1 if left unspecified.

##### **Rectangles** {#calibre_link-1867 .h2}

The [rectangle(]{.literal}[xy]{.codeitalic}[,]{.literal}
[fill]{.codeitalic}[,]{.literal} [outline]{.codeitalic}[)]{.literal}
method draws a rectangle. The [xy]{.codeitalic} argument is a box tuple
of the form [(]{.literal}[left]{.codeitalic}[,]{.literal}
[top]{.codeitalic}[,]{.literal} [right]{.codeitalic}[,]{.literal}
[bottom]{.codeitalic}[)]{.literal}. The [left]{.codeitalic} and
[top]{.codeitalic} values specify the x- and y-coordinates of the
upper-left corner of the rectangle, while [right]{.codeitalic} and
[bottom]{.codeitalic} specify the lower-right corner. The optional
[fill]{.codeitalic} argument is the color that will fill the inside of
the rectangle. The optional [outline]{.codeitalic} argument is the color
of the rectangle's outline.

##### **Ellipses** {#calibre_link-1868 .h2}

The [ellipse(]{.literal}[xy]{.codeitalic}[,]{.literal}
[fill]{.codeitalic}[,]{.literal} [outline]{.codeitalic}[)]{.literal}
method draws an ellipse. If the width and height of the ellipse are
identical, this method will draw a circle. The [xy]{.codeitalic}
argument is a box tuple ([left]{.codeitalic}, [top]{.codeitalic},
[right]{.codeitalic}, [bottom]{.codeitalic}) that represents a box that
precisely contains the ellipse. The optional [fill]{.codeitalic}
argument is the color of the inside of the ellipse, and the optional
[outline]{.codeitalic} argument is the color of the ellipse's outline.

##### **[]{#calibre_link-923 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}Polygons** {#calibre_link-1869 .h2}

The [polygon(]{.literal}[xy]{.codeitalic}[,]{.literal}
[fill]{.codeitalic}[,]{.literal} [outline]{.codeitalic}[)]{.literal}
method draws an arbitrary polygon. The [xy]{.codeitalic} argument is a
list of tuples, such as [\[(x, y), (x, y), \...\]]{.literal}, or
integers, such as [\[x1, y1, x2, y2, \...\]]{.literal}, representing the
connecting points of the polygon's sides. The last pair of coordinates
will be automatically connected to the first pair. The optional
[fill]{.codeitalic} argument is the color of the inside of the polygon,
and the optional [outline]{.codeitalic} argument is the color of the
polygon's outline.

##### **Drawing Example** {#calibre_link-1870 .h2}

Enter the following into the interactive shell:

   \>\>\> [from PIL import Image, ImageDraw]{.codestrong1}\
   \>\>\> [im = Image.new(\'RGBA\', (200, 200),
\'white\')]{.codestrong1}\
   \>\>\> [draw = ImageDraw.Draw(im)]{.codestrong1}\
[➊]{.ent} \>\>\> [draw.line(\[(0, 0), (199, 0), (199, 199), (0, 199),
(0, 0)\], fill=\'black\')]{.codestrong1}\
[➋]{.ent} \>\>\> [draw.rectangle((20, 30, 60, 60),
fill=\'blue\')]{.codestrong1}\
[➌]{.ent} \>\>\> [draw.ellipse((120, 30, 160, 60),
fill=\'red\')]{.codestrong1}\
[➍]{.ent} \>\>\> [draw.polygon(((57, 87), (79, 62), (94, 85), (120, 90),
(103, 113)),]{.codestrong1}\
   [fill=\'brown\')]{.codestrong1}\
[➎]{.ent} \>\>\> [for i in range(100, 200, 10):]{.codestrong1}\
           [draw.line(\[(i, 0), (200, i - 100)\],
fill=\'green\')]{.codestrong1}\
\
   \>\>\> [im.save(\'drawing.png\')]{.codestrong1}

After making an [Image]{.literal} object for a 200×200 white image,
passing it to [ImageDraw.Draw()]{.literal} to get an
[ImageDraw]{.literal} object, and storing the [ImageDraw]{.literal}
object in [draw]{.literal}, you can call drawing methods on
[draw]{.literal}. Here we make a thin, black outline at the edges of the
image [➊]{.ent}, a blue rectangle with its top-left corner at (20, 30)
and bottom-right corner at (60, 60) [➋]{.ent}, a red ellipse defined by
a box from (120, 30) to (160, 60) [➌]{.ent}, a brown polygon with five
points [➍]{.ent}, and a pattern of green lines drawn with a
[for]{.literal} loop [➎]{.ent}. The resulting *drawing.png* file will
look like [Figure 19-14](#calibre_link-1501){.calibre6}.


[]{#calibre_link-1501
.calibre6}![image](../images/000051.jpg){.calibre3}


*Figure 19-14: The resulting* drawing.png *image*

[]{#calibre_link-924 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}There are several other shape-drawing methods for
[ImageDraw]{.literal} objects. The full documentation is available at
*[https://pillow.readthedocs.io/en/latest/reference/ImageDraw.html](https://pillow.readthedocs.io/en/latest/reference/ImageDraw.html){.calibre6}*.

#### ***Drawing Text*** {#calibre_link-627 .h2}

The [ImageDraw]{.literal} object also has a [text()]{.literal} method
for drawing text onto an image. The [text()]{.literal} method takes four
arguments: [xy]{.codeitalic}, [text]{.codeitalic}, [fill]{.codeitalic},
and [font]{.codeitalic}.

-   The [xy]{.codeitalic} argument is a two-integer tuple specifying the
    upper-left corner of the text box.
-   The [text]{.codeitalic} argument is the string of text you want to
    write.
-   The optional [fill]{.codeitalic} argument is the color of the text.
-   The optional [font]{.codeitalic} argument is an
    [ImageFont]{.literal} object, used to set the typeface and size of
    the text. This is described in more detail in the next section.

Since it's often hard to know in advance what size a block of text will
be in a given font, the [ImageDraw]{.literal} module also offers a
[textsize()]{.literal} method. Its first argument is the string of text
you want to measure, and its second argument is an optional
[ImageFont]{.literal} object. The [textsize()]{.literal} method will
then return a two-integer tuple of the width and height that the text in
the given font would be if it were written onto the image. You can use
this width and height to help you calculate exactly where you want to
put the text on your image.

The first three arguments for [text()]{.literal} are straightforward.
Before we use [text()]{.literal} to draw text onto an image, let's look
at the optional fourth argument, the [ImageFont]{.literal} object.

Both [text()]{.literal} and [textsize()]{.literal} take an optional
[ImageFont]{.literal} object as their final arguments. To create one of
these objects, first run the following:

\>\>\> [from PIL import ImageFont]{.codestrong1}

Now that you've imported Pillow's [ImageFont]{.literal} module, you can
call the [ImageFont.truetype()]{.literal} function, which takes two
arguments. The first argument is a string for the font's *TrueType
file*---this is the actual font file that lives on your hard drive. A
TrueType file has the *.ttf* file extension and can usually be found in
the following folders:

-   On Windows: *C:\\Windows\\Fonts*
-   On macOS: */Library/Fonts* and */System/Library/Fonts*
-   On Linux: */usr/share/fonts/truetype*

You don't actually need to enter these paths as part of the TrueType
file string because Python knows to automatically search for fonts in
these directories. But Python will display an error if it is unable to
find the font you specified.

The second argument to [ImageFont.truetype()]{.literal} is an integer
for the font size in *points* (rather than, say, pixels). Keep in mind
that Pillow creates PNG images that are 72 pixels per inch by default,
and a point is 1/72 of an inch.

[]{#calibre_link-921 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Enter the following into the interactive shell,
replacing [FONT_FOLDER]{.literal} with the actual folder name your
operating system uses:

   \>\>\> [from PIL import Image, ImageDraw, ImageFont]{.codestrong1}\
   \>\>\> [import os]{.codestrong1}\
[➊]{.ent} \>\>\> [im = Image.new(\'RGBA\', (200, 200),
\'white\')]{.codestrong1}\
[➋]{.ent} \>\>\> [draw = ImageDraw.Draw(im)]{.codestrong1}\
[➌]{.ent} \>\>\> [draw.text((20, 150), \'Hello\',
fill=\'purple\')]{.codestrong1}\
   \>\>\> [fontsFolder = \'FONT_FOLDER\' \# e.g.
'/Library/Fonts\']{.codestrong1}\
[➍]{.ent} \>\>\> [arialFont =
ImageFont.truetype(os.path.join(fontsFolder, \'arial.ttf\'),
32)]{.codestrong1}\
[➎]{.ent} \>\>\> [draw.text((100, 150), \'Howdy\', fill=\'gray\',
font=arialFont)]{.codestrong1}\
   \>\>\> [im.save(\'text.png\')]{.codestrong1}

After importing [Image]{.literal}, [ImageDraw]{.literal},
[ImageFont]{.literal}, and [os]{.literal}, we make an [Image]{.literal}
object for a new 200×200 white image [➊]{.ent} and make an
[ImageDraw]{.literal} object from the [Image]{.literal} object
[➋]{.ent}. We use [text()]{.literal} to draw *Hello* at (20, 150) in
purple [➌]{.ent}. We didn't pass the optional fourth argument in this
[text()]{.literal} call, so the typeface and size of this text aren't
customized.

To set a typeface and size, we first store the folder name (like
*/Library/Fonts*) in [fontsFolder]{.literal}. Then we call
[ImageFont.truetype()]{.literal}, passing it the *.ttf* file for the
font we want, followed by an integer font size [➍]{.ent}. Store the
[Font]{.literal} object you get from [ImageFont.truetype()]{.literal} in
a variable like [arialFont]{.literal}, and then pass the variable to
[text()]{.literal} in the final keyword argument. The [text()]{.literal}
call at [➎]{.ent} draws *Howdy* at (100, 150) in gray in 32-point Arial.

The resulting *text.png* file will look like [Figure
19-15](#calibre_link-1502){.calibre6}.


[]{#calibre_link-1502
.calibre6}![image](../images/000146.jpg){.calibre3}


*Figure 19-15: The resulting* text.png *image*

### **Summary** {#calibre_link-628 .h1}

Images consist of a collection of pixels, and each pixel has an RGBA
value for its color and its addressable by x- and y-coordinates. Two
common image formats are JPEG and PNG. The [pillow]{.literal} module can
handle both of these image formats and others.

[]{#calibre_link-1871 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}When an image is loaded into an [Image]{.literal}
object, its width and height dimensions are stored as a two-integer
tuple in the [size]{.literal} attribute. Objects of the
[Image]{.literal} data type also have methods for common image
manipulations: [crop()]{.literal}, [copy()]{.literal},
[paste()]{.literal}, [resize()]{.literal}, [rotate()]{.literal}, and
[transpose()]{.literal}. To save the [Image]{.literal} object to an
image file, call the [save()]{.literal} method.

If you want your program to draw shapes onto an image, use
[ImageDraw]{.literal} methods to draw points, lines, rectangles,
ellipses, and polygons. The module also provides methods for drawing
text in a typeface and font size of your choosing.

Although advanced (and expensive) applications such as Photoshop provide
automatic batch processing features, you can use Python scripts to do
many of the same modifications for free. In the previous chapters, you
wrote Python programs to deal with plaintext files, spreadsheets, PDFs,
and other formats. With the [pillow]{.literal} module, you've extended
your programming powers to processing images as well!

### **Practice Questions** {#calibre_link-629 .h1}

[1](#calibre_link-1503){#calibre_link-1464 .calibre6}. What is an RGBA
value?

[2](#calibre_link-1504){#calibre_link-1465 .calibre6}. How can you get
the RGBA value of [\'CornflowerBlue\']{.literal} from the
[Pillow]{.literal} module?

[3](#calibre_link-1505){#calibre_link-1466 .calibre6}. What is a box
tuple?

[4](#calibre_link-1506){#calibre_link-1467 .calibre6}. What function
returns an [Image]{.literal} object for, say, an image file named
*zophie.png*?

[5](#calibre_link-1507){#calibre_link-1468 .calibre6}. How can you find
out the width and height of an [Image]{.literal} object's image?

[6](#calibre_link-1508){#calibre_link-1469 .calibre6}. What method would
you call to get [Image]{.literal} object for a 100×100 image, excluding
the lower-left quarter of it?

[7](#calibre_link-1509){#calibre_link-1470 .calibre6}. After making
changes to an [Image]{.literal} object, how could you save it as an
image file?

[8](#calibre_link-1510){#calibre_link-1471 .calibre6}. What module
contains Pillow's shape-drawing code?

[9](#calibre_link-1511){#calibre_link-1472 .calibre6}. [Image]{.literal}
objects do not have drawing methods. What kind of object does? How do
you get this kind of object?

### **Practice Projects** {#calibre_link-630 .h1}

For practice, write programs that do the following.

#### ***Extending and Fixing the Chapter Project Programs*** {#calibre_link-631 .h2}

The *resizeAndAddLogo.py* program in this chapter works with PNG and
JPEG files, but Pillow supports many more formats than just these two.
Extend *resizeAndAddLogo.py* to process GIF and BMP images as well.

Another small issue is that the program modifies PNG and JPEG files only
if their file extensions are set in lowercase. For example, it will
process []{#calibre_link-1872 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}*zophie.png* but not *zophie.PNG*. Change the code
so that the file extension check is case insensitive.

Finally, the logo added to the bottom-right corner is meant to be just a
small mark, but if the image is about the same size as the logo itself,
the result will look like [Figure 19-16](#calibre_link-1512){.calibre6}.
Modify *resizeAndAddLogo.py* so that the image must be at least twice
the width and height of the logo image before the logo is pasted.
Otherwise, it should skip adding the logo.


[]{#calibre_link-1512
.calibre6}![image](../images/000005.jpg){.calibre3}


*Figure 19-16: When the image isn't much larger than the logo, the
results look ugly.*

#### ***Identifying Photo Folders on the Hard Drive*** {#calibre_link-632 .h2}

I have a bad habit of transferring files from my digital camera to
temporary folders somewhere on the hard drive and then forgetting about
these folders. It would be nice to write a program that could scan the
entire hard drive and find these leftover "photo folders."

Write a program that goes through every folder on your hard drive and
finds potential photo folders. Of course, first you'll have to define
what you consider a "photo folder" to be; let's say that it's any folder
where more than half of the files are photos. And how do you define what
files are photos? First, a photo file must have the file extension
*.png* or *.jpg*. Also, photos are large images; a photo file's width
and height must both be larger than 500 pixels. This is a safe bet,
since most digital camera photos are several thousand pixels in width
and height.

As a hint, here's a rough skeleton of what this program might look like:

#! python3\
\# Import modules and write comments to describe this program.\
\
for foldername, subfolders, filenames in os.walk(\'C:\\\\\'):\
    numPhotoFiles = 0\
    numNonPhotoFiles = 0\
    for filename in filenames:\
        # Check if file extension isn\'t .png or .jpg.\
[]{#calibre_link-1873 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}        if TODO:\
            numNonPhotoFiles += 1\
            continue    # skip to next filename\
\
        # Open image file using Pillow.\
\
        # Check if width & height are larger than 500.\
        if TODO:\
            # Image is large enough to be considered a photo.\
            numPhotoFiles += 1\
        else:\
            # Image is too small to be a photo.\
            numNonPhotoFiles += 1\
\
    # If more than half of files were photos,\
    # print the absolute path of the folder.\
    if TODO:\
        print(TODO)

When the program runs, it should print the absolute path of any photo
folders to the screen.

#### ***Custom Seating Cards*** {#calibre_link-633 .h2}

[Chapter 15](#calibre_link-477){.calibre6} included a practice project
to create custom invitations from a list of guests in a plaintext file.
As an additional project, use the [pillow]{.literal} module to create
images for custom seating cards for your guests. For each of the guests
listed in the *guests.txt* file from the resources at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*,
generate an image file with the guest name and some flowery decoration.
A public domain flower image is also available in the book\'s resources.

To ensure that each seating card is the same size, add a black rectangle
on the edges of the invitation image so that when the image is printed
out, there will be a guideline for cutting. The PNG files that Pillow
produces are set to 72 pixels per inch, so a 4×5-inch card would require
a 288×360-pixel image.

