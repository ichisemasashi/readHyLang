# Hyスタイル・ガイド

Hyスタイル・ガイドは、Hyve（そう、Hyコミュニティは何にでもHyをつけることに誇りを持っています）が慣用的なHyコードを書くための基本ルールのセットになることを意図しています。HyはClojureとCommon Lispから多くのものを得ていますが、常にPythonの相互運用性を維持しています。

## レイアウトとインデント

Lispに対する不満の第1位は？

> *括弧が多くて見づらいわ！どうやって読むんだ？*

そして、その通りなのです。Lispはもともと読みにくかったんです。その後、レイアウトやインデントが見直されました。そして、それは見事に実現されたのです。

### 3つの法則

本当のLisperは括弧を数えない。Lispを読むとき、末尾の閉じ括弧は無視する---それはコンピュータのためのもので、人間のためのものではありません。Pythonのように、インデントでコード構造を読みます。

Lispのコードは文字列ではなく、木---Abstract Syntax Trees---で構成されています。S式はASTを非常に直接的にテキストで表現したものです。これがLispのマクロが扱う*homoiconicity*のレベルなのです。CのプリプロセッサやPythonの補間されたeval-stringのような、コードをただの文字と見なすようなトリックとは違います。Lispのコードは、区切り文字ではなく、木構造だと考えてください。

1.  〆括弧は、決して自分の線上に一人で、悲しく、寂しく放置してはならない。

``` clj
;; PREFERRED
(defn fib [n]
  (if (<= n 2)
      n
      (+ (fib (- n 1))
         (fib (- n 2)))))  ; Lots of Irritating Superfluous Parentheses L.I.S.P.（イラッとするような余計な括弧がたくさん）。 ;))

;; 経験豊富なリスパーはどう見るか。インデントされたツリー。Pythonのように。
(defn fib [n
  (if (<= n 2
      n
      (+ (fib (- n 1
         (fib (- n 2

;; BAD
;; 無視しようとしてるのに、独自路線にするのか？
;; ヒステリックなほどバカバカしい。
(defn fib [
    n
]  ; My eyes!
  (if (<= n 2)
    n
    (+ (fib (- n 1)) (fib (- n 2)))
  )
)  ; ガーッ、火で燃やせ！
```

2.  改行した行は、必ず親側の開き括弧を越えてインデントしなければならない。

``` clj
;; PREFERRED
(foo #(arg1
       arg2))

;; BAD. And evil.
;; 上記と同じブラケット構造ですが、インデントが足りません。
(foo #(arg1
  arg2))

;; PREFERRED. 上と同じインデントですが、今度は括弧と一致します。
(fn [arg]
  arg)

;; Lispを読むときは、末尾の括弧を無視することを忘れないでください。
;; それを取り除くとどうなるかというと。
;; インデントでどこに入れるかわかりますか？

(foo #(arg1
       arg2

(foo #(arg1
  arg2

(fn [arg
  arg

;; 最後の2つの構造が見分けがつかなくなったのがわかりますか？

;; インデントによる悪例の再現。
;; 私たちが始めたことではありませんね。
(foo #(arg1)
  arg2)

;; リーダーシンタックスを持つブラケットに注意。
;; それでも、それらを越えてインデントする必要があります。

;; BAD
`#{(foo)
 ~@[(bar)
  1 2]}

;; 上、トレールなし。
`#{(foo
 ~@[(bar
  1 2

;; 再構成する。です。間違っています。
`#{(foo)}
 ~@[(bar)]
  1 2

;; PREFERRED
`#{(foo)
   ~@[(bar)
      1
      2]}

;; OK
;; 文字列はアトムであり、シークエンスではありません。
(foo "abc
  xyz")

;; 末尾の括弧がなくても読めます。
(foo "abc
  xyz"  ; ダブルクオートは閉じ括弧ではありません。無視しないでください。
```

3.  新しい行は、前の要素の開始括弧を越えて決してインデントしてはいけません。

