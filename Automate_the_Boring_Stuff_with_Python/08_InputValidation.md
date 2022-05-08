
## **INPUT VALIDATION**


![Image](../images/000031.jpg){.calibre3}


*Input validation* code checks that values entered by the user, such as
text from the [input()]{.literal} function, are formatted correctly. For
example, if you want users to enter their ages, your code shouldn't
accept nonsensical answers such as negative numbers (which are outside
the range of acceptable integers) or words (which are the wrong data
type). Input validation can also prevent bugs or security
vulnerabilities. If you implement a [withdrawFromAccount()]{.literal}
function that takes an argument for the amount to subtract from an
account, you need to ensure the amount is a positive number. If the
[withdrawFromAccount()]{.literal} function subtracts a negative number
from the account, the "withdrawal" will end up adding money!

[]{#calibre_link-1072 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Typically, we perform input validation by
repeatedly asking the user for input until they enter valid text, as in
the following example:

while True:\
    print(\'Enter your age:\')\
    age = input()\
    try:\
        age = int(age)\
    except:\
        print(\'Please use numeric digits.\')\
        continue\
    if age \< 1:\
        print(\'Please enter a positive number.\')\
        continue\
    break\
\
print(f\'Your age is {age}.\')

When you run this program, the output could look like this:

Enter your age:\
[five]{.codestrong1}\
Please use numeric digits.\
Enter your age:\
[-2]{.codestrong1}\
Please enter a positive number.\
Enter your age:\
[30]{.codestrong1}\
Your age is 30.

When you run this code, you'll be prompted for your age until you enter
a valid one. This ensures that by the time the execution leaves the
[while]{.literal} loop, the [age]{.literal} variable will contain a
valid value that won't crash the program later on.

However, writing input validation code for every [input()]{.literal}
call in your program quickly becomes tedious. Also, you may miss certain
cases and allow invalid input to pass through your checks. In this
chapter, you'll learn how to use the third-party PyInputPlus module for
input validation.

### **The PyInputPlus Module** {#calibre_link-277 .h1}

PyInputPlus contains functions similar to [input()]{.literal} for
several kinds of data: numbers, dates, email addresses, and more. If the
user ever enters invalid input, such as a badly formatted date or a
number that is outside of an intended range, PyInputPlus will reprompt
them for input just like our code in the previous section did.
PyInputPlus also has other useful features like a limit for the number
of times it reprompts users and a timeout if users are required to
respond within a time limit.

[]{#calibre_link-1011 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}PyInputPlus is not a part of the Python Standard
Library, so you must install it separately using Pip. To install
PyInputPlus, run [pip install \--user pyinputplus]{.literal} from the
command line. [Appendix A](#calibre_link-2){.calibre6} has complete
instructions for installing third-party modules. To check if PyInputPlus
installed correctly, import it in the interactive shell:

\>\>\> [import pyinputplus]{.codestrong1}

If no errors appear when you import the module, it has been successfully
installed.

PyInputPlus has several functions for different kinds of input:

[inputStr()]{.codestrong} Is like the built-in [input()]{.literal}
function but has the general PyInputPlus features. You can also pass a
custom validation function to it

[inputNum()]{.codestrong} Ensures the user enters a number and returns
an int or float, depending on if the number has a decimal point in it

[inputChoice()]{.codestrong} Ensures the user enters one of the provided
choices

[inputMenu()]{.codestrong} Is similar to [inputChoice()]{.literal}, but
provides a menu with numbered or lettered options

[inputDatetime()]{.codestrong} Ensures the user enters a date and time

[inputYesNo()]{.codestrong} Ensures the user enters a "yes" or "no"
response

[inputBool()]{.codestrong} Is similar to [inputYesNo()]{.literal}, but
takes a "True" or "False" response and returns a Boolean value

[inputEmail()]{.codestrong} Ensures the user enters a valid email
address

[inputFilepath()]{.codestrong} Ensures the user enters a valid file path
and filename, and can optionally check that a file with that name exists

[inputPassword()]{.codestrong} Is like the built-in [input()]{.literal},
but displays \* characters as the user types so that passwords, or other
sensitive information, aren't displayed on the screen

These functions will automatically reprompt the user for as long as they
enter invalid input:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response = pyip.inputNum()]{.codestrong1}\
[five]{.codestrong1}\
\'five\' is not a number.\
[42]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
42

The [as pyip]{.literal} code in the [import]{.literal} statement saves
us from typing [pyinputplus]{.literal} each time we want to call a
PyInputPlus function. Instead we can use the shorter [pyip]{.literal}
name. If you take a look at the example, you see that unlike
[input()]{.literal}, these functions return an [int]{.literal} or
[float]{.literal} value: [42]{.literal} and [3.14]{.literal} instead of
the strings [\'42\']{.literal} and [\'3.14\']{.literal}.

[]{#calibre_link-1013 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Just as you can pass a string to
[input()]{.literal} to provide a prompt, you can pass a string to a
PyInputPlus function's [prompt]{.literal} keyword argument to display a
prompt:

\>\>\> [response = input(\'Enter a number: \')]{.codestrong1}\
Enter a number: [42]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
\'42\'\
\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response = pyip.inputInt(prompt=\'Enter a number:
\')]{.codestrong1}\
Enter a number: [cat]{.codestrong1}\
\'cat\' is not an integer.\
Enter a number: [42]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
42

Use Python's [help()]{.literal} function to find out more about each of
these functions. For example, [help(pyip.inputChoice)]{.literal}
displays help information for the [inputChoice()]{.literal} function.
Complete documentation can be found at
*[https://pyinputplus.readthedocs.io/](https://pyinputplus.readthedocs.io/){.calibre6}*.

Unlike Python's built-in [input()]{.literal}, PyInputPlus functions have
several additional features for input validation, as shown in the next
section.

#### ***The min, max, greaterThan, and lessThan Keyword Arguments*** {#calibre_link-278 .h2}

The [inputNum()]{.literal}, [inputInt()]{.literal}, and
[inputFloat()]{.literal} functions, which accept int and float numbers,
also have [min]{.literal}, [max]{.literal}, [greaterThan]{.literal}, and
[lessThan]{.literal} keyword arguments for specifying a range of valid
values. For example, enter the following into the interactive shell:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response = pyip.inputNum(\'Enter num: \', min=4)]{.codestrong1}\
Enter num:[3]{.codestrong1}\
Input must be at minimum 4.\
Enter num:[4]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
4\
\>\>\> [response = pyip.inputNum(\'Enter num: \',
greaterThan=4)]{.codestrong1}\
Enter num: [4]{.codestrong1}\
Input must be greater than 4.\
Enter num: [5]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
5\
\>\>\> [response = pyip.inputNum(\'\>\', min=4,
lessThan=6)]{.codestrong1}\
Enter num: [6]{.codestrong1}\
Input must be less than 6.\
Enter num: [3]{.codestrong1}\
Input must be at minimum 4.\
Enter num: [4]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
4

[]{#calibre_link-1775 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}These keyword arguments are optional, but if
supplied, the input cannot be less than the [min]{.literal} argument or
greater than the [max]{.literal} argument (though the input can be equal
to them). Also, the input must be greater than the
[greaterThan]{.literal} and less than the [lessThan]{.literal} arguments
(that is, the input cannot be equal to them).

#### ***The blank Keyword Argument*** {#calibre_link-279 .h2}

By default, blank input isn't allowed unless the [blank]{.literal}
keyword argument is set to [True]{.literal}:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response = pyip.inputNum(\'Enter num: \')]{.codestrong1}\
Enter num:[(blank input entered here)]{.codeitalic1}\
Blank values are not allowed.\
Enter num: [42]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
42\
\>\>\> [response = pyip.inputNum(blank=True)]{.codestrong1}\
[(blank input entered here)]{.codeitalic1}\
\>\>\> [response]{.codestrong1}\
\'\'

Use [blank=True]{.literal} if you'd like to make input optional so that
the user doesn't need to enter anything.

#### ***The limit, timeout, and default Keyword Arguments*** {#calibre_link-280 .h2}

By default, the PyInputPlus functions will continue to ask the user for
valid input forever (or for as long as the program runs). If you'd like
a function to stop asking the user for input after a certain number of
tries or a certain amount of time, you can use the [limit]{.literal} and
[timeout]{.literal} keyword arguments. Pass an integer for the
[limit]{.literal} keyword argument to determine how many attempts a
PyInputPlus function will make to receive valid input before giving up,
and pass an integer for the [timeout]{.literal} keyword argument to
determine how many seconds the user has to enter valid input before the
PyInputPlus function gives up.

If the user fails to enter valid input, these keyword arguments will
cause the function to raise a [RetryLimitException]{.literal} or
[TimeoutException]{.literal}, respectively. For example, enter the
following into the interactive shell:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response = pyip.inputNum(limit=2)]{.codestrong1}\
[blah]{.codestrong1}\
\'blah\' is not a number.\
Enter num: [number]{.codestrong1}\
\'number\' is not a number.\
Traceback (most recent call last):\
    [\--snip\--]{.codeitalic1}\
pyinputplus.RetryLimitException\
\>\>\> [response = pyip.inputNum(timeout=10)]{.codestrong1}\
[42]{.codestrong1} [(entered after 10 seconds of
waiting)]{.codeitalic1}\
[]{#calibre_link-1776 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Traceback (most recent call last):\
    [\--snip\--]{.codeitalic1}\
pyinputplus.TimeoutException

When you use these keyword arguments and also pass a [default]{.literal}
keyword argument, the function returns the default value instead of
raising an exception. Enter the following into the interactive shell:

\>\>\> [response = pyip.inputNum(limit=2,
default=\'N/A\')]{.codestrong1}\
[hello]{.codestrong1}\
\'hello\' is not a number.\
[world]{.codestrong1}\
\'world\' is not a number.\
\>\>\> [response]{.codestrong1}\
\'N/A\'

Instead of raising [RetryLimitException]{.literal}, the
[inputNum()]{.literal} function simply returns the string
[\'N/A\']{.literal}.

#### ***The allowRegexes and blockRegexes Keyword Arguments*** {#calibre_link-281 .h2}

You can also use regular expressions to specify whether an input is
allowed or not. The [allowRegexes]{.literal} and
[blockRegexes]{.literal} keyword arguments take a list of regular
expression strings to determine what the PyInputPlus function will
accept or reject as valid input. For example, enter the following code
into the interactive shell so that [inputNum()]{.literal} will accept
Roman numerals in addition to the usual numbers:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response =
pyip.inputNum(allowRegexes=\[r\'(I\|V\|X\|L\|C\|D\|M)+\',
r\'zero\'\])]{.codestrong1}\
[XLII]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
\'XLII\'\
\>\>\> [response =
pyip.inputNum(allowRegexes=\[r\'(i\|v\|x\|l\|c\|d\|m)+\',
r\'zero\'\])]{.codestrong1}\
[xlii]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
\'xlii\'

Of course, this regex affects only what letters the
[inputNum()]{.literal} function will accept from the user; the function
will still accept Roman numerals with invalid ordering such as
[\'XVX\']{.literal} or [\'MILLI\']{.literal} because the
[r\'(I\|V\|X\|L\|C\|D\|M)+\']{.literal} regular expression accepts those
strings.

You can also specify a list of regular expression strings that a
PyInputPlus function won't accept by using the [blockRegexes]{.literal}
keyword argument. Enter the following into the interactive shell so that
[inputNum()]{.literal} won't accept even numbers:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response =
pyip.inputNum(blockRegexes=\[r\'\[02468\]\$\'\])]{.codestrong1}\
[42]{.codestrong1}\
This response is invalid.\
[]{#calibre_link-1012 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[44]{.codestrong1}\
This response is invalid.\
[43]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
43

If you specify both an [allowRegexes]{.literal} and
[blockRegexes]{.literal} argument, the allow list overrides the block
list. For example, enter the following into the interactive shell, which
allows [\'caterpillar\']{.literal} and [\'category\']{.literal} but
blocks anything else that has the word [\'cat\']{.literal} in it:

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [response = pyip.inputStr(allowRegexes=\[r\'caterpillar\',
\'category\'\],]{.codestrong1}\
[blockRegexes=\[r\'cat\'\])]{.codestrong1}\
[cat]{.codestrong1}\
This response is invalid.\
[catastrophe]{.codestrong1}\
This response is invalid.\
[category]{.codestrong1}\
\>\>\> [response]{.codestrong1}\
\'category\'

The PyInputPlus module's functions can save you from writing tedious
input validation code yourself. But there's more to the PyInputPlus
module than what has been detailed here. You can examine its full
documentation online at
*[https://pyinputplus.readthedocs.io/](https://pyinputplus.readthedocs.io/){.calibre6}*.

#### ***Passing a Custom Validation Function to inputCustom()*** {#calibre_link-282 .h2}

You can write a function to perform your own custom validation logic by
passing the function to [inputCustom()]{.literal}. For example, say you
want the user to enter a series of digits that adds up to 10. There is
no [pyinputplus.inputAddsUpToTen()]{.literal} function, but you can
create your own function that:

-   Accepts a single string argument of what the user entered
-   Raises an exception if the string fails validation
-   Returns [None]{.literal} (or has no [return]{.literal} statement) if
    [inputCustom()]{.literal} should return the string unchanged
-   Returns a non-[None]{.literal} value if [inputCustom()]{.literal}
    should return a different string from the one the user entered
-   Is passed as the first argument to [inputCustom()]{.literal}

For example, we can create our own [addsUpToTen()]{.literal} function,
and then pass it to [inputCustom()]{.literal}. Note that the function
call looks like [inputCustom(addsUpToTen)]{.literal} and not
[inputCustom(addsUpToTen())]{.literal} because we are passing the
[addsUpToTen()]{.literal} function itself to [inputCustom()]{.literal},
not calling [addsUpToTen()]{.literal} and passing its return value.

\>\>\> [import pyinputplus as pyip]{.codestrong1}\
\>\>\> [def addsUpToTen(numbers):]{.codestrong1}\
[]{#calibre_link-1001 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\...   [numbersList = list(numbers)]{.codestrong1}\
\...   [for i, digit in enumerate(numbersList):]{.codestrong1}\
\...     [numbersList\[i\] = int(digit)]{.codestrong1}\
\...   [if sum(numbersList) != 10:]{.codestrong1}\
\...     [raise Exception(\'The digits must add up to 10, not %s.\'
%]{.codestrong1}\
[(sum(numbersList)))]{.codestrong1}\
\...   [return int(numbers)]{.codestrong1} \# Return an int form of
numbers.\
\...\
\>\>\> [response =]{.codestrong1}
[pyip.inputCustom(addsUpToTen)]{.codestrong1} \# No parentheses after\
addsUpToTen here.\
[123]{.codestrong1}\
The digits must add up to 10, not 6.\
[1235]{.codestrong1}\
The digits must add up to 10, not 11.\
[1234]{.codestrong1}\
\>\>\> [response]{.codestrong1} \# inputStr() returned an int, not a
string.\
1234\
\>\>\> [response =]{.codestrong1}
[pyip.inputCustom(addsUpToTen)]{.codestrong1}\
[hello]{.codestrong1}\
invalid literal for int() with base 10: \'h\'\
[55]{.codestrong1}\
\>\>\> [response]{.codestrong1}

The [inputCustom()]{.literal} function also supports the general
PyInputPlus features, such as the [blank]{.literal}, [limit]{.literal},
[timeout]{.literal}, [default]{.literal}, [allowRegexes]{.literal}, and
[blockRegexes]{.literal} keyword arguments. Writing your own custom
validation function is useful when it's otherwise difficult or
impossible to write a regular expression for valid input, as in the
"adds up to 10" example.

### **Project: How to Keep an Idiot Busy for Hours** {#calibre_link-283 .h1}

Let's use PyInputPlus to create a simple program that does the
following:

1.  Ask the user if they'd like to know how to keep an idiot busy for
    hours.
2.  If the user answers no, quit.
3.  If the user answers yes, go to Step 1.

Of course, we don't know if the user will enter something besides "yes"
or "no," so we need to perform input validation. It would also be
convenient for the user to be able to enter "y" or "n" instead of the
full words. PyInputPlus's [inputYesNo()]{.literal} function will handle
this for us and, no matter what case the user enters, return a lowercase
[\'yes\']{.literal} or [\'no\']{.literal} string value.

When you run this program, it should look like the following:

Want to know how to keep an idiot busy for hours?\
[sure]{.codestrong1}\
\'sure\' is not a valid yes/no response.\
Want to know how to keep an idiot busy for hours?\
[yes]{.codestrong1}\
Want to know how to keep an idiot busy for hours?\
[y]{.codestrong1}\
[]{#calibre_link-1002 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Want to know how to keep an idiot busy for hours?\
[Yes]{.codestrong1}\
Want to know how to keep an idiot busy for hours?\
[YES]{.codestrong1}\
Want to know how to keep an idiot busy for hours?\
[YES!!!!!!]{.codestrong1}\
\'YES!!!!!!\' is not a valid yes/no response.\
Want to know how to keep an idiot busy for hours?\
[TELL ME HOW TO KEEP AN IDIOT BUSY FOR HOURS.]{.codestrong1}\
\'TELL ME HOW TO KEEP AN IDIOT BUSY FOR HOURS.\' is not a valid yes/no
response.\
Want to know how to keep an idiot busy for hours?\
[no]{.codestrong1}\
Thank you. Have a nice day.

Open a new file editor tab and save it as *idiot.py*. Then enter the
following code:

[import pyinputplus as pyip]{.codestrong1}

This imports the PyInputPlus module. Since [pyinputplus]{.literal} is a
bit much to type, we'll use the name [pyip]{.literal} for short.

while True:\
    prompt = \'Want to know how to keep an idiot busy for hours?\\n\'\
    response = pyip.inputYesNo(prompt)

Next, [while True:]{.literal} creates an infinite loop that continues to
run until it encounters a [break]{.literal} statement. In this loop, we
call [pyip.inputYesNo()]{.literal} to ensure that this function call
won't return until the user enters a valid answer.

    if response == \'no\':\
        break

The [pyip.inputYesNo()]{.literal} call is guaranteed to only return
either the string [yes]{.literal} or the string [no]{.literal}. If it
returned [no]{.literal}, then our program breaks out of the infinite
loop and continues to the last line, which thanks the user:

print(\'Thank you. Have a nice day.\')

Otherwise, the loop iterates once again.

You can also make use of the [inputYesNo()]{.literal} function in
non-English languages by passing [yesVal]{.literal} and
[noVal]{.literal} keyword arguments. For example, the Spanish version of
this program would have these two lines:

    prompt = \'¿Quieres saber cómo mantener ocupado a un idiota durante
horas?\\n\'\
    response = pyip.inputYesNo(prompt, yesVal=\'sí\', noVal=\'no\')\
    if response == \'sí\':

Now the user can enter either [sí]{.literal} or [s]{.literal} (in lower-
or uppercase) instead of [yes]{.literal} or [y]{.literal} for an
affirmative answer.

### []{#calibre_link-1047 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Project: Multiplication Quiz** {#calibre_link-284 .h1}

PyInputPlus's features can be useful for creating a timed multiplication
quiz. By setting the [allowRegexes]{.literal}, [blockRegexes]{.literal},
[timeout]{.literal}, and [limit]{.literal} keyword argument to
[pyip.inputStr()]{.literal}, you can leave most of the implementation to
PyInputPlus. The less code you need to write, the faster you can write
your programs. Let's create a program that poses 10 multiplication
problems to the user, where the valid input is the problem's correct
answer. Open a new file editor tab and save the file as
*multiplicationQuiz.py*.

First, we'll import [pyinputplus]{.literal}, [random]{.literal}, and
[time]{.literal}. We'll keep track of how many questions the program
asks and how many correct answers the user gives with the variables
[numberOfQuestions]{.literal} and [correctAnswers]{.literal}. A
[for]{.literal} loop will repeatedly pose a random multiplication
problem 10 times:

import pyinputplus as pyip\
import random, time\
\
numberOfQuestions = 10\
correctAnswers = 0\
for questionNumber in range(numberOfQuestions):

Inside the [for]{.literal} loop, the program will pick two single-digit
numbers to multiply. We'll use these numbers to create a [#Q: N × N
=]{.literal} prompt for the user, where [Q]{.literal} is the question
number (1 to 10) and [N]{.literal} are the two numbers to multiply.

    # Pick two random numbers:\
    num1 = random.randint(0, 9)\
    num2 = random.randint(0, 9)\
\
    prompt = \'#%s: %s x %s = \' % (questionNumber, num1, num2)

The [pyip.inputStr()]{.literal} function will handle most of the
features of this quiz program. The argument we pass for
[allowRegexes]{.literal} is a list with the regex string
[\'\^%s\$\']{.literal}, where [%s]{.literal} is replaced with the
correct answer. The [\^]{.literal} and [%]{.literal} characters ensure
that the answer begins and ends with the correct number, though
PyInputPlus trims any whitespace from the start and end of the user's
response first just in case they inadvertently pressed the spacebar
before or after their answer. The argument we pass for
[blocklistRegexes]{.literal} is a list with [(\'.\*\',
\'Incorrect!\')]{.literal}. The first string in the tuple is a regex
that matches every possible string. Therefore, if the user response
doesn't match the correct answer, the program will reject any other
answer they provide. In that case, the [\'Incorrect!\']{.literal} string
is displayed and the user is prompted to answer again. Additionally,
passing [8]{.literal} for [timeout]{.literal} and [3]{.literal} for
[limit]{.literal} will ensure that the user only has 8 seconds and 3
tries to provide a correct answer:

    try:\
        # Right answers are handled by allowRegexes.\
        # Wrong answers are handled by blockRegexes, with a custom
message.\
        pyip.inputStr(prompt, allowRegexes=\[\'\^%s\$\' % (num1 \*
num2)\],\
[]{#calibre_link-1048 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}                              blockRegexes=\[(\'.\*\',
\'Incorrect!\')\],\
                              timeout=8, limit=3)

If the user answers after the 8-second timeout has expired, even if they
answer correctly, [pyip.inputStr()]{.literal} raises a
[TimeoutException]{.literal} exception. If the user answers incorrectly
more than 3 times, it raises a [RetryLimitException]{.literal}
exception. Both of these exception types are in the PyInputPlus module,
so [pyip.]{.literal} needs to prepend them:

    except pyip.TimeoutException:\
        print(\'Out of time!\')\
    except pyip.RetryLimitException:\
        print(\'Out of tries!\')

Remember that, just like how [else]{.literal} blocks can follow an
[if]{.literal} or [elif]{.literal} block, they can optionally follow the
last [except]{.literal} block. The code inside the following
[else]{.literal} block will run if no exception was raised in the
[try]{.literal} block. In our case, that means the code runs if the user
entered the correct answer:

    else:\
        # This block runs if no exceptions were raised in the try
block.\
        print(\'Correct!\')\
        correctAnswers += 1

No matter which of the three messages, "Out of time!", "Out of tries!",
or "Correct!", displays, let's place a 1-second pause at the end of the
[for]{.literal} loop to give the user time to read it. After the program
has asked 10 questions and the [for]{.literal} loop continues, let's
show the user how many correct answers they made:

    time.sleep(1) \# Brief pause to let user see the result.\
print(\'Score: %s / %s\' % (correctAnswers, numberOfQuestions))

PyInputPlus is flexible enough that you can use it in a wide variety of
programs that take keyboard input from the user, as demonstrated by the
programs in this chapter.

### **Summary** {#calibre_link-285 .h1}

It's easy to forget to write input validation code, but without it, your
programs will almost certainly have bugs. The values you expect users to
enter and the values they actually enter can be completely different,
and your programs need to be robust enough to handle these exceptional
cases. You can use regular expressions to create your own input
validation code, but for common cases, it's easier to use an existing
module, such as PyInputPlus. You can import the module with [import
pyinputplus as pyip]{.literal} so that you can enter a shorter name when
calling the module's functions.

PyInputPlus has functions for entering a variety of input, including
strings, numbers, dates, yes/no, [True]{.literal}/[False]{.literal},
emails, and files. While [input()]{.literal} []{#calibre_link-1777
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}always returns a
string, these functions return the value in an appropriate data type.
The [inputChoice()]{.literal} function allow you to select one of
several pre-selected options, while [inputMenu()]{.literal} also adds
numbers or letters for quick selection.

All of these functions have the following standard features: stripping
whitespace from the sides, setting timeout and retry limits with the
[timeout]{.literal} and [limit]{.literal} keyword arguments, and passing
lists of regular expression strings to [allowRegexes]{.literal} or
[blockRegexes]{.literal} to include or exclude particular responses.
You\'ll no longer need to write your own tedious while loops that check
for valid input and reprompt the user.

If none of the PyInputPlus module's, functions fit your needs, but you'd
still like the other features that PyInputPlus provides, you can call
[inputCustom()]{.literal} and pass your own custom validation function
for PyInputPlus to use. The documentation at
*[https://pyinputplus.readthedocs.io/en/latest/](https://pyinputplus.readthedocs.io/en/latest/){.calibre6}*
has a complete listing of PyInputPlus's functions and additional
features. There's far more in the PyInputPlus online documentation than
what was described in this chapter. There's no use in reinventing the
wheel, and learning to use this module will save you from having to
write and debug code for yourself.

Now that you have expertise manipulating and validating text, it's time
to learn how to read from and write to files on your computer's hard
drive.

### **Practice Questions** {#calibre_link-286 .h1}

[1](#calibre_link-1098){#calibre_link-1345 .calibre6}. Does PyInputPlus
come with the Python Standard Library?

[2](#calibre_link-1099){#calibre_link-1346 .calibre6}. Why is
PyInputPlus commonly imported with [import pyinputplus as
pyip]{.literal}?

[3](#calibre_link-1100){#calibre_link-1347 .calibre6}. What is the
difference between [inputInt()]{.literal} and [inputFloat()]{.literal}?

[4](#calibre_link-1101){#calibre_link-1348 .calibre6}. How can you
ensure that the user enters a whole number between [0]{.literal} and
[99]{.literal} using PyInputPlus?

[5](#calibre_link-1102){#calibre_link-1349 .calibre6}. What is passed to
the [allowRegexes]{.literal} and [blockRegexes]{.literal} keyword
arguments?

[6](#calibre_link-1103){#calibre_link-1350 .calibre6}. What does
[inputStr(limit=3)]{.literal} do if blank input is entered three times?

[7](#calibre_link-1104){#calibre_link-1351 .calibre6}. What does
[inputStr(limit=3, default=\'hello\')]{.literal} do if blank input is
entered three times?

### **Practice Projects** {#calibre_link-287 .h1}

For practice, write programs to do the following tasks.

#### ***Sandwich Maker*** {#calibre_link-288 .h2}

Write a program that asks users for their sandwich preferences. The
program should use PyInputPlus to ensure that they enter valid input,
such as:

-   Using [inputMenu()]{.literal} for a bread type: wheat, white, or
    sourdough.
-   Using [inputMenu()]{.literal} for a protein type: chicken, turkey,
    ham, or tofu.
-   []{#calibre_link-1778 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Using [inputYesNo()]{.literal} to ask if they
    want cheese.
-   If so, using [inputMenu()]{.literal} to ask for a cheese type:
    cheddar, Swiss, or mozzarella.
-   Using [inputYesNo()]{.literal} to ask if they want mayo, mustard,
    lettuce, or tomato.
-   Using [inputInt()]{.literal} to ask how many sandwiches they want.
    Make sure this number is 1 or more.

Come up with prices for each of these options, and have your program
display a total cost after the user enters their selection.

#### ***Write Your Own Multiplication Quiz*** {#calibre_link-289 .h2}

To see how much PyInputPlus is doing for you, try re-creating the
multiplication quiz project on your own without importing it. This
program will prompt the user with 10 multiplication questions, ranging
from 0 × 0 to 9 × 9. You'll need to implement the following features:

-   If the user enters the correct answer, the program displays
    "Correct!" for 1 second and moves on to the next question.
-   The user gets three tries to enter the correct answer before the
    program moves on to the next question.
-   Eight seconds after first displaying the question, the question is
    marked as incorrect even if the user enters the correct answer after
    the 8-second limit.

Compare your code to the code using PyInputPlus in "[Project:
Multiplication Quiz](#calibre_link-284){.calibre6}" on [page
196](#calibre_link-1047){.calibre6}.[]{#calibre_link-1779 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}

