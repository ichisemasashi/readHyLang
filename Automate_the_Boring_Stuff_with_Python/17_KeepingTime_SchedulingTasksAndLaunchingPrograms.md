
## **KEEPING TIME, SCHEDULING TASKS, AND LAUNCHING PROGRAMS**


![Image](../images/000055.jpg){.calibre3}


Running programs while you're sitting at your computer is fine, but it's
also useful to have programs run without your direct supervision. Your
computer's clock can schedule programs to run code at some specified
time and date or at regular intervals. For example, your program could
scrape a website every hour to check for changes or do a CPU-intensive
task at 4 [AM]{.small} while you sleep. Python's [time]{.literal} and
[datetime]{.literal} modules provide these functions.

You can also write programs that launch other programs on a schedule by
using the [subprocess]{.literal} and [threading]{.literal} modules.
Often, the fastest way to program is to take advantage of applications
that other people have already written.

### []{#calibre_link-867 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**The time Module** {#calibre_link-531 .h1}

Your computer's system clock is set to a specific date, time, and time
zone. The built-in [time]{.literal} module allows your Python programs
to read the system clock for the current time. The
[time.time()]{.literal} and [time.sleep()]{.literal} functions are the
most useful in the [time]{.literal} module.

#### ***The time.time() Function*** {#calibre_link-532 .h2}

The *Unix epoch* is a time reference commonly used in programming: 12
[AM]{.small} on January 1, 1970, Coordinated Universal Time (UTC). The
[time.time()]{.literal} function returns the number of seconds since
that moment as a float value. (Recall that a float is just a number with
a decimal point.) This number is called an *epoch timestamp*. For
example, enter the following into the interactive shell:

\>\>\> [import time]{.codestrong1}\
\>\>\> [time.time()]{.codestrong1}\
1543813875.3518236

Here I'm calling [time.time()]{.literal} on December 2, 2018, at 9:11
[PM]{.small} Pacific Standard Time. The return value is how many seconds
have passed between the Unix epoch and the moment
[time.time()]{.literal} was called.

Epoch timestamps can be used to *profile* code, that is, measure how
long a piece of code takes to run. If you call [time.time()]{.literal}
at the beginning of the code block you want to measure and again at the
end, you can subtract the first timestamp from the second to find the
elapsed time between those two calls. For example, open a new file
editor tab and enter the following program:

    import time\
[➊]{.ent} def calcProd():\
       # Calculate the product of the first 100,000 numbers.\
       product = 1\
       for i in range(1, 100000):\
           product = product \* i\
       return product\
\
[➋]{.ent} startTime = time.time()\
   prod = calcProd()\
[➌]{.ent} endTime = time.time()\
[➍]{.ent} print(\'The result is %s digits long.\' % (len(str(prod))))\
[➎]{.ent} print(\'Took %s seconds to calculate.\' % (endTime -
startTime))

At [➊]{.ent}, we define a function [calcProd()]{.literal} to loop
through the integers from 1 to 99,999 and return their product. At
[➋]{.ent}, we call [time.time()]{.literal} and store it in
[startTime]{.literal}. Right after calling [calcProd()]{.literal}, we
call [time.time()]{.literal} again and store it in [endTime]{.literal}
[➌]{.ent}. We end by printing the length of the product returned by
[calcProd()]{.literal} [➍]{.ent} and how long it took to run
[calcProd()]{.literal} [➎]{.ent}.