``` clj
;; BAD
((get-fn q)
  x
  y)

;; 上記から末尾の括弧を削除したものです。問題がわかりましたか？
((get-fn q
  x
  y

;; インデントによって、ここに括弧が入るはずです。
((get-fn q
  x
  y))

;; OK
((get-fn q) x
            y)

;; 上記から末尾の括弧を除いたもの。それでも（人間には）OK。
((get-fn q) x  ; この行の ) は、末尾に付いていません!
            y

;; PREFERRED, ) は行を終了させる必要があるからです。
((get-fn q)
 x
 y)
```

### 制限事項

PEP 8 の行数制限のルールに従ってください。

> -   テキスト（docstringとコメント）は最大72カラムです。
> -   その他のコードで最大79列、または
> -   99に同意できるチームが主にメンテナンスしている場合、その他のコードには99。

### 空白

最後のスペースは使わないでください。最悪です!

コード中のタブを避ける。インデントにはスペースのみを使用する。

1行の文字列リテラルに含まれるタブ文字に対して、`\t`エスケープシーケンスを優先します。

> -   警告コメントも付ければ、複数行の文字列の中でもリテラルタブはOKです。
> -   しかし、複数行の文字列では `\t` が優先されます。
> -   コメントは、文字列の直前に表示することが望ましい。
> -   しかし、関数、クラス、ファイルの先頭にある包括的な警告はOKです。

### 整列

複数行に分割された関数呼び出しの引数を整列させます。

> -   最初の引数は、関数名と一緒に最初の行に置かれることが望ましい。
> -   代わりにその親ブラケットから1スペースインデントした次の行から始めることができます。

``` clj
;; PREFERRED. すべての引数は最初の引数で整列されます。
(foofunction arg1
             (barfunction bararg1
                          bararg2
                          bararg3)  ; bararg1と並んでいる。
             arg3)

;; BAD
(foofunction arg1
             (barfunction bararg1
               bararg2  ; 間違っている。マクロボディのようだ。
                    bararg3)  ; Why?!
             arg3)

;; PREFERRED. Argsは適当であれば1行にまとめてもよい。
(foofunction arg1
             (barfunction bararg1 bararg2 bararg3)
             arg3)

;; OK. Argsが1行目にない場合でも、整列している。
(foofunction
  arg1  ; 親(から1列分インデントしている
  (barfunction
    bararg1  ; 再度インデントする。
    bararg2  ; bararg1と並んでいる。
    bararg3)
  arg3)  ; arg1 と整列する。
```

### 開いたままにする

もしブラケットトレイルを分離する必要がある場合は、`#_ /` のコメントを使用して、ブラケットを開いたままにします。これにより、法則その1への抵触を回避することができます。

``` clj
;; PREFERRED
[(foo)
 (bar)
 (baz)]

;; OK, 特にリストが長い場合は。(3つが長いというわけではありませんが)
;; バージョン管理用の行間差分にはこちらの方が適しています。
[  ; 開き括弧が「末尾の閉じ括弧」になることはありませんよ。
 (foo)
 (bar)
 (baz)
 #_ /]  ; 何もありません。移動してください。

;; リストの末尾の項目をコメントアウトする例は次のとおりです。
;; REPLでの入力と同様、このようなケースも、自分だけが見ているのであれば、それほど重要ではありません。
;; しかし、たとえそうであっても、良いスタイルを維持することは、エラーを防ぐのに役立ちます。

;; BAD and a syntax error. ブラケットを紛失
[(foo)
 ;; (bar)
 ;; (baz)]

;; BAD. Broke law #1.
[(foo)
 ;; (bar)
 ;; (baz)
 ]

;; PREFERRED
;; 廃棄構文は、コード構造を尊重する。
;; ので、エラーが発生しにくくなります。
[(foo)
 #_(bar)
 #_(baz)]

;; OK. 最後の捨て要素を追加することで、ラインコメントの安全性を高めています。
[(foo)
 ;; (bar)
 ;; (baz)
 #_ /]
```

### 添い寝

ブラケットは寄り添うのが好きです。

``` clj
;; PREFERRED
[1 2 3]
(foo (bar 2))

;; BAD
[ 1 2 3 ]
( foo ( bar 2 ) )

;; BAD. And ugly.
[ 1 2 3]
(foo( bar 2) )
```

### グループ化

