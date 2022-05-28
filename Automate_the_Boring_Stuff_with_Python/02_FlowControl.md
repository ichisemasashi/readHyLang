## **フロー制御**


![Image](../images/000137.jpg)


というわけで、個々の命令の基本はわかったし、プログラムは命令の羅列に過ぎないこともわかったと思います。しかし、プログラミングの本当の強さは、週末の用事リストのように次々と命令を実行することだけではありません。 プログラムは、式の評価に基づいて、命令をスキップしたり、繰り返したり、複数の命令の中から実行する命令を選んだりすることができます。 実際、プログラムが最初の行から始まって、すべての行を最後まで単純に実行することはほとんどないでしょう。*フロー制御文*は、どの条件下でどのPython命令を実行するかを決定することができます。

これらのフロー制御文は、フローチャートの記号に直接対応しているので、この章で取り上げるコードのフローチャート版を提供します。[図2-1](#calibre_link-1533)は、雨が降っているときにどうするかというフローチャートです。StartからEndまで、矢印の作る道をたどってください。


![image](../images/000039.jpg)


*図2-1: 雨が降っているときの対処法を伝えるフローチャート*。

フローチャートでは、通常、始まりから終わりまで複数の方法があります。コンピュータ・プログラムのコード行も同様である。 フローチャートでは、これらの分岐点を菱形で表し、その他のステップを長方形で表す。開始ステップと終了ステップは、丸みを帯びた長方形で表現される。

しかし、フロー制御文について学ぶ前に、まずこれらの*yes*と*no*のオプションの表現方法を学び、それらの分岐点をPythonのコードとしてどのように書くかを理解する必要があります。そのために、ブール値、比較演算子、ブール演算子について調べてみましょう。

### **ブール値** 

整数、浮動小数点、文字列のデータ型は取り得る値の数に制限がないのに対し、*Boolean*データ型は2つの値しか持ちません。`True` と `False` です。(Booleanは数学者George Booleにちなんで大文字で表記されています)。Pythonのコードとして入力する場合、ブール値の `True` と `False` は文字列を囲む引用符がなく、常に大文字の *T* または *F* で始まり、残りの単語は小文字になります。対話型シェルに次のように入力します．(これらの命令のいくつかは意図的に間違っており，エラーメッセージが表示されます)．

```
>>> spam = True
>>> spam
   True
[➋] >>> true
   Traceback (most recent call last):
     File "<pyshell#2>", line 1, in <module>
       true
   NameError: name 'true' is not defined
[➌] >>> True = 2 + 2
   SyntaxError: can't assign to keyword
```

他の値と同様に、ブール値も式の中で使用され、変数 [➊] に格納することができます。もし適切な大文字小文字を使わなかったり [➋] 、変数名に `True` や `False` を使おうとすると [➌] Python はあなたにエラーメッセージを出します。

### **比較演算子** 

*比較演算子*は、*関係演算子*とも呼ばれ、2つの値を比較し、1つのブール値として評価します。[表2-1](#calibre_link-1534)は、比較演算子をリストアップしています。

**表 2-1: ** 比較演算子

 **演算子** **意味**
  ----------------- --------------------------
  `==`    等しい
  `!=`    イコールではない
  `<`    未満
  `>`    より大きい
  `<=`   以下
  `>=`   以上

これらの演算子は与えられた値によって `True` または `False` に評価されます。それでは、 `==` と `!=` から始めて、いくつかの演算子を試してみましょう。

```
>>> 42 == 42
True
>>> 42 == 99
False
>>> 2 != 3
True
>>> 2 != 2
False
```

ご想像のとおり、 `==` (equal to) は両者の値が同じであれば `True` と評価され、 `!=` (not equal to) は両者の値が異なる場合に `True` と評価されます。演算子 `==` と `!=` は、実際にはどのようなデータ型の値に対しても機能します。

```
   >>> 'hello' == 'hello'
   True
   >>> 'hello' == 'Hello'
   False
   >>> 'dog' != 'cat'
   True
   >>> True == True
   True
   >>> True != False
   True
   >>> 42 == 42.0
   True
[➊] >>> 42 == '42'
   False
```

整数や浮動小数点数の値は、常に文字列の値とは等しくないことに注意してください。42 == '42'` [➊] という式は `False` と評価されますが、これは Python が整数の `42` を文字列 `'42'` と異なるものとみなしているからです。

一方、 `<`, `>`, `<=`, `>=` 演算子は、整数と浮動小数点数に対してのみ正しく働きます。

```
   >>> 42 < 100
   True
   >>> 42 > 100
   False
   >>> 42 < 42
   False
   >>> eggCount = 42
[➊] >>> eggCount <= 42
   True
   >>> myAge = 29
[➋] >>> myAge >= 10
   True
```


**==と=の違い**

 `==`演算子(equal to)は等号が2つあるのに対し、`=`演算子(assign)は1つであることにお気づきでしょうか。この2つの演算子を混同してしまうのは簡単なことです。ただ、以下の点を覚えておいてください。

- `==`演算子(equal to)は、2つの値が互いに同じかどうかを尋ねます。
- 演算子 `=` (代入) は、右側の値を左側の変数に代入します。

`==`演算子は2文字で、`!=`演算子は2文字で構成されることに注意してください。


`eggCount <= 42` [➊] と `myAge >= 10` [➋] の例のように、変数の値を他の値と比較するために比較演算子をよく使うでしょう。 (結局のところ、コード中に `'dog' != 'cat'` と入力する代わりに、 `True` と入力すればよかったのです)。この例は、後でフロー制御文について学習するときに、もっとたくさん見ることになります。

### **論理演算子** 

3 つの論理演算子 (`and`、`or`、`not`) は、論理値を比較するために使用されます。比較演算子のように、これらの演算子は式を評価し、ブール値に変換します。 これらの演算子について、まずは `and` 演算子から詳しく見ていきましょう。

#### ***二項論理演算子*** 

演算子 `and` と `or` は常に 2 つのブール値（または式）を取るので、*二項演算子* とみなされます。`and` 演算子は論理値の両方が `True` ならば式を `True` と評価し、そうでないなら `False` と評価します。そうでない場合は `False` と評価されます。 `and` を使ったいくつかの式をインタラクティブシェルに入力して、その動きを見てみましょう。

```
>>> True and True
True
>>> True and False
False
```

*真理値表*は、論理演算子のすべての可能な結果を示しています。 [表2-2](#calibre_link-1535)は、`and`演算子の真理値表です。

**表2-2:** `and`演算子の真理値表

  **Expression**                **Evaluates to . . .**
  ----------------------------- ------------------------
  `True and True`     `True`
  `True and False`    `False`
  `False and True`    `False`
  `False and False`   `False`

一方、`or` 演算子は、2つのブール値の*どちらか*が `True` であれば、その式を `True` と評価します。もし両方が `False` ならば、`False` と評価されます。

```
>>> False or True
True
>>> False or False
False
```

[表2-3](#calibre_link-1536)に示すように、`or`演算子のすべての可能な結果を真理値表で確認することができます。

**表2-3:** `or`演算子の真理値表

  **Expression**               **Evaluates to . . .**
  ---------------------------- ------------------------
  `True or True`     `True`
  `True or False`    `True`
  `False or True`    `True`
  `False or False`   `False`

#### ***not演算子*** 

`AND` や `OR` とは異なり、 `not` 演算子は 1 つのブール値 (または式) に対してのみ作用します。そのため、この演算子は単項演算子です。`not` 演算子は、単純に反対のブール値として評価されます。

```
   >>> not True
   False
[➊] >>> not not not not True
   True
```

会話や文章で二重否定を使うのと同じように、`not` 演算子 [➊] を入れ子にすることができますが、実際のプログラムでこれを行う理由は決してありません。[表2-4](#calibre_link-1537)は`not`の真理値表です。

**表2-4:** `not` 演算子の真理値表

  **Expression**          **Evaluates to . . .**
  ----------------------- ------------------------
  `not True`    `False`
  `not False`   `True`

### **ブール演算子と比較演算子の混在** 

比較演算子はブール値で評価されるので、ブール演算子とともに式の中で使用することができます。

演算子 `and`, `or`, `not` は常に `True` と `False` というブール値で動作するので、ブール演算子と呼ばれることを思い出してください。`4 < 5` のような式はブール値ではありませんが、ブール値として評価される式です。対話型シェルに比較演算子を使ったブール演算式をいくつか入力してみてください。

```
>>> (4 < 5) and (5 < 6)
True
>>> (4 < 5) and (9 < 6)
False
>>> (1 == 2) or (2 == 2)
True
```

コンピュータは、まず左の式を評価し、次に右の式を評価します。それぞれのブール値がわかったら、式全体を評価し、一つのブール値にします。`(4 < 5) and (5 < 6)`に対するコンピュータの評価プロセスは、次のように考えることができます。


![image](../images/000095.jpg)


また、比較演算子とともに、複数のブール演算子を式の中で使用することができます。

```
>>> 2 + 2 == 4 and not 2 + 2 == 5 and 2 * 2 == 2 + 2
True
```

論理演算子には、数学の演算子と同じように操作の順序があります。演算子や比較演算子が評価された後、Pythonはまず `not` 演算子を評価し、次に `and` 演算子、そして `or` 演算子を評価します。

### **Elements of Flow Control** 

Flow control statements often start with a part called the *condition* and are always followed by a block of code called the *clause*. Before you learn about Python's specific flow control statements, I'll cover what a condition and a block are.

#### ***Conditions*** 

The Boolean expressions you've seen so far could all be considered conditions, which are the same thing as expressions; *condition* is just a more specific name in the context of flow control statements.  Conditions always evaluate down to a Boolean value, `True` or `False`. A flow control statement decides what to do based on whether its condition is `True` or `False`, and almost every flow control statement uses a condition.

#### ***Blocks of Code*** 

Lines of Python code can be grouped together in *blocks*. You can tell when a block begins and ends from the indentation of the lines of code.  There are three rules for blocks.

-   Blocks begin when the indentation increases.
-   Blocks can contain other blocks.
-   Blocks end when the indentation decreases to zero or to a containing block's indentation.

Blocks are easier to understand by looking at some indented code, so let's find the blocks in part of a small game program, shown here:

```
  name = 'Mary'
  password = 'swordfish'
  if name == 'Mary':
    [➊] print('Hello, Mary')
       if password == 'swordfish':
        [➋] print('Access granted.')
       else:
        [➌] print('Wrong password.')
```

You can view the execution of this program at *[https://autbor.com/blocks/](https://autbor.com/blocks/)*.  The first block of code [➊] starts at the line `print('Hello, Mary')` and contains all the lines after it. Inside this block is another block [➋], which has only a single line in it: `print('Access Granted.')`. The third block [➌] is also one line long: `print('Wrong password.')`.

### **Program Execution** 

In the previous chapter's *hello.py* program, Python started executing instructions at the top of the program going down, one after another.  The *program execution* (or simply, *execution*) is a term for the current instruction being executed. If you print the source code on paper and put your finger on each line as it is executed, you can think of your finger as the program execution.

Not all programs execute by simply going straight down, however. If you use your finger to trace through a program with flow control statements, you'll likely find yourself jumping around the source code based on conditions, and you'll probably skip entire clauses.

### **Flow Control Statements** 

Now, let's explore the most important piece of flow control: the statements themselves. The statements represent the diamonds you saw in the flowchart in [Figure 2-1](#calibre_link-1533), and they are the actual decisions your programs will make.

#### ***if Statements*** 

The most common type of flow control statement is the `if` statement. An `if` statement's clause (that is, the block following the `if` statement) will execute if the statement's condition is `True`. The clause is skipped if the condition is `False`.

In plain English, an `if` statement could be read as, "If this condition is true, execute the code in the clause." In Python, an `if` statement consists of the following:

-   The `if` keyword
-   A condition (that is, an expression that evaluates to `True` or `False`)
-   A colon
-   Starting on the next line, an indented block of code (called the `if` clause)

For example, let's say you have some code that checks to see whether someone's name is Alice. (Pretend `name` was assigned some value earlier.)

```
if name == 'Alice':
    print('Hi, Alice.')
```

All flow control statements end with a colon and are followed by a new block of code (the clause). This `if` statement's clause is the block with `print('Hi, Alice.')`. [Figure 2-2](#calibre_link-1538) shows what a flowchart of this code would look like.


![image](../images/000147.jpg)


*Figure 2-2: The flowchart for an `if` statement*

#### ***else Statements*** 

An `if` clause can optionally be followed by an `else` statement. The `else` clause is executed only when the `if` statement's condition is `False`. In plain English, an `else` statement could be read as, "If this condition is true, execute this code. Or else, execute that code." An `else` statement doesn't have a condition, and in code, an `else` statement always consists of the following:

-   The `else` keyword
-   A colon
-   Starting on the next line, an indented block of code (called the `else` clause)

Returning to the Alice example, let's look at some code that uses an `else` statement to offer a different greeting if the person's name isn't Alice.

```
if name == 'Alice':
    print('Hi, Alice.')
else:
    print('Hello, stranger.')
```

[Figure 2-3](#calibre_link-1539) shows what a flowchart of this code would look like.


![image](../images/000118.jpg)


*Figure 2-3: The flowchart for an `else` statement*

#### ***elif Statements*** 

While only one of the `if` or `else` clauses will execute, you may have a case where you want one of *many* possible clauses to execute. The `elif` statement is an "else if" statement that always follows an `if` or another `elif` statement. It provides another condition that is checked only if all of the previous conditions were `False`.  In code, an `elif` statement always consists of the following:

-   The `elif` keyword
-   A condition (that is, an expression that evaluates to `True` or `False`)
-   A colon
-   Starting on the next line, an indented block of code (called the `elif` clause)

Let's add an `elif` to the name checker to see this statement in action.

```
if name == 'Alice':
    print('Hi, Alice.')
elif age < 12:
    print('You are not Alice, kiddo.')
```

This time, you check the person's age, and the program will tell them something different if they're younger than 12. You can see the flowchart for this in [Figure 2-4](#calibre_link-1540).


![image](../images/000145.jpg)


*Figure 2-4: The flowchart for an `elif` statement*

The `elif` clause executes if `age < 12` is `True` and `name == 'Alice'` is `False`.  However, if both of the conditions are `False`, then both of the clauses are skipped. It is *not* guaranteed that at least one of the clauses will be executed. When there is a chain of `elif` statements, only one or none of the clauses will be executed. Once one of the statements' conditions is found to be `True`, the rest of the `elif` clauses are automatically skipped. For example, open a new file editor window and enter the following code, saving it as *vampire.py*:

```
name = 'Carol'
age = 3000
if name == 'Alice':
    print('Hi, Alice.')
elif age < 12:
    print('You are not Alice, kiddo.')
elif age > 2000:
    print('Unlike you, Alice is not an undead, immortal vampire.')
elif age > 100:
    print('You are not Alice, grannie.')
```

You can view the execution of this program at *[https://autbor.com/vampire/](https://autbor.com/vampire/)*.  Here, I've added two more `elif` statements to make the name checker greet a person with different answers based on `age`.  [Figure 2-5](#calibre_link-1541) shows the flowchart for this.


![image](../images/000080.jpg)


*Figure 2-5: The flowchart for multiple `elif` statements in the* vampire.py *program*

The order of the `elif` statements does matter, however. Let's rearrange them to introduce a bug. Remember that the rest of the `elif` clauses are automatically skipped once a `True` condition has been found, so if you swap around some of the clauses in *vampire.py*, you run into a problem. Change the code to look like the following, and save it as *vampire2.py*:

```
   name = 'Carol'
   age = 3000
   if name == 'Alice':
       print('Hi, Alice.')
   elif age < 12:
       print('You are not Alice, kiddo.')
[➊] elif age > 100:
       print('You are not Alice, grannie.')
   elif age > 2000:
       print('Unlike you, Alice is not an undead, immortal vampire.')
```

You can view the execution of this program at *[https://autbor.com/vampire2/](https://autbor.com/vampire2/)*.  Say the `age` variable contains the value `3000` before this code is executed. You might expect the code to print the string `'Unlike you, Alice is not an undead, immortal vampire.'`. However, because the `age > 100` condition is `True` (after all, 3,000 *is* greater than 100) [➊], the string `'You are not Alice, grannie.'` is printed, and the rest of the `elif` statements are automatically skipped. Remember that at most only one of the clauses will be executed, and for `elif` statements, the order matters!

[Figure 2-6](#calibre_link-1542) shows the flowchart for the previous code. Notice how the diamonds for `age > 100` and `age > 2000` are swapped.

Optionally, you can have an `else` statement after the last `elif` statement. In that case, it *is* guaranteed that at least one (and only one) of the clauses will be executed. If the conditions in every `if` and `elif` statement are `False`, then the `else` clause is executed. For example, let's re-create the Alice program to use `if`, `elif`, and `else` clauses.

```
name = 'Carol'
age = 3000
if name == 'Alice':
    print('Hi, Alice.')
elif age < 12:
    print('You are not Alice, kiddo.')
else:
    print('You are neither Alice nor a little kid.')
```

You can view the execution of this program at *[https://autbor.com/littlekid/](https://autbor.com/littlekid/)*.  [Figure 2-7](#calibre_link-1543) shows the flowchart for this new code, which we'll save as *littleKid.py*.

In plain English, this type of flow control structure would be "If the first condition is true, do this. Else, if the second condition is true, do that. Otherwise, do something else." When you use `if`, `elif`, and `else` statements together, remember these rules about how to order them to avoid bugs like the one in [Figure 2-6](#calibre_link-1542). First, there is always exactly one `if` statement. Any `elif` statements you need should follow the `if` statement. Second, if you want to be sure that at least one clause is executed, close the structure with an `else` statement.


![image](../images/000136.jpg)


*Figure 2-6: The flowchart for the* vampire2.py *program. The X path will logically never happen, because if `age` were greater than `2000`, it would have already been greater than `100`.*


![image](../images/000032.jpg)


*Figure 2-7: Flowchart for the previous* littleKid.py *program*

#### ***while Loop Statements*** 

You can make a block of code execute over and over again using a `while` statement. The code in a `while` clause will be executed as long as the `while` statement's condition is `True`. In code, a `while` statement always consists of the following:

-   The `while` keyword
-   A condition (that is, an expression that evaluates to `True` or `False`)
-   A colon
-   Starting on the next line, an indented block of code (called the `while` clause)

You can see that a `while` statement looks similar to an `if` statement. The difference is in how they behave. At the end of an `if` clause, the program execution continues after the `if` statement. But at the end of a `while` clause, the program execution jumps back to the start of the `while` statement. The `while` clause is often called the *while loop* or just the *loop*.

Let's look at an `if` statement and a `while` loop that use the same condition and take the same actions based on that condition. Here is the code with an `if` statement:

```
spam = 0
if spam < 5:
    print('Hello, world.')
    spam = spam + 1
```

Here is the code with a `while` statement:

```
spam = 0
while spam < 5:
    print('Hello, world.')
    spam = spam + 1
```

These statements are similar---both `if` and `while` check the value of `spam`, and if it's less than 5, they print a message. But when you run these two code snippets, something very different happens for each one. For the `if` statement, the output is simply `"Hello, world."`. But for the `while` statement, it's `"Hello, world."` repeated five times! Take a look at the flowcharts for these two pieces of code, [Figures 2-8](#calibre_link-1544) and [2-9](#calibre_link-1545), to see why this happens.


![image](../images/000072.jpg)


*Figure 2-8: The flowchart for the `if` statement code*


![image](../images/000112.jpg)


*Figure 2-9: The flowchart for the `while` statement code*

The code with the `if` statement checks the condition, and it prints `Hello, world.` only once if that condition is true.  The code with the `while` loop, on the other hand, will print it five times. The loop stops after five prints because the integer in `spam` increases by one at the end of each loop iteration, which means that the loop will execute five times before `spam < 5` is `False`.

In the `while` loop, the condition is always checked at the start of each *iteration* (that is, each time the loop is executed). If the condition is `True`, then the clause is executed, and afterward, the condition is checked again. The first time the condition is found to be `False`, the `while` clause is skipped.

##### **An Annoying while Loop** 

Here's a small example program that will keep asking you to type, literally, `your name`. Select **File** ▸ **New** to open a new file editor window, enter the following code, and save the file as *yourName.py*:

```
[➊] name = ''
[➋] while name != 'your name':
       print('Please type your name.')
    [➌] name = input()
[➍] print('Thank you!')
```

You can view the execution of this program at *[https://autbor.com/yourname/](https://autbor.com/yourname/)*.  First, the program sets the `name` variable [➊] to an empty string. This is so that the `name != 'your name'` condition will evaluate to `True` and the program execution will enter the `while` loop's clause [➋].

The code inside this clause asks the user to type their name, which is assigned to the `name` variable [➌]. Since this is the last line of the block, the execution moves back to the start of the `while` loop and reevaluates the condition. If the value in `name` is *not equal* to the string `'your name'`, then the condition is `True`, and the execution enters the `while` clause again.

But once the user types `your name`, the condition of the `while` loop will be `'your name' != 'your name'`, which evaluates to `False`. The condition is now `False`, and instead of the program execution reentering the `while` loop's clause, Python skips past it and continues running the rest of the program [➍]. [Figure 2-10](#calibre_link-1546) shows a flowchart for the *yourName.py* program.


![image](../images/000042.jpg)


*Figure 2-10: A flowchart of the* yourName.py *program*

Now, let's see *yourName.py* in action. Press **F5** to run it, and enter something other than `your name` a few times before you give the program what it wants.

```
Please type your name.
[Al]
Please type your name.
[Albert]
Please type your name.
[%#@#%*(^&!!!]
Please type your name.
[your name]
Thank you!
```

If you never enter `your name`, then the `while` loop's condition will never be `False`, and the program will just keep asking forever. Here, the `input()` call lets the user enter the right string to make the program move on. In other programs, the condition might never actually change, and that can be a problem. Let's look at how you can break out of a `while` loop.

#### ***break Statements*** 

There is a shortcut to getting the program execution to break out of a `while` loop's clause early. If the execution reaches a `break` statement, it immediately exits the `while` loop's clause. In code, a `break` statement simply contains the `break` keyword.

Pretty simple, right? Here's a program that does the same thing as the previous program, but it uses a `break` statement to escape the loop. Enter the following code, and save the file as *yourName2.py*:

```
[➊] while True:
       print('Please type your name.')
    [➋] name = input()
    [➌] if name == 'your name':
        [➍] break
[➎] print('Thank you!')
```

You can view the execution of this program at *[https://autbor.com/yourname2/](https://autbor.com/yourname2/)*.  The first line [➊] creates an *infinite loop*; it is a `while` loop whose condition is always `True`. (The expression `True`, after all, always evaluates down to the value `True`.) After the program execution enters this loop, it will exit the loop only when a `break` statement is executed. (An infinite loop that *never* exits is a common programming bug.)

Just like before, this program asks the user to enter `your name` [➋]. Now, however, while the execution is still inside the `while` loop, an `if` statement checks [➌] whether `name` is equal to `'your name'`. If this condition is `True`, the `break` statement is run [➍], and the execution moves out of the loop to `print('Thank you!')` [➎].  Otherwise, the `if` statement's clause that contains the `break` statement is skipped, which puts the execution at the end of the `while` loop. At this point, the program execution jumps back to the start of the `while` statement [➊] to recheck the condition. Since this condition is merely the `True` Boolean value, the execution enters the loop to ask the user to type `your name` again. See [Figure 2-11](#calibre_link-1547) for this program's flowchart.

Run *yourName2.py*, and enter the same text you entered for *yourName.py*. The rewritten program should respond in the same way as the original.


![image](../images/000134.jpg)


*Figure 2-11: The flowchart for the* yourName2.py *program with an infinite loop. Note that the X path will logically never happen, because the loop condition is always `True`.*

#### ***continue Statements*** 

Like `break` statements, `continue` statements are used inside loops. When the program execution reaches a `continue` statement, the program execution immediately jumps back to the start of the loop and reevaluates the loop's condition.  (This is also what happens when the execution reaches the end of the loop.)

Let's use `continue` to write a program that asks for a name and password. Enter the following code into a new file editor window and save the program as *swordfish.py*.


**TRAPPED IN AN INFINITE LOOP?**

If you ever run a program that has a bug causing it to get stuck in an infinite loop, press \[CTRL\]-C or select **Shell** ▸ **Restart Shell** from IDLE's menu. This will send a `KeyboardInterrupt` error to your program and cause it to stop immediately. Try stopping a program by creating a simple infinite loop in the file editor, and save the program as *infiniteLoop.py*.

```
while True:
    print('Hello, world!')
```

When you run this program, it will print `Hello, world!` to the screen forever because the `while` statement's condition is always `True`. \[CTRL\]-C is also handy if you want to simply terminate your program immediately, even if it's not stuck in an infinite loop.


```
  while True:
      print('Who are you?')
      name = input()
    [➊] if name != 'Joe':
        [➋] continue
       print('Hello, Joe. What is the password? (It is a fish.)')
    [➌] password = input()
       if password == 'swordfish':
        [➍] break
[➎] print('Access granted.')    
```

If the user enters any name besides `Joe` [➊], the `continue` statement [➋] causes the program execution to jump back to the start of the loop. When the program reevaluates the condition, the execution will always enter the loop, since the condition is simply the value `True`. Once the user makes it past that `if` statement, they are asked for a password [➌]. If the password entered is `swordfish`, then the `break` statement [➍] is run, and the execution jumps out of the `while` loop to print `Access granted` [➎]. Otherwise, the execution continues to the end of the `while` loop, where it then jumps back to the start of the loop. See [Figure 2-12](#calibre_link-1548) for this program's flowchart.


![image](../images/000078.jpg)


*Figure 2-12: A flowchart for* swordfish.py. *The X path will logically never happen, because the loop condition is always `True`.*


**"TRUTHY" AND "FALSEY" VALUES**

Conditions will consider some values in other data types equivalent to `True` and `False`. When used in conditions, `0`, `0.0`, and `''` (the empty string) are considered `False`, while all other values are considered `True`. For example, look at the following program:

```
name = ''
[➊] while not name:
    print('Enter your name:')
    name = input()
print('How many guests will you have?')
numOfGuests = int(input())
[➋] if numOfGuests:
    [➌] print('Be sure to have enough room for all your guests.')
print('Done')
```

You can view the execution of this program at *[https://autbor.com/howmanyguests/](https://autbor.com/howmanyguests/)*.  If the user enters a blank string for `name`, then the `while` statement's condition will be `True` [➊], and the program continues to ask for a name. If the value for `numOfGuests` is not `0` [➋], then the condition is considered to be `True`, and the program will print a reminder for the user [➌].

You could have entered `not name != ''` instead of `not name`, and `numOfGuests != 0` instead of `numOfGuests`, but using the truthy and falsey values can make your code easier to read.


Run this program and give it some input. Until you claim to be Joe, the program shouldn't ask for a password, and once you enter the correct password, it should exit.

```
Who are you?
[I'm fine, thanks. Who are you?]
Who are you?
[Joe]
Hello, Joe. What is the password? (It is a fish.)
[Mary]
Who are you?
[Joe]
Hello, Joe. What is the password? (It is a fish.)
[swordfish]
Access granted.
```

You can view the execution of this program at *[https://autbor.com/hellojoe/](https://autbor.com/hellojoe/)*.

#### ***for Loops and the range() Function*** 

The `while` loop keeps looping while its condition is `True` (which is the reason for its name), but what if you want to execute a block of code only a certain number of times? You can do this with a `for` loop statement and the `range()` function.

In code, a `for` statement looks something like `for i in range(5):` and includes the following:

-   The `for` keyword
-   A variable name
-   The `in` keyword
-   A call to the `range()` method with up to three integers passed to it
-   A colon
-   Starting on the next line, an indented block of code (called the `for` clause)

Let's create a new program called *fiveTimes.py* to help you see a `for` loop in action.

```
print('My name is')
for i in range(5):
    print('Jimmy Five Times (' + str(i) + ')')
```

You can view the execution of this program at *[https://autbor.com/fivetimesfor/](https://autbor.com/fivetimesfor/)*.  The code in the `for` loop's clause is run five times. The first time it is run, the variable `i` is set to `0`. The `print()` call in the clause will print `Jimmy Five Times (0)`. After Python finishes an iteration through all the code inside the `for` loop's clause, the execution goes back to the top of the loop, and the `for` statement increments `i` by one. This is why `range(5)` results in five iterations through the clause, with `i` being set to `0`, then `1`, then `2`, then `3`, and then `4`. The variable `i` will go up to, but will not include, the integer passed to `range()`. [Figure 2-13](#calibre_link-1549) shows a flowchart for the *fiveTimes.py* program.

When you run this program, it should print `Jimmy Five Times` followed by the value of `i` five times before leaving the `for` loop.

```
My name is
Jimmy Five Times (0)
Jimmy Five Times (1)
Jimmy Five Times (2)
Jimmy Five Times (3)
Jimmy Five Times (4)
```


**[NOTE]**

*You can use `break` and `continue` statements inside `for` loops as well. The `continue` statement will continue to the next value of the `for` loop's counter, as if the program execution had reached the end of the loop and returned to the start. In fact, you can use `continue` and `break` statements only inside `while` and `for` loops. If you try to use these statements elsewhere, Python will give you an error.*



![image](../images/000027.jpg)


*Figure 2-13: The flowchart for* fiveTimes.py

As another `for` loop example, consider this story about the mathematician Carl Friedrich Gauss. When Gauss was a boy, a teacher wanted to give the class some busywork. The teacher told them to add up all the numbers from 0 to 100. Young Gauss came up with a clever trick to figure out the answer in a few seconds, but you can write a Python program with a `for` loop to do this calculation for you.

```
[➊] total = 0
[➋] for num in range(101):
    [➌] total = total + num
[➍] print(total)  
```

The result should be 5,050. When the program first starts, the `total` variable is set to `0` [➊]. The `for` loop [➋] then executes `total = total + num` [➌] 100 times. By the time the loop has finished all of its 100 iterations, every integer from `0` to `100` will have been added to `total`. At this point, `total` is printed to the screen [➍]. Even on the slowest computers, this program takes less than a second to complete.

(Young Gauss figured out a way to solve the problem in seconds. There are 50 pairs of numbers that add up to 101: 1 + 100, 2 + 99, 3 + 98, and so on, until 50 + 51. Since 50 × 101 is 5,050, the sum of all the numbers from 0 to 100 is 5,050. Clever kid!)

##### **An Equivalent while Loop** 

You can actually use a `while` loop to do the same thing as a `for` loop; `for` loops are just more concise. Let's rewrite *fiveTimes.py* to use a `while` loop equivalent of a `for` loop.

```
print('My name is')
i = 0
while i < 5:
    print('Jimmy Five Times (' + str(i) + ')')
    i = i + 1
```

You can view the execution of this program at *[https://autbor.com/fivetimeswhile/](https://autbor.com/fivetimeswhile/)*.  If you run this program, the output should look the same as the *fiveTimes.py* program, which uses a `for` loop.

##### **The Starting, Stopping, and Stepping Arguments to range()** 

Some functions can be called with multiple arguments separated by a comma, and `range()` is one of them. This lets you change the integer passed to `range()` to follow any sequence of integers, including starting at a number other than zero.

```
for i in range(12, 16):
    print(i)
```

The first argument will be where the `for` loop's variable starts, and the second argument will be up to, but not including, the number to stop at.

```
12
13
14
15
```

The `range()` function can also be called with three arguments. The first two arguments will be the start and stop values, and the third will be the *step argument*. The step is the amount that the variable is increased by after each iteration.

```
for i in range(0, 10, 2):
    print(i)
```

So calling `range(0, 10, 2)` will count from zero to eight by intervals of two.

```
0
2
4
6
8
```

The `range()` function is flexible in the sequence of numbers it produces for `for` loops. *For* example (I never apologize for my puns), you can even use a negative number for the step argument to make the `for` loop count down instead of up.

```
for i in range(5, -1, -1):
    print(i)
```

This `for` loop would have the following output:

```
5
4
3
2
1
0
```

Running a `for` loop to print `i` with `range(5, -1, -1)` should print from five down to zero.

### **Importing Modules** 

All Python programs can call a basic set of functions called *built-in functions*, including the `print()`, `input()`, and `len()` functions you've seen before. Python also comes with a set of modules called the *standard library*. Each module is a Python program that contains a related group of functions that can be embedded in your programs. For example, the `math` module has mathematics-related functions, the `random` module has random number-related functions, and so on.

Before you can use the functions in a module, you must import the module with an `import` statement. In code, an `import` statement consists of the following:

-   The `import` keyword
-   The name of the module
-   Optionally, more module names, as long as they are separated by commas

Once you import a module, you can use all the cool functions of that module. Let's give it a try with the `random` module, which will give us access to the `random.randint()` function.

Enter this code into the file editor, and save it as *printRandom.py*:

```
import random
for i in range(5):
    print(random.randint(1, 10))
```


**DON'T OVERWRITE MODULE NAMES**

When you save your Python scripts, take care not to give them a name that is used by one of Python's modules, such as *random.py*, *sys.py*, *os.py*, or *math.py*. If you accidentally name one of your programs, say, *random.py*, and use an `import random` statement in another program, your program would import your *random.py* file instead of Python's `random` module. This can lead to errors such as `AttributeError: module 'random' has no attribute 'randint'`, since your *random.py* doesn't have the functions that the real `random` module has. Don't use the names of any built-in Python functions either, such as `print()` or `input()`.

Problems like these are uncommon, but can be tricky to solve. As you gain more programming experience, you'll become more aware of the standard names used by Python's modules and functions, and will run into these problems less frequently.


When you run this program, the output will look something like this:

```
4
1
8
4
1
```

You can view the execution of this program at *[https://autbor.com/printrandom/](https://autbor.com/printrandom/)*.  The `random.randint()` function call evaluates to a random integer value between the two integers that you pass it. Since `randint()` is in the `random` module, you must first type **random.** in front of the function name to tell Python to look for this function inside the `random` module.

Here's an example of an `import` statement that imports four different modules:

import random, sys, os, math

Now we can use any of the functions in these four modules. We'll learn more about them later in the book.

#### ***from import Statements*** 

An alternative form of the `import` statement is composed of the `from` keyword, followed by the module name, the `import` keyword, and a star; for example, `from random import *`.

With this form of `import` statement, calls to functions in `random` will not need the `random.` prefix.  However, using the full name makes for more readable code, so it is better to use the `import random` form of the statement.

### **Ending a Program Early with the sys.exit() Function** 

The last flow control concept to cover is how to terminate the program.  Programs always terminate if the program execution reaches the bottom of the instructions. However, you can cause the program to terminate, or exit, before the last instruction by calling the `sys.exit()` function. Since this function is in the `sys` module, you have to import `sys` before your program can use it.

Open a file editor window and enter the following code, saving it as *exitExample.py*:

```
import sys

while True:
    print('Type exit to exit.')
    response = input()
    if response == 'exit':
        sys.exit()
    print('You typed ' + response + '.')
```

Run this program in IDLE. This program has an infinite loop with no `break` statement inside. The only way this program will end is if the execution reaches the `sys.exit()` call. When `response` is equal to `exit`, the line containing the `sys.exit()` call is executed. Since the `response` variable is set by the `input()` function, the user must enter `exit` in order to stop the program.

### **A Short Program: Guess the Number** 

The examples I've shown you so far are useful for introducing basic concepts, but now let's see how everything you've learned comes together in a more complete program. In this section, I'll show you a simple "guess the number" game. When you run this program, the output will look something like this:

```
I am thinking of a number between 1 and 20.
Take a guess.
[10]
Your guess is too low.
Take a guess.
[15]
Your guess is too low.
Take a guess.
[17]
Your guess is too high.
Take a guess.
[16]
Good job! You guessed my number in 4 guesses!
```

Enter the following source code into the file editor, and save the file as *guessTheNumber.py*:

```
# This is a guess the number game.
import random
secretNumber = random.randint(1, 20)
print('I am thinking of a number between 1 and 20.')

# Ask the player to guess 6 times.
for guessesTaken in range(1, 7):
    print('Take a guess.')
    guess = int(input())

    if guess < secretNumber:
        print('Your guess is too low.')
    elif guess > secretNumber:
        print('Your guess is too high.')
    else:
        break    # This condition is the correct guess!

if guess == secretNumber:
    print('Good job! You guessed my number in ' + str(guessesTaken) + ' guesses!')
else:
    print('Nope. The number I was thinking of was ' +
str(secretNumber))
```

You can view the execution of this program at *[https://autbor.com/guessthenumber/](https://autbor.com/guessthenumber/)*.  Let's look at this code line by line, starting at the top.

```
# This is a guess the number game.
import random
secretNumber = random.randint(1, 20)
```

First, a comment at the top of the code explains what the program does.  Then, the program imports the `random` module so that it can use the `random.randint()` function to generate a number for the user to guess. The return value, a random integer between 1 and 20, is stored in the variable `secretNumber`.

```
print('I am thinking of a number between 1 and 20.')

# Ask the player to guess 6 times.
for guessesTaken in range(1, 7):
    print('Take a guess.')
    guess = int(input())
```

The program tells the player that it has come up with a secret number and will give the player six chances to guess it. The code that lets the player enter a guess and checks that guess is in a `for` loop that will loop at most six times. The first thing that happens in the loop is that the player types in a guess. Since `input()` returns a string, its return value is passed straight into `int()`, which translates the string into an integer value. This gets stored in a variable named `guess`.

```
if guess < secretNumber:
    print('Your guess is too low.')
elif guess > secretNumber:
    print('Your guess is too high.')
```

These few lines of code check to see whether the guess is less than or greater than the secret number. In either case, a hint is printed to the screen.

```
else:
    break    # This condition is the correct guess!
```

If the guess is neither higher nor lower than the secret number, then it must be equal to the secret number---in which case, you want the program execution to break out of the `for` loop.

```
if guess == secretNumber:
    print('Good job! You guessed my number in ' + str(guessesTaken) + ' guesses!')
else:
    print('Nope. The number I was thinking of was ' +
str(secretNumber))
```

After the `for` loop, the previous `if...else` statement checks whether the player has correctly guessed the number and then prints an appropriate message to the screen. In both cases, the program displays a variable that contains an integer value (`guessesTaken` and `secretNumber`). Since it must concatenate these integer values to strings, it passes these variables to the `str()` function, which returns the string value form of these integers. Now these strings can be concatenated with the `+` operators before finally being passed to the `print()` function call.

### **A Short Program: Rock, Paper, Scissors** 

Let's use the programming concepts we've learned so far to create a simple rock, paper, scissors game. The output will look like this:

```
ROCK, PAPER, SCISSORS
0 Wins, 0 Losses, 0 Ties
Enter your move: (r)ock (p)aper (s)cissors or (q)uit
[p]
PAPER versus\...
PAPER
It is a tie!
0 Wins, 1 Losses, 1 Ties
Enter your move: (r)ock (p)aper (s)cissors or (q)uit
[s]
SCISSORS versus\...
PAPER
You win!
1 Wins, 1 Losses, 1 Ties
Enter your move: (r)ock (p)aper (s)cissors or (q)uit
[q]
```

Type the following source code into the file editor, and save the file as *rpsGame.py*:

```
import random, sys

print('ROCK, PAPER, SCISSORS')

# These variables keep track of the number of wins, losses, and ties.
wins = 0
losses = 0
ties = 0

while True: \# The main game loop.
    print('%s Wins, %s Losses, %s Ties' % (wins, losses, ties))
    while True: \# The player input loop.
        print('Enter your move: (r)ock (p)aper (s)cissors or (q)uit')
        playerMove = input()
        if playerMove == 'q':
            sys.exit() \# Quit the program.
        if playerMove == 'r' or playerMove == 'p' or playerMove == 's':
            break \# Break out of the player input loop.
        print('Type one of r, p, s, or q.')

    # Display what the player chose:
    if playerMove == 'r':
        print('ROCK versus\...')
    elif playerMove == 'p':
        print('PAPER versus\...')
    elif playerMove == 's':
        print('SCISSORS versus\...')

    # Display what the computer chose:
    randomNumber = random.randint(1, 3)
    if randomNumber == 1:
        computerMove = 'r'
        print('ROCK')
    elif randomNumber == 2:
        computerMove = 'p'
        print('PAPER')
    elif randomNumber == 3:
        computerMove = 's'
        print('SCISSORS')

    # Display and record the win/loss/tie:
    if playerMove == computerMove:
        print('It is a tie!')
        ties = ties + 1
    elif playerMove == 'r' and computerMove == 's':
        print('You win!')
        wins = wins + 1
    elif playerMove == 'p' and computerMove == 'r':
        print('You win!')
        wins = wins + 1
    elif playerMove == 's' and computerMove == 'p':
        print('You win!')
        wins = wins + 1
    elif playerMove == 'r' and computerMove == 'p':
        print('You lose!')
        losses = losses + 1
    elif playerMove == 'p' and computerMove == 's':
        print('You lose!')
        losses = losses + 1
    elif playerMove == 's' and computerMove == 'r':
        print('You lose!')
        losses = losses + 1
```

Let's look at this code line by line, starting at the top.

```
import random, sys

print('ROCK, PAPER, SCISSORS')

# These variables keep track of the number of wins, losses, and ties.
wins = 0
losses = 0
ties = 0
```

First, we import the `random` and `sys` module so that our program can call the `random.randint()` and `sys.exit()` functions. We also set up three variables to keep track of how many wins, losses, and ties the player has had.

```
while True: # The main game loop.
    print('%s Wins, %s Losses, %s Ties' % (wins, losses, ties))
    while True: \# The player input loop.
        print('Enter your move: (r)ock (p)aper (s)cissors or (q)uit')
        playerMove = input()
        if playerMove == 'q':
            sys.exit() \# Quit the program.
        if playerMove == 'r' or playerMove == 'p' or playerMove == 's':
            break \# Break out of the player input loop.
        print('Type one of r, p, s, or q.')
```

This program uses a `while` loop inside of another `while` loop. The first loop is the main game loop, and a single game of rock, paper, scissors is played on each iteration through this loop. The second loop asks for input from the player, and keeps looping until the player has entered an `r`, `p`, `s`, or `q` for their move. The `r`, `p`, and `s` correspond to rock, paper, and scissors, respectively, while the `q` means the player intends to quit. In that case, `sys.exit()` is called and the program exits. If the player has entered `r`, `p`, or `s`, the execution breaks out of the loop. Otherwise, the program reminds the player to enter `r`, `p`, `s`, or `q` and goes back to the start of the loop.

```
# Display what the player chose:
if playerMove == 'r':
    print('ROCK versus\...')
elif playerMove == 'p':
    print('PAPER versus\...')
elif playerMove == 's':
    print('SCISSORS versus\...')
```

The player's move is displayed on the screen.

```
# Display what the computer chose:
randomNumber = random.randint(1, 3)
if randomNumber == 1:
    computerMove = 'r'
    print('ROCK')
elif randomNumber == 2:
    computerMove = 'p'
    print('PAPER')
elif randomNumber == 3:
    computerMove = 's'
    print('SCISSORS')
```

Next, the computer's move is randomly selected. Since `random.randint()` can only return a random number, the `1`, `2`, or `3` integer value it returns is stored in a variable named `randomNumber`. The program stores a `'r'`, `'p'`, or `'s'` string in `computerMove` based on the integer in `randomNumber`, as well as displays the computer's move.

```
# Display and record the win/loss/tie:
if playerMove == computerMove:
    print('It is a tie!')
    ties = ties + 1
elif playerMove == 'r' and computerMove == 's':
    print('You win!')
    wins = wins + 1
elif playerMove == 'p' and computerMove == 'r':
    print('You win!')
    wins = wins + 1
elif playerMove == 's' and computerMove == 'p':
    print('You win!')
    wins = wins + 1
elif playerMove == 'r' and computerMove == 'p':
    print('You lose!')
    losses = losses + 1
elif playerMove == 'p' and computerMove == 's':
    print('You lose!')
    losses = losses + 1
elif playerMove == 's' and computerMove == 'r':
    print('You lose!')
    losses = losses + 1
```

Finally, the program compares the strings in `playerMove` and `computerMove`, and displays the results on the screen. It also increments the `wins`, `losses`, or `ties` variable appropriately. Once the execution reaches the end, it jumps back to the start of the main program loop to begin another game.

### **Summary** 

By using expressions that evaluate to `True` or `False` (also called conditions), you can write programs that make decisions on what code to execute and what code to skip. You can also execute code over and over again in a loop while a certain condition evaluates to `True`. The `break` and `continue` statements are useful if you need to exit a loop or jump back to the loop's start.

These flow control statements will let you write more intelligent programs. You can also use another type of flow control by writing your own functions, which is the topic of the next chapter.

### **Practice Questions** 

[1](#calibre_link-1550). What are the two values of the Boolean data type? How do you write them?

[2](#calibre_link-1551). What are the three Boolean operators?

[3](#calibre_link-1552). Write out the truth tables of each Boolean operator (that is, every possible combination of Boolean values for the operator and what they evaluate to).

[4](#calibre_link-1553). What do the following expressions evaluate to?

```
(5 > 4) and (3 == 5)
not (5 > 4)
(5 > 4) or (3 == 5)
not ((5 > 4) or (3 == 5))
(True and True) and (True == False)
(not False) or (not True)
```

[5](#calibre_link-1554). What are the six comparison operators?

[6](#calibre_link-1555). What is the difference between the equal to operator and the assignment operator?

[7](#calibre_link-1556). Explain what a condition is and where you would use one.

[8](#calibre_link-1557). Identify the three blocks in this code:

```
spam = 0
if spam == 10:
    print('eggs')
    if spam > 5:
        print('bacon')
    else:
        print('ham')
    print('spam')
print('spam')
```

[9](#calibre_link-1558). Write code that
```
prints [Hello] if [1] is stored in [spam],
prints [Howdy] if [2] is stored in [spam],
and prints [Greetings!] if anything else is stored in
[spam].
```

[10](#calibre_link-1559). What keys can you press if your program is stuck in an infinite loop?

[11](#calibre_link-1560). What is the difference between `break` and `continue`?

[12](#calibre_link-1561). What is the difference between `range(10)`, `range(0, 10)`, and `range(0, 10, 1)` in a `for` loop?

[13](#calibre_link-1562). Write a short program that prints the numbers `1` to `10` using a `for` loop. Then write an equivalent program that prints the numbers `1` to `10` using a `while` loop.

[14](#calibre_link-1563). If you had a function named `bacon()` inside a module named `spam`, how would you call it after importing `spam`?

**Extra credit:** Look up the `round()` and `abs()` functions on the internet, and find out what they do. Experiment with them in the interactive shell.


