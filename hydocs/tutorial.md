# チュートリアル

![Hy's mascot, Cuddles the cuttlefish.](_static/cuddles-transparent-small.png)

この章では、Hyの簡単な紹介をします。プログラミングの基本的な背景を想定していますが、PythonやLispの特別な予備知識はありません。

## PythonにLispを刺す

まずは定番から。

    (print "Hy, world!")

このプログラムは `print` を呼び出すもので、Hy で利用できます。

Pythonの`二項演算子、単項演算子` もすべて利用できますが、Lispの伝統にしたがって `==` は `=` と綴られます。以下は、加算演算子 `+` の使い方です。

    (+ 1 3)

このコードは `4` を返します。これはPythonや他の多くの言語では `1 + 3` と等価です。Hyを含む [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)) 系の言語はプリフィックス構文を使用します。`print` や `sqrt` と同じように、 `+` はすべての引数の前に置かれます。呼び出しは括弧で区切られますが、開始括弧は呼び出される演算子の後ではなく前に現れます。したがって、 `sqrt(2)` の代わりに `(sqrt 2)` と書きます。複数の引数、例えば `(+ 1 3)` の 2 つの整数は、空白で区切られます。多くの演算子は、 `+` を含めて、2つ以上の引数をとることができます。(+ 1 2 3)` は `1 + 2 + 3` と同じです。

もっと複雑な例を挙げます。

    (- (* (+ 1 3 88) 2) 8)

このコードは `176` を返します。なぜでしょうか？コマンド `echo "(- (* (+ 1 3 88) 2) 8)"| hy2py`  を実行すると、与えられたHyコードに対応するPythonコードが返され、これで等価なinfixを見ることができます。また、REPLを起動するときにHyに `--spy` オプションを渡すと、結果の前にそれぞれの入力行のPython等価物が表示されます。この場合のinfix相当は

```
((1 + 3 + 88) * 2) - 8
```

このinfix式を評価するには、当然ながら、一番内側の括弧で囲まれた式を最初に評価し、その後、外側に向かって評価することになる。Lispの場合も同じです。上のHyのコードを一歩ずつ評価すると、次のようになる。

    (- (* (+ 1 3 88) 2) 8)
    (- (* 92 2) 8)
    (- 184 8)
    176

CやPythonの式に似たLispの構文の基本単位は**form**です。92`, `*`, そして `(* 92 2)` はすべてフォームです。Lispのプログラムは、フォームの中にフォームがネストされたシーケンスで構成されています。フォームは通常空白で区切られますが、文字列リテラル (`"Hy, world!"`) のように、フォーム自体が空白を含むものもあります。式** は括弧で囲まれたフォームです。その最初の子フォームは **head** と呼ばれ、式が何を行うかを決定し、一般的には関数かマクロであるべきです。関数は最も一般的なヘッドであり、マクロ（詳細は後述）はコンパイル時に実行される関数で、ランタイムに実行されるコードを返します。

コメントは `;` 文字で始まり、行末まで続きます。コメントは、機能的には空白文字と同じです。

    (print (** 2 64))   ; Max 64-bit unsigned integer value