空白を使用して暗黙のグループを表示しますが、フォーム内では一貫性を保つようにしてください。

``` clj
;; 古いLispsでは、このようなグループはさらに括弧でくくられるのが普通である。
;; (Common LispのLOOPマクロは顕著な例外であった）。
;; しかし、Hyは軽いタッチのClojureに倣っています。

;; BAD. カウントしないとキーと値を区別できない
{1 9 2 8 3 7 4 6 5 5}

;; PREFERRED. これは1行に収まります。Clojureはここでカンマを使ったでしょうが、
;; それはHyでは空白ではありません。代わりに余分な空白を使いましょう。
{1 9  2 8  3 7  4 6  5 5}

;; OK. And preferred 1行に収まらない場合は
{1 9
 2 8
 3 7
 4 6
 5 5}  ; 改行はdictのkey-valueペアを表示します。

;; BAD
;; このグループ分けは意味がない。
#{1 2
  3 4}  ; セットなのに、なぜペアがあるのですか？

;; BAD
;; このグループ分けも意味がない。でも、このグループ分けに何らかの意味があれば、
;; マクロとかではOKかもしれませんね。
[1
 1 2
 1 2 3]  ; なぜランダムなパターンが好きなのですか？[sicのシャレです、すみません]

;; 一貫性を持たせる。フォームの中ですべてのグループを同じように区切る。

;; BAD
{1 9  2 8
 3 7  4 6  5 5}  ; どちらか一方を選んでください

;; BAD
{1 9  2 8 3 7  4 6  5 5}  ; 何か忘れてないか？

;; グループ・オブ・ワンも一貫性が必要です。

;; PREFERRED
(foo 1 2 3)  ; ここに余分なスペースは必要ありません。

;; OK, しかし、これを1行に収めることができたはずです。
(foo 1
     2
     3)

;; OK, しかし、それでも1行で収まったはずです。
[1
 2]

;; BAD
(foo 1 2  ; これってペアじゃないんですか？
     3)  ; ラインかスペースか、どちらかを選んでください。

;; PREFERRRED
(foofunction (make-arg)
             (get-arg)
             #tag(do-stuff)  ; タグは、タグ付けされたものに属する。
             #* args  ; #* は展開されたものと一緒になります。
             :foo spam
             :bar eggs  ; キーワードの引数もペアです。グループ化する。
             #** kwargs)

;; PREFERRED. スペースは1行のグループを分割する。
(quux :foo spam  :bar eggs  #* with-spam)
{:foo spam  :bar eggs}

;; OK. グループを示すには、やはりコロンで十分です。
(quux :foo spam :bar eggs #* with-spam)
{:foo spam :bar eggs}
;; OK.
("foo" spam "bar" eggs}

;; BAD. キーと値を区別できない。
(quux :foo :spam :bar :eggs :baz :bacon)
{:foo :spam :bar :eggs :baz :bacon}
{"foo" "spam" "bar" "eggs" "baz" "bacon"}

;; PREFERRED
(quux :foo :spam  :bar :eggs  :baz :bacon)
{:foo :spam  :bar :eggs  :baz :bacon}
{"foo" "spam"  "bar" "eggs"  "baz" "bacon"}

;; OK. そう、それもペアです。
(setv x 1
      y 2)

;; PREFERRED. これは1行に収まる。
(setv x 1  y 2)

;; BAD. グループを分けない。
(print (if (< n 0.0)
           "negative"
           (= n 0.0)
           "zero"
           (> n 0.0)
           "positive"
           "not a number"))

;; BAD. And evil. 法則その3を破った。グループは表示されるが、引数は整列していない。
(print (if (< n 0.0)
               "negative"
           (= n 0.0)
               "zero"
           (> n 0.0)
               "positive"
           "not a number"))

;; BAD. グループを表示するが、引数が揃わない。
;; もし、当時の部品がアトムでなかったら、これは法則の3番を破ることになる。
(print (if (< n 0.0)
         "negative"
           (= n 0.0)
         "zero"
           (> n 0.0)
         "positive"
           "not a number"))

;; OK. 冗長な（do）フォームは、法則3に違反することなく、
;; グループを表示するための余分なインデントを可能にします。
(print (if (< n 0.0)
           (do
             "negative")
           (= n 0.0)
           (do
             "zero")
           (> n 0.0)
           (do
             "positive")
           "not a number"))
```