[]{#calibre_link-874 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Save this program as *calcProd.py* and run it. The
output will look something like this:

The result is 456569 digits long.\
Took 2.844162940979004 seconds to calculate.


**[NOTE]{.notes}**

*Another way to profile your code is to use the
[cProfile.run()]{.codeitalic} function, which provides a much more
informative level of detail than the simple [time.time()]{.codeitalic}
technique. The [cProfile.run()]{.codeitalic} function is explained at*
[https://docs.python.org/3/library/profile.html](https://docs.python.org/3/library/profile.html){.calibre6}.


The return value from [time.time()]{.literal} is useful, but not
human-readable. The [time.ctime()]{.literal} function returns a string
description of the current time. You can also optionally pass the number
of seconds since the Unix epoch, as returned by [time.time()]{.literal},
to get a string value of that time. Enter the following into the
interactive shell:

\>\>\> import time\
\>\>\> time.ctime()\
\'Mon Jun 15 14:00:38 2020\'\
\>\>\> thisMoment = time.time()\
\>\>\> time.ctime(thisMoment)\
\'Mon Jun 15 14:00:45 2020\'

#### ***The time.sleep() Function*** {#calibre_link-533 .h2}

If you need to pause your program for a while, call the
[time.sleep()]{.literal} function and pass it the number of seconds you
want your program to stay paused. Enter the following into the
interactive shell:

   \>\>\> [import time]{.codestrong1}\
   \>\>\> [for i in range(3):]{.codestrong1}\
        [➊]{.ent} [print(\'Tick\')]{.codestrong1}\
        [➋]{.ent} [time.sleep(1)]{.codestrong1}\
        [➌]{.ent} [print(\'Tock\')]{.codestrong1}\
        [➍]{.ent} [time.sleep(1)]{.codestrong1}\
\
   Tick\
   Tock\
   Tick\
   Tock\
   Tick\
   Tock\
[➎]{.ent} \>\>\> [time.sleep(5)]{.codestrong1}

The [for]{.literal} loop will print [Tick]{.literal} [➊]{.ent}, pause
for 1 second [➋]{.ent}, print [Tock]{.literal} [➌]{.ent}, pause for 1
second [➍]{.ent}, print [Tick]{.literal}, pause, and so on until
[Tick]{.literal} and [Tock]{.literal} have each been printed three
times.

[]{#calibre_link-818 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [time.sleep()]{.literal} function will
*block*---that is, it will not return and release your program to
execute other code---until after the number of seconds you passed to
[time.sleep()]{.literal} has elapsed. For example, if you enter
[time.sleep(5)]{.literal} [➎]{.ent}, you'll see that the next prompt
([\>\>\>]{.literal}) doesn't appear until 5 seconds have passed.

### **Rounding Numbers** {#calibre_link-534 .h1}

When working with times, you'll often encounter float values with many
digits after the decimal. To make these values easier to work with, you
can shorten them with Python's built-in [round()]{.literal} function,
which rounds a float to the precision you specify. Just pass in the
number you want to round, plus an optional second argument representing
how many digits after the decimal point you want to round it to. If you
omit the second argument, [round()]{.literal} rounds your number to the
nearest whole integer. Enter the following into the interactive shell:

\>\>\> [import time]{.codestrong1}\
\>\>\> [now = time.time()]{.codestrong1}\
\>\>\> [now]{.codestrong1}\
1543814036.6147408\
\>\>\> [round(now, 2)]{.codestrong1}\
1543814036.61\
\>\>\> [round(now, 4)]{.codestrong1}\
1543814036.6147\
\>\>\> [round(now)]{.codestrong1}\
1543814037

After importing [time]{.literal} and storing [time.time()]{.literal} in
[now]{.literal}, we call [round(now, 2)]{.literal} to round
[now]{.literal} to two digits after the decimal, [round(now,
4)]{.literal} to round to four digits after the decimal, and
[round(now)]{.literal} to round to the nearest integer.

### **Project: Super Stopwatch** {#calibre_link-535 .h1}

Say you want to track how much time you spend on boring tasks you
haven't automated yet. You don't have a physical stopwatch, and it's
surprisingly difficult to find a free stopwatch app for your laptop or
smartphone that isn't covered in ads and doesn't send a copy of your
browser history to marketers. (It says it can do this in the license
agreement you agreed to. You did read the license agreement, didn't
you?) You can write a simple stopwatch program yourself in Python.

At a high level, here's what your program will do:

1.  Track the amount of time elapsed between presses of the
    [ENTER]{.small} key, with each key press starting a new "lap" on the
    timer.
2.  Print the lap number, total time, and lap time.

[]{#calibre_link-1845 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}This means your code will need to do the following:

1.  Find the current time by calling [time.time()]{.literal} and store
    it as a timestamp at the start of the program, as well as at the
    start of each lap.
2.  Keep a lap counter and increment it every time the user presses
    [ENTER]{.small}.
3.  Calculate the elapsed time by subtracting timestamps.
4.  Handle the [KeyboardInterrupt]{.literal} exception so the user can
    press [CTRL]{.small}-C to quit.

Open a new file editor tab and save it as *stopwatch.py*.

#### ***Step 1: Set Up the Program to Track Times*** {#calibre_link-536 .h2}

The stopwatch program will need to use the current time, so you'll want
to import the [time]{.literal} module. Your program should also print
some brief instructions to the user before calling [input()]{.literal},
so the timer can begin after the user presses [ENTER]{.small}. Then the
code will start tracking lap times.

Enter the following code into the file editor, writing a
[TODO]{.literal} comment as a placeholder for the rest of the code:

#! python3\
\# stopwatch.py - A simple stopwatch program.\
\
import time\
\
\# Display the program\'s instructions.\
print(\'Press ENTER to begin. Afterward, press ENTER to \"click\" the
stopwatch.\
Press Ctrl-C to quit.\')\
input()                    # press Enter to begin\
print(\'Started.\')\
startTime = time.time()    # get the first lap\'s start time\
lastTime = startTime\
lapNum = 1\
\
\# TODO: Start tracking the lap times.

Now that we've written the code to display the instructions, start the
first lap, note the time, and set our lap count to 1.

#### ***Step 2: Track and Print Lap Times*** {#calibre_link-537 .h2}

Now let's write the code to start each new lap, calculate how long the
previous lap took, and calculate the total time elapsed since starting
the stopwatch. We'll display the lap time and total time and increase
the lap count for each new lap. Add the following code to your program:

   #! python3\
   # stopwatch.py - A simple stopwatch program.\
\
   import time\
\
   \--[snip]{.codeitalic1}\--\
   []{#calibre_link-1068 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[\# Start tracking the lap times.]{.codestrong1}\
[➊]{.ent} [try:]{.codestrong1}\
    [➋]{.ent} [while True:]{.codestrong1}\
        [input()]{.codestrong1}\
        [➌]{.ent} [lapTime = round(time.time() - lastTime,
2)]{.codestrong1}\
        [➍]{.ent} [totalTime = round(time.time() - startTime,
2)]{.codestrong1}\
  [      ]{.codestrong1}[➎]{.ent} [print(\'Lap #%s: %s (%s)\' % (lapNum,
totalTime, lapTime), end=\'\')]{.codestrong1}\
[           lapNum += 1]{.codestrong1}\
[           lastTime = time.time() \# reset the last lap
time]{.codestrong1}\
[➏]{.ent} [except KeyboardInterrupt:]{.codestrong1}\
   [    # Handle the Ctrl-C exception to keep its error message from
displaying.]{.codestrong1}\
   [    print(\'\\nDone.\')]{.codestrong1}

If the user presses [CTRL]{.small}-C to stop the stopwatch, the
[KeyboardInterrupt]{.literal} exception will be raised, and the program
will crash if its execution is not a [try]{.literal} statement. To
prevent crashing, we wrap this part of the program in a [try]{.literal}
statement [➊]{.ent}. We'll handle the exception in the
[except]{.literal} clause [➏]{.ent}, so when [CTRL]{.small}-C is pressed
and the exception is raised, the program execution moves to the except
clause to print [Done]{.literal}, instead of the
[KeyboardInterrupt]{.literal} error message. Until this happens, the
execution is inside an infinite loop [➋]{.ent} that calls
[input()]{.literal} and waits until the user presses [ENTER]{.small} to
end a lap. When a lap ends, we calculate how long the lap took by
subtracting the start time of the lap, [lastTime]{.literal}, from the
current time, [time.time()]{.literal} [➌]{.ent}. We calculate the total
time elapsed by subtracting the overall start time of the stopwatch,
[startTime]{.literal}, from the current time [➍]{.ent}.

Since the results of these time calculations will have many digits after
the decimal point (such as [4.766272783279419]{.literal}), we use the
[round()]{.literal} function to round the float value to two digits at
[➌]{.ent} and [➍]{.ent}.

At [➎]{.ent}, we print the lap number, total time elapsed, and the lap
time. Since the user pressing [ENTER]{.small} for the
[input()]{.literal} call will print a newline to the screen, pass
[end=\'\']{.literal} to the [print()]{.literal} function to avoid
double-spacing the output. After printing the lap information, we get
ready for the next lap by adding 1 to the count [lapNum]{.literal} and
setting [lastTime]{.literal} to the current time, which is the start
time of the next lap.

#### ***Ideas for Similar Programs*** {#calibre_link-538 .h2}

Time tracking opens up several possibilities for your programs. Although
you can download apps to do some of these things, the benefit of writing
programs yourself is that they will be free and not bloated with ads and
useless features. You could write similar programs to do the following:

-   Create a simple timesheet app that records when you type a person's
    name and uses the current time to clock them in or out.
-   Add a feature to your program to display the elapsed time since a
    process started, such as a download that uses the
    [requests]{.literal} module. (See [Chapter
    12](#calibre_link-372){.calibre6}.)
-   Intermittently check how long a program has been running and offer
    the user a chance to cancel tasks that are taking too long.

### []{#calibre_link-889 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**The datetime Module** {#calibre_link-539 .h1}

The [time]{.literal} module is useful for getting a Unix epoch timestamp
to work with. But if you want to display a date in a more convenient
format, or do arithmetic with dates (for example, figuring out what date
was 205 days ago or what date is 123 days from now), you should use the
[datetime]{.literal} module.

The [datetime]{.literal} module has its own [datetime]{.literal} data
type. [datetime]{.literal} values represent a specific moment in time.
Enter the following into the interactive shell:

   \>\>\> [import datetime]{.codestrong1}\
[➊]{.ent} \>\>\> [datetime.datetime.now()]{.codestrong1}\
[➋]{.ent} datetime.datetime(2019, 2, 27, 11, 10, 49, 55, 53)\
[➌]{.ent} \>\>\> [dt = datetime.datetime(2019, 10, 21, 16, 29,
0)]{.codestrong1}\
[➍]{.ent} \>\>\> [dt.year, dt.month, dt.day]{.codestrong1}\
   (2019, 10, 21)\
[➎]{.ent} \>\>\> [dt.hour, dt.minute, dt.second]{.codestrong1}\
   (16, 29, 0)

Calling [datetime.datetime.now()]{.literal} [➊]{.ent} returns a
[datetime]{.literal} object [➋]{.ent} for the current date and time,
according to your computer's clock. This object includes the year,
month, day, hour, minute, second, and microsecond of the current moment.
You can also retrieve a [datetime]{.literal} object for a specific
moment by using the [datetime.datetime()]{.literal} function [➌]{.ent},
passing it integers representing the year, month, day, hour, and second
of the moment you want. These integers will be stored in the
[datetime]{.literal} object's [year]{.literal}, [month]{.literal},
[day]{.literal} [➍]{.ent}, [hour]{.literal}, [minute]{.literal}, and
[second]{.literal} [➎]{.ent} attributes.

A Unix epoch timestamp can be converted to a [datetime]{.literal} object
with the [datetime.datetime.fromtimestamp()]{.literal} function. The
date and time of the [datetime]{.literal} object will be converted for
the local time zone. Enter the following into the interactive shell:

\>\>\> [import datetime, time]{.codestrong1}\
\>\>\> [datetime.datetime.fromtimestamp(1000000)]{.codestrong1}\
datetime.datetime(1970, 1, 12, 5, 46, 40)\
\>\>\> [datetime.datetime.fromtimestamp(time.time())]{.codestrong1}\
datetime.datetime(2019, 10, 21, 16, 30, 0, 604980)

Calling [datetime.datetime.fromtimestamp()]{.literal} and passing it
[1000000]{.literal} returns a [datetime]{.literal} object for the moment
1,000,000 seconds after the Unix epoch. Passing [time.time()]{.literal},
the Unix epoch timestamp for the current moment, returns a
[datetime]{.literal} object for the current moment. So the expressions
[datetime.datetime.now()]{.literal} and
[datetime.datetime.fromtimestamp(time.time())]{.literal} do the same
thing; they both give you a [datetime]{.literal} object for the present
moment.

You can compare [datetime]{.literal} objects with each other using
comparison operators to find out which one precedes the other. The later
[datetime]{.literal} object is the "greater" value. Enter the following
into the interactive shell:

[➊]{.ent} \>\>\> [halloween2019 = datetime.datetime(2019, 10, 31, 0, 0,
0)]{.codestrong1}\
[➋]{.ent} \>\>\> [newyears2020 = datetime.datetime(2020, 1, 1, 0, 0,
0)]{.codestrong1}\
   \>\>\> [oct31_2019 = datetime.datetime(2019, 10, 31, 0, 0,
0)]{.codestrong1}\
[]{#calibre_link-894 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}[➌]{.ent} \>\>\> [halloween2019 ==
oct31_2019]{.codestrong1}\
   True\
[➍]{.ent} \>\>\> [halloween2019 \> newyears2020]{.codestrong1}\
   False\
[➎]{.ent} \>\>\> [newyears2020 \> halloween2019]{.codestrong1}\
   True\
   \>\>\> [newyears2020 != oct31_2019]{.codestrong1}\
   True

Make a [datetime]{.literal} object for the first moment (midnight) of
October 31, 2019, and store it in [halloween2019]{.literal} [➊]{.ent}.
Make a [datetime]{.literal} object for the first moment of January 1,
2020, and store it in [newyears2020]{.literal} [➋]{.ent}. Then make
another object for midnight on October 31, 2019, and store it in
[oct31_2019]{.literal}. Comparing [halloween2019]{.literal} and
[oct31_2019]{.literal} shows that they're equal [➌]{.ent}. Comparing
[newyears2020]{.literal} and [halloween2019]{.literal} shows that
[newyears2020]{.literal} is greater (later) than
[halloween2019]{.literal} [➍]{.ent} [➎]{.ent}.

#### ***The timedelta Data Type*** {#calibre_link-540 .h2}

The [datetime]{.literal} module also provides a [timedelta]{.literal}
data type, which represents a *duration* of time rather than a *moment*
in time. Enter the following into the interactive shell:

[➊]{.ent} \>\>\> [delta = datetime.timedelta(days=11, hours=10,
minutes=9, seconds=8)]{.codestrong1}\
[➋]{.ent} \>\>\> [delta.days, delta.seconds,
delta.microseconds]{.codestrong1}\
   (11, 36548, 0)\
   \>\>\> [delta.total_seconds()]{.codestrong1}\
   986948.0\
   \>\>\> str(delta)\
   \'11 days, 10:09:08\'

To create a [timedelta]{.literal} object, use the
[datetime.timedelta()]{.literal} function. The
[datetime.timedelta()]{.literal} function takes keyword arguments
[weeks]{.literal}, [days]{.literal}, [hours]{.literal},
[minutes]{.literal}, [seconds]{.literal}, [milliseconds]{.literal}, and
[microseconds]{.literal}. There is no [month]{.literal} or
[year]{.literal} keyword argument, because "a month" or "a year" is a
variable amount of time depending on the particular month or year. A
[timedelta]{.literal} object has the total duration represented in days,
seconds, and microseconds. These numbers are stored in the
[days]{.literal}, [seconds]{.literal}, and [microseconds]{.literal}
attributes, respectively. The [total_seconds()]{.literal} method will
return the duration in number of seconds alone. Passing a
[timedelta]{.literal} object to [str()]{.literal} will return a nicely
formatted, human-readable string representation of the object.

In this example, we pass keyword arguments to
[datetime.delta()]{.literal} to specify a duration of 11 days, 10 hours,
9 minutes, and 8 seconds, and store the returned [timedelta]{.literal}
object in [delta]{.literal} [➊]{.ent}. This [timedelta]{.literal}
object's [days]{.literal} attributes stores [11]{.literal}, and its
[seconds]{.literal} attribute stores [36548]{.literal} (10 hours, 9
minutes, and 8 seconds, expressed in seconds) [➋]{.ent}. Calling
[total_seconds()]{.literal} tells us that 11 days, 10 hours, 9 minutes,
and 8 seconds is 986,948 seconds. Finally, passing the
[timedelta]{.literal} object to [str()]{.literal} returns a string that
plainly describes the duration.

[]{#calibre_link-888 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The arithmetic operators can be used to perform
*date arithmetic* on [datetime]{.literal} values. For example, to
calculate the date 1,000 days from now, enter the following into the
interactive shell:

\>\>\> [dt = datetime.datetime.now()]{.codestrong1}\
\>\>\> [dt]{.codestrong1}\
datetime.datetime(2018, 12, 2, 18, 38, 50, 636181)\
\>\>\> [thousandDays = datetime.timedelta(days=1000)]{.codestrong1}\
\>\>\> [dt + thousandDays]{.codestrong1}\
datetime.datetime(2021, 8, 28, 18, 38, 50, 636181)

First, make a [datetime]{.literal} object for the current moment and
store it in [dt]{.literal}. Then make a [timedelta]{.literal} object for
a duration of 1,000 days and store it in [thousandDays]{.literal}. Add
[dt]{.literal} and [thousandDays]{.literal} together to get a
[datetime]{.literal} object for the date 1,000 days from now. Python
will do the date arithmetic to figure out that 1,000 days after December
2, 2018, will be August 18, 2021. This is useful because when you
calculate 1,000 days from a given date, you have to remember how many
days are in each month and factor in leap years and other tricky
details. The [datetime]{.literal} module handles all of this for you.

[timedelta]{.literal} objects can be added or subtracted with
[datetime]{.literal} objects or other [timedelta]{.literal} objects
using the [+]{.literal} and [-]{.literal} operators. A
[timedelta]{.literal} object can be multiplied or divided by integer or
float values with the [\*]{.literal} and [/]{.literal} operators. Enter
the following into the interactive shell:

[➊]{.ent} \>\>\> [oct21st = datetime.datetime(2019, 10, 21, 16, 29,
0)]{.codestrong1}\
[➋]{.ent} \>\>\> [aboutThirtyYears = datetime.timedelta(days=365 \*
30)]{.codestrong1}\
   \>\>\> [oct21st]{.codestrong1}\
   datetime.datetime(2019, 10, 21, 16, 29)\
   \>\>\> [oct21st - aboutThirtyYears]{.codestrong1}\
   datetime.datetime(1989, 10, 28, 16, 29)\
   \>\>\> [oct21st - (2 \* aboutThirtyYears)]{.codestrong1}\
   datetime.datetime(1959, 11, 5, 16, 29)

Here we make a [datetime]{.literal} object for October 21, 2019,
[➊]{.ent} and a [timedelta]{.literal} object for a duration of about 30
years (we're assuming 365 days for each of those years) [➋]{.ent}.
Subtracting [aboutThirtyYears]{.literal} from [oct21st]{.literal} gives
us a [datetime]{.literal} object for the date 30 years before October
21, 2019. Subtracting [2 \* aboutThirtyYears]{.literal} from
[oct21st]{.literal} returns a [datetime]{.literal} object for the date
60 years before October 21, 2019.

#### ***Pausing Until a Specific Date*** {#calibre_link-541 .h2}

The [time.sleep()]{.literal} method lets you pause a program for a
certain number of seconds. By using a [while]{.literal} loop, you can
pause your programs until a specific date. For example, the following
code will continue to loop until Halloween 2016:

import datetime\
import time\
halloween2016 = datetime.datetime(2016, 10, 31, 0, 0, 0)\
while datetime.datetime.now() \< halloween2016:\
    time.sleep(1)

[]{#calibre_link-892 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [time.sleep(1)]{.literal} call will pause your
Python program so that the computer doesn't waste CPU processing cycles
simply checking the time over and over. Rather, the [while]{.literal}
loop will just check the condition once per second and continue with the
rest of the program after Halloween 2016 (or whenever you program it to
stop).

#### ***Converting datetime Objects into Strings*** {#calibre_link-542 .h2}

Epoch timestamps and [datetime]{.literal} objects aren't very friendly
to the human eye. Use the [strftime()]{.literal} method to display a
[datetime]{.literal} object as a string. (The *f* in the name of the
[strftime()]{.literal} function stands for *format*.)

The [strftime()]{.literal} method uses directives similar to Python's
string formatting. [Table 17-1](#calibre_link-1522){.calibre6} has a
full list of [strftime()]{.literal} directives.

**Table 17-1:** [strftime()]{.literal1} Directives

  [strftime()]{.codestrong} **directive**   **Meaning**
  ----------------------------------------- -------------------------------------------------------------------------------
  [%Y]{.literal}                            Year with century, as in [\'2014\']{.literal}
  [%y]{.literal}                            Year without century, [\'00\']{.literal} to [\'99\']{.literal} (1970 to 2069)
  [%m]{.literal}                            Month as a decimal number, [\'01\']{.literal} to [\'12\']{.literal}
  [%B]{.literal}                            Full month name, as in [\'November\']{.literal}
  [%b]{.literal}                            Abbreviated month name, as in [\'Nov\']{.literal}
  [%d]{.literal}                            Day of the month, [\'01\']{.literal} to [\'31\']{.literal}
  [%j]{.literal}                            Day of the year, [\'001\']{.literal} to [\'366\']{.literal}
  [%w]{.literal}                            Day of the week, [\'0\']{.literal} (Sunday) to [\'6\']{.literal} (Saturday)
  [%A]{.literal}                            Full weekday name, as in [\'Monday\']{.literal}
  [%a]{.literal}                            Abbreviated weekday name, as in [\'Mon\']{.literal}
  [%H]{.literal}                            Hour (24-hour clock), [\'00\']{.literal} to [\'23\']{.literal}
  [%I]{.literal}                            Hour (12-hour clock), [\'01\']{.literal} to [\'12\']{.literal}
  [%M]{.literal}                            Minute, [\'00\']{.literal} to [\'59\']{.literal}
  [%S]{.literal}                            Second, [\'00\']{.literal} to [\'59\']{.literal}
  [%p]{.literal}                            [\'AM\']{.literal} or [\'PM\']{.literal}
  [%%]{.literal}                            Literal [\'%\']{.literal} character

Pass [strftime()]{.literal} a custom format string containing formatting
directives (along with any desired slashes, colons, and so on), and
[strftime()]{.literal} will return the [datetime]{.literal} object's
information as a formatted string. Enter the following into the
interactive shell:

\>\>\> [oct21st = datetime.datetime(2019, 10, 21, 16, 29,
0)]{.codestrong1}\
\>\>\> [oct21st.strftime(\'%Y/%m/%d %H:%M:%S\')]{.codestrong1}\
\'2019/10/21 16:29:00\'\
\>\>\> [oct21st.strftime(\'%I:%M %p\')]{.codestrong1}\
\'04:29 PM\'\
\>\>\> [oct21st.strftime(\"%B of \'%y\")]{.codestrong1}\
\"October of \'19\"

[]{#calibre_link-893 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here we have a [datetime]{.literal} object for
October 21, 2019, at 4:29 [PM]{.small}, stored in [oct21st]{.literal}.
Passing [strftime()]{.literal} the custom format string [\'%Y/%m/%d
%H:%M:%S\']{.literal} returns a string containing 2019, 10, and 21
separated by slashes and 16, 29, and 00 separated by colons. Passing
[\'%I:%M% p\']{.literal} returns [\'04:29 PM\']{.literal}, and passing
[\"%B of \'%y\"]{.literal} returns [\"October of \'19\"]{.literal}. Note
that [strftime()]{.literal} doesn't begin with
[datetime.datetime]{.literal}.

#### ***Converting Strings into datetime Objects*** {#calibre_link-543 .h2}

If you have a string of date information, such as [\'2019/10/21
16:29:00\']{.literal} or [\'October 21, 2019\']{.literal}, and need to
convert it to a [datetime]{.literal} object, use the
[datetime.datetime.strptime()]{.literal} function. The
[strptime()]{.literal} function is the inverse of the
[strftime()]{.literal} method. A custom format string using the same
directives as [strftime()]{.literal} must be passed so that
[strptime()]{.literal} knows how to parse and understand the string.
(The *p* in the name of the [strptime()]{.literal} function stands for
*parse*.)

Enter the following into the interactive shell:

[➊]{.ent} \>\>\> [datetime.datetime.strptime(\'October 21, 2019\', \'%B
%d, %Y\')]{.codestrong1}\
   datetime.datetime(2019, 10, 21, 0, 0)\
   \>\>\> [datetime.datetime.strptime(\'2019/10/21 16:29:00\',
\'%Y/%m/%d %H:%M:%S\')]{.codestrong1}\
   datetime.datetime(2019, 10, 21, 16, 29)\
   \>\>\> [datetime.datetime.strptime(\"October of \'19\", \"%B of
\'%y\")]{.codestrong1}\
   datetime.datetime(2019, 10, 1, 0, 0)\
   \>\>\> [datetime.datetime.strptime(\"November of \'63\", \"%B of
\'%y\")]{.codestrong1}\
   datetime.datetime(2063, 11, 1, 0, 0)

To get a [datetime]{.literal} object from the string [\'October 21,
2019\']{.literal}, pass that string as the first argument to
[strptime()]{.literal} and the custom format string that corresponds to
[\'October 21, 2019\']{.literal} as the second argument [➊]{.ent}. The
string with the date information must match the custom format string
exactly, or Python will raise a [ValueError]{.literal} exception.

### **Review of Python's Time Functions** {#calibre_link-544 .h1}

Dates and times in Python can involve quite a few different data types
and functions. Here's a review of the three different types of values
used to represent time:

-   A Unix epoch timestamp (used by the [time]{.literal} module) is a
    float or integer value of the number of seconds since 12
    [AM]{.small} on January 1, 1970, UTC.
-   A [datetime]{.literal} object (of the [datetime]{.literal} module)
    has integers stored in the attributes [year]{.literal},
    [month]{.literal}, [day]{.literal}, [hour]{.literal},
    [minute]{.literal}, and [second]{.literal}.
-   A [timedelta]{.literal} object (of the [datetime]{.literal} module)
    represents a time duration, rather than a specific moment.

[]{#calibre_link-890 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here's a review of time functions and their
parameters and return values:

[time.time()]{.codestrong} This function returns an epoch timestamp
float value of the current moment.

[time.sleep(]{.codestrong}[seconds]{.codestrongitalic}[)]{.codestrong}
This function stops the program for the number of seconds specified by
the [seconds]{.codeitalic} argument.

[datetime.datetime(]{.codestrong}[year]{.codestrongitalic}[,]{.codestrong}
[month]{.codestrongitalic}[,]{.codestrong}
[day]{.codestrongitalic}[,]{.codestrong}
[hour]{.codestrongitalic}[,]{.codestrong}
[minute]{.codestrongitalic}[,]{.codestrong}
[second]{.codestrongitalic}[)]{.codestrong} This function returns a
[datetime]{.literal} object of the moment specified by the arguments. If
[hour]{.codeitalic}, [minute]{.codeitalic}, or [second]{.codeitalic}
arguments are not provided, they default to [0]{.literal}.

[datetime.datetime.now()]{.codestrong} This function returns a
[datetime]{.literal} object of the current moment.

[datetime.datetime.fromtimestamp(]{.codestrong}[epoch]{.codestrongitalic}[)]{.codestrong}
This function returns a [datetime]{.literal} object of the moment
represented by the [epoch]{.codeitalic} timestamp argument.

[datetime.timedelta(]{.codestrong}[weeks]{.codestrongitalic}[,]{.codestrong}
[days]{.codestrongitalic}[,]{.codestrong}
[hours]{.codestrongitalic}[,]{.codestrong}
[minutes]{.codestrongitalic}[,]{.codestrong}
[seconds]{.codestrongitalic}[,]{.codestrong}
[milliseconds]{.codestrongitalic}[,]{.codestrong}
[microseconds]{.codestrongitalic}[)]{.codestrong} This function returns
a [timedelta]{.literal} object representing a duration of time. The
function's keyword arguments are all optional and do not include
[month]{.codeitalic} or [year]{.codeitalic}.

[total_seconds()]{.codestrong} This method for [timedelta]{.literal}
objects returns the number of seconds the [timedelta]{.literal} object
represents.

[strftime(]{.codestrong}[format]{.codestrongitalic}[)]{.codestrong} This
method returns a string of the time represented by the
[datetime]{.literal} object in a custom format that's based on the
[format]{.codeitalic} string. See [Table
17-1](#calibre_link-1522){.calibre6} for the format details.

[datetime.datetime.strptime(]{.codestrong}[time_string]{.codestrongitalic}[,]{.codestrong}
[format]{.codestrongitalic}[)]{.codestrong} This function returns a
[datetime]{.literal} object of the moment specified by
[time_string]{.codeitalic}, parsed using the [format]{.codeitalic}
string argument. See [Table 17-1](#calibre_link-1522){.calibre6} for the
format details.

### **Multithreading** {#calibre_link-545 .h1}

To introduce the concept of multithreading, let's look at an example
situation. Say you want to schedule some code to run after a delay or at
a specific time. You could add code like the following at the start of
your program:

import time, datetime\
\
startTime = datetime.datetime(2029, 10, 31, 0, 0, 0)\
while datetime.datetime.now() \< startTime:\
    time.sleep(1)\
\
print(\'Program now starting on Halloween 2029\')\
\--[snip]{.codeitalic1}\--

This code designates a start time of October 31, 2029, and keeps calling
[time.sleep(1)]{.literal} until the start time arrives. Your program
cannot do anything while waiting for the loop of
[time.sleep()]{.literal} calls to finish; it just sits around
[]{#calibre_link-1049 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}until Halloween 2029. This is because Python
programs by default have a single *thread* of execution.

To understand what a thread of execution is, remember the [Chapter
2](#calibre_link-106){.calibre6} discussion of flow control, when you
imagined the execution of a program as placing your finger on a line of
code in your program and moving to the next line or wherever it was sent
by a flow control statement. A *single-threaded* program has only one
finger. But a *multithreaded* program has multiple fingers. Each finger
still moves to the next line of code as defined by the flow control
statements, but the fingers can be at different places in the program,
executing different lines of code at the same time. (All of the programs
in this book so far have been single threaded.)

Rather than having all of your code wait until the
[time.sleep()]{.literal} function finishes, you can execute the delayed
or scheduled code in a separate thread using Python's
[threading]{.literal} module. The separate thread will pause for the
[time.sleep]{.literal} calls. Meanwhile, your program can do other work
in the original thread.

To make a separate thread, you first need to make a [Thread]{.literal}
object by calling the [threading.Thread()]{.literal} function. Enter the
following code in a new file and save it as *threadDemo.py*:

   import threading, time\
   print(\'Start of program.\')\
\
[➊]{.ent} def takeANap():\
       time.sleep(5)\
       print(\'Wake up!\')\
\
[➋]{.ent} threadObj = threading.Thread(target=takeANap)\
[➌]{.ent} threadObj.start()\
\
   print(\'End of program.\')

At [➊]{.ent}, we define a function that we want to use in a new thread.
To create a [Thread]{.literal} object, we call
[threading.Thread()]{.literal} and pass it the keyword argument
[target=takeANap]{.literal} [➋]{.ent}. This means the function we want
to call in the new thread is [takeANap()]{.literal}. Notice that the
keyword argument is [target=takeANap]{.literal}, not
[target=takeANap()]{.literal}. This is because you want to pass the
[takeANap()]{.literal} function itself as the argument, not call
[takeANap()]{.literal} and pass its return value.

After we store the [Thread]{.literal} object created by
[threading.Thread()]{.literal} in [threadObj]{.literal}, we call
[threadObj.start()]{.literal} [➌]{.ent} to create the new thread and
start executing the target function in the new thread. When this program
is run, the output will look like this:

Start of program.\
End of program.\
Wake up!

This can be a bit confusing. If [print(\'End of program.\')]{.literal}
is the last line of the program, you might think that it should be the
last thing printed. The reason [Wake up!]{.literal} comes after it is
that when [threadObj.start()]{.literal} is called, the
[]{#calibre_link-951 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}target function for [threadObj]{.literal} is run in
a new thread of execution. Think of it as a second finger appearing at
the start of the [takeANap()]{.literal} function. The main thread
continues to [print(\'End of program.\')]{.literal}. Meanwhile, the new
thread that has been executing the [time.sleep(5)]{.literal} call,
pauses for 5 seconds. After it wakes from its 5-second nap, it prints
[\'Wake up!\']{.literal} and then returns from the
[takeANap()]{.literal} function. Chronologically, [\'Wake
up!\']{.literal} is the last thing printed by the program.

Normally a program terminates when the last line of code in the file has
run (or the [sys.exit()]{.literal} function is called). But
*threadDemo.py* has two threads. The first is the original thread that
began at the start of the program and ends after [print(\'End of
program.\')]{.literal}. The second thread is created when
[threadObj.start()]{.literal} is called, begins at the start of the
[takeANap()]{.literal} function, and ends after [takeANap()]{.literal}
returns.

A Python program will not terminate until all its threads have
terminated. When you ran *threadDemo.py*, even though the original
thread had terminated, the second thread was still executing the
[time.sleep(5)]{.literal} call.

#### ***Passing Arguments to the Thread's Target Function*** {#calibre_link-546 .h2}

If the target function you want to run in the new thread takes
arguments, you can pass the target function's arguments to
[threading.Thread()]{.literal}. For example, say you wanted to run this
[print()]{.literal} call in its own thread:

\>\>\> [print(\'Cats\', \'Dogs\', \'Frogs\', sep=\' &
\')]{.codestrong1}\
Cats & Dogs & Frogs

This [print()]{.literal} call has three regular arguments,
[\'Cats\']{.literal}, [\'Dogs\']{.literal}, and [\'Frogs\']{.literal},
and one keyword argument, [sep=\' & \']{.literal}. The regular arguments
can be passed as a list to the [args]{.literal} keyword argument in
[threading.Thread()]{.literal}. The keyword argument can be specified as
a dictionary to the [kwargs]{.literal} keyword argument in
[threading.Thread()]{.literal}.

Enter the following into the interactive shell:

\>\>\> [import threading]{.codestrong1}\
\>\>\> [threadObj = threading.Thread(target=print, args=\[\'Cats\',
\'Dogs\', \'Frogs\'\],]{.codestrong1}\
[kwargs={\'sep\': \' & \'})]{.codestrong1}\
\>\>\> [threadObj.start()]{.codestrong1}\
Cats & Dogs & Frogs

To make sure the arguments [\'Cats\']{.literal}, [\'Dogs\']{.literal},
and [\'Frogs\']{.literal} get passed to [print()]{.literal} in the new
thread, we pass [args=\[\'Cats\', \'Dogs\', \'Frogs\'\]]{.literal} to
[threading.Thread()]{.literal}. To make sure the keyword argument
[sep=\' & \']{.literal} gets passed to [print()]{.literal} in the new
thread, we pass [kwargs={\'sep\': \'& \'}]{.literal} to
[threading.Thread()]{.literal}.

The [threadObj.start()]{.literal} call will create a new thread to call
the [print()]{.literal} function, and it will pass [\'Cats\']{.literal},
[\'Dogs\']{.literal}, and [\'Frogs\']{.literal} as arguments and [\' &
\']{.literal} for the [sep]{.literal} keyword argument.

This is an incorrect way to create the new thread that calls
[print()]{.literal}:

threadObj = threading.Thread(target=print(\'Cats\', \'Dogs\', \'Frogs\',
sep=\' & \'))

[]{#calibre_link-862 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}What this ends up doing is calling the
[print()]{.literal} function and passing its return value
([print()]{.literal}'s return value is always [None]{.literal}) as the
[target]{.literal} keyword argument. It *doesn't* pass the
[print()]{.literal} function itself. When passing arguments to a
function in a new thread, use the [threading.Thread()]{.literal}
function's [args]{.literal} and [kwargs]{.literal} keyword arguments.

#### ***Concurrency Issues*** {#calibre_link-547 .h2}

You can easily create several new threads and have them all running at
the same time. But multiple threads can also cause problems called
*concurrency issues*. These issues happen when threads read and write
variables at the same time, causing the threads to trip over each other.
Concurrency issues can be hard to reproduce consistently, making them
hard to debug.

Multithreaded programming is its own wide subject and beyond the scope
of this book. What you have to keep in mind is this: to avoid
concurrency issues, never let multiple threads read or write the same
variables. When you create a new [Thread]{.literal} object, make sure
its target function uses only local variables in that function. This
will avoid hard-to-debug concurrency issues in your programs.


**[NOTE]{.notes}**

*A beginner's tutorial on multithreaded programming is available at*
[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}.


### **Project: Multithreaded XKCD Downloader** {#calibre_link-548 .h1}

In [Chapter 12](#calibre_link-372){.calibre6}, you wrote a program that
downloaded all of the XKCD comic strips from the XKCD website. This was
a single-threaded program: it downloaded one comic at a time. Much of
the program's running time was spent establishing the network connection
to begin the download and writing the downloaded images to the hard
drive. If you have a broadband internet connection, your single-threaded
program wasn't fully utilizing the available bandwidth.

A multithreaded program that has some threads downloading comics while
others are establishing connections and writing the comic image files to
disk uses your internet connection more efficiently and downloads the
collection of comics more quickly. Open a new file editor tab and save
it as *threadedDownloadXkcd.py*. You will modify this program to add
multithreading. The completely modified source code is available to
download from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

#### ***Step 1: Modify the Program to Use a Function*** {#calibre_link-549 .h2}

This program will mostly be the same downloading code from [Chapter
12](#calibre_link-372){.calibre6}, so I'll skip the explanation for the
[requests]{.literal} and Beautiful Soup code. The main changes you need
to make are importing the [threading]{.literal} module and making a
[downloadXkcd()]{.literal} function, which takes starting and ending
comic numbers as parameters.

[]{#calibre_link-1085 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}For example, calling [downloadXkcd(140,
280)]{.literal} would loop over the downloading code to download the
comics at *[https://xkcd.com/140/](https://xkcd.com/140/){.calibre6}*,
*[https://xkcd.com/141/](https://xkcd.com/141/){.calibre6}*,
*[https://xkcd.com/142/](https://xkcd.com/142/){.calibre6}*, and so on,
up to *[https://xkcd.com/279/](https://xkcd.com/279/){.calibre6}*. Each
thread that you create will call [downloadXkcd()]{.literal} and pass a
different range of comics to download.

Add the following code to your *threadedDownloadXkcd.py* program:

   #! python3\
   # threadedDownloadXkcd.py - Downloads XKCD comics using multiple
threads.\
\
   import requests, os, bs4, threading\
[➊]{.ent} os.makedirs(\'xkcd\', exist_ok=True)    # store comics in
./xkcd\
\
[➋]{.ent} def downloadXkcd(startComic, endComic):\
     [➌]{.ent} for urlNumber in range(startComic, endComic):\
           # Download the page.\
           print(\'Downloading page https://xkcd.com/%s\...\' %
(urlNumber))\
        [➍]{.ent} res = requests.get(\'https://xkcd.com/%s\' %
(urlNumber))\
           res.raise_for_status()\
\
        [➎]{.ent} soup = bs4.BeautifulSoup(res.text, \'html.parser\')\
\
           # Find the URL of the comic image.\
        [➏]{.ent} comicElem = soup.select(\'#comic img\')\
           if comicElem == \[\]:\
               print(\'Could not find comic image.\')\
           else:\
            [➐]{.ent} comicUrl = comicElem\[0\].get(\'src\')\
               # Download the image.\
               print(\'Downloading image %s\...\' % (comicUrl))\
            [➑]{.ent} res = requests.get(\'https:\' + comicUrl)\
               res.raise_for_status()\
\
               # Save the image to ./xkcd.\
               imageFile = open(os.path.join(\'xkcd\',
os.path.basename(comicUrl)),\
\'wb\')\
               for chunk in res.iter_content(100000):\
                   imageFile.write(chunk)\
               imageFile.close()\
\
\# TODO: Create and start the Thread objects.\
\# TODO: Wait for all threads to end.

After importing the modules we need, we make a directory to store comics
in [➊]{.ent} and start defining [downloadxkcd()]{.literal} [➋]{.ent}. We
loop through all the numbers in the specified range [➌]{.ent} and
download each page [➍]{.ent}. We use Beautiful Soup to look through the
HTML of each page [➎]{.ent} and find the comic image [➏]{.ent}. If no
comic image is found on a page, we print a message. Otherwise, we get
the URL of the image [➐]{.ent} and download the image [➑]{.ent}.
Finally, we save the image to the directory we created.

#### []{#calibre_link-1019 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 2: Create and Start Threads*** {#calibre_link-550 .h2}

Now that we've defined [downloadXkcd()]{.literal}, we'll create the
multiple threads that each call [downloadXkcd()]{.literal} to download
different ranges of comics from the XKCD website. Add the following code
to *threadedDownloadXkcd.py* after the [downloadXkcd()]{.literal}
function definition:

#! python3\
\# threadedDownloadXkcd.py - Downloads XKCD comics using multiple
threads.\
\
\--[snip]{.codeitalic1}\--\
\
[\# Create and start the Thread objects.]{.codestrong1}\
[downloadThreads = \[\]             # a list of all the Thread
objects]{.codestrong1}\
[for i in range(0, 140, 10):    # loops 14 times, creates 14
threads]{.codestrong1}\
[    start = i]{.codestrong1}\
[    end = i + 9]{.codestrong1}\
[    if start == 0:]{.codestrong1}\
[        start = 1 \# There is no comic 0, so set it to
1.]{.codestrong1}\
[    downloadThread = threading.Thread(target=downloadXkcd, args=(start,
end))]{.codestrong1}\
[    downloadThreads.append(downloadThread)]{.codestrong1}\
[    downloadThread.start()]{.codestrong1}

First we make an empy list [downloadThreads]{.literal}; the list will
help us keep track of the many [Thread]{.literal} objects we'll create.
Then we start our [for]{.literal} loop. Each time through the loop, we
create a [Thread]{.literal} object with [threading.Thread()]{.literal},
append the [Thread]{.literal} object to the list, and call
[start()]{.literal} to start running [downloadXkcd()]{.literal} in the
new thread. Since the [for]{.literal} loop sets the [i]{.literal}
variable from [0]{.literal} to [140]{.literal} at steps of
[10]{.literal}, [i]{.literal} will be set to [0]{.literal} on the first
iteration, [10]{.literal} on the second iteration, [20]{.literal} on the
third, and so on. Since we pass [args=(start, end)]{.literal} to
[threading.Thread()]{.literal}, the two arguments passed to
[downloadXkcd()]{.literal} will be [1]{.literal} and [9]{.literal} on
the first iteration, [10]{.literal} and [19]{.literal} on the second
iteration, [20]{.literal} and [29]{.literal} on the third, and so on.

As the [Thread]{.literal} object's [start()]{.literal} method is called
and the new thread begins to run the code inside
[downloadXkcd()]{.literal}, the main thread will continue to the next
iteration of the [for]{.literal} loop and create the next thread.

#### ***Step 3: Wait for All Threads to End*** {#calibre_link-551 .h2}

The main thread moves on as normal while the other threads we create
download comics. But say there's some code you don't want to run in the
main thread until all the threads have completed. Calling a
[Thread]{.literal} object's [join()]{.literal} method will block until
that thread has finished. By using a [for]{.literal} loop to iterate
over all the [Thread]{.literal} objects in the
[downloadThreads]{.literal} list, the main thread can call the
[join()]{.literal} method on each of the other threads. Add the
following to the bottom of your program:

#! python3\
\# threadedDownloadXkcd.py - Downloads XKCD comics using multiple
threads.\
\
[]{#calibre_link-1027 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\--[snip]{.codeitalic1}\--\
\
[\# Wait for all threads to end.]{.codestrong1}\
[for downloadThread in downloadThreads:]{.codestrong1}\
[    downloadThread.join()]{.codestrong1}\
[print(\'Done.\')]{.codestrong1}

The [\'Done.\']{.literal} string will not be printed until all of the
[join()]{.literal} calls have returned. If a [Thread]{.literal} object
has already completed when its [join()]{.literal} method is called, then
the method will simply return immediately. If you wanted to extend this
program with code that runs only after all of the comics downloaded, you
could replace the [print(\'Done.\')]{.literal} line with your new code.

### **Launching Other Programs from Python** {#calibre_link-552 .h1}

Your Python program can start other programs on your computer with the
[Popen()]{.literal} function in the built-in [subprocess]{.literal}
module. (The *P* in the name of the [Popen()]{.literal} function stands
for *process*.) If you have multiple instances of an application open,
each of those instances is a separate process of the same program. For
example, if you open multiple windows of your web browser at the same
time, each of those windows is a different process of the web browser
program. See [Figure 17-1](#calibre_link-1523){.calibre6} for an example
of multiple calculator processes open at once.


[]{#calibre_link-1523
.calibre6}![image](../images/000087.jpg){.calibre3}


*Figure 17-1: Six running processes of the same calculator program*

Every process can have multiple threads. Unlike threads, a process
cannot directly read and write another process's variables. If you think
of a multithreaded program as having multiple fingers following source
code, then having multiple processes of the same program open is like
having a friend with a separate copy of the program's source code. You
are both independently executing the same program.

[]{#calibre_link-950 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}If you want to start an external program from your
Python script, pass the program's filename to
[subprocess.Popen()]{.literal}. (On Windows, right-click the
application's **Start** menu item and select **Properties** to view the
application's filename. On macOS, [CTRL]{.small}-click the application
and select **Show Package Contents** to find the path to the executable
file.) The [Popen()]{.literal} function will then immediately return.
Keep in mind that the launched program is not run in the same thread as
your Python program.

On a Windows computer, enter the following into the interactive shell:

\>\>\> [import subprocess]{.codestrong1}\
\>\>\>
[subprocess.Popen(\'C:\\\\Windows\\\\System32\\\\calc.exe\')]{.codestrong1}\
\<subprocess.Popen object at 0x0000000003055A58\>

On Ubuntu Linux, you would enter the following:

\>\>\> [import subprocess]{.codestrong1}\
\>\>\> [subprocess.Popen(\'/snap/bin/gnome-calculator\')]{.codestrong1}\
\<subprocess.Popen object at 0x7f2bcf93b20\>

On macOS, the process is slightly different. See "[Opening Files with
Default Applications](#calibre_link-557){.calibre6}" on [page
409](#calibre_link-1026){.calibre6}.

The return value is a [Popen]{.literal} object, which has two useful
methods: [poll()]{.literal} and [wait()]{.literal}.

You can think of the [poll()]{.literal} method as asking your driver
"Are we there yet?" over and over until you arrive. The
[poll()]{.literal} method will return [None]{.literal} if the process is
still running at the time [poll()]{.literal} is called. If the program
has terminated, it will return the process's integer *exit code*. An
exit code is used to indicate whether the process terminated without
errors (an exit code of [0]{.literal}) or whether an error caused the
process to terminate (a nonzero exit code---generally [1]{.literal}, but
it may vary depending on the program).

The [wait()]{.literal} method is like waiting until the driver has
arrived at your destination. The [wait()]{.literal} method will block
until the launched process has terminated. This is helpful if you want
your program to pause until the user finishes with the other program.
The return value of [wait()]{.literal} is the process's integer exit
code.

On Windows, enter the following into the interactive shell. Note that
the [wait()]{.literal} call will block until you quit the launched MS
Paint program.

   \>\>\> [import subprocess]{.codestrong1}\
[➊]{.ent} \>\>\> [paintProc =
subprocess.Popen(\'c:\\\\Windows\\\\System32\\\\mspaint.exe\')]{.codestrong1}\
[➋]{.ent} \>\>\> [paintProc.poll() == None]{.codestrong1}\
   True\
[➌]{.ent} \>\>\> [paintProc.wait() \# Doesn\'t return until MS Paint
closes.]{.codestrong1}\
   0\
   \>\>\> [paintProc.poll()]{.codestrong1}\
   0

[]{#calibre_link-856 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here we open an MS Paint process [➊]{.ent}. While
it's still running, we check whether [poll()]{.literal} returns
[None]{.literal} [➋]{.ent}. It should, as the process is still running.
Then we close the MS Paint program and call [wait()]{.literal} on the
terminated process [➌]{.ent}. Now [wait()]{.literal} and
[poll()]{.literal}return [0]{.literal}, indicating that the process
terminated without errors.


**[NOTE]{.notes}**

*Unlike* mspaint.exe, *if you run* calc.exe *on Windows 10 using*
[subprocess.Popen()]{.codeitalic}, *you'll notice that*
[wait()]{.codeitalic} *instantly returns even though the calculator app
is still running. This is because* calc.exe *launches the calculator app
and then instantly closes itself. Windows' calculator program is a
"Trusted Microsoft Store app," and its specifics are beyond the scope of
this book. Suffice it to say, programs can run in many application- and
operating system--specific ways.*


#### ***Passing Command Line Arguments to the Popen() Function*** {#calibre_link-553 .h2}

You can pass command line arguments to processes you create with
[Popen()]{.literal}. To do so, you pass a list as the sole argument to
[Popen()]{.literal}. The first string in this list will be the
executable filename of the program you want to launch; all the
subsequent strings will be the command line arguments to pass to the
program when it starts. In effect, this list will be the value of
[sys.argv]{.literal} for the launched program.

Most applications with a graphical user interface (GUI) don't use
command line arguments as extensively as command line--based or
terminal-based programs do. But most GUI applications will accept a
single argument for a file that the applications will immediately open
when they start. For example, if you're using Windows, create a simple
text file called *C:\\Users\\Al\\hello.txt* and then enter the following
into the interactive shell:

\>\>\> [subprocess.Popen(\[\'C:\\\\Windows\\\\notepad.exe\',
\'C:\\\\Users\\Al\\\\hello.txt\'\])]{.codestrong1}\
\<subprocess.Popen object at 0x00000000032DCEB8\>

This will not only launch the Notepad application but also have it
immediately open the *C:\\Users\\Al\\hello.txt* file.

#### ***Task Scheduler, launchd, and cron*** {#calibre_link-554 .h2}

If you are computer savvy, you may know about Task Scheduler on Windows,
launchd on macOS, or the cron scheduler on Linux. These well-documented
and reliable tools all allow you to schedule applications to launch at
specific times. If you'd like to learn more about them, you can find
links to tutorials at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

Using your operating system's built-in scheduler saves you from writing
your own clock-checking code to schedule your programs. However, use the
[time.sleep()]{.literal} function if you just need your program to pause
briefly. Or instead of using the operating system's scheduler, your code
can loop until a certain date and time, calling
[time.sleep(1)]{.literal} each time through the loop.

#### []{#calibre_link-1026 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Opening Websites with Python*** {#calibre_link-555 .h2}

The [webbrowser.open()]{.literal} function can launch a web browser from
your program to a specific website, rather than opening the browser
application with [subprocess.Popen()]{.literal}. See "[Project:
*mapIt.py* with the [webbrowser]{.literal}
Module](#calibre_link-373){.calibre6}" on [page
268](#calibre_link-824){.calibre6} for more details.

#### ***Running Other Python Scripts*** {#calibre_link-556 .h2}

You can launch a Python script from Python just like any other
application. Simply pass the *python.exe* executable to
[Popen()]{.literal} and the filename of the *.py* script you want to run
as its argument. For example, the following would run the *hello.py*
script from [Chapter 1](#calibre_link-89){.calibre6}:

\>\>\> [subprocess.Popen(\[\'C:\\\\Users\\\\\<YOUR
USERNAME\>\\\\AppData\\\\Local\\\\Programs\\\\]{.codestrong1}\
[Python\\\\Python38\\\\python.exe\', \'hello.py\'\])]{.codestrong1}\
\<subprocess.Popen object at 0x000000000331CF28\>

Pass [Popen()]{.literal} a list containing a string of the Python
executable's path and a string of the script's filename. If the script
you're launching needs command line arguments, add them to the list
after the script's filename. The location of the Python executable on
Windows is *C:\\Users\\\<YOUR
USERNAME\>\\AppData\\Local\\Programs\\Python\\Python38\\python.exe*. On
macOS, it is
*/Library/Frameworks/Python.framework/Versions/3.8/bin/python3*. On
Linux, it is */usr/bin/python3.8*.

Unlike importing the Python program as a module, when your Python
program launches another Python program, the two are run in separate
processes and will not be able to share each other's variables.

#### ***Opening Files with Default Applications*** {#calibre_link-557 .h2}

Double-clicking a *.txt* file on your computer will automatically launch
the application associated with the *.txt* file extension. Your computer
will have several of these file extension associations set up already.
Python can also open files this way with [Popen()]{.literal}.

Each operating system has a program that performs the equivalent of
double-clicking a document file to open it. On Windows, this is the
[start]{.literal} program. On macOS, this is the [open]{.literal}
program. On Ubuntu Linux, this is the [see]{.literal} program. Enter the
following into the interactive shell, passing [\'start\']{.literal},
[\'open\']{.literal}, or [\'see\']{.literal} to [Popen()]{.literal}
depending on your system:

\>\>\> [fileObj = open(\'hello.txt\', \'w\')]{.codestrong1}\
\>\>\> [fileObj.write(\'Hello, world!\')]{.codestrong1}\
12\
\>\>\> [fileObj.close()]{.codestrong1}\
\>\>\> [import subprocess]{.codestrong1}\
\>\>\>
[subprocess.Popen(\[\']{.codestrong1}[[start]{.codestrong1}]{.codeitalic1}[\',
\'hello.txt\'\], shell=True)]{.codestrong1}

[]{#calibre_link-873 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Here we write [Hello, world!]{.literal} to a new
*hello.txt* file. Then we call [Popen()]{.literal}, passing it a list
containing the program name (in this example, [\'start\']{.literal} for
Windows) and the filename. We also pass the [shell=True]{.literal}
keyword argument, which is needed only on Windows. The operating system
knows all of the file associations and can figure out that it should
launch, say, *Notepad.exe* to handle the *hello.txt* file.

On macOS, the [open]{.literal} program is used for opening both document
files and programs. Enter the following into the interactive shell if
you have a Mac:

\>\>\> [subprocess.Popen(\[\'open\',
\'/Applications/Calculator.app/\'\])]{.codestrong1}\
\<subprocess.Popen object at 0x10202ff98\>

The Calculator app should open.

### **Project: Simple Countdown Program** {#calibre_link-558 .h1}

Just like it's hard to find a simple stopwatch application, it can be
hard to find a simple countdown application. Let's write a countdown
program that plays an alarm at the end of the countdown.

At a high level, here's what your program will do:

1.  Count down from 60.
2.  Play a sound file (*alarm.wav*) when the countdown reaches zero.

This means your code will need to do the following:

1.  Pause for 1 second in between displaying each number in the
    countdown by calling [time.sleep()]{.literal}.
2.  Call [subprocess.Popen()]{.literal} to open the sound file with the
    default application.

Open a new file editor tab and save it as *countdown.py*.

#### ***Step 1: Count Down*** {#calibre_link-559 .h2}

This program will require the [time]{.literal} module for the
[time.sleep()]{.literal} function and the [subprocess]{.literal} module
for the [subprocess.Popen()]{.literal} function. Enter the following
code and save the file as *countdown.py*:

   #! python3\
   # countdown.py - A simple countdown script.\
\
   import time, subprocess\
\
[➊]{.ent} timeLeft = 60\
   while timeLeft \> 0:\
    [➋]{.ent} print(timeLeft, end=\'\')\
    [➌]{.ent} time.sleep(1)\
[]{#calibre_link-1081 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    [➍]{.ent} timeLeft = timeLeft - 1\
\
  # TODO: At the end of the countdown, play a sound file.

After importing [time]{.literal} and [subprocess]{.literal}, make a
variable called [timeLeft]{.literal} to hold the number of seconds left
in the countdown [➊]{.ent}. It can start at 60---or you can change the
value here to whatever you need, or even have it get set from a command
line argument.

In a [while]{.literal} loop, you display the remaining count [➋]{.ent},
pause for 1 second [➌]{.ent}, and then decrement the
[timeLeft]{.literal} variable [➍]{.ent} before the loop starts over
again. The loop will keep looping as long as [timeLeft]{.literal} is
greater than [0]{.literal}. After that, the countdown will be over.

#### ***Step 2: Play the Sound File*** {#calibre_link-560 .h2}

While there are third-party modules to play sound files of various
formats, the quick and easy way is to just launch whatever application
the user already uses to play sound files. The operating system will
figure out from the *.wav* file extension which application it should
launch to play the file. This *.wav* file could easily be some other
sound file format, such as *.mp3* or *.ogg*.

You can use any sound file that is on your computer to play at the end
of the countdown, or you can download *alarm.wav* from
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.

Add the following to your code:

#! python3\
\# countdown.py - A simple countdown script.\
\
import time, subprocess\
\
\--[snip]{.codeitalic1}\--\
\
[\# At the end of the countdown, play a sound file.]{.codestrong1}\
[subprocess.Popen(\[\'start\', \'alarm.wav\'\],
shell=True)]{.codestrong1}

After the [while]{.literal} loop finishes, *alarm.wav* (or the sound
file you choose) will play to notify the user that the countdown is
over. On Windows, be sure to include [\'start\']{.literal} in the list
you pass to [Popen()]{.literal} and pass the keyword argument
[shell=True]{.literal}. On macOS, pass [\'open\']{.literal} instead of
[\'start\']{.literal} and remove [shell=True]{.literal}.

Instead of playing a sound file, you could save a text file somewhere
with a message like *Break time is over!* and use [Popen()]{.literal} to
open it at the end of the countdown. This will effectively create a
pop-up window with a message. Or you could use the
[webbrowser.open()]{.literal} function to open a specific website at the
end of the countdown. Unlike some free countdown application you'd find
online, your own countdown program's alarm can be anything you want!

#### []{#calibre_link-1067 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Ideas for Similar Programs*** {#calibre_link-561 .h2}

A countdown is a simple delay before continuing the program's execution.
This can also be used for other applications and features, such as the
following:

-   Use [time.sleep()]{.literal} to give the user a chance to press
    [CTRL]{.small}-C to cancel an action, such as deleting files. Your
    program can print a "Press [CTRL]{.small}-C to cancel" message and
    then handle any [KeyboardInterrupt]{.literal} exceptions with
    [try]{.literal} and [except]{.literal} statements.
-   For a long-term countdown, you can use [timedelta]{.literal} objects
    to measure the number of days, hours, minutes, and seconds until
    some point (a birthday? an anniversary?) in the future.

### **Summary** {#calibre_link-562 .h1}

The Unix epoch (January 1, 1970, at midnight, UTC) is a standard
reference time for many programming languages, including Python. While
the [time.time()]{.literal} function module returns an epoch timestamp
(that is, a float value of the number of seconds since the Unix epoch),
the [datetime]{.literal} module is better for performing date arithmetic
and formatting or parsing strings with date information.

The [time.sleep()]{.literal} function will block (that is, not return)
for a certain number of seconds. It can be used to add pauses to your
program. But if you want to schedule your programs to start at a certain
time, the instructions at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
can tell you how to use the scheduler already provided by your operating
system.

The [threading]{.literal} module is used to create multiple threads,
which is useful when you need to download multiple files or do other
tasks simultaneously. But make sure the thread reads and writes only
local variables, or you might run into concurrency issues.

Finally, your Python programs can launch other applications with the
[subprocess.Popen()]{.literal} function. Command line arguments can be
passed to the [Popen()]{.literal} call to open specific documents with
the application. Alternatively, you can use the [start]{.literal},
[open]{.literal}, or [see]{.literal} program with [Popen()]{.literal} to
use your computer's file associations to automatically figure out which
application to use to open a document. By using the other applications
on your computer, your Python programs can leverage their capabilities
for your automation needs.

### **Practice Questions** {#calibre_link-563 .h1}

[1](#calibre_link-1524){#calibre_link-1446 .calibre6}. What is the Unix
epoch?

[2](#calibre_link-1525){#calibre_link-1447 .calibre6}. What function
returns the number of seconds since the Unix epoch?

[3](#calibre_link-1526){#calibre_link-1448 .calibre6}. How can you pause
your program for exactly 5 seconds?

[4](#calibre_link-1527){#calibre_link-1449 .calibre6}.
[]{#calibre_link-1846 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}What does the [round()]{.literal} function return?

[5](#calibre_link-1528){#calibre_link-1450 .calibre6}. What is the
difference between a [datetime]{.literal} object and a
[timedelta]{.literal} object?

[6](#calibre_link-1529){#calibre_link-1451 .calibre6}. Using the
[datetime]{.literal} module, what day of the week was January 7, 2019?

[7](#calibre_link-1530){#calibre_link-1452 .calibre6}. Say you have a
function named [spam()]{.literal}. How can you call this function and
run the code inside it in a separate thread?

[8](#calibre_link-1531){#calibre_link-1453 .calibre6}. What should you
do to avoid concurrency issues with multiple threads?

### **Practice Projects** {#calibre_link-564 .h1}

For practice, write programs that do the following.

#### ***Prettified Stopwatch*** {#calibre_link-565 .h2}

Expand the stopwatch project from this chapter so that it uses the
[rjust()]{.literal} and [ljust()]{.literal} string methods to "prettify"
the output. (These methods were covered in [Chapter
6](#calibre_link-207){.calibre6}.) Instead of output such as this:

Lap #1: 3.56 (3.56)\
Lap #2: 8.63 (5.07)\
Lap #3: 17.68 (9.05)\
Lap #4: 19.11 (1.43)

. . . the output will look like this:

Lap \# 1:   3.56 (  3.56)\
Lap \# 2:   8.63 (  5.07)\
Lap \# 3:  17.68 (  9.05)\
Lap \# 4:  19.11 (  1.43)

Note that you will need string versions of the [lapNum]{.literal},
[lapTime]{.literal}, and [totalTime]{.literal} integer and float
variables in order to call the string methods on them.

Next, use the [pyperclip]{.literal} module introduced in [Chapter
6](#calibre_link-207){.calibre6} to copy the text output to the
clipboard so the user can quickly paste the output to a text file or
email.

#### ***Scheduled Web Comic Downloader*** {#calibre_link-566 .h2}

Write a program that checks the websites of several web comics and
automatically downloads the images if the comic was updated since the
program's last visit. Your operating system's scheduler (Scheduled Tasks
on Windows, launchd on macOS, and cron on Linux) can run your Python
program once a day. The Python program itself can download the comic and
then copy it to your desktop so that it is easy to find. This will free
you from having to check the website yourself to see whether it has
updated. (A list of web comics is available at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*.)[]{#calibre_link-1847
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}

