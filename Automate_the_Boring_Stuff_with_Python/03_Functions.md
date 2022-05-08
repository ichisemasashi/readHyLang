
## []{#calibre_link-1726 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[3]{.big} FUNCTIONS** {#calibre_link-132 .h2b}


![Image](../images/000014.jpg){.calibre3}


You're already familiar with the [print()]{.literal},
[input()]{.literal}, and [len()]{.literal} functions from the previous
chapters. Python provides several built-in functions like these, but you
can also write your own functions. A *function* is like a miniprogram
within a program.

To better understand how functions work, let's create one. Enter this
program into the file editor and save it as *helloFunc.py*:

[➊]{.ent} def hello():\
    [➋]{.ent} print(\'Howdy!\')\
       print(\'Howdy!!!\')\
       print(\'Hello there.\')\
\
[➌]{.ent} hello()\
   hello()\
   hello()

[]{#calibre_link-795 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can view the execution of this program at
*[https://autbor.com/hellofunc/](https://autbor.com/hellofunc/){.calibre6}*.
The first line is a [def]{.literal} statement [➊]{.ent}, which defines a
function named [hello()]{.literal}. The code in the block that follows
the [def]{.literal} statement [➋]{.ent} is the body of the function.
This code is executed when the function is called, not when the function
is first defined.

The [hello()]{.literal} lines after the function [➌]{.ent} are function
calls. In code, a function call is just the function's name followed by
parentheses, possibly with some number of arguments in between the
parentheses. When the program execution reaches these calls, it will
jump to the top line in the function and begin executing the code there.
When it reaches the end of the function, the execution returns to the
line that called the function and continues moving through the code as
before.

Since this program calls [hello()]{.literal} three times, the code in
the [hello()]{.literal} function is executed three times. When you run
this program, the output looks like this:

Howdy!\
Howdy!!!\
Hello there.\
Howdy!\
Howdy!!!\
Hello there.\
Howdy!\
Howdy!!!\
Hello there.

A major purpose of functions is to group code that gets executed
multiple times. Without a function defined, you would have to copy and
paste this code each time, and the program would look like this:

print(\'Howdy!\')\
print(\'Howdy!!!\')\
print(\'Hello there.\')\
print(\'Howdy!\')\
print(\'Howdy!!!\')\
print(\'Hello there.\')\
print(\'Howdy!\')\
print(\'Howdy!!!\')\
print(\'Hello there.\')

In general, you always want to avoid duplicating code because if you
ever decide to update the code---if, for example, you find a bug you
need to fix---you'll have to remember to change the code everywhere you
copied it.

As you get more programming experience, you'll often find yourself
*deduplicating* code, which means getting rid of duplicated or
copy-and-pasted code. Deduplication makes your programs shorter, easier
to read, and easier to update.

### []{#calibre_link-765 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**def Statements with Parameters** {#calibre_link-133 .h1}

When you call the [print()]{.literal} or [len()]{.literal} function, you
pass them values, called *arguments*, by typing them between the
parentheses. You can also define your own functions that accept
arguments. Type this example into the file editor and save it as
*helloFunc2.py*:

[➊]{.ent} def hello(name):\
    [➋]{.ent} print(\'Hello, \' + name)\
\
[➌]{.ent} hello(\'Alice\')\
   hello(\'Bob\')

When you run this program, the output looks like this:

Hello, Alice\
Hello, Bob

You can view the execution of this program at
*[https://autbor.com/hellofunc2/](https://autbor.com/hellofunc2/){.calibre6}*.
The definition of the [hello()]{.literal} function in this program has a
parameter called [name]{.literal} [➊]{.ent}. *Parameters* are variables
that contain arguments. When a function is called with arguments, the
arguments are stored in the parameters. The first time the
[hello()]{.literal} function is called, it is passed the argument
[\'Alice\']{.literal} [➌]{.ent}. The program execution enters the
function, and the parameter [name]{.literal} is automatically set to
[\'Alice\']{.literal}, which is what gets printed by the
[print()]{.literal} statement [➋]{.ent}.

One special thing to note about parameters is that the value stored in a
parameter is forgotten when the function returns. For example, if you
added [print(name)]{.literal} after [hello(\'Bob\')]{.literal} in the
previous program, the program would give you a [NameError]{.literal}
because there is no variable named [name]{.literal}. This variable is
destroyed after the function call [hello(\'Bob\')]{.literal} returns, so
[print(name)]{.literal} would refer to a [name]{.literal} variable that
does not exist.

This is similar to how a program's variables are forgotten when the
program terminates. I'll talk more about why that happens later in the
chapter, when I discuss what a function's local scope is.

#### ***Define, Call, Pass, Argument, Parameter*** {#calibre_link-134 .h2}

The terms *define*, *call*, *pass*, *argument*, and *parameter* can be
confusing. Let's look at a code example to review these terms:

[➊]{.ent} def sayHello(name):\
       print(\'Hello, \' + name)\
[➋]{.ent} sayHello(\'Al\')

To *define* a function is to create it, just like an assignment
statement like [spam = 42]{.literal} creates the [spam]{.literal}
variable. The [def]{.literal} statement defines the
[sayHello()]{.literal} function [➊]{.ent}. The
[sayHello(\'Al\')]{.literal} line [➋]{.ent} *calls* the now-created
function, sending the execution to the top of the function's code. This
function call is also known as *passing* the string value
[\'Al\']{.literal} to the function. A value being []{#calibre_link-937
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}passed to a
function in a function call is an *argument*. The argument
[\'Al\']{.literal} is assigned to a local variable named
[name]{.literal}. Variables that have arguments assigned to them are
*parameters*.

It's easy to mix up these terms, but keeping them straight will ensure
that you know precisely what the text in this chapter means.

### **Return Values and return Statements** {#calibre_link-135 .h1}

When you call the [len()]{.literal} function and pass it an argument
such as [\'Hello\']{.literal}, the function call evaluates to the
integer value [5]{.literal}, which is the length of the string you
passed it. In general, the value that a function call evaluates to is
called the *return value* of the function.

When creating a function using the [def]{.literal} statement, you can
specify what the return value should be with a [return]{.literal}
statement. A [return]{.literal} statement consists of the following:

-   The [return]{.literal} keyword
-   The value or expression that the function should return

When an expression is used with a [return]{.literal} statement, the
return value is what this expression evaluates to. For example, the
following program defines a function that returns a different string
depending on what number it is passed as an argument. Enter this code
into the file editor and save it as *magic8Ball.py*:

[➊]{.ent} import random\
\
[➋]{.ent} def getAnswer(answerNumber):\
    [➌]{.ent} if answerNumber == 1:\
           return \'It is certain\'\
       elif answerNumber == 2:\
           return \'It is decidedly so\'\
       elif answerNumber == 3:\
           return \'Yes\'\
       elif answerNumber == 4:\
           return \'Reply hazy try again\'\
       elif answerNumber == 5:\
           return \'Ask again later\'\
       elif answerNumber == 6:\
           return \'Concentrate and ask again\'\
       elif answerNumber == 7:\
           return \'My reply is no\'\
       elif answerNumber == 8:\
           return \'Outlook not so good\'\
       elif answerNumber == 9:\
           return \'Very doubtful\'\
\
[➍]{.ent} r = random.randint(1, 9)\
[➎]{.ent} fortune = getAnswer(r)\
[➏]{.ent} print(fortune)

[]{#calibre_link-887 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can view the execution of this program at
*[https://autbor.com/magic8ball/](https://autbor.com/magic8ball/){.calibre6}*.
When this program starts, Python first imports the [random]{.literal}
module [➊]{.ent}. Then the [getAnswer()]{.literal} function is defined
[➋]{.ent}. Because the function is being defined (and not called), the
execution skips over the code in it. Next, the
[random.randint()]{.literal} function is called with two arguments:
[1]{.literal} and [9]{.literal} [➍]{.ent}. It evaluates to a random
integer between [1]{.literal} and [9]{.literal} (including [1]{.literal}
and [9]{.literal} themselves), and this value is stored in a variable
named [r]{.literal}.

The [getAnswer()]{.literal} function is called with [r]{.literal} as the
argument [➎]{.ent}. The program execution moves to the top of the
[getAnswer()]{.literal} function [➌]{.ent}, and the value [r]{.literal}
is stored in a parameter named [answerNumber]{.literal}. Then, depending
on the value in [answerNumber]{.literal}, the function returns one of
many possible string values. The program execution returns to the line
at the bottom of the program that originally called
[getAnswer()]{.literal} [➎]{.ent}. The returned string is assigned to a
variable named [fortune]{.literal}, which then gets passed to a
[print()]{.literal} call [➏]{.ent} and is printed to the screen.

Note that since you can pass return values as an argument to another
function call, you could shorten these three lines:

r = random.randint(1, 9)\
fortune = getAnswer(r)\
print(fortune)

to this single equivalent line:

print(getAnswer(random.randint(1, 9)))

Remember, expressions are composed of values and operators. A function
call can be used in an expression because the call evaluates to its
return value.

### **The None Value** {#calibre_link-136 .h1}

In Python, there is a value called [None]{.literal}, which represents
the absence of a value. The [None]{.literal} value is the only value of
the [NoneType]{.literal} data type. (Other programming languages might
call this value [null]{.literal}, [nil]{.literal}, or
[undefined]{.literal}.) Just like the Boolean [True]{.literal} and
[False]{.literal} values, [None]{.literal} must be typed with a capital
*N*.

This value-without-a-value can be helpful when you need to store
something that won't be confused for a real value in a variable. One
place where [None]{.literal} is used is as the return value of
[print()]{.literal}. The [print()]{.literal} function displays text on
the screen, but it doesn't need to return anything in the same way
[len()]{.literal} or [input()]{.literal} does. But since all function
calls need to evaluate to a return value, [print()]{.literal} returns
[None]{.literal}. To see this in action, enter the following into the
interactive shell:

\>\>\> [spam = print(\'Hello!\')]{.codestrong1}\
Hello!\
\>\>\> [None == spam]{.codestrong1}\
True

[]{#calibre_link-796 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Behind the scenes, Python adds [return
None]{.literal} to the end of any function definition with no
[return]{.literal} statement. This is similar to how a [while]{.literal}
or [for]{.literal} loop implicitly ends with a [continue]{.literal}
statement. Also, if you use a [return]{.literal} statement without a
value (that is, just the [return]{.literal} keyword by itself), then
[None]{.literal} is returned.

### **Keyword Arguments and the print() Function** {#calibre_link-137 .h1}

Most arguments are identified by their position in the function call.
For example, [random.randint(1, 10)]{.literal} is different from
[random.randint(10, 1)]{.literal}. The function call [random.randint(1,
10)]{.literal} will return a random integer between [1]{.literal} and
[10]{.literal} because the first argument is the low end of the range
and the second argument is the high end (while [random.randint(10,
1)]{.literal} causes an error).

However, rather than through their position, *keyword arguments* are
identified by the keyword put before them in the function call. Keyword
arguments are often used for *optional parameters*. For example, the
[print()]{.literal} function has the optional parameters [end]{.literal}
and [sep]{.literal} to specify what should be printed at the end of its
arguments and between its arguments (separating them), respectively.

If you ran a program with the following code:

[print(\'Hello\')]{.codestrong1}\
[print(\'World\')]{.codestrong1}

the output would look like this:

Hello\
World

The two outputted strings appear on separate lines because the
[print()]{.literal} function automatically adds a newline character to
the end of the string it is passed. However, you can set the
[end]{.literal} keyword argument to change the newline character to a
different string. For example, if the code were this:

[print(\'Hello\', end=\'\')]{.codestrong1}\
[print(\'World\')]{.codestrong1}

the output would look like this:

HelloWorld

The output is printed on a single line because there is no longer a
newline printed after [\'Hello\']{.literal}. Instead, the blank string
is printed. This is useful if you need to disable the newline that gets
added to the end of every [print()]{.literal} function call.

[]{#calibre_link-828 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Similarly, when you pass multiple string values to
[print()]{.literal}, the function will automatically separate them with
a single space. Enter the following into the interactive shell:

\>\>\> [print(\'cats\', \'dogs\', \'mice\')]{.codestrong1}\
cats dogs mice

But you could replace the default separating string by passing the
[sep]{.literal} keyword argument a different string. Enter the following
into the interactive shell:

\>\>\> [print(\'cats\', \'dogs\', \'mice\', sep=\',\')]{.codestrong1}\
cats,dogs,mice

You can add keyword arguments to the functions you write as well, but
first you'll have to learn about the list and dictionary data types in
the next two chapters. For now, just know that some functions have
optional keyword arguments that can be specified when the function is
called.

### **The Call Stack** {#calibre_link-138 .h1}

Imagine that you have a meandering conversation with someone. You talk
about your friend Alice, which then reminds you of a story about your
coworker Bob, but first you have to explain something about your cousin
Carol. You finish you story about Carol and go back to talking about
Bob, and when you finish your story about Bob, you go back to talking
about Alice. But then you are reminded about your brother David, so you
tell a story about him, and then get back to finishing your original
story about Alice. Your conversation followed a *stack*-like structure,
like in [Figure 3-1](#calibre_link-1213){.calibre6}. The conversation is
stack-like because the current topic is always at the top of the stack.


[]{#calibre_link-1213
.calibre6}![image](../images/000109.jpg){.calibre3}


*Figure 3-1: Your meandering conversation stack*

Similar to our meandering conversation, calling a function doesn't send
the execution on a one-way trip to the top of a function. Python will
remember which line of code called the function so that the execution
can return there when it encounters a [return]{.literal} statement. If
that original function called other functions, the execution would
return to *those* function calls first, before returning from the
original function call.

[]{#calibre_link-1727 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Open a file editor window and enter the following
code, saving it as *abcdCallStack.py*:

   def a():\
       print(\'a() starts\')\
    [➊]{.ent} b()\
    [➋]{.ent} d()\
       print(\'a() returns\')\
\
   def b():\
       print(\'b() starts\')\
    [➌]{.ent} c()\
       print(\'b() returns\')\
\
   def c():\
    [➍]{.ent} print(\'c() starts\')\
       print(\'c() returns\')\
\
   def d():\
       print(\'d() starts\')\
       print(\'d() returns\')\
\
[➎]{.ent} a()

If you run this program, the output will look like this:

a() starts\
b() starts\
c() starts\
c() returns\
b() returns\
d() starts\
d() returns\
a() returns

You can view the execution of this program at
*[https://autbor.com/abcdcallstack/](https://autbor.com/abcdcallstack/){.calibre6}*.
When [a()]{.literal} is called [➎]{.ent}, it calls [b()]{.literal}
[➊]{.ent}, which in turn calls [c()]{.literal} [➌]{.ent}. The
[c()]{.literal} function doesn't call anything; it just displays [c()
starts]{.literal} [➍]{.ent} and [c() returns]{.literal} before returning
to the line in [b()]{.literal} that called it [➌]{.ent}. Once execution
returns to the code in [b()]{.literal} that called [c()]{.literal}, it
returns to the line in [a()]{.literal} that called [b()]{.literal}
[➊]{.ent}. The execution continues to the next line in the
[b()]{.literal} function [➋]{.ent}, which is a call to [d()]{.literal}.
Like the [c()]{.literal} function, the [d()]{.literal} function also
doesn't call anything. It just displays [d() starts]{.literal} and [d()
returns]{.literal} before returning to the line in [b()]{.literal} that
called it. Since [b()]{.literal} contains no other code, the execution
returns to the line in [a()]{.literal} that called [b()]{.literal}
[➋]{.ent}. The last line in [a()]{.literal} displays [a()
returns]{.literal} before returning to the original [a()]{.literal} call
at the end of the program [➎]{.ent}.

The *call stack* is how Python remembers where to return the execution
after each function call. The call stack isn't stored in a variable in
your program; rather, Python handles it behind the scenes. When your
program calls a function, Python creates a *frame object* on the top of
the call stack. Frame []{#calibre_link-829 {http:="" www.idpf.org=""
2007="" ops}type="pagebreak"}objects store the line number of the
original function call so that Python can remember where to return. If
another function call is made, Python puts another frame object on the
call stack above the other one.

When a function call returns, Python removes a frame object from the top
of the stack and moves the execution to the line number stored in it.
Note that frame objects are always added and removed from the top of the
stack and not from any other place. [Figure
3-2](#calibre_link-1214){.calibre6} illustrates the state of the call
stack in *abcdCallStack.py* as each function is called and returns.


[]{#calibre_link-1214
.calibre6}![image](../images/000054.jpg){.calibre3}


*Figure 3-2: The frame objects of the call stack as* abcdCallStack.py
*calls and returns from functions*

The top of the call stack is which function the execution is currently
in. When the call stack is empty, the execution is on a line outside of
all functions.

The call stack is a technical detail that you don't strictly need to
know about to write programs. It's enough to understand that function
calls return to the line number they were called from. However,
understanding call stacks makes it easier to understand local and global
scopes, described in the next section.

### **Local and Global Scope** {#calibre_link-139 .h1}

Parameters and variables that are assigned in a called function are said
to exist in that function's *local scope*. Variables that are assigned
outside all functions are said to exist in the *global scope*. A
variable that exists in a local scope is called a *local variable*,
while a variable that exists in the global scope is called a *global
variable*. A variable must be one or the other; it cannot be both local
and global.

Think of a *scope* as a container for variables. When a scope is
destroyed, all the values stored in the scope's variables are forgotten.
There is only one global scope, and it is created when your program
begins. When your program terminates, the global scope is destroyed, and
all its variables are forgotten. Otherwise, the next time you ran a
program, the variables would remember their values from the last time
you ran it.

A local scope is created whenever a function is called. Any variables
assigned in the function exist within the function's local scope. When
the function returns, the local scope is destroyed, and these variables
are forgotten. The next time you call the function, the local variables
will not remember the values stored in them from the last time the
function was called. Local variables are also stored in frame objects on
the call stack.

[]{#calibre_link-984 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Scopes matter for several reasons:

-   Code in the global scope, outside of all functions, cannot use any
    local variables.
-   However, code in a local scope can access global variables.
-   Code in a function's local scope cannot use variables in any other
    local scope.
-   You can use the same name for different variables if they are in
    different scopes. That is, there can be a local variable named
    [spam]{.literal} and a global variable also named [spam]{.literal}.

The reason Python has different scopes instead of just making everything
a global variable is so that when variables are modified by the code in
a particular call to a function, the function interacts with the rest of
the program only through its parameters and the return value. This
narrows down the number of lines of code that may be causing a bug. If
your program contained nothing but global variables and had a bug
because of a variable being set to a bad value, then it would be hard to
track down where this bad value was set. It could have been set from
anywhere in the program, and your program could be hundreds or thousands
of lines long! But if the bug is caused by a local variable with a bad
value, you know that only the code in that one function could have set
it incorrectly.

While using global variables in small programs is fine, it is a bad
habit to rely on global variables as your programs get larger and
larger.

#### ***Local Variables Cannot Be Used in the Global Scope*** {#calibre_link-140 .h2}

Consider this program, which will cause an error when you run it:

def spam():\
[➊]{.ent} eggs = 31337\
spam()\
print(eggs)

If you run this program, the output will look like this:

Traceback (most recent call last):\
  File \"C:/test1.py\", line 4, in \<module\>\
    print(eggs)\
NameError: name \'eggs\' is not defined

The error happens because the [eggs]{.literal} variable exists only in
the local scope created when [spam()]{.literal} is called [➊]{.ent}.
Once the program execution returns from [spam]{.literal}, that local
scope is destroyed, and there is no longer a variable named
[eggs]{.literal}. So when your program tries to run
[print(eggs)]{.literal}, Python gives you an error saying that
[eggs]{.literal} is not defined. This makes sense if you think about it;
when the program execution is in the global scope, no local scopes
exist, so there can't be any local variables. This is why only global
variables can be used in the global scope.

#### []{#calibre_link-986 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Local Scopes Cannot Use Variables in Other Local Scopes*** {#calibre_link-141 .h2}

A new local scope is created whenever a function is called, including
when a function is called from another function. Consider this program:

  def spam():\
    [➊]{.ent} eggs = 99\
    [➋]{.ent} bacon()\
    [➌]{.ent} print(eggs)\
\
   def bacon():\
       ham = 101\
    [➍]{.ent} eggs = 0\
\
[➎]{.ent} spam()

You can view the execution of this program at
*[https://autbor.com/otherlocalscopes/](https://autbor.com/otherlocalscopes/){.calibre6}*.
When the program starts, the [spam()]{.literal} function is called
[➎]{.ent}, and a local scope is created. The local variable
[eggs]{.literal} [➊]{.ent} is set to [99]{.literal}. Then the
[bacon()]{.literal} function is called [➋]{.ent}, and a second local
scope is created. Multiple local scopes can exist at the same time. In
this new local scope, the local variable [ham]{.literal} is set to
[101]{.literal}, and a local variable [eggs]{.literal}---which is
different from the one in [spam()]{.literal}'s local scope---is also
created [➍]{.ent} and set to [0]{.literal}.

When [bacon()]{.literal} returns, the local scope for that call is
destroyed, including its [eggs]{.literal} variable. The program
execution continues in the [spam()]{.literal} function to print the
value of [eggs]{.literal} [➌]{.ent}. Since the local scope for the call
to [spam()]{.literal} still exists, the only [eggs]{.literal} variable
is the [spam()]{.literal} function's [eggs]{.literal} variable, which
was set to [99]{.literal}. This is what the program prints.

The upshot is that local variables in one function are completely
separate from the local variables in another function.

#### ***Global Variables Can Be Read from a Local Scope*** {#calibre_link-142 .h2}

Consider the following program:

def spam():\
    print(eggs)\
eggs = 42\
spam()\
print(eggs)

You can view the execution of this program at
*[https://autbor.com/readglobal/](https://autbor.com/readglobal/){.calibre6}*.
Since there is no parameter named [eggs]{.literal} or any code that
assigns [eggs]{.literal} a value in the [spam()]{.literal} function,
when [eggs]{.literal} is used in [spam()]{.literal}, Python considers it
a reference to the global variable [eggs]{.literal}. This is why
[42]{.literal} is printed when the previous program is run.

#### []{#calibre_link-985 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Local and Global Variables with the Same Name*** {#calibre_link-143 .h2}

Technically, it's perfectly acceptable to use the same variable name for
a global variable and local variables in different scopes in Python.
But, to simplify your life, avoid doing this. To see what happens, enter
the following code into the file editor and save it as
*localGlobalSameName.py*:

   def spam():\
    [➊]{.ent} eggs = \'spam local\'\
       print(eggs)    # prints \'spam local\'\
\
   def bacon():\
    [➋]{.ent} eggs = \'bacon local\'\
       print(eggs)    # prints \'bacon local\'\
       spam()\
       print(eggs)    # prints \'bacon local\'\
\
[➌]{.ent} eggs = \'global\'\
   bacon()\
   print(eggs)        # prints \'global\'

When you run this program, it outputs the following:

bacon local\
spam local\
bacon local\
global

You can view the execution of this program at
*[https://autbor.com/localglobalsamename/](https://autbor.com/localglobalsamename/){.calibre6}*.
There are actually three different variables in this program, but
confusingly they are all named [eggs]{.literal}. The variables are as
follows:

[➊]{.ent} A variable named [eggs]{.literal} that exists in a local scope
when [spam()]{.literal} is called.

[➋]{.ent} A variable named [eggs]{.literal} that exists in a local scope
when [bacon()]{.literal} is called.

[➌]{.ent} A variable named [eggs]{.literal} that exists in the global
scope.

Since these three separate variables all have the same name, it can be
confusing to keep track of which one is being used at any given time.
This is why you should avoid using the same variable name in different
scopes.

### **The global Statement** {#calibre_link-144 .h1}

If you need to modify a global variable from within a function, use the
[global]{.literal} statement. If you have a line such as [global
eggs]{.literal} at the top of a function, it tells Python, "In this
function, [eggs]{.literal} refers to the global variable, so don't
create a local variable with this name." For example, enter the
following code into the file editor and save it as *globalStatement.py*:

def spam():\
  [➊]{.ent} global eggs\
  [➋]{.ent} eggs = \'spam\'\
[]{#calibre_link-1728 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\
eggs = \'global\'\
spam()\
print(eggs)

When you run this program, the final [print()]{.literal} call will
output this:

spam

You can view the execution of this program at
*[https://autbor.com/globalstatement/](https://autbor.com/globalstatement/){.calibre6}*.
Because [eggs]{.literal} is declared [global]{.literal} at the top of
[spam()]{.literal} [➊]{.ent}, when [eggs]{.literal} is set to
[\'spam\']{.literal} [➋]{.ent}, this assignment is done to the globally
scoped [eggs]{.literal}. No local [eggs]{.literal} variable is created.

There are four rules to tell whether a variable is in a local scope or
global scope:

-   If a variable is being used in the global scope (that is, outside of
    all functions), then it is always a global variable.
-   If there is a [global]{.literal} statement for that variable in a
    function, it is a global variable.
-   Otherwise, if the variable is used in an assignment statement in the
    function, it is a local variable.
-   But if the variable is not used in an assignment statement, it is a
    global variable.

To get a better feel for these rules, here's an example program. Enter
the following code into the file editor and save it as
*sameNameLocalGlobal.py*:

def spam():\
  [➊]{.ent} global eggs\
     eggs = \'spam\' \# this is the global\
\
def bacon():\
  [➋]{.ent} eggs = \'bacon\' \# this is a local\
\
def ham():\
  [➌]{.ent} print(eggs) \# this is the global\
\
eggs = 42 \# this is the global\
spam()\
print(eggs)

In the [spam()]{.literal} function, [eggs]{.literal} is the global
[eggs]{.literal} variable because there's a [global]{.literal} statement
for [eggs]{.literal} at the beginning of the function [➊]{.ent}. In
[bacon()]{.literal}, [eggs]{.literal} is a local variable because
there's an assignment statement for it in that function [➋]{.ent}. In
[ham()]{.literal} [➌]{.ent}, [eggs]{.literal} is the global variable
because there is no assignment statement or [global]{.literal} statement
for it in that function. If you run *sameNameLocalGlobal.py*, the output
will look like this:

spam

[]{#calibre_link-977 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can view the execution of this program at
*[https://autbor.com/sameNameLocalGlobal/](https://autbor.com/sameNameLocalGlobal/){.calibre6}*.
In a function, a variable will either always be global or always be
local. The code in a function can't use a local variable named
[eggs]{.literal} and then use the global [eggs]{.literal} variable later
in that same function.


**[NOTE]{.notes}**

*If you ever want to modify the value stored in a global variable from
in a function, you must use a [global]{.codeitalic} statement on that
variable.*


If you try to use a local variable in a function before you assign a
value to it, as in the following program, Python will give you an error.
To see this, enter the following into the file editor and save it as
*sameNameError.py*:

   def spam():\
       print(eggs) \# ERROR!\
    [➊]{.ent} eggs = \'spam local\'\
\
[➋]{.ent} eggs = \'global\'\
   spam()

If you run the previous program, it produces an error message.

Traceback (most recent call last):\
  File \"C:/sameNameError.py\", line 6, in \<module\>\
    spam()\
  File \"C:/sameNameError.py\", line 2, in spam\
    print(eggs) \# ERROR!\
UnboundLocalError: local variable \'eggs\' referenced before assignment

You can view the execution of this program at
*[https://autbor.com/sameNameError/](https://autbor.com/sameNameError/){.calibre6}*.
This error happens because Python sees that there is an assignment
statement for [eggs]{.literal} in the [spam()]{.literal} function
[➊]{.ent} and, therefore, considers [eggs]{.literal} to be local. But
because [print(eggs)]{.literal} is executed before [eggs]{.literal} is
assigned anything, the local variable [eggs]{.literal} doesn't exist.
Python will *not* fall back to using the global [eggs]{.literal}
variable [➋]{.ent}.


**FUNCTIONS AS "BLACK BOXES"**

Often, all you need to know about a function are its inputs (the
parameters) and output value; you don't always have to burden yourself
with how the function's code actually works. When you think about
functions in this high-level way, it's common to say that you're
treating a function as a "black box."

This idea is fundamental to modern programming. Later chapters in this
book will show you several modules with functions that were written by
other people. While you can take a peek at the source code if you're
curious, you don't need to know how these functions work in order to use
them. And because writing functions without global variables is
encouraged, you usually don't have to worry about the function's code
interacting with the rest of your program.


### []{#calibre_link-945 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Exception Handling** {#calibre_link-145 .h1}

Right now, getting an error, or *exception*, in your Python program
means the entire program will crash. You don't want this to happen in
real-world programs. Instead, you want the program to detect errors,
handle them, and then continue to run.

For example, consider the following program, which has a divide-by-zero
error. Open a file editor window and enter the following code, saving it
as *zeroDivide.py*:

def spam(divideBy):\
    return 42 / divideBy\
\
print(spam(2))\
print(spam(12))\
print(spam(0))\
print(spam(1))

We've defined a function called [spam]{.literal}, given it a parameter,
and then printed the value of that function with various parameters to
see what happens. This is the output you get when you run the previous
code:

21.0\
3.5\
Traceback (most recent call last):\
  File \"C:/zeroDivide.py\", line 6, in \<module\>\
    print(spam(0))\
  File \"C:/zeroDivide.py\", line 2, in spam\
    return 42 / divideBy\
ZeroDivisionError: division by zero

You can view the execution of this program at
*[https://autbor.com/zerodivide/](https://autbor.com/zerodivide/){.calibre6}*.
A [ZeroDivisionError]{.literal} happens whenever you try to divide a
number by zero. From the line number given in the error message, you
know that the [return]{.literal} statement in [spam()]{.literal} is
causing an error.

Errors can be handled with [try]{.literal} and [except]{.literal}
statements. The code that could potentially have an error is put in a
[try]{.literal} clause. The program execution moves to the start of a
following [except]{.literal} clause if an error happens.

You can put the previous divide-by-zero code in a [try]{.literal} clause
and have an [except]{.literal} clause contain code to handle what
happens when this error occurs.

def spam(divideBy):\
    try:\
        return 42 / divideBy\
    except ZeroDivisionError:\
        print(\'Error: Invalid argument.\')\
\
print(spam(2))\
print(spam(12))\
print(spam(0))\
print(spam(1))

[]{#calibre_link-946 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}When code in a [try]{.literal} clause causes an
error, the program execution immediately moves to the code in the
[except]{.literal} clause. After running that code, the execution
continues as normal. The output of the previous program is as follows:

21.0\
3.5\
Error: Invalid argument.\
None\
42.0

You can view the execution of this program at
*[https://autbor.com/tryexceptzerodivide/](https://autbor.com/tryexceptzerodivide/){.calibre6}*.
Note that any errors that occur in function calls in a [try]{.literal}
block will also be caught. Consider the following program, which instead
has the [spam()]{.literal} calls in the [try]{.literal} block:

def spam(divideBy):\
    return 42 / divideBy\
\
try:\
    print(spam(2))\
    print(spam(12))\
    print(spam(0))\
    print(spam(1))\
except ZeroDivisionError:\
    print(\'Error: Invalid argument.\')

When this program is run, the output looks like this:

21.0\
3.5\
Error: Invalid argument.

You can view the execution of this program at
*[https://autbor.com/spamintry/](https://autbor.com/spamintry/){.calibre6}*.
The reason [print(spam(1))]{.literal} is never executed is because once
the execution jumps to the code in the [except]{.literal} clause, it
does not return to the [try]{.literal} clause. Instead, it just
continues moving down the program as normal.

### **A Short Program: Zigzag** {#calibre_link-146 .h1}

Let's use the programming concepts you've learned so far to create a
small animation program. This program will create a back-and-forth,
zigzag pattern until the user stops it by pressing the Mu editor's Stop
button or by pressing [CTRL-C]{.small}. When you run this program, the
output will look something like this:

    \*\*\*\*\*\*\*\*\
   \*\*\*\*\*\*\*\*\
  \*\*\*\*\*\*\*\*\
[]{#calibre_link-1729 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"} \*\*\*\*\*\*\*\*\
\*\*\*\*\*\*\*\*\
 \*\*\*\*\*\*\*\*\
  \*\*\*\*\*\*\*\*\
   \*\*\*\*\*\*\*\*\
    \*\*\*\*\*\*\*\*

Type the following source code into the file editor, and save the file
as *zigzag.py*:

import time, sys\
indent = 0 \# How many spaces to indent.\
indentIncreasing = True \# Whether the indentation is increasing or
not.\
\
try:\
    while True: \# The main program loop.\
        print(\' \' \* indent, end=\'\')\
        print(\'\*\*\*\*\*\*\*\*\')\
        time.sleep(0.1) \# Pause for 1/10 of a second.\
\
        if indentIncreasing:\
            # Increase the number of spaces:\
            indent = indent + 1\
            if indent == 20:\
                # Change direction:\
                indentIncreasing = False\
\
        else:\
            # Decrease the number of spaces:\
            indent = indent - 1\
            if indent == 0:\
                # Change direction:\
                indentIncreasing = True\
except KeyboardInterrupt:\
    sys.exit()

Let's look at this code line by line, starting at the top.

import time, sys\
indent = 0 \# How many spaces to indent.\
indentIncreasing = True \# Whether the indentation is increasing or not.

First, we'll import the [time]{.literal} and [sys]{.literal} modules.
Our program uses two variables: the [indent]{.literal} variable keeps
track of how many spaces of indentation are before the band of eight
asterisks and [indentIncreasing]{.literal} contains a Boolean value to
determine if the amount of indentation is increasing or decreasing.

try:\
    while True: \# The main program loop.\
        print(\' \' \* indent, end=\'\')\
        print(\'\*\*\*\*\*\*\*\*\')\
        time.sleep(0.1) \# Pause for 1/10 of a second.

[]{#calibre_link-1070 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Next, we place the rest of the program inside a try
statement. When the user presses [CTRL-C]{.small} while a Python program
is running, Python raises the [KeyboardInterrupt]{.literal} exception.
If there is no [try]{.literal}-[except]{.literal} statement to catch
this exception, the program crashes with an ugly error message. However,
for our program, we want it to cleanly handle the
[KeyboardInterrupt]{.literal} exception by calling
[sys.exit()]{.literal}. (The code for this is in the [except]{.literal}
statement at the end of the program.)

The [while True:]{.literal} infinite loop will repeat the instructions
in our program forever. This involves using [\' \' \* indent]{.literal}
to print the correct amount of spaces of indentation. We don't want to
automatically print a newline after these spaces, so we also pass
[end=\'\']{.literal} to the first [print()]{.literal} call. A second
[print()]{.literal} call prints the band of asterisks. The
[time.sleep()]{.literal} function hasn't been covered yet, but suffice
it to say that it introduces a one-tenth-second pause in our program at
this point.

        if indentIncreasing:\
            # Increase the number of spaces:\
            indent = indent + 1\
            if indent == 20:\
                indentIncreasing = False \# Change direction.

Next, we want to adjust the amount of indentation for the next time we
print asterisks. If [indentIncreasing]{.literal} is [True]{.literal},
then we want to add one to [indent]{.literal}. But once indent reaches
[20]{.literal}, we want the indentation to decrease.

        else:\
            # Decrease the number of spaces:\
            indent = indent - 1\
            if indent == 0:\
                indentIncreasing = True \# Change direction.

Meanwhile, if [indentIncreasing]{.literal} was [False]{.literal}, we
want to subtract one from [indent]{.literal}. Once indent reaches
[0]{.literal}, we want the indentation to increase once again. Either
way, the program execution will jump back to the start of the main
program loop to print the asterisks again.

except KeyboardInterrupt:\
    sys.exit()

If the user presses [CTRL-C]{.small} at any point that the program
execution is in the [try]{.literal} block, the
[KeyboardInterrrupt]{.literal} exception is raised and handled by this
[except]{.literal} statement. The program execution moves inside the
[except]{.literal} block, which runs [sys.exit()]{.literal} and quits
the program. This way, even though the main program loop is an infinite
loop, the user has a way to shut down the program.

### []{#calibre_link-1730 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Summary** {#calibre_link-147 .h1}

Functions are the primary way to compartmentalize your code into logical
groups. Since the variables in functions exist in their own local
scopes, the code in one function cannot directly affect the values of
variables in other functions. This limits what code could be changing
the values of your variables, which can be helpful when it comes to
debugging your code.

Functions are a great tool to help you organize your code. You can think
of them as black boxes: they have inputs in the form of parameters and
outputs in the form of return values, and the code in them doesn't
affect variables in other functions.

In previous chapters, a single error could cause your programs to crash.
In this chapter, you learned about [try]{.literal} and
[except]{.literal} statements, which can run code when an error has been
detected. This can make your programs more resilient to common error
cases.

### **Practice Questions** {#calibre_link-148 .h1}

[1](#calibre_link-1215){#calibre_link-1274 .calibre6}. Why are functions
advantageous to have in your programs?

[2](#calibre_link-1216){#calibre_link-1275 .calibre6}. When does the
code in a function execute: when the function is defined or when the
function is called?

[3](#calibre_link-1217){#calibre_link-1276 .calibre6}. What statement
creates a function?

[4](#calibre_link-1218){#calibre_link-1277 .calibre6}. What is the
difference between a function and a function call?

[5](#calibre_link-1219){#calibre_link-1278 .calibre6}. How many global
scopes are there in a Python program? How many local scopes?

[6](#calibre_link-1220){#calibre_link-1279 .calibre6}. What happens to
variables in a local scope when the function call returns?

[7](#calibre_link-1221){#calibre_link-1280 .calibre6}. What is a return
value? Can a return value be part of an expression?

[8](#calibre_link-1222){#calibre_link-1281 .calibre6}. If a function
does not have a return statement, what is the return value of a call to
that function?

[9](#calibre_link-1223){#calibre_link-1282 .calibre6}. How can you force
a variable in a function to refer to the global variable?

[10](#calibre_link-1224){#calibre_link-1283 .calibre6}. What is the data
type of [None]{.literal}?

[11](#calibre_link-1225){#calibre_link-1284 .calibre6}. What does the
[import areallyourpetsnamederic]{.literal} statement do?

[12](#calibre_link-1226){#calibre_link-1285 .calibre6}. If you had a
function named [bacon()]{.literal} in a module named [spam]{.literal},
how would you call it after importing [spam]{.literal}?

[13](#calibre_link-1227){#calibre_link-1286 .calibre6}. How can you
prevent a program from crashing when it gets an error?

[14](#calibre_link-1228){#calibre_link-1287 .calibre6}. What goes in the
[try]{.literal} clause? What goes in the [except]{.literal} clause?

### []{#calibre_link-1090 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Practice Projects** {#calibre_link-149 .h1}

For practice, write programs to do the following tasks.

#### ***The Collatz Sequence*** {#calibre_link-150 .h2}

Write a function named [collatz()]{.literal} that has one parameter
named [number]{.literal}. If [number]{.literal} is even, then
[collatz()]{.literal} should print [number // 2]{.literal} and return
this value. If [number]{.literal} is odd, then [collatz()]{.literal}
should print and return [3 \* number + 1]{.literal}.

Then write a program that lets the user type in an integer and that
keeps calling [collatz()]{.literal} on that number until the function
returns the value [1]{.literal}. (Amazingly enough, this sequence
actually works for any integer---sooner or later, using this sequence,
you'll arrive at 1! Even mathematicians aren't sure why. Your program is
exploring what's called the *Collatz sequence*, sometimes called "the
simplest impossible math problem.")

Remember to convert the return value from [input()]{.literal} to an
integer with the [int()]{.literal} function; otherwise, it will be a
string value.

Hint: An integer [number]{.literal} is even if [number % 2 ==
0]{.literal}, and it's odd if [number % 2 == 1]{.literal}.

The output of this program could look something like this:

Enter number:\
3\
10\
5\
16\
8\
4\
2\
1

#### ***Input Validation*** {#calibre_link-151 .h2}

Add [try]{.literal} and [except]{.literal} statements to the previous
project to detect whether the user types in a noninteger string.
Normally, the [int()]{.literal} function will raise a
[ValueError]{.literal} error if it is passed a noninteger string, as in
[int(\'puppy\')]{.literal}. In the [except]{.literal} clause, print a
message to the user saying they must enter an integer.