Pythonのように2行ではなく、1行の空白行でトップレベルフォーム（特定のフォームに関するものではないトップレベルコメントを含む）を区切ります。

> -   これは、緊密に関連するフォームでは省略可能です。

defclass内のメソッドは、空白行で区切る必要はありません。

### 特殊な引数

マクロや特殊な書式は通常、親カッコから1字下げられますが、関数の引数のように字下げされた「特殊」な引数を持つこともできます。

> -   `#* body` 引数を持つマクロは暗黙のうちに `do` を含んでいます。
> -   本体は決して特別なものではありませんが、その前の引数は特別なものです。

``` clj
;; PREFERRED
(assoc foo  ; fooは特別です
  "x" 1  ; 残りの引数は特別なものではありません。スペース2つ分インデントしてください。
  "y" 2)

;; PREFERRED
;; do形式は特別な引数を持たない。関数呼び出しのようにインデントしてください。
(do (foo)
    (bar)
    (baz))

;; OK
;; 区別するための特別な引数はありません。また、関数インデントも有効である。
(do
  (foo)
  (bar)
  (baz))

;; PREFERRED
(defn fib [n]
  (if (<= n 2)
      n
      (+ (fib (- n 1))
         (fib (- n 2)))))

;; OK
(defn fib
      [n]  ; nameとargslistは特別です。関数の引数のようにインデントする。
  ;; defn 本体は特別なものではありません。親カッコから1字分インデントする。
  (if (<= n 2)
      n
    (+ (fib (- n 1))  ; Emacs 風の else インデント。
       (fib (- n 2)))))
```

### ホワイトスペースの除去

空白を削除することで、グループをより明確にすることができます。

``` clj
;; lookups

;; OK
(. foo ["bar"])

;; PREFERRED
(. foo["bar"])

;; BAD. グループをはっきり表示しない。
(import foo foo [spam :as sp eggs :as eg] bar bar [bacon])

;; OK. 余分なスペースはグループを示す。
(import foo  foo [spam :as sp  eggs :as eg]  bar  bar [bacon])

;; PREFERRED. スペースを削除すると、さらに分かりやすくなります。
(import foo foo[spam :as sp  eggs :as eg] bar bar[bacon])

;; OK. 改行はグループを示す。
(import foo
        foo [spam :as sp
             eggs :as eg]
        bar
        bar [bacon])

;; PREFERRED, その方が、望ましい一行版との整合性が保たれます。
(import foo
        foo[spam :as sp
            eggs :as eg]
        bar
        bar[bacon])

;; タグの後の空白は避けてください。

;; どちらのグループがより良く表示されるかに注目してください。

;; BAD
(foofunction #tag "foo" #tag (foo) #* (get-args))

;; OK
(foofunction #tag "foo"  #tag (foo)  #* (get-args))

;; PREFERRED
(foofunction #tag"foo" #tag(foo) #*(get-args))

;; PREFERRED
;; 空白を削除してグループ化することはできません。代わりに余分なスペースを使用してください。
(foofunction #x foo  #x bar  #* args)

;; OK
;; 同じ考えですが、これは1行で収まったかもしれませんね。
(foofunction #x foo
             #x bar
             #* args)

;; OK, しかし、関数名と最初の引数を分離する必要はありません。
(foofunction  #x foo  #x bar  #* args)

;; OK. しかし、同じ考えです。
;; 最初のグループと関数名を分ける必要はありません。
(foofunction
  #x foo
  #x bar
  #* args)

;; PREFERRED. これが何にタグ付けされているのかはまだ不明です。
;; しかも、インデントし直す必要はない。
#_
(def foo []
  stuff)

;; OK, しかし、より多くの仕事があります。
#_(def foo []
    stuff)

;; BAD, インデントがうまくいかず、法則2を破ってしまいましたね。
#_(def foo []
  stuff)

;; BAD, タグを引数でグループ化したままにしておきます。
#_

(def foo []
  stuff)
```

