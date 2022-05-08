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

[![](/images/prev.png)](../chapter3)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter5)

</div>

::: {#calibre_link-707 .calibre}
## []{#calibre_link-1731 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[4]{.big} LISTS** {#calibre_link-152 .h2b}

::: image
![Image](../images/000149.jpg){.calibre3}
:::

One more topic you'll need to understand before you can begin writing
programs in earnest is the list data type and its cousin, the tuple.
Lists and tuples can contain multiple values, which makes writing
programs that handle large amounts of data easier. And since lists
themselves can contain other lists, you can use them to arrange data
into hierarchical structures.

In this chapter, I'll discuss the basics of lists. I'll also teach you
about methods, which are functions that are tied to values of a certain
data type. Then I'll briefly cover the sequence data types (lists,
tuples, and strings) and show how they compare with each other. In the
next chapter, I'll introduce you to the dictionary data type.

### []{#calibre_link-771 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**The List Data Type** {#calibre_link-153 .h1}

A *list* is a value that contains multiple values in an ordered
sequence. The term *list value* refers to the list itself (which is a
value that can be stored in a variable or passed to a function like any
other value), not the values inside the list value. A list value looks
like this: [\[\'cat\', \'bat\', \'rat\', \'elephant\'\]]{.literal}. Just
as string values are typed with quote characters to mark where the
string begins and ends, a list begins with an opening square bracket and
ends with a closing square bracket, [\[\]]{.literal}. Values inside the
list are also called *items*. Items are separated with commas (that is,
they are *comma-delimited*). For example, enter the following into the
interactive shell:

   \>\>\> [\[1, 2, 3\]]{.codestrong1}\
   \[1, 2, 3\]\
   \>\>\> [\[\'cat\', \'bat\', \'rat\', \'elephant\'\]]{.codestrong1}\
   \[\'cat\', \'bat\', \'rat\', \'elephant\'\]\
   \>\>\> [\[\'hello\', 3.1415, True, None, 42\]]{.codestrong1}\
   \[\'hello\', 3.1415, True, None, 42\]\
[➊]{.ent} \>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
   \>\>\> [spam]{.codestrong1}\
   \[\'cat\', \'bat\', \'rat\', \'elephant\'\]

The [spam]{.literal} variable [➊]{.ent} is still assigned only one
value: the list value. But the list value itself contains other values.
The value [\[\]]{.literal} is an empty list that contains no values,
similar to [\'\']{.literal}, the empty string.

#### ***Getting Individual Values in a List with Indexes*** {#calibre_link-154 .h2}

Say you have the list [\[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.literal} stored in a variable named [spam]{.literal}.
The Python code [spam\[0\]]{.literal} would evaluate to
[\'cat\']{.literal}, and [spam\[1\]]{.literal} would evaluate to
[\'bat\']{.literal}, and so on. The integer inside the square brackets
that follows the list is called an *index*. The first value in the list
is at index [0]{.literal}, the second value is at index [1]{.literal},
the third value is at index [2]{.literal}, and so on. [Figure
4-1](#calibre_link-708){.calibre6} shows a list value assigned to
[spam]{.literal}, along with what the index expressions would evaluate
to. Note that because the first index is [0]{.literal}, the last index
is one less than the size of the list; a list of four items has
[3]{.literal} as its last index.

::: image1
[]{#calibre_link-708 .calibre6}![image](../images/000090.jpg){.calibre3}
:::

*Figure 4-1: A list value stored in the variable [spam]{.literal1},
showing which value each index refers to*

For example, enter the following expressions into the interactive shell.
Start by assigning a list to the variable [spam]{.literal}.

   \>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
   \>\>\> [spam\[0\]]{.codestrong1}\
   \'cat\'\
 []{#calibre_link-1008 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}  \>\>\> [spam\[1\]]{.codestrong1}\
   \'bat\'\
   \>\>\> [spam\[2\]]{.codestrong1}\
   \'rat\'\
   \>\>\> [spam\[3\]]{.codestrong1}\
   \'elephant\'\
   \>\>\> [\[\'cat\', \'bat\', \'rat\',
\'elephant\'\]\[3\]]{.codestrong1}\
   \'elephant\'\
[➊]{.ent} \>\>\> [\'Hello, \' + spam\[0\]]{.codestrong1}\
[➋]{.ent} \'Hello, cat\'\
   \>\>\> [\'The \' + spam\[1\] + \' ate the \' + spam\[0\] +
\'.\']{.codestrong1}\
   \'The bat ate the cat.\'

Notice that the expression [\'Hello, \' + spam\[0\]]{.literal} [➊]{.ent}
evaluates to [\'Hello, \' + \'cat\']{.literal} because
[spam\[0\]]{.literal} evaluates to the string [\'cat\']{.literal}. This
expression in turn evaluates to the string value [\'Hello,
cat\']{.literal} [➋]{.ent}.

Python will give you an [IndexError]{.literal} error message if you use
an index that exceeds the number of values in your list value.

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam\[10000\]]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#9\>\", line 1, in \<module\>\
    spam\[10000\]\
IndexError: list index out of range

Indexes can be only integer values, not floats. The following example
will cause a [TypeError]{.literal} error:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam\[1\]]{.codestrong1}\
\'bat\'\
\>\>\> [spam\[1.0\]]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#13\>\", line 1, in \<module\>\
    spam\[1.0\]\
TypeError: list indices must be integers or slices, not float\
\>\>\> [spam\[int(1.0)\]]{.codestrong1}\
\'bat\'

Lists can also contain other list values. The values in these lists of
lists can be accessed using multiple indexes, like so:

\>\>\> [spam = \[\[\'cat\', \'bat\'\], \[10, 20, 30, 40,
50\]\]]{.codestrong1}\
\>\>\> [spam\[0\]]{.codestrong1}\
\[\'cat\', \'bat\'\]\
\>\>\> [spam\[0\]\[1\]]{.codestrong1}\
\'bat\'\
\>\>\> [spam\[1\]\[4\]]{.codestrong1}\
50

[]{#calibre_link-754 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The first index dictates which list value to use,
and the second indicates the value within the list value. For example,
[spam\[0\]\[1\]]{.literal} prints [\'bat\']{.literal}, the second value
in the first list. If you only use one index, the program will print the
full list value at that index.

#### ***Negative Indexes*** {#calibre_link-155 .h2}

While indexes start at [0]{.literal} and go up, you can also use
negative integers for the index. The integer value [-1]{.literal} refers
to the last index in a list, the value [-2]{.literal} refers to the
second-to-last index in a list, and so on. Enter the following into the
interactive shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam\[-1\]]{.codestrong1}\
\'elephant\'\
\>\>\> [spam\[-3\]]{.codestrong1}\
\'bat\'\
\>\>\> [\'The \' + spam\[-1\] + \' is afraid of the \' + spam\[-3\] +
\'.\']{.codestrong1}\
\'The elephant is afraid of the bat.\'

#### ***Getting a List from Another List with Slices*** {#calibre_link-156 .h2}

Just as an index can get a single value from a list, a *slice* can get
several values from a list, in the form of a new list. A slice is typed
between square brackets, like an index, but it has two integers
separated by a colon. Notice the difference between indexes and slices.

-   [spam\[2\]]{.literal} is a list with an index (one integer).
-   [spam\[1:4\]]{.literal} is a list with a slice (two integers).

In a slice, the first integer is the index where the slice starts. The
second integer is the index where the slice ends. A slice goes up to,
but will not include, the value at the second index. A slice evaluates
to a new list value. Enter the following into the interactive shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam\[0:4\]]{.codestrong1}\
\[\'cat\', \'bat\', \'rat\', \'elephant\'\]\
\>\>\> [spam\[1:3\]]{.codestrong1}\
\[\'bat\', \'rat\'\]\
\>\>\> [spam\[0:-1\]]{.codestrong1}\
\[\'cat\', \'bat\', \'rat\'\]

As a shortcut, you can leave out one or both of the indexes on either
side of the colon in the slice. Leaving out the first index is the same
as using [0]{.literal}, or the beginning of the list. Leaving out the
second index is the same as using the length of the list, which will
slice to the end of the list. Enter the following into the interactive
shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam\[:2\]]{.codestrong1}\
[]{#calibre_link-769 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\[\'cat\', \'bat\'\]\
\>\>\> [spam\[1:\]]{.codestrong1}\
\[\'bat\', \'rat\', \'elephant\'\]\
\>\>\> [spam\[:\]]{.codestrong1}\
\[\'cat\', \'bat\', \'rat\', \'elephant\'\]

#### ***Getting a List's Length with the len() Function*** {#calibre_link-157 .h2}

The [len()]{.literal} function will return the number of values that are
in a list value passed to it, just like it can count the number of
characters in a string value. Enter the following into the interactive
shell:

\>\>\> [spam = \[\'cat\', \'dog\', \'moose\'\]]{.codestrong1}\
\>\>\> [len(spam)]{.codestrong1}\
3

#### ***Changing Values in a List with Indexes*** {#calibre_link-158 .h2}

Normally, a variable name goes on the left side of an assignment
statement, like [spam = 42]{.literal}. However, you can also use an
index of a list to change the value at that index. For example,
[spam\[1\] = \'aardvark\']{.literal} means "Assign the value at index
[1]{.literal} in the list [spam]{.literal} to the string
[\'aardvark\']{.literal}." Enter the following into the interactive
shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam\[1\] = \'aardvark\']{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'aardvark\', \'rat\', \'elephant\'\]\
\>\>\> [spam\[2\] = spam\[1\]]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'aardvark\', \'aardvark\', \'elephant\'\]\
\>\>\> [spam\[-1\] = 12345]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'aardvark\', \'aardvark\', 12345\]

#### ***List Concatenation and List Replication*** {#calibre_link-159 .h2}

Lists can be concatenated and replicated just like strings. The
[+]{.literal} operator combines two lists to create a new list value and
the [\*]{.literal} operator can be used with a list and an integer value
to replicate the list. Enter the following into the interactive shell:

\>\>\> [\[1, 2, 3\] + \[\'A\', \'B\', \'C\'\]]{.codestrong1}\
\[1, 2, 3, \'A\', \'B\', \'C\'\]\
\>\>\> [\[\'X\', \'Y\', \'Z\'\] \* 3]{.codestrong1}\
\[\'X\', \'Y\', \'Z\', \'X\', \'Y\', \'Z\', \'X\', \'Y\', \'Z\'\]\
\>\>\> [spam = \[1, 2, 3\]]{.codestrong1}\
\>\>\> [spam = spam + \[\'A\', \'B\', \'C\'\]]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[1, 2, 3, \'A\', \'B\', \'C\'\]

#### []{#calibre_link-907 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Removing Values from Lists with del Statements*** {#calibre_link-160 .h2}

The [del]{.literal} statement will delete values at an index in a list.
All of the values in the list after the deleted value will be moved up
one index. For example, enter the following into the interactive shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [del spam\[2\]]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'bat\', \'elephant\'\]\
\>\>\> [del spam\[2\]]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'bat\'\]

The [del]{.literal} statement can also be used on a simple variable to
delete it, as if it were an "unassignment" statement. If you try to use
the variable after deleting it, you will get a [NameError]{.literal}
error because the variable no longer exists. In practice, you almost
never need to delete simple variables. The [del]{.literal} statement is
mostly used to delete values from lists.

### **Working with Lists** {#calibre_link-161 .h1}

When you first begin writing programs, it's tempting to create many
individual variables to store a group of similar values. For example, if
I wanted to store the names of my cats, I might be tempted to write code
like this:

catName1 = \'Zophie\'\
catName2 = \'Pooka\'\
catName3 = \'Simon\'\
catName4 = \'Lady Macbeth\'\
catName5 = \'Fat-tail\'\
catName6 = \'Miss Cleo\'

It turns out that this is a bad way to write code. (Also, I don't
actually own this many cats, I swear.) For one thing, if the number of
cats changes, your program will never be able to store more cats than
you have variables. These types of programs also have a lot of duplicate
or nearly identical code in them. Consider how much duplicate code is in
the following program, which you should enter into the file editor and
save as *allMyCats1.py*:

print(\'Enter the name of cat 1:\')\
catName1 = input()\
print(\'Enter the name of cat 2:\')\
catName2 = input()\
print(\'Enter the name of cat 3:\')\
catName3 = input()\
print(\'Enter the name of cat 4:\')\
catName4 = input()\
print(\'Enter the name of cat 5:\')\
catName5 = input()\
print(\'Enter the name of cat 6:\')\
[]{#calibre_link-1732 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}catName6 = input()\
print(\'The cat names are:\')\
print(catName1 + \' \' + catName2 + \' \' + catName3 + \' \' +
catName4 + \' \' +\
catName5 + \' \' + catName6)

Instead of using multiple, repetitive variables, you can use a single
variable that contains a list value. For example, here's a new and
improved version of the *allMyCats1.py* program. This new version uses a
single list and can store any number of cats that the user types in. In
a new file editor window, enter the following source code and save it as
*allMyCats2.py*:

catNames = \[\]\
while True:\
    print(\'Enter the name of cat \' + str(len(catNames) + 1) +\
      \' (Or enter nothing to stop.):\')\
    name = input()\
    if name == \'\':\
        break\
    catNames = catNames + \[name\]  # list concatenation\
print(\'The cat names are:\')\
for name in catNames:\
    print(\'  \' + name)

When you run this program, the output will look something like this:

Enter the name of cat 1 (Or enter nothing to stop.):\
[Zophie]{.codestrong1}\
Enter the name of cat 2 (Or enter nothing to stop.):\
[Pooka]{.codestrong1}\
Enter the name of cat 3 (Or enter nothing to stop.):\
[Simon]{.codestrong1}\
Enter the name of cat 4 (Or enter nothing to stop.):\
[Lady Macbeth]{.codestrong1}\
Enter the name of cat 5 (Or enter nothing to stop.):\
[Fat-tail]{.codestrong1}\
Enter the name of cat 6 (Or enter nothing to stop.):\
[Miss Cleo]{.codestrong1}\
Enter the name of cat 7 (Or enter nothing to stop.):\
\
The cat names are:\
  Zophie\
  Pooka\
  Simon\
  Lady Macbeth\
  Fat-tail\
  Miss Cleo

You can view the execution of these programs at
*[https://autbor.com/allmycats1/](https://autbor.com/allmycats1/){.calibre6}*
and
*[https://autbor.com/allmycats2/](https://autbor.com/allmycats2/){.calibre6}*.
The benefit of using a list is that your data is now in a structure, so
your program is much more flexible in processing the data than it would
be with several repetitive variables.

#### []{#calibre_link-710 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Using for Loops with Lists*** {#calibre_link-162 .h2}

In [Chapter 2](#calibre_link-106){.calibre6}, you learned about using
[for]{.literal} loops to execute a block of code a certain number of
times. Technically, a [for]{.literal} loop repeats the code block once
for each item in a list value. For example, if you ran this code:

for i in range(4):\
    print(i)

the output of this program would be as follows:

0\
1\
2\
3

This is because the return value from [range(4)]{.literal} is a sequence
value that Python considers similar to [\[0, 1, 2, 3\]]{.literal}.
(Sequences are described in "[Sequence Data
Types](#calibre_link-175){.calibre6}" on [page
93](#calibre_link-709){.calibre6}.) The following program has the same
output as the previous one:

for i in \[0, 1, 2, 3\]:\
    print(i)

The previous [for]{.literal} loop actually loops through its clause with
the variable [i]{.literal} set to a successive value in the [\[0, 1, 2,
3\]]{.literal} list in each iteration.

A common Python technique is to use
[range(len(]{.literal}[someList]{.codeitalic}[))]{.literal} with a
[for]{.literal} loop to iterate over the indexes of a list. For example,
enter the following into the interactive shell:

\>\>\> [supplies = \[\'pens\', \'staplers\', \'flamethrowers\',
\'binders\'\]]{.codestrong1}\
\>\>\> [for i in range(len(supplies)):]{.codestrong1}\
\...     [print(\'Index \' + str(i) + \' in supplies is: \' +
supplies\[i\])]{.codestrong1}\
\
Index 0 in supplies is: pens\
Index 1 in supplies is: staplers\
Index 2 in supplies is: flamethrowers\
Index 3 in supplies is: binders

Using [range(len(supplies))]{.literal} in the previously shown
[for]{.literal} loop is handy because the code in the loop can access
the index (as the variable [i]{.literal}) and the value at that index
(as [supplies\[i\]]{.literal}). Best of all,
[range(len(supplies))]{.literal} will iterate through all the indexes of
[supplies]{.literal}, no matter how many items it contains.

#### ***The in and not in Operators*** {#calibre_link-163 .h2}

You can determine whether a value is or isn't in a list with the
[in]{.literal} and [not in]{.literal} operators. Like other operators,
[in]{.literal} and [not in]{.literal} are used in expressions and
connect two values: a value to look for in a list and the list where it
may be []{#calibre_link-820 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}found. These expressions will evaluate to a Boolean
value. Enter the following into the interactive shell:

\>\>\> [\'howdy\' in \[\'hello\', \'hi\', \'howdy\',
\'heyas\'\]]{.codestrong1}\
True\
\>\>\> [spam = \[\'hello\', \'hi\', \'howdy\',
\'heyas\'\]]{.codestrong1}\
\>\>\> [\'cat\' in spam]{.codestrong1}\
False\
\>\>\> [\'howdy\' not in spam]{.codestrong1}\
False\
\>\>\> [\'cat\' not in spam]{.codestrong1}\
True

For example, the following program lets the user type in a pet name and
then checks to see whether the name is in a list of pets. Open a new
file editor window, enter the following code, and save it as
*myPets.py*:

myPets = \[\'Zophie\', \'Pooka\', \'Fat-tail\'\]\
print(\'Enter a pet name:\')\
name = input()\
if name not in myPets:\
    print(\'I do not have a pet named \' + name)\
else:\
    print(name + \' is my pet.\')

The output may look something like this:

Enter a pet name:\
[Footfoot]{.codestrong1}\
I do not have a pet named Footfoot

You can view the execution of this program at
*[https://autbor.com/mypets/](https://autbor.com/mypets/){.calibre6}*.

#### ***The Multiple Assignment Trick*** {#calibre_link-164 .h2}

The *multiple assignment trick* (technically called *tuple unpacking*)
is a shortcut that lets you assign multiple variables with the values in
a list in one line of code. So instead of doing this:

\>\>\> [cat = \[\'fat\', \'gray\', \'loud\'\]]{.codestrong1}\
\>\>\> [size = cat\[0\]]{.codestrong1}\
\>\>\> [color = cat\[1\]]{.codestrong1}\
\>\>\> [disposition = cat\[2\]]{.codestrong1}

you could type this line of code:

\>\>\> [cat = \[\'fat\', \'gray\', \'loud\'\]]{.codestrong1}\
\>\>\> [size, color, disposition = cat]{.codestrong1}

[]{#calibre_link-935 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The number of variables and the length of the list
must be exactly equal, or Python will give you a [ValueError]{.literal}:

\>\>\> [cat = \[\'fat\', \'gray\', \'loud\'\]]{.codestrong1}\
\>\>\> [size, color, disposition, name = cat]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#84\>\", line 1, in \<module\>\
    size, color, disposition, name = cat\
ValueError: not enough values to unpack (expected 4, got 3)

#### ***Using the enumerate() Function with Lists*** {#calibre_link-165 .h2}

Instead of using the
[range(len(]{.literal}[someList]{.codeitalic}[))]{.literal} technique
with a [for]{.literal} loop to obtain the integer index of the items in
the list, you can call the [enumerate()]{.literal} function instead. On
each iteration of the loop, [enumerate()]{.literal} will return two
values: the index of the item in the list, and the item in the list
itself. For example, this code is equivalent to the code in the "[Using
for Loops with Lists](#calibre_link-162){.calibre6}" on [page
84](#calibre_link-710){.calibre6}:

\>\>\> [supplies = \[\'pens\', \'staplers\', \'flamethrowers\',
\'binders\'\]]{.codestrong1}\
\>\>\> [for index, item in enumerate(supplies):]{.codestrong1}\
\...     [print(\'Index \' + str(index) + \' in supplies is: \' +
item)]{.codestrong1}\
\
Index 0 in supplies is: pens\
Index 1 in supplies is: staplers\
Index 2 in supplies is: flamethrowers\
Index 3 in supplies is: binders

The [enumerate()]{.literal} function is useful if you need both the item
and the item's index in the loop's block.

#### ***Using the random.choice() and random.shuffle() Functions with Lists*** {#calibre_link-166 .h2}

The [random]{.literal} module has a couple functions that accept lists
for arguments. The [random.choice()]{.literal} function will return a
randomly selected item from the list. Enter the following into the
interactive shell:

\>\>\> [import random]{.codestrong1}\
\>\>\> [pets = \[\'Dog\', \'Cat\', \'Moose\'\]]{.codestrong1}\
\>\>\> [random.choice(pets)]{.codestrong1}\
\'Dog\'\
\>\>\> [random.choice(pets)]{.codestrong1}\
\'Cat\'\
\>\>\> [random.choice(pets)]{.codestrong1}\
\'Cat\'

You can consider [random.choice(someList)]{.literal} to be a shorter
form of [someList\[random.randint(0, len(someList) -- 1\]]{.literal}.

[]{#calibre_link-741 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [random.shuffle()]{.literal} function will
reorder the items in a list. This function modifies the list in place,
rather than returning a new list. Enter the following into the
interactive shell:

\>\>\> [import random]{.codestrong1}\
\>\>\> [people = \[\'Alice\', \'Bob\', \'Carol\',
\'David\'\]]{.codestrong1}\
\>\>\> [random.shuffle(people)]{.codestrong1}\
\>\>\> [people]{.codestrong1}\
\[\'Carol\', \'David\', \'Alice\', \'Bob\'\]\
\>\>\> [random.shuffle(people)]{.codestrong1}\
\>\>\> [people]{.codestrong1}\
\[\'Alice\', \'David\', \'Bob\', \'Carol\'\]

### **Augmented Assignment Operators** {#calibre_link-167 .h1}

When assigning a value to a variable, you will frequently use the
variable itself. For example, after assigning [42]{.literal} to the
variable [spam]{.literal}, you would increase the value in
[spam]{.literal} by [1]{.literal} with the following code:

\>\>\> [spam = 42]{.codestrong1}\
\>\>\> [spam = spam + 1]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
43

As a shortcut, you can use the augmented assignment operator
[+=]{.literal} to do the same thing:

\>\>\> [spam = 42]{.codestrong1}\
\>\>\> [spam += 1]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
43

There are augmented assignment operators for the [+]{.literal},
[-]{.literal}, [\*]{.literal}, [/]{.literal}, and [%]{.literal}
operators, described in [Table 4-1](#calibre_link-711){.calibre6}.

**Table 4-1:** The Augmented Assignment Operators

  **Augmented assignment statement**   **Equivalent assignment statement**
  ------------------------------------ -------------------------------------
  [spam += 1]{.literal}                [spam = spam + 1]{.literal}
  [spam -= 1]{.literal}                [spam = spam - 1]{.literal}
  [spam \*= 1]{.literal}               [spam = spam \* 1]{.literal}
  [spam /= 1]{.literal}                [spam = spam / 1]{.literal}
  [spam %= 1]{.literal}                [spam = spam % 1]{.literal}

[]{#calibre_link-1009 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [+=]{.literal} operator can also do string and
list concatenation, and the [\*=]{.literal} operator can do string and
list replication. Enter the following into the interactive shell:

\>\>\> [spam = \'Hello,\']{.codestrong1}\
\>\>\> [spam += \' world!\']{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\'Hello world!\'\
\>\>\> [bacon = \[\'Zophie\'\]]{.codestrong1}\
\>\>\> [bacon \*= 3]{.codestrong1}\
\>\>\> [bacon]{.codestrong1}\
\[\'Zophie\', \'Zophie\', \'Zophie\'\]

### **Methods** {#calibre_link-168 .h1}

A *method* is the same thing as a function, except it is "called on" a
value. For example, if a list value were stored in [spam]{.literal}, you
would call the [index()]{.literal} list method (which I'll explain
shortly) on that list like so: [spam.index(\'hello\')]{.literal}. The
method part comes after the value, separated by a period.

Each data type has its own set of methods. The list data type, for
example, has several useful methods for finding, adding, removing, and
otherwise manipulating values in a list.

#### ***Finding a Value in a List with the index() Method*** {#calibre_link-169 .h2}

List values have an [index()]{.literal} method that can be passed a
value, and if that value exists in the list, the index of the value is
returned. If the value isn't in the list, then Python produces a
[ValueError]{.literal} error. Enter the following into the interactive
shell:

\>\>\> [spam = \[\'hello\', \'hi\', \'howdy\',
\'heyas\'\]]{.codestrong1}\
\>\>\> [spam.index(\'hello\')]{.codestrong1}\
0\
\>\>\> [spam.index(\'heyas\')]{.codestrong1}\
3\
\>\>\> [spam.index(\'howdy howdy howdy\')]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#31\>\", line 1, in \<module\>\
    spam.index(\'howdy howdy howdy\')\
ValueError: \'howdy howdy howdy\' is not in list

When there are duplicates of the value in the list, the index of its
first appearance is returned. Enter the following into the interactive
shell, and notice that [index()]{.literal} returns [1]{.literal}, not
[3]{.literal}:

\>\>\> [spam = \[\'Zophie\', \'Pooka\', \'Fat-tail\',
\'Pooka\'\]]{.codestrong1}\
\>\>\> [spam.index(\'Pooka\')]{.codestrong1}\
1

#### []{#calibre_link-794 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Adding Values to Lists with the append() and insert() Methods*** {#calibre_link-170 .h2}

To add new values to a list, use the [append()]{.literal} and
[insert()]{.literal} methods. Enter the following into the interactive
shell to call the [append()]{.literal} method on a list value stored in
the variable [spam]{.literal}:

\>\>\> [spam = \[\'cat\', \'dog\', \'bat\'\]]{.codestrong1}\
\>\>\> [spam.append(\'moose\')]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'dog\', \'bat\', \'moose\'\]

The previous [append()]{.literal} method call adds the argument to the
end of the list. The [insert()]{.literal} method can insert a value at
any index in the list. The first argument to [insert()]{.literal} is the
index for the new value, and the second argument is the new value to be
inserted. Enter the following into the interactive shell:

\>\>\> [spam = \[\'cat\', \'dog\', \'bat\'\]]{.codestrong1}\
\>\>\> [spam.insert(1, \'chicken\']{.codestrong1})\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'chicken\', \'dog\', \'bat\'\]

Notice that the code is [spam.append(\'moose\')]{.literal} and
[spam.insert(1, \'chicken\')]{.literal}, not [spam =
spam.append(\'moose\')]{.literal} and [spam = spam.insert(1,
\'chicken\')]{.literal}. Neither [append()]{.literal} nor
[insert()]{.literal} gives the new value of [spam]{.literal} as its
return value. (In fact, the return value of [append()]{.literal} and
[insert()]{.literal} is [None]{.literal}, so you definitely wouldn't
want to store this as the new variable value.) Rather, the list is
modified *in place*. Modifying a list in place is covered in more detail
later in "[Mutable and Immutable Data
Types](#calibre_link-176){.calibre6}" on [page
94](#calibre_link-712){.calibre6}.

Methods belong to a single data type. The [append()]{.literal} and
[insert()]{.literal} methods are list methods and can be called only on
list values, not on other values such as strings or integers. Enter the
following into the interactive shell, and note the
[AttributeError]{.literal} error messages that show up:

\>\>\> [eggs = \'hello\']{.codestrong1}\
\>\>\> [eggs.append(\'world\')]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#19\>\", line 1, in \<module\>\
    eggs.append(\'world\')\
AttributeError: \'str\' object has no attribute \'append\'\
\>\>\> [bacon = 42]{.codestrong1}\
\>\>\> [bacon.insert(1, \'world\')]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#22\>\", line 1, in \<module\>\
    bacon.insert(1, \'world\')\
AttributeError: \'int\' object has no attribute \'insert\'

#### []{#calibre_link-908 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Removing Values from Lists with the remove() Method*** {#calibre_link-171 .h2}

The [remove()]{.literal} method is passed the value to be removed from
the list it is called on. Enter the following into the interactive
shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam.remove(\'bat\')]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'cat\', \'rat\', \'elephant\'\]

Attempting to delete a value that does not exist in the list will result
in a [ValueError]{.literal} error. For example, enter the following into
the interactive shell and notice the error that is displayed:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\',
\'elephant\'\]]{.codestrong1}\
\>\>\> [spam.remove(\'chicken\')]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#11\>\", line 1, in \<module\>\
    spam.remove(\'chicken\')\
ValueError: list.remove(x): x not in list

If the value appears multiple times in the list, only the first instance
of the value will be removed. Enter the following into the interactive
shell:

\>\>\> [spam = \[\'cat\', \'bat\', \'rat\', \'cat\', \'hat\',
\'cat\'\]]{.codestrong1}\
\>\>\> [spam.remove(\'cat\')]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'bat\', \'rat\', \'cat\', \'hat\', \'cat\'\]

The [del]{.literal} statement is good to use when you know the index of
the value you want to remove from the list. The [remove()]{.literal}
method is useful when you know the value you want to remove from the
list.

#### ***Sorting the Values in a List with the sort() Method*** {#calibre_link-172 .h2}

Lists of number values or lists of strings can be sorted with the
[sort()]{.literal} method. For example, enter the following into the
interactive shell:

\>\>\> [spam = \[2, 5, 3.14, 1, -7\]]{.codestrong1}\
\>\>\> [spam.sort()]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[-7, 1, 2, 3.14, 5\]\
\>\>\> [spam = \[\'ants\', \'cats\', \'dogs\', \'badgers\',
\'elephants\'\]]{.codestrong1}\
\>\>\> [spam.sort()]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'ants\', \'badgers\', \'cats\', \'dogs\', \'elephants\'\]

[]{#calibre_link-798 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can also pass [True]{.literal} for the
[reverse]{.literal} keyword argument to have [sort()]{.literal} sort the
values in reverse order. Enter the following into the interactive shell:

\>\>\> [spam.sort(reverse=True)]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'elephants\', \'dogs\', \'cats\', \'badgers\', \'ants\'\]

There are three things you should note about the [sort()]{.literal}
method. First, the [sort()]{.literal} method sorts the list in place;
don't try to capture the return value by writing code like [spam =
spam.sort()]{.literal}.

Second, you cannot sort lists that have both number values *and* string
values in them, since Python doesn't know how to compare these values.
Enter the following into the interactive shell and notice the
[TypeError]{.literal} error:

\>\>\> [spam = \[1, 3, 2, 4, \'Alice\', \'Bob\'\]]{.codestrong1}\
\>\>\> [spam.sort()]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#70\>\", line 1, in \<module\>\
    spam.sort()\
TypeError: \'\<\' not supported between instances of \'str\' and \'int\'

Third, [sort()]{.literal} uses "ASCIIbetical order" rather than actual
alphabetical order for sorting strings. This means uppercase letters
come before lowercase letters. Therefore, the lowercase *a* is sorted so
that it comes *after* the uppercase *Z*. For an example, enter the
following into the interactive shell:

\>\>\> [spam = \[\'Alice\', \'ants\', \'Bob\', \'badgers\', \'Carol\',
\'cats\'\]]{.codestrong1}\
\>\>\> [spam.sort()]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'Alice\', \'Bob\', \'Carol\', \'ants\', \'badgers\', \'cats\'\]

If you need to sort the values in regular alphabetical order, pass
[str.lower]{.literal} for the [key]{.literal} keyword argument in the
[sort()]{.literal} method call.

\>\>\> [spam = \[\'a\', \'z\', \'A\', \'Z\'\]]{.codestrong1}\
\>\>\> [spam.sort(key=str.lower)]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'a\', \'A\', \'z\', \'Z\'\]

This causes the [sort()]{.literal} function to treat all the items in
the list as if they were lowercase without actually changing the values
in the list.

#### ***Reversing the Values in a List with the reverse() Method*** {#calibre_link-173 .h2}

If you need to quickly reverse the order of the items in a list, you can
call the [reverse()]{.literal} list method. Enter the following into the
interactive shell:

\>\>\> [spam = \[\'cat\', \'dog\', \'moose\'\]]{.codestrong1}\
\>\>\> [spam.reverse()]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'moose\', \'dog\', \'cat\'\]

::: sidebar
**EXCEPTIONS TO INDENTATION RULES IN PYTHON**

In most cases, the amount of indentation for a line of code tells Python
what block it is in. There are some exceptions to this rule, however.
For example, lists can actually span several lines in the source code
file. The indentation of these lines does not matter; Python knows that
the list is not finished until it sees the ending square bracket. For
example, you can have code that looks like this:

spam = \[\'apples\',\
    \'oranges\',\
                    \'bananas\',\
\'cats\'\]\
print(spam)

Of course, practically speaking, most people use Python's behavior to
make their lists look pretty and readable, like the messages list in the
Magic 8 Ball program.

You can also split up a single instruction across multiple lines using
the [\\]{.literal1} *line continuation character* at the end. Think of
[\\]{.literal1} as saying, "This instruction continues on the next
line." The indentation on the line after a [\\]{.literal1} line
continuation is not significant. For example, the following is valid
Python code:

print(\'Four score and seven \' + \\\
      \'years ago\...\')

These tricks are useful when you want to rearrange long lines of Python
code to be a bit more readable.
:::

[]{#calibre_link-744 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Like the [sort()]{.literal} list method,
[reverse()]{.literal} doesn't return a list. This is why you write
[spam.reverse()]{.literal}, instead of [spam =
spam.reverse()]{.literal}.

### **Example Program: Magic 8 Ball with a List** {#calibre_link-174 .h1}

Using lists, you can write a much more elegant version of the previous
chapter's Magic 8 Ball program. Instead of several lines of nearly
identical [elif]{.literal} statements, you can create a single list that
the code works with. Open a new file editor window and enter the
following code. Save it as *magic8Ball2.py*.

import random\
\
messages = \[\'It is certain\',\
    \'It is decidedly so\',\
    \'Yes definitely\',\
    \'Reply hazy try again\',\
    \'Ask again later\',\
    \'Concentrate and ask again\',\
    \'My reply is no\',\
[]{#calibre_link-709 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    \'Outlook not so good\',\
    \'Very doubtful\'\]\
\
print(messages\[random.randint(0, len(messages) - 1)\])

You can view the execution of this program at
*[https://autbor.com/magic8ball2/](https://autbor.com/magic8ball2/){.calibre6}*.

When you run this program, you'll see that it works the same as the
previous *magic8Ball.py* program.

Notice the expression you use as the index for [messages]{.literal}:
[random.randint (0, len(messages) - 1)]{.literal}. This produces a
random number to use for the index, regardless of the size of
[messages]{.literal}. That is, you'll get a random number between
[0]{.literal} and the value of [len(messages) - 1]{.literal}. The
benefit of this approach is that you can easily add and remove strings
to the [messages]{.literal} list without changing other lines of code.
If you later update your code, there will be fewer lines you have to
change and fewer chances for you to introduce bugs.

### **Sequence Data Types** {#calibre_link-175 .h1}

Lists aren't the only data types that represent ordered sequences of
values. For example, strings and lists are actually similar if you
consider a string to be a "list" of single text characters. The Python
sequence data types include lists, strings, range objects returned by
[range()]{.literal}, and tuples (explained in the "[The Tuple Data
Type](#calibre_link-177){.calibre6}" on [page
96](#calibre_link-713){.calibre6}). Many of the things you can do with
lists can also be done with strings and other values of sequence types:
indexing; slicing; and using them with [for]{.literal} loops, with
[len()]{.literal}, and with the [in]{.literal} and [not in]{.literal}
operators. To see this, enter the following into the interactive shell:

\>\>\> [name = \'Zophie\']{.codestrong1}\
\>\>\> [name\[0\]]{.codestrong1}\
\'Z\'\
\>\>\> [name\[-2\]]{.codestrong1}\
\'i\'\
\>\>\> [name\[0:4\]]{.codestrong1}\
\'Zoph\'\
\>\>\> [\'Zo\' in name]{.codestrong1}\
True\
\>\>\> [\'z\' in name]{.codestrong1}\
False\
\>\>\> [\'p\' not in name]{.codestrong1}\
False\
\>\>\> [for i in name:]{.codestrong1}\
\...     [print(\'\* \* \* \' + i + \' \* \* \*\')]{.codestrong1}\
\
\* \* \* Z \* \* \*\
\* \* \* o \* \* \*\
\* \* \* p \* \* \*\
\* \* \* h \* \* \*\
\* \* \* i \* \* \*\
\* \* \* e \* \* \*

#### []{#calibre_link-712 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Mutable and Immutable Data Types*** {#calibre_link-176 .h2}

But lists and strings are different in an important way. A list value is
a *mutable* data type: it can have values added, removed, or changed.
However, a string is *immutable*: it cannot be changed. Trying to
reassign a single character in a string results in a
[TypeError]{.literal} error, as you can see by entering the following
into the interactive shell:

\>\>\> [name = \'Zophie a cat\']{.codestrong1}\
\>\>\> [name\[7\] = \'the\']{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#50\>\", line 1, in \<module\>\
    name\[7\] = \'the\'\
TypeError: \'str\' object does not support item assignment

The proper way to "mutate" a string is to use slicing and concatenation
to build a *new* string by copying from parts of the old string. Enter
the following into the interactive shell:

\>\>\> [name = \'Zophie a cat\']{.codestrong1}\
\>\>\> [newName = name\[0:7\] + \'the\' + name\[8:12\]]{.codestrong1}\
\>\>\> [name]{.codestrong1}\
\'Zophie a cat\'\
\>\>\> [newName]{.codestrong1}\
\'Zophie the cat\'

We used [\[0:7\]]{.literal} and [\[8:12\]]{.literal} to refer to the
characters that we don't wish to replace. Notice that the original
[\'Zophie a cat\']{.literal} string is not modified, because strings are
immutable.

Although a list value *is* mutable, the second line in the following
code does not modify the list [eggs]{.literal}:

\>\>\> [eggs = \[1, 2, 3\]]{.codestrong1}\
\>\>\> [eggs = \[4, 5, 6\]]{.codestrong1}\
\>\>\> [eggs]{.codestrong1}\
\[4, 5, 6\]

The list value in [eggs]{.literal} isn't being changed here; rather, an
entirely new and different list value ([\[4, 5, 6\]]{.literal}) is
overwriting the old list value ([\[1, 2, 3\]]{.literal}). This is
depicted in [Figure 4-2](#calibre_link-714){.calibre6}.

If you wanted to actually modify the original list in [eggs]{.literal}
to contain [\[4, 5, 6\]]{.literal}, you would have to do something like
this:

\>\>\> [eggs = \[1, 2, 3\]]{.codestrong1}\
\>\>\> [del eggs\[2\]]{.codestrong1}\
\>\>\> [del eggs\[1\]]{.codestrong1}\
\>\>\> [del eggs\[0\]]{.codestrong1}\
\>\>\> [eggs.append(4)]{.codestrong1}\
\>\>\> [eggs.append(5)]{.codestrong1}\
\>\>\> [eggs.append(6)]{.codestrong1}\
\>\>\> [eggs]{.codestrong1}\
\[4, 5, 6\]

::: image1
[]{#calibre_link-886 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-714
.calibre6}![image](../images/000037.jpg){.calibre3}
:::

*Figure 4-2: When [eggs = \[4, 5, 6\]]{.literal1} is executed, the
contents of [eggs]{.literal1} are replaced with a new list value.*

In the first example, the list value that [eggs]{.literal} ends up with
is the same list value it started with. It's just that this list has
been changed, rather than overwritten. [Figure
4-3](#calibre_link-715){.calibre6} depicts the seven changes made by the
first seven lines in the previous interactive shell example.

::: image1
[]{#calibre_link-715 .calibre6}![image](../images/000128.jpg){.calibre3}
:::

*Figure 4-3: The [del]{.literal1} statement and the
[append()]{.literal1} method modify the same list value in place.*

Changing a value of a mutable data type (like what the [del]{.literal}
statement and [append()]{.literal} method do in the previous example)
changes the value in place, since the variable's value is not replaced
with a new list value.

Mutable versus immutable types may seem like a meaningless distinction,
but "[Passing References](#calibre_link-181){.calibre6}" on [page
100](#calibre_link-716){.calibre6} will explain the different behavior
when calling functions with mutable arguments versus immutable
arguments. But first, let's find out about the tuple data type, which is
an immutable form of the list data type.

#### []{#calibre_link-713 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***The Tuple Data Type*** {#calibre_link-177 .h2}

The *tuple* data type is almost identical to the list data type, except
in two ways. First, tuples are typed with parentheses, [(]{.literal} and
[)]{.literal}, instead of square brackets, [\[]{.literal} and
[\]]{.literal}. For example, enter the following into the interactive
shell:

\>\>\> [eggs = (\'hello\', 42, 0.5)]{.codestrong1}\
\>\>\> [eggs\[0\]]{.codestrong1}\
\'hello\'\
\>\>\> [eggs\[1:3\]]{.codestrong1}\
(42, 0.5)\
\>\>\> [len(eggs)]{.codestrong1}\
3

But the main way that tuples are different from lists is that tuples,
like strings, are immutable. Tuples cannot have their values modified,
appended, or removed. Enter the following into the interactive shell,
and look at the [TypeError]{.literal} error message:

\>\>\> [eggs = (\'hello\', 42, 0.5)]{.codestrong1}\
\>\>\> [eggs\[1\] = 99]{.codestrong1}\
Traceback (most recent call last):\
  File \"\<pyshell#5\>\", line 1, in \<module\>\
    eggs\[1\] = 99\
TypeError: \'tuple\' object does not support item assignment

If you have only one value in your tuple, you can indicate this by
placing a trailing comma after the value inside the parentheses.
Otherwise, Python will think you've just typed a value inside regular
parentheses. The comma is what lets Python know this is a tuple value.
(Unlike some other programming languages, it's fine to have a trailing
comma after the last item in a list or tuple in Python.) Enter the
following [type()]{.literal} function calls into the interactive shell
to see the distinction:

\>\>\> [type((\'hello\',))]{.codestrong1}\
\<class \'tuple\'\>\
\>\>\> [type((\'hello\'))]{.codestrong1}\
\<class \'str\'\>

You can use tuples to convey to anyone reading your code that you don't
intend for that sequence of values to change. If you need an ordered
sequence of values that never changes, use a tuple. A second benefit of
using tuples instead of lists is that, because they are immutable and
their contents don't change, Python can implement some optimizations
that make code using tuples slightly faster than code using lists.

#### []{#calibre_link-1032 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Converting Types with the list() and tuple() Functions*** {#calibre_link-178 .h2}

Just like how [str(42)]{.literal} will return [\'42\']{.literal}, the
string representation of the integer [42]{.literal}, the functions
[list()]{.literal} and [tuple()]{.literal} will return list and tuple
versions of the values passed to them. Enter the following into the
interactive shell, and notice that the return value is of a different
data type than the value passed:

\>\>\> [tuple(\[\'cat\', \'dog\', 5\])]{.codestrong1}\
(\'cat\', \'dog\', 5)\
\>\>\> [list((\'cat\', \'dog\', 5))]{.codestrong1}\
\[\'cat\', \'dog\', 5\]\
\>\>\> [list(\'hello\')]{.codestrong1}\
\[\'h\', \'e\', \'l\', \'l\', \'o\'\]

Converting a tuple to a list is handy if you need a mutable version of a
tuple value.

### **References** {#calibre_link-179 .h1}

As you've seen, variables "store" strings and integer values. However,
this explanation is a simplification of what Python is actually doing.
Technically, variables are storing references to the computer memory
locations where the values are stored. Enter the following into the
interactive shell:

\>\>\> [spam = 42]{.codestrong1}\
\>\>\> [cheese = spam]{.codestrong1}\
\>\>\> [spam = 100]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
100\
\>\>\> [cheese]{.codestrong1}\
42

When you assign [42]{.literal} to the [spam]{.literal} variable, you are
actually creating the [42]{.literal} value in the computer's memory and
storing a *reference* to it in the spam variable. When you copy the
value in [spam]{.literal} and assign it to the variable
[cheese]{.literal}, you are actually copying the reference. Both the
[spam]{.literal} and [cheese]{.literal} variables refer to the
[42]{.literal} value in the computer's memory. When you later change the
value in [spam]{.literal} to [100]{.literal}, you're creating a new
[100]{.literal} value and storing a reference to it in [spam]{.literal}.
This doesn't affect the value in [cheese]{.literal}. Integers are
*immutable* values that don't change; changing the *spam* variable is
actually making it refer to a completely different value in memory.

But lists don't work this way, because list values can change; that is,
lists are *mutable*. Here is some code that will make this distinction
easier to understand. Enter this into the interactive shell:

[➊]{.ent} \>\>\> [spam = \[0, 1, 2, 3, 4, 5\]]{.codestrong1}\
[➋]{.ent} \>\>\> [cheese = spam]{.codestrong1} \# The reference is being
copied, not the list.\
[➌]{.ent} \>\>\> [cheese\[1\] = \'Hello!\']{.codestrong1} \# This
changes the list value.\
   \>\>\> [spam]{.codestrong1}\
   []{#calibre_link-1733 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\[0, \'Hello!\', 2, 3, 4, 5\]\
   \>\>\> [cheese]{.codestrong1} \# The cheese variable refers to the
same list.\
   \[0, \'Hello!\', 2, 3, 4, 5\]

This might look odd to you. The code touched only the [cheese]{.literal}
list, but it seems that both the [cheese]{.literal} and [spam]{.literal}
lists have changed.

When you create the list [➊]{.ent}, you assign a reference to it in the
[spam]{.literal} variable. But the next line [➋]{.ent} copies only the
list reference in [spam]{.literal} to [cheese]{.literal}, not the list
value itself. This means the values stored in [spam]{.literal} and
[cheese]{.literal} now both refer to the same list. There is only one
underlying list because the list itself was never actually copied. So
when you modify the first element of [cheese]{.literal} [➌]{.ent}, you
are modifying the same list that [spam]{.literal} refers to.

Remember that variables are like boxes that contain values. The previous
figures in this chapter show that lists in boxes aren't exactly
accurate, because list variables don't actually contain lists---they
contain *references* to lists. (These references will have ID numbers
that Python uses internally, but you can ignore them.) Using boxes as a
metaphor for variables, [Figure 4-4](#calibre_link-717){.calibre6} shows
what happens when a list is assigned to the [spam]{.literal} variable.

::: image1
[]{#calibre_link-717 .calibre6}![image](../images/000041.jpg){.calibre3}
:::

*Figure 4-4: [spam = \[0, 1, 2, 3, 4, 5\]]{.literal1} stores a reference
to a list, not the actual list.*

Then, in [Figure 4-5](#calibre_link-718){.calibre6}, the reference in
[spam]{.literal} is copied to [cheese]{.literal}. Only a new reference
was created and stored in [cheese]{.literal}, not a new list. Note how
both references refer to the same list.

::: image1
[]{#calibre_link-718 .calibre6}![image](../images/000132.jpg){.calibre3}
:::

*Figure 4-5: [spam = cheese]{.literal1} copies the reference, not the
list.*

[]{#calibre_link-1734 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}When you alter the list that [cheese]{.literal}
refers to, the list that [spam]{.literal} refers to is also changed,
because both [cheese]{.literal} and [spam]{.literal} refer to the same
list. You can see this in [Figure 4-6](#calibre_link-719){.calibre6}.

::: image1
[]{#calibre_link-719 .calibre6}![image](../images/000077.jpg){.calibre3}
:::

*Figure 4-6: [cheese\[1\] = \'Hello!\']{.literal1} modifies the list
that both variables refer to.*

Although Python variables technically contain references to values,
people often casually say that the variable contains the value.

#### ***Identity and the id() Function*** {#calibre_link-180 .h2}

You may be wondering why the weird behavior with mutable lists in the
previous section doesn't happen with immutable values like integers or
strings. We can use Python's [id()]{.literal} function to understand
this. All values in Python have a unique identity that can be obtained
with the [id()]{.literal} function. Enter the following into the
interactive shell:

\>\>\> [id(\'Howdy\')]{.codestrong1} \# The returned number will be
different on your machine.\
44491136

When Python runs [id(\'Howdy\')]{.literal}, it creates the
[\'Howdy\']{.literal} string in the computer's memory. The numeric
memory address where the string is stored is returned by the
[id()]{.literal} function. Python picks this address based on which
memory bytes happen to be free on your computer at the time, so it'll be
different each time you run this code.

Like all strings, [\'Howdy\']{.literal} is immutable and cannot be
changed. If you "change" the string in a variable, a new string object
is being made at a different place in memory, and the variable refers to
this new string. For example, enter the following into the interactive
shell and see how the identity of the string referred to by
[bacon]{.literal} changes:

\>\>\> [bacon = \'Hello\']{.codestrong1}\
\>\>\> [id(bacon)]{.codestrong1}\
44491136\
\>\>\> [bacon += \' world!\']{.codestrong1} \# A new string is made from
\'Hello\' and \' world!\'.\
\>\>\> [id(bacon)]{.codestrong1} \# bacon now refers to a completely
different string.\
44609712

[]{#calibre_link-716 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}However, lists can be modified because they are
mutable objects. The [append()]{.literal} method doesn't create a new
list object; it changes the existing list object. We call this
"modifying the object *in-place.*"

\>\>\> [eggs = \[\'cat\', \'dog\'\]]{.codestrong1} \# This creates a new
list.\
\>\>\> [id(eggs)]{.codestrong1}\
35152584\
\>\>\> [eggs.append(\'moose\')]{.codestrong1} \# append() modifies the
list \"in place\".\
\>\>\> [id(eggs)]{.codestrong1} \# eggs still refers to the same list as
before.\
35152584\
\>\>\> [eggs = \[\'bat\', \'rat\', \'cow\'\]]{.codestrong1} \# This
creates a new list, which has a new\
identity.\
\>\>\> [id(eggs)]{.codestrong1} \# eggs now refers to a completely
different list.\
44409800

If two variables refer to the same list (like [spam]{.literal} and
[cheese]{.literal} in the previous section) and the list value itself
changes, both variables are affected because they both refer to the same
list. The [append()]{.literal}, [extend()]{.literal},
[remove()]{.literal}, [sort()]{.literal}, [reverse()]{.literal}, and
other list methods modify their lists in place.

Python's *automatic garbage collector* deletes any values not being
referred to by any variables to free up memory. You don't need to worry
about how the garbage collector works, which is a good thing: manual
memory management in other programming languages is a common source of
bugs.

#### ***Passing References*** {#calibre_link-181 .h2}

References are particularly important for understanding how arguments
get passed to functions. When a function is called, the values of the
arguments are copied to the parameter variables. For lists (and
dictionaries, which I'll describe in the next chapter), this means a
copy of the reference is used for the parameter. To see the consequences
of this, open a new file editor window, enter the following code, and
save it as *passingReference.py*:

def eggs(someParameter):\
    someParameter.append(\'Hello\')\
\
spam = \[1, 2, 3\]\
eggs(spam)\
print(spam)

Notice that when [eggs()]{.literal} is called, a return value is not
used to assign a new value to [spam]{.literal}. Instead, it modifies the
list in place, directly. When run, this program produces the following
output:

\[1, 2, 3, \'Hello\'\]

Even though [spam]{.literal} and [someParameter]{.literal} contain
separate references, they both refer to the same list. This is why the
[append(\'Hello\')]{.literal} method call inside the function affects
the list even after the function call has returned.

[]{#calibre_link-868 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Keep this behavior in mind: forgetting that Python
handles list and dictionary variables this way can lead to confusing
bugs.

#### ***The copy Module's copy() and deepcopy() Functions*** {#calibre_link-182 .h2}

Although passing around references is often the handiest way to deal
with lists and dictionaries, if the function modifies the list or
dictionary that is passed, you may not want these changes in the
original list or dictionary value. For this, Python provides a module
named [copy]{.literal} that provides both the [copy()]{.literal} and
[deepcopy()]{.literal} functions. The first of these,
[copy.copy()]{.literal}, can be used to make a duplicate copy of a
mutable value like a list or dictionary, not just a copy of a reference.
Enter the following into the interactive shell:

\>\>\> [import copy]{.codestrong1}\
\>\>\> [spam = \[\'A\', \'B\', \'C\', \'D\'\]]{.codestrong1}\
\>\>\> [id(spam)]{.codestrong1}\
44684232\
\>\>\> [cheese = copy.copy(spam)]{.codestrong1}\
\>\>\> [id(cheese)]{.codestrong1} \# cheese is a different list with
different identity.\
44685832\
\>\>\> [cheese\[1\] = 42]{.codestrong1}\
\>\>\> [spam]{.codestrong1}\
\[\'A\', \'B\', \'C\', \'D\'\]\
\>\>\> [cheese]{.codestrong1}\
\[\'A\', 42, \'C\', \'D\'\]

Now the [spam]{.literal} and [cheese]{.literal} variables refer to
separate lists, which is why only the list in [cheese]{.literal} is
modified when you assign [42]{.literal} at index [1]{.literal}. As you
can see in [Figure 4-7](#calibre_link-720){.calibre6}, the reference ID
numbers are no longer the same for both variables because the variables
refer to independent lists.

::: image1
[]{#calibre_link-720 .calibre6}![image](../images/000025.jpg){.calibre3}
:::

*Figure 4-7: [cheese = copy.copy(spam)]{.literal1} creates a second list
that can be modified independently of the first.*

If the list you need to copy contains lists, then use the
[copy.deepcopy()]{.literal} function instead of [copy.copy()]{.literal}.
The [deepcopy()]{.literal} function will copy these inner lists as well.

### []{#calibre_link-865 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**A Short Program: Conway's Game of Life** {#calibre_link-183 .h1}

Conway's Game of Life is an example of *cellular automata*: a set of
rules governing the behavior of a field made up of discrete cells. In
practice, it creates a pretty animation to look at. You can draw out
each step on graph paper, using the squares as cells. A filled-in square
will be "alive" and an empty square will be "dead." If a living square
has two or three living neighbors, it continues to live on the next
step. If a dead square has exactly three living neighbors, it comes
alive on the next step. Every other square dies or remains dead on the
next step. You can see an example of the progression of steps in [Figure
4-8](#calibre_link-721){.calibre6}.

::: image1
[]{#calibre_link-721 .calibre6}![image](../images/000117.jpg){.calibre3}
:::

*Figure 4-8: Four steps in a Conway's Game of Life simulation*

Even though the rules are simple, there are many surprising behaviors
that emerge. Patterns in Conway's Game of Life can move, self-replicate,
or even mimic CPUs. But at the foundation of all of this complex,
advanced behavior is a rather simple program.

We can use a list of lists to represent the two-dimensional field. The
inner list represents each column of squares and stores a
[\'#\']{.literal} hash string for living squares and a [\' \']{.literal}
space string for dead squares. Type the following source code into the
file editor, and save the file as *conway.py*. It's fine if you don't
quite understand how all of the code works; just enter it and follow
along with comments and explanations provided here as close as you can:

\# Conway\'s Game of Life\
import random, time, copy\
WIDTH = 60\
HEIGHT = 20\
\
\# Create a list of list for the cells:\
nextCells = \[\]\
for x in range(WIDTH):\
    column = \[\] \# Create a new column.\
    for y in range(HEIGHT):\
        if random.randint(0, 1) == 0:\
            column.append(\'#\') \# Add a living cell.\
        else:\
            column.append(\' \') \# Add a dead cell.\
    nextCells.append(column) \# nextCells is a list of column lists.\
\
while True: \# Main program loop.\
    print(\'\\n\\n\\n\\n\\n\') \# Separate each step with newlines.\
    currentCells = copy.deepcopy(nextCells)\
\
[]{#calibre_link-1735 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    # Print currentCells on the screen:\
    for y in range(HEIGHT):\
        for x in range(WIDTH):\
            print(currentCells\[x\]\[y\], end=\'\') \# Print the \# or
space.\
        print() \# Print a newline at the end of the row.\
\
    # Calculate the next step\'s cells based on current step\'s cells:\
    for x in range(WIDTH):\
        for y in range(HEIGHT):\
            # Get neighboring coordinates:\
            # \`% WIDTH\` ensures leftCoord is always between 0 and
WIDTH - 1\
            leftCoord  = (x - 1) % WIDTH\
            rightCoord = (x + 1) % WIDTH\
            aboveCoord = (y - 1) % HEIGHT\
            belowCoord = (y + 1) % HEIGHT\
\
            # Count number of living neighbors:\
            numNeighbors = 0\
            if currentCells\[leftCoord\]\[aboveCoord\] == \'#\':\
                numNeighbors += 1 \# Top-left neighbor is alive.\
            if currentCells\[x\]\[aboveCoord\] == \'#\':\
                numNeighbors += 1 \# Top neighbor is alive.\
            if currentCells\[rightCoord\]\[aboveCoord\] == \'#\':\
                numNeighbors += 1 \# Top-right neighbor is alive.\
            if currentCells\[leftCoord\]\[y\] == \'#\':\
                numNeighbors += 1 \# Left neighbor is alive.\
            if currentCells\[rightCoord\]\[y\] == \'#\':\
                numNeighbors += 1 \# Right neighbor is alive.\
            if currentCells\[leftCoord\]\[belowCoord\] == \'#\':\
                numNeighbors += 1 \# Bottom-left neighbor is alive.\
            if currentCells\[x\]\[belowCoord\] == \'#\':\
                numNeighbors += 1 \# Bottom neighbor is alive.\
            if currentCells\[rightCoord\]\[belowCoord\] == \'#\':\
                numNeighbors += 1 \# Bottom-right neighbor is alive.\
\
            # Set cell based on Conway\'s Game of Life rules:\
            if currentCells\[x\]\[y\] == \'#\' and (numNeighbors == 2
or\
numNeighbors == 3):\
                # Living cells with 2 or 3 neighbors stay alive:\
                nextCells\[x\]\[y\] = \'#\'\
            elif currentCells\[x\]\[y\] == \' \' and numNeighbors == 3:\
                # Dead cells with 3 neighbors become alive:\
                nextCells\[x\]\[y\] = \'#\'\
            else:\
                # Everything else dies or stays dead:\
                nextCells\[x\]\[y\] = \' \'\
    time.sleep(1) \# Add a 1-second pause to reduce flickering.

Let's look at this code line by line, starting at the top.

\# Conway\'s Game of Life\
import random, time, copy\
WIDTH = 60\
HEIGHT = 20

[]{#calibre_link-1736 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}First we import modules that contain functions
we'll need, namely the [random.randint()]{.literal},
[time.sleep()]{.literal}, and [copy.deepcopy()]{.literal} functions.

\# Create a list of list for the cells:\
nextCells = \[\]\
for x in range(WIDTH):\
    column = \[\] \# Create a new column.\
    for y in range(HEIGHT):\
        if random.randint(0, 1) == 0:\
            column.append(\'#\') \# Add a living cell.\
        else:\
            column.append(\' \') \# Add a dead cell.\
    nextCells.append(column) \# nextCells is a list of column lists.

The very first step of our cellular automata will be completely random.
We need to create a list of lists data structure to store the
[\'#\']{.literal} and [\' \']{.literal} strings that represent a living
or dead cell, and their place in the list of lists reflects their
position on the screen. The inner lists each represent a column of
cells. The [random.randint(0, 1)]{.literal} call gives an even 50/50
chance between the cell starting off alive or dead.

We put this list of lists in a variable called [nextCells]{.literal},
because the first step in our main program loop will be to copy
[nextCells]{.literal} into [currentCells]{.literal}. For our list of
lists data structure, the x-coordinates start at 0 on the left and
increase going right, while the y-coordinates start at 0 at the top and
increase going down. So [nextCells\[0\]\[0\]]{.literal} will represent
the cell at the top left of the screen, while
[nextCells\[1\]\[0\]]{.literal} represents the cell to the right of that
cell and [nextCells\[0\]\[1\]]{.literal} represents the cell beneath it.

while True: \# Main program loop.\
    print(\'\\n\\n\\n\\n\\n\') \# Separate each step with newlines.\
    currentCells = copy.deepcopy(nextCells)

Each iteration of our main program loop will be a single step of our
cellular automata. On each step, we'll copy [nextCells]{.literal} to
[currentCells]{.literal}, print [currentCells]{.literal} on the screen,
and then use the cells in [currentCells]{.literal} to calculate the
cells in [nextCells]{.literal}.

    # Print currentCells on the screen:\
    for y in range(HEIGHT):\
        for x in range(WIDTH):\
            print(currentCells\[x\]\[y\], end=\'\') \# Print the \# or
space.\
        print() \# Print a newline at the end of the row.

These nested [for]{.literal} loops ensure that we print a full row of
cells to the screen, followed by a newline character at the end of the
row. We repeat this for each row in [nextCells]{.literal}.

    # Calculate the next step\'s cells based on current step\'s cells:\
    for x in range(WIDTH):\
        for y in range(HEIGHT):\
            # Get neighboring coordinates:\
[]{#calibre_link-1737 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}            # \`% WIDTH\` ensures leftCoord is
always between 0 and WIDTH - 1\
            leftCoord  = (x - 1) % WIDTH\
            rightCoord = (x + 1) % WIDTH\
            aboveCoord = (y - 1) % HEIGHT\
            belowCoord = (y + 1) % HEIGHT

Next, we need to use two nested [for]{.literal} loops to calculate each
cell for the next step. The living or dead state of the cell depends on
the neighbors, so let's first calculate the index of the cells to the
left, right, above, and below the current x- and y-coordinates.

The [%]{.literal} mod operator performs a "wraparound." The left
neighbor of a cell in the leftmost column [0]{.literal} would be [0 -
1]{.literal} or [-1]{.literal}. To wrap this around to the rightmost
column's index, [59]{.literal}, we calculate [(0 - 1) %
WIDTH]{.literal}. Since [WIDTH]{.literal} is [60]{.literal}, this
expression evaluates to [59]{.literal}. This mod-wraparound technique
works for the right, above, and below neighbors as well.

            # Count number of living neighbors:\
            numNeighbors = 0\
            if currentCells\[leftCoord\]\[aboveCoord\] == \'#\':\
                numNeighbors += 1 \# Top-left neighbor is alive.\
            if currentCells\[x\]\[aboveCoord\] == \'#\':\
                numNeighbors += 1 \# Top neighbor is alive.\
            if currentCells\[rightCoord\]\[aboveCoord\] == \'#\':\
                numNeighbors += 1 \# Top-right neighbor is alive.\
            if currentCells\[leftCoord\]\[y\] == \'#\':\
                numNeighbors += 1 \# Left neighbor is alive.\
            if currentCells\[rightCoord\]\[y\] == \'#\':\
                numNeighbors += 1 \# Right neighbor is alive.\
            if currentCells\[leftCoord\]\[belowCoord\] == \'#\':\
                numNeighbors += 1 \# Bottom-left neighbor is alive.\
            if currentCells\[x\]\[belowCoord\] == \'#\':\
                numNeighbors += 1 \# Bottom neighbor is alive.\
            if currentCells\[rightCoord\]\[belowCoord\] == \'#\':\
                numNeighbors += 1 \# Bottom-right neighbor is alive.

To decide if the cell at [nextCells\[x\]\[y\]]{.literal} should be
living or dead, we need to count the number of living neighbors
[currentCells\[x\]\[y\]]{.literal} has. This series of [if]{.literal}
statements checks each of the eight neighbors of this cell, and adds
[1]{.literal} to [numNeighbors]{.literal} for each living one.

            # Set cell based on Conway\'s Game of Life rules:\
            if currentCells\[x\]\[y\] == \'#\' and (numNeighbors == 2
or\
numNeighbors == 3):\
                # Living cells with 2 or 3 neighbors stay alive:\
                nextCells\[x\]\[y\] = \'#\'\
            elif currentCells\[x\]\[y\] == \' \' and numNeighbors == 3:\
                # Dead cells with 3 neighbors become alive:\
                nextCells\[x\]\[y\] = \'#\'\
            else:\
                # Everything else dies or stays dead:\
                nextCells\[x\]\[y\] = \' \'\
    time.sleep(1) \# Add a 1-second pause to reduce flickering.

[]{#calibre_link-866 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Now that we know the number of living neighbors for
the cell at [currentCells\[x\]\[y\]]{.literal}, we can set
[nextCells\[x\]\[y\]]{.literal} to either [\'#\']{.literal} or [\'
\']{.literal}. After we loop over every possible x- and y-coordinate,
the program takes a 1-second pause by calling [time.sleep(1)]{.literal}.
Then the program execution goes back to the start of the main program
loop to continue with the next step.

Several patterns have been discovered with names such as "glider,"
"propeller," or "heavyweight spaceship." The glider pattern, pictured in
[Figure 4-8](#calibre_link-721){.calibre6}, results in a pattern that
"moves" diagonally every four steps. You can create a single glider by
replacing this line in our *conway.py* program:

        if random.randint(0, 1) == 0:

with this line:

        if (x, y) in ((1, 0), (2, 1), (0, 2), (1, 2), (2, 2)):

You can find out more about the intriguing devices made using Conway's
Game of Life by searching the web. And you can find other short,
text-based Python programs like this one at
*[https://github.com/asweigart/pythonstdiogames](https://github.com/asweigart/pythonstdiogames){.calibre6}*.

### **Summary** {#calibre_link-184 .h1}

Lists are useful data types since they allow you to write code that
works on a modifiable number of values in a single variable. Later in
this book, you will see programs using lists to do things that would be
difficult or impossible to do without them.

Lists are a sequence data type that is mutable, meaning that their
contents can change. Tuples and strings, though also sequence data
types, are immutable and cannot be changed. A variable that contains a
tuple or string value can be overwritten with a new tuple or string
value, but this is not the same thing as modifying the existing value in
place---like, say, the [append()]{.literal} or [remove()]{.literal}
methods do on lists.

Variables do not store list values directly; they store *references* to
lists. This is an important distinction when you are copying variables
or passing lists as arguments in function calls. Because the value that
is being copied is the list reference, be aware that any changes you
make to the list might impact another variable in your program. You can
use [copy()]{.literal} or [deepcopy()]{.literal} if you want to make
changes to a list in one variable without modifying the original list.

### **Practice Questions** {#calibre_link-185 .h1}

[1](#calibre_link-722){#calibre_link-1288 .calibre6}. What is
[\[\]]{.literal}?

[2](#calibre_link-723){#calibre_link-1289 .calibre6}. How would you
assign the value [\'hello\']{.literal} as the third value in a list
stored in a variable named [spam]{.literal}? (Assume [spam]{.literal}
contains [\[2, 4, 6, 8, 10\]]{.literal}.)

[]{#calibre_link-1738 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}For the following three questions, let's say
[spam]{.literal} contains the list [\[\'a\', \'b\', \'c\',
\'d\'\]]{.literal}.

[3](#calibre_link-724){#calibre_link-1290 .calibre6}. What does
[spam\[int(int(\'3\' \* 2) // 11)\]]{.literal} evaluate to?

[4](#calibre_link-725){#calibre_link-1291 .calibre6}. What does
[spam\[-1\]]{.literal} evaluate to?

[5](#calibre_link-726){#calibre_link-1292 .calibre6}. What does
[spam\[:2\]]{.literal} evaluate to?

For the following three questions, let's say [bacon]{.literal} contains
the list [\[3.14, \'cat\', 11, \'cat\', True\]]{.literal}.

[6](#calibre_link-727){#calibre_link-1293 .calibre6}. What does
[bacon.index(\'cat\')]{.literal} evaluate to?

[7](#calibre_link-728){#calibre_link-1294 .calibre6}. What does
[bacon.append(99)]{.literal} make the list value in [bacon]{.literal}
look like?

[8](#calibre_link-729){#calibre_link-1295 .calibre6}. What does
[bacon.remove(\'cat\')]{.literal} make the list value in
[bacon]{.literal} look like?

[9](#calibre_link-730){#calibre_link-1296 .calibre6}. What are the
operators for list concatenation and list replication?

[10](#calibre_link-731){#calibre_link-1297 .calibre6}. What is the
difference between the [append()]{.literal} and [insert()]{.literal}
list methods?

[11](#calibre_link-732){#calibre_link-1298 .calibre6}. What are two ways
to remove values from a list?

[12](#calibre_link-733){#calibre_link-1299 .calibre6}. Name a few ways
that list values are similar to string values.

[13](#calibre_link-734){#calibre_link-1300 .calibre6}. What is the
difference between lists and tuples?

[14](#calibre_link-735){#calibre_link-1301 .calibre6}. How do you type
the tuple value that has just the integer value [42]{.literal} in it?

[15](#calibre_link-736){#calibre_link-1302 .calibre6}. How can you get
the tuple form of a list value? How can you get the list form of a tuple
value?

[16](#calibre_link-737){#calibre_link-1303 .calibre6}. Variables that
"contain" list values don't actually contain lists directly. What do
they contain instead?

[17](#calibre_link-738){#calibre_link-1304 .calibre6}. What is the
difference between [copy.copy()]{.literal} and
[copy.deepcopy()]{.literal}?

### **Practice Projects** {#calibre_link-186 .h1}

For practice, write programs to do the following tasks.

#### ***Comma Code*** {#calibre_link-187 .h2}

Say you have a list value like this:

spam = \[\'apples\', \'bananas\', \'tofu\', \'cats\'\]

Write a function that takes a list value as an argument and returns a
string with all the items separated by a comma and a space, with *and*
inserted before the last item. For example, passing the previous
[spam]{.literal} list to the function would return [\'apples, bananas,
tofu, and cats\']{.literal}. But your function should be able to work
with any list value passed to it. Be sure to test the case where an
empty list [\[\]]{.literal} is passed to your function.

#### ***Coin Flip Streaks*** {#calibre_link-188 .h2}

For this exercise, we'll try doing an experiment. If you flip a coin 100
times and write down an "H" for each heads and "T" for each tails,
you'll create a list that looks like "T T T T H H H H T T." If you ask a
human to make []{#calibre_link-1739 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}up 100 random coin flips, you'll probably end up
with alternating head-tail results like "H T H T H H T H T T," which
looks random (to humans), but isn't mathematically random. A human will
almost never write down a streak of six heads or six tails in a row,
even though it is highly likely to happen in truly random coin flips.
Humans are predictably bad at being random.

Write a program to find out how often a streak of six heads or a streak
of six tails comes up in a randomly generated list of heads and tails.
Your program breaks up the experiment into two parts: the first part
generates a list of randomly selected [\'heads\']{.literal} and
[\'tails\']{.literal} values, and the second part checks if there is a
streak in it. Put all of this code in a loop that repeats the experiment
10,000 times so we can find out what percentage of the coin flips
contains a streak of six heads or tails in a row. As a hint, the
function call [random.randint(0, 1)]{.literal} will return a
[0]{.literal} value 50% of the time and a [1]{.literal} value the other
50% of the time.

You can start with the following template:

import random\
numberOfStreaks = 0\
for experimentNumber in range(10000):\
    # Code that creates a list of 100 \'heads\' or \'tails\' values.\
\
    # Code that checks if there is a streak of 6 heads or tails in a
row.\
print(\'Chance of streak: %s%%\' % (numberOfStreaks / 100))

Of course, this is only an estimate, but 10,000 is a decent sample size.
Some knowledge of mathematics could give you the exact answer and save
you the trouble of writing a program, but programmers are notoriously
bad at math.

#### ***Character Picture Grid*** {#calibre_link-189 .h2}

Say you have a list of lists where each value in the inner lists is a
one-character string, like this:

grid = \[\[\'.\', \'.\', \'.\', \'.\', \'.\', \'.\'\],\
        \[\'.\', \'O\', \'O\', \'.\', \'.\', \'.\'\],\
        \[\'O\', \'O\', \'O\', \'O\', \'.\', \'.\'\],\
        \[\'O\', \'O\', \'O\', \'O\', \'O\', \'.\'\],\
        \[\'.\', \'O\', \'O\', \'O\', \'O\', \'O\'\],\
        \[\'O\', \'O\', \'O\', \'O\', \'O\', \'.\'\],\
        \[\'O\', \'O\', \'O\', \'O\', \'.\', \'.\'\],\
        \[\'.\', \'O\', \'O\', \'.\', \'.\', \'.\'\],\
        \[\'.\', \'.\', \'.\', \'.\', \'.\', \'.\'\]\]

Think of [grid\[x\]\[y\]]{.literal} as being the character at the x- and
y-coordinates of a "picture" drawn with text characters. The [(0,
0)]{.literal} origin is in the upper-left corner, the x-coordinates
increase going right, and the y-coordinates increase going down.

[]{#calibre_link-1740 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Copy the previous grid value, and write code that
uses it to print the image.

..OO.OO..\
.OOOOOOO.\
.OOOOOOO.\
..OOOOO..\
\...OOO\...\
\....O\....

Hint: You will need to use a loop in a loop in order to print
[grid\[0\]\[0\]]{.literal}, then [grid\[1\]\[0\]]{.literal}, then
[grid\[2\]\[0\]]{.literal}, and so on, up to [grid\[8\]\[0\]]{.literal}.
This will finish the first row, so then print a newline. Then your
program should print [grid\[0\]\[1\]]{.literal}, then
[grid\[1\]\[1\]]{.literal}, then [grid\[2\]\[1\]]{.literal}, and so on.
The last thing your program will print is [grid\[8\]\[5\]]{.literal}.

Also, remember to pass the [end]{.literal} keyword argument to
[print()]{.literal} if you don't want a newline printed automatically
after each [print()]{.literal} call.[]{#calibre_link-1741 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}
:::

<div>

[![](/images/prev.png)](../chapter3)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter5)

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
