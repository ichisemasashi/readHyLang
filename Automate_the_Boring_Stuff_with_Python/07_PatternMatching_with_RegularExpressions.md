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

[![](/images/prev.png)](../chapter6)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter8)

</div>

::: {#calibre_link-1142 .calibre}
## []{#calibre_link-1076 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**[7]{.big} PATTERN MATCHING WITH REGULAR EXPRESSIONS** {#calibre_link-238 .h2b}

::: image
![Image](../images/000015.jpg){.calibre3}
:::

You may be familiar with searching for text by pressing [CTRL-]{.small}F
and entering the words you're looking for. *Regular expressions* go one
step further: they allow you to specify a *pattern* of text to search
for. You may not know a business's exact phone number, but if you live
in the United States or Canada, you know it will be three digits,
followed by a hyphen, and then four more digits (and optionally, a
three-digit area code at the start). This is how you, as a human, know a
phone number when you see it: 415-555-1234 is a phone number, but
4,155,551,234 is not.

We also recognize all sorts of other text patterns every day: email
addresses have @ symbols in the middle, US social security numbers have
nine digits and two hyphens, website URLs often have periods and forward
slashes, news headlines use title case, social media hashtags begin with
\# and contain no spaces, and more.

[]{#calibre_link-1077 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Regular expressions are helpful, but few
non-programmers know about them even though most modern text editors and
word processors, such as Microsoft Word or OpenOffice, have find and
find-and-replace features that can search based on regular expressions.
Regular expressions are huge time-savers, not just for software users
but also for programmers. In fact, tech writer Cory Doctorow argues that
we should be teaching regular expressions even before programming:

Knowing \[regular expressions\] can mean the difference between solving
a problem in 3 steps and solving it in 3,000 steps. When you're a nerd,
you forget that the problems you solve with a couple keystrokes can take
other people days of tedious, error-prone work to slog
through.^[1](#calibre_link-1143){#calibre_link-1169 .calibre6}^

In this chapter, you'll start by writing a program to find text patterns
*without* using regular expressions and then see how to use regular
expressions to make the code much less bloated. I'll show you basic
matching with regular expressions and then move on to some more powerful
features, such as string substitution and creating your own character
classes. Finally, at the end of the chapter, you'll write a program that
can automatically extract phone numbers and email addresses from a block
of text.

### **Finding Patterns of Text Without Regular Expressions** {#calibre_link-239 .h1}

Say you want to find an American phone number in a string. You know the
pattern if you're American: three numbers, a hyphen, three numbers, a
hyphen, and four numbers. Here's an example: 415-555-4242.

Let's use a function named [isPhoneNumber()]{.literal} to check whether
a string matches this pattern, returning either [True]{.literal} or
[False]{.literal}. Open a new file editor tab and enter the following
code; then save the file as *isPhoneNumber.py*:

def isPhoneNumber(text):\
  [➊]{.ent} if len(text) != 12:\
         return False\
     for i in range(0, 3):\
      [➋]{.ent} if not text\[i\].isdecimal():\
             return False\
  [➌]{.ent} if text\[3\] != \'-\':\
         return False\
     for i in range(4, 7):\
      [➍]{.ent} if not text\[i\].isdecimal():\
             return False\
  [➎]{.ent} if text\[7\] != \'-\':\
         return False\
 []{#calibre_link-1767 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    for i in range(8, 12):\
      [➏]{.ent} if not text\[i\].isdecimal():\
             return False\
  [➐]{.ent} return True\
\
print(\'Is 415-555-4242 a phone number?\')\
print(isPhoneNumber(\'415-555-4242\'))\
print(\'Is Moshi moshi a phone number?\')\
print(isPhoneNumber(\'Moshi moshi\'))

When this program is run, the output looks like this:

Is 415-555-4242 a phone number?\
True\
Is Moshi moshi a phone number?\
False

The [isPhoneNumber()]{.literal} function has code that does several
checks to see whether the string in [text]{.literal} is a valid phone
number. If any of these checks fail, the function returns
[False]{.literal}. First the code checks that the string is exactly 12
characters [➊]{.ent}. Then it checks that the area code (that is, the
first three characters in [text]{.literal}) consists of only numeric
characters [➋]{.ent}. The rest of the function checks that the string
follows the pattern of a phone number: the number must have the first
hyphen after the area code [➌]{.ent}, three more numeric characters
[➍]{.ent}, then another hyphen [➎]{.ent}, and finally four more numbers
[➏]{.ent}. If the program execution manages to get past all the checks,
it returns [True]{.literal} [➐]{.ent}.

Calling [isPhoneNumber()]{.literal} with the argument
[\'415-555-4242\']{.literal} will return [True]{.literal}. Calling
[isPhoneNumber()]{.literal} with [\'Moshi moshi\']{.literal} will return
[False]{.literal}; the first test fails because [\'Moshi
moshi\']{.literal} is not 12 characters long.

If you wanted to find a phone number within a larger string, you would
have to add even more code to find the phone number pattern. Replace the
last four [print()]{.literal} function calls in *isPhoneNumber.py* with
the following:

message = \'Call me at 415-555-1011 tomorrow. 415-555-9999 is my
office.\'\
for i in range(len(message)):\
  [➊]{.ent} chunk = message\[i:i+12\]\
  [➋]{.ent} if isPhoneNumber(chunk):\
          print(\'Phone number found: \' + chunk)\
print(\'Done\')

When this program is run, the output will look like this:

Phone number found: 415-555-1011\
Phone number found: 415-555-9999\
Done

[]{#calibre_link-1768 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}On each iteration of the [for]{.literal} loop, a
new chunk of 12 characters from [message]{.literal} is assigned to the
variable [chunk]{.literal} [➊]{.ent}. For example, on the first
iteration, [i]{.literal} is [0]{.literal}, and [chunk]{.literal} is
assigned [message\[0:12\]]{.literal} (that is, the string [\'Call me at
4\']{.literal}). On the next iteration, [i]{.literal} is [1]{.literal},
and [chunk]{.literal} is assigned [message\[1:13\]]{.literal} (the
string [\'all me at 41\']{.literal}). In other words, on each iteration
of the [for]{.literal} loop, [chunk]{.literal} takes on the following
values:

-   [\'Call me at 4\']{.literal}
-   [\'all me at 41\']{.literal}
-   [\'ll me at 415\']{.literal}
-   [\'l me at 415-\']{.literal}
-   . . . and so on.

You pass [chunk]{.literal} to [isPhoneNumber()]{.literal} to see whether
it matches the phone number pattern [➋]{.ent}, and if so, you print the
chunk.

Continue to loop through [message]{.literal}, and eventually the 12
characters in [chunk]{.literal} will be a phone number. The loop goes
through the entire string, testing each 12-character piece and printing
any [chunk]{.literal} it finds that satisfies
[isPhoneNumber()]{.literal}. Once we're done going through
[message]{.literal}, we print [Done]{.literal}.

While the string in [message]{.literal} is short in this example, it
could be millions of characters long and the program would still run in
less than a second. A similar program that finds phone numbers using
regular expressions would also run in less than a second, but regular
expressions make it quicker to write these programs.

### **Finding Patterns of Text with Regular Expressions** {#calibre_link-240 .h1}

The previous phone number--finding program works, but it uses a lot of
code to do something limited: the [isPhoneNumber()]{.literal} function
is 17 lines but can find only one pattern of phone numbers. What about a
phone number formatted like 415.555.4242 or (415) 555-4242? What if the
phone number had an extension, like 415-555-4242 x99? The
[isPhoneNumber()]{.literal} function would fail to validate them. You
could add yet more code for these additional patterns, but there is an
easier way.

Regular expressions, called *regexes* for short, are descriptions for a
pattern of text. For example, a [\\d]{.literal} in a regex stands for a
digit character---that is, any single numeral from 0 to 9. The regex
[\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d]{.literal} is used by Python to match
the same text pattern the previous [isPhoneNumber()]{.literal} function
did: a string of three numbers, a hyphen, three more numbers, another
hyphen, and four numbers. Any other string would not match the
[\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d]{.literal} regex.

But regular expressions can be much more sophisticated. For example,
adding a [3]{.literal} in braces ([{3}]{.literal}) after a pattern is
like saying, "Match this pattern three times." So the slightly shorter
regex [\\d{3}-\\d{3}-\\d{4}]{.literal} also matches the correct phone
number format.

#### []{#calibre_link-857 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Creating Regex Objects*** {#calibre_link-241 .h2}

All the regex functions in Python are in the [re]{.literal} module.
Enter the following into the interactive shell to import this module:

\>\>\> [import re]{.codestrong1}

::: note
**[NOTE]{.notes}**

*Most of the examples in this chapter will require the [re]{.codeitalic}
module, so remember to import it at the beginning of any script you
write or any time you restart Mu. Otherwise, you'll get a [NameError:
name \'re\' is not defined]{.codeitalic} error message.*
:::

Passing a string value representing your regular expression to
[re.compile()]{.literal} returns a [Regex]{.literal} pattern object (or
simply, a [Regex]{.literal} object).

To create a [Regex]{.literal} object that matches the phone number
pattern, enter the following into the interactive shell. (Remember that
[\\d]{.literal} means "a digit character" and
[\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d]{.literal} is the regular expression
for a phone number pattern.)

\>\>\> [phoneNumRegex =
re.compile(r\'\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d\')]{.codestrong1}

Now the [phoneNumRegex]{.literal} variable contains a [Regex]{.literal}
object.

#### ***Matching Regex Objects*** {#calibre_link-242 .h2}

A [Regex]{.literal} object's [search()]{.literal} method searches the
string it is passed for any matches to the regex. The
[search()]{.literal} method will return [None]{.literal} if the regex
pattern is not found in the string. If the pattern *is* found, the
[search()]{.literal} method returns a [Match]{.literal} object, which
have a [group()]{.literal} method that will return the actual matched
text from the searched string. (I'll explain groups shortly.) For
example, enter the following into the interactive shell:

\>\>\> [phoneNumRegex =
re.compile(r\'\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d\')]{.codestrong1}\
\>\>\> [mo = phoneNumRegex.search(\'My number is
415-555-4242.\')]{.codestrong1}\
\>\>\> [print(\'Phone number found: \' + mo.group())]{.codestrong1}\
Phone number found: 415-555-4242

The [mo]{.literal} variable name is just a generic name to use for
[Match]{.literal} objects. This example might seem complicated at first,
but it is much shorter than the earlier *isPhoneNumber.py* program and
does the same thing.

Here, we pass our desired pattern to [re.compile()]{.literal} and store
the resulting [Regex]{.literal} object in [phoneNumRegex]{.literal}.
Then we call [search()]{.literal} on [phoneNumRegex]{.literal} and pass
[search()]{.literal} the string we want to match for during the search.
The result of the search gets stored in the variable [mo]{.literal}. In
this example, we know that our pattern will be found in the string, so
we know that a [Match]{.literal} object will be returned. Knowing that
[mo]{.literal} contains a [Match]{.literal} object and not the null
value [None]{.literal}, we can call [group()]{.literal} on
[mo]{.literal} to return the match. Writing [mo.group()]{.literal}
inside our [print()]{.literal} function call displays the whole match,
[415-555-4242]{.literal}.

#### []{#calibre_link-766 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Review of Regular Expression Matching*** {#calibre_link-243 .h2}

While there are several steps to using regular expressions in Python,
each step is fairly simple.

1.  Import the regex module with [import re]{.literal}.
2.  Create a [Regex]{.literal} object with the [re.compile()]{.literal}
    function. (Remember to use a raw string.)
3.  Pass the string you want to search into the [Regex]{.literal}
    object's [search()]{.literal} method. This returns a
    [Match]{.literal} object.
4.  Call the [Match]{.literal} object's [group()]{.literal} method to
    return a string of the actual matched text.

::: note
**[NOTE]{.notes}**

*While I encourage you to enter the example code into the interactive
shell, you should also make use of web-based regular expression testers,
which can show you exactly how a regex matches a piece of text that you
enter. I recommend the tester at*
[https://pythex.org/](https://pythex.org/){.calibre6}.
:::

### **More Pattern Matching with Regular Expressions** {#calibre_link-244 .h1}

Now that you know the basic steps for creating and finding regular
expression objects using Python, you're ready to try some of their more
powerful pattern-matching capabilities.

#### ***Grouping with Parentheses*** {#calibre_link-245 .h2}

Say you want to separate the area code from the rest of the phone
number. Adding parentheses will create *groups* in the regex:
[(\\d\\d\\d)-(\\d\\d\\d-\\d\\d\\d\\d)]{.literal}. Then you can use the
[group()]{.literal} match object method to grab the matching text from
just one group.

The first set of parentheses in a regex string will be group
[1]{.literal}. The second set will be group [2]{.literal}. By passing
the *integer* [1]{.literal} or [2]{.literal} to the [group()]{.literal}
match object method, you can grab different parts of the matched text.
Passing [0]{.literal} or nothing to the [group()]{.literal} method will
return the entire matched text. Enter the following into the interactive
shell:

\>\>\> [phoneNumRegex =
re.compile(r\'(\\d\\d\\d)-(\\d\\d\\d-\\d\\d\\d\\d)\')]{.codestrong1}\
\>\>\> [mo = phoneNumRegex.search(\'My number is
415-555-4242.\')]{.codestrong1}\
\>\>\> [mo.group(1)]{.codestrong1}\
\'415\'\
\>\>\> [mo.group(2)]{.codestrong1}\
\'555-4242\'\
\>\>\> [mo.group(0)]{.codestrong1}\
\'415-555-4242\'\
\>\>\> [mo.group()]{.codestrong1}\
\'415-555-4242\'

If you would like to retrieve all the groups at once, use the
[groups()]{.literal} method---note the plural form for the name.

[]{#calibre_link-1769 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [mo.groups()]{.codestrong1}\
(\'415\', \'555-4242\')\
\>\>\> [areaCode, mainNumber = mo.groups()]{.codestrong1}\
\>\>\> [print(areaCode)]{.codestrong1}\
415\
\>\>\> [print(mainNumber)]{.codestrong1}\
555-4242

Since [mo.groups()]{.literal} returns a tuple of multiple values, you
can use the multiple-assignment trick to assign each value to a separate
variable, as in the previous [areaCode, mainNumber =
mo.groups()]{.literal} line.

Parentheses have a special meaning in regular expressions, but what do
you do if you need to match a parenthesis in your text? For instance,
maybe the phone numbers you are trying to match have the area code set
in parentheses. In this case, you need to escape the [(]{.literal} and
[)]{.literal} characters with a backslash. Enter the following into the
interactive shell:

\>\>\> [phoneNumRegex = re.compile(r\'(\\(\\d\\d\\d\\))
(\\d\\d\\d-\\d\\d\\d\\d)\')]{.codestrong1}\
\>\>\> [mo = phoneNumRegex.search(\'My phone number is (415)
555-4242.\')]{.codestrong1}\
\>\>\> [mo.group(1)]{.codestrong1}\
\'(415)\'\
\>\>\> [mo.group(2)]{.codestrong1}\
\'555-4242\'

The [\\(]{.literal} and [\\)]{.literal} escape characters in the raw
string passed to [re.compile()]{.literal} will match actual parenthesis
characters. In regular expressions, the following characters have
special meanings:

.  \^  \$  \*  +  ?  {  }  \[  \]  \\  \|  (  )

If you want to detect these characters as part of your text pattern, you
need to escape them with a backslash:

\\.  \\\^  \\\$  \\\*  \\+  \\?  \\{  \\}  \\\[  \\\]  \\\\  \\\|  \\(  \\)

Make sure to double-check that you haven't mistaken escaped parentheses
[\\(]{.literal} and [\\)]{.literal} for parentheses [(]{.literal} and
[)]{.literal} in a regular expression. If you receive an error message
about "missing )" or "unbalanced parenthesis," you may have forgotten to
include the closing unescaped parenthesis for a group, like in this
example:

\>\>\> [re.compile(r\'(\\(Parentheses\\)\')]{.codestrong1}\
Traceback (most recent call last):\
    \--[snip]{.codeitalic1}\--\
re.error: missing ), unterminated subpattern at position 0

The error message tells you that there is an opening parenthesis at
index [0]{.literal} of the [r\'(\\(Parentheses\\)\']{.literal} string
that is missing its corresponding closing parenthesis.

#### []{#calibre_link-767 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Matching Multiple Groups with the Pipe*** {#calibre_link-246 .h2}

The [\|]{.literal} character is called a *pipe*. You can use it anywhere
you want to match one of many expressions. For example, the regular
expression [r\'Batman\|Tina Fey\']{.literal} will match either
[\'Batman\']{.literal} or [\'Tina Fey\']{.literal}.

When *both* Batman and Tina Fey occur in the searched string, the first
occurrence of matching text will be returned as the [Match]{.literal}
object. Enter the following into the interactive shell:

\>\>\> [heroRegex = re.compile (r\'Batman\|Tina Fey\')]{.codestrong1}\
\>\>\> [mo1 = heroRegex.search(\'Batman and Tina Fey\')]{.codestrong1}\
\>\>\> [mo1.group()]{.codestrong1}\
\'Batman\'\
\
\>\>\> [mo2 = heroRegex.search(\'Tina Fey and Batman\')]{.codestrong1}\
\>\>\> [mo2.group()]{.codestrong1}\
\'Tina Fey\'

::: note
**[NOTE]{.notes}**

*You can find [all]{.codeitalic} matching occurrences with the
[findall()]{.codeitalic} method that's discussed in "[The findall()
Method](#calibre_link-252){.calibre6}" on [page
171](#calibre_link-746){.calibre6}.*
:::

You can also use the pipe to match one of several patterns as part of
your regex. For example, say you wanted to match any of the strings
[\'Batman\']{.literal}, [\'Batmobile\']{.literal},
[\'Batcopter\']{.literal}, and [\'Batbat\']{.literal}. Since all these
strings start with [Bat]{.literal}, it would be nice if you could
specify that prefix only once. This can be done with parentheses. Enter
the following into the interactive shell:

\>\>\> [batRegex =
re.compile(r\'Bat(man\|mobile\|copter\|bat)\')]{.codestrong1}\
\>\>\> [mo = batRegex.search(\'Batmobile lost a wheel\')]{.codestrong1}\
\>\>\> [mo.group()]{.codestrong1}\
\'Batmobile\'\
\>\>\> [mo.group(1)]{.codestrong1}\
\'mobile\'

The method call [mo.group()]{.literal} returns the full matched text
[\'Batmobile\']{.literal}, while [mo.group(1)]{.literal} returns just
the part of the matched text inside the first parentheses group,
[\'mobile\']{.literal}. By using the pipe character and grouping
parentheses, you can specify several alternative patterns you would like
your regex to match.

If you need to match an actual pipe character, escape it with a
backslash, like [\\\|]{.literal}.

#### ***Optional Matching with the Question Mark*** {#calibre_link-247 .h2}

Sometimes there is a pattern that you want to match only optionally.
That is, the regex should find a match regardless of whether that bit of
text is there. The [?]{.literal} character flags the group that precedes
it as an optional part of the pattern. For example, enter the following
into the interactive shell:

\>\>\> [batRegex = re.compile(r\'Bat(wo)?man\')]{.codestrong1}\
\>\>\> [mo1 = batRegex.search(\'The Adventures of
Batman\')]{.codestrong1}\
[]{#calibre_link-770 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}\>\>\> [mo1.group()]{.codestrong1}\
\'Batman\'\
\
\>\>\> [mo2 = batRegex.search(\'The Adventures of
Batwoman\')]{.codestrong1}\
\>\>\> [mo2.group()]{.codestrong1}\
\'Batwoman\'

The [(wo)?]{.literal} part of the regular expression means that the
pattern [wo]{.literal} is an optional group. The regex will match text
that has zero instances or one instance of *wo* in it. This is why the
regex matches both [\'Batwoman\']{.literal} and [\'Batman\']{.literal}.

Using the earlier phone number example, you can make the regex look for
phone numbers that do or do not have an area code. Enter the following
into the interactive shell:

\>\>\> [phoneRegex =
re.compile(r\'(\\d\\d\\d-)?\\d\\d\\d-\\d\\d\\d\\d\')]{.codestrong1}\
\>\>\> [mo1 = phoneRegex.search(\'My number is
415-555-4242\')]{.codestrong1}\
\>\>\> [mo1.group()]{.codestrong1}\
\'415-555-4242\'\
\
\>\>\> [mo2 = phoneRegex.search(\'My number is
555-4242\')]{.codestrong1}\
\>\>\> [mo2.group()]{.codestrong1}\
\'555-4242\'

You can think of the [?]{.literal} as saying, "Match zero or one of the
group preceding this question mark."

If you need to match an actual question mark character, escape it with
[\\?]{.literal}.

#### ***Matching Zero or More with the Star*** {#calibre_link-248 .h2}

The [\*]{.literal} (called the *star* or *asterisk*) means "match zero
or more"---the group that precedes the star can occur any number of
times in the text. It can be completely absent or repeated over and over
again. Let's look at the Batman example again.

\>\>\> [batRegex = re.compile(r\'Bat(wo)\*man\')]{.codestrong1}\
\>\>\> [mo1 = batRegex.search(\'The Adventures of
Batman\')]{.codestrong1}\
\>\>\> [mo1.group()]{.codestrong1}\
\'Batman\'\
\
\>\>\> [mo2 = batRegex.search(\'The Adventures of
Batwoman\')]{.codestrong1}\
\>\>\> [mo2.group()]{.codestrong1}\
\'Batwoman\'\
\
\>\>\> [mo3 = batRegex.search(\'The Adventures of
Batwowowowoman\')]{.codestrong1}\
\>\>\> [mo3.group()]{.codestrong1}\
\'Batwowowowoman\'

For [\'Batman\']{.literal}, the [(wo)\*]{.literal} part of the regex
matches zero instances of [wo]{.literal} in the string; for
[\'Batwoman\']{.literal}, the [(wo)\*]{.literal} matches one instance of
[wo]{.literal}; and for [\'Batwowowowoman\']{.literal},
[(wo)\*]{.literal} matches four instances of [wo]{.literal}.

If you need to match an actual star character, prefix the star in the
regular expression with a backslash, [\\\*]{.literal}.

#### []{#calibre_link-748 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Matching One or More with the Plus*** {#calibre_link-249 .h2}

While [\*]{.literal} means "match zero or more," the [+]{.literal} (or
*plus*) means "match one or more." Unlike the star, which does not
require its group to appear in the matched string, the group preceding a
plus must appear *at least once*. It is not optional. Enter the
following into the interactive shell, and compare it with the star
regexes in the previous section:

\>\>\> [batRegex = re.compile(r\'Bat(wo)+man\')]{.codestrong1}\
\>\>\> [mo1 = batRegex.search(\'The Adventures of
Batwoman\')]{.codestrong1}\
\>\>\> [mo1.group()]{.codestrong1}\
\'Batwoman\'\
\
\>\>\> [mo2 = batRegex.search(\'The Adventures of
Batwowowowoman\')]{.codestrong1}\
\>\>\> [mo2.group()]{.codestrong1}\
\'Batwowowowoman\'\
\
\>\>\> [mo3 = batRegex.search(\'The Adventures of
Batman\')]{.codestrong1}\
\>\>\> [mo3 == None]{.codestrong1}\
True

The regex [Bat(wo)+man]{.literal} will not match the string [\'The
Adventures of Batman\']{.literal}, because at least one [wo]{.literal}
is required by the plus sign.

If you need to match an actual plus sign character, prefix the plus sign
with a backslash to escape it: [\\+]{.literal}.

#### ***Matching Specific Repetitions with Braces*** {#calibre_link-250 .h2}

If you have a group that you want to repeat a specific number of times,
follow the group in your regex with a number in braces. For example, the
regex [(Ha){3}]{.literal} will match the string [\'HaHaHa\']{.literal},
but it will not match [\'HaHa\']{.literal}, since the latter has only
two repeats of the [(Ha)]{.literal} group.

Instead of one number, you can specify a range by writing a minimum, a
comma, and a maximum in between the braces. For example, the regex
[(Ha){3,5}]{.literal} will match [\'HaHaHa\']{.literal},
[\'HaHaHaHa\']{.literal}, and [\'HaHaHaHaHa\']{.literal}.

You can also leave out the first or second number in the braces to leave
the minimum or maximum unbounded. For example, [(Ha){3,}]{.literal} will
match three or more instances of the [(Ha)]{.literal} group, while
[(Ha){,5}]{.literal} will match zero to five instances. Braces can help
make your regular expressions shorter. These two regular expressions
match identical patterns:

(Ha){3}\
(Ha)(Ha)(Ha)

And these two regular expressions also match identical patterns:

(Ha){3,5}\
((Ha)(Ha)(Ha))\|((Ha)(Ha)(Ha)(Ha))\|((Ha)(Ha)(Ha)(Ha)(Ha))

[]{#calibre_link-746 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Enter the following into the interactive shell:

\>\>\> [haRegex = re.compile(r\'(Ha){3}\')]{.codestrong1}\
\>\>\> [mo1 = haRegex.search(\'HaHaHa\')]{.codestrong1}\
\>\>\> [mo1.group()]{.codestrong1}\
\'HaHaHa\'\
\
\>\>\> [mo2 = haRegex.search(\'Ha\')]{.codestrong1}\
\>\>\> [mo2 == None]{.codestrong1}\
True

Here, [(Ha){3}]{.literal} matches [\'HaHaHa\']{.literal} but not
[\'Ha\']{.literal}. Since it doesn't match [\'Ha\']{.literal},
[search()]{.literal} returns [None]{.literal}.

### **Greedy and Non-greedy Matching** {#calibre_link-251 .h1}

Since [(Ha){3,5}]{.literal} can match three, four, or five instances of
[Ha]{.literal} in the string [\'HaHaHaHaHa\']{.literal}, you may wonder
why the [Match]{.literal} object's call to [group()]{.literal} in the
previous brace example returns [\'HaHaHaHaHa\']{.literal} instead of the
shorter possibilities. After all, [\'HaHaHa\']{.literal} and
[\'HaHaHaHa\']{.literal} are also valid matches of the regular
expression [(Ha){3,5}]{.literal}.

Python's regular expressions are *greedy* by default, which means that
in ambiguous situations they will match the longest string possible. The
*non-greedy* (also called *lazy*) version of the braces, which matches
the shortest string possible, has the closing brace followed by a
question mark.

Enter the following into the interactive shell, and notice the
difference between the greedy and non-greedy forms of the braces
searching the same string:

\>\>\> [greedyHaRegex = re.compile(r\'(Ha){3,5}\')]{.codestrong1}\
\>\>\> [mo1 = greedyHaRegex.search(\'HaHaHaHaHa\')]{.codestrong1}\
\>\>\> [mo1.group()]{.codestrong1}\
\'HaHaHaHaHa\'\
\
\>\>\> [nongreedyHaRegex = re.compile(r\'(Ha){3,5}?\')]{.codestrong1}\
\>\>\> [mo2 = nongreedyHaRegex.search(\'HaHaHaHaHa\')]{.codestrong1}\
\>\>\> [mo2.group()]{.codestrong1}\
\'HaHaHa\'

Note that the question mark can have two meanings in regular
expressions: declaring a non-greedy match or flagging an optional group.
These meanings are entirely unrelated.

### **The findall() Method** {#calibre_link-252 .h1}

In addition to the [search()]{.literal} method, [Regex]{.literal}
objects also have a [findall()]{.literal} method. While
[search()]{.literal} will return a [Match]{.literal} object of the
*first* matched text in the searched string, the [findall()]{.literal}
method will return the strings of *every* []{#calibre_link-836 {http:=""
www.idpf.org="" 2007="" ops}type="pagebreak"}match in the searched
string. To see how [search()]{.literal} returns a [Match]{.literal}
object only on the first instance of matching text, enter the following
into the interactive shell:

\>\>\> [phoneNumRegex =
re.compile(r\'\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d\')]{.codestrong1}\
\>\>\> [mo = phoneNumRegex.search(\'Cell: 415-555-9999 Work:
212-555-0000\')]{.codestrong1}\
\>\>\> [mo.group()]{.codestrong1}\
\'415-555-9999\'

On the other hand, [findall()]{.literal} will not return a
[Match]{.literal} object but a list of strings---*as long as there are
no groups in the regular expression*. Each string in the list is a piece
of the searched text that matched the regular expression. Enter the
following into the interactive shell:

\>\>\> [phoneNumRegex =
re.compile(r\'\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d\') \# has no
groups]{.codestrong1}\
\>\>\> [phoneNumRegex.findall(\'Cell: 415-555-9999 Work:
212-555-0000\')]{.codestrong1}\
\[\'415-555-9999\', \'212-555-0000\'\]

If there *are* groups in the regular expression, then
[findall()]{.literal} will return a list of tuples. Each tuple
represents a found match, and its items are the matched strings for each
group in the regex. To see [findall()]{.literal} in action, enter the
following into the interactive shell (notice that the regular expression
being compiled now has groups in parentheses):

\>\>\> [phoneNumRegex =
re.compile(r\'(\\d\\d\\d)-(\\d\\d\\d)-(\\d\\d\\d\\d)\') \# has
groups]{.codestrong1}\
\>\>\> [phoneNumRegex.findall(\'Cell: 415-555-9999 Work:
212-555-0000\')]{.codestrong1}\
\[(\'415\', \'555\', \'9999\'), (\'212\', \'555\', \'0000\')\]

To summarize what the [findall()]{.literal} method returns, remember the
following:

-   When called on a regex with no groups, such as
    [\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d]{.literal}, the method
    [findall()]{.literal} returns a list of string matches, such as
    [\[\'415-555-9999\', \'212-555-0000\'\]]{.literal}.
-   When called on a regex that has groups, such as
    [(\\d\\d\\d)-(\\d\\d\\d)-(\\d\\d\\d\\d)]{.literal}, the method
    [findall()]{.literal} returns a list of tuples of strings (one
    string for each group), such as [\[(\'415\', \'555\', \'9999\'),
    (\'212\', \'555\', \'0000\')\]]{.literal}.

### **Character Classes** {#calibre_link-253 .h1}

In the earlier phone number regex example, you learned that
[\\d]{.literal} could stand for any numeric digit. That is,
[\\d]{.literal} is shorthand for the regular expression
[(0\|1\|2\|3\|4\|5\|6\|7\|8\|9)]{.literal}. There are many such
*shorthand character classes*, as shown in [Table
7-1](#calibre_link-1144){.calibre6}.

[]{#calibre_link-773 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}**Table 7-1:** Shorthand Codes for Common Character
Classes

  **Shorthand character class**   **Represents**
  ------------------------------- --------------------------------------------------------------------------------------------------------
  [\\d]{.literal}                 Any numeric digit from 0 to 9.
  [\\D]{.literal}                 Any character that is *not* a numeric digit from 0 to 9.
  [\\w]{.literal}                 Any letter, numeric digit, or the underscore character. (Think of this as matching "word" characters.)
  [\\W]{.literal}                 Any character that is *not* a letter, numeric digit, or the underscore character.
  [\\s]{.literal}                 Any space, tab, or newline character. (Think of this as matching "space" characters.)
  [\\S]{.literal}                 Any character that is *not* a space, tab, or newline.

Character classes are nice for shortening regular expressions. The
character class [\[0-5\]]{.literal} will match only the numbers
[0]{.literal} to [5]{.literal}; this is much shorter than typing
[(0\|1\|2\|3\|4\|5)]{.literal}. Note that while [\\d]{.literal} matches
digits and [\\w]{.literal} matches digits, letters, and the underscore,
there is no shorthand character class that matches only letters. (Though
you can use the [\[a-zA-Z\]]{.literal} character class, as explained
next.)

For example, enter the following into the interactive shell:

\>\>\> [xmasRegex = re.compile(r\'\\d+\\s\\w+\')]{.codestrong1}\
\>\>\> [xmasRegex.findall(\'12 drummers, 11 pipers, 10 lords, 9 ladies,
8 maids, 7]{.codestrong1}\
[swans, 6 geese, 5 rings, 4 birds, 3 hens, 2 doves, 1
partridge\')]{.codestrong1}\
\[\'12 drummers\', \'11 pipers\', \'10 lords\', \'9 ladies\', \'8
maids\', \'7 swans\', \'6\
geese\', \'5 rings\', \'4 birds\', \'3 hens\', \'2 doves\', \'1
partridge\'\]

The regular expression [\\d+\\s\\w+]{.literal} will match text that has
one or more numeric digits ([\\d+]{.literal}), followed by a whitespace
character ([\\s]{.literal}), followed by one or more
letter/digit/underscore characters ([\\w+]{.literal}). The
[findall()]{.literal} method returns all matching strings of the regex
pattern in a list.

### **Making Your Own Character Classes** {#calibre_link-254 .h1}

There are times when you want to match a set of characters but the
shorthand character classes ([\\d]{.literal}, [\\w]{.literal},
[\\s]{.literal}, and so on) are too broad. You can define your own
character class using square brackets. For example, the character class
[\[aeiouAEIOU\]]{.literal} will match any vowel, both lowercase and
uppercase. Enter the following into the interactive shell:

\>\>\> [vowelRegex = re.compile(r\'\[aeiouAEIOU\]\')]{.codestrong1}\
\>\>\> [vowelRegex.findall(\'RoboCop eats baby food. BABY
FOOD.\')]{.codestrong1}\
\[\'o\', \'o\', \'o\', \'e\', \'a\', \'a\', \'o\', \'o\', \'A\', \'O\',
\'O\'\]

[]{#calibre_link-749 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}You can also include ranges of letters or numbers
by using a hyphen. For example, the character class
[\[a-zA-Z0-9\]]{.literal} will match all lowercase letters, uppercase
letters, and numbers.

Note that inside the square brackets, the normal regular expression
symbols are not interpreted as such. This means you do not need to
escape the [.]{.literal}, [\*]{.literal}, [?]{.literal}, or
[()]{.literal} characters with a preceding backslash. For example, the
character class [\[0-5.\]]{.literal} will match digits [0]{.literal} to
[5]{.literal} and a period. You do not need to write it as
[\[0-5\\.\]]{.literal}.

By placing a caret character ([\^]{.literal}) just after the character
class's opening bracket, you can make a *negative character class*. A
negative character class will match all the characters that are *not* in
the character class. For example, enter the following into the
interactive shell:

\>\>\> [consonantRegex =
re.compile(r\'\[\^aeiouAEIOU\]\')]{.codestrong1}\
\>\>\> [consonantRegex.findall(\'RoboCop eats baby food. BABY
FOOD.\')]{.codestrong1}\
\[\'R\', \'b\', \'C\', \'p\', \' \', \'t\', \'s\', \' \', \'b\', \'b\',
\'y\', \' \', \'f\', \'d\', \'.\', \'\
\', \'B\', \'B\', \'Y\', \' \', \'F\', \'D\', \'.\'\]

Now, instead of matching every vowel, we're matching every character
that isn't a vowel.

### **The Caret and Dollar Sign Characters** {#calibre_link-255 .h1}

You can also use the caret symbol ([\^]{.literal}) at the start of a
regex to indicate that a match must occur at the *beginning* of the
searched text. Likewise, you can put a dollar sign ([\$]{.literal}) at
the end of the regex to indicate the string must *end* with this regex
pattern. And you can use the [\^]{.literal} and [\$]{.literal} together
to indicate that the entire string must match the regex---that is, it's
not enough for a match to be made on some subset of the string.

For example, the [r\'\^Hello\']{.literal} regular expression string
matches strings that begin with [\'Hello\']{.literal}. Enter the
following into the interactive shell:

\>\>\> [beginsWithHello = re.compile(r\'\^Hello\')]{.codestrong1}\
\>\>\> [beginsWithHello.search(\'Hello, world!\')]{.codestrong1}\
\<re.Match object; span=(0, 5), match=\'Hello\'\>\
\>\>\> [beginsWithHello.search(\'He said hello.\') ==
None]{.codestrong1}\
True

The [r\'\\d\$\']{.literal} regular expression string matches strings
that end with a numeric character from 0 to 9. Enter the following into
the interactive shell:

\>\>\> [endsWithNumber = re.compile(r\'\\d\$\')]{.codestrong1}\
\>\>\> [endsWithNumber.search(\'Your number is 42\')]{.codestrong1}\
\<re.Match object; span=(16, 17), match=\'2\'\>\
\>\>\> [endsWithNumber.search(\'Your number is forty two.\') ==
None]{.codestrong1}\
True

[]{#calibre_link-755 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [r\'\^\\d+\$\']{.literal} regular expression
string matches strings that both begin and end with one or more numeric
characters. Enter the following into the interactive shell:

\>\>\> [wholeStringIsNum = re.compile(r\'\^\\d+\$\')]{.codestrong1}\
\>\>\> [wholeStringIsNum.search(\'1234567890\')]{.codestrong1}\
\<re.Match object; span=(0, 10), match=\'1234567890\'\>\
\>\>\> [wholeStringIsNum.search(\'12345xyz67890\') ==
None]{.codestrong1}\
True\
\>\>\> [wholeStringIsNum.search(\'12  34567890\') ==
None]{.codestrong1}\
True

The last two [search()]{.literal} calls in the previous interactive
shell example demonstrate how the entire string must match the regex if
[\^]{.literal} and [\$]{.literal} are used.

I always confuse the meanings of these two symbols, so I use the
mnemonic "Carrots cost dollars" to remind myself that the caret comes
first and the dollar sign comes last.

### **The Wildcard Character** {#calibre_link-256 .h1}

The [.]{.literal} (or *dot*) character in a regular expression is called
a *wildcard* and will match any character except for a newline. For
example, enter the following into the interactive shell:

\>\>\> [atRegex = re.compile(r\'.at\')]{.codestrong1}\
\>\>\> [atRegex.findall(\'The cat in the hat sat on the flat
mat.\')]{.codestrong1}\
\[\'cat\', \'hat\', \'sat\', \'lat\', \'mat\'\]

Remember that the dot character will match just one character, which is
why the match for the text [flat]{.literal} in the previous example
matched only [lat]{.literal}. To match an actual dot, escape the dot
with a backslash: [\\.]{.literal}.

#### ***Matching Everything with Dot-Star*** {#calibre_link-257 .h2}

Sometimes you will want to match everything and anything. For example,
say you want to match the string [\'First Name:\']{.literal}, followed
by any and all text, followed by [\'Last Name:\']{.literal}, and then
followed by anything again. You can use the dot-star ([.\*]{.literal})
to stand in for that "anything." Remember that the dot character means
"any single character except the newline," and the star character means
"zero or more of the preceding character."

Enter the following into the interactive shell:

\>\>\> [nameRegex = re.compile(r\'First Name: (.\*) Last Name:
(.\*)\')]{.codestrong1}\
\>\>\> [mo = nameRegex.search(\'First Name: Al Last Name:
Sweigart\')]{.codestrong1}\
\>\>\> [mo.group(1)]{.codestrong1}\
\'Al\'\
\>\>\> [mo.group(2)]{.codestrong1}\
\'Sweigart\'

[]{#calibre_link-747 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The dot-star uses *greedy* mode: It will always try
to match as much text as possible. To match any and all text in a
*non-greedy* fashion, use the dot, star, and question mark
([.\*?]{.literal}). Like with braces, the question mark tells Python to
match in a non-greedy way.

Enter the following into the interactive shell to see the difference
between the greedy and non-greedy versions:

\>\>\> [nongreedyRegex = re.compile(r\'\<.\*?\>\')]{.codestrong1}\
\>\>\> [mo = nongreedyRegex.search(\'\<To serve man\> for
dinner.\>\')]{.codestrong1}\
\>\>\> [mo.group()]{.codestrong1}\
\'\<To serve man\>\'\
\
\>\>\> [greedyRegex = re.compile(r\'\<.\*\>\')]{.codestrong1}\
\>\>\> [mo = greedyRegex.search(\'\<To serve man\> for
dinner.\>\')]{.codestrong1}\
\>\>\> [mo.group()]{.codestrong1}\
\'\<To serve man\> for dinner.\>\'

Both regexes roughly translate to "Match an opening angle bracket,
followed by anything, followed by a closing angle bracket." But the
string [\'\<To serve man\> for dinner.\>\']{.literal} has two possible
matches for the closing angle bracket. In the non-greedy version of the
regex, Python matches the shortest possible string: [\'\<To serve
man\>\']{.literal}. In the greedy version, Python matches the longest
possible string: [\'\<To serve man\> for dinner.\>\']{.literal}.

#### ***Matching Newlines with the Dot Character*** {#calibre_link-258 .h2}

The dot-star will match everything except a newline. By passing
[re.DOTALL]{.literal} as the second argument to
[re.compile()]{.literal}, you can make the dot character match *all*
characters, including the newline character.

Enter the following into the interactive shell:

\>\>\> [noNewlineRegex = re.compile(\'.\*\')]{.codestrong1}\
\>\>\> [noNewlineRegex.search(\'Serve the public trust.\\nProtect the
innocent.]{.codestrong1}\
[\\nUphold the law.\').group()]{.codestrong1}\
\'Serve the public trust.\'\
\
\
\>\>\> [newlineRegex = re.compile(\'.\*\', re.DOTALL)]{.codestrong1}\
\>\>\> [newlineRegex.search(\'Serve the public trust.\\nProtect the
innocent.]{.codestrong1}\
[\\nUphold the law.\').group()]{.codestrong1}\
\'Serve the public trust.\\nProtect the innocent.\\nUphold the law.\'

The regex [noNewlineRegex]{.literal}, which did not have
[re.DOTALL]{.literal} passed to the [re.compile()]{.literal} call that
created it, will match everything only up to the first newline
character, whereas [newlineRegex]{.literal}, which *did* have
[re.DOTALL]{.literal} passed to [re.compile()]{.literal}, matches
everything. This is why the [newlineRegex.search()]{.literal} call
matches the full string, including its newline characters.

### []{#calibre_link-831 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Review of Regex Symbols** {#calibre_link-259 .h1}

This chapter covered a lot of notation, so here's a quick review of what
you learned about basic regular expression syntax:

-   The [?]{.literal} matches zero or one of the preceding group.
-   The [\*]{.literal} matches zero or more of the preceding group.
-   The [+]{.literal} matches one or more of the preceding group.
-   The [{n}]{.literal} matches exactly *n* of the preceding group.
-   The [{n,}]{.literal} matches *n* or more of the preceding group.
-   The [{,m}]{.literal} matches 0 to *m* of the preceding group.
-   The [{n,m}]{.literal} matches at least *n* and at most *m* of the
    preceding group.
-   [{n,m}?]{.literal} or [\*?]{.literal} or [+?]{.literal} performs a
    non-greedy match of the preceding group.
-   [\^spam]{.literal} means the string must begin with *spam*.
-   [spam\$]{.literal} means the string must end with *spam*.
-   The [.]{.literal} matches any character, except newline characters.
-   [\\d]{.literal}, [\\w]{.literal}, and [\\s]{.literal} match a digit,
    word, or space character, respectively.
-   [\\D]{.literal}, [\\W]{.literal}, and [\\S]{.literal} match anything
    except a digit, word, or space character, respectively.
-   [\[abc\]]{.literal} matches any character between the brackets (such
    as *a*, *b*, or *c*).
-   [\[\^abc\]]{.literal} matches any character that isn't between the
    brackets.

### **Case-Insensitive Matching** {#calibre_link-260 .h1}

Normally, regular expressions match text with the exact casing you
specify. For example, the following regexes match completely different
strings:

\>\>\> [regex1 = re.compile(\'RoboCop\')]{.codestrong1}\
\>\>\> [regex2 = re.compile(\'ROBOCOP\')]{.codestrong1}\
\>\>\> [regex3 = re.compile(\'robOcop\')]{.codestrong1}\
\>\>\> [regex4 = re.compile(\'RobocOp\')]{.codestrong1}

But sometimes you care only about matching the letters without worrying
whether they're uppercase or lowercase. To make your regex
case-insensitive, you can pass [re.IGNORECASE]{.literal} or
[re.I]{.literal} as a second argument to [re.compile()]{.literal}. Enter
the following into the interactive shell:

\>\>\> [robocop = re.compile(r\'robocop\', re.I)]{.codestrong1}\
\>\>\> [robocop.search(\'RoboCop is part man, part machine, all
cop.\').group()]{.codestrong1}\
\'RoboCop\'\
\
\>\>\> [robocop.search(\'ROBOCOP protects the
innocent.\').group()]{.codestrong1}\
\'ROBOCOP\'\
\
\>\>\> [robocop.search(\'Al, why does your programming book talk about
robocop so much?\').group()]{.codestrong1}\
\'robocop\'

### []{#calibre_link-1078 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}**Substituting Strings with the sub() Method** {#calibre_link-261 .h1}

Regular expressions can not only find text patterns but can also
substitute new text in place of those patterns. The [sub()]{.literal}
method for [Regex]{.literal} objects is passed two arguments. The first
argument is a string to replace any matches. The second is the string
for the regular expression. The [sub()]{.literal} method returns a
string with the substitutions applied.

For example, enter the following into the interactive shell:

\>\>\> [namesRegex = re.compile(r\'Agent \\w+\')]{.codestrong1}\
\>\>\> [namesRegex.sub(\'CENSORED\', \'Agent Alice gave the secret
documents to Agent Bob.\')]{.codestrong1}\
\'CENSORED gave the secret documents to CENSORED.\'

Sometimes you may need to use the matched text itself as part of the
substitution. In the first argument to [sub()]{.literal}, you can type
[\\1]{.literal}, [\\2]{.literal}, [\\3]{.literal}, and so on, to mean
"Enter the text of group [1]{.literal}, [2]{.literal}, [3]{.literal},
and so on, in the substitution."

For example, say you want to censor the names of the secret agents by
showing just the first letters of their names. To do this, you could use
the regex [Agent (\\w)\\w\*]{.literal} and pass
[r\'\\1\*\*\*\*\']{.literal} as the first argument to [sub()]{.literal}.
The [\\1]{.literal} in that string will be replaced by whatever text was
matched by group [1]{.literal}---that is, the [(\\w)]{.literal} group of
the regular expression.

\>\>\> [agentNamesRegex = re.compile(r\'Agent
(\\w)\\w\*\')]{.codestrong1}\
\>\>\> [agentNamesRegex.sub(r\'\\1\*\*\*\*\', \'Agent Alice told Agent
Carol that Agent]{.codestrong1}\
[Eve knew Agent Bob was a double agent.\')]{.codestrong1}\
A\*\*\*\* told C\*\*\*\* that E\*\*\*\* knew B\*\*\*\* was a double
agent.\'

### **Managing Complex Regexes** {#calibre_link-262 .h1}

Regular expressions are fine if the text pattern you need to match is
simple. But matching complicated text patterns might require long,
convoluted regular expressions. You can mitigate this by telling the
[re.compile()]{.literal} function to ignore whitespace and comments
inside the regular expression string. This "verbose mode" can be enabled
by passing the variable [re.VERBOSE]{.literal} as the second argument to
[re.compile()]{.literal}.

Now instead of a hard-to-read regular expression like this:

phoneRegex =
re.compile(r\'((\\d{3}\|\\(\\d{3}\\))?(\\s\|-\|\\.)?\\d{3}(\\s\|-\|\\.)\\d{4}\
(\\s\*(ext\|x\|ext.)\\s\*\\d{2,5})?)\')

you can spread the regular expression over multiple lines with comments
like this:

phoneRegex = re.compile(r\'\'\'(\
    (\\d{3}\|\\(\\d{3}\\))?            # area code\
    (\\s\|-\|\\.)?                    # separator\
    \\d{3}                         # first 3 digits\
    (\\s\|-\|\\.)                     # separator\
    \\d{4}                         # last 4 digits\
[]{#calibre_link-817 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}    (\\s\*(ext\|x\|ext.)\\s\*\\d{2,5})?  #
extension\
    )\'\'\', re.VERBOSE)

Note how the previous example uses the triple-quote syntax
([\'\'\']{.literal}) to create a multiline string so that you can spread
the regular expression definition over many lines, making it much more
legible.

The comment rules inside the regular expression string are the same as
regular Python code: the [\#]{.literal} symbol and everything after it
to the end of the line are ignored. Also, the extra spaces inside the
multiline string for the regular expression are not considered part of
the text pattern to be matched. This lets you organize the regular
expression so it's easier to read.

### **Combining re.IGNORECASE, re.DOTALL, and re.VERBOSE** {#calibre_link-263 .h1}

What if you want to use [re.VERBOSE]{.literal} to write comments in your
regular expression but also want to use [re.IGNORECASE]{.literal} to
ignore capitalization? Unfortunately, the [re.compile()]{.literal}
function takes only a single value as its second argument. You can get
around this limitation by combining the [re.IGNORECASE]{.literal},
[re.DOTALL]{.literal}, and [re.VERBOSE]{.literal} variables using the
pipe character ([\|]{.literal}), which in this context is known as the
*bitwise or* operator.

So if you want a regular expression that's case-insensitive *and*
includes newlines to match the dot character, you would form your
[re.compile()]{.literal} call like this:

\>\>\> [someRegexValue = re.compile(\'foo\', re.IGNORECASE \|
re.DOTALL)]{.codestrong1}

Including all three options in the second argument will look like this:

\>\>\> [someRegexValue = re.compile(\'foo\', re.IGNORECASE \| re.DOTALL
\| re.VERBOSE)]{.codestrong1}

This syntax is a little old-fashioned and originates from early versions
of Python. The details of the bitwise operators are beyond the scope of
this book, but check out the resources at
*[https://nostarch.com/automatestuff2/](https://nostarch.com/automatestuff2/){.calibre6}*
for more information. You can also pass other options for the second
argument; they're uncommon, but you can read more about them in the
resources, too.

### **Project: Phone Number and Email Address Extractor** {#calibre_link-264 .h1}

Say you have the boring task of finding every phone number and email
address in a long web page or document. If you manually scroll through
the page, you might end up searching for a long time. But if you had a
program that could search the text in your clipboard for phone numbers
and email addresses, you could simply press [CTRL-]{.small}A to select
all the text, press [CTRL-]{.small}C to copy it to the clipboard, and
then run your program. It could replace the text on the clipboard with
just the phone numbers and email addresses it finds.

[]{#calibre_link-1770 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}Whenever you're tackling a new project, it can be
tempting to dive right into writing code. But more often than not, it's
best to take a step back and consider the bigger picture. I recommend
first drawing up a high-level plan for what your program needs to do.
Don't think about the actual code yet---you can worry about that later.
Right now, stick to broad strokes.

For example, your phone and email address extractor will need to do the
following:

1.  Get the text off the clipboard.
2.  Find all phone numbers and email addresses in the text.
3.  Paste them onto the clipboard.

Now you can start thinking about how this might work in code. The code
will need to do the following:

1.  Use the [pyperclip]{.literal} module to copy and paste strings.
2.  Create two regexes, one for matching phone numbers and the other for
    matching email addresses.
3.  Find all matches, not just the first match, of both regexes.
4.  Neatly format the matched strings into a single string to paste.
5.  Display some kind of message if no matches were found in the text.

This list is like a road map for the project. As you write the code, you
can focus on each of these steps separately. Each step is fairly
manageable and expressed in terms of things you already know how to do
in Python.

#### ***Step 1: Create a Regex for Phone Numbers*** {#calibre_link-265 .h2}

First, you have to create a regular expression to search for phone
numbers. Create a new file, enter the following, and save it as
*phoneAndEmail.py*:

#! python3\
\# phoneAndEmail.py - Finds phone numbers and email addresses on the
clipboard.\
\
import pyperclip, re\
\
phoneRegex = re.compile(r\'\'\'(\
    (\\d{3}\|\\(\\d{3}\\))?                # area code\
    (\\s\|-\|\\.)?                        # separator\
    (\\d{3})                           # first 3 digits\
    (\\s\|-\|\\.)                         # separator\
    (\\d{4})                           # last 4 digits\
    (\\s\*(ext\|x\|ext.)\\s\*(\\d{2,5}))?    # extension\
    )\'\'\', re.VERBOSE)\
\
\# TODO: Create email regex.\
\
\# TODO: Find matches in clipboard text.\
\
\# TODO: Copy results to the clipboard.

[]{#calibre_link-1771 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}The [TODO]{.literal} comments are just a skeleton
for the program. They'll be replaced as you write the actual code.

The phone number begins with an *optional* area code, so the area code
group is followed with a question mark. Since the area code can be just
three digits (that is, [\\d{3}]{.literal}) *or* three digits within
parentheses (that is, [\\(\\d{3}\\)]{.literal}), you should have a pipe
joining those parts. You can add the regex comment [\# Area
code]{.literal} to this part of the multiline string to help you
remember what [(\\d{3}\|\\(\\d{3}\\))?]{.literal} is supposed to match.

The phone number separator character can be a space ([\\s]{.literal}),
hyphen ([-]{.literal}), or period ([.]{.literal}), so these parts should
also be joined by pipes. The next few parts of the regular expression
are straightforward: three digits, followed by another separator,
followed by four digits. The last part is an optional extension made up
of any number of spaces followed by [ext]{.literal}, [x]{.literal}, or
[ext.]{.literal}, followed by two to five digits.

::: note
**[NOTE]{.notes}**

*It's easy to get mixed up with regular expressions that contain groups
with parentheses [( )]{.literal} and escaped parentheses [\\(
\\)]{.literal}. Remember to double-check that you're using the correct
one if you get a "missing ), unterminated subpattern" error message.*
:::

#### ***Step 2: Create a Regex for Email Addresses*** {#calibre_link-266 .h2}

You will also need a regular expression that can match email addresses.
Make your program look like the following:

#! python3\
\# phoneAndEmail.py - Finds phone numbers and email addresses on the
clipboard.\
\
import pyperclip, re\
\
phoneRegex = re.compile(r\'\'\'(\
\--[snip]{.codeitalic1}\--\
\
[\# Create email regex.]{.codestrong1}\
[emailRegex = re.compile(r\'\'\'(]{.codestrong1}\
[  ]{.codestrong1}[➊]{.ent} [\[a-zA-Z0-9.\_%+-\]+      #
username]{.codestrong1}\
[  ]{.codestrong1}[➋]{.ent} [@                      # @
symbol]{.codestrong1}\
[  ]{.codestrong1}[➌]{.ent} [\[a-zA-Z0-9.-\]+         # domain
name]{.codestrong1}\
[    (\\.\[a-zA-Z\]{2,4})       # dot-something]{.codestrong1}\
[    )\'\'\', re.VERBOSE)]{.codestrong1}\
\
\# TODO: Find matches in clipboard text.\
\
\# TODO: Copy results to the clipboard.

The username part of the email address [➊]{.ent} is one or more
characters that can be any of the following: lowercase and uppercase
letters, numbers, a dot, an underscore, a percent sign, a plus sign, or
a hyphen. You can put all of these into a character class:
[\[a-zA-Z0-9.\_%+-\]]{.literal}.

The domain and username are separated by an *@* symbol [➋]{.ent}. The
domain name [➌]{.ent} has a slightly less permissive character class
with only letters, numbers, periods, and hyphens:
[\[a-zA-Z0-9.-\]]{.literal}. And last will be []{#calibre_link-1088
{http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}the "dot-com"
part (technically known as the *top-level domain*), which can really be
dot-anything. This is between two and four characters.

The format for email addresses has a lot of weird rules. This regular
expression won't match every possible valid email address, but it'll
match almost any typical email address you'll encounter.

#### ***Step 3: Find All Matches in the Clipboard Text*** {#calibre_link-267 .h2}

Now that you have specified the regular expressions for phone numbers
and email addresses, you can let Python's [re]{.literal} module do the
hard work of finding all the matches on the clipboard. The
[pyperclip.paste()]{.literal} function will get a string value of the
text on the clipboard, and the [findall()]{.literal} regex method will
return a list of tuples.

Make your program look like the following:

   #! python3\
   # phoneAndEmail.py - Finds phone numbers and email addresses on the
clipboard.\
\
   import pyperclip, re\
\
   phoneRegex = re.compile(r\'\'\'(\
   \--[snip]{.codeitalic1}\--\
\
   [\# Find matches in clipboard text.]{.codestrong1}\
   [text = str(pyperclip.paste())]{.codestrong1}\
\
[➊]{.ent} [matches = \[\]]{.codestrong1}\
[➋]{.ent} [for groups in phoneRegex.findall(text):]{.codestrong1}\
[       phoneNum = \'-\'.join(\[groups\[1\], groups\[3\],
groups\[5\]\])]{.codestrong1}\
[       if groups\[8\] != \'\':]{.codestrong1}\
[           phoneNum += \' x\' + groups\[8\]]{.codestrong1}\
[       matches.append(phoneNum)]{.codestrong1}\
[➌]{.ent} [for groups in emailRegex.findall(text):]{.codestrong1}\
[       matches.append(groups\[0\])]{.codestrong1}\
\
   # TODO: Copy results to the clipboard.

There is one tuple for each match, and each tuple contains strings for
each group in the regular expression. Remember that group [0]{.literal}
matches the entire regular expression, so the group at index
[0]{.literal} of the tuple is the one you are interested in.

As you can see at [➊]{.ent}, you'll store the matches in a list variable
named [matches]{.literal}. It starts off as an empty list, and a couple
[for]{.literal} loops. For the email addresses, you append group
[0]{.literal} of each match [➌]{.ent}. For the matched phone numbers,
you don't want to just append group [0]{.literal}. While the program
*detects* phone numbers in several formats, you want the phone number
appended to be in a single, standard format. The [phoneNum]{.literal}
variable contains a string built from groups [1]{.literal},
[3]{.literal}, [5]{.literal}, and [8]{.literal} of the matched text
[➋]{.ent}. (These groups are the area code, first three digits, last
four digits, and extension.)

#### []{#calibre_link-1772 .calibre1 {http:="" www.idpf.org="" 2007="" ops}type="pagebreak"}***Step 4: Join the Matches into a String for the Clipboard*** {#calibre_link-268 .h2}

Now that you have the email addresses and phone numbers as a list of
strings in [matches]{.literal}, you want to put them on the clipboard.
The [pyperclip.copy()]{.literal} function takes only a single string
value, not a list of strings, so you call the [join()]{.literal} method
on [matches]{.literal}.

To make it easier to see that the program is working, let's print any
matches you find to the terminal. If no phone numbers or email addresses
were found, the program should tell the user this.

Make your program look like the following:

#! python3\
\# phoneAndEmail.py - Finds phone numbers and email addresses on the
clipboard.\
\
\--[snip]{.codeitalic1}\--\
for groups in emailRegex.findall(text):\
    matches.append(groups\[0\])\
\
[\# Copy results to the clipboard.]{.codestrong1}\
[if len(matches) \> 0:]{.codestrong1}\
[    pyperclip.copy(\'\\n\'.join(matches))]{.codestrong1}\
[    print(\'Copied to clipboard:\')]{.codestrong1}\
[    print(\'\\n\'.join(matches))]{.codestrong1}\
[else:]{.codestrong1}\
[    print(\'No phone numbers or email addresses
found.\')]{.codestrong1}

#### ***Running the Program*** {#calibre_link-269 .h2}

For an example, open your web browser to the No Starch Press contact
page at
*[https://nostarch.com/contactus/](https://nostarch.com/contactus/){.calibre6}*,
press [CTRL-]{.small}A to select all the text on the page, and press
[CTRL]{.small}-C to copy it to the clipboard. When you run this program,
the output will look something like this:

Copied to clipboard:\
800-420-7240\
415-863-9900\
415-863-9950\
info@nostarch.com\
media@nostarch.com\
academic@nostarch.com\
info@nostarch.com

#### ***Ideas for Similar Programs*** {#calibre_link-270 .h2}

Identifying patterns of text (and possibly substituting them with the
[sub()]{.literal} method) has many different potential applications. For
example, you could:

-   Find website URLs that begin with *http://* or *https://*.
-   Clean up dates in different date formats (such as 3/14/2019,
    03-14-2019, and 2015/3/19) by replacing them with dates in a single,
    standard format.
-   []{#calibre_link-1059 {http:="" www.idpf.org="" 2007=""
    ops}type="pagebreak"}Remove sensitive information such as Social
    Security or credit card numbers.
-   Find common typos such as multiple spaces between words,
    accidentally accidentally repeated words, or multiple exclamation
    marks at the end of sentences. Those are annoying!!

### **Summary** {#calibre_link-271 .h1}

While a computer can search for text quickly, it must be told precisely
what to look for. Regular expressions allow you to specify the pattern
of characters you are looking for, rather than the exact text itself. In
fact, some word processing and spreadsheet applications provide
find-and-replace features that allow you to search using regular
expressions.

The [re]{.literal} module that comes with Python lets you compile
[Regex]{.literal} objects. These objects have several methods:
[search()]{.literal} to find a single match, [findall()]{.literal} to
find all matching instances, and [sub()]{.literal} to do a
find-and-replace substitution of text.

You can find out more in the official Python documentation at
*[https://docs.python.org/3/library/re.html](https://docs.python.org/3/library/re.html){.calibre6}*.
Another useful resource is the tutorial website
*[https://www.regular-expressions.info/](https://www.regular-expressions.info/){.calibre6}*.

### **Practice Questions** {#calibre_link-272 .h1}

[1](#calibre_link-1145){#calibre_link-1323 .calibre6}. What is the
function that creates [Regex]{.literal} objects?

[2](#calibre_link-1146){#calibre_link-1324 .calibre6}. Why are raw
strings often used when creating [Regex]{.literal} objects?

[3](#calibre_link-1147){#calibre_link-1325 .calibre6}. What does the
[search()]{.literal} method return?

[4](#calibre_link-1148){#calibre_link-1326 .calibre6}. How do you get
the actual strings that match the pattern from a [Match]{.literal}
object?

[5](#calibre_link-1149){#calibre_link-1327 .calibre6}. In the regex
created from [r\'(\\d\\d\\d)-(\\d\\d\\d-\\d\\d\\d\\d)\']{.literal}, what
does group [0]{.literal} cover? Group [1]{.literal}? Group
[2]{.literal}?

[6](#calibre_link-1150){#calibre_link-1328 .calibre6}. Parentheses and
periods have specific meanings in regular expression syntax. How would
you specify that you want a regex to match actual parentheses and period
characters?

[7](#calibre_link-1151){#calibre_link-1329 .calibre6}. The
[findall()]{.literal} method returns a list of strings or a list of
tuples of strings. What makes it return one or the other?

[8](#calibre_link-1152){#calibre_link-1330 .calibre6}. What does the
[\|]{.literal} character signify in regular expressions?

[9](#calibre_link-1153){#calibre_link-1331 .calibre6}. What two things
does the [?]{.literal} character signify in regular expressions?

[10](#calibre_link-1154){#calibre_link-1332 .calibre6}. What is the
difference between the [+]{.literal} and [\*]{.literal} characters in
regular expressions?

[11](#calibre_link-1155){#calibre_link-1333 .calibre6}. What is the
difference between [{3}]{.literal} and [{3,5}]{.literal} in regular
expressions?

[12](#calibre_link-1156){#calibre_link-1334 .calibre6}. What do the
[\\d]{.literal}, [\\w]{.literal}, and [\\s]{.literal} shorthand
character classes signify in regular expressions?

[13](#calibre_link-1157){#calibre_link-1335 .calibre6}.
[]{#calibre_link-1773 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}What do the [\\D]{.literal}, [\\W]{.literal}, and
[\\S]{.literal} shorthand character classes signify in regular
expressions?

[14](#calibre_link-1158){#calibre_link-1336 .calibre6}. What is the
difference between [.\*]{.literal} and [.\*?]{.literal}?

[15](#calibre_link-1159){#calibre_link-1337 .calibre6}. What is the
character class syntax to match all numbers and lowercase letters?

[16](#calibre_link-1160){#calibre_link-1338 .calibre6}. How do you make
a regular expression case-insensitive?

[17](#calibre_link-1161){#calibre_link-1339 .calibre6}. What does the
[.]{.literal} character normally match? What does it match if
[re.DOTALL]{.literal} is passed as the second argument to
[re.compile()]{.literal}?

[18](#calibre_link-1162){#calibre_link-1340 .calibre6}. If [numRegex =
re.compile(r\'\\d+\')]{.literal}, what will [numRegex.sub(\'X\', \'12
drummers, 11 pipers, five rings, 3 hens\')]{.literal} return?

[19](#calibre_link-1163){#calibre_link-1341 .calibre6}. What does
passing [re.VERBOSE]{.literal} as the second argument to
[re.compile()]{.literal} allow you to do?

[20](#calibre_link-1164){#calibre_link-1342 .calibre6}. How would you
write a regex that matches a number with commas for every three digits?
It must match the following:

-   [\'42\']{.literal}
-   [\'1,234\']{.literal}
-   [\'6,368,745\']{.literal}

but not the following:

-   [\'12,34,567\']{.literal} (which has only two digits between the
    commas)
-   [\'1234\']{.literal} (which lacks commas)

[21](#calibre_link-1165){#calibre_link-1343 .calibre6}. How would you
write a regex that matches the full name of someone whose last name is
Watanabe? You can assume that the first name that comes before it will
always be one word that begins with a capital letter. The regex must
match the following:

-   [\'Haruto Watanabe\']{.literal}
-   [\'Alice Watanabe\']{.literal}
-   [\'RoboCop Watanabe\']{.literal}

but not the following:

-   [\'haruto Watanabe\']{.literal} (where the first name is not
    capitalized)
-   [\'Mr. Watanabe\']{.literal} (where the preceding word has a
    nonletter character)
-   [\'Watanabe\']{.literal} (which has no first name)
-   [\'Haruto watanabe\']{.literal} (where Watanabe is not capitalized)

[22](#calibre_link-1166){#calibre_link-1344 .calibre6}. How would you
write a regex that matches a sentence where the first word is either
*Alice*, *Bob*, or *Carol*; the second word is either *eats*, *pets*, or
*throws*; the third word is *apples*, *cats*, or *baseballs*; and the
sentence ends with a period? This regex should be case-insensitive. It
must match the following:

-   [\'Alice eats apples.\']{.literal}
-   [\'Bob pets cats.\']{.literal}
-   [\'Carol throws baseballs.\']{.literal}
-   [\'Alice throws Apples.\']{.literal}
-   [\'BOB EATS CATS.\']{.literal}

[]{#calibre_link-1774 {http:="" www.idpf.org="" 2007=""
ops}type="pagebreak"}but not the following:

-   [\'RoboCop eats apples.\']{.literal}
-   [\'ALICE THROWS FOOTBALLS.\']{.literal}
-   [\'Carol eats 7 cats.\']{.literal}

### **Practice Projects** {#calibre_link-273 .h1}

For practice, write programs to do the following tasks.

#### ***Date Detection*** {#calibre_link-274 .h2}

Write a regular expression that can detect dates in the *DD/MM/YYYY*
format. Assume that the days range from 01 to 31, the months range from
01 to 12, and the years range from 1000 to 2999. Note that if the day or
month is a single digit, it'll have a leading zero.

The regular expression doesn't have to detect correct days for each
month or for leap years; it will accept nonexistent dates like
31/02/2020 or 31/04/2021. Then store these strings into variables named
[month]{.literal}, [day]{.literal}, and [year]{.literal}, and write
additional code that can detect if it is a valid date. April, June,
September, and November have 30 days, February has 28 days, and the rest
of the months have 31 days. February has 29 days in leap years. Leap
years are every year evenly divisible by 4, except for years evenly
divisible by 100, unless the year is also evenly divisible by 400. Note
how this calculation makes it impossible to make a reasonably sized
regular expression that can detect a valid date.

#### ***Strong Password Detection*** {#calibre_link-275 .h2}

Write a function that uses regular expressions to make sure the password
string it is passed is strong. A strong password is defined as one that
is at least eight characters long, contains both uppercase and lowercase
characters, and has at least one digit. You may need to test the string
against multiple regex patterns to validate its strength.

#### ***Regex Version of the strip() Method*** {#calibre_link-276 .h2}

Write a function that takes a string and does the same thing as the
[strip()]{.literal} string method. If no other arguments are passed
other than the string to strip, then whitespace characters will be
removed from the beginning and end of the string. Otherwise, the
characters specified in the second argument to the function will be
removed from the string.
:::

<div>

[![](/images/prev.png)](../chapter6)[![](/images/toc.png)](/#toc)[![](/images/next.png)](../chapter8)

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