### 閉じブラケット、行を閉じる

暗黙のグループの途中でない限り、1つの閉じ括弧で行を終了させるべきです。

> -   もしフォームが小さくてシンプルなら、1行に残すこともできます。

行末には必ず閉じ括弧を付けてください。

``` clj
;; ワンライナーは過大評価されている。
;; REPLに打ち込むだけならOKかもしれない．
;; しかし、その場合でも、良いスタイルを維持することは、エラーを防ぐのに役立ちます。

;; BAD. ワンライナーは読みにくすぎる。
(defn fib [n] (if (<= n 2) n (+ (fib (- n 1)) (fib (- n 2)))))

;; BAD. 良くなってきているが、最初の行はまだ複雑すぎる。
(defn fib [n] (if (<= n 2) n (+ (fib (- n 1))
                                (fib (- n 2)))))
;; OK. かろうじて。
(defn fib [n]
  (if (<= n 2) n (+ (fib (- n 1))  ; このラインは無理してますね。
                    (fib (- n 2)))))

;; OK
(defn fib [n]  ; "]”、改行を見て。
  (if (<= n 2)  ; OK ワンペアしかないので、ここでブレイクします。
      n
    (+ (fib (- n 1))  ; 空白の区切り（Emacsのelse-indent）。
       (fib (- n 2)))))

;; OK
(defn fib [n]  ; “]"、行末が表示されます。(マージンコメントはカウントされません）。
  (if (<= n 2) n  ; “)”が表示されたが、この行から始まるペアの中にある。
      (+ (fib (- n 1))  ; Saw a "))" MUST end line.
         (fib (- n 2)))))

;; OK. Pairs.
(print (if (< n 0.0) "negative"  ; グループ内のシングル）。ブレイクなし。
           (= n 0.0) "zero"
           (> n 0.0) "positive"
           :else "not a number"))  ; :elseは魔法ではなく、Trueも効きます。

;; OK. ペアを表示するため、single )で改行しないようにした。
(print (if (< n 0.0) "negative"
           (= n 0.0) "zero"
           (> n 0.0) (do (do-foo)  ; グループ内のシングル）。ブレイクなし。
                         (do-bar)
                         "positive")
           "not a number"))  ; 暗黙のelseがあることが望ましい。

;; BAD
(print (if (< n 0.0) "negative"
           (= n 0.0) "zero"
           (and (even? n)
                (> n 0.0)) "even-positive"  ; Bad. “))" がブレイクする必要があります。
           (> n 0.0) "positive"
           "not a number"))

;; BAD
(print (if (< n 0.0) "negative"
           (= n 0.0) "zero"
           (and (even? n)
                (> n 0.0)) (do (do-foo)  ; Y U no break?
                               (do-bar)
                               "even-positive")
           (> n 0.0) "positive"
           "not a number"))

;; OK. 空白行で複数行のグループを区切ります。
(print (if (< n 0.0) "negative"

           (= n 0.0) "zero"

           (and (even? n)
                (> n 0.0))
           (do (do-foo)
               (do-bar)
                "even-positive")

           (> n 0.0) "positive"

           "not a number"))

;; BAD. グループ分けが統一されていない。
(print (if (< n 0.0) "negative"
           (= n 0.0) "zero"

           (> n 0.0)
           (do (do-foo)
               "positive")

           "not a number"))

;; OK. シングル ）とフォームがシンプルでいい。
(with [f (open "names.txt")]
  (-> (.read f) .strip (.replace "\"" "") (.split ",") sorted)))

;; PREFERRED. それでも、このバージョンの方がより鮮明です。
(with [f (open "names.txt")]
  (-> (.read f)
      .strip
      (.replace "\"" "")
      (.split ",")
      sorted)))
```

### コメント

docstring はコメントよりも優先されます。モジュールの先頭の `fn`, `defclass` や、そこから派生した docstring を受け取ることのできるマクロ (例えば `defmacro/g!`, `defn` など) で利用できます。

Docstringsの内容は、Pythonと同じ規約に従います。

`(comment)`マクロはまだ3つの法則に従います。もし違反したくなったら、代わりに `#_` で文字列を破棄することを検討してください。

