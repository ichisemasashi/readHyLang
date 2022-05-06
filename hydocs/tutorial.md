# Tutorial

![Hy\'s mascot, Cuddles the
cuttlefish.](_static/cuddles-transparent-small.png)

This chapter provides a quick introduction to Hy. It assumes a basic
background in programming, but no specific prior knowledge of Python or
Lisp.

## Lisp-stick on a Python

Let\'s start with the classic:

    (print "Hy, world!")

This program calls the `print`{.interpreted-text role="func"} function,
which, like all of Python\'s
`built-in functions <py:built-in-funcs>`{.interpreted-text role="ref"},
is available in Hy.

All of Python\'s
`binary and unary operators <py:expressions>`{.interpreted-text
role="ref"} are available, too, although `==` is spelled `=` in
deference to Lisp tradition. Here\'s how we\'d use the addition operator
`+`:

    (+ 1 3)

This code returns `4`. It\'s equivalent to `1 + 3` in Python and many
other languages. Languages in the
[Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language))
family, including Hy, use a prefix syntax: `+`, just like `print` or
`sqrt`, appears before all of its arguments. The call is delimited by
parentheses, but the opening parenthesis appears before the operator
being called instead of after it, so instead of `sqrt(2)`, we write
`(sqrt 2)`. Multiple arguments, such as the two integers in `(+ 1 3)`,
are separated by whitespace. Many operators, including `+`, allow more
than two arguments: `(+ 1 2 3)` is equivalent to `1 + 2 + 3`.

Here\'s a more complex example:

    (- (* (+ 1 3 88) 2) 8)

This code returns `176`. Why? We can see the infix equivalent with the
command `echo "(- (* (+ 1 3 88) 2) 8)" | hy2py`, which returns the
Python code corresponding to the given Hy code, or by passing the
`--spy` option to Hy when starting the REPL, which shows the Python
equivalent of each input line before the result. The infix equivalent in
this case is:

``` python
((1 + 3 + 88) * 2) - 8
```

To evaluate this infix expression, you\'d of course evaluate the
innermost parenthesized expression first and work your way outwards. The
same goes for Lisp. Here\'s what we\'d get by evaluating the above Hy
code one step at a time:

    (- (* (+ 1 3 88) 2) 8)
    (- (* 92 2) 8)
    (- 184 8)
    176

The basic unit of Lisp syntax, which is similar to a C or Python
expression, is the **form**. `92`, `*`, and `(* 92 2)` are all forms. A
Lisp program consists of a sequence of forms nested within forms. Forms
are typically separated from each other by whitespace, but some forms,
such as string literals (`"Hy, world!"`), can contain whitespace
themselves. An **expression** is a form enclosed in parentheses; its
first child form, called the **head**, determines what the expression
does, and should generally be a function or macro. Functions are the
most ordinary sort of head, whereas macros (described in more detail
below) are functions executed at compile-time instead and return code to
be executed at run-time.

Comments start with a `;` character and continue till the end of the
line. A comment is functionally equivalent to whitespace. :

    (print (** 2 64))   ; Max 64-bit unsigned integer value

