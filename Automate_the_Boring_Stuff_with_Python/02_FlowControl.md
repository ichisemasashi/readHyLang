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

[![](/images/prev.png)](../chapter1)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter3)

</div>

::: {#calibre_link-1532 .calibre}
## []{#calibre_link-1711 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[2]{.big} FLOW CONTROL** {#calibre_link-106 .h2b}

::: image
![Image](../images/000137.jpg){.calibre3}
:::

So, you know the basics of individual instructions and that a program is
just a series of instructions. But programming's real strength isn't
just running one instruction after another like a weekend errand list.
Based on how expressions evaluate, a program can decide to skip
instructions, repeat them, or choose one of several instructions to run.
In fact, you almost never want your programs to start from the first
line of code and simply execute every line, straight to the end. *Flow
control statements* can decide which Python instructions to execute
under which conditions.

These flow control statements directly correspond to the symbols in a
flowchart, so I'll provide flowchart versions of the code discussed in
this chapter. [Figure 2-1](#calibre_link-1533){.calibre6} shows a
flowchart for what to do if it's raining. Follow the path made by the
arrows from Start to End.

::: image1
[]{#calibre_link-819 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1533
.calibre6}![image](../images/000039.jpg){.calibre3}
:::

*Figure 2-1: A flowchart to tell you what to do if it is raining*

In a flowchart, there is usually more than one way to go from the start
to the end. The same is true for lines of code in a computer program.
Flowcharts represent these branching points with diamonds, while the
other steps are represented with rectangles. The starting and ending
steps are represented with rounded rectangles.

But before you learn about flow control statements, you first need to
learn how to represent those *yes* and *no* options, and you need to
understand how to write those branching points as Python code. To that
end, let's explore Boolean values, comparison operators, and Boolean
operators.

### **Boolean Values** {#calibre_link-107 .h1}

While the integer, floating-point, and string data types have an
unlimited number of possible values, the *Boolean* data type has only
two values: [True]{.literal} and [False]{.literal}. (Boolean is
capitalized because the data type is named after mathematician George
Boole.) When entered as Python code, the Boolean values [True]{.literal}
and [False]{.literal} lack the quotes you place around strings, and they
always start with a capital *T* or *F*, with the rest of the word in
lowercase. Enter the following into the interactive shell. (Some of
these instructions are intentionally incorrect, and they'll cause error
messages to appear.)

[]{#calibre_link-758 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[➊]{.ent} \>\>\> [spam = True]{.codestrong1}\
   \>\>\> [spam]{.codestrong1}\
   True\
[➋]{.ent} \>\>\> [true]{.codestrong1}\
   Traceback (most recent call last):\
     File \"\<pyshell#2\>\", line 1, in \<module\>\
       true\
   NameError: name \'true\' is not defined\
[➌]{.ent} \>\>\> [True = 2 + 2]{.codestrong1}\
   SyntaxError: can\'t assign to keyword

Like any other value, Boolean values are used in expressions and can be
stored in variables [➊]{.ent}. If you don't use the proper case
[➋]{.ent} or you try to use [True]{.literal} and [False]{.literal} for
variable names [➌]{.ent}, Python will give you an error message.

### **Comparison Operators** {#calibre_link-108 .h1}

*Comparison operators*, also called *relational operators*, compare two
values and evaluate down to a single Boolean value. [Table
2-1](#calibre_link-1534){.calibre6} lists the comparison operators.

**Table 2-1:** Comparison Operators

  **Operator**      **Meaning**
  ----------------- --------------------------
  [==]{.literal}    Equal to
  [!=]{.literal}    Not equal to
  [\<]{.literal}    Less than
  [\>]{.literal}    Greater than
  [\<=]{.literal}   Less than or equal to
  [\>=]{.literal}   Greater than or equal to

These operators evaluate to [True]{.literal} or [False]{.literal}
depending on the values you give them. Let's try some operators now,
starting with [==]{.literal} and [!=]{.literal}.

\>\>\> [42 == 42]{.codestrong1}\
True\
\>\>\> [42 == 99]{.codestrong1}\
False\
\>\>\> [2 != 3]{.codestrong1}\
True\
\>\>\> [2 != 2]{.codestrong1}\
False

As you might expect, [==]{.literal} (equal to) evaluates to
[True]{.literal} when the values on both sides are the same, and
[!=]{.literal} (not equal to) evaluates to [True]{.literal} when the two
values are different. The [==]{.literal} and [!=]{.literal} operators
can actually work with values of any data type.

[]{#calibre_link-759 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}   \>\>\> [\'hello\' == \'hello\']{.codestrong1}\
   True\
   \>\>\> [\'hello\' == \'Hello\']{.codestrong1}\
   False\
   \>\>\> [\'dog\' != \'cat\']{.codestrong1}\
   True\
   \>\>\> [True == True]{.codestrong1}\
   True\
   \>\>\> [True != False]{.codestrong1}\
   True\
   \>\>\> [42 == 42.0]{.codestrong1}\
   True\
[➊]{.ent} \>\>\> [42 == \'42\']{.codestrong1}\
   False

Note that an integer or floating-point value will always be unequal to a
string value. The expression [42 == \'42\']{.literal} [➊]{.ent}
evaluates to [False]{.literal} because Python considers the integer
[42]{.literal} to be different from the string [\'42\']{.literal}.

The [\<]{.literal}, [\>]{.literal}, [\<=]{.literal}, and [\>=]{.literal}
operators, on the other hand, work properly only with integer and
floating-point values.

   \>\>\> [42 \< 100]{.codestrong1}\
   True\
   \>\>\> [42 \> 100]{.codestrong1}\
   False\
   \>\>\> [42 \< 42]{.codestrong1}\
   False\
   \>\>\> [eggCount = 42]{.codestrong1}\
[➊]{.ent} \>\>\> [eggCount \<= 42]{.codestrong1}\
   True\
   \>\>\> [myAge = 29]{.codestrong1}\
[➋]{.ent} \>\>\> [myAge \>= 10]{.codestrong1}\
   True

::: sidebar
**THE DIFFERENCE BETWEEN THE == AND = OPERATORS**

You might have noticed that the [==]{.literal1} operator (equal to) has
two equal signs, while the [=]{.literal1} operator (assignment) has just
one equal sign. It's easy to confuse these two operators with each
other. Just remember these points:

-   The [==]{.literal} operator (equal to) asks whether two values are
    the same as each other.
-   The [=]{.literal} operator (assignment) puts the value on the right
    into the variable on the left.

To help remember which is which, notice that the [==]{.literal1}
operator (equal to) consists of two characters, just like the
[!=]{.literal1} operator (not equal to) consists of two characters.
:::

[]{#calibre_link-790 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You'll often use comparison operators to compare a
variable's value to some other value, like in the [eggCount \<=
42]{.literal} [➊]{.ent} and [myAge \>= 10]{.literal} [➋]{.ent} examples.
(After all, instead of entering [\'dog\' != \'cat\']{.literal} in your
code, you could have just entered [True]{.literal}.) You'll see more
examples of this later when you learn about flow control statements.

### **Boolean Operators** {#calibre_link-109 .h1}

The three Boolean operators ([and]{.literal}, [or]{.literal}, and
[not]{.literal}) are used to compare Boolean values. Like comparison
operators, they evaluate these expressions down to a Boolean value.
Let's explore these operators in detail, starting with the
[and]{.literal} operator.

#### ***Binary Boolean Operators*** {#calibre_link-110 .h2}

The [and]{.literal} and [or]{.literal} operators always take two Boolean
values (or expressions), so they're considered *binary* operators. The
[and]{.literal} operator evaluates an expression to [True]{.literal} if
*both* Boolean values are [True]{.literal}; otherwise, it evaluates to
[False]{.literal}. Enter some expressions using [and]{.literal} into the
interactive shell to see it in action.

\>\>\> [True and True]{.codestrong1}\
True\
\>\>\> [True and False]{.codestrong1}\
False

A *truth table* shows every possible result of a Boolean operator.
[Table 2-2](#calibre_link-1535){.calibre6} is the truth table for the
[and]{.literal} operator.

**Table 2-2:** The [and]{.literal1} Operator's Truth Table

  **Expression**                **Evaluates to . . .**
  ----------------------------- ------------------------
  [True and True]{.literal}     [True]{.literal}
  [True and False]{.literal}    [False]{.literal}
  [False and True]{.literal}    [False]{.literal}
  [False and False]{.literal}   [False]{.literal}

On the other hand, the [or]{.literal} operator evaluates an expression
to [True]{.literal} if *either* of the two Boolean values is
[True]{.literal}. If both are [False]{.literal}, it evaluates to
[False]{.literal}.

\>\>\> [False or True]{.codestrong1}\
True\
\>\>\> [False or False]{.codestrong1}\
False

You can see every possible outcome of the [or]{.literal} operator in its
truth table, shown in [Table 2-3](#calibre_link-1536){.calibre6}.

[]{#calibre_link-815 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}**Table 2-3:** The [or]{.literal1} Operator's Truth
Table

  **Expression**               **Evaluates to . . .**
  ---------------------------- ------------------------
  [True or True]{.literal}     [True]{.literal}
  [True or False]{.literal}    [True]{.literal}
  [False or True]{.literal}    [True]{.literal}
  [False or False]{.literal}   [False]{.literal}

#### ***The not Operator*** {#calibre_link-111 .h2}

Unlike [and]{.literal} and [or]{.literal}, the [not]{.literal} operator
operates on only one Boolean value (or expression). This makes it a
*unary* operator. The [not]{.literal} operator simply evaluates to the
opposite Boolean value.

   \>\>\> [not True]{.codestrong1}\
   False\
[➊]{.ent} \>\>\> [not not not not True]{.codestrong1}\
   True

Much like using double negatives in speech and writing, you can nest
[not]{.literal} operators [➊]{.ent}, though there's never not no reason
to do this in real programs. [Table 2-4](#calibre_link-1537){.calibre6}
shows the truth table for [not]{.literal}.

**Table 2-4:** The [not]{.literal1} Operator's Truth Table

  **Expression**          **Evaluates to . . .**
  ----------------------- ------------------------
  [not True]{.literal}    [False]{.literal}
  [not False]{.literal}   [True]{.literal}

### **Mixing Boolean and Comparison Operators** {#calibre_link-112 .h1}

Since the comparison operators evaluate to Boolean values, you can use
them in expressions with the Boolean operators.

Recall that the [and]{.literal}, [or]{.literal}, and [not]{.literal}
operators are called Boolean operators because they always operate on
the Boolean values [True]{.literal} and [False]{.literal}. While
expressions like [4 \< 5]{.literal} aren't Boolean values, they are
expressions that evaluate down to Boolean values. Try entering some
Boolean expressions that use comparison operators into the interactive
shell.

\>\>\> [(]{.codestrong1}[4 \< 5) and (5 \< 6)]{.codestrong1}\
True\
\>\>\> [(]{.codestrong1}[4 \< 5) and (9 \< 6)]{.codestrong1}\
False\
\>\>\> [(]{.codestrong1}[1 == 2) or (2 == 2)]{.codestrong1}\
True

The computer will evaluate the left expression first, and then it will
evaluate the right expression. When it knows the Boolean value for each,
[]{#calibre_link-816 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}it will then evaluate the whole expression down to
one Boolean value. You can think of the computer's evaluation process
for [(4 \< 5) and (5 \< 6)]{.literal} as the following:

::: imagec
![image](../images/000095.jpg){.calibre3}
:::

You can also use multiple Boolean operators in an expression, along with
the comparison operators:

\>\>\> [2 + 2 == 4 and not 2 + 2 == 5 and 2 \* 2 == 2 +
2]{.codestrong1}\
True

The Boolean operators have an order of operations just like the math
operators do. After any math and comparison operators evaluate, Python
evaluates the [not]{.literal} operators first, then the [and]{.literal}
operators, and then the [or]{.literal} operators.

### **Elements of Flow Control** {#calibre_link-113 .h1}

Flow control statements often start with a part called the *condition*
and are always followed by a block of code called the *clause*. Before
you learn about Python's specific flow control statements, I'll cover
what a condition and a block are.

#### ***Conditions*** {#calibre_link-114 .h2}

The Boolean expressions you've seen so far could all be considered
conditions, which are the same thing as expressions; *condition* is just
a more specific name in the context of flow control statements.
Conditions always evaluate down to a Boolean value, [True]{.literal} or
[False]{.literal}. A flow control statement decides what to do based on
whether its condition is [True]{.literal} or [False]{.literal}, and
almost every flow control statement uses a condition.

#### ***Blocks of Code*** {#calibre_link-115 .h2}

Lines of Python code can be grouped together in *blocks*. You can tell
when a block begins and ends from the indentation of the lines of code.
There are three rules for blocks.

-   Blocks begin when the indentation increases.
-   Blocks can contain other blocks.
-   Blocks end when the indentation decreases to zero or to a containing
    block's indentation.

[]{#calibre_link-947 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Blocks are easier to understand by looking at some
indented code, so let's find the blocks in part of a small game program,
shown here:

  name = \'Mary\'\
  password = \'swordfish\'\
  if name == \'Mary\':\
    [➊]{.ent} print(\'Hello, Mary\')\
       if password == \'swordfish\':\
        [➋]{.ent} print(\'Access granted.\')\
       else:\
        [➌]{.ent} print(\'Wrong password.\')

You can view the execution of this program at
*[https://autbor.com/blocks/](https://autbor.com/blocks/){.calibre6}*.
The first block of code [➊]{.ent} starts at the line [print(\'Hello,
Mary\')]{.literal} and contains all the lines after it. Inside this
block is another block [➋]{.ent}, which has only a single line in it:
[print(\'Access Granted.\')]{.literal}. The third block [➌]{.ent} is
also one line long: [print(\'Wrong password.\')]{.literal}.

### **Program Execution** {#calibre_link-116 .h1}

In the previous chapter's *hello.py* program, Python started executing
instructions at the top of the program going down, one after another.
The *program execution* (or simply, *execution*) is a term for the
current instruction being executed. If you print the source code on
paper and put your finger on each line as it is executed, you can think
of your finger as the program execution.

Not all programs execute by simply going straight down, however. If you
use your finger to trace through a program with flow control statements,
you'll likely find yourself jumping around the source code based on
conditions, and you'll probably skip entire clauses.

### **Flow Control Statements** {#calibre_link-117 .h1}

Now, let's explore the most important piece of flow control: the
statements themselves. The statements represent the diamonds you saw in
the flowchart in [Figure 2-1](#calibre_link-1533){.calibre6}, and they
are the actual decisions your programs will make.

#### ***if Statements*** {#calibre_link-118 .h2}

The most common type of flow control statement is the [if]{.literal}
statement. An [if]{.literal} statement's clause (that is, the block
following the [if]{.literal} statement) will execute if the statement's
condition is [True]{.literal}. The clause is skipped if the condition is
[False]{.literal}.

In plain English, an [if]{.literal} statement could be read as, "If this
condition is true, execute the code in the clause." In Python, an
[if]{.literal} statement consists of the following:

-   The [if]{.literal} keyword
-   A condition (that is, an expression that evaluates to
    [True]{.literal} or [False]{.literal})
-   []{#calibre_link-750 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}A colon
-   Starting on the next line, an indented block of code (called the
    [if]{.literal} clause)

For example, let's say you have some code that checks to see whether
someone's name is Alice. (Pretend [name]{.literal} was assigned some
value earlier.)

if name == \'Alice\':\
    print(\'Hi, Alice.\')

All flow control statements end with a colon and are followed by a new
block of code (the clause). This [if]{.literal} statement's clause is
the block with [print(\'Hi, Alice.\')]{.literal}. [Figure
2-2](#calibre_link-1538){.calibre6} shows what a flowchart of this code
would look like.

::: image1
[]{#calibre_link-1538
.calibre6}![image](../images/000147.jpg){.calibre3}
:::

*Figure 2-2: The flowchart for an [if]{.literal1} statement*

#### ***else Statements*** {#calibre_link-119 .h2}

An [if]{.literal} clause can optionally be followed by an
[else]{.literal} statement. The [else]{.literal} clause is executed only
when the [if]{.literal} statement's condition is [False]{.literal}. In
plain English, an [else]{.literal} statement could be read as, "If this
condition is true, execute this code. Or else, execute that code." An
[else]{.literal} statement doesn't have a condition, and in code, an
[else]{.literal} statement always consists of the following:

-   The [else]{.literal} keyword
-   A colon
-   Starting on the next line, an indented block of code (called the
    [else]{.literal} clause)

[]{#calibre_link-751 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Returning to the Alice example, let's look at some
code that uses an [else]{.literal} statement to offer a different
greeting if the person's name isn't Alice.

if name == \'Alice\':\
    print(\'Hi, Alice.\')\
else:\
    print(\'Hello, stranger.\')

[Figure 2-3](#calibre_link-1539){.calibre6} shows what a flowchart of
this code would look like.

::: image1
[]{#calibre_link-1539
.calibre6}![image](../images/000118.jpg){.calibre3}
:::

*Figure 2-3: The flowchart for an [else]{.literal1} statement*

#### ***elif Statements*** {#calibre_link-120 .h2}

While only one of the [if]{.literal} or [else]{.literal} clauses will
execute, you may have a case where you want one of *many* possible
clauses to execute. The [elif]{.literal} statement is an "else if"
statement that always follows an [if]{.literal} or another
[elif]{.literal} statement. It provides another condition that is
checked only if all of the previous conditions were [False]{.literal}.
In code, an [elif]{.literal} statement always consists of the following:

-   The [elif]{.literal} keyword
-   A condition (that is, an expression that evaluates to
    [True]{.literal} or [False]{.literal})
-   A colon
-   Starting on the next line, an indented block of code (called the
    [elif]{.literal} clause)

Let's add an [elif]{.literal} to the name checker to see this statement
in action.

if name == \'Alice\':\
    print(\'Hi, Alice.\')\
[]{#calibre_link-1712 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}elif age \< 12:\
    print(\'You are not Alice, kiddo.\')

This time, you check the person's age, and the program will tell them
something different if they're younger than 12. You can see the
flowchart for this in [Figure 2-4](#calibre_link-1540){.calibre6}.

::: image1
[]{#calibre_link-1540
.calibre6}![image](../images/000145.jpg){.calibre3}
:::

*Figure 2-4: The flowchart for an [elif]{.literal1} statement*

The [elif]{.literal} clause executes if [age \< 12]{.literal} is
[True]{.literal} and [name == \'Alice\']{.literal} is [False]{.literal}.
However, if both of the conditions are [False]{.literal}, then both of
the clauses are skipped. It is *not* guaranteed that at least one of the
clauses will be executed. When there is a chain of [elif]{.literal}
statements, only one or none of the clauses will be executed. Once one
of the statements' conditions is found to be [True]{.literal}, the rest
of the [elif]{.literal} clauses are automatically skipped. For example,
open a new file editor window and enter the following code, saving it as
*vampire.py*:

name = \'Carol\'\
age = 3000\
if name == \'Alice\':\
    print(\'Hi, Alice.\')\
elif age \< 12:\
    print(\'You are not Alice, kiddo.\')\
elif age \> 2000:\
[]{#calibre_link-1713 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    print(\'Unlike you, Alice is not an undead,
immortal vampire.\')\
elif age \> 100:\
    print(\'You are not Alice, grannie.\')

You can view the execution of this program at
*[https://autbor.com/vampire/](https://autbor.com/vampire/){.calibre6}*.
Here, I've added two more [elif]{.literal} statements to make the name
checker greet a person with different answers based on [age]{.literal}.
[Figure 2-5](#calibre_link-1541){.calibre6} shows the flowchart for
this.

::: image1
[]{#calibre_link-1541
.calibre6}![image](../images/000080.jpg){.calibre3}
:::

*Figure 2-5: The flowchart for multiple [elif]{.literal1} statements in
the* vampire.py *program*

[]{#calibre_link-1714 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The order of the [elif]{.literal} statements does
matter, however. Let's rearrange them to introduce a bug. Remember that
the rest of the [elif]{.literal} clauses are automatically skipped once
a [True]{.literal} condition has been found, so if you swap around some
of the clauses in *vampire.py*, you run into a problem. Change the code
to look like the following, and save it as *vampire2.py*:

   name = \'Carol\'\
   age = 3000\
   if name == \'Alice\':\
       print(\'Hi, Alice.\')\
   elif age \< 12:\
       print(\'You are not Alice, kiddo.\')\
[➊]{.ent} elif age \> 100:\
       print(\'You are not Alice, grannie.\')\
   elif age \> 2000:\
       print(\'Unlike you, Alice is not an undead, immortal vampire.\')

You can view the execution of this program at
*[https://autbor.com/vampire2/](https://autbor.com/vampire2/){.calibre6}*.
Say the [age]{.literal} variable contains the value [3000]{.literal}
before this code is executed. You might expect the code to print the
string [\'Unlike you, Alice is not an undead, immortal
vampire.\']{.literal}. However, because the [age \> 100]{.literal}
condition is [True]{.literal} (after all, 3,000 *is* greater than 100)
[➊]{.ent}, the string [\'You are not Alice, grannie.\']{.literal} is
printed, and the rest of the [elif]{.literal} statements are
automatically skipped. Remember that at most only one of the clauses
will be executed, and for [elif]{.literal} statements, the order
matters!

[Figure 2-6](#calibre_link-1542){.calibre6} shows the flowchart for the
previous code. Notice how the diamonds for [age \> 100]{.literal} and
[age \> 2000]{.literal} are swapped.

Optionally, you can have an [else]{.literal} statement after the last
[elif]{.literal} statement. In that case, it *is* guaranteed that at
least one (and only one) of the clauses will be executed. If the
conditions in every [if]{.literal} and [elif]{.literal} statement are
[False]{.literal}, then the [else]{.literal} clause is executed. For
example, let's re-create the Alice program to use [if]{.literal},
[elif]{.literal}, and [else]{.literal} clauses.

name = \'Carol\'\
age = 3000\
if name == \'Alice\':\
    print(\'Hi, Alice.\')\
elif age \< 12:\
    print(\'You are not Alice, kiddo.\')\
else:\
    print(\'You are neither Alice nor a little kid.\')

You can view the execution of this program at
*[https://autbor.com/littlekid/](https://autbor.com/littlekid/){.calibre6}*.
[Figure 2-7](#calibre_link-1543){.calibre6} shows the flowchart for this
new code, which we'll save as *littleKid.py*.

In plain English, this type of flow control structure would be "If the
first condition is true, do this. Else, if the second condition is true,
do that. Otherwise, do something else." When you use [if]{.literal},
[elif]{.literal}, and [else]{.literal} statements together, remember
these rules about how to order them to avoid bugs like the one in
[Figure 2-6](#calibre_link-1542){.calibre6}. First, there is always
exactly one [if]{.literal} statement. Any []{#calibre_link-1715
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}[elif]{.literal}
statements you need should follow the [if]{.literal} statement. Second,
if you want to be sure that at least one clause is executed, close the
structure with an [else]{.literal} statement.

::: image1
[]{#calibre_link-1542
.calibre6}![image](../images/000136.jpg){.calibre3}
:::

*Figure 2-6: The flowchart for the* vampire2.py *program. The X path
will logically never happen, because if [age]{.literal1} were greater
than [2000]{.literal1}, it would have already been greater than
[100]{.literal1}.*

::: image1
[]{#calibre_link-752 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1543
.calibre6}![image](../images/000032.jpg){.calibre3}
:::

*Figure 2-7: Flowchart for the previous* littleKid.py *program*

#### ***while Loop Statements*** {#calibre_link-121 .h2}

You can make a block of code execute over and over again using a
[while]{.literal} statement. The code in a [while]{.literal} clause will
be executed as long as the [while]{.literal} statement's condition is
[True]{.literal}. In code, a [while]{.literal} statement always consists
of the following:

-   The [while]{.literal} keyword
-   A condition (that is, an expression that evaluates to
    [True]{.literal} or [False]{.literal})
-   A colon
-   Starting on the next line, an indented block of code (called the
    [while]{.literal} clause)

[]{#calibre_link-1716 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can see that a [while]{.literal} statement
looks similar to an [if]{.literal} statement. The difference is in how
they behave. At the end of an [if]{.literal} clause, the program
execution continues after the [if]{.literal} statement. But at the end
of a [while]{.literal} clause, the program execution jumps back to the
start of the [while]{.literal} statement. The [while]{.literal} clause
is often called the *while loop* or just the *loop*.

Let's look at an [if]{.literal} statement and a [while]{.literal} loop
that use the same condition and take the same actions based on that
condition. Here is the code with an [if]{.literal} statement:

spam = 0\
if spam \< 5:\
    print(\'Hello, world.\')\
    spam = spam + 1

Here is the code with a [while]{.literal} statement:

spam = 0\
while spam \< 5:\
    print(\'Hello, world.\')\
    spam = spam + 1

These statements are similar---both [if]{.literal} and [while]{.literal}
check the value of [spam]{.literal}, and if it's less than 5, they print
a message. But when you run these two code snippets, something very
different happens for each one. For the [if]{.literal} statement, the
output is simply [\"Hello, world.\"]{.literal}. But for the
[while]{.literal} statement, it's [\"Hello, world.\"]{.literal} repeated
five times! Take a look at the flowcharts for these two pieces of code,
[Figures 2-8](#calibre_link-1544){.calibre6} and
[2-9](#calibre_link-1545){.calibre6}, to see why this happens.

::: image1
[]{#calibre_link-1544
.calibre6}![image](../images/000072.jpg){.calibre3}
:::

*Figure 2-8: The flowchart for the [if]{.literal1} statement code*

::: image1
[]{#calibre_link-1018 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1545
.calibre6}![image](../images/000112.jpg){.calibre3}
:::

*Figure 2-9: The flowchart for the [while]{.literal1} statement code*

The code with the [if]{.literal} statement checks the condition, and it
prints [Hello, world.]{.literal} only once if that condition is true.
The code with the [while]{.literal} loop, on the other hand, will print
it five times. The loop stops after five prints because the integer in
[spam]{.literal} increases by one at the end of each loop iteration,
which means that the loop will execute five times before [spam \<
5]{.literal} is [False]{.literal}.

In the [while]{.literal} loop, the condition is always checked at the
start of each *iteration* (that is, each time the loop is executed). If
the condition is [True]{.literal}, then the clause is executed, and
afterward, the condition is checked again. The first time the condition
is found to be [False]{.literal}, the [while]{.literal} clause is
skipped.

##### **An Annoying while Loop** {#calibre_link-1717 .h2}

Here's a small example program that will keep asking you to type,
literally, [your name]{.literal}. Select **File** ▸ **New** to open a
new file editor window, enter the following code, and save the file as
*yourName.py*:

[➊]{.ent} name = \'\'\
[➋]{.ent} while name != \'your name\':\
       print(\'Please type your name.\')\
    [➌]{.ent} name = input()\
[➍]{.ent} print(\'Thank you!\')

You can view the execution of this program at
*[https://autbor.com/yourname/](https://autbor.com/yourname/){.calibre6}*.
First, the program sets the [name]{.literal} variable [➊]{.ent} to an
empty string. This is so []{#calibre_link-1718 {http:="" www.idpf.org=""
2007="" ops}type="pagebreak"}that the [name != \'your name\']{.literal}
condition will evaluate to [True]{.literal} and the program execution
will enter the [while]{.literal} loop's clause [➋]{.ent}.

The code inside this clause asks the user to type their name, which is
assigned to the [name]{.literal} variable [➌]{.ent}. Since this is the
last line of the block, the execution moves back to the start of the
[while]{.literal} loop and reevaluates the condition. If the value in
[name]{.literal} is *not equal* to the string [\'your name\']{.literal},
then the condition is [True]{.literal}, and the execution enters the
[while]{.literal} clause again.

But once the user types [your name]{.literal}, the condition of the
[while]{.literal} loop will be [\'your name\' != \'your
name\']{.literal}, which evaluates to [False]{.literal}. The condition
is now [False]{.literal}, and instead of the program execution
reentering the [while]{.literal} loop's clause, Python skips past it and
continues running the rest of the program [➍]{.ent}. [Figure
2-10](#calibre_link-1546){.calibre6} shows a flowchart for the
*yourName.py* program.

::: image1
[]{#calibre_link-1546
.calibre6}![image](../images/000042.jpg){.calibre3}
:::

*Figure 2-10: A flowchart of the* yourName.py *program*

Now, let's see *yourName.py* in action. Press **F5** to run it, and
enter something other than [your name]{.literal} a few times before you
give the program what it wants.

Please type your name.\
[Al]{.codestrong1}\
Please type your name.\
[Albert]{.codestrong1}\
Please type your name.\
[%#@#%\*(\^&!!!]{.codestrong1}\
[]{#calibre_link-823 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Please type your name.\
[your name]{.codestrong1}\
Thank you!

If you never enter [your name]{.literal}, then the [while]{.literal}
loop's condition will never be [False]{.literal}, and the program will
just keep asking forever. Here, the [input()]{.literal} call lets the
user enter the right string to make the program move on. In other
programs, the condition might never actually change, and that can be a
problem. Let's look at how you can break out of a [while]{.literal}
loop.

#### ***break Statements*** {#calibre_link-122 .h2}

There is a shortcut to getting the program execution to break out of a
[while]{.literal} loop's clause early. If the execution reaches a
[break]{.literal} statement, it immediately exits the [while]{.literal}
loop's clause. In code, a [break]{.literal} statement simply contains
the [break]{.literal} keyword.

Pretty simple, right? Here's a program that does the same thing as the
previous program, but it uses a [break]{.literal} statement to escape
the loop. Enter the following code, and save the file as *yourName2.py*:

[➊]{.ent} while True:\
       print(\'Please type your name.\')\
    [➋]{.ent} name = input()\
    [➌]{.ent} if name == \'your name\':\
        [➍]{.ent} break\
[➎]{.ent} print(\'Thank you!\')

You can view the execution of this program at
*[https://autbor.com/yourname2/](https://autbor.com/yourname2/){.calibre6}*.
The first line [➊]{.ent} creates an *infinite loop*; it is a
[while]{.literal} loop whose condition is always [True]{.literal}. (The
expression [True]{.literal}, after all, always evaluates down to the
value [True]{.literal}.) After the program execution enters this loop,
it will exit the loop only when a [break]{.literal} statement is
executed. (An infinite loop that *never* exits is a common programming
bug.)

Just like before, this program asks the user to enter [your
name]{.literal} [➋]{.ent}. Now, however, while the execution is still
inside the [while]{.literal} loop, an [if]{.literal} statement checks
[➌]{.ent} whether [name]{.literal} is equal to [\'your
name\']{.literal}. If this condition is [True]{.literal}, the
[break]{.literal} statement is run [➍]{.ent}, and the execution moves
out of the loop to [print(\'Thank you!\')]{.literal} [➎]{.ent}.
Otherwise, the [if]{.literal} statement's clause that contains the
[break]{.literal} statement is skipped, which puts the execution at the
end of the [while]{.literal} loop. At this point, the program execution
jumps back to the start of the [while]{.literal} statement [➊]{.ent} to
recheck the condition. Since this condition is merely the
[True]{.literal} Boolean value, the execution enters the loop to ask the
user to type [your name]{.literal} again. See [Figure
2-11](#calibre_link-1547){.calibre6} for this program's flowchart.

Run *yourName2.py*, and enter the same text you entered for
*yourName.py*. The rewritten program should respond in the same way as
the original.

::: image1
[]{#calibre_link-863 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1547
.calibre6}![image](../images/000134.jpg){.calibre3}
:::

*Figure 2-11: The flowchart for the* yourName2.py *program with an
infinite loop. Note that the X path will logically never happen, because
the loop condition is always [True]{.literal1}.*

#### ***continue Statements*** {#calibre_link-123 .h2}

Like [break]{.literal} statements, [continue]{.literal} statements are
used inside loops. When the program execution reaches a
[continue]{.literal} statement, the program execution immediately jumps
back to the start of the loop and reevaluates the loop's condition.
(This is also what happens when the execution reaches the end of the
loop.)

Let's use [continue]{.literal} to write a program that asks for a name
and password. Enter the following code into a new file editor window and
save the program as *swordfish.py*.

::: sidebar
**TRAPPED IN AN INFINITE LOOP?**

If you ever run a program that has a bug causing it to get stuck in an
infinite loop, press [CTRL]{.small}-C or select **Shell** ▸ **Restart
Shell** from IDLE's menu. This will send a
[KeyboardInterrupt]{.literal1} error to your program and cause it to
stop immediately. Try stopping a program by creating a simple infinite
loop in the file editor, and save the program as *infiniteLoop.py*.

while True:\
    print(\'Hello, world!\')

When you run this program, it will print [Hello, world!]{.literal1} to
the screen forever because the [while]{.literal1} statement's condition
is always [True]{.literal1}. [CTRL]{.small}-C is also handy if you want
to simply terminate your program immediately, even if it's not stuck in
an infinite loop.
:::

[]{#calibre_link-1025 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}  while True:\
      print(\'Who are you?\')\
      name = input()\
    [➊]{.ent} if name != \'Joe\':\
        [➋]{.ent} continue\
       print(\'Hello, Joe. What is the password? (It is a fish.)\')\
    [➌]{.ent} password = input()\
       if password == \'swordfish\':\
        [➍]{.ent} break\
[➎]{.ent} print(\'Access granted.\')    

If the user enters any name besides [Joe]{.literal} [➊]{.ent}, the
[continue]{.literal} statement [➋]{.ent} causes the program execution to
jump back to the start of the loop. When the program reevaluates the
condition, the execution will always enter the loop, since the condition
is simply the value [True]{.literal}. Once the user makes it past that
[if]{.literal} statement, they are asked for a password [➌]{.ent}. If
the password entered is [swordfish]{.literal}, then the
[break]{.literal} statement [➍]{.ent} is run, and the execution jumps
out of the [while]{.literal} loop to print [Access granted]{.literal}
[➎]{.ent}. Otherwise, the execution continues to the end of the
[while]{.literal} loop, where it then jumps back to the start of the
loop. See [Figure 2-12](#calibre_link-1548){.calibre6} for this
program's flowchart.

::: image1
[]{#calibre_link-1719 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1548
.calibre6}![image](../images/000078.jpg){.calibre3}
:::

*Figure 2-12: A flowchart for* swordfish.py. *The X path will logically
never happen, because the loop condition is always [True]{.literal1}.*

::: sidebar
**"TRUTHY" AND "FALSEY" VALUES**

Conditions will consider some values in other data types equivalent to
[True]{.literal1} and [False]{.literal1}. When used in conditions,
[0]{.literal1}, [0.0]{.literal1}, and [\'\']{.literal1} (the empty
string) are considered [False]{.literal1}, while all other values are
considered [True]{.literal1}. For example, look at the following
program:

name = \'\'\
[➊]{.ent} while not name:\
    print(\'Enter your name:\')\
    name = input()\
print(\'How many guests will you have?\')\
numOfGuests = int(input())\
[➋]{.ent} if numOfGuests:\
    [➌]{.ent} print(\'Be sure to have enough room for all your
guests.\')\
print(\'Done\')

You can view the execution of this program at
*[https://autbor.com/howmanyguests/](https://autbor.com/howmanyguests/){.calibre6}*.
If the user enters a blank string for [name]{.literal1}, then the
[while]{.literal1} statement's condition will be [True]{.literal1}
[➊]{.ent}, and the program continues to ask for a name. If the value for
[numOfGuests]{.literal1} is not [0]{.literal1} [➋]{.ent}, then the
condition is considered to be [True]{.literal1}, and the program will
print a reminder for the user [➌]{.ent}.

You could have entered [not name != \'\']{.literal1} instead of [not
name]{.literal1}, and [numOfGuests != 0]{.literal1} instead of
[numOfGuests]{.literal1}, but using the truthy and falsey values can
make your code easier to read.
:::

[]{#calibre_link-821 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Run this program and give it some input. Until you
claim to be Joe, the program shouldn't ask for a password, and once you
enter the correct password, it should exit.

Who are you?\
[I\'m fine, thanks. Who are you?]{.codestrong1}\
Who are you?\
[Joe]{.codestrong1}\
Hello, Joe. What is the password? (It is a fish.)\
[Mary]{.codestrong1}\
Who are you?\
[Joe]{.codestrong1}\
Hello, Joe. What is the password? (It is a fish.)\
[swordfish]{.codestrong1}\
Access granted.

You can view the execution of this program at
*[https://autbor.com/hellojoe/](https://autbor.com/hellojoe/){.calibre6}*.

#### []{#calibre_link-753 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***for Loops and the range() Function*** {#calibre_link-124 .h2}

The [while]{.literal} loop keeps looping while its condition is
[True]{.literal} (which is the reason for its name), but what if you
want to execute a block of code only a certain number of times? You can
do this with a [for]{.literal} loop statement and the
[range()]{.literal} function.

In code, a [for]{.literal} statement looks something like [for i in
range(5):]{.literal} and includes the following:

-   The [for]{.literal} keyword
-   A variable name
-   The [in]{.literal} keyword
-   A call to the [range()]{.literal} method with up to three integers
    passed to it
-   A colon
-   Starting on the next line, an indented block of code (called the
    [for]{.literal} clause)

Let's create a new program called *fiveTimes.py* to help you see a
[for]{.literal} loop in action.

print(\'My name is\')\
for i in range(5):\
    print(\'Jimmy Five Times (\' + str(i) + \')\')

You can view the execution of this program at
*[https://autbor.com/fivetimesfor/](https://autbor.com/fivetimesfor/){.calibre6}*.
The code in the [for]{.literal} loop's clause is run five times. The
first time it is run, the variable [i]{.literal} is set to
[0]{.literal}. The [print()]{.literal} call in the clause will print
[Jimmy Five Times (0)]{.literal}. After Python finishes an iteration
through all the code inside the [for]{.literal} loop's clause, the
execution goes back to the top of the loop, and the [for]{.literal}
statement increments [i]{.literal} by one. This is why
[range(5)]{.literal} results in five iterations through the clause, with
[i]{.literal} being set to [0]{.literal}, then [1]{.literal}, then
[2]{.literal}, then [3]{.literal}, and then [4]{.literal}. The variable
[i]{.literal} will go up to, but will not include, the integer passed to
[range()]{.literal}. [Figure 2-13](#calibre_link-1549){.calibre6} shows
a flowchart for the *fiveTimes.py* program.

When you run this program, it should print [Jimmy Five Times]{.literal}
followed by the value of [i]{.literal} five times before leaving the
[for]{.literal} loop.

My name is\
Jimmy Five Times (0)\
Jimmy Five Times (1)\
Jimmy Five Times (2)\
Jimmy Five Times (3)\
Jimmy Five Times (4)

::: note
**[NOTE]{.notes}**

*You can use [break]{.codeitalic} and [continue]{.codeitalic} statements
inside [for]{.codeitalic} loops as well. The [continue]{.codeitalic}
statement will continue to the next value of the [for]{.codeitalic}
loop's counter, as if the program execution had reached the end of the
loop and returned to the start. In fact, you can use
[continue]{.codeitalic} and [break]{.codeitalic} statements only inside
[while]{.codeitalic} and [for]{.codeitalic} loops. If you try to use
these statements elsewhere, Python will give you an error.*
:::

::: image1
[]{#calibre_link-978 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[]{#calibre_link-1549
.calibre6}![image](../images/000027.jpg){.calibre3}
:::

*Figure 2-13: The flowchart for* fiveTimes.py

As another [for]{.literal} loop example, consider this story about the
mathematician Carl Friedrich Gauss. When Gauss was a boy, a teacher
wanted to give the class some busywork. The teacher told them to add up
all the numbers from 0 to 100. Young Gauss came up with a clever trick
to figure out the answer in a few seconds, but you can write a Python
program with a [for]{.literal} loop to do this calculation for you.

[➊]{.ent} total = 0\
[➋]{.ent} for num in range(101):\
    [➌]{.ent} total = total + num\
[➍]{.ent} print(total)  

The result should be 5,050. When the program first starts, the
[total]{.literal} variable is set to [0]{.literal} [➊]{.ent}. The
[for]{.literal} loop [➋]{.ent} then executes [total = total +
num]{.literal} [➌]{.ent} 100 times. By the time the loop has finished
all of its 100 iterations, every integer from [0]{.literal} to
[100]{.literal} will have been added to [total]{.literal}. At this
point, [total]{.literal} is printed to the screen [➍]{.ent}. Even on the
slowest computers, this program takes less than a second to complete.

(Young Gauss figured out a way to solve the problem in seconds. There
are 50 pairs of numbers that add up to 101: 1 + 100, 2 + 99, 3 + 98, and
so on, until 50 + 51. Since 50 × 101 is 5,050, the sum of all the
numbers from 0 to 100 is 5,050. Clever kid!)

##### **[]{#calibre_link-975 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}An Equivalent while Loop** {#calibre_link-1720 .h2}

You can actually use a [while]{.literal} loop to do the same thing as a
[for]{.literal} loop; [for]{.literal} loops are just more concise. Let's
rewrite *fiveTimes.py* to use a [while]{.literal} loop equivalent of a
[for]{.literal} loop.

print(\'My name is\')\
i = 0\
while i \< 5:\
    print(\'Jimmy Five Times (\' + str(i) + \')\')\
    i = i + 1

You can view the execution of this program at
*[https://autbor.com/fivetimeswhile/](https://autbor.com/fivetimeswhile/){.calibre6}*.
If you run this program, the output should look the same as the
*fiveTimes.py* program, which uses a [for]{.literal} loop.

##### **The Starting, Stopping, and Stepping Arguments to range()** {#calibre_link-1721 .h2}

Some functions can be called with multiple arguments separated by a
comma, and [range()]{.literal} is one of them. This lets you change the
integer passed to [range()]{.literal} to follow any sequence of
integers, including starting at a number other than zero.

for i in range(12, 16):\
    print(i)

The first argument will be where the [for]{.literal} loop's variable
starts, and the second argument will be up to, but not including, the
number to stop at.

12\
13\
14\
15

The [range()]{.literal} function can also be called with three
arguments. The first two arguments will be the start and stop values,
and the third will be the *step argument*. The step is the amount that
the variable is increased by after each iteration.

for i in range(0, 10, 2):\
    print(i)

So calling [range(0, 10, 2)]{.literal} will count from zero to eight by
intervals of two.

0\
2\
4\
6\
8

[]{#calibre_link-827 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [range()]{.literal} function is flexible in the
sequence of numbers it produces for [for]{.literal} loops. *For* example
(I never apologize for my puns), you can even use a negative number for
the step argument to make the [for]{.literal} loop count down instead of
up.

for i in range(5, -1, -1):\
    print(i)

This [for]{.literal} loop would have the following output:

5\
4\
3\
2\
1\
0

Running a [for]{.literal} loop to print [i]{.literal} with [range(5, -1,
-1)]{.literal} should print from five down to zero.

### **Importing Modules** {#calibre_link-125 .h1}

All Python programs can call a basic set of functions called *built-in
functions*, including the [print()]{.literal}, [input()]{.literal}, and
[len()]{.literal} functions you've seen before. Python also comes with a
set of modules called the *standard library*. Each module is a Python
program that contains a related group of functions that can be embedded
in your programs. For example, the [math]{.literal} module has
mathematics-related functions, the [random]{.literal} module has random
number-related functions, and so on.

Before you can use the functions in a module, you must import the module
with an [import]{.literal} statement. In code, an [import]{.literal}
statement consists of the following:

-   The [import]{.literal} keyword
-   The name of the module
-   Optionally, more module names, as long as they are separated by
    commas

Once you import a module, you can use all the cool functions of that
module. Let's give it a try with the [random]{.literal} module, which
will give us access to the [random.randint()]{.literal} function.

Enter this code into the file editor, and save it as *printRandom.py*:

import random\
for i in range(5):\
    print(random.randint(1, 10))

::: sidebar
**DON'T OVERWRITE MODULE NAMES**

When you save your Python scripts, take care not to give them a name
that is used by one of Python's modules, such as *random.py*, *sys.py*,
*os.py*, or *math.py*. If you accidentally name one of your programs,
say, *random.py*, and use an [import random]{.literal1} statement in
another program, your program would import your *random.py* file instead
of Python's [random]{.literal1} module. This can lead to errors such as
[AttributeError: module \'random\' has no attribute
\'randint\']{.literal1}, since your *random.py* doesn't have the
functions that the real [random]{.literal1} module has. Don't use the
names of any built-in Python functions either, such as
[print()]{.literal1} or [input()]{.literal1}.

Problems like these are uncommon, but can be tricky to solve. As you
gain more programming experience, you'll become more aware of the
standard names used by Python's modules and functions, and will run into
these problems less frequently.
:::

[]{#calibre_link-802 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}When you run this program, the output will look
something like this:

4\
1\
8\
4\
1

You can view the execution of this program at
*[https://autbor.com/printrandom/](https://autbor.com/printrandom/){.calibre6}*.
The [random.randint()]{.literal} function call evaluates to a random
integer value between the two integers that you pass it. Since
[randint()]{.literal} is in the [random]{.literal} module, you must
first type **random.** in front of the function name to tell Python to
look for this function inside the [random]{.literal} module.

Here's an example of an [import]{.literal} statement that imports four
different modules:

import random, sys, os, math

Now we can use any of the functions in these four modules. We'll learn
more about them later in the book.

#### ***from import Statements*** {#calibre_link-126 .h2}

An alternative form of the [import]{.literal} statement is composed of
the [from]{.literal} keyword, followed by the module name, the
[import]{.literal} keyword, and a star; for example, [from random import
\*]{.literal}.

With this form of [import]{.literal} statement, calls to functions in
[random]{.literal} will not need the [random.]{.literal} prefix.
However, using the full name makes for more readable code, so it is
better to use the [import random]{.literal} form of the statement.

### []{#calibre_link-948 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Ending a Program Early with the sys.exit() Function** {#calibre_link-127 .h1}

The last flow control concept to cover is how to terminate the program.
Programs always terminate if the program execution reaches the bottom of
the instructions. However, you can cause the program to terminate, or
exit, before the last instruction by calling the [sys.exit()]{.literal}
function. Since this function is in the [sys]{.literal} module, you have
to import [sys]{.literal} before your program can use it.

Open a file editor window and enter the following code, saving it as
*exitExample.py*:

import sys\
\
while True:\
    print(\'Type exit to exit.\')\
    response = input()\
    if response == \'exit\':\
        sys.exit()\
    print(\'You typed \' + response + \'.\')

Run this program in IDLE. This program has an infinite loop with no
[break]{.literal} statement inside. The only way this program will end
is if the execution reaches the [sys.exit()]{.literal} call. When
[response]{.literal} is equal to [exit]{.literal}, the line containing
the [sys.exit()]{.literal} call is executed. Since the
[response]{.literal} variable is set by the [input()]{.literal}
function, the user must enter [exit]{.literal} in order to stop the
program.

### **A Short Program: Guess the Number** {#calibre_link-128 .h1}

The examples I've shown you so far are useful for introducing basic
concepts, but now let's see how everything you've learned comes together
in a more complete program. In this section, I'll show you a simple
"guess the number" game. When you run this program, the output will look
something like this:

I am thinking of a number between 1 and 20.\
Take a guess.\
[10]{.codestrong1}\
Your guess is too low.\
Take a guess.\
[15]{.codestrong1}\
Your guess is too low.\
Take a guess.\
[17]{.codestrong1}\
Your guess is too high.\
Take a guess.\
[16]{.codestrong1}\
Good job! You guessed my number in 4 guesses!

[]{#calibre_link-1074 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Enter the following source code into the file
editor, and save the file as *guessTheNumber.py*:

\# This is a guess the number game.\
import random\
secretNumber = random.randint(1, 20)\
print(\'I am thinking of a number between 1 and 20.\')\
\
\# Ask the player to guess 6 times.\
for guessesTaken in range(1, 7):\
    print(\'Take a guess.\')\
    guess = int(input())\
\
    if guess \< secretNumber:\
        print(\'Your guess is too low.\')\
    elif guess \> secretNumber:\
        print(\'Your guess is too high.\')\
    else:\
        break    # This condition is the correct guess!\
\
if guess == secretNumber:\
    print(\'Good job! You guessed my number in \' + str(guessesTaken) +
\'\
guesses!\')\
else:\
    print(\'Nope. The number I was thinking of was \' +
str(secretNumber))

You can view the execution of this program at
*[https://autbor.com/guessthenumber/](https://autbor.com/guessthenumber/){.calibre6}*.
Let's look at this code line by line, starting at the top.

\# This is a guess the number game.\
import random\
secretNumber = random.randint(1, 20)

First, a comment at the top of the code explains what the program does.
Then, the program imports the [random]{.literal} module so that it can
use the [random.randint()]{.literal} function to generate a number for
the user to guess. The return value, a random integer between 1 and 20,
is stored in the variable [secretNumber]{.literal}.

print(\'I am thinking of a number between 1 and 20.\')\
\
\# Ask the player to guess 6 times.\
for guessesTaken in range(1, 7):\
    print(\'Take a guess.\')\
    guess = int(input())

The program tells the player that it has come up with a secret number
and will give the player six chances to guess it. The code that lets the
player enter a guess and checks that guess is in a [for]{.literal} loop
that will loop at most six times. The first thing that happens in the
loop is that the player types in a guess. Since [input()]{.literal}
returns a string, its return value is passed straight into
[]{#calibre_link-928 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[int()]{.literal}, which translates the string into
an integer value. This gets stored in a variable named
[guess]{.literal}.

    if guess \< secretNumber:\
        print(\'Your guess is too low.\')\
    elif guess \> secretNumber:\
        print(\'Your guess is too high.\')

These few lines of code check to see whether the guess is less than or
greater than the secret number. In either case, a hint is printed to the
screen.

    else:\
        break    # This condition is the correct guess!

If the guess is neither higher nor lower than the secret number, then it
must be equal to the secret number---in which case, you want the program
execution to break out of the [for]{.literal} loop.

if guess == secretNumber:\
    print(\'Good job! You guessed my number in \' + str(guessesTaken) +
\' guesses!\')\
else:\
    print(\'Nope. The number I was thinking of was \' +
str(secretNumber))

After the [for]{.literal} loop, the previous [if\...else]{.literal}
statement checks whether the player has correctly guessed the number and
then prints an appropriate message to the screen. In both cases, the
program displays a variable that contains an integer value
([guessesTaken]{.literal} and [secretNumber]{.literal}). Since it must
concatenate these integer values to strings, it passes these variables
to the [str()]{.literal} function, which returns the string value form
of these integers. Now these strings can be concatenated with the
[+]{.literal} operators before finally being passed to the
[print()]{.literal} function call.

### **A Short Program: Rock, Paper, Scissors** {#calibre_link-129 .h1}

Let's use the programming concepts we've learned so far to create a
simple rock, paper, scissors game. The output will look like this:

ROCK, PAPER, SCISSORS\
0 Wins, 0 Losses, 0 Ties\
Enter your move: (r)ock (p)aper (s)cissors or (q)uit\
[p]{.codestrong1}\
PAPER versus\...\
PAPER\
It is a tie!\
0 Wins, 1 Losses, 1 Ties\
Enter your move: (r)ock (p)aper (s)cissors or (q)uit\
[s]{.codestrong1}\
SCISSORS versus\...\
PAPER\
You win!\
[]{#calibre_link-1722 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}1 Wins, 1 Losses, 1 Ties\
Enter your move: (r)ock (p)aper (s)cissors or (q)uit\
[q]{.codestrong1}

Type the following source code into the file editor, and save the file
as *rpsGame.py*:

import random, sys\
\
print(\'ROCK, PAPER, SCISSORS\')\
\
\# These variables keep track of the number of wins, losses, and ties.\
wins = 0\
losses = 0\
ties = 0\
\
while True: \# The main game loop.\
    print(\'%s Wins, %s Losses, %s Ties\' % (wins, losses, ties))\
    while True: \# The player input loop.\
        print(\'Enter your move: (r)ock (p)aper (s)cissors or (q)uit\')\
        playerMove = input()\
        if playerMove == \'q\':\
            sys.exit() \# Quit the program.\
        if playerMove == \'r\' or playerMove == \'p\' or playerMove ==
\'s\':\
            break \# Break out of the player input loop.\
        print(\'Type one of r, p, s, or q.\')\
\
    # Display what the player chose:\
    if playerMove == \'r\':\
        print(\'ROCK versus\...\')\
    elif playerMove == \'p\':\
        print(\'PAPER versus\...\')\
    elif playerMove == \'s\':\
        print(\'SCISSORS versus\...\')\
\
    # Display what the computer chose:\
    randomNumber = random.randint(1, 3)\
    if randomNumber == 1:\
        computerMove = \'r\'\
        print(\'ROCK\')\
    elif randomNumber == 2:\
        computerMove = \'p\'\
        print(\'PAPER\')\
    elif randomNumber == 3:\
        computerMove = \'s\'\
        print(\'SCISSORS\')\
\
    # Display and record the win/loss/tie:\
    if playerMove == computerMove:\
        print(\'It is a tie!\')\
        ties = ties + 1\
    elif playerMove == \'r\' and computerMove == \'s\':\
        print(\'You win!\')\
        wins = wins + 1\
[]{#calibre_link-1723 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    elif playerMove == \'p\' and computerMove ==
\'r\':\
        print(\'You win!\')\
        wins = wins + 1\
    elif playerMove == \'s\' and computerMove == \'p\':\
        print(\'You win!\')\
        wins = wins + 1\
    elif playerMove == \'r\' and computerMove == \'p\':\
        print(\'You lose!\')\
        losses = losses + 1\
    elif playerMove == \'p\' and computerMove == \'s\':\
        print(\'You lose!\')\
        losses = losses + 1\
    elif playerMove == \'s\' and computerMove == \'r\':\
        print(\'You lose!\')\
        losses = losses + 1

Let's look at this code line by line, starting at the top.

import random, sys\
\
print(\'ROCK, PAPER, SCISSORS\')\
\
\# These variables keep track of the number of wins, losses, and ties.\
wins = 0\
losses = 0\
ties = 0

First, we import the [random]{.literal} and [sys]{.literal} module so
that our program can call the [random.randint()]{.literal} and
[sys.exit()]{.literal} functions. We also set up three variables to keep
track of how many wins, losses, and ties the player has had.

while True: \# The main game loop.\
    print(\'%s Wins, %s Losses, %s Ties\' % (wins, losses, ties))\
    while True: \# The player input loop.\
        print(\'Enter your move: (r)ock (p)aper (s)cissors or (q)uit\')\
        playerMove = input()\
        if playerMove == \'q\':\
            sys.exit() \# Quit the program.\
        if playerMove == \'r\' or playerMove == \'p\' or playerMove ==
\'s\':\
            break \# Break out of the player input loop.\
        print(\'Type one of r, p, s, or q.\')

This program uses a [while]{.literal} loop inside of another
[while]{.literal} loop. The first loop is the main game loop, and a
single game of rock, paper, scissors is played on each iteration through
this loop. The second loop asks for input from the player, and keeps
looping until the player has entered an [r]{.literal}, [p]{.literal},
[s]{.literal}, or [q]{.literal} for their move. The [r]{.literal},
[p]{.literal}, and [s]{.literal} correspond to rock, paper, and
scissors, respectively, while the [q]{.literal} means the player intends
to quit. In that case, [sys.exit()]{.literal} is called and the program
exits. If the player has entered [r]{.literal}, [p]{.literal}, or
[s]{.literal}, the execution breaks out of the loop. Otherwise, the
program reminds the player to enter [r]{.literal}, [p]{.literal},
[s]{.literal}, or [q]{.literal} and goes back to the start of the loop.

[]{#calibre_link-1724 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    # Display what the player chose:\
    if playerMove == \'r\':\
        print(\'ROCK versus\...\')\
    elif playerMove == \'p\':\
        print(\'PAPER versus\...\')\
    elif playerMove == \'s\':\
        print(\'SCISSORS versus\...\')

The player's move is displayed on the screen.

    # Display what the computer chose:\
    randomNumber = random.randint(1, 3)\
    if randomNumber == 1:\
        computerMove = \'r\'\
        print(\'ROCK\')\
    elif randomNumber == 2:\
        computerMove = \'p\'\
        print(\'PAPER\')\
    elif randomNumber == 3:\
        computerMove = \'s\'\
        print(\'SCISSORS\')

Next, the computer's move is randomly selected. Since
[random.randint()]{.literal} can only return a random number, the
[1]{.literal}, [2]{.literal}, or [3]{.literal} integer value it returns
is stored in a variable named [randomNumber]{.literal}. The program
stores a [\'r\']{.literal}, [\'p\']{.literal}, or [\'s\']{.literal}
string in [computerMove]{.literal} based on the integer in
[randomNumber]{.literal}, as well as displays the computer's move.

    # Display and record the win/loss/tie:\
    if playerMove == computerMove:\
        print(\'It is a tie!\')\
        ties = ties + 1\
    elif playerMove == \'r\' and computerMove == \'s\':\
        print(\'You win!\')\
        wins = wins + 1\
    elif playerMove == \'p\' and computerMove == \'r\':\
        print(\'You win!\')\
        wins = wins + 1\
    elif playerMove == \'s\' and computerMove == \'p\':\
        print(\'You win!\')\
        wins = wins + 1\
    elif playerMove == \'r\' and computerMove == \'p\':\
        print(\'You lose!\')\
        losses = losses + 1\
    elif playerMove == \'p\' and computerMove == \'s\':\
        print(\'You lose!\')\
        losses = losses + 1\
    elif playerMove == \'s\' and computerMove == \'r\':\
        print(\'You lose!\')\
        losses = losses + 1

[]{#calibre_link-1065 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Finally, the program compares the strings in
[playerMove]{.literal} and [computerMove]{.literal}, and displays the
results on the screen. It also increments the [wins]{.literal},
[losses]{.literal}, or [ties]{.literal} variable appropriately. Once the
execution reaches the end, it jumps back to the start of the main
program loop to begin another game.

### **Summary** {#calibre_link-130 .h1}

By using expressions that evaluate to [True]{.literal} or
[False]{.literal} (also called conditions), you can write programs that
make decisions on what code to execute and what code to skip. You can
also execute code over and over again in a loop while a certain
condition evaluates to [True]{.literal}. The [break]{.literal} and
[continue]{.literal} statements are useful if you need to exit a loop or
jump back to the loop's start.

These flow control statements will let you write more intelligent
programs. You can also use another type of flow control by writing your
own functions, which is the topic of the next chapter.

### **Practice Questions** {#calibre_link-131 .h1}

[1](#calibre_link-1550){#calibre_link-1260 .calibre6}. What are the two
values of the Boolean data type? How do you write them?

[2](#calibre_link-1551){#calibre_link-1261 .calibre6}. What are the
three Boolean operators?

[3](#calibre_link-1552){#calibre_link-1262 .calibre6}. Write out the
truth tables of each Boolean operator (that is, every possible
combination of Boolean values for the operator and what they evaluate
to).

[4](#calibre_link-1553){#calibre_link-1263 .calibre6}. What do the
following expressions evaluate to?

(5 \> 4) and (3 == 5)\
not (5 \> 4)\
(5 \> 4) or (3 == 5)\
not ((5 \> 4) or (3 == 5))\
(True and True) and (True == False)\
(not False) or (not True)

[5](#calibre_link-1554){#calibre_link-1264 .calibre6}. What are the six
comparison operators?

[6](#calibre_link-1555){#calibre_link-1265 .calibre6}. What is the
difference between the equal to operator and the assignment operator?

[7](#calibre_link-1556){#calibre_link-1266 .calibre6}. Explain what a
condition is and where you would use one.

[8](#calibre_link-1557){#calibre_link-1267 .calibre6}. Identify the
three blocks in this code:

spam = 0\
if spam == 10:\
    print(\'eggs\')\
    if spam \> 5:\
        print(\'bacon\')\
[]{#calibre_link-1725 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    else:\
        print(\'ham\')\
    print(\'spam\')\
print(\'spam\')

[9](#calibre_link-1558){#calibre_link-1268 .calibre6}. Write code that
prints [Hello]{.literal} if [1]{.literal} is stored in [spam]{.literal},
prints [Howdy]{.literal} if [2]{.literal} is stored in [spam]{.literal},
and prints [Greetings!]{.literal} if anything else is stored in
[spam]{.literal}.

[10](#calibre_link-1559){#calibre_link-1269 .calibre6}. What keys can
you press if your program is stuck in an infinite loop?

[11](#calibre_link-1560){#calibre_link-1270 .calibre6}. What is the
difference between [break]{.literal} and [continue]{.literal}?

[12](#calibre_link-1561){#calibre_link-1271 .calibre6}. What is the
difference between [range(10)]{.literal}, [range(0, 10)]{.literal}, and
[range(0, 10, 1)]{.literal} in a [for]{.literal} loop?

[13](#calibre_link-1562){#calibre_link-1272 .calibre6}. Write a short
program that prints the numbers [1]{.literal} to [10]{.literal} using a
[for]{.literal} loop. Then write an equivalent program that prints the
numbers [1]{.literal} to [10]{.literal} using a [while]{.literal} loop.

[14](#calibre_link-1563){#calibre_link-1273 .calibre6}. If you had a
function named [bacon()]{.literal} inside a module named
[spam]{.literal}, how would you call it after importing
[spam]{.literal}?

**Extra credit:** Look up the [round()]{.literal} and [abs()]{.literal}
functions on the internet, and find out what they do. Experiment with
them in the interactive shell.
:::

<div>

[![](/images/prev.png)](../chapter1)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter3)

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