セミコロンコメントは、セミコロンとコメントの始まりの間に必ずスペースを1つ入れてください。また、当たり前のことをコメントしないように心がけましょう。

一語以上のコメントは、大文字で始め、句読点を使用すること。

文章は半角スペースで区切ってください。

``` clj
;; この解説は、特定のフォームについてのものではありません。
;; これらは複数の行にまたがることができます。PEP8に従って、72列目までに制限してください。
;; 次のフォームやフォームのコメントとは空白行で区切る。

;; PREFERRED.
(setv ind (dec x))  ; インデックスは0から始まります。
                                ; マージン・コメントは改行されます。

;; OK
;; スタイルコンプライアントでありながら、当たり前のことを当たり前に述べるだけ。
(setv ind (dec x))  ; インデックスをx-1にセットする。

;; BAD
(setv ind (dec x));ことばあそび

;; foofunction関数コールの全体像についてコメントする。
;; また、これらは複数のラインにまたがることも可能です。
(foofunction ;; (get-arg1)についてのフォームコメントです。マージンコメントではありません
             (get-arg1)
             ;; arg2に関するフォームコメント。インデントが一致する。
             arg2)
```

フォームコメントは、コメントするフォームと同じレベルで字下げします。コメントは常に2つのセミコロン `;;` で始めなければなりません。フォームコメントはコメントする対象の真上に表示され、決して下に表示されることはありません。

一般的なトップレベルコメントはインデントされません。これらは常に2つのセミコロン `;;` から始まり、次のフォームとは空白行で区切らなければなりません。長いコメントには、代わりに `#_` を文字列に適用して使うことを検討してください。

マージンコメントはコードの最後から空白2文字分のところから始めます。マージンコメントは常にセミコロン `;` で始めなければなりません。マージンコメントは次の行に続けることができます。

フォーム全体をコメントアウトする場合は、`#_` シンタックスを使用します。しかし、行のコメントが必要な場合は、より一般的なダブルコロンフォームを使用します。

## コーディングスタイル

### Pythonicな名前

Pythonの命名規則がHyに適用可能な場合は、Pythonの命名規則を使用してください。

> -   メソッドの最初のパラメータは `self` であり、クラスメソッドの最初のパラメータは `cls` である。

### スレッディングマクロ

スレッディングマクロやスレッディングテイルマクロは、深いネストしたS式に遭遇したときに優先的に使用されます。しかし、それらを使用するときは慎重になります。わかりやすく、読みやすくするために使用するのであって、複雑で理解しにくい式を作らないようにしましょう。

``` clj
;; BAD. 間違ってはいないが、スレッドマクロを使えばもっとクリアになるはず。
(setv NAMES
  (with [f (open "names.txt")]
    (sorted (.split (.replace (.strip (.read f))
                              "\""
                              "")
                    ","))))

;; PREFERRED. これは、上記と全く同じようにコンパイルします。
(setv NAMES
  (with [f (open "names.txt")]
    (-> (.read f)
        .strip
        (.replace "\"" "")
        (.split ",")
        sorted)))

;; BAD. Probably. マクロでは、この場合、あまり明確にはなりません。
(defn square? [x]
  (->> 2
       (pow (int (sqrt x)))
       (= x)))

;; OK. 上の前の例よりずっとわかりやすい。
(defn square? [x]
  (-> x
      sqrt
      int
      (pow 2)
      (= x))

;; PREFERRED. 使用上の注意
;; わかりやすさを向上させるのであれば、すべてをスレッド化する必要はありません。
(defn square? [x]
  (= x (-> x sqrt int (pow 2))))

;; OK. 今回はスレッドマクロがないのでまだ十分クリア。
(defn square? [x]
  (= x (pow (int (sqrt x))  ; saw a "))", break.
            2))  ; aligned with first arg to pow
```

### メソッド呼び出し

Clojureスタイルのドット表記は、オブジェクトのメソッドの直接呼び出しよりも優先されますが、両方とも引き続きサポートされます。

``` clj
;; PREFERRED
(with [fd (open "/etc/passwd")]
  (print (.readlines fd)))

;; OK
(with [fd (open "/etc/passwd")]
  (print (fd.readlines)))
```

### より多くの引数を使用する

複数の形式よりも複数の引数を使用することを優先する。しかし、冗長なフォームを適切に使用することで、意図を明確にすることができます。この場合、トップレベルフォームのための区切り空白行を避ける。

``` clj
;; BAD
(setv x 1)
(setv y 2)
(setv z 3)
(setv foo 9)
(setv bar 10)

;; OK
(setv x 1
      y 2
      z 3
      foo 9
      bar 10)

;; PREFERRED
(setv x 1
      y 2
      z 3)
(setv foo 9
      bar 10)
```

### Imports

Pythonのように、グループインポートを行う。

> -   標準ライブラリのインポート(Hyのものを含む)が先。
> -   次にサードパーティーモジュール、最後に内部モジュールです。

各グループに1つのインポートフォームを用意することをお勧めします。

グループ内ではアルファベット順を優先する。

インポートの前にマクロを必須とし、グループ分けも同じようにする。

しかし、プログラム上の理由で、インポートが条件付きであったり、特定の順序で並べなければならないことがありますが、これは問題ありません。

``` clj
;; PREFERRED
(require hy.extra.anaphoric [%])
(require thirdparty [some-macro])
(require mymacros [my-macro])

(import json re)
(import numpy :as np
        pandas :as pd)
(import mymodule1)
```

### アンダースコア

単語を区切るときは、ハイフンを優先します。

-   PREFERRED `foo-bar`
-   BAD `foo_bar`

演算子やハイフンを含むと解釈されるシンボル (例: `-Inf`, `->foo`) を除いて、先頭にハイフンを使用しないでください。

プライベート名のプレフィックスには、ダッシュではなくアンダースコアを使用します。これは `-Inf`, `-42`, `-4/2` のような反転したリテラルと混同しないようにするためです。

-   PREFERRED `_x`
-   BAD `-x`

Pythonの魔法の「dunder」名をPythonと同じように書きます。上記のプライベート名のルールと矛盾しないように、 `--init--` などではなく、 `__init__` のようにします。

プライベート名は、アンダースコアの代わりにダッシュで単語を区切らなければなりません。これは、プライベートでないパラメータ名など、プレフィックスなしで同じ名前を必要とするものと矛盾しないように、`foo_bar`ではなく、`foo-bar`のようにします。

-   PREFERRED `_foo-bar`
-   BAD `_foo_bar`

``` clj
;; BAD
;; 何してるんですか？
(_= spam 2)  ; 捨ててしまう？
(_ 100 7)  ; i18n?

;; PREFERRED
;; 明らかに引き算。
(-= spam 2)
(- 100 7)

;; BAD
;; これは変な感じですね。
(_>> foo bar baz)

;; PREFERRED
;; OH、それは矢だ!
(->> foo bar baz)

;; Negative x?
(setv -x 100)  ; BAD. 本気でそう思っているのでなければ？

;; PREFERRED
;; あ、モジュールのプライベートだけです。
(setv _x 100)

;; BAD
(class Foo []
  (defn __init-- [self] ...))

;; OK
(class Foo []
  ;; Less weird?
  (defn --init-- [self] ...))

;; PREFERRED
(class Foo []
  (defn __init__ [self] ...))

;; OK, ただし、モジュールのプライベートとなる。(インポートなし *)
(def ->dict [#* pairs]
  (dict (partition pairs)))
```

## Thanks

-   このガイドは、[\@paultag](https://github.com/paultag) のブログ記事 [Hy Survival Guide](https://notes.pault.ag/hy-survival-guide/) から大いにインスピレーションを受けたものです。
-   The [Clojure Style Guide](https://github.com/bbatsov/clojure-style-guide)
-   [Parinfer](https://shaunlebron.github.io/parinfer/) and [Parlinter](https://github.com/shaunlebron/parlinter) (the three laws)
-   The Community Scheme Wiki [scheme-style](http://community.schemewiki.org/?scheme-style) (ending bracket ends the line)
-   [Riastradh\'s Lisp Style Rules](http://mumble.net/~campbell/scheme/style.txt) (Lisp programmers do not ... Azathoth forbid, count brackets)