Hyでは `#` はコメント文字ではありませんが、Hyのプログラムは [shebang line](https://en.wikipedia.org/wiki/Shebang_(Unix)) で始めることができ、Hy自身はこれを無視します。

    #!/usr/bin/env hy
    (print "Make me executable, and run me!")

## リテラル

HyはPythonと同じすべてのデータ型に対して`リテラル構文`を持っています。以下はそれぞれの型に対するHyのコードの例と、Pythonのそれに相当するものです。

  Hy             Python           Type
  -------------- ---------------- -------------------------------------------
  `1`            `1`              `int`
  `1.2`          `1.2`            `float`
  `4j`           `4j`             `complex`
  `True`         `True`           `bool`
  `None`         `None`           `NoneType`
  `"hy"`         `'hy'`           `str`
  `b"hy"`        `b'hy'`          `bytes`
  `#(1 2 3)`     `(1, 2, 3)`      `tuple`
  `[1 2 3]`      `[1, 2, 3]`      `list`
  `#{1 2 3}`     `{1, 2, 3}`      `set`
  `{1 2  3 4}`   `{1: 2, 3: 4}`   `dict`

さらに、Hy は `fractions.Fraction` に対して Clojure スタイルのリテラル構文を持ちます： `1/3` は `fractions.Fraction(1, 3)` と等価です。

Hy REPL はデフォルトで Hy 構文で出力を表示します。関数 :hy`hy.repr` で出力します。

    => [1 2 3]
    [1 2 3]

でも、HYをこうしてスタートさせたら

    $ hy --repl-output-fn=repr

を実行すると、REPLは代わりにPythonのネイティブ関数である `repr` を使用するので、Pythonの構文で値を見ることができます。

    => [1 2 3]
    [1, 2, 3]

## 基本操作

hyの`setv` で変数を設定する。

    (setv zone-plane 8)

リスト、辞書、その他のデータ構造の要素に :hyの`get` でアクセスします。

    (setv fruit ["apple" "banana" "cantaloupe"])
    (print (get fruit 0))  ; => apple
    (setv (get fruit 1) "durian")
    (print (get fruit 1))  ; => durian

hy`cut` を用いて、順序構造の要素の範囲にアクセスします。

    (print (cut "abcdef" 1 4))  ; => bcd

条件付きロジックは :hy`if` で構築することができます。

    (if (= 1 1)
      (print "Math works. The universe is safe.")
      (print "Math has failed. The universe is doomed."))

この例のように、`if` は `(if CONDITION THEN ELSE)` のように呼び出されます。これは、`CONDITION` が (`bool` に従って) 真であれば `THEN` を、そうでなければ `ELSE` という形式で実行され、返されます。もし、 `ELSE` が省略された場合は、 `None` が代わりに使用されます。

もし、 `THEN` や `ELSE` 節、あるいは `CONDITION` の代わりに more than form を使いたい場合はどうしたらよいでしょうか。マクロ :hy`do` (Lispではより伝統的に `progn` として知られています) を使用します。これは複数の形式をひとつにまとめて、最後の形式を返します。

    (if (do (print "Let's check.") (= 1 1))
      (do
        (print "Math works.")
        (print "The universe is safe."))
      (do
        (print "Math has failed.")
        (print "The universe is doomed.")))

複数のケースで分岐させるには、 :hy`cond` を試してください。

    (setv somevar 33)
    (cond
      (> somevar 50)
        (print "That variable is too big!")
      (< somevar 10)
        (print "That variable is too small!")
      True
        (print "That variable is jussssst right!"))

マクロ `(when CONDITION THEN-1 THEN-2 ...)` は `(if CONDITION (do THEN-1 THEN-2 ...))` の省略形です。unless` は `when` と同じ働きをしますが、`not` で条件を反転させます。

Hy の基本的なループは :hy`while` です。

    (setv x 3)
    (while (> x 0)
      (print x)
      (setv x (- x 1)))  ; => 3 2 1

    (for [x [1 2 3]]
      (print x))         ; => 1 2 3

より機能的な反復処理の方法は :hy`lfor` のような内包的な形式によって提供されます。`for` が常に `None` を返すのに対して、 `lfor` は繰り返しごとに一つの要素を持つリストを返します。

    (print (lfor  x [1 2 3]  (* x 2)))  ; => [2, 4, 6]

## 関数、クラス、モジュール

hy`defn` で名前付き関数を定義する。

    (defn fib [n]
      (if (< n 2)
        n
        (+ (fib (- n 1)) (fib (- n 2)))))
    (print (fib 8))  ; => 21

hy`fn` を使って無名関数を定義する。

    (print (list (filter (fn [x] (% x 2)) (range 10))))
      ; => [1, 3, 5, 7, 9]

`defn` や `fn` のパラメータリストにある特別なシンボルによって、オプションの引数を示したり、デフォルト値を提供したり、リストにない引数を収集したりすることができます。

    (defn test [a b [c None] [d "x"] #* e]
      [a b c d e])
    (print (test 1 2))            ; => [1, 2, None, 'x', ()]
    (print (test 1 2 3 4 5 6 7))  ; => [1, 2, 3, 4, (5, 6, 7)]

関数のパラメータを `:keyword` という名前で設定します。

    (test 1 2 :d "y")             ; => [1, 2, None, 'y', ()]

hy`defclass` でクラスを定義する。

    (defclass FooBar []
      (defn __init__ [self x]
        (setv self.x x))
      (defn get-x [self]
        self.x))

ここでは、`FooBar`の新しいインスタンス `fb` を作成し、その属性に様々な手段でアクセスする。

    (setv fb (FooBar 15))
    (print fb.x)         ; => 15
    (print (. fb x))     ; => 15
    (print (.get-x fb))  ; => 15
    (print (fb.get-x))   ; => 15

`fb.x` や `fb.get-x` といった構文は、呼び出されるオブジェクト (この場合は `fb`) が単純な変数名である場合のみ動作することに注意してください。任意のフォーム `FORM` の属性を取得したりメソッドを呼び出したりするには、 `(. FORM x)` または `(.get-x FORM)` という構文を使用しなければなりません。

Python で書かれたものでも Hy で書かれたものでも、外部モジュールにアクセスするには :hy`import` とします。

    (import math)
    (print (math.sqrt 2))  ; => 1.4142135623730951

Pythonは、Hyそのものが先にインポートされている限り、他のモジュールと同様にHyモジュールをインポートすることができます。

## マクロ

マクロはLispの基本的なメタプログラミングのツールです。マクロはコンパイル時（HyプログラムがPythonの `ast` オブジェクトに変換される時）に呼び出され、最終的なプログラムの一部となるコードを返す関数です。以下は簡単な例です。

    (print "Executing")
    (defmacro m []
      (print "Now for a slow computation")
      (setv x (% (** 10 10 7) 3))
      (print "Done computing")
      x)
    (print "Value:" (m))
    (print "Done executing")

このプログラムを2回連続で実行すると、このようになります。

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

最初の起動時にプログラムをコンパイルしている間、低速計算が行われる。プログラム全体がコンパイルされた後でのみ、通常の実行が先頭から始まり、"Executing "と表示される。このプログラムが2回目に呼ばれたときは、先にコンパイルされたバイトコードから実行され、これは単純に等価である。

    (print "Executing")
    (print "Value:" 1)
    (print "Done executing")

私たちのマクロ `m` は特に単純な戻り値である整数を持ち、それはコンパイル時に 整数リテラルに変換されます。一般に、マクロはコードとして実行される任意の Hy フォームを返すことができます。例えば :hy`quote`\` のように、プログラムでフォームを簡単に構築するための特別な演算子やマクロがいくつかあります。

異なるモジュールで定義されたマクロを使いたい場合はどうしたらよいでしょうか？ なぜなら、それは単に実行時に実行される Python の `import` ステートメントに変換されるだけで、マクロはコンパイル時、つまり Hy から Python への変換の間に展開されるからです。 その代わりに :hy`require` を使ってください。これはモジュールをインポートして、コンパイル時にマクロを利用できるようにします。 `require` は `import` と同じシンタックスを使用します。

    => (require tutorial.macros)
    => (tutorial.macros.rev (1 2 3 +))
    6

これは通常のマクロに似ていますが、あらかじめ解析されたHyフォームではなく、生のソース・テキストに対して操作します。これらは、呼び出された時点以降に消費するソースコードの量を選択することができ、任意のコードを返すことができます。したがって、リーダー・マクロはHyにまったく新しい構文を追加することができます。例えば、Pythonの `decimal.Decimal` クラスに対するリテラル記法を次のように追加することができます。

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

`require` は、 `(require mymodule :readers [d])` のような構文で、異なるモジュールで定義されたリーダーマクロを引き込むことができます。

## Hyrule

[Hyrule](https://pypi.org/project/hyrule) は Hy の標準ユーティリティ・ライブラリです。Hy のプログラムを書くのに便利な様々な関数やマクロを提供しています。

    => (import hyrule [inc])
    => (list (map inc [1 2 3]))
    [2 3 4]
    => (require hyrule [assoc])
    => (setv d {})
    => (assoc d  "a" 1  "b" 2)
    => d
    {"a" 1  "b" 2}

## 次のステップ

あなたは今、Hyと一緒に危険なことをするのに十分な知識を持っています。あなたは今、邪悪な笑みを浮かべて、Hydeawayに忍び込み、言いようのないことをしてもよいのです。

Pythonのセマンティックスの詳細についてはPythonのドキュメントを、Hy特有の機能についてはこのマニュアルの残りを参照してください。Hyそのものと同様に、このマニュアルも不完全ですが、`貢献`はいつでも歓迎します。