Although `#` isn\'t a comment character in Hy, a Hy program can begin
with a [shebang line](https://en.wikipedia.org/wiki/Shebang_(Unix)),
which Hy itself will ignore:

    #!/usr/bin/env hy
    (print "Make me executable, and run me!")

## Literals

Hy has `literal syntax <syntax>`{.interpreted-text role="ref"} for all
of the same data types that Python does. Here\'s an example of Hy code
for each type and the Python equivalent.

  Hy             Python           Type
  -------------- ---------------- -------------------------------------------
  `1`            `1`              `int`{.interpreted-text role="class"}
  `1.2`          `1.2`            `float`{.interpreted-text role="class"}
  `4j`           `4j`             `complex`{.interpreted-text role="class"}
  `True`         `True`           `bool`{.interpreted-text role="class"}
  `None`         `None`           `NoneType`
  `"hy"`         `'hy'`           `str`{.interpreted-text role="class"}
  `b"hy"`        `b'hy'`          `bytes`{.interpreted-text role="class"}
  `#(1 2 3)`     `(1, 2, 3)`      `tuple`{.interpreted-text role="class"}
  `[1 2 3]`      `[1, 2, 3]`      `list`{.interpreted-text role="class"}
  `#{1 2 3}`     `{1, 2, 3}`      `set`{.interpreted-text role="class"}
  `{1 2  3 4}`   `{1: 2, 3: 4}`   `dict`{.interpreted-text role="class"}

In addition, Hy has a Clojure-style literal syntax for
`fractions.Fraction`{.interpreted-text role="class"}: `1/3` is
equivalent to `fractions.Fraction(1, 3)`.

The Hy REPL prints output in Hy syntax by default, with the function
:hy`hy.repr`{.interpreted-text role="func"}:

    => [1 2 3]
    [1 2 3]

But if you start Hy like this:

    $ hy --repl-output-fn=repr

the REPL will use Python\'s native `repr` function instead, so you\'ll
see values in Python syntax:

    => [1 2 3]
    [1, 2, 3]

## Basic operations

Set variables with :hy`setv`{.interpreted-text role="func"}:

    (setv zone-plane 8)

Access the elements of a list, dictionary, or other data structure with
:hy`get <hy.pyops.get>`{.interpreted-text role="func"}:

    (setv fruit ["apple" "banana" "cantaloupe"])
    (print (get fruit 0))  ; => apple
    (setv (get fruit 1) "durian")
    (print (get fruit 1))  ; => durian

Access a range of elements in an ordered structure with
:hy`cut`{.interpreted-text role="func"}:

    (print (cut "abcdef" 1 4))  ; => bcd

Conditional logic can be built with :hy`if`{.interpreted-text
role="func"}:

    (if (= 1 1)
      (print "Math works. The universe is safe.")
      (print "Math has failed. The universe is doomed."))

As in this example, `if` is called like `(if CONDITION THEN ELSE)`. It
executes and returns the form `THEN` if `CONDITION` is true (according
to `bool`{.interpreted-text role="class"}) and `ELSE` otherwise. If
`ELSE` is omitted, `None` is used in its place.

What if you want to use more than form in place of the `THEN` or `ELSE`
clauses, or in place of `CONDITION`, for that matter? Use the macro
:hy`do`{.interpreted-text role="func"} (known more traditionally in Lisp
as `progn`), which combines several forms into one, returning the last:

    (if (do (print "Let's check.") (= 1 1))
      (do
        (print "Math works.")
        (print "The universe is safe."))
      (do
        (print "Math has failed.")
        (print "The universe is doomed.")))

For branching on more than one case, try
:hy`cond <hy.core.macros.cond>`{.interpreted-text role="func"}:

    (setv somevar 33)
    (cond
      (> somevar 50)
        (print "That variable is too big!")
      (< somevar 10)
        (print "That variable is too small!")
      True
        (print "That variable is jussssst right!"))

The macro `(when CONDITION THEN-1 THEN-2 …)` is shorthand for
`(if CONDITION (do THEN-1 THEN-2 …))`. `unless` works the same as
`when`, but inverts the condition with `not`.

Hy\'s basic loops are :hy`while`{.interpreted-text role="func"} and
:hy`for`{.interpreted-text role="func"}:

    (setv x 3)
    (while (> x 0)
      (print x)
      (setv x (- x 1)))  ; => 3 2 1

    (for [x [1 2 3]]
      (print x))         ; => 1 2 3

A more functional way to iterate is provided by the comprehension forms
such as :hy`lfor`{.interpreted-text role="func"}. Whereas `for` always
returns `None`, `lfor` returns a list with one element per iteration. :

    (print (lfor  x [1 2 3]  (* x 2)))  ; => [2, 4, 6]

## Functions, classes, and modules

Define named functions with :hy`defn`{.interpreted-text role="func"}:

    (defn fib [n]
      (if (< n 2)
        n
        (+ (fib (- n 1)) (fib (- n 2)))))
    (print (fib 8))  ; => 21

Define anonymous functions with :hy`fn`{.interpreted-text role="func"}:

    (print (list (filter (fn [x] (% x 2)) (range 10))))
      ; => [1, 3, 5, 7, 9]

Special symbols in the parameter list of `defn` or `fn` allow you to
indicate optional arguments, provide default values, and collect
unlisted arguments:

    (defn test [a b [c None] [d "x"] #* e]
      [a b c d e])
    (print (test 1 2))            ; => [1, 2, None, 'x', ()]
    (print (test 1 2 3 4 5 6 7))  ; => [1, 2, 3, 4, (5, 6, 7)]

Set a function parameter by name with a `:keyword`:

    (test 1 2 :d "y")             ; => [1, 2, None, 'y', ()]

Define classes with :hy`defclass`{.interpreted-text role="func"}:

    (defclass FooBar []
      (defn __init__ [self x]
        (setv self.x x))
      (defn get-x [self]
        self.x))

Here we create a new instance `fb` of `FooBar` and access its attributes
by various means:

    (setv fb (FooBar 15))
    (print fb.x)         ; => 15
    (print (. fb x))     ; => 15
    (print (.get-x fb))  ; => 15
    (print (fb.get-x))   ; => 15

Note that syntax like `fb.x` and `fb.get-x` only works when the object
being invoked (`fb`, in this case) is a simple variable name. To get an
attribute or call a method of an arbitrary form `FORM`, you must use the
syntax `(. FORM x)` or `(.get-x FORM)`.

Access an external module, whether written in Python or Hy, with
:hy`import`{.interpreted-text role="func"}:

    (import math)
    (print (math.sqrt 2))  ; => 1.4142135623730951

Python can import a Hy module like any other module so long as Hy itself
has been imported first, which, of course, must have already happened if
you\'re running a Hy program.

## Macros

Macros are the basic metaprogramming tool of Lisp. A macro is a function
that is called at compile time (i.e., when a Hy program is being
translated to Python `ast`{.interpreted-text role="mod"} objects) and
returns code, which becomes part of the final program. Here\'s a simple
example:

    (print "Executing")
    (defmacro m []
      (print "Now for a slow computation")
      (setv x (% (** 10 10 7) 3))
      (print "Done computing")
      x)
    (print "Value:" (m))
    (print "Done executing")

If you run this program twice in a row, you\'ll see this:

    $ hy example.hy
    Now for a slow computation
    Done computing
    Executing
    Value: 1
    Done executing
    $ hy example.hy
    Executing
    Value: 1
    Done executing

The slow computation is performed while compiling the program on its
first invocation. Only after the whole program is compiled does normal
execution begin from the top, printing \"Executing\". When the program
is called a second time, it is run from the previously compiled
bytecode, which is equivalent to simply:

    (print "Executing")
    (print "Value:" 1)
    (print "Done executing")

Our macro `m` has an especially simple return value, an integer, which
at compile-time is converted to an integer literal. In general, macros
can return arbitrary Hy forms to be executed as code. There are several
special operators and macros that make it easy to construct forms
programmatically, such as :hy`quote`{.interpreted-text role="func"}
(`'`), :hy`quasiquote`{.interpreted-text role="func"} (``[),
:hy:func:\`unquote]{.title-ref} (`~`), and
:hy`defmacro! <hyrule.macrotools.defmacro!>`{.interpreted-text
role="func"}. The previous chapter has
`a simple example <do-while>`{.interpreted-text role="ref"} of using ``[
and ]{.title-ref}[\~]{.title-ref}[ to define a new control construct
]{.title-ref}[do-while]{.title-ref}\`.

What if you want to use a macro that\'s defined in a different module?
`import` won\'t help, because it merely translates to a Python `import`
statement that\'s executed at run-time, and macros are expanded at
compile-time, that is, during the translation from Hy to Python.
Instead, use :hy`require <require>`{.interpreted-text role="func"},
which imports the module and makes macros available at compile-time.
`require` uses the same syntax as `import`. :

    => (require tutorial.macros)
    => (tutorial.macros.rev (1 2 3 +))
    6

Hy also supports reader macros, which are similar to ordinary macros,
but operate on raw source text rather than pre-parsed Hy forms. They can
choose how much of the source code to consume after the point they are
called, and return any code. Thus, reader macros can add entirely new
syntax to Hy. For example, you could add a literal notation for
Python\'s `decimal.Decimal`{.interpreted-text role="class"} class like
so:

    => (import  decimal [Decimal]  fractions [Fraction])
    => (defreader d
    ...   (.slurp-space &reader)
    ...   `(Decimal ~(.read-ident &reader)))
    => (print (repr #d .1))
    Decimal('0.1')
    => (print (Fraction #d .1))
    1/10
    => ;; Contrast with the normal floating-point .1:
    => (print (Fraction .1))
    3602879701896397/36028797018963968

`require` can pull in a reader macro defined in a different module with
syntax like `(require mymodule :readers [d])`.

## Hyrule

[Hyrule](https://pypi.org/project/hyrule) is Hy\'s standard utility
library. It provides a variety of functions and macros that are useful
for writing Hy programs. :

    => (import hyrule [inc])
    => (list (map inc [1 2 3]))
    [2 3 4]
    => (require hyrule [assoc])
    => (setv d {})
    => (assoc d  "a" 1  "b" 2)
    => d
    {"a" 1  "b" 2}

## Next steps

You now know enough to be dangerous with Hy. You may now smile
villainously and sneak off to your Hydeaway to do unspeakable things.

Refer to Python\'s documentation for the details of Python semantics,
and the rest of this manual for Hy-specific features. Like Hy itself,
the manual is incomplete, but
`contributions <hacking>`{.interpreted-text role="ref"} are always
welcome.