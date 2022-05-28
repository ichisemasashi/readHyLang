

A Lisp Programmer Living in Python-Land: The Hy Programming Language



Mark Watson




## Preface

本書はHy Lisp言語に関する書籍ですが、ここにはより広いテーマがあります。人工知能（AI）が大企業や政府機関を動かす時代において、小規模というデメリットを考慮した上で、個人や小さな組織がAI技術をどう活用するかという問題です。ここで書く材料は、読者の皆さんが大きな絆の中で健全な雑魚として生き残るために選んだものです。

私は1982年から専門的にLisp言語を使っており、Common LispやScheme言語をカバーする書籍も執筆しています。私のキャリアのほとんどはAIプロジェクトに携わってきましたので、AIアプリケーションを開発するためのツールが大きなテーマとなります。本書では、Hy言語に加えて、コンサルタント、スタートアップ企業、企業などを問わず、独自のAIプラットフォームを構築するのに役立つAIツールやテクニックを体験することができます。

本書では、Python ASTにコンパイルされ、Pythonで書かれたコード、ライブラリ、フレームワークと互換性のあるLisp言語 **Hy** を用いて、多くのプログラミングトピックを扱います。主なトピックとして、以下のものを取り上げ、サンプルアプリケーションを作成します。

- リレーショナルデータベース、グラフデータベース
- ウェブアプリ開発
- ウェブスクレイピング
- Wikipedia、DBpedia、Wikidataなどのセマンティックウェブやリンクされたデータソースへのアクセス
- テキスト文書、セマンティックウェブ、リンクデータから自動的にナレッジグラフを構築する
- ディープラーニング
- Deep Learningを用いた自然言語処理(NLP)

トピックは私の仕事の経験から選ばれました。本書のテーマは、ボトムアップ開発スタイルでLisp言語を使ってプログラマの生産性と幸福度をいかに高めるか、ということです。このスタイルは、APIを探索し新しいコードを書くために、インタラクティブなREPLの使用に大きく依存しています。私は、開発者と研究者として働いてきた経験をもとに、上記のトピックを選びました。注意：本書では、REPLという用語を頻繁に目にします。REPLとは、*Read Eval Print Loop*の略です。

例題の中には、非常にシンプルなもの（Webアプリの例など）もあれば、複雑なもの（Deep Learningやナレッジグラフの例など）もあります。例のシンプルさ、複雑さに関わらず、コードが面白く、プロジェクトに役立ち、実験する楽しさを感じていただければと思います。

### 開発環境を整える

本書は実践的な本です。親愛なる読者の皆さんには、この本を読みながら、例題に沿って進めていただくことを期待しています。Pythonをある程度知っていて、コマンドラインツール **python** と **pip** の使い方を知っていて、[Anaconda (**conda**)](https://www.anaconda.com/) や [**virtualenv**](https://virtualenv.pypa.io/en/latest/) などの仮想Python環境を使っていると仮定しています。個人的には**conda**が好きですが、いくつかのパッケージがインストールされている限り、好きなPython 3.xのセットアップを使用することができます。

現在の安定版である**Hy**は、以下を使用してインストールすることができます。


    1     pip install git+https://github.com/hylang/hy.git


どのサンプルを実行して実験するかにもよりますが、以下のライブラリのいくつかをインストールする必要があります。


    1     pip install beautifulsoup4 Flask Jinja2 Keras psycopg2
    2     pip install rdflib rdflib-sqlite spacy tensorflow
    3     pip install PyInquirer


Hy言語は活発に開発されており、現在のHyリリースの数ヶ月以上前に作成されたライブラリやフレームワークが壊れることは珍しくありません。このため、本のネタを選ぶ際には、新しいリリースでは動かないかもしれないと感じるHyエコシステムの興味深い機能やライブラリは省くように注意しています。ここでは、Keras、TensorFlow、spaCyといったいくつかの人気のあるPythonライブラリにこだわり、それ以外はほとんど純粋なHy言語のコードで例題を進めていく予定です。

### Lispプログラミングスタイルとは？

ここではいくつかの例を挙げ、この本の後半では探索的Hy言語REPLの例も紹介します。あるライブラリの使い方のドキュメントをウェブで検索し、コードを書いてみて、後でAPIを正しく使っていなかったことに気づくことがよくあるのではないでしょうか？私は、Lisp REPLを開いておき、ドキュメントを読みながらAPIの呼び出しと返される結果を実験できるようにして、コードを書く時間を減らしています。

新しいコードやアルゴリズムに取り組むときは、LispのREPLを開いて、低レベルの問題を解決するための動作コードを得るために短いコードの断片を試し、より複雑なコードへと発展させるのが好きです。そして、そのコードを自分のライブラリに変換します。そして、新しいライブラリをREPLにロードしてストレステストを行い、APIの改善点を探すといったことを繰り返しています。

私は、一般的に、「ボトムアップ」のアプローチの方が、前もって計画や設計に時間をかけすぎるよりも、高品質なシステムをより早く作ることができると考えています。設計に時間をかけすぎると、コードを試しながら問題を解決するために何が最も理にかなっているのか、考えが変わってしまうという問題があります。私は、やり直したり、捨てたりしなければならないような仕事には、なるべく時間をかけないようにしています。

### HyはPythonだがLispの構文で

Hyのプロジェクトでライブラリが必要なとき、私はPythonのライブラリを探し、Pythonライブラリの周りに薄いHy言語の「ラッパー」を書くか、Hyのコードから直接PythonのAPIを呼び出すかのどちらかです。この本では、両方のアプローチの多くの例を見ることができます。

### 人工知能と社会・技術の未来に対する私の考え方が本書に反映されている点

1982年に人工知能の研究を始めて以来、私はこの分野が、国際会議の出席者数さえ少ないニッチな技術から、一般に変革的とみなされている分野へと発展するのを目の当たりにしてきました。米国では、中国のような経済的敵対国が、AIの中核技術を開発し、その技術を商用および軍事システムに統合する我々の能力を超えるという正当な懸念がある。2020年2月にこれを書いている時点で、私を含むこの分野の人々の中には、中国企業のバイドゥが応用AIですでにグーグルやマイクロソフトを追い抜いたかもしれないと考えている人もいます。

過去5年間の私の専門的な仕事のほとんどはDeep Learningでしたが（その前はGoogleで知識表現問題とその応用に取り組んでいました）、私は人間レベルの人工一般知能（AGI）はDeep Learningと「昔ながらの」記号的AI、そしてまだ発見されていない技術をハイブリッドで使うことになると信じています。

ディープラーニングではAGIの能力は得られないというこの信念が、私がHy言語を使う動機となっています。Hy言語は、私が記号AIと知識表現を使って何十年も使ってきたボトムアップのLisp開発スタイルでPython Deep Learningフレームワークへの透明なアクセスを提供してくれるからです。

Hyが私のニーズと同じようにあなたのニーズにも合致していることを、私は願っています。

### ブックカバーについて

Hy Languageの公式ロゴはタコです。


![The Hy Language logo Cuddles by Karen Rustad](/site_images1/hy-lisp-python/hylisplogo.jpg)


普段、LeanPubの本の表紙は自分で撮った写真を使っています。私は13歳からSCUBAダイビングをしていますが、悲しいかな、自分で撮ったタコの写真は持っていません。しかし、Wikimediaで気に入ったパブリックドメインの写真（この本の表紙です）を見つけました。**表紙クレジット**。ウィキメディア・ユーザーのPseudopanaxが、この表紙画像をパブリックドメインに置いてくれたことに感謝します。

### 著者からのお願い

本書は、読者の皆様のお役に立ちたいと思い、時間をかけて書き上げました。私は、多くの読者に読んでいただくために、本書をクリエイティブ・コモンズの「共有・共用、改変禁止、商用利用禁止」ライセンスで公開し、最低購入価格を5ドルに設定しました。このライセンスのもとでは、本書のPDF版を友人や同僚と共有することができます。もし、あなたがこの本をウェブで見つけて（あるいは、もらって）、それがあなたにとって価値あるものであれば、私の今後の執筆活動や、この本の今後の更新をサポートするために、以下のいずれかを行うことを検討してください。

-   本書やその他のリーンパブブックを購入する場合は、<https://leanpub.com/u/markwatson>までご連絡ください。
-   [コンサルタントとして雇用する](https://markwatson.com/)

私は書くことを楽しんでおり、皆様のご支援は、私の本の新しい版や更新を書いたり、新しい本のプロジェクトを開発するのに役立っています。ありがとうございました。

### 謝辞

この原稿を編集し、誤字脱字を見つけ、改善点を提案してくれた妻キャロルに感謝する。

修正と提案をいただいたパスカル（Redditユーザー chuchana）に感謝したい。誤字を発見し報告してくれたCarlos Ungilに感謝したい。いくつかの誤字を発見してくれたJud Taylorに感謝したい。

## Hy言語入門

[Hyプログラミング言語](http://docs.hylang.org/en/stable/)は、Pythonとスムーズに相互運用できるLisp言語です。まず、いくつかの対話的な例から始めますので、読みながら実験することをお勧めします。その後、本書の残りの部分で使用されるHyデータ型とよく使われる組み込み関数について見ていきます。

私は、あなたが少なくとも少しのPythonと、さらに重要なPythonエコシステムと**pip**のような一般的なツールを知っていると仮定しています。

あなたの現在のPython環境にHyをインストールすることから始めてください。


    pip install git+https://github.com/hylang/hy.git


### 書籍のサンプルコードでは、寄稿された**let**マクロをしばしば使用します。

Scheme、Clojure、Common Lispの各言語では、ローカル変数や関数を含むコードブロックの定義に **let** という特殊な形式が使われます。この本のほとんどの例では、組み込みの特別な形式を代替する寄稿された**let**マクロを要求(またはインポート)しますが、短いコードのリストでは**require**を含めないかもしれません。各例題の冒頭には必ず次のような行があると仮定してください。


    1 #!/usr/bin/env hy
    2 
    3 (require [hy.contrib.walk [let]])


1行目は、Pythonスクリプトを実行可能なプログラムにする方法と同様です。 ここでは、**python**の代わりに**hy**を実行します。3行目は**let**マクロをインポートしています。ローカル変数や関数を定義したコードブロックやクロージャを使用する際に **let** を使用することがあります（クロージャについては本章の最後で説明します）。


     1 #!/usr/bin/env hy
     2 
     3 (require [hy.contrib.walk [let]])
     4 
     5 (let [x 1]
     6   (print x)
     7   (let [x 33]
     8     (print x)
     9     (setv x 44)
    10     (print x))
    11   (print x))


出力は以下となります。


    1
    33
    44
    1


内側の**let**式で**x**に新しい値を設定しても、外側の**let**式で変数**x**に束縛された値は変更されないことに注意してください。

### Pythonライブラリの利用

TensorFlow、Keras、BeautifulSoupなどのPythonのライブラリを使うことが、私がHy言語を使う理由です。PythonのコードやライブラリをインポートしてPythonに呼び出すのは簡単で、ここでは十分な例を見て、後で見るサンプルコードを理解するようにします。

例えば、**Responsible Web Scraping**の章では、BeautifulSoupライブラリを使用します。いくつかのPythonのコード・スニペットと、これらのスニペットに対応するHy言語版を見ていきます。まず、Pythonの例を見て、それをHyに変換してみましょう。


    1 from bs4 import BeautifulSoup
    2 
    3 raw_data = '<html><body><a href="http://markwatson.com">Mark</a></body></html>'
    4 soup = BeautifulSoup(raw_data)
    5 a_tags = soup.find_all("a")
    6 print("a tags:", a_tags)


次のリストでは、Hyで他のコードやライブラリをどのようにインポートしているかに注目してください。特殊な形式である **setv** はローカルな文脈で変数を定義するために使われます。行目、5行目、6行目の**setv**文はトップレベルで使用されているので、ソースファイルのルート名と同じ名前のPython/Hyモジュールではグローバルです。


     1 $ hy
     2 hy 0.18.0 using CPython(default) 3.7.4 on Darwin
     3 => (import [bs4 [BeautifulSoup]])
     4 => (setv raw-data "<html><body><a href=\"http://markwatson.com\">Mark</a></body></ht\
     5 ml>")
     6 => (setv soup (BeautifulSoup raw-data "lxml"))
     7 => (setv a (.find-all soup "a"))
     8 => (print "atags:" a)
     9 atags: [<a href="http://markwatson.com">Mark</a>]
    10 => (type a)
    11 <class 'bs4.element.ResultSet'>
    12 => (dir a)
    13 ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dict__', '\
    14 __dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattr__', '__getattribut\
    15 e__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__in\
    16 it_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__module__', '__mul__', \
    17 '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__r\
    18 mul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', '\
    19 __weakref__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop'\
    20 , 'remove', 'reverse', 'sort', 'source']


3行目と6行目では、Pythonではアンダースコア" \_"を使うところを、Hy言語では変数名や関数名の中に"-"を使うことができます（この例では**raw-data**と**find-all**です）。Pythonと同様、**type**で値の型を、**dir**でオブジェクトに利用可能なシンボルを得ることができます。

### Global vs. Local Variables

一般的にはお勧めしませんが、**setv** や **let** マクロ展開で定義されたローカル変数を、現在のモジュールのコンテキストでグローバル変数としてエクスポートすると便利な場合があります（現在のソースファイルで定義されている）。例として


     1 Marks-MacBook:deeplearning $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (defn foo []
     4 ... (global x)
     5 ... (setv x 1)
     6 ... (print x))
     7 => (foo)
     8 1
     9 => x
    10 1
    11 => 


関数**foo**を実行する前は、グローバル変数**x**は未定義です（偶然にすでにどこかで定義されている場合を除く）。関数 **foo** が呼ばれると、グローバル変数 **x** が定義され、値 1 に等しくなります。

### HyプログラムでのPythonコードの使用

Hy言語ファイルと同じディレクトリに、例えば *test.py* という名前のPythonのソース・ファイルがある場合。


    1 def factorial (n):
    2   if n < 2:
    3     return 1
    4   return n * factorial(n - 1)


このコードは、ルートソースコードファイル名であるため、**test**という名前のモジュールに含まれることになります。PythonのコードをPythonで次のようにインポートすることもできます。


    1 import test
    2 
    3 print(test.factorial(5))


で、Pythonモジュールの**test**（*test.py*で定義）をインポートするためにHyで以下を使用することができます。


    1 (import test)
    2 
    3 (print (test.factorial 5))


これをHyで対話的に実行する。


    1 $ hy
    2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
    3 => (import test)
    4 => test
    5 <module 'test' from '/Users/markw/GITHUB/hy-lisp-python/test.py'>
    6 => (print (test.factorial 5))
    7 120


PythonのBeautifulSoupライブラリ **bs4** から **BeautifulSoup** をインポートしたいだけなら、**import** 形式で指定すればよいのです。


    1 (import [bs4 [BeautifulSoup]])


### PythonプログラムでのHyライブラリの使用

Pythonのプログラムの中でHyライブラリのコードや自作のHyスクリプトをインポートして使うことについては、特別なことは何もありません。[この本のgitリポジトリ https://github.com/mark-watson/hy-lisp-python](https://github.com/mark-watson/hy-lisp-python) にある **hy-lisp-python/use_hy_in_python** というディレクトリには、後のウェブスクレイピングの章で説明・使用するコードを少し修正したサンプルHyスクリプト **get_web_page.hy** と Hy で定義されている関数を使用した短い Python スクリプト **use_hy_stuff.py** があります。

**get_web_page.hy:**


     1 (import argparse os)
     2 (import [urllib.request [Request urlopen]])
     3 
     4 (defn get-raw-data-from-web [aUri &optional [anAgent
     5                                             {"User-Agent" "HyLangBook/1.0"}]]
     6   (setv req (Request aUri :headers anAgent))
     7   (setv httpResponse (urlopen req))
     8   (setv data (.read httpResponse))
     9   data)
    10 
    11 (defn main_hy []
    12   (print (get-raw-data-from-web "http://markwatson.com")))


ここでは、2つの関数を定義しています。4-5行目で定義されているオプションの引数**anAgent**に注目してください。ここでは呼び出し側のコードが値を提供しない場合に備えてデフォルト値を提供しています。次のPythonのリストでは、最後のリストのファイルをインポートし、Pythonの呼び出し構文を使って4行目でHy関数 **main** を呼び出しています。

Hyはいったん抽象構文木（AST）にコンパイルされるとPythonと同じになります。

**hy-lisp-python/use_in_python:**


    1 import hy
    2 from get_web_page import main_hy
    3 
    4 main_hy()


HyとPythonは本当に同じものですが、構文が違うだけで、どちらの言語も簡単に並べて使えるということを理解し、感覚を養ってほしいのです。

### Pythonのslice(cut)記法をHy関数型に置き換える

Pythonでは、リストや文字列から部分列を抽出するために特別な記法を使います。


    $ python
    Python 3.7.3 (default, Mar 27 2019, 16:54:48) 
    >>> s = '0123456789'
    >>> s[2:4]
    '23'
    >>> s[-4:]
    '6789'
    >>> s[-4:-1]
    '678'
    >>> 


Hyではこうなる。


    $ hy
    hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
    => (setv s "0123456789")
    => (cut s 2 4)
    '23'
    => (cut s -4)
    '6789'
    => (cut s -4 -1)
    '678'
    => 


また、**cut** と **setv** を使って、リストを破壊的に変更することも可能です。


    => (setv x [0 1 2 3 4 5 6 7 8])
    => x
    [0, 1, 2, 3, 4, 5, 6, 7, 8]
    => (cut x 2 4)
    [2, 3]
    => (setv (cut x 2 4) [22 33])
    => x
    [0, 1, 22, 33, 4, 5, 6, 7, 8]


### 各要素のインデックスを持つリストの反復処理

ここでは、Pythonのリスト内包の一形態として**lfor**を使用することにします。


     1 => (setv sentence "The ball rolled")
     2 => (lfor i (enumerate sentence) i)
     3 [(0, 'T'), (1, 'h'), (2, 'e'), (3, ' '), (4, 'b'), (5, 'a'), (6, 'l'), (7, 'l'), (8,\
     4  ' '), (9, 'r'), (10, 'o'), (11, 'l'), (12, 'l'), (13, 'e'), (14, 'd')]
     5 => (setv vv (lfor i (enumerate sentence) i))
     6 => vv
     7 [(0, 'T'), (1, 'h'), (2, 'e'), (3, ' '), (4, 'b'), (5, 'a'), (6, 'l'), (7, 'l'), (8,\
     8  ' '), (9, 'r'), (10, 'o'), (11, 'l'), (12, 'l'), (13, 'e'), (14, 'd')]
     9 => (for [[a b] vv]
    10 ... (print a b))
    11 0 T
    12 1 h
    13 2 e
    14 3  
    15 4 b
    16 5 a
    17 6 l
    18 7 l
    19 8  
    20 9 r
    21 10 o
    22 11 l
    23 12 l
    24 13 e
    25 14 d
    26 => 


2行目の**(enumerate sentence)**という式は、文字列から一文字ずつ生成しています。リストを操作する**enumerate**は、一度に1つのリスト要素を生成する。

9行目は*destructuring*の例である。リスト**vv**の値は2つの値を持つタプル（タプルはリストに似ているが不変である、つまり、一度タプルが構築されるとそれが持つ値を変更することはできない）である。各タプルの値は、リスト **[a b]** のバインディング変数にコピーされます。代わりに以下のコードを使用することもできますが、より冗長です。


    => (for [x vv]
        (setv a (first x))
        (setv b (second x))
    ... (print a b))
    0 T
    1 h
    2 e
    3  
    4 b
     . . .
    13 e
    14 d
    => 


### フォーマットされた出力

出力をフォーマットする必要があるときは、Pythonの**format**メソッドを使用することをお勧めします。次のリプライリストでは、いくつかのフォーマットオプションを見ることができます：任意のHyデータを文字列に挿入する（3行目）、特定の幅で右寄せで値を表示する（5行目では両方の値の幅は15文字です）、特定の幅で左寄せで値を表示する（7行目）、値が表現できる文字数を制限する（9行目でオブジェクト "cat" は最初の2文字だけ、値 3.14159 は3つの数字だけで表されピリオは含まれません）。


    $ hy
    hy 0.18.0 using CPython(default) 3.7.4 on Darwin
    => (.format "first: {} second: {}" "cat" 3.14159)
    'first: cat second: 3.14159'
    => (.format "first: {:>15} second: {:>15}" "cat" 3.14159)
    'first:             cat second:         3.14159'
    => (.format "first: {:15} second: {:15}" "cat" 3.14159)
    'first: cat             second:         3.14159'
    => (.format "first: {:.2} second: {:.3}" "cat" 3.14159)
    'first: ca second: 3.14'
    => 


ここで **.format** を呼び出すと、出力ストリームに書き込むのではなく、文字列の値が返されることに注意してください。

### ラップトップ上の異なるディレクトリからライブラリをインポートする

私は通常、作業中のアプリケーションと同じディレクトリパスにないことが多い、より単純な低レベルユーティリティライブラリを最初に実装することによって、アプリケーションを書きます。ここでは、**hy-lisp-python/nlp** というディレクトリにある **nlp_lib.hy** というライブラリに **hy-lisp-python/webscraping** というディレクトリからアクセスする簡単な例を見てみましょう。


     1 Marks-MacBook:hy-lisp-python $ pwd
     2 /Users/markw/GITHUB/hy-lisp-python
     3 Marks-MacBook:hy-lisp-python $ cd webscraping 
     4 Marks-MacBook:webscraping $ hy
     5 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     6 => (import sys)
     7 => (sys.path.insert 1 "../nlp")
     8 => (import [nlp-lib [nlp]])
     9 => (nlp "President George Bush went to Mexico and he had a very good meal")
    10 {'text': 'President George Bush went to Mexico and he had a very good meal', 
    11   ...
    12  'entities': [['George Bush', 'PERSON'], ['Mexico', 'GPE']]}
    13 => (import [coref-nlp-lib [coref-nlp]])
    14 => (coref-nlp "President George Bush went to Mexico and he had a very good meal")
    15 {'corefs': 'President George Bush went to Mexico and President George Bush had a ver\
    16 y good meal',  ...  }}}
    17 => 


ここでは、Python setuptools (この本では扱いません。[ドキュメントを読む](https://setuptools.readthedocs.io)) を使ってライブラリ **nlp_lib.hy** をシステム上にライブラリとしてインストールしなかったのです。私は、ライブラリディレクトリとライブラリを使用するアプリケーションコードとの間の相対パスを頼りにしています。

6行目でPythonのシステムロードパスにライブラリディレクトリを挿入し、8行目のimport文が**nlp-lib**ライブラリを、13行目が**coref-nlp-lib**ライブラリを見つけることができるようにしています。

### クロージャの使用

関数定義は、関数の外側で定義された値を捕捉することができ、この例で見られるように、捕捉した値を変更することもできます（ディレクトリ **hy-lisp-python/misc** のファイル **closure_example.hy**）．


     1 #!/usr/bin/env hy
     2 
     3 (require [hy.contrib.walk [let]])
     4 
     5 (let [x 1]
     6   (defn increment []
     7     (setv x (+ x 1))
     8     x))
     9 
    10 (print (increment))
    11 (print (increment))
    12 (print (increment))


それが生み出すもの。


    2
    3
    4


クロージャを使うことは、オブジェクト指向プログラミングの代わりに、（クロージャの内部で定義された）1つまたはいくつかの関数だけがアクセスや変更を許されるプライベートな状態を維持するのに適していることが多いのです。この例では、**let**文は初期値を持つ複数の変数を定義することができ、これらの変数の値を用いて様々な計算を実行したり、定義した変数の値を変更するための多くの関数が定義されています。これにより、**let**文で定義された変数はlet文の外側のコードから効果的に隠蔽されますが、関数は**let**文の外側からアクセスできるようになります。

### HyはClojureに似ている。どのように似ていますか？

[Clojure](https://clojure.org/)は、JVMのための動的汎用Lisp言語です。Clojureの大きな特徴の1つは、マルチスレッドコードの記述と保守を容易にするイミュータブルデータ（作成後は読み込みのみ可能）のサポートです。

残念ながら、Clojureのイミュータブルなデータ構造はPythonでは簡単に効率的に実装できないので、Hy言語はタプルを除いてイミュータブルなデータをサポートしません。それ以外は、関数の定義、マップ/ハッシュテーブル/辞書の使用などの構文は、2つの言語で類似しています。

Hy言語の開発者であるPaul Tagliamonteは、明らかにClojureに触発されました。

Julien Danjou著の**Serious Python**という本には、Python AST（抽象構文木）についての章（第9章）、Hyの紹介、Paul Tagliamonteのインタビューが掲載されています。おすすめです

[2015年の【このポッドキャスト】(https://www.pythonpodcast.com/episode-23-hylang-core-developers/)では、Hy開発者のPaul Tagliamonte、Tuukka Turto、Morten Linderudにインタビューしています。[githubの現在のHy contributer list](https://github.com/hylang/hy/graphs/contributors)を見ることができます。

### Numpy と Matplotlib ライブラリを使ったデータのプロット

データの可視化は数値データを扱う際によく行われる作業です。後のディープラーニングの章では、**relu**と**sigmoid**という2つの関数を使うことになります。ここでは、いくつかの簡単なHy言語スクリプトを使って、これらの関数をプロットしてみましょう。

Numpyライブラリは、Pythonでいうところの「ブロードキャスト」をサポートしています。以下のREPLで定義している関数**sigmoid**では、引数として単一の浮動小数点数かNumpy配列のどちらかを渡すことができます。Numpy配列を渡した場合、関数**sigmoid**はNumpy配列の各要素に適用されます。


     1 $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (import [numpy :as np])
     4 => (import [matplotlib.pyplot :as plt])
     5 => 
     6 => (defn sigmoid [x]
     7 ...   (/ 1.0 (+ 1.0 (np.exp (- x)))))
     8 => (sigmoid 0.2)
     9 0.549833997312478
    10 => (sigmoid 2)
    11 0.8807970779778823
    12 => (np.array [-5 -2 0 2 5])
    13 array([-5, -2,  0,  2,  5])
    14 => (sigmoid (np.array [-5 -2 0 2 5]))
    15 array([0.00669285, 0.11920292, 0.5, 0.88079708, 0.99330715])
    16 => 


gitリポジトリのディレクトリ **hy-lisp-python/matplotlib** には、**sigmoid** と **relu** 関数をプロットするための2つの類似したスクリプトが含まれています。 以下は、**sigmoid**関数をプロットするスクリプトです。


     1 (import [numpy :as np])
     2 (import [matplotlib.pyplot :as plt])
     3 
     4 (defn sigmoid [x]
     5   (/ 1.0 (+ 1.0 (np.exp (- x)))))
     6 
     7 (setv X (np.linspace -8 8 50))
     8 (plt.plot X (sigmoid X))
     9 (plt.title "Sigmoid Function")
    10 (plt.ylabel "Sigmoid")
    11 (plt.xlabel "X")
    12 (plt.grid)
    13 (plt.show)


生成されたプロットはmacOSでは以下のようになります（Matplotlibは移植可能で、WindowsやLinuxでも動作します）。


![Sigmoid Function](/site_images1/hy-lisp-python/sigmoid.png)


### ボーナスポイント Hy REPLとシェルでインラインにプロットを生成するためのmacOSとITerm2用の設定

macOSのITerm2ターミナルアプリやほとんどのLinuxターミナルアプリでは、シェル（bash、zshなど）やEmacsなどでインラインでmatplotlibのプロットを取得することが可能です。これはいくつかのセットアップ作業が必要ですが、特にSSHやtmuxでリモートサーバーで作業する場合は、その価値は十分にあります。以下は、macOSでの設定です。


    1   pip3 install itermplot


.profile、.bash_profile、または .zshrc (シェルの設定による) に以下を追加してください。


    1   export MPLBACKEND="module://itermplot"


ここでは、前節の例をzshシェルで実行してみます（bash等でも可能です）。


![macOSのITermのzshシェルでmatplotlibをインラインで使用する](/site_images1/hy-lisp-python/mac-inline-matplotlib.png)


インラインプロット生成の醍醐味は、REPLを使ったインタラクティブなコーディングセッションの時に発揮されます。


![macOS上のHy REPLでmatplotlibをインラインで使用する](/site_images1/hy-lisp-python/mac-inline-matplotlib2.png)


Macラップトップを使用してリモートのLinuxサーバーにSSH接続する場合、リモートサーバーに**itermplot**をインストールし、環境変数**MPLBACKEND**を設定する必要があります。

## なぜLispなのか？

前章でHy Lisp言語の基本を学んだところで、なぜLispを使おうと思ったのかという、より広い問いかけに話を移したいと思っています。まず、私が1970年代後半に創作・研究開発のほとんどをLisp言語で行うようになり、その後、生産現場でもLisp言語を使用するようになったという個人的な経緯から始めたいと思います。

### 1970年代はウォーターフォール方式が嫌いだったが、ボトムアップ型のプログラミングスタイルが好きになった

私は1970年代半ばにUCSBで物理学の学位を取得し、100％従業員所有の会社SAICで科学プログラマーとして就職しました。 私の上司はコンピューターサイエンスの博士号を持ち、私たちのチームと所属していた組織は、トップダウンで慎重にシステムを設計し、慎重に全体像を計画し、それからコーディングする、いわゆるウォーターフォール方式を採用していました。私たち、そして業界全体が、初期の計画や設計作業で多くの時間を浪費し、システムの実装をある程度経験した後で破棄するか、大きく修正しなければならなかったのでしょう。

何がいいんだろう？私はボトムアッププログラミングが大好きになりました。新しいプロジェクトを任されたとき、私はまず低レベルの操作、つまり必要だと確信できるものに対して小さなプロシージャを書いてテストしました。そして、その機能を制御ロジックやデータへのアクセスなど、より高度なレベルに集約していきました。そして最後に、高レベルのアプリケーションを書きました。

SAICではFORTRANでコードを書き、週末のコンサルティングではAlgolを使い、Roger Guilleminの研究室で研究機器をミニコンピュータに接続する作業をしていました（彼はこの間にノーベル賞を受賞し、とてもエキサイティングでした）。FORTRANとは全く異なる言語であるAlgolを学んだことで、視野が広がりました。

「もっといいプログラミング言語が欲しい！」。また、プログラマーとしての仕事と、週に数回ある自由時間を自分の研究や人工知能（AI）学習のために有効に使うために、より生産的な方法が欲しいと思っていました。まず低レベルのライブラリやユーティリティを書き、十分にテストされた低レベルのコードの上に完全なプログラムを重ねるというボトムアップのスタイルを採用し、「より良い開発方法」を見いだしたのです。

### 最初のLisp入門

1970年代後半、会社のタイムシェアリング・コンピュータDECsystem-10にLispの実装があるのを発見しました。Lispのことは、Bertram Raphaelの著書「THE THINKING COMPUTER」を読んで知っていました。そして、昼休みの時間を利用して、会社でLispを学びたい人に週に1日だけ教える教室を開いていました。数ヶ月のLispの経験の後、私は自分のビルで働いている人でLispを学びたい人に、我々のDECsystem-10で非公式にランチタイムクラスを教える許可を得ました。

Lispは、私が好きなボトムアップ型の反復プログラミングをサポートするのに最適な言語です。

### Lispを使った商用製品の開発と展開

私の会社SAICは、1980年代初頭にAIを重要な技術として認識しました。SAICの創業者であるボブ・ベイスターと、MITで学んだLispが好きで会計を担当していたジョー・ウォークシュの2人の友人が、会社にハードウェアLispマシン（Xerox 1108）を購入するよう手配し、私はそのうちの1台を購入してもらいました。私はCharles Forgyのエキスパートシステム開発言語OPS5をXerox Lisp Machine上のInterLisp-Dで動くように移植し、これを製品として販売することに成功しました。1984年にApple Macintosh用のCoral Common Lispがリリースされると、研究開発をMacに切り替え、ExperOPS5をリリースし、これも売れました。また、Common Lispを使ってSAICのANSimニューラルネットライブラリの最初のプロトタイプを書き上げました。そのコードを製品化するためにC++に変換しました。また、IR&DプロジェクトやDARPAのNMRDプロジェクトに取り組んでいる間もLispを使い続けました。

その後、多くの開発でC++を使うようになり、McGraw-Hill社やJ. Riley社のC++の本も書きましたが、Lispは私の「思考と研究」のための言語として残りました。

### HyマクロでHy言語をプログラム中に拡張する

私の仕事では、アプリケーションタイプのプログラムを書くことが多いので、マクロを使うことはあまりありません。マクロは、Lisp言語で書かれたプログラムに許される構文を拡張するのに便利です。

私が最もよく使うマクロは、引数を評価せずに柔軟に処理することです。次の例では、未定義のシンボルを含むオブジェクトのリストを受け取るマクロ **all-to-string** を書きたいと思います。例えば、変数 **x** が未定義である場合、 **(print x 1)** を評価しようとすると、次のようなエラーが発生する。


    1     NameError: name 'x' is not defined


以下のリストは、Hy REPLでマクロ **all-to-string** を記述する実験を行ったものです。


     1 $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (list (map str ["a" 4]))
     4 ['a', '4']
     5 => (.join " " (list (map str ["a" 4])))
     6 'a 4'
     7 => (defmacro foo2 [&rest x] x)
     8 <function foo2 at 0x10b91b488>
     9 => (foo2 1 2 3)
    10 [1, 2, 3]
    11 => (foo2 1 runpuppyrun 3)
    12 Traceback (most recent call last):
    13   File "stdin-3241d1d4f129e0da87f331bfe8f9f7aba903073a", line 1, in <module>
    14     (foo2 1 runpuppyrun 3)
    15 NameError: name 'runpuppyrun' is not defined
    16 => (defmacro all-to-string [&rest x] (.join " " (list (map str x))))
    17 <function all-to-string at 0x10b91b158>
    18 => (all-to-string cater123 22)
    19 'cater123 22'
    20 => (all-to-string the boy ran to get 1 new helmet)
    21 'the boy ran to get 1 new helmet'
    22 => (all-to-string the boy "ran" to get 1 "new" helmet)
    23 'the boy ran to get 1 new helmet'
    24 => 


マクロは引数をエコーする関数を返すだけですが、引数の1つが定義されていないシンボルである場合にエラー(15行目)を投げます。16行目の2回目の試行では、関数 **str** (あらゆる引数を文字列に変換する) を引数リストにマッピングしているので、意図したとおりに動作します。

### REPL内部でボトムアップ開発を行うことはライフスタイルの選択である

Hy（あるいは他のLisp）言語を、特定の問題を解決するためにカスタム設計され構築されたもののように効果的に拡張する、ボトムアップスタイルのコーディングを好むのは、私の個人的な選択です。Lisp言語では、一旦関数やマクロが定義されると、我々の目的のためにそれはHy言語の一部となるため、これが可能なのです。例えば、データベースを利用するWebアプリケーションを書くのであれば、まず、データベースからの顧客データの作成と更新、Webアプリケーションで使用するユーティリティ関数（次の章で取り上げます）等、必要だと分かっている操作を行う低レベル関数を書くことが理にかなっていると私は思います。アプリケーションの残りの部分では、これらの新しい低レベル関数を、あたかも言語に組み込まれているように使用します。

新しい低レベルの関数を書く必要があるとき、私はREPLで始めて、関数の引数が何になるかを変数（テスト値付き）で定義します。そして、この「引数」を使って一度に一行ずつ関数のコードを書き、後でHyソース・ファイルにコピーします。REPLで結果をすぐに見ることができるので、間違いを早期に発見することができます。多くの場合、中間計算の型や値に対する誤解が原因です。このコーディングスタイルは私に合っていますし、皆さんにも気に入っていただけると思います。

## Webアプリケーションを書く

PythonはWebアプリケーションを構築するための優れたライブラリとフレームワークを持っています。ここでは、**Flask**ライブラリとフレームワークを「ボンネットの中で」使い、2つの簡単なHy言語Webアプリケーションを書きます。Pythonでの単純な「Hello World」の例から始めて、それをHyでどのように再定式化するかを見ていきます。そして、HTML生成テンプレート、セッション、クッキーを使って、次にあなたのWebサイトを訪れたときのためにユーザーのデータを保存する方法を示す、より複雑な例へと進んでいきます。後の章では、Webアプリケーションでユーザーのデータを保持するために一般的に使用されるSQLiteとPostgreSQLデータベースの使用について説明します。このパターンでは、ユーザーをログインさせ、そのユーザーに固有のトークンをウェブブラウザーのクッキーに保存します。原理的には、Web ブラウザのクッキーでも同じことができますが、ユーザーが別のブラウザやデバイスで Web サイトを訪れた場合、以前の訪問でクッキーに保存されたデータにアクセスすることはできません。

私は軽量のウェブフレームワークが好きです。RubyではSinatraを、HaskellではSpockを、そしてJavaのWebアプリケーションを作るときはJSPのような軽量なツールを好んで使っていました。Flaskはシンプルだけど高機能で、Hyから使うのは生産的で楽しいです。軽量なフレームワークを使うことに加えて、私はできるだけシンプルな方法でWebアプリをデプロイするのが好きです。HerokuとGoogle Cloud PlatformのAppEngineプラットフォームの使い方を説明して、この章を締めくくります。

### Flask入門：HyでPythonのデコレータを使おう

まず、Flaskをインストールする必要があります。


    pip install flask


Pythonのコードをアノテーションに置き換えるために、Hyマクロ **with-decorator** を使用します。ここでは **@app.route** というデコレータを使って、URIパターンとPythonのコールバック関数を対応させています。以下のケースでは、Webアプリのインデックスページにアクセスしたときの振る舞いを定義しています。


    1 from flask import Flask
    2 
    3 @app.route('/')
    4   def index():
    5      return "Hello World !")
    6 
    7 app.run()


私が初めてFlaskをHy言語で使ったのは、HNユーザーの「volent」さんが投稿した、上記のPythonコードと機能的に同等な、ディレクトリ **hy-lisp-python/webapp** 内のファイル **flask_test.hy** を見たのがきっかけです。


     1 #!/usr/bin/env hy
     2 
     3 ;; snippet by HN user volent:
     4 
     5 (import [flask [Flask]])
     6 
     7 (setv app (Flask "Flask test"))
     8 (with-decorator (app.route "/")
     9   (defn index []
    10     "Hello World !"))
    11 (app.run)


Hyのマクロ **with-decorator** は、HyアプリケーションでPythonスタイルのデコレータを使用するために使われます。

私はこの例が好きで、このコードを試した後、HyとFlaskを使い始めました。この例を実行して、Flaskで正しくセットアップされていることを確認してみてください。


    (base) Marks-MacBook:webapp $ ./flask_test.hy 
     * Serving Flask app "Flask test" (lazy loading)
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)


ウェブブラウザで<http://127.0.0.1:5000/>を開きます。


![Hello world Flask web app](/site_images1/hy-lisp-python/flask1.jpg)


### Jinja2 Templates を使って HTML を生成する

[Jinja2](https://pypi.org/project/Jinja2/)は、HTMLのマークアップにPythonの変数参照や簡単なPythonループなどを付加できるテンプレート・システムです。アプリケーション変数の値はコンテキストに格納することができ、HTMLテンプレートは、ユーザーのWebブラウザにHTML応答を返す前に、変数の値が現在の値に置き換えられます。

デフォルトでは、Jinja2 テンプレートは **templates** という名前のサブディレクトリに保存されます。この例のテンプレートは、ここに示すファイル **hy-lisp-python/webapp/templates/template1.j2** の中にあります。


     1 <html>
     2   <head>
     3     <title>Testing Jinja2 and Flask with the Hy language</title>
     4   </head>
     5   <body>
     6      {% if name %}
     7        <h1>Hello {{name}}</h1>
     8      {% else %}
     9        <h1>Hey, please enter your name!</h1>
    10      {% endif %}
    11     
    12     <form method="POST" action="/response">
    13       Name: <input type="text" name="name" required>
    14       <input type="submit" value="Submit">
    15     </form>
    16   </body>
    17 </html>


6行目では、変数 **name** が現在のアプリの実行コンテキストで定義されているかどうかをチェックするために Python の **if** 式を使用していることに注意してください。

実行中のFlaskアプリのコンテキストでは、以下のようにすると、変数 **name** が **None** として定義された状態で上記のテンプレートがレンダリングされます。


    1 (render_template "template1.j2")


テンプレート内で使用される変数に対して、名前付きパラメータとして値を設定することができる、といった具合です。


    1 (render_template "template1.j2" :name "Mark")


私は、あなたがHTMLの基礎やHTTPリクエストにおけるGETとPOSTの操作を理解していると仮定しています。

以下のFlask Webアプリでは、変数 **name** を設定せずにテンプレートをレンダリングする動作と、HTMLフォームに入力された名前をPOSTレスポンスハンドラに渡すHTML POSTハンドラを定義しています。


     1 #!/usr/bin/env hy
     2 
     3 (import [flask [Flask render_template request]])
     4 
     5 (setv app (Flask "Flask and Jinja2 test"))
     6 
     7 (with-decorator (app.route "/")
     8   (defn index []
     9     (render_template "template1.j2")))
    10 
    11 (with-decorator (app.route "/response" :methods ["POST"])
    12   (defn response []
    13     (setv name (request.form.get "name"))
    14     (print name)
    15     (render_template "template1.j2" :name name)))
    16 
    17 (app.run)


関数 **index** と **response** は **a123** や **b17** のような任意の名前を持つことができます。関数名**index**と**response**は、関数が何を行うかを説明するのに役立つので、私はこの関数名を使いました。

ウェブブラウザで<http://127.0.0.1:5000/>を開いてください。


![Flask web app using a Jinja2 Template](/site_images1/hy-lisp-python/flask2.jpg)



![Flask web app using a Jinja2 Template after entering my name and submitting the HTML input form](/site_images1/hy-lisp-python/flask3.jpg)


### HTTP セッションとクッキーの扱い

Flask は Flask の Web アプリの各クライアントに対して、特別な変数 **session** を保持しています。Webアプリを利用する異なる人々は、独立したセッションを持つことになります。Webアプリでは、与えられたユーザーのセッションを辞書として扱うことで、セッションの値を設定することができます。


    => (setv (get session "name") "Mark")
    => session
    {'name': 'Mark'}


Jinja2のテンプレート内部では、テンプレートから生成されるHTMLにセッション変数の値を配置するために、簡単なPython式を使用することができます。


    {{ session['name'] }}


ウェブアプリでは、次の方法でセッションにアクセスできます。


    (get session "name")


名前付きクッキーの値を設定するためには、次のようにします。


    1 (import [flask [Flask render_template request make_response]])
    2 
    3 (with-decorator (app.route "/response" :methods ["POST"])
    4   (defn response []
    5     (setv name (request.form.get "name"))
    6     (setv resp (make_reponse (render_template "template1.j2" :name name)))
    7     (resp.set_cookie "name" name)
    8     resp))


名前付きCookieの値は、以下の方法で取得することができます。


    (request.cookies.get "name")


内側で、**with-decorator** フォームを使用します。HTTPリクエストを処理するとき、**request**の値はFlaskによって実行コンテキストで定義されます。 以下は、*cookie_test.hy* というファイルにあるクッキーを扱う完全な例です。


     1 #!/usr/bin/env hy
     2 
     3 (import [flask [Flask render_template request make-response]])
     4 
     5 (setv app (Flask "Flask and Jinja2 test"))
     6 
     7 (with-decorator (app.route "/")
     8   (defn index []
     9     (setv cookie-data (request.cookies.get "hy-cookie"))
    10     (print "cookie-data:" cookie-data)
    11     (setv a-response (render_template "template1.j2" :name cookie-data))
    12     a-response))
    13 
    14 (with-decorator (app.route "/response" :methods ["POST"])
    15   (defn response []
    16     (setv name (request.form.get "name"))
    17     (print name)
    18     (setv a-response (make-response (render-template "template1.j2" :name name)))
    19     (a-response.set-cookie "hy-cookie" name)
    20     a-response))
    21 
    22 (app.run)


このサンプルをそのまま実行するだけでなく、テンプレートを変更してみたり、一般的にコードを実験してみることをお勧めします。簡単なコード変更であっても、コードをより深く理解するのに役立ちます。

### Hy言語FlaskアプリをGoogle Cloud Platform AppEngineにデプロイする

このセクションの例は[別のgithubリポジトリ](https://github.com/mark-watson/hy-lisp-gcp-starter-project)にあります。AppEngineにデプロイするつもりなら、スタータープロジェクトとしてクローンするか、新しいプロジェクトにコピーすべきです。

このAppEngineの例は、静的アセットを提供することと、Hy言語ライブラリをロードしてHy言語コードをインポートするための小さなPythonスタブメインプログラムを持っていること以外は、前節の例と非常によく似ています。

以下はそのPythonスタブ・メイン・プログラムです。


    1 import hy
    2 import flask_test
    3 from flask_test import app
    4 
    5 if __name__ == '__main__':
    6     # Used when running locally only. When deploying to Google App
    7     # Engine, a webserver process such as Gunicorn will serve the app.
    8     app.run(host='localhost', port=9090, debug=True)


Hyアプリは、前のセクションで見たのとは少し違います。6行目で静的アセットの場所を指定していますし、**app**オブジェクトの**run()**メソッドを呼び出してはいません。


     1 (import [flask [Flask render_template request]])
     2 (import os)
     3 
     4 (setv port (int (os.environ.get "PORT" 5000)))
     5 
     6 (setv app (Flask "Flask test" :static_folder "./static" :static_url_path "/static"))
     7 
     8 (with-decorator (app.route "/")
     9   (defn index []
    10     (render_template "template1.j2")))
    11 
    12 (with-decorator (app.route "/response" :methods ["POST"])
    13   (defn response []
    14     (setv name (request.form.get "name"))
    15     (render_template "template1.j2" :name name)))


GCPの使用経験があり、以下をお持ちの方を想定しています。

- GCPコマンドラインツールがインストールされている。
- GCPのAppEngineコンソールで、hy-gcp-testのような名前の新しいプロジェクトを作成しました（すでに使われている名前を選ぶと、警告が出ます）。

このプロジェクトをクローンするなどしてコピーした後、コマンドラインツールを使ってFlaskアプリをデプロイしてテストします。


    gcloud auth login
    gcloud config set project hy-gcp-test
    gcloud app deploy
    gcloud app browse


問題が発生した場合は、ログを見てください。


    gcloud app logs tail -s default


ローカルで変更を編集し、ローカルでテストすることができます。


    python main.py


変更があれば、再度デプロイしてテストすることができます。


    gcloud app deploy


デプロイするたびに、新しいインスタンスが作成されることに注意してください。GCP AppEngineコンソールを使って古いインスタンスを削除し、終了したらすべてのインスタンスを削除することをお勧めします。

#### 今後の展開

この例のコピーを作り、github repoを作成し、AppEngine上でHy言語アプリケーションを作成する第一歩として、上記の指示に従えばよいでしょう。Google Cloud Platformには、アプリで使える（Hyプログラムから呼び出されるPython APIを使う）以下のようなサービスがたくさんあります。

- ストレージとデータベース。
- ビッグデータ
- 機械学習

### Hy言語FlaskアプリをHerokuプラットフォームへデプロイする

このセクションのサンプルは[別のgithubリポジトリ](https://github.com/mark-watson/hy-lisp-heroku-starter-project)にあります。Herokuプラットフォームにデプロイするつもりなら、cloneするか他の方法でスタータープロジェクトとして使用すべきです。

Python のスタブプログラム **wsgi.python** を使って、Flask アプリが Heroku が使っている WSGI インターフェースで動作するようにします。


    import hy
    import flask_test
    from flask_test import app


Herokuプラットフォームは、このプロジェクトのHeroku **Proc** ファイルの設定により、インポートされた **app** オブジェクトの **run()** メソッドを呼び出すでしょう。


    web: gunicorn 'wsgi:app' --log-file -


ここでは、本番環境に適した **gunicorn** サーバーが、 **wsgi** モジュール（ここではモジュール名は Python WSGI ハンドラファイルのプレフィックス名）で定義されている **app** オブジェクトの **run()** メソッドを呼び出すように Heroku プラットフォームに指定しています。

Hy Flaskアプリは、以前の例から少し変更されています。すべての変更は3行目にあります。


     1 (import [flask [Flask render_template request]])
     2 
     3 (setv app (Flask "Flask test" :static_folder "./static" :static_url_path "/"))
     4 
     5 (with-decorator (app.route "/")
     6   (defn index []
     7     (render_template "template1.j2")))
     8 
     9 (with-decorator (app.route "/response" :methods ["POST"])
    10   (defn response []
    11     (setv name (request.form.get "name"))
    12     (print name)
    13     (render_template "template1.j2" :name name)))


Herokuのコマンドラインツールをインストールする必要があります。

<https://devcenter.heroku.com/categories/command-line>

このレポをチェックアウトした後、このディレクトリから以下を実行します。


    heroku login
    heroku create
    git push heroku master


Heroku アカウントを設定していれば、これらのコマンドでこのサンプルをデプロイできます。

アプリケーションのHerokuログファイルを見るには、次のコマンドを使います。


    heroku logs --tail


このHello Worldアプリは、デフォルトのWebブラウザーを使って開くことができます。


    heroku open


デフォルトでは、Hello Worldアプリは無料のHerokuモードで実行されます。それでも終了したら削除する必要があります。

- ログイン先: https://dashboard.heroku.com/apps
- アプリケーション名をクリックします。
- 設定タブをクリックします。
- ページの一番下までスクロールして、アプリを削除するオプションを使用します。

#### 今後について

この例のコピーを作成し、github リポジトリを作成し、上記の指示に従うことができます。

Herokuのセットアップをローカルでテストしたり、開発用に使うには


    heroku local


Herokuプラットフォームは、サポートするサービスが多岐にわたり、[データサービス](https://www.heroku.com/managed-data-services) や [Heroku and third party addons](https://elements.heroku.com/addons) などの多くのサードパーティ製サービスも含まれています。

### まとめ

私は、シンプルなものをシンプルに、多くの儀式なしに実装できることが好きです。これらの例を通して作業すれば、HyやFlaskベースのWebアプリを素早く、必要最低限のコードで生成できることを実感してもらえると思います。

ボトムアップ・プログラミングのテーマに戻ると、短い低レベルのユーティリティ関数から始めて、それらを別のファイルに配置することで、再利用が簡単になり、将来の同様のプロジェクトがさらに容易になることがわかります。 私は、それぞれの言語について、有用なコードの断片や短いユーティリティを集め、別のファイルに保存しています。コードを書くときは、Webで検索する前に、低レベルの機能を実装するために使用している言語のsnippetsディレクトリを探し始めます。Common Lispで作業するときは、小さなライブラリに書いた低レベルのコードをすべてQuicklispのソースルートディレクトリに格納し、PythonとHyではPythonの**setuptools**ライブラリを使ってライブラリを生成し、ラップトップにグローバルインストールし、簡単に再利用できるようにしています。将来の再利用のために自分の仕事を整理することは、多少の努力に値します。

## 責任あるウェブスクレイピング

この章のタイトルに「責任ある」という言葉を入れたのは、（すぐに分かるように）Webサイトからデータを引き出すのが簡単だからといって、Webサイトの所有者の財産権を尊重し、その使用条件を守ることが重要であることを思い出してほしいからです。この[Wikipediaのフェアユースに関する記事](https://en.wikipedia.org/wiki/Fair_use)は、著作権物の使用に関する良い概要を提供しています。

ここで開発するWebスクレイピングコードは、PythonのBeautifulSoupとURIライブラリを使用しています。

私の仕事や研究では、自然言語処理のためのテキストデータを収集するためにWebスクレイピングを使用することに最も興味を持っていますが、他の一般的なアプリケーションには、AIニュース収集や要約アシスタントを書いたり、2000年と2001年に株式会社ウェブマインドで行った、ソーシャルメディア上のコメントに基づいて株価を予測しようとしたり、などということがあります。

### Hy言語でのPython BeautifulSoupライブラリの使用

HTMLテキストを解析して、構造（見出し、太字のものなど）と埋め込まれた生のテキストの両方を抽出するための良いライブラリがたくさんあります。 私はPythonのBeautiful Soupライブラリが特に好きなので、ここでそれを使うことにします。

ファイル **get_web_page.hy** の以下のリストの4行目では、デフォルトのユーザーエージェントを説明的な文字列 "HyLangBook" に設定していますが、いくつかのWebサイトでは、FirefoxやChromeブラウザ（iOS、Android、Windows、Linux、macOS）として表示するようにこれを設定する必要があるかもしれません。関数 **get-raw-data** は、Webサイトの全コンテンツを1つの文字列値として取得します。


    1 (import [urllib.request [Request urlopen]])
    2 
    3 (defn get-raw-data-from-web [aUri
    4                              &optional [anAgent {"User-Agent" "HyLangBook/1.0"}]]
    5   (setv req (Request aUri :headers anAgent))
    6   (setv httpResponse (urlopen req))
    7   (setv data (.read httpResponse))
    8   data)


この関数をREPLでテストしてみましょう。


     1 $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (import [get-page-data [get-raw-data-from-web]])
     4 => (get-raw-data-from-web "http://knowledgebooks.com")
     5 b'<!DOCTYPE html><html><head><title>KnowledgeBooks.com - research on the Knowledge M\
     6 anagement, and the Semantic Web ...'
     7 => 
     8 => (import [get-page-data [get-page-html-elements]])
     9 => (get-page-html-elements "http://knowledgebooks.com")
    10 {'title': [<title>KnowledgeBooks.com - research on the Knowledge Management, and the\
    11  Semantic Web </title>],
    12 'a': [<a class="brand" href="#">KnowledgeBooks.com  </a>,  ...
    13 => 


このREPLセッションでは、前のリストで定義した関数 **get-raw-data-from-web** がウェブページを文字列として返すことを示しています。9行目では、関数 **get-page-html-elements** を使用して、HTMLを含む文字列内のすべての要素を検索しています。この関数は次のリストで定義されており、ウェブページの文字列コンテンツをパースして処理する方法を示しています。注意：この例では、**lxml**ライブラリをインストールする必要があります（Pythonの構成に応じてpipまたはpip3を使用します）。


    1 pip install lxml


次のファイル **get_page_data.hy** のリストは、Beautiful Soup ライブラリを使用して、Web サイトから HTML テキストの文字列データをパースしています。関数 **get-page-html-elements** は、文字列として表された HTML の各要素の名前と関連データを返します (20-24 行目の余分なコードは単なるデバッグ用のサンプルコードです)。


     1 (import [get_web_page [get-raw-data-from-web]])
     2 
     3 (import [bs4 [BeautifulSoup]])
     4 
     5 (defn get-element-data [anElement]
     6   {"text" (.getText anElement)
     7    "name" (. anElement name)
     8    "class" (.get anElement "class")
     9    "href" (.get anElement "href")})
    10 
    11 (defn get-page-html-elements [aUri]
    12   (setv raw-data (get-raw-data-from-web aUri))
    13   (setv soup (BeautifulSoup raw-data "lxml"))
    14   (setv title (.find_all soup "title"))
    15   (setv a (.find_all soup "a"))
    16   (setv h1 (.find_all soup "h1"))
    17   (setv h2 (.find_all soup "h2"))
    18   {"title" title "a" a "h1" h1 "h2" h2})
    19 
    20 (setv elements (get-page-html-elements "http://markwatson.com"))
    21 
    22 (print (get elements "a"))
    23 
    24 (for [ta (get elements "a")] (print (get-element-data ta)))


5-9行目で定義されている関数 **get-element-data** は、引数として (Beautiful soup library で定義されているような) HTML 要素オブジェクトを受け取り、もし利用可能なら text, name, class, および href 値のデータを抽出します。11-18行目で定義された関数 **get-page-html-elements** は、URIを含む文字列を引数として受け取り、入力URIによって指されるWebページ内のすべての **a**, **h1**, **h2**, **title** 要素のリストを含む辞書（またはマップ、またはハッシュテーブル）を返します。必要に応じて、追加のHTML要素タイプを追加するために**get-page-html-elements**を修正することができます。

以下はその出力である（簡潔にするために多くの行を削除してある）。


     1 {'text': 'Mark Watson artificial intelligence consultant and author',
     2  'name': 'a', 'class': ['navbar-brand'], 'href': '#'}
     3 {'text': 'Home page', 'name': 'a', 'class': None, 'href': '/'}
     4 {'text': 'My Blog', 'name': 'a', 'class': None,
     5  'href': 'https://mark-watson.blogspot.com'}
     6 {'text': 'GitHub', 'name': 'a', 'class': None,
     7  'href': 'https://github.com/mark-watson'}
     8 {'text': 'Twitter', 'name': 'a', 'class': None, 'href': 'https://twitter.com/mark_l_\
     9 watson'}
    10 {'text': 'WikiData', 'name': 'a', 'class': None, 'href': 'https://www.wikidata.org/w\
    11 iki/Q18670263'}


### DemocracyNow.orgニュースのWebサイトからHTMLリンクを取得する

私はNPR.orgとDemocracyNow.orgのニュースを主なニュース源として財政的に支援し、信頼しているので、ここと次の節でそのニュースサイトを例に挙げて説明します。Webサイトの形式は実にさまざまなので、個々のWebサイト用に高度にカスタマイズしたWebスクレイパーを作成し、サイトの形式が変わってもWebスクレイピングコードを維持する必要があることがよくあります。

この例や次のセクションの例で作業する前に、データを取得するために **Makefile** ファイルを使用してください。


    make data


これで、両方のウェブサイトのホームページがファイルにコピーされるはずです。

- democracynow_home_page.html (ここで使用)
- npr_home_page.html (次のセクションの例で使用します)

次のリストは、**democracynow_front_page.hy**を示しています。


     1 #!/usr/bin/env hy
     2 
     3 (import [get-web-page [get-web-page-from-disk]])
     4 (import [bs4 [BeautifulSoup]])
     5 
     6 ;; you need to run 'make data' to fetch sample HTML data for dev and testing
     7 
     8 (defn get-democracy-now-links []
     9   (setv test-html (get-web-page-from-disk "democracynow_home_page.html"))
    10   (setv bs (BeautifulSoup test-html :features "lxml"))
    11   (setv all-anchor-elements (.findAll bs "a"))
    12   (lfor e all-anchor-elements
    13           :if (> (len (.get-text e)) 0)
    14           (, (.get e "href") (.get-text e))))
    15 
    16 (if (= __name__ "__main__")
    17   (for [[uri text] (get-democracy-now-links)]
    18     (print uri ":" text)))


これは、ホームページの各リンクの URI とテキスト (文字列 ":" で区切られています) を表示するだけです。13行目では、テキストを含まないアンカー要素をすべて破棄しています。14行目では、戻り値のリストの最初にあるカンマ文字が、タプルを構築していることを表しています。16-18行目では、このファイルをコマンドラインで実行するときに使用されるメイン関数を定義しています。これは、Pythonでライブラリファイルをコマンドラインツールとして実行するためにメイン関数を定義する方法と似ています。

今日のフロントページの出力は数行です。


    /2020/1/7/the_great_hack_cambridge_analytica : Meet Brittany Kaiser, Cambridge Analy\
    tica Whistleblower Releasing Troves of New Files from Data Firm
    /2019/11/8/remembering_orangeburg_massacre_1968_south_carolina : Remembering the 196\
    8 Orangeburg Massacre When Police Shot Dead Three Unarmed Black Students
    /2020/1/15/democratic_debate_higher_education_universal_programs : Democrats Debate \
    Wealth Tax, Free Public College & Student Debt Relief as Part of New Economic Plan
    /2020/1/14/dahlia_lithwick_impeachment : GOP Debate on Impeachment Witnesses Intensi\
    fies as Pelosi Prepares to Send Articles to Senate
    /2020/1/14/oakland_california_moms_4_housing : Moms 4 Housing: Meet the Oakland Moth\
    ers Facing Eviction After Two Months Occupying Vacant House
    /2020/1/14/luis_garden_acosta_martin_espada : “Morir Soñando”: Martín Espada Reads P\
    oem About Luis Garden Acosta, Young Lord & Community Activist


URIはルートURI（https://www.democracynow.org/）からの相対値です。

### NPR.orgのニュースウェブサイトからフロントページの要約を取得する

この例は、ホームページのリンクからのテキストが毎日のニュースの要約を提供するためにフォーマットされていることを除いて、最後のセクションの例と似ています。 最後のセクションの例を実行したため、Webサイトのホームページがローカルファイルにコピーされていることを想定しています。

次のリストは、**npr_front_page_summary.hy** を示しています。


     1 #!/usr/bin/env hy
     2 
     3 (import [get-web-page [get-web-page-from-disk]])
     4 (import [bs4 [BeautifulSoup]])
     5 
     6 ;; you need to run 'make data' to fetch sample HTML data for dev and testing
     7 
     8 (defn get-npr-links []
     9   (setv test-html (get-web-page-from-disk "npr_home_page.html"))
    10   (setv bs (BeautifulSoup test-html :features "lxml"))
    11   (setv all-anchor-elements (.findAll bs "a"))
    12   (setv filtered-a
    13     (lfor e all-anchor-elements
    14           :if (> (len (.get-text e)) 0)
    15           (, (.get e "href") (.get-text e))))
    16   filtered-a)
    17 
    18 (defn create-npr-summary []
    19   (setv links (get-npr-links))
    20   (setv filtered-links (lfor [uri text] links :if (> (len (.strip text)) 40) (.strip\
    21  text)))
    22   (.join "\n\n" filtered-links))
    23 
    24 (if (= __name__ "__main__")
    25   (print (create-npr-summary)))


12～15行目では、テキストを含まないすべてのアンカーHTML要素をフィルタリングしています（または削除しています）。以下は、今日収集したデータに対して生成された出力の数行を示しています。


    January 16, 2020  Birds change the shape of their wings far more than
    planes. The complexities of bird flight have posed a major design challenge
    for scientists trying to translate the way birds fly into robots.

    FBI Vows To Warn More Election Officials If Discovering A Cyberattack

    January 16, 2020  The bureau was faulted after the Russian attack on the
    2016 election for keeping too much information from state and local
    authorities. It says it'll use a new policy going forward.

    Ukraine Is Investigating Whether U.S. Ambassador Yovanovitch Was Surveilled

    January 16, 2020  Ukraine's Internal Affairs Ministry says it's asking the
    FBI to help determine whether international laws were broken, or "whether it
    is just a bravado and a fake information" from a U.S. politician.

    Electric Burn: Those Who Bet Against Elon Musk And Tesla Are Paying A Big Price

    January 16, 2020  For years, Elon Musk skeptics have shorted Tesla stock, confident \
    the electric carmaker was on the brink of disaster. Instead, share value has skyrock\
    eted, costing short sellers billions.

    TSA Says It Seized A Record Number Of Firearms At U.S. Airports Last Year


ここで見た例は簡単なものですが、ウェブからテキストデータを収集するのに十分なものでしょう。

## Microsoft Bing Search API の使用方法

この章の資料を使用するには、Microsoft の Azure 検索サービスに登録する必要があります。皆さんは、検索を人間中心の手作業と捉えているのではないでしょうか。検索を自動化するアプリケーション、Web上の情報を見つけるアプリケーション、情報を自動的に整理するアプリケーションを検討することで、考えを広げていただければと思います。

### Microsoft Bing Search APIs のアクセスキーの取得について

Azureのアカウントが必要です。私はBing検索APIを研究でかなり頻繁に使っていますが、月に1ドル程度以上使ったことはなく、通常は全く請求が来ません。個人で使う分には、安価なサービスだと思います。

ウェブページ<https://azure.microsoft.com/en-us/try/cognitive-services/>にアクセスし、アクセスキーにサインアップすることで始められます。Search APIsのサインアップは、現在このウェブフォームの4番目のタブにあります。Search APIsタブに移動したら、Bing Search APIs v7というオプションを選択します。 APIキーが発行されるので、すぐに必要となる環境変数に保存します。


    export BING_SEARCH_V7_SUBSCRIPTION_KEY=4e97234341d9891191c772b7371ad5b1


これは私の本当のサブスクリプションキーではありません!

これを **.profile** ファイル (または **.zshrc** や **.bashrc** など) に追加した後、新しいターミナル ウィンドウを開き、次のように動作することを確認してください。


    $ hy
    hy 0.18.0 using CPython(default) 3.7.4 on Darwin
    => (import os)
    => (get os.environ "BING_SEARCH_V7_SUBSCRIPTION_KEY")
    '4e97234341d9891191c772b7371ad5b1'
    => 


### 検索スクリプトの例

Bing検索APIにアクセスするために必要なHyコードはごくわずかです。ここでは、検索語を含む文字列である単一のコマンドライン引数を期待する、長いスクリプト例を見ていきます。次のサンプル・スクリプトは、JSON形式の検索結果を要求する検索クエリを作る方法を示しています。また、返されたJSONデータの構文解析についても見ています。このリストは、ページの幅に合うようにフォーマットしています。


    #!/usr/bin/env hy

    (import json)
    (import os)
    (import sys)
    (import [pprint [pprint]])
    (import requests)

    ;; Add your Bing Search V7 subscription key and 
    ;; the endpoint to your environment variables.
    (setv subscription_key (get os.environ "BING_SEARCH_V7_SUBSCRIPTION_KEY"))
    (setv endpoint "https://api.cognitive.microsoft.com/bing/v7.0/search")

    ;; Query term(s) to search for. 
    (setv query (get sys.argv 1)) ;; an example: "site:wikidata.org Sedona Arizona"

    ;; Construct a request
    (setv mkt "en-US")
    (setv params { "q" query "mkt" mkt })
    (setv headers { "Ocp-Apim-Subscription-Key" subscription_key })

    ;; Call the API
    (setv response (requests.get endpoint :headers headers :params params))

    (print "\nFull JSON response from Bing search query:\n")
    (pprint (response.json))

    ;; pull out resuts and print them individually:

    (setv results (get (response.json) "webPages"))

    (print "\nResults from the key 'webPages':\n")
    (pprint results)

    (print "\nDetailed printout from the first search result:\n")

    (setv result-list (get results "value"))
    (setv first-result (first result-list))

    (print "\nFirst result, all data:\n")
    (pprint first-result)

    (print "\nSummary of first search result:\n")
    (pprint (get first-result "displayUrl"))

    (if (in "displayUrl" first-result)
        (print
         (.format
           " key: {:15} \t:\t {}" "displayUrl"
           (get first-result "displayUrl"))))
    (if (in "language" first-result)
        (print
          (.format " key: {:15} \t:\t {}" "language" 
          (get first-result "language"))))
    (if (in "name" first-result)
        (print 
          (.format 
            " key: {:15} \t:\t {}" "name" 
            (get first-result "name"))))


「site:wikidata.org」のような検索ヒントを使用すると、特定のWebサイトのみを検索することができます。次の例では、検索クエリを使用しています。


    1 "site:wikidata.org Sedona Arizona"


この例では364行の出力を生成しているので、ここでは選択された数行のみを表示します。


    $ ./bing.hy "site:wikidata.org Sedona Arizona" | wc -l
         364
    $ ./bing.hy "site:wikidata.org Sedona Arizona"

    Full JSON response from Bing search query:

    {'_type': 'SearchResponse',
     'queryContext': {'originalQuery': 'site:wikidata.org Sedona Arizona'},
     
       ...

    Results from the key 'webPages':

    {'totalEstimatedMatches': 27,
     'value': [{'about': [{'name': 'Sedona'}, {'name': 'Sedona'}],
                'dateLastCrawled': '2020-05-24T00:04:00.0000000Z',
                'displayUrl': 'https://www.wikidata.org/wiki/Q80041',
        ...

    Summary of first search result:

    'https://www.wikidata.org/wiki/Q80041'
     key: displayUrl       :   https://www.wikidata.org/wiki/Q80041
     key: language         :   en
     key: name             :   Sedona - Wikidata


### まとめ

個人的な研究のためにデータを取得するために自動化されたウェブスクレイピングを使うことに加えて、私は自動化されたウェブ検索をよく利用します。MicrosoftのAzure Bingの検索APIが一番使い勝手がよく、自分が使うサービスにはお金を払うのが好きなんです。検索エンジンのDuck Duck Goも無料の検索APIを提供していますが、手動でのウェブ検索の90%はDuck Duck Goを使っていますが、自動化システムを構築するときは、お金を払うサービスに依存する方が好きなんです。

## ディープラーニング

2014年以降の私の専門的なキャリアのほとんどは、Keras APIを使用したTensorFlowによるDeep Learningに関係しています。1980年代後半には、DARPAのニューラルネットワーク技術諮問委員会に1年間参加し、SAIC ANSimニューラルネットワークライブラリの商用製品の最初のプロトタイプを書き、私の会社がFAAのために設計・製作した空港に配備する爆弾検知器のためのニューラルネットワーク予測コードを書きました。最近では、GAN（generative adversarial networks）モデルを表計算データの合成に、LSTM（long short term memory）モデルをネストしたJSONなどの高度な構造化テキストデータの合成やNLP（自然言語処理）に使用しています。私は、ニューラルネットワークやディープラーニングの技術を用いた米国特許を55件、欧州特許を数件保有しています。

ここで開発しているHy言語ユーティリティやサンプルプログラムは、すべてTensorFlowとKerasを「ボンネットの中」で使って、重い仕事をこなしています。KerasはTensorFlowのAPIとしてよりシンプルに使えるので、私は通常、低レベルのTensorFlow APIではなくKerasを使用しています。

TensorFlowやKeras以外にも、興味を引くようなライブラリやフレームワークがあります。私は特にプログラミング言語JuliaのFluxライブラリが好きです。現在、PythonはDeep Learningのための最も包括的なライブラリを持っていますが、JuliaやSwiftのような微分計算（これについては後述）をサポートする他の言語が将来的に人気を博すかもしれません。

ここでは、Deep Learningのニューラルネットワークモデルを議論するための語彙を学び、可能なアーキテクチャを見て、KerasをHy言語で使用することに慣れるのに十分な2つのHy言語の例を紹介します。すでにDeep Learningアプリケーションの開発経験がある場合は、以下の復習資料をスキップして、Hy言語の例題に飛ぶとよいでしょう。

Deep Learningを専門的に使用したい場合、私が推奨する2つの特定のオンラインリソースがあります。Andrew Ngは[deeplearning.ai](https://www.deeplearning.ai/)で、Jeremy Howardは[fast.ai](https://www.fast.ai/)で活動をリードしている。ここでは、いくつかの便利なテクニックの使い方を紹介します。AndrewとJeremyは、彼らのコースを受講すれば、プロフェッショナルなレベルにつながるかもしれないスキルを教えてくれるでしょう。

現在実用化されているDeep Learningのニューラルアーキテクチャはたくさんありますが、私が使っているのは数種類です。

- 多くの完全連結層を持つ多層パーセプトロンネットワーク。入力層は入力データのプレースホルダーを含む。入力層の各要素は2次元の重み行列によって第1隠れ層の各要素に接続される。任意の数の完全連結隠れ層を使用することができ、最後の隠れ層は出力層に接続される。
- 画像処理とテキスト分類のための畳み込みネットワーク。 畳み込み、またはフィルタは、入力画像（フィルタは2次元）またはテキストなどのシーケンス（フィルタは1次元）を処理できる小さなウィンドウである。各フィルターは、入力画像や入力シーケンスのどこにフィルターを適用するかに関係なく、1組の学習された重みを使用する。
- オートエンコーダーは同じ数の入力層と出力層の要素を持ち、1つ以上の隠れ完全連結層がある。 オートエンコーダーは、比較的少数の隠れ層要素を用いて、学習用入力値と同じ出力を生成するように学習される。オートエンコーダーは、入力データに含まれるノイズを除去することができる。
- LSTM（long short term memory）は、配列内の要素を順番に処理し、配列の中で先に見たパターンを記憶することができる。
-   GAN (Generative adversarial networks) モデルは、生成器と識別器という2つの異なる競合するニューラル・モデルから構成される。 GANは多くの場合、入力画像に対して学習される（ただし、私の仕事では、GANを2次元の数値スプレッドシートデータに適用したことがある）。生成器モデルは「潜在的な入力ベクトル」（これはランダムな値を持つ特定の大きさのベクトルである）を入力とし、ランダムな出力画像を生成する。生成器モデルの重みは、学習画像の見え方に近いランダムな画像を生成するように学習される。 識別器モデルは、任意の出力画像が元の学習データなのか、生成器モデルによって生成された画像なのかを認識するように学習させる。生成器モデルと識別器モデルは一緒に学習される。

TensorFlowのようなライブラリの中核機能はC++で書かれており、GPU、カスタムASIC、GoogleのTPUのようなデバイスといった特殊なハードウェアを活用する。Deep Learningモデルを扱うほとんどの人は、Deep Learningモデルの学習と使用をより効率的にするために使用される低レベルの最適化について意識する必要すらありません。とはいえ、次のセクションでは、シンプルなニューラルネットワークがどのようにトレーニングされ、使用されているかを紹介するつもりです。

### 簡単な多層パーセプトロンのニューラルネットワーク

私は、多層パーセプトロンネットワーク、バックプロパゲーションニューラルネットワーク、デルタルールネットワークという言葉を使い分けしています。バックプロパゲーションとは、入力層→隠れ層→出力層と順方向に学習入力を渡していく際に、出力誤差を計算するモデル学習のことを言います。このとき、計算された出力と学習出力の差である誤差が発生する。この誤差を利用して、最後の隠れ層から出力層までの重みを調整することで、誤差を小さくすることができる。この誤差は隠れ層を通してバックプロゲートされ、モデル内のすべての重みが更新されます。私の古い人工知能の本のどれかに詳しいコード例が載っている。ここでは、単純なニューラルネットワークがどのように学習されるのか、直感的に理解できるようになれば満足です。

基本的な考え方は、ランダムな重みで初期化されたネットワークからスタートし、各トレーニングケースについて、入力をネットワークを通して出力ニューロンへ伝搬し、出力エラーを計算し、出力ニューロンからのエラーを入力ニューロンへバックアップし、現在のトレーニング例の誤差を小さくするために重みを変更することである。このプロセスを何度も繰り返して、学習例を循環させる。

次の図は、隠れ層が1つの単純なバックプロパゲーションネットワークである。隣接する層のニューロンは浮動小数点数の接続強度の重みで接続されている。これらの重みは最初は小さなランダムな値であり、ネットワークの学習とともに変化する。下図では重みは矢印で表されているが、コードでは入力と出力ニューロンを結ぶ重みは2次元の配列として表されている。


![Example Backpropagation network with One Hidden Layer](/site_images1/hy-lisp-python/nn_backprop2d.png)


各非入力ニューロンは、そこに入力する接続ニューロンの活性化値から、接続重みでゲートをかけて（調整して）計算した活性化値を持っている。例えば上図では、出力1ニューロンの値は、入力1の活性化×重みW1,1と入力2の活性化×重みW2,1を合計し、この合計にSigmoidやRelu（下図参照）などの「スカッシング関数」を適用して出力1の活性化値の最終値を得ることによって算出される。活性値を比較的小さな範囲に平坦化したいが、相対的な値は維持したい。この平坦化のために、次の図に示すようなシグモイド関数と、シグモイド関数の微分を使用します。シグモイド関数は、重みを調整してネットワークを学習させるコードで使用する予定です。


![Sigmoid Function and Derivative of Sigmoid Function (SigmoidP)](/site_images1/hy-lisp-python/nn_sigmoid.png)


隠れ層が1つか2つの単純なニューラルネットワークは、バックプロパゲーションを使って簡単に学習することができます。JavaとCommon Lispのゼロからの実装は、オンラインで読める私の2冊の本、[Practical Artificial Intelligence Programming With Java](https://leanpub.com/javaai) と [Loving Common Lisp, or the Savvy Programmer's Secret Weapon](https://leanpub.com/lovinglisp) で見ることができます。しかし、ここではHyを使ってTensorFlowフレームワークを使ってモデルを書いています。このフレームワークは、ラップトップで実験している小さなモデルを、より多くのパラメータ（通常これは隠れ層のニューロンの数を増やして、モデルの重みの数を増やすことを意味します）に拡張でき、複数のGPUを使ってクラウドで実行できるという大きな利点を備えています。

私は現在、ペンダント的な目的を除いて、ニューラルネットワークのコードをゼロから書くことはなく、TensorFlow、PyTorch、mxnetなどのフレームワークの開発に費やされた何人年分ものエンジニアリングワークを利用しています。次に、TensorFlowで構築された2つの例を紹介します。

### Deep Learning

ディープラーニングモデルは、一般に単純な多層パーセプトロンニューラルネットワークよりも多くの隠れ層を持ち、多くの場合、複数の単純なモデルを直列または並列に組み合わせたもので構成されると理解されている。複雑なアーキテクチャは、モデルの構成要素の大きさを手動で調整し、構成要素を変更するなどして、繰り返し開発することができる。 また、モデル・アーキテクチャの検索を自動化することもできます。Capital Oneでは、1つのTensorFlowセッション内で効果的なモデルアーキテクチャを効率的に検索するGoogleの[AdaNetプロジェクト](https://github.com/tensorflow/adanet)を使いました。 ここで使用したモデルアーキテクチャは単純で、ウィスコンシン大学の癌データのサンプルの入力値を表す1つの入力層、1つの隠れ層、そして活性化値が良性または悪性の予測と解釈される1つのニューロンからなる出力層である。

本章の資料は2つの目的を果たすことを意図している。

- もしあなたが既にDeep LearningとTensorFlowに精通しているなら、ここにある例はHyからTensorFlowのAPIを呼び出す方法を示すのに役立つでしょう。
- もしあなたがDeep Learningにほとんど、あるいは全く触れていないのであれば、短いHy言語の例は、実験するための簡潔なコードを提供し、その後さらに勉強するかどうかを決めることができます。

もう一度言いますが、2つのオンラインDeep Learningコースシーケンスを受講することを検討することをお勧めします。Jeremy Howardは[fast.ai](https://fast.ai)で非常に優れたレッスンを無料で提供しており、後半のクラスではTensorFlowに類似したフレームワークであるPyTorchを使用しています。Andrew Ngは[deeplearning.ai](https://www.deeplearning.ai/)でTensorFlowを使った授業を安価で提供しています。私は1980年代から機械学習の分野で仕事をしていますが、最新の情報を得るために、今でもAndrewのオンラインクラスを受講しています。この8年間で、スタンフォード大学の機械学習のクラスを2回受講し、TensorFlowを使った一連のコースもすべて受講しています。また、Jeremyの教材の多くも勉強しました。私は、この2つのコースシリーズを自信を持ってお勧めします。

### KerasとTensorFlowを使ってWisconsin Cancer Data Setをモデル化する

ウィスコンシン大学の癌データベースには646のサンプルがある。各サンプルは9つの入力値と1つの出力値、ターゲット出力クラス（良性は0、癌は1）を持つ。

- 0 クランプの厚さ 1～10
- 1 セルサイズの均一性 1～10
- 2 細胞の形状の均一性 1 - 10
- 3 辺縁部付着性 1 - 10
- 4 単一上皮細胞サイズ 1 - 10
- 5 むき出しの核 1 - 10
- 6 ブランドクロマチン 1 - 10
- 7 正常な核小体 1 - 10
- 8 有糸分裂 1 - 10
- 9 クラス（良性は0、悪性は1）

ここでは、学習用とテスト用の別々のファイル **hy-lisp-python/deeplearning/train.csv** と **hy-lisp-python/deeplearning/test.csv** を使用することにします。以下は学習ファイルからのサンプルです。


    6,2,1,1,1,1,7,1,1,0
    2,5,3,3,6,7,7,5,1,1
    10,4,3,1,3,3,6,5,2,1
    6,10,10,2,8,10,7,3,3,1
    5,6,5,6,10,1,3,1,1,1
    1,1,1,1,2,1,2,1,2,0
    3,7,7,4,4,9,4,8,1,1
    1,1,1,1,2,1,2,1,1,0


このデータを見た後、もしあなたが機械学習の経験があまりないのであれば、ウィスコンシン州のデータセットに見られるような患者のサンプルを受け入れ、そのサンプルが患者の良性または癌の結果を暗示するかどうかを予測するモデルを構築する方法は明らかではないかもしれません。TensorFlowとシンプルなニューラルネットワークモデルを使って、この例を実装するために約40行のHyコードでモデルを実装します。

入力値が9つあるので、トレーニングデータまたは別のテストデータのどちらかのサンプルの入力値を表す9つの入力ニューロンが必要になります。これらの9つの入力ニューロン（以下のリストの9-10行目で作成）は、隠れ層の12個のニューロンに完全に接続されます。ここで、完全に接続されているとは、9つの入力ニューロンのそれぞれが、隠れ層の各ニューロンにウェイトを介して接続されていることを意味します。 入力層と隠れ層の間には、9 \* 12 = 108 個の重みがある。 各隠れ層のニューロンには、1つの出力層ニューロンが接続されています。

以下のリストの12行目と14行目では、隠れ層と出力層をつなぐ活性化関数が先にプロットした**sigmoid**活性化関数を使用しながら、**relu**活性化関数を指定していることに注意してください。

git example repo ディレクトリ **hy-lisp-python/matplotlib** にあるファイル **plot_relu.hy** で以下の図を生成する例があります。


![Relu Function](/site_images1/hy-lisp-python/relu.png)


以下のリストでは、Keras TensorFlow APIを使用して、1つの入力層、2つの隠れ層、および1つのニューロンだけを持つ出力層でモデルを構築しています（9～19行目）。モデルを構築した後、トレーニング入力（**x**引数）と対応するトレーニング出力（**y**引数）を与えてモデルをトレーニングする2つのユーティリティ関数 **train**（21-23 行）と、テスト入力値（**x-data**引数）を与えて癌または良性予測を行うトレーニングしたモデルを使用する **predict**（25-26 行）を定義しています。

28～33行目はユーティリティ関数 **load-data** で、University of Wisconsin の癌データセット CSV ファイルをロードし、入出力値を範囲 [0.0, 1.0] にスケールし、入力データ (**x-data**) とターゲット出力データ (**y-data**) を含むリストを返しています。この例をREPLで読み込んで、CSVファイルの1つに対して**load-data**を評価するとよいでしょう。

関数**main**（35～45行目）では、学習とテスト（学習に使用しなかったデータに対するモデルの精度の評価）をロードし、モデルを学習し、テスト（評価）データでモデルの精度をテストしています。


     1 #!/usr/bin/env hy
     2 
     3 (import argparse os)
     4 (import keras
     5         keras.utils.data-utils)
     6 
     7 (import [pandas [read-csv]])
     8 
     9 (defn build-model []
    10   (setv model (keras.models.Sequential))
    11   (.add model (keras.layers.core.Dense 9
    12                  :activation "relu"))
    13   (.add model (keras.layers.core.Dense 12
    14                  :activation "relu"))
    15   (.add model (keras.layers.core.Dense 1
    16                  :activation "sigmoid"))
    17   (.compile model :loss      "binary_crossentropy"
    18                   :optimizer (keras.optimizers.RMSprop))
    19   model)
    20 
    21 (defn train [batch-size model x y]
    22   (for [it (range 50)]
    23     (.fit model x y :batch-size batch-size :epochs 10 :verbose False)))
    24 
    25 (defn predict [model x-data]
    26     (.predict model x-data))
    27 
    28 (defn load-data [file-name]
    29   (setv all-data (read-csv file-name :header None))
    30   (setv x-data10 (. all-data.iloc [(, (slice 0 10) [0 1 2 3 4 5 6 7 8])] values))
    31   (setv x-data (* 0.1 x-data10))
    32   (setv y-data (. all-data.iloc [(, (slice 0 10) [9])] values))
    33   [x-data y-data])
    34 
    35 (defn main []
    36   (setv xyd (load-data "train.csv"))
    37   (setv model (build-model))
    38   (setv xytest (load-data "test.csv"))
    39   (train 10 model (. xyd [0]) (. xyd [1]))
    40   (print "* predictions (calculated, expected):")
    41   (setv predictions (list (map first (predict model (. xytest [0])))))
    42   (setv expected (list (map first (. xytest [1]))))
    43   (print
    44     (list
    45       (zip predictions expected))))
    46 
    47 (main)


次のリストは、その出力を示しています。


    1 $ hy wisconsin.hy 
    2 Using TensorFlow backend.
    3 * predictions (calculated, expected):
    4 [(0.9759052, 1), (0.99994254, 1), (0.8564741, 1), (0.95866203, 1), (0.03042546, 0), \
    5 (0.21845636, 0), (0.99662805, 1), (0.08626339, 0), (0.045683343, 0), (0.9992156, 1)]


最初のテストケースを見てみましょう。学習データからの「実際の」出力は値1であり、計算された予測値（学習済みモデルを使用）は0.9759052です。予測を行う際、カットオフ値、例えば0.5を選び、計算された予測値がカットオフ値より小さい場合はブール値の*false*予測、カットオフ値以上の場合はブール値の*true*予測として解釈することが可能です。

### LSTMリカレントニューラルネットワークを使って、哲学者ニーチェの文章に似た英文を生成してみる

Googleの[Kerasドキュメント（Hyのサンプルコードに含まれるLSTM.pyのリスト）](https://keras.io/examples/lstm_text_generation/)にあるPythonのサンプルプログラムをHyに翻訳してみます。これは中程度の長さの例で、Kerasを使ってPythonで実装された他のモデルをHyで使いたい場合の変換の一般的なガイドとして、元のPythonと翻訳したHyコードを使用することができます。PythonとHyのコードを比較しやすいように、ほとんどの場合、同じ変数名にしています。

nietzsche.txtのデータセットを使用するには、かなりの量のメモリを必要とすることに注意してください。もしあなたのコンピュータのRAMが16G以下なら、まず以下の例を "Create sentences and next_chars data... "というプリントが出るまで実行して、学習用テキストのサイズを小さくしてから、プログラムを終了させるとよいでしょう。このプログラムを初めて実行したとき、学習データはウェブから取得され、ローカルに保存されます。ファイル **~/.keras/datasets/nietzsche.txt** を手動で編集することで、75%のデータを削除することができます。


    1     pushd ~/.keras/datasets/
    2     mv nietzsche.txt nietzsche_large.txt
    3     head -800 nietzsche_large.txt > nietzsche.txt
    4     popd


次にサンプルを実行するとき、Kerasサンプルデータローディングユーティリティはローカルコピーに気付き、ファイルがはるかに小さくなっても、データローディングユーティリティは新しいコピーをダウンロードしません。

私は新しいDeep Learningモデルのトレーニングを開始するとき、**top**コマンドラインアクティビティを使用してシステムリソースを監視し、CPU上でトレーニングするときにページフォルトを監視したいのですが、これは私がシステムメモリに対して大きすぎるモデルをトレーニングしようとしていることを示す可能性があります。CUDAとGPUを使用している場合、GPUの使用状況を監視するためにCUDAコマンドラインユーティリティを使用します。この入門チュートリアルの範囲を超えていますが、[TensorBoard](https://www.tensorflow.org/tensorboard/)というツールはモデルの学習状態を監視するのに非常に便利です。

以下のコード例では、ウィスコンシン大学のがんデータセットを用いた例よりも複雑な点がいくつかあります。学習データの各文字を、1.0という単一の値を除くすべての0.0の値のベクトルであるワンホットエンコーディングに変換する必要があるのです。これから短いREPLセッションをお見せして、これがどのように働くかを理解していただき、その後完全なHyコードの例を見ていただくことにします。


     1 $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (import [keras.callbacks [LambdaCallback]])
     4 Using TensorFlow backend.
     5 => (import [keras.models [Sequential]])
     6 => (import [keras.layers [Dense LSTM]])
     7 => (import [keras.optimizers [RMSprop]])
     8 => (import [keras.utils.data_utils [get_file]])
     9 => (import [numpy :as np]) ;; note the syntax for aliasing a module name
    10 => (import random sys io)
    11 => (with [f (io.open "/Users/markw/.keras/datasets/nietzsche.txt" :encoding "utf-8")]
    12 ... (setv text (.read f)))
    13 => (cut text 98 130)
    14 'philosophers, in so far as they '
    15 => (setv chars (sorted (list (set text))))
    16 => chars
    17 ['\n', ' ', '!', '"', "'", '(', ')', ',', '-', '.', '0', '1', '2', '3', '4', '5', '6\
    18 ', '7', '8', '9', ':', ';', '?', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', '\
    19 K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', '_', 'a', \
    20 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r',\
    21  's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
    22 => (setv char_indices (dict (lfor i (enumerate chars) (, (last i) (first i)))))
    23 => char_indices
    24 {'\n': 0, ' ': 1, '!': 2, '"': 3, "'": 4, '(': 5, ')': 6, ',': 7, '-': 8, '.': 9, '0\
    25 ': 10, '1': 11, '2': 12, '3': 13, '4': 14, '5': 15, '6': 16, '7': 17, '8': 18, '9': \
    26 19, ':': 20, ';': 21, '?': 22, 'A': 23, 'B': 24, 'C': 25, 'D': 26, 'E': 27, 'F': 28,\
    27  'G': 29, 'H': 30, 'I': 31, 'J': 32, 'K': 33, 'L': 34, 'M': 35, 'N': 36, 'O': 37, 'P\
    28 ': 38, 'Q': 39, 'R': 40, 'S': 41, 'T': 42, 'U': 43, 'V': 44, 'W': 45, 'X': 46, 'Y': \
    29 47, '_': 48, 'a': 49, 'b': 50, 'c': 51, 'd': 52, 'e': 53, 'f': 54, 'g': 55, 'h': 56,\
    30  'i': 57, 'j': 58, 'k': 59, 'l': 60, 'm': 61, 'n': 62, 'o': 63, 'p': 64, 'q': 65, 'r\
    31 ': 66, 's': 67, 't': 68, 'u': 69, 'v': 70, 'w': 71, 'x': 72, 'y': 73, 'z': 74}
    32 => (setv indices_char (dict (lfor i (enumerate chars) i)))
    33 => indices_char
    34 {0: '\n', 1: ' ', 2: '!', 3: '"', 4: "'", 5: '(', 6: ')', 7: ',', 8: '-', 9: '.', 10\
    35 : '0', 11: '1', 12: '2', 13: '3', 14: '4', 15: '5', 16: '6', 17: '7', 18: '8', 19: '\
    36 9', 20: ':', 21: ';', 22: '?', 23: 'A', 24: 'B', 25: 'C', 26: 'D', 27: 'E', 28: 'F',\
    37  29: 'G', 30: 'H', 31: 'I', 32: 'J', 33: 'K', 34: 'L', 35: 'M', 36: 'N', 37: 'O', 38\
    38 : 'P', 39: 'Q', 40: 'R', 41: 'S', 42: 'T', 43: 'U', 44: 'V', 45: 'W', 46: 'X', 47: '\
    39 Y', 48: '_', 49: 'a', 50: 'b', 51: 'c', 52: 'd', 53: 'e', 54: 'f', 55: 'g', 56: 'h',\
    40  57: 'i', 58: 'j', 59: 'k', 60: 'l', 61: 'm', 62: 'n', 63: 'o', 64: 'p', 65: 'q', 66\
    41 : 'r', 67: 's', 68: 't', 69: 'u', 70: 'v', 71: 'w', 72: 'x', 73: 'y', 74: 'z'}
    42 => (setv maxlen 40)
    43 => (setv s "Oh! I saw 1 dog (yesterday)")
    44 => (setv x_pred (np.zeros [1 maxlen (len chars)]))
    45 => (for [[t char] (lfor j (enumerate s) j)]
    46 ... (setv (get x_pred 0 t (get char_indices char)) 1))
    47 => x_pred
    48 array([[[0., 0., 0., ..., 0., 0., 0.],
    49         [0., 0., 0., ..., 0., 0., 0.],
    50         [0., 0., 1., ..., 0., 0., 0.],   // here 1. is the third character "!"
    51         ...,
    52         [0., 0., 0., ..., 0., 0., 0.],
    53         [0., 0., 0., ..., 0., 0., 0.],
    54         [0., 0., 0., ..., 0., 0., 0.]]])
    55 => 


48-54行目については、各行が1文字のワンホットエンコーディングを表しています。50行目の3番目の文字のインデックス2が "1. "であることに注目してください。

これでワンホットエンコーディングの仕組みがわかったと思いますので、次の例を読んで納得してください。ワンホットエンコーディングについては、次のコード一覧の後でさらに説明します。学習用に、一度に40文字（変数**maxlen**の値）を取り、一度に1文字ずつワンホットエンコーディングを使って入力とし、目標出力は入力列の次の文字のワンホットエンコーディングになります。しばらくはモデルの学習を繰り返し、数文字のテキストが与えられたら、次の文字を予測する--このプロセスを繰り返す。生成されたテキストは、さらに多くのテキストを生成するためのモデルの入力として使用されます。十分な量のテキストが生成されるまで、このプロセスを繰り返すことができる。

これは、複雑なスキーマが深く入れ子になっているJSONをモデル化し、学習データと同じスキーマの合成JSONを生成するのに使った強力なテクニックです。ここでは、哲学者ニーチェの文章を模倣するモデルの学習は、JSONのような高度に構造化されたデータの学習よりずっと簡単です。


     1 #!/usr/bin/env hy
     2 
     3 ;; This example was translated from the Python example in the Keras
     4 ;; documentation at: https://keras.io/examples/lstm_text_generation/
     5 ;; The original Python file LSTM.py is included in the directory
     6 ;; hy-lisp-python/deeplearning for reference.
     7 
     8 (import [keras.callbacks [LambdaCallback]])
     9 (import [keras.models [Sequential]])
    10 (import [keras.layers [Dense LSTM]])
    11 (import [keras.optimizers [RMSprop]])
    12 (import [keras.utils.data_utils [get_file]])
    13 (import [numpy :as np]) ;; note the syntax for aliasing a module name
    14 (import random sys io)
    15 
    16 (setv path
    17       (get_file        ;; this saves a local copy in ~/.keras/datasets
    18         "nietzsche.txt"
    19         :origin "https://s3.amazonaws.com/text-datasets/nietzsche.txt"))
    20 
    21 (with [f (io.open path :encoding "utf-8")]
    22   (setv text (.read f))) ;; note: sometimes we use (.lower text) to
    23 ;;       convert text to all lower case
    24 (print "corpus length:" (len text))
    25 
    26 (setv chars (sorted (list (set text))))
    27 (print "total chars (unique characters in input text):" (len chars))
    28 (setv char_indices (dict (lfor i (enumerate chars) (, (last i) (first i)))))
    29 (setv indices_char (dict (lfor i (enumerate chars) i)))
    30 
    31 ;; cut the text in semi-redundant sequences of maxlen characters
    32 (setv maxlen 40)
    33 (setv step 3) ;; when we sample text, slide sampling window 3 characters
    34 (setv sentences (list))
    35 (setv next_chars (list))
    36 
    37 (print "Create sentences and next_chars data...")
    38 (for [i (range 0 (- (len text) maxlen) step)]
    39   (.append sentences (cut text i (+ i maxlen)))
    40   (.append next_chars (get text (+ i maxlen))))
    41 
    42 (print "Vectorization...")
    43 (setv x (np.zeros [(len sentences) maxlen (len chars)] :dtype np.bool))
    44 (setv y (np.zeros [(len sentences) (len chars)] :dtype np.bool))
    45 (for [[i sentence] (lfor j (enumerate sentences) j)]
    46   (for [[t char] (lfor j (enumerate sentence) j)]
    47     (setv (get x i t (get char_indices char)) 1))
    48   (setv (get y i (get char_indices (get next_chars i))) 1))
    49 (print "Done creating one-hot encoded training data.")
    50 
    51 (print "Building model...")
    52 (setv model (Sequential))
    53 (.add model (LSTM 128 :input_shape [maxlen (len chars)]))
    54 (.add model (Dense (len chars) :activation "softmax"))
    55 
    56 (setv optimizer (RMSprop 0.01))
    57 (.compile model :loss "categorical_crossentropy" :optimizer optimizer)
    58 
    59 (defn sample [preds &optional [temperature 1.0]]
    60   (setv preds (.astype (np.array preds) "float64"))
    61   (setv preds (/ (np.log preds) temperature))
    62   (setv exp_preds (np.exp preds))
    63   (setv preds (/ exp_preds (np.sum exp_preds)))
    64   (setv probas (np.random.multinomial 1 preds 1))
    65   (np.argmax probas))
    66 
    67 (defn on_epoch_end [epoch &optional not-used]
    68   (print)
    69   (print "----- Generating text after Epoch:" epoch)
    70   (setv start_index (random.randint 0 (- (len text) maxlen 1)))
    71   (for [diversity [0.2 0.5 1.0 1.2]]
    72     (print "----- diversity:" diversity)
    73     (setv generated "")
    74     (setv sentence (cut text start_index (+ start_index maxlen)))
    75     (setv generated (+ generated sentence))
    76     (print "----- Generating with seed:" sentence)
    77     (sys.stdout.write generated)
    78     (for [i (range 400)]
    79       (setv x_pred (np.zeros [1 maxlen (len chars)]))
    80       (for [[t char] (lfor j (enumerate sentence) j)]
    81         (setv (get x_pred 0 t (get char_indices char)) 1))
    82       (setv preds (first (model.predict x_pred :verbose 0)))
    83       (setv next_index (sample preds diversity))
    84       (setv next_char (get indices_char next_index))
    85       (setv sentence (+ (cut sentence 1) next_char))
    86       (sys.stdout.write next_char)
    87       (sys.stdout.flush))
    88     (print)))
    89 
    90 (setv print_callback (LambdaCallback :on_epoch_end on_epoch_end))
    91 
    92 (model.fit x y :batch_size 128 :epochs 60 :callbacks [print_callback])


52～54行目でKerasのAPIを使ってモデルを定義し、56～57行目で[categorical crossentropy loss function with an RMSprop optimizer](https://keras.io/optimizers/) を使ってモデルをコンパイルしています。

59-65行目では、最初の必須引数**preds**を取る関数**sample**を定義しており、これは（maxlenまたは40個の値）のように見えるかもしれないワンショット予測符号化された文字です。

[2.80193929e-02 6.78635418e-01 7.85831537e-04 4.92034527e-03 . . .  6.62320468e-04 9.14627407e-03 2.31375365e-04]

さて、ここで予測されるワンホットエンコーディングの値は厳密には0や1ではなく、むしろ他よりもはるかに大きな1つの数値の小さな浮動小数点数になっています。最大の数値はインデックス1の6.78635418e-01で、これはスペース文字" "のワンホットエンコーディングに相当します。

学習用テキストファイルnietzsche.txtのテキスト中の文字数と文字の固有リスト（変数**chars**）を出力してみると、以下のようになります。


    corpus length: 600893
    ['\n', ' ', '!', '"', "'", '(', ')', ',', '-', '.', '0', '1', '2', '3', '4', '5', '6\
    ', '7', '8', '9', ':', ';', '=', '?', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', '\
    J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', \
    '[', ']', '_', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n',\
     'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'Æ', 'ä', 'æ', 'é', 'ë'\
    ]


**ワンホットエンコーディングの復習:**。

先ほどのワンホットエンコーディングの話を、より単純なケースでおさらいしてみましょう。入力テキストをどのようにしてモデルの入力にワンホットエンコードし、学習済みモデルを使ってテキストを生成するときにワンホットベクトルをテキストにデコードバックするのかを理解することが重要です。文字からインデックスに変換し、さらにインデックスを元の文字に逆変換するための辞書を、先ほど見たように、いくつかの出力を削除して見ることが助けになるでしょう。


    char_indices:
     {'\n': 0, ' ': 1, '!': 2, '"': 3, "'": 4, '(': 5, ')': 6, ',': 7, '-': 8, '.': 9, '\
    0': 10, '1': 11, '2': 12, '3': 13, '4': 14, '5': 15, '6': 16, '7': 17, '8': 18, '9':\
     19, 
       . . .
     'f': 58, 'g': 59, 'h': 60, 'i': 61, 'j': 62, 'k': 63, 'l': 64, 'm': 65, 'n': 66, 'o\
    ': 67, 'p': 68, 'q': 69, 'r': 70, 's': 71, 't': 72, 'u': 73, 'v': 74, 'w': 75, 'x': \
    76, 'y': 77, 'z': 78, 'Æ': 79, 'ä': 80, 'æ': 81, 'é': 82, 'ë': 83}
    indices_char:
     {0: '\n', 1: ' ', 2: '!', 3: '"', 4: "'", 5: '(', 6: ')', 7: ',', 8: '-', 9: '.', 1\
    0: '0', 11: '1', 12: '2', 13: '3', 14: '4', 15: '5', 16: '6', 17: '7', 18: '8', 19: \
    '9', 
       . . .
     'o', 68: 'p', 69: 'q', 70: 'r', 71: 's', 72: 't', 73: 'u', 74: 'v', 75: 'w', 76: 'x\
    ', 77: 'y', 78: 'z', 79: 'Æ', 80: 'ä', 81: 'æ', 82: 'é', 83: 'ë'}


前回のコードリストの43行目から48行目で入力データとターゲット出力データを用意しました。短い文字列を使って、入力文字列に対してこれらの入力と出力の学習例がどのように抽出されるか、次のREPLセッションのリストで見てみましょう。


     1 Marks-MacBook:deeplearning $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (setv text "0123456789abcdefg")
     4 => (setv maxlen 4)
     5 => (setv i 3)
     6 => (cut text i (+ i maxlen))
     7 '3456'
     8 => (cut text (+ 1 maxlen))
     9 '56789abcdefg'
    10 => (setv i 4)                 ;; i is the for loop variable for
    11 => (cut text i (+ i maxlen))  ;; defining sentences and next_chars
    12 '4567'
    13 => (cut text (+ i maxlen))
    14 '89abcdefg'
    15 => 


入力訓練文はそれぞれ**maxlen**文字で、**next-chars**ターゲット出力はそれぞれ対応する入力訓練文の最後の文字の後の文字から始まる。

このスクリプトは各訓練エポックの間に一時停止し、0.2, 0.5, 1.0, 1.2の多様性値でテキストを生成する。多様性の値が小さいほど、生成されるテキストはより訓練テキストに近くなる。何度も学習用エポックを繰り返すと、生成されるテキストはより現実的なものになる。以下では、数回の学習エポックを実行したものを高度に編集してリストアップしている。生成されたテキストは多様性が0.2の場合のみ表示する。


     1 ----- Generating text after Epoch: 0
     2 ----- diversity: 0.2
     3 ----- Generating with seed: ocity. Equally so, gratitude.--Justice r
     4 ocity. Equally so, gratitude.--Justice read in the become to the conscience the seen\
     5 er and the conception that the becess of the power to the procentical that the becau\
     6 se and the prostice of the prostice and the will to the conscience of the power of t\
     7 he perhaps the self-distance of the all the soul and the world and the soul of the s\
     8 oul of the world and the soul and an an and the profound the self-dister the all the\
     9  belief and the
    10 
    11 ----- Generating text after Epoch: 8
    12 ----- diversity: 0.2
    13 ----- Generating with seed: nations
    14 laboring simultaneously under th
    15 nations
    16 laboring simultaneously under the subjection of the soul of the same to the subjecti\
    17 on of the subjection of the same not a strong the soul of the spiritual to the same \
    18 really the propers to the stree be the subjection of the spiritual that is to probab\
    19 ly the stree concerning the spiritual the sublicities and the spiritual to the proce\
    20 ssities the spirit to the soul of the subjection of the self-constitution and proper\
    21 s to the
    22 
    23 ----- Generating text after Epoch: 14
    24 ----- diversity: 0.2
    25 ----- Generating with seed:  to which no other path could conduct us
    26  to which no other path could conduct us a stronger that is the self-delight and the\
    27  strange the soul of the world of the sense of the sense of the consider the such a \
    28 state of the sense of the sense of the sense of such a sandine and interpretation of\
    29  the process of the sense of the sense of the sense of the soul of the process of th\
    30 e world in the sense of the sense of the spirit and superstetion of the world the se\
    31 nse of the
    32 
    33 ----- Generating text after Epoch: 17
    34 ----- diversity: 0.2
    35 ----- Generating with seed: hemselves although they could easily hav
    36 hemselves although they could easily have been moral morality and the self-in which \
    37 the self-in the world to the same man in the standard to the possibility that is to \
    38 the strength of the sense-in the former the sense-in the special and the same man in\
    39  the consequently the soul of the superstition of the special in the end to the poss\
    40 ible that it is will not be a sort of the superior of the superstition of the same m\
    41 an to the same man


ここでは哲学者Nietzscheの英語に翻訳された例で学習しました。この例と似たようなコードを使って、高度に構造化されたJSONデータで学習したことがあります。その結果、LSTMをベースとしたモデルは、通常、同じような構造のJSONを生成することができました。私は、学習データがC++のコードである他の例を見たことがあります。

この例はどのように動作しているのでしょうか？このモデルはどのような文字の組み合わせがどのような順序で一緒に現れる傾向があるかを学習しています。

次の章では、自然言語処理（NLP）のために事前に学習されたDeep Learningモデルを使用します。

## 自然言語処理

私は1985年から自然言語処理（NLP）の分野で仕事をしているので、2014年以降に起きたNLPの革命的な変化を「生きて」きました：深層学習の結果がそれまでの記号的手法の結果を凌駕したのです。

ここでは古い記号的なNLPの手法は取り上げず、拙著[Javaで実践する人工知能プログラミング](https://leanpub.com/javaai)、[Loving Common Lisp, The Savvy Programmer's Secret Weapon](https://leanpub.com/lovinglisp)、[Haskell Tutorial and Cookbook](https://leanpub.com/haskell-cookbook)を参考にしていただければと思います。NLPにDeep Learning（DL）を使うとより良い結果が得られ、本章で使用するライブラリ**spaCy**（[https://spacy.io](https://spacy.io/)）は、ほぼ最先端の性能を提供しています。spaCy**の作者は、この分野の最新のブレークスルーを利用するために頻繁に更新しています。

最先端のフルフィーチャライブラリ[spaCy](https://spacy.io/)を使って、DLとNLPの両方を適用する方法を学びます。この章では、私が自分の仕事で使っているNLPの厳選されたいくつかの問題を解決するために、Hy言語でのspaCyの使い方に焦点を当てます。私は、[spaCy documentation](https://spacy.io/usage) の "Guides" セクションを見直すことを強く勧めます。そこでは、例はPythonで書かれていますが、この章の例で実験した後、spaCy Pythonの例をHy言語に翻訳することに何の困難もないはずです。

まだインストールしていない場合は、**spaCy**ライブラリと完全英語版モデルをインストールしてください。


    pip install spacy
    python -m spacy download en


より小さなモデルを使用することができます（以下の例では、"en "の代わりに "en_core_web_sm "を読み込む必要があります）。


    pip install spacy
    python -m spacy download en_core_web_sm


### spaCyライブラリの探索

私たちはLispスタイルでspaCyを実験するためにHy REPLを使用します。以下のREPLリストはすべて同じセッションからのもので、私が例を通して話すことができるように別々のリストに分割されています。


     1 Marks-MacBook:nlp $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (import spacy)
     4 => (setv nlp-model (spacy.load "en"))
     5 => (setv doc (nlp-model "President George Bush went to Mexico and he had a very good\
     6  meal"))
     7 => doc
     8 President George Bush went to Mexico and he had a very good meal
     9 => (dir doc)
    10 ['_', '__bytes__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__fo\
    11 rmat__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init_\
    12 _', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__ne__', '__new\
    13 __', '__pyx_vtable__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__\
    14 setstate__', '__sizeof__', '__str__', '__subclasshook__', '__unicode__', '_bulk_merg\
    15 e', '_py_tokens', '_realloc', '_vector', '_vector_norm', 'cats', 'char_span', 'count\
    16 _by', 'doc', 'ents', 'extend_tensor', 'from_array', 'from_bytes', 'from_disk', 'get_\
    17 extension', 'get_lca_matrix', 'has_extension', 'has_vector', 'is_nered', 'is_parsed'\
    18 , 'is_sentenced', 'is_tagged', 'lang', 'lang_', 'mem', 'merge', 'noun_chunks', 'noun\
    19 _chunks_iterator', 'print_tree', 'remove_extension', 'retokenize', 'sentiment', 'sen\
    20 ts', 'set_extension', 'similarity', 'tensor', 'text', 'text_with_ws', 'to_array', 't\
    21 o_bytes', 'to_disk', 'to_json', 'user_data', 'user_hooks', 'user_span_hooks', 'user_\
    22 token_hooks', 'vector', 'vector_norm', 'vocab']


3-6行目でspaCyライブラリをインポートし、英語モデルをロードし、入力テキストからドキュメントを作成しています。spaCyドキュメントとは何でしょうか？ 9行目では、Pythonの標準関数**dir**を使って、テキストを含む文字列にspaCyモデルを適用して得られたオブジェクト**doc**に対して定義されたすべての名前と関数を調べています。出力された値には多くの「ダンダー」（ダブルアンダースコアの属性）が組み込まれており、これらを削除することができます。

23-26行目では、再び**dir**関数を使ってこのクラスの属性とメソッドを表示していますが、"absolute "の文字が含まれる属性はすべて取り除いています。


    23 => (lfor
    24 ... x (dir doc)
    25 ... :if (not (.startswith x "__"))
    26 ... x)
    27 ['_', '_bulk_merge', '_py_tokens', '_realloc', '_vector', '_vector_norm', 'cats', 'c\
    28 har_span', 'count_by', 'doc', 'ents', 'extend_tensor', 'from_array', 'from_bytes', '\
    29 from_disk', 'get_extension', 'get_lca_matrix', 'has_extension', 'has_vector', 'is_ne\
    30 red', 'is_parsed', 'is_sentenced', 'is_tagged', 'lang', 'lang_', 'mem', 'merge', 'no\
    31 un_chunks', 'noun_chunks_iterator', 'print_tree', 'remove_extension', 'retokenize', \
    32 'sentiment', 'sents', 'set_extension', 'similarity', 'tensor', 'text', 'text_with_ws\
    33 ', 'to_array', 'to_bytes', 'to_disk', 'to_json', 'user_data', 'user_hooks', 'user_sp\
    34 an_hooks', 'user_token_hooks', 'vector', 'vector_norm', 'vocab']
    35 =>


**to_json**メソッドは期待できそうなので、Python pretty printライブラリをインポートして、**doc**に格納されているドキュメントに対して**to_json**メソッドを呼び出した結果を、きれいに印刷して見てみましょう。


     36 => (import [pprint [pprint]])
     37 => (pprint (doc.to_json))
     38 {'ents': [{'end': 21, 'label': 'PERSON', 'start': 10},
     39           {'end': 36, 'label': 'GPE', 'start': 30}],
     40  'sents': [{'end': 64, 'start': 0}],
     41  'text': 'President George Bush went to Mexico and he had a very good meal',
     42  'tokens': [{'dep': 'compound',
     43              'end': 9,
     44              'head': 2,
     45              'id': 0,
     46              'pos': 'PROPN',
     47              'start': 0,
     48              'tag': 'NNP'},
     49             {'dep': 'compound',
     50              'end': 16,
     51              'head': 2,
     52              'id': 1,
     53              'pos': 'PROPN',
     54              'start': 10,
     55              'tag': 'NNP'},
     56             {'dep': 'nsubj',
     57              'end': 21,
     58              'head': 3,
     59              'id': 2,
     60              'pos': 'PROPN',
     61              'start': 17,
     62              'tag': 'NNP'},
     63             {'dep': 'ROOT',
     64              'end': 26,
     65              'head': 3,
     66              'id': 3,
     67              'pos': 'VERB',
     68              'start': 22,
     69              'tag': 'VBD'},
     70             {'dep': 'prep',
     71              'end': 29,
     72              'head': 3,
     73              'id': 4,
     74              'pos': 'ADP',
     75              'start': 27,
     76              'tag': 'IN'},
     77             {'dep': 'pobj',
     78              'end': 36,
     79              'head': 4,
     80              'id': 5,
     81              'pos': 'PROPN',
     82              'start': 30,
     83              'tag': 'NNP'},
     84             {'dep': 'cc',
     85              'end': 40,
     86              'head': 3,
     87              'id': 6,
     88              'pos': 'CCONJ',
     89              'start': 37,
     90              'tag': 'CC'},
     91             {'dep': 'nsubj',
     92              'end': 43,
     93              'head': 8,
     94              'id': 7,
     95              'pos': 'PRON',
     96              'start': 41,
     97              'tag': 'PRP'},
     98             {'dep': 'conj',
     99              'end': 47,
    100              'head': 3,
    101              'id': 8,
    102              'pos': 'VERB',
    103              'start': 44,
    104              'tag': 'VBD'},
    105             {'dep': 'det',
    106              'end': 49,
    107              'head': 12,
    108              'id': 9,
    109              'pos': 'DET',
    110              'start': 48,
    111              'tag': 'DT'},
    112             {'dep': 'advmod',
    113              'end': 54,
    114              'head': 11,
    115              'id': 10,
    116              'pos': 'ADV',
    117              'start': 50,
    118              'tag': 'RB'},
    119             {'dep': 'amod',
    120              'end': 59,
    121              'head': 12,
    122              'id': 11,
    123              'pos': 'ADJ',
    124              'start': 55,
    125              'tag': 'JJ'},
    126             {'dep': 'dobj',
    127              'end': 64,
    128              'head': 8,
    129              'id': 12,
    130              'pos': 'NOUN',
    131              'start': 60,
    132              'tag': 'NN'}]}
    133 => 


JSONデータはネストした辞書になっています。後のナレッジグラフの章では、人、組織などの名前付きエンティティをテキストから取得し、この情報を使ってナレッジグラフ用のデータを自動生成したいと思います。その際、キーである **ents** (entityの略) の値が役に立ちます。原文の単語は、開始と終了のテキストトークンインデックス（52～142行目の**head**と**end**の値）で指定されていることに注目しよう。

42行目から132行目に記載されているキー**tokens**の値には、先頭（または開始インデックス、終了インデックス）、トークン番号（**id**）、品詞（**pos**）が含まれています。品詞が何を意味するかは後で列挙する。

各エンティティの単語を連結して、各エンティティの単一の文字列にしたいのですが、ここでは136～137行目でこれを行い、138～139行目でその結果を見ています。

エンティティ名の文字列を文書を表す辞書に戻すのが好きで、140行目では**lfor**を使用して、エンティティ名を単一の文字列として含むリストのサブリストと、エンティティのタイプを作成することを示しています。次節では、spaCyがサポートするエンティティタイプを列挙します。


    134 => doc.ents
    135 (George Bush, Mexico)
    136 => (for [entity doc.ents]
    137 ... (print "entity text:" entity.text "entity label:" entity.label_))
    138 entity text: George Bush entity label: PERSON
    139 entity text: Mexico entity label: GPE
    140 => (lfor entity doc.ents [entity.text entity.label_])
    141 [['George Bush', 'PERSON'], ['Mexico', 'GPE']]
    142 => 


また、各文章を個別の文字列としてアクセスすることもできます。この例では、サンプル文書を作成するために使用した元のテキストには1つの文しかなかったので、**sents** プロパティは1つの文字列を含むリストを返します。


    147 => (list doc.sents)
    148 [President George Bush went to Mexico and he had a very good meal]
    149 => 


最後に、spaCy文書オブジェクトの使い方を示す例として、各単語とその品詞をリストアップします。


    150 => (for [word doc]
    151 ... (print word.text word.pos_))
    152 President PROPN
    153 George PROPN
    154 Bush PROPN
    155 went VERB
    156 to ADP
    157 Mexico PROPN
    158 and CCONJ
    159 he PRON
    160 had VERB
    161 a DET
    162 very ADV
    163 good ADJ
    164 meal NOUN
    165 => 


品詞タグの定義を以下に示す。

- ADJ：形容詞
- ADP：アドポジション
- ADV：副詞
- AUX：助動詞
- CONJ：調整用接続詞
- DET: 決定詞
- INTJ：間投詞
- NOUN: 名詞
- NUM: 数値
- PART: 助詞
- PRON: 代名詞
- PROPN: 固有名詞
- PUNCT: 句読点
- SCONJ: 従属接続詞
- SYM: 記号
- VERB: 動詞
- X: その他

### Python spaCyライブラリのためのHyNLPラッパーの実装

我々は2つのライブラリ（ファイル **nlp_lib.hy** と **coref_nlp_lib.hy** ）を生成します。一つは一般的な自然言語処理ライブラリで、もう一つは特にアナフォーラ解決、または共参照の問題を解決するものです。 各ライブラリのテストプログラムが **nlp_example.hy** と **coref_example.hy** というファイルにあります。

後の章の例では，ここで開発したライブラリを使って，テキストデータから知識グラフを自動生成することにする。テキスト中の人名、会社名、地名などを検索する機能が必要になる。 ここではspaCyを使ってこれを行う。spaCyが事前に学習している名前付きエンティティの種類は以下の通りです。

-   CARDINAL：お金や時間など、より具体的な種類として特定されていないあらゆる数字。
-   DATE
-   FAC：高速道路、橋梁、空港などの施設。
-   GPE：国、州（または県）、および都市
-   LOC：GPE以外の任意の場所
-   PRODUCT
-   EVENT
-   LANGUAGE：名前のついた任意の言語
-   MONEY: あらゆる貨幣価値または貨幣の単位
-   NORP: 国籍または宗教団体
-   ORG：企業、非営利団体、学校などあらゆる組織。
-   PERCENT: 数値の後にパーセント文字 % が続く。
-   PERSON
-   ORDINAL："1"、"2 "などのように綴られた任意の数字。
-   TIME

hy-lisp-python/nlp/nlp_lib.hyのリスト。


     1 (import spacy)
     2 
     3 (setv nlp-model (spacy.load "en"))
     4 
     5 (defn nlp [some-text]
     6   (setv doc (nlp-model some-text))
     7   (setv entities (lfor entity doc.ents [entity.text entity.label_]))
     8   (setv j (doc.to_json))
     9   (setv (get j "entities") entities)
    10   j)


hy-lisp-python/nlp/nlp_example.hyのリストです。


    1 #!/usr/bin/env hy
    2 
    3 (import [nlp-lib [nlp]])
    4 
    5 (print
    6   (nlp "President George Bush went to Mexico and he had a very good meal"))
    7 
    8 (print
    9   (nlp "Lucy threw a ball to Bill and he caught it"))



    1 Marks-MacBook:nlp $ ./nlp_example.hy
    2 {'text': 'President George Bush went to Mexico and he had a very good meal', 'ents':\
    3  [{'start': 10, 'end': 21, 'label': 'PERSON'}, {'start': 30, 'end': 36, 'label': 'GP\
    4 E'}], 'sents': [{'start': 0, 'end': 64}], 'tokens': 
    5 
    6   ..LOTS OF OUTPUT NOT SHOWN..


### 共参照 (アナフォーラ解決)

もう一つの一般的な NLP タスクは、テキスト内の代名詞（彼、彼女、それなど）を、代名詞が参照する先行固有名詞と解決するプロセスである共参照（またはアナフォラ解決）です。簡単な例では、"John ran fast and he fell" を "John ran fast and John fell" と訳すことができます。これは簡単な例ですが、代名詞が参照する固有名詞は前の文にあることが多く、共参照の解決は曖昧で、一般的な単語の使い方や文法の知識が必要になる場合があります。この問題は、現在では[BERT](https://github.com/google-research/bert)のような深層学習による転移モデルで処理されています。

**spaCy**のインストールに加え、**neuralcoref**というライブラリが必要です。特定のバージョンの**spaCy**と**neuralcoref**のみ、互換性があります。2020年7月31日現在、依存関係を取得し、このセクションのサンプルを実行するには、次のように動作します。


    pip uninstall spacy neuralcoref
    pip install spacy==2.1.3
    python -m spacy download en
    pip install neuralcoref==4.0.0
    ./coref_example.hy 


**spaCy**のバージョン2.1.3はpipがインストールするデフォルトのバージョンより古いことに注意してください。この例のために新しいPython仮想環境を作成するか、Anacondaを使用している場合は別のAnaconda環境を使用するとよいでしょう。

coref_nlp_lib.hyのリストには、spaCyの共参照モデルのラッパーが含まれています。


     1 (import argparse os)
     2 (import spacy neuralcoref)
     3 
     4 (setv nlp2 (spacy.load "en"))
     5 (neuralcoref.add_to_pipe nlp2)
     6 
     7 (defn coref-nlp [some-text]
     8   (setv doc (nlp2 some-text))
     9   { "corefs" doc._.coref_resolved
    10     "clusters" doc._.coref_clusters
    11     "scores" doc._.coref_scores})


**coref_example.hy** のリストは、Hy spaCy と coreference のラッパーをテストするためのコードを示しています。


    1 #!/usr/bin/env hy
    2 
    3 (import [coref-nlp-lib [coref-nlp]])
    4 
    5 ;; tests:
    6 (print (coref-nlp "President George Bush went to Mexico and he had a very good meal"\
    7 ))
    8 (print (coref-nlp "Lucy threw a ball to Bill and he caught it"))


このように出力されます。


     1 Marks-MacBook:nlp $ ./coref_example.hy 
     2 {'corefs': 'President George Bush went to Mexico and President George Bush had a ver\
     3 y good meal', 'clusters': [President George Bush: [President George Bush, he]], 'sco\
     4 res': {President George Bush: {President George Bush: 1.5810412168502808}, George Bu\
     5 sh: {George Bush: 4.11817741394043, President George Bush: -1.546141266822815}, Mexi\
     6 co: {Mexico: 1.4138349294662476, President George Bush: -4.650205612182617, George B\
     7 ush: -3.666614532470703}, he: {he: -0.5704692006111145, President George Bush: 9.385\
     8 97583770752, George Bush: -1.4178757667541504, Mexico: -3.6565260887145996}, a very \
     9 good meal: {a very good meal: 1.652894377708435, President George Bush: -2.554375886\
    10 9171143, George Bush: -2.13267183303833, Mexico: -1.6889561414718628, he: -2.7667927\
    11 742004395}}}
    12 
    13 {'corefs': 'Lucy threw a ball to Bill and Bill caught a ball', 'clusters': [a ball: \
    14 [a ball, it], Bill: [Bill, he]], 'scores': {Lucy: {Lucy: 0.41820740699768066}, a bal\
    15 l: {a ball: 1.8033190965652466, Lucy: -2.721518039703369}, Bill: {Bill: 1.5611814260\
    16 482788, Lucy: -2.8222298622131348, a ball: -1.806389570236206}, he: {he: -0.57600766\
    17 42036438, Lucy: 3.054243326187134, a ball: -1.818403720855713, Bill: 3.0774276256561\
    18 28}, it: {it: -1.0269954204559326, Lucy: -3.4972281455993652, a ball: -0.31290221214\
    19 294434, Bill: -2.5343685150146484, he: -3.6687228679656982}}}


アナフォラの解決は、共参照とも呼ばれ、入力テキスト内の2つ以上の単語またはフレーズが同じ名詞を参照することを指します。この分析には、通常、代名詞がどの名詞句を参照しているかを特定することが含まれます。

### まとめ

私は1984年から2015年までの数年間、自然言語処理技術に開発時間を費やし、個人のサイドプロジェクトとして、RubyとCommon Lispで自作した商用NLPライブラリの販売をしていました。Deep Learningで強化されたNLPの最先端技術は非常に優れており、オープンソースのspaCyライブラリは、従来のNLP技術と事前に学習されたDeep Learningモデルの両方をうまく利用しています。私はもはや自分のNLPライブラリを書くのにあまり時間をかけず、代わりにspaCyを使用しています。

この章では、ナレッジグラフのデータを自動生成するために必要な基本的な機能のみをカバーしましたので、[spaCy documentation](https://spacy.io/api/doc) に目を通しておくことを強くお勧めします。対話的なREPLセッションとこの章の例を通して作業した後、あなたはどんなPython APIサンプルコードもHyに翻訳することができるはずです。

## データストア

私は過去20年以上、コンサルティング業務において、ほとんどのデータの保存と処理にフラットファイルとPostgreSQLリレーショナルデータベースを使用してきました。Compass LabsやGoogleでの大規模データプロジェクトでは、HadoopとBig Tableを使った。ここでは、ビッグデータのデータストアは取り上げず、むしろ私が考える「ラップトップ開発」の要件、つまり適度なデータ量と開発スピードの最適化、インフラのセットアップのしやすさに焦点を当てたいと考えています。3つのデータストアを取り上げます。

- Sqlite シングルファイルベースリレーショナルデータベース
- PostgreSQLリレーショナルデータベース
- RDFライブラリ **rdflib** - セマンティックウェブやリンクデータのアプリケーションに便利です。

グラフデータについては、RDFがかなり広く使われている標準であるため、RDFにこだわることにする。Google、Microsoft、Yahoo、Yandexは、Web上の構造化データのスキーマを定義するための[schema.org](https://schema.org/)をサポートしています。次の章では、RDFについてさらに詳しく説明します。ここでは、RDFデータを操作して問い合わせるための**rdflib**ライブラリの「配管」と、RDFデータをいくつかのフォーマットでエクスポートする方法について見ていきます。さらに後の章では、カスタマイズされたナレッジグラフを開発するためのツールとして、生のテキストからRDFデータを自動的に生成するツールを開発する予定です。

私の以前の著書[Loving Common Lisp, or the Savvy Programmer's Secret Weapon](https://leanpub.com/lovinglisp) では、汎用グラフデータベースNeo4jも取り上げ、いくつかのユースケースで愛用していますが、この本の目的ではRDFにこだわります。

### Sqlite

ここでは、2つのリレーショナルデータベースを取り上げます。SqliteとPostgreSQLです。Sqliteは組み込み型のデータベースです。多くのプログラミング言語用のSqliteライブラリがありますが、ここではPythonのライブラリを使用します。

以下の例は単純ですが、単一ファイルのSqliteデータベースを開き、データを追加し、データを変更し、データを照会し、データを削除する方法を示すのに十分です。リレーショナルデータベース、特にデータの列や行、SQLクエリなどの概念にある程度慣れていることを前提にしています。

まず、Sqlite を使用するための共通のコードを再利用可能なライブラリとして **sqlite_lib.hy** というファイルにまとめるところから始めましょう。


     1 (import [sqlite3 [connect version Error ]])
     2 
     3 (defn create-db [db-file-path] ;; db-file-path can also be ":memory:"
     4   (setv conn (connect db-file-path))
     5   (print version)
     6   (conn.close))
     7 
     8 (defn connection [db-file-path] ;; db-file-path can also be ":memory:"
     9   (connect db-file-path))
    10 
    11 (defn query [conn sql &optional variable-bindings]
    12   (setv cur (conn.cursor))
    13   (if variable-bindings
    14     (cur.execute sql variable-bindings)
    15     (cur.execute sql))
    16   (cur.fetchall))


3-6行目の関数 **create-db** は、データベースがまだ存在しない場合、ファイルパスからデータベースを作成します。関数 **connection** (8-9行目) は、Sqliteデータベースに使用される単一ファイルへのファイルパスで定義されるデータベースへの持続的な接続を作成します。この接続は再利用することができます。関数 **query** (11-16行目) は、接続オブジェクトと文字列として表されるSQLクエリを必要とし、データベースへの問い合わせを行い、一致するすべてのデータを入れ子リストで返します。

以下のファイル **sqlite_example.hy** のリストは、この単純なライブラリの使い方を示している。


     1 #!/usr/bin/env hy
     2 
     3 (import [sqlite-lib [create-db connection query]])
     4 
     5 (defn test_sqlite-lib []
     6   (setv dbpath ":memory:")
     7   (create-db dbpath)
     8   (setv conn (connection ":memory:"))
     9   (query conn "CREATE TABLE people (name TEXT, email TEXT);")
    10   (print
    11     (query conn "INSERT INTO people VALUES ('Mark', 'mark@markwatson.com')"))
    12   (print
    13     (query conn "INSERT INTO people VALUES ('Kiddo', 'kiddo@markwatson.com')"))
    14   (print
    15     (query conn "SELECT * FROM people"))
    16   (print
    17     (query conn "UPDATE people SET name = ? WHERE email = ?"
    18       ["Mark Watson" "mark@markwatson.com"]))
    19   (print
    20     (query conn "SELECT * FROM people"))
    21   (print
    22     (query conn "DELETE FROM people  WHERE name=?" ["Kiddo"]))
    23     (print
    24     (query conn "SELECT * FROM people"))
    25   (conn.close))
    26 
    27 (test_sqlite-lib)


7行目と8行目でインメモリデータベースを開いていますが、**:memory** の代わりに例えば "test_database.db" を使ってディスク上に永続データベースを作成することも可能です。9行目では、2つのカラムだけを持つデータベーステーブルを作成し、各カラムには文字列の値を格納しています。

15、20、24行目では、アスタリスク文字を使用したワイルドカードクエリを使用して、データベース内の一致した各行に対してすべての列の値を返しています。

このサンプルプログラムを実行すると、次のような出力が得られます。


    1 $ ./sqlite_example.hy
    2 2.6.0
    3 []
    4 []
    5 [('Mark', 'mark@markwatson.com'), ('Kiddo', 'kiddo@markwatson.com')]
    6 []
    7 [('Mark Watson', 'mark@markwatson.com'), ('Kiddo', 'kiddo@markwatson.com')]
    8 []
    9 [('Mark Watson', 'mark@markwatson.com')]


2行目は、使用しているSQlistのバージョンを示しています。テーブルの作成、テーブルへのデータの挿入、テーブルの行の更新、行の削除を行う関数が値を返さないため、1-2、4、6行目のリストは空になっています。

次の節では、PostgreSQLがJSONデータをネイティブなデータ型としてどのように扱うかを見ていきます。sqliteでは、JSONデータを "ダンプされた "文字列値として格納することができますが、データ内のキーと値のペアで問い合わせを行うことはできません。JSONを文字列としてエンコードし、それをデコードしてJSONに戻す（あるいは辞書として戻す）には、以下を使用します。


    1 (import [json [dumps loads]])
    2 
    3 (setv json-data .....)
    4 (setv s-data (json.dumps json-data))
    5 (setv restored-json-data (json.loads s-data))


### PostgreSQL

Sqlite組み込みデータベースの使用例を見たばかりです。今度は、私のお気に入りの汎用データベースであるPostgreSQLを見てみましょう。PostgreSQLデータベースサーバーは、ほとんどのクラウドプロバイダーでマネージドサービスとして提供されていますし、ラップトップやVPS、サーバー上でPostgreSQLサーバーを実行することも簡単です。

ここでは、CPythonと互換性のある[psycopg](http://initd.org/psycopg/) PostgreSQLアダプタを使用し、それを使用してインストールすることができます。


    1     pip install psycopg2


以下の資料は自己完結していますが、あなた自身のアプリケーションでPostgreSQLとpsycopgを使用する前に、psycopgのドキュメントを参照することをお勧めします。

#### macOSとLinuxでPostgreSQLを使い、サンプルデータベース "hybook "をセットアップするための注意事項

以下の2つのセクションは、macOSとLinuxでPostgreSQLをセットアップする際に役立つと思います。

##### macOS

macOSではPostgreSQLアプリケーションを使用しますが、まずは**postgres**コマンドラインユーティリティを使用して、新しいデータベースとこのデータベース内のテーブルを作成するところから始めます。**postgres**アカウントを使用して、新しいデータベース**hybook**を作成します。


     1 Marks-MacBook:datastores $ psql -d "postgres"
     2 psql (9.6.3)
     3 Type "help" for help.
     4 
     5 postgres=# \d
     6 No relations found.
     7 postgres=# CREATE DATABASE hybook;
     8 CREATE DATABASE
     9 postgres=# \q
    10 Marks-MacBook:datastores $ 


データベース **hybook** に **news** テーブルを作成します。


    1 markw $ psql -d "hybook"
    2 psql (9.6.3)
    3 Type "help" for help.
    4 
    5 hybook=# CREATE TABLE news (uri VARCHAR(50) not null, title VARCHAR(50), articletext\
    6  VARCHAR(500), nlpdata VARCHAR(50)); 
    7 CREATE TABLE
    8 hybook=# 


##### Linux

**Ubuntu** **Linux** の場合、まず PostgreSQL をインストールし、**sudo** を使って **postgres** というアカウントを使用します。

ローカルサーバーを起動する。


    1 sudo su - postgres
    2 /usr/lib/postgresql/10/bin/pg_ctl -D /var/lib/postgresql/10/main -l logfile start


そして、サーバーを停止させることができます。


    1 sudo su - postgres
    2 /usr/lib/postgresql/10/bin/pg_ctl -D /var/lib/postgresql/10/main -l logfile stop


PostgreSQLサーバーが起動しているときは、**psql**コマンドライン・プログラムを使用することができます。


     1 sudo su - postgres
     2 psql
     3 
     4 postgres@pop-os:~$ psql -d "hybook"
     5 psql (10.7 (Ubuntu 10.7-0ubuntu0.18.10.1))
     6 Type "help" for help.
     7 
     8 hybook=# CREATE TABLE news (uri VARCHAR(50) not null, title VARCHAR(50), articletext\
     9  VARCHAR(500), nlpdata VARCHAR(50));
    10 CREATE TABLE
    11 hybook=# \d
    12         List of relations
    13  Schema | Name | Type  |  Owner   
    14 --------+------+-------+----------
    15  public | news | table | postgres
    16 (1 row)


#### HyとPostgreSQLを使う

Hy（あるいは他のLisp言語やHaskellの場合も）を使うとき、私は通常REPLでコーディングと新しいライブラリやAPIの実験の両方を始めます。ここでは、最後のセクションで作成したデータベース**hybook**のテーブル**news**に対してpsycopgをどのように使用できるかを高いレベルで見てみましょう。


     1 Marks-MacBook:datastores $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (import json psycopg2)
     4 => (setv conn (psycopg2.connect :dbname "hybook" :user "markw"))
     5 => (setv cur (conn.cursor))
     6 => (cur.execute "INSERT INTO news VALUES (%s, %s, %s, %s)"
     7       ["http://knowledgebooks.com/schema" "test schema"
     8       "text in article" (json.dumps {"type" "news"})])
     9 => (conn.commit)
    10 => (cur.execute "SELECT * FROM news")
    11 => (for [record cur]
    12 ... (print record))
    13 ('http://knowledgebooks.com/schema', 'test schema', 'text in article',
    14  '{"type": "news"}')
    15 => (cur.execute "SELECT nlpdata FROM news")
    16 => (for [record cur]
    17 ... (print record))
    18 ('{"type": "news"}',)
    19 => (cur.execute "SELECT nlpdata FROM news")
    20 => (for [record cur]
    21 ... (print (json.loads (first record))))
    22 {'type': 'news'}
    23 => 


6-8行目と13-14行目で、PostgreSQLのネイティブなJSONサポートを使用していることにお気づきでしょう。

この本のほとんどの資料と同様に、Hy REPLを開いて、この本の対話式REPLの例にあるAPIとコードを実験していることを望みます。

ファイル **postgres_lib.hy** は、データベースへのアクセス、データの追加、変更、および問い合わせのためによく使われる機能を、短い再利用可能なライブラリでラップしています。


     1 (import [psycopg2 [connect]])
     2 
     3 (defn connection-and-cursor [dbname username]
     4   (setv conn (connect :dbname dbname :user username))
     5   (setv cursor (conn.cursor))
     6   [conn cursor])
     7 
     8 (defn query [cursor sql &optional variable-bindings]
     9   (if variable-bindings
    10     (cursor.execute sql variable-bindings)
    11     (cursor.execute sql)))


8-11行目の関数**query**は任意のSQLコマンドを実行するため，データベースへの問い合わせだけでなく，適切なSQLコマンドを使用して行の削除，行の更新，テーブルの作成と削除を行うことができます．

次のファイル **postgres_example.hy** は，今定義したライブラリの使用例である．


     1 #!/usr/bin/env hy
     2 
     3 (import [postgres-lib [connection-and-cursor query]])
     4 
     5 (defn test-postgres-lib []
     6   (setv [conn cursor] (connection-and-cursor "hybook" "markw"))
     7   (query cursor "CREATE TABLE people (name TEXT, email TEXT);")
     8   (conn.commit)
     9   (query cursor "INSERT INTO people VALUES ('Mark', 'mark@markwatson.com')")
    10   (query cursor "INSERT INTO people VALUES ('Kiddo', 'kiddo@markwatson.com')")
    11   (conn.commit)
    12   (query cursor "SELECT * FROM people")
    13   (print (cursor.fetchall))
    14   (query cursor "UPDATE people SET name = %s WHERE email = %s"
    15       ["Mark Watson" "mark@markwatson.com"])
    16   (query cursor "SELECT * FROM people")
    17   (print (cursor.fetchall))
    18   (query cursor "DELETE FROM people  WHERE name = %s" ["Kiddo"])
    19   (query cursor "SELECT * FROM people")
    20   (print (cursor.fetchall))
    21   (query cursor "DROP TABLE people;")
    22   (conn.commit)
    23   (conn.close))
    24 
    25 (test-postgres-lib)


以下は、この例のHyスクリプトの出力です。


    1 Marks-MacBook:datastores $ ./postgres_example.hy
    2 [('Mark', 'mark@markwatson.com'), ('Kiddo', 'kiddo@markwatson.com')]
    3 [('Kiddo', 'kiddo@markwatson.com'), ('Mark Watson', 'mark@markwatson.com')]
    4 [('Mark Watson', 'mark@markwatson.com')]


私は他のどのデータストアよりもPostgreSQLを使用しています。PostgreSQLサーバの管理方法とアプリケーションソフトウェアの書き方を学ぶために時間を取ることは、仕事で新しいアイデアを試作したりデータ指向の製品を開発したりする際に時間と労力を節約することができます。私はPostgreSQLを使うのが大好きで、個人的には、非常に小さなデータベース作業やアプリケーションにのみSqliteを使用しています。

### 「rdflib」ライブラリを使用したRDFデータ

SqliteとPostgreSQLに関する最後の2つのセクションでは、あなた自身の仕事で使う可能性が高い例を提供しましたが、ここでは、より難解ではありますが、セマンティックWebやリンクデータのアプリケーションでデータスキーマとRDFトリプルグラフデータを使用するためのRDF表記法について説明します。私は、GoogleのKnowledge Graphと連携したグラフデータベースを勤務時代に使っていましたし、リンクデータを使ったコンサルティングプロジェクトも何件か経験しています。セマンティックウェブやリンクデータについて深く掘り下げ、ナレッジグラフを自動的に作成する例も開発する2章では、このセクションの資料を理解する必要があります。

私の仕事では、グラフデータの表記法としてRDFを、RDFデータにおけるデータ型や関係性の型を正式に定義するためにRDFS（RDF Schema）を、そして時にはRDFデータに関する推論や明示的に定義したデータから新しいグラフトリプルデータを推測するためにOWL（Web Ontology Language）を使用しています。RDFは最も実用的なリンクデータ・ツールなので、ここではRDFだけを取り上げます。RDFやRDFS、OWLについてより深く知りたい方は、私の他のセマンティックウェブに関する本を参照してください。

次の章では、セマンティックウェブとリンクされたデータリソースの使用について、もう少し詳しく説明します。ここでは、データストアとしての**rdflib**ライブラリの使用、ディスクやWebリソースからのRDFデータの読み込み、RDF文（主語、述語、目的語を含むトリプル）の追加、インメモリグラフを標準RDF XML、Turtle、NT形式のいずれかのファイルにシリアライズすることについて学習する。

次のREPLセッションでは、**rdflib**ライブラリのインポート、私の個人ウェブサイトからのRDF（XML形式）のフェッチ、グラフ内のトリプルのNT形式でのプリントアウト、グラフのクエリ方法を示しています。この RDF の大部分は 2005 年に私の Web サイトに追加されましたが、その後、いくつかの更新が行われました。以下の REPL セッションは、**rdflib** がどのように使用されているかを説明するために、いくつかのリストに分割されています (一部の長い出力は削除されています)。最初の REPL リストでは、私の Web サイトから XML 形式の RDF ファイルをロードし、NT 形式で出力しています。NT形式では、主語・述語・目的語をすべて空白で区切って1行にし、ピリオドで終了させるか、下図のように、主語を1行に、述語と目的語をさらに2行にインデントして印刷することができます。どちらの場合も、ピリオド文字". "が検索RDF NT文の終了に使用されます。文は任意の順序で表示される。


     1 Marks-MacBook:datastores $ hy
     2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
     3 => (import [rdflib [Graph]])
     4 => (setv graph (Graph))
     5 => (graph.load "http://markwatson.com/index.rdf")
     6 => (for [[subject predicate object] graph]
     7 ... (print subject "\n  " predicate "\n  " object " ."))
     8 http://markwatson.com/index.rdf#mark_watson_consulting_services 
     9    http://www.w3.org/1999/02/22-rdf-syntax-ns#label 
    10    Mark Watson Consulting Services  .
    11 http://markwatson.com/index.rdf#mark_watson 
    12    http://www.w3.org/2000/10/swap/pim/contact#firstName 
    13    Mark  .
    14 http://markwatson.com/index.rdf#mark_watson 
    15    http://www.ontoweb.org/ontology/1#name 
    16    Mark Watson  .
    17 http://www.markwatson.com/ 
    18    http://purl.org/dc/elements/1.1/language 
    19    en-us  .
    20 http://markwatson.com/index.rdf#mark_watson 
    21    http://www.ontoweb.org/ontology/1#researchTopic 
    22    Semantic Web  .
    23 http://www.markwatson.com/ 
    24    http://purl.org/dc/elements/1.1/date 
    25    2005-7-10  .
    26 http://markwatson.com/index.rdf#mark_watson 
    27    http://www.ontoweb.org/ontology/1#researchTopic 
    28    RDF and RDF Schema  .
    29 http://markwatson.com/index.rdf#mark_watson 
    30    http://www.ontoweb.org/ontology/1#researchTopic 
    31    ontologies  .
    32 http://www.markwatson.com/ 
    33    http://purl.org/dc/elements/1.1/title 
    34    Mark Watson's Home Page  .
    35 http://markwatson.com/index.rdf#mark_watson 
    36    http://www.w3.org/2000/10/swap/pim/contact#mailbox 
    37    mailto:markw@markwatson.com  .
    38 http://markwatson.com/index.rdf#mark_watson 
    39    http://www.w3.org/2000/10/swap/pim/contact#homepage 
    40    http://www.markwatson.com/  .
    41 http://markwatson.com/index.rdf#mark_watson 
    42    http://www.w3.org/2000/10/swap/pim/contact#fullName 
    43    Mark Watson  .
    44 http://markwatson.com/index.rdf#mark_watson_consulting_services 
    45    http://www.ontoweb.org/ontology/1#name 
    46    Mark Watson Consulting Services  .
    47 http://markwatson.com/index.rdf#mark_watson 
    48    http://www.w3.org/2000/10/swap/pim/contact#company 
    49    Mark Watson Consulting Services  .
    50 http://markwatson.com/index.rdf#mark_watson 
    51    http://www.w3.org/1999/02/22-rdf-syntax-ns#type 
    52    http://www.w3.org/2000/10/swap/pim/contact#Person  .
    53 http://markwatson.com/index.rdf#mark_watson 
    54    http://www.w3.org/1999/02/22-rdf-syntax-ns#value 
    55    Mark Watson  .
    56 http://www.markwatson.com/ 
    57    http://purl.org/dc/elements/1.1/creator 
    58    http://markwatson.com/index.rdf#mark_watson  .
    59 http://www.markwatson.com/ 
    60    http://purl.org/dc/elements/1.1/description 
    61    
    62       Mark Watson is the author of 16 published books and a consultant specializing \
    63 in artificial intelligence and Java technologies.
    64       .
    65 http://markwatson.com/index.rdf#mark_watson 
    66    http://www.w3.org/1999/02/22-rdf-syntax-ns#type 
    67    http://www.ontoweb.org/ontology/1#Person  .
    68 http://markwatson.com/index.rdf#mark_watson 
    69    http://www.w3.org/2000/10/swap/pim/contact#motherTongue 
    70    en  .
    71 http://markwatson.com/index.rdf#mark_watson 
    72    http://www.w3.org/1999/02/22-rdf-syntax-ns#type 
    73    http://markwatson.com/index.rdf#Consultant  .
    74 http://markwatson.com/index.rdf#mark_watson_consulting_services 
    75    http://www.w3.org/1999/02/22-rdf-syntax-ns#type 
    76    http://www.ontoweb.org/ontology/1#Organization  .
    77 http://markwatson.com/index.rdf#mark_watson 
    78    http://www.w3.org/1999/02/22-rdf-syntax-ns#label 
    79    Mark Watson  .
    80 http://markwatson.com/index.rdf#Sun_ONE 
    81    http://www.w3.org/1999/02/22-rdf-syntax-ns#type 
    82    http://www.ontoweb.org/ontology/1#Book  .
    83 => 


SPARQL クエリ言語については次の章で詳しく説明しますが、今のところ、SPARQL は SQL クエリに似ていることに注意してください。 SPARQL クエリーは、単純なパターンに一致するグラフ内のトリプルの検索、複雑なパターンのマッチング、グラフ内のトリプルの更新と削除を行うことができます。次の単純な SPARQL クエリは、述語が <http://www.w3.org/2000/10/swap/pim/contact#company> と等しいすべてのトリプルを見つけ、一致するトリプルのサブジェクトとオブジェクトを出力します。


    84 => (for [[subject object
    85 ... (graph.query
    86 ...   "select ?s ?o where { ?s <http://www.w3.org/2000/10/swap/pim/contact#company> \
    87 ?o }")]
    88 ... (print subject "\n contact company: " object))
    89 http://markwatson.com/index.rdf#mark_watson
    90   contact company:  Mark Watson Consulting Services


次の章では、SPARQL クエリ言語の例をさらに見ていきます。今のところ、**select** クエリー文の一般的な形式は、クエリー変数のリスト（クエスチョンマークで始まる名前）と、中括弧内の **where** 節で、マッチングパターンを含んでいることに注目してください。このSPARQLクエリーは単純ですが、SQLクエリーと同様に、SPARQLクエリーは非常に複雑になる可能性があります。本書ではSPARQLについて軽く触れるにとどめています。 私が以前に書いた2冊のセマンティックウェブに関する本のPDF版を無料で入手することができます。[実践セマンティックウェブとリンクされたデータアプリケーション、Java、Scala、Clojure、JRuby版](https://markwatson.com/opencontentdata/book_java.pdf) と [実践セマンティックウェブとリンクされたデータアプリケーション、Common Lisp版](https://markwatson.com/opencontentdata/book_lisp.pdf)です。私の[book web page](https://markwatson.com/books/)に関連するgit repos等へのリンクがあります。

先ほども述べたように、私のウェブサイトのRDFデータは2005年以来、基本的に変更されていません。最近、Googleで契約社員として働き、Capital OneでDeep Learningエンジニアリング・マネージャーとして働いていたことを示すために更新したい場合はどうすればよいでしょうか。次のリストでは、同じREPLセッションを続けながら、追加の仕事を示す2つのRDFステートメントを追加し、新しく更新されたグラフを3つのフォーマットでシリアライズする方法を説明します。XML、turtle (私のお気に入りの RDF 表記法)、NT の 3 つの形式で、更新された新しいグラフをシリアライズする方法を示します。


     89 => (import rdflib)
     90 => (setv mark-node (rdflib.URIRef  "http://markwatson.com/index.rdf#mark_watson"))
     91 => mark-node
     92 rdflib.term.URIRef('http://markwatson.com/index.rdf#mark_watson')
     93 => (setv company-node (rdflib.URIRef "http://www.w3.org/2000/10/swap/pim/contact#com\
     94 pany"))
     95 => (graph.add [mark-node company-node (rdflib.Literal "Google")])
     96 => (graph.add [mark-node company-node (rdflib.Literal "Capital One")])
     97 => (for [[subject object]
     98 ...       (graph.query
     99 ...         "select ?s ?o where { ?s <http://www.w3.org/2000/10/swap/pim/contact#com\
    100 pany> ?o }")]
    101 ... (print subject " contact company: " object))
    102 http://markwatson.com/index.rdf#mark_watson  contact company:  Mark Watson Consultin\
    103 g Services
    104 http://markwatson.com/index.rdf#mark_watson  contact company:  Google
    105 http://markwatson.com/index.rdf#mark_watson  contact company:  Capital One
    106 => 
    107 => (graph.serialize :format "pretty-xml")
    108 <?xml version="1.0" encoding="utf-8"?>
    109 <rdf:RDF\n  xmlns:dc="http://purl.org/dc/elements/1.1/"
    110   xmlns:contact="http://www.w3.org/2000/10/swap/pim/contact#"
    111   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    112   xmlns:ow="http://www.ontoweb.org/ontology/1#"
    113   xmlns:ns1="http://markwatson.com/index.rdf#">
    114 
    115     LOTS OF STUFF NOT SHOWN
    116 
    117 </rdf:Description>\n</rdf:RDF>


私は、XML表記よりもTurtle RDF表記の方が読みやすく、理解しやすいので好きです。ここでは、118行目でグラフ（90～96行目で新しいノードを追加したもの）をTurtleに直列化しています。


    118 => (graph.serialize :format "turtle")
    119 @prefix contact: <http://www.w3.org/2000/10/swap/pim/contact#> .
    120 @prefix dc: <http://purl.org/dc/elements/1.1/> .
    121 @prefix ns1: <http://markwatson.com/index.rdf#> .
    122 @prefix ow: <http://www.ontoweb.org/ontology/1#> .
    123 @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    124 @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    125 @prefix xml: <http://www.w3.org/XML/1998/namespace> .
    126 @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
    127 
    128 ns1:mark_watson_consulting_services a ow:Organization ;
    129     ow:name "Mark Watson Consulting Services" ;
    130     rdf:label "Mark Watson Consulting Services" .
    131 <http://www.markwatson.com/> 
    132     dc:creator ns1:mark_watson ;
    133     dc:date "2005-7-10" ;
    134     dc:description 
    135           "Mark Watson is the author of 16 published books and a consultant
    136           specializing in artificial intelligence and Java technologies." ;
    137      dc:format "text/html" ;
    138      dc:language "en-us" ;
    139      dc:title "Mark Watson\'s Home Page" ;
    140      dc:type "Consultant Homepage" .
    141 ns1:mark_watson a ns1:Consultant,
    142                   ow:Person,
    143                   contact:Person ;
    144                 ow:name "Mark Watson" ;
    145                 ow:researchTopic "RDF and RDF Schema",
    146                                  "Semantic Web",
    147                                  "ontologies" ;
    148                 rdf:label "Mark Watson" ;
    149                 rdf:value "Mark Watson" ;
    150                 contact:company "Capital One",
    151                                 "Google",
    152                                 "Mark Watson Consulting Services" ;
    153                 contact:familyName "Watson" ;
    154                 contact:firstName "Mark" ;
    155                 contact:fullName "Mark Watson" ;
    156                 contact:homepage <http://www.markwatson.com/> ;
    157                 contact:mailbox <mailto:markw@markwatson.com> ;
    158                 contact:motherTongue "en" .


Turtle形式に加えて、私はよりシンプルなNT形式も使っています。これはURIプレフィックスをインラインで配置し、Turtleと違ってプレフィックス検索を使いません。ここでは、159行目でNT形式にシリアライズしています。


    159 => (graph.serialize :format "nt")
    160 <http://markwatson.com/index.rdf#Sun_ONE> <http://www.ontoweb.org/ontology/1#booktit\
    161 le> "Sun ONE Services - J2EE" .
    162 <http://www.markwatson.com/> <http://purl.org/dc/elements/1.1/language> "en-us" .
    163 <http://markwatson.com/index.rdf#Sun_ONE> <http://www.ontoweb.org/ontology/1#author>\
    164  <http://markwatson.com/index.rdf#mark_watson> .
    165 <http://www.markwatson.com/> <http://purl.org/dc/elements/1.1/date> "2005-7-10" .
    166 <http://markwatson.com/index.rdf#mark_watson> <http://www.ontoweb.org/ontology/1#res\
    167 earchTopic> "ontologies" .
    168 <http://www.markwatson.com/> <http://purl.org/dc/elements/1.1/type> "Consultant Home\
    169 page" .
    170 <http://markwatson.com/index.rdf#mark_watson> <http://www.w3.org/2000/10/swap/pim/co\
    171 ntact#homepage> <http://www.markwatson.com/> .
    172 <http://markwatson.com/index.rdf#mark_watson> <http://www.w3.org/2000/10/swap/pim/co\
    173 ntact#company> "Google" .
    174 <http://markwatson.com/index.rdf#mark_watson> <http://www.w3.org/2000/10/swap/pim/co\
    175 ntact#company> "Capital One" .
    176 <http://markwatson.com/index.rdf#mark_watson> <http://www.ontoweb.org/ontology/1#res\
    177 earchTopic> "Semantic Web" .
    178 <http://markwatson.com/index.rdf#mark_watson> <http://www.ontoweb.org/ontology/1#res\
    179 earchTopic> "RDF and RDF Schema" .
    180 <http://www.markwatson.com/> <http://purl.org/dc/elements/1.1/title> "Mark Watson\'s\
    181  Home Page" .
    182 <http://markwatson.com/index.rdf#mark_watson> <http://www.w3.org/2000/10/swap/pim/co\
    183 ntact#mailbox> <mailto:markw@markwatson.com> .
    184 <http://www.markwatson.com/> <http://purl.org/dc/elements/1.1/format> "text/html" .
    185 <http://markwatson.com/index.rdf#mark_watson> <http://www.w3.org/2000/10/swap/pim/co\
    186 ntact#fullName> "Mark Watson" .
    187 <http://markwatson.com/index.rdf#mark_watson> <http://www.w3.org/1999/02/22-rdf-synt\
    188 ax-ns#type> <http://www.w3.org/2000/10/swap/pim/contact#Person> .
    189 <http://www.markwatson.com/> <http://purl.org/dc/elements/1.1/creator> <http://markw\
    190 atson.com/index.rdf#mark_watson> .
    191 
    192       LOTS OF STUFF NOT SHOWN
    193 => 


#### rdflib のバックエンドとしてリレーショナルデータベースを使用する

デフォルトでない rdflib のバックエンドを使うことは取り上げませんが、もし大きな RDF データセットをロードしてデータを持続させたいなら、SQLAlchemy プラグイン拡張を使うことができます。

まず、Python の SQLAlchemy RDF ライブラリをインストールします。


    1     pip install rdflib_sqlalchemy


そして、[rdflib-sqlalchemy](https://github.com/RDFLib/rdflib-sqlalchemy) github リポジトリにあるテスト例に従ってください。

もし大きなRDFデータセットを使う必要があるなら、rdflibを使わずに、SPARQLを使って[OpenLink Virtuoso](https://en.wikipedia.org/wiki/Virtuoso_Universal_Server) や [GraphDB™ Free Edition](https://www.ontotext.com/products/graphdb/graphdb-free/) などのフリーまたはオープンソースのスタンドアロンRDFデータストアにアクセスするのがいいと思います。また、商用のAllegroGraphやStardog RDFサーバー製品も好きで、おすすめです。

### まとめ

次の章では、RDF と SPARQL の実用的な使用方法について、より詳しく説明します。このセクションのREPLの例を通して、**rdflib**の使い方の基本を理解すれば、次の章のより抽象的な内容をより簡単に理解できるようになるからです。

## リンクトデータとセマンティックウェブ

Tim Berners Lee、James Hendler、Ora Lassilaは2001年にScientific American誌に記事を書き、セマンティックウェブという言葉を紹介した。 ここでは、セマンティックウェブを大文字で書かず、セマンティックウェブと似たような用語であるリンクデータを多少互換的に使用することにする。

前章の[「rdflib」ライブラリを使ったRDFデータ](#rdflibintro)の項を読まれたことと思います。

ウェブが関連するウェブページ間のリンクを可能にするのと同じように、リンクデータはウェブ上の関連するデータを一緒にリンクすることをサポートします。セマンティックウェブは、人間の知識のすべてをウェブ上のデータとして表現し、ソフトウェアエージェントが質問に答えたり、研究を行ったり、既存のデータから新しいデータを推測したりできるような形になる可能性を秘めています。

「ウェブ」が人間の読者のために情報を記述するのに対し、セマンティックウェブはソフトウェアエージェントが取り込むための構造化されたデータを提供することを目的としています。この違いは、人間の読者のために作られたWikiPediaと、WikiPediaトピックの情報ボックスを利用してWikiPediaトピックを記述するRDFデータを自動的に抽出するDBPediaを比較すれば明らかであろう。 私が住んでいる町、アリゾナ州セドナのWikiPediaトピックを見てみましょう。英語版の[WikiPedia topic page for Sedona https://en.wikipedia.org/wiki/Sedona,\_Arizona](https://en.wikipedia.org/wiki/Sedona,_Arizona) の情報ボックスが[DBPedia page http://dbpedia.org/page/Sedona,\_Arizona](http://dbpedia.org/page/Sedona,_Arizona) にどうマッピングされているかを示しています。 これらのWikiPediaとDBPediaのURIは、2つのブラウザのタブで開き、参照できるようにしておいてください。

WikiPediaのページのフォーマットはよく知られていると思うので、アリゾナ州セドナを主題とするRDF文を人間が読める形で示したセドナのDBPediaのページを見てみよう。RDFは、データのモデル化と表現に用いられる。RDFは3つの値で定義されるので、RDF文のインスタンスは3つの部分を持つ*triple*と呼ばれる。

- subject：URI（以下、リソースと呼ぶ）。
- property: URI (リソース)。
- value: URI（リソース）またはリテラル値（文字列など）。

Sedona関連の各トリプルのサブジェクトは、上記のDBPedia human readableページのURIです。RDFトリプルのサブジェクトとプロパティの参照は、ほとんどの場合、ウェブ上の情報に対して実体を基づかせることができるURIになります。Sedonaのhuman readableページには、いくつかのプロパティとその値が記載されています。プロパティの1つは「dbo:areaCode」で、「dbo」は名前空間参照（この場合、[DatatypeProperty](http://www.w3.org/2002/07/owl#DatatypeProperty)）である。

次の2つの図は、リンクデータの抽象的な表現と、リソースとプロパティに実際のWeb URIを使用したリンクデータのサンプルである。


![Abstract RDF representation with 2 Resources, 2 literal values, and 3 Properties](/site_images1/hy-lisp-python/rdf1.png)



![Concrete example using RDF seen in last chapter showing the RDF representation with 2 Resources, 2 literal values, and 3 Properties](/site_images1/hy-lisp-python/rdf2.png)


前章でSPARQLクエリ（RDFデータのSPARQLはリレーショナルデータベースのクエリのSQLに似ています）を見てきました。最後の図にあるRDFを使った別の例を見てみましょう。


    1     select ?v where {  <http://markwatson.com/index.rdf#Sun_ONE>
    2                        <http://www.ontoweb.org/ontology/1#booktitle>
    3                        ?v }


このクエリでは、"Sun ONE Services - J2EE "という結果が返されるはずです。もし、書籍であるすべてのURIリソースを、タイトルのリテラル値でクエリしたい場合は


    1     select ?s ?v where {  ?s
    2                           <http://www.ontoweb.org/ontology/1#booktitle>
    3                           ?v }


s** と **?v** は任意のクエリ変数名で、ここでは "subject" と "value" を表していることに注意してください。もっと説明的な変数名を使うこともできます。


    1     select ?bookURI ?bookTitle where 
    2         { ?bookURI
    3           <http://www.ontoweb.org/ontology/1#booktitle>
    4           ?bookTitle }


次の章では、生のテキスト入力からRDFデータを生成するツールを書くので、RDFの例についてもう少し深く掘り下げることになります。今のところは、RDFステートメントがトリプルとして表されること、Web URIがモノやプロパティ、時には値を表すこと、URIを手動でたどって（しばしば「デリフェレンス」と呼ばれます）、その参照先を人間が読める形で確認できること、などを理解していただきたいと考えています。

### リソース記述フレームワーク（RDF）を理解する

ウェブ上のテキストデータは、ヘッダ、ページタイトル、アンカーリンクなどのHTML要素の形で何らかの構造を持っているが、この構造はソフトウェアエージェントが一般に使うにはあまりに不正確である。RDFは、構造化されたデータをより正確に符号化するための手法である。

前章で私のWebサイトのRDFデータを使って、**rdflib**というPythonライブラリを使ってRDFデータにアクセス、操作、問い合わせをするための「配管」を紹介しました。

### rdflibで提供されるリソースの名前空間

**rdflib**では、以下の標準的な名前空間があらかじめ定義されています。

-   RDF <https://www.w3.org/TR/rdf-syntax-grammar/>
-   RDFS <https://www.w3.org/TR/rdf-schema/>
-   OWL <http://www.w3.org/2002/07/owl#>
-   XSD <http://www.w3.org/2001/XMLSchema#>
-   FOAF <http://xmlns.com/foaf/0.1/>
-   SKOS <http://www.w3.org/2004/02/skos/core#>
-   DOAP <http://usefulinc.com/ns/doap#>
-   DC <http://purl.org/dc/elements/1.1/>
-   DCTERMS <http://purl.org/dc/terms/>
-   VOID <http://rdfs.org/ns/void#>

Friend of a Friend (FOAF) 名前空間について調べてみましょう。上記のFOAF <http://xmlns.com/foaf/0.1/>のリンクをクリックすると、FOAFコアの定義が見つかります。

```
 1     Agent
 2     Person
 3     name
 4     title
 5     img
 6     depiction (depicts)
 7     familyName
 8     givenName
 9     knows
10     based_near
11     age
12     made (maker)
13     primaryTopic (primaryTopicOf)
14     Project
15     Organization
16     Group
17     member
18     Document
19     Image
```

とソーシャルウェブのために

```
 1 nick
 2 mbox
 3 homepage
 4 weblog
 5 openid
 6 jabberID
 7 mbox_sha1sum
 8 interest
 9 topic_interest
10 topic (page)
11 workplaceHomepage
12 workInfoHomepage
13 schoolHomepage
14 publications
15 currentProject
16 pastProject
17 account
18 OnlineAccount
19 accountName
20 accountServiceHomepage
21 PersonalProfileDocument
22 tipjar
23 sha1
24 thumbnail
25 logo
```

これで、RDFデータの一般的なスキーマをいくつか見てきました。Webサイトのアノテーションに広く使われている別のスキーマは、ここでの例では必要ありませんが、[schema.org](https://schema.org)です。では、Hy REPLセッションを使って名前空間を調べ、**rdflib**を使ってプログラム的にRDFを作成してみましょう。

```
 1 Marks-MacBook:database $ hy
 2 hy 0.17.0+108.g919a77e using CPython(default) 3.7.3 on Darwin
 3 => (import [rdflib.namespace [FOAF]])
 4 => FOAF
 5 Namespace('http://xmlns.com/foaf/0.1/')
 6 => FOAF.name
 7 rdflib.term.URIRef('http://xmlns.com/foaf/0.1/name')
 8 => FOAF.title
 9 rdflib.term.URIRef('http://xmlns.com/foaf/0.1/title')
10 => (import rdflib)
11 => (setv graph (rdflib.Graph))
12 => (setv mark (rdflib.BNode))
13 => (graph.bind "foaf" FOAF)
14 => (import [rdflib [RDF]])
15 => (graph.add [mark RDF.type FOAF.Person])
16 => (graph.add [mark FOAF.nick (rdflib.Literal "Mark" :lang "en")])
17 => (graph.add [mark FOAF.name (rdflib.Literal "Mark Watson" :lang "en")])
18 => (for [node graph] (print node))
19 (rdflib.term.BNode('N21c7fa7385b545eb8a7e3821b7cb5'), rdflib.term.URIRef('http://www\
20 .w3.org/1999/02/22-rdf-syntax-ns#type'), rdflib.term.URIRef('http://xmlns.com/foaf/0\
21 .1/Person'))
22 (rdflib.term.BNode('N21c7fa7385b545eb8a7e3821b7cb5'), rdflib.term.URIRef('http://xml\
23 ns.com/foaf/0.1/name'), rdflib.term.Literal('Mark Watson', lang='en'))
24 (rdflib.term.BNode('N21c7fa7385b545eb8a7e3821b7cb5'), rdflib.term.URIRef('http://xml\
25 ns.com/foaf/0.1/nick'), rdflib.term.Literal('Mark', lang='en'))
26 => (graph.serialize :format "pretty-xml")
27 b'<?xml version="1.0" encoding="utf-8"?>
28 <rdf:RDF
29     xmlns:foaf="http://xmlns.com/foaf/0.1/"
30     xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
31 >
32   <foaf:Person rdf:nodeID="N21c7fa7385b545eb8a7e3821b75b9cb5">
33     <foaf:name xml:lang="en">Mark Watson</foaf:name>
34     <foaf:nick xml:lang="en">Mark</foaf:nick>
35   </foaf:Person>
36 </rdf:RDF>\n'
37 => (graph.serialize :format "turtle")
38 @prefix foaf: <http://xmlns.com/foaf/0.1/> .
39 @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
40 @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
41 @prefix xml: <http://www.w3.org/XML/1998/namespace> .
42 @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
43 
44 [] a foaf:Person ;
45      foaf:name "Mark Watson"@en ;
46      foaf:nick "Mark"@en .
47 
48 => (graph.serialize :format "nt")
49 _:N21c7fa7385b545eb8a7e3821b75b9cb5
50    <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
51    <http://xmlns.com/foaf/0.1/Person> .
52 _:N21c7fa7385b545eb8a7e3821b75b9cb5 <http://xmlns.com/foaf/0.1/name> "Mark Watson"@e\
53 n .
54 _:N21c7fa7385b545eb8a7e3821b75b9cb5 <http://xmlns.com/foaf/0.1/nick> "Mark"@en .
55 => 
```

### SPARQL クエリ言語を理解する

本書の資料の目的からすると、任意のRDFデータソースと簡単なクエリで**rdflib**を使い始めるには、ここと最後の章にある2つのサンプルSPARQLクエリで十分でしょう。

Apache Foundationには[good introduction to SPARQL](https://jena.apache.org/tutorials/sparql.html)があるので、より詳しい情報を得るにはそちらを参照してください。

### Python **rdflib** ライブラリのラッピング

RDFデータソースを探求し、あなたのプロジェクトにリンクデータ/セマンティックウェブ技術の利用を検討するための十分な動機を提供できたと思います。

インストールは、[source code for **rdflib**](https://github.com/RDFLib/rdflib) を使うか、以下の方法で行います。

```
pip install rdflib
```

プログラミング言語に関係なく、ライブラリに依存する場合、私はソースコードの最新版を手元に置いておきたいと考えています。ライブラリのコードを読むことができるのは、時として代えがたいものです。

次の章では、自然言語処理を使って生のテキストから構造化情報を抽出し、RDFデータを自動生成します。

## ナレッジグラフ作成ツール

ナレッジグラフとは、私はよく **KG** と略しますが、スキーマを使って型（オブジェクトとオブジェクト間の関係の両方）とプロパティ値をオブジェクトに結びつけるプロパティを定義したグラフデータベースのことです。 ナレッジグラフという言葉は、一般的な言葉であると同時に、私が2013年に勤務していたGoogleで使われていた特定のナレッジグラフを指すこともあります。ここでは、グラフデータベースに知識を格納する一般的な技術を指すためにKGを使用しています。

ここで開発するアプリケーション、Knowledge Graph Creator（私はしばしばKGCreatorと呼びます）は、入力テキストから小さなナレッジグラフを生成するために使用するユーティリティです。

知識工学と知識表現は1980年代に始まった学問分野であり、現在も研究テーマとして、また産業界で利用されている。私は、リンクデータ、セマンティックウェブ、そしてKGを、この初期の研究の延長線上にあると考えています。

ここでは、RDFをベースにしています。また、産業界で広く使われているが、ここでは取り上げない一般的なタイプのKGがある。Neo4Jで使われているプロパティ・グラフである。プロパティ・グラフは、グラフ・ノードが持つリンクの数に制約がなく、ノード・データおよびノード間のプロパティ・リンクとして一般的なデータ構造を格納することができる一般的なグラフである。プロパティリンクは、グラフのノードと同様に属性を持つことができる。

サブジェクト／プロパティ／値のRDFトリプルで表現されるセマンティックウェブデータは、プロパティグラフよりも制約が大きいが、グラフの中に暗黙的に存在するが明示的に記述されていないデータをうまく利用するための強力な論理推論をサポートしている（つまり、データの推論がより容易になる）。

前章でRDFデータについて少し詳しく説明しました。ここでは、[schema.org](https://schema.org/)のいくつかのスキーマ定義を使って、非構造化テキストをRDFデータに変換するためのツールセットを実装してみます。私は、RDFと一般的なグラフデータベースの両方のアプローチを信じていますが、ここではRDFだけを使うことにします。

歴史的にナレッジグラフは、[Resource Description Framework (RDF)](https://en.wikipedia.org/wiki/Resource_Description_Framework) や [Web Ontology Language (OWL)](https://en.wikipedia.org/wiki/Web_Ontology_Language) といったセマンティックWeb技術を利用していました。私は2010年にセマンティックウェブ技術に関する2冊の本を書きました。【Common Lisp版】(http://markwatson.com/opencontentdata/book_lisp.pdf)(コードはこちら(https://github.com/mark-watson/lisp_practical_semantic_web))と【Java/Clojure/Scala版】(http://markwatson.com/opencontentdata/book_java.pdf)(コードはこちら(https://github.com/mark-watson/java_practical_semantic_web))は無料でPDFを入手することができます。 この章を読み終えたら、これらの無料の本を読んでみてください。

私は、様々なデータソースからナレッジグラフを作成するための個人的な研究プロジェクトを進めています。詳しくは、[my KGCreator web site](http://www.kgcreator.com/)で読むことができます。私の KGCreator ソフトウェアの簡易版は、私の [Haskell Book](https://leanpub.com/haskell-cookbook) と最新の [Common Lisp book](https://leanpub.com/lovinglisp) の両方に実装されています。この例は、Hy言語で実装されていることと、RDFの生成のみをサポートしていることを除けば、私のCommon Lispの実装と同様です。HaskellとCommon Lispの本の例では、Neo4Jグラフ・データベース用のデータも生成しています。

KGとは？構造化されたデータを整理してアクセスし、データとメタデータを他の自動化されたシステムと統合するための現代的な方法である。

ナレッジ・グラフは、グラフ・データを含む単なるグラフ・データベースとは異なります。一般的には、スキーマ、タクソノミ、オントロジーを用いて、データの種類や構造、関係を定義する。

また、KGの主な用途は、組織内の他のシステムをサポートすることであるため、実行可能な側面もある。

### ナレッジグラフの産業利用のすすめ

誰がKGを必要としているのか？どのように始めればいいのでしょうか？

あなたの組織の人々が一般的なWeb検索を行う多くの時間を費やしている場合、それはあなたが人間の検索可能な、ソフトウェアアクセス可能な方法であなたの組織の精選された知識を維持する必要があるという信号である可能性があります。可能なアプリケーションは、公共のウェブ検索APIと、組織内で内部的に使用される知識の検索をミックスした内部検索エンジンです。

いくつかのユースケースを紹介します。

- Googleでは、標準のKnowledge Graphをベースに新しいスキーマやデータを追加した新しい社内システムの調査にKnowledge Graphを使用しました。
- デジタルトランスフォーメーション: すでにあるデータベース内の現在のデータのメタデータを保持するために、KGを使用することから始めてください。メタデータのKGは、仮想のデータレイクを提供することができます。大規模なデータレイクを構築した後、スタッフがデータを見つけられなくなることはよくあることです。一度にすべてを行おうとしないことです。
- シニアの人間の専門知識を捕らえ、保存する。社内の知識のためにオントロジーを構築することは、データの整理方法を理解するのに役立ち、ビジネスプロセスを議論し、モデル化するための共通の語彙を人々に提供することになります。
- 多くの多様なデータソースからのデータを使用するKYC（Know Your Customer）アプリケーション。
- ヘルスケアや金融サービスなどのドメインに関する専門知識を活用して、タクソノミやオントロジーを構築し、利用可能なデータを整理することができます。ほとんどのドメインには、既存の標準的なスキーマ、タクソノミー、オントロジーがあり、それらを特定し、そのまま使用したり、拡張したりすることができます。

始めるには

- 1つのユースケースから小さく始める。
- オブジェクトの種類と関係を特定するスキーマを設計する。
- プロトタイプを開発する際のベースラインとなるような受け入れテストケースをいくつか書いておく。
- 初期のプロトタイププロジェクトでは、利害関係者を多く持ちすぎないようにする。利害関係者となりうる人たちの最初の熱意に基づいて、利害関係者を選ぶようにする。

まず、一つの問題を特定し、使用する最適なデータソースを決定し、現在の問題を解決するのに十分なオントロジーを定義し、「垂直スライス」アプリケーションのプロトタイプを構築するのが良い方法である。 簡単なプロトタイプから得られた教訓は、KGを拡大する際に何が重要で、何に力を入れるべきかを教えてくれる。小さく始めて、小さな開発・評価ステップを数多く踏むことなく、巨大なシステムを構築しようとしないことです。

小規模な組織のためのKGはどうでしょうか？小さな会社は開発リソースが少ないですが、小さく始めて、重要なデータの関係や顧客関係などをモデル化したシステムを導入すれば、過大なリソースは必要ありません。データがどこから来るのか、誰が重要なデータソースの維持に責任を持つのかを把握するだけでも、価値があります。

個人向けのKGはどうでしょうか？カスタムKGの作成にかかる労力を考えると、個人のユースケースとして考えられるのは、商業的に販売するためのKGの開発です。

次に開発するアプリケーションは、新しいKGを素早くブートストラップする方法の一つで、自動的に生成されたRDFを入力し、必要に応じて文を削除したり新しい文を追加したりして手動で管理できるようにするものです。

### KGCreatorアプリケーションの設計

ここで開発したアプリケーションの例では、サブディレクトリ**test_data**にある入力テキストファイルを処理します。**test_data**内の拡張子**.txt**の各ファイルに対して、対応するテキストファイルのオリジンURIを含む拡張子**.meta**のファイルが存在する必要があります。 本書のgitリポジトリには、**test_data**にいくつかのファイルがあり、実験したり、自分のデータと置き換えたりすることができます。

```
$ ls test_data 
test1.meta test1.txt test2.meta test2.txt test3.meta test3.txt
```

また、「 \*.txt 」ファイルには解析用のプレーンテキストが、「 \*.meta 」ファイルには対応するオリジナルのウェブソースURIが格納されています。 ファイルアクセスにspaCyライブラリとPython/Hyの標準ライブラリを使用することで、KGCreatorは簡単に実装することができます。この例の全体設計は次のとおりです。


![Overview of the Knowledge Graph Creator script](/site_images1/hy-lisp-python/kg1.png)


知識グラフ作成ツールは2種類開発する予定です。1つ目は、生成されたRDF文のオブジェクト部分に文字列値を用いたRDFを生成するもの。2つ目の実装は、この文字列値をDBPediaのURIに解決しようとするものです。

先に使ったspaCy NLPライブラリとHy/Pythonの内蔵ライブラリだけを使って、この最初の例（オブジェクト値に文字列を使う）は、次の3つのコードリストに見られるように、わずか58行のHyコードで実装されています。

```
 1 #!/usr/bin/env hy
 2 
 3 (import [os [scandir]])
 4 (import [os.path [splitext exists]])
 5 (import spacy)
 6 
 7 (setv nlp-model (spacy.load "en"))
 8 
 9 (defn find-entities-in-text [some-text]
10   (defn clean [s]
11     (.strip (.replace s "\n" " ")))
12   (setv doc (nlp-model some-text))
13   (map list (lfor entity doc.ents [(clean entity.text) entity.label_])))
```

3行目と4行目では、ディレクトリ内のすべてのファイルを見つける、ファイルが存在するかどうかを確認する、テキストをトークンに分割するという、Pythonの標準ユーティリティを3つインポートしています。7行目では、英語のspaCyモデルをロードし、その値を変数**nlp-model**に保存しています。find-entities-in-text関数はspaCy英語モデルを使ってテキスト中の組織や人などの実体を見つけ、改行文字やその他の不要な空白を削除して実体名をクリーニングします（10、11行目の関数**clean**を入れ子にしています）。REPLでテストを実行することができます。

```
=> (list (find-entities-in-text "John Smith went to Los Angeles to work at IBM"))
[['John Smith', 'PERSON'], ['Los Angeles', 'GPE'], ['IBM', 'ORG']]
```

関数 **find-entities-in-text** は map オブジェクトを返すので、その結果を **list** でラップして、テスト文中のエンティティを出力してみました。spaCyが使用するエンティティタイプは以前の章で定義しましたが、ここでは以下のリストの21〜26行目で定義されているエンティティタイプだけを使用します。

```
14 (defn data2Rdf [meta-data entities fout]
15   (for [[value abbreviation] entities]
16     (if (in abbreviation e2umap)
17       (.write fout (+ "<" meta-data ">\t" (get e2umap abbreviation) "\t" "\""
18                        value "\"" " .\n")))))
19 
20 (setv e2umap {
21   "ORG" "<https://schema.org/Organization>"
22   "LOC" "<https://schema.org/location>"
23   "GPE" "<https://schema.org/location>"
24   "NORP" "<https://schema.org/nationality>"
25   "PRODUCT" "<https://schema.org/Product>"
26   "PERSON" "<https://schema.org/Person>"})
```

28〜39行目では、生成されたRDFデータを書き込むための出力ファイルを開き、入力ディレクトリ内のすべてのテキストファイルをループして、入力ディレクトリ内のテキスト＋メタファイルのペアごとに関数 **process-file** を呼び出しています。

```
28 (defn process-directory [directory-name output-rdf]
29   (with [frdf (open output-rdf "w")]
30     (with [entries (scandir directory-name)]
31       (for [entry entries]
32         (setv [_ file-extension] (splitext entry.name))
33         (if (= file-extension ".txt")
34             (do
35               (setv check-file-name (+ (cut entry.path 0 -4) ".meta"))
36               (if (exists check-file-name)
37                   (process-file entry.path check-file-name frdf)
38                   (print "Warning: no .meta file for" entry.path
39                          "in directory" directory-name))))))))
```

```
40 (defn process-file [txt-path meta-path frdf]
41   
42   (defn read-data [text-path meta-path]
43     (with [f (open text-path)] (setv t1 (.read f)))
44     (with [f (open meta-path)] (setv t2 (.read f)))
45     [t1 t2])
46   
47   (defn modify-entity-names [ename]
48     (.replace ename "the " ""))
49   
50   (setv [txt meta] (read-data txt-path meta-path))
51   (setv entities (find-entities-in-text txt))
52   (setv entities ;; only operate on a few entity types
53         (lfor [e t] entities
54               :if (in t ["NORP" "ORG" "PRODUCT" "GPE" "PERSON" "LOC"])
55               [(modify-entity-names e) t]))
56   (data2Rdf meta entities frdf))
57 
58 (process-directory "test_data" "output.rdf")
```

次章では、生成された出力とその問題点、そしてこれらの問題点を解決する方法について見ていきます。

### RDFでリテラル値を使用する際の問題点

前節のHyスクリプトを使って、input testディレクトリのテキストファイルに対して生成されたRDFの一部を見てみましょう（ほとんどの出力は表示されていません）。各トリプルにおいて、最初の項目であるサブジェクトはデータソースのURI、各ステートメントの2番目の項目は関係（またはプロパティ）を表すURI、3番目の項目はリテラルな文字列値である。

```
<https://newsshop.com/may/a1023.html>
  <https://schema.org/nationality>    "Portuguese" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "Banco Espirito Santo SA" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Person>       "John Evans" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "Banco Espirito" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "The Wall Street Journal" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "IBM" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/location>   "Canada" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "Australian Broadcasting Corporation" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Person> "Frank Smith" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "Australian Writers Guild" .
<https://newsshop.com/may/a1023.html>
  <https://schema.org/Organization>   "American University" .
<https://localnews.com/june/z902.html>
  <https://schema.org/Organization>   "The Wall Street Journal" .
<https://localnews.com/june/z902.html>
  <https://schema.org/location>   "Mexico" .
<https://localnews.com/june/z902.html>
  <https://schema.org/location>   "Canada" .
<https://localnews.com/june/z902.html>
  <https://schema.org/Person> "Bill Clinton" .
<https://localnews.com/june/z902.html>
  <https://schema.org/Organization>   "IBM" .
<https://localnews.com/june/z902.html>
  <https://schema.org/Organization>   "Microsoft" .
<https://abcnews.go.com/US/violent-long-lasting-\
tornadoes-threaten-oklahoma-texas/story?id=63146361>
  <https://schema.org/Person> "Jane Deerborn" .
<https://abcnews.go.com/US/violent-long-lasting-\
tornadoes-threaten-oklahoma-texas/story?id=63146361>
  <https://schema.org/location>   "Texas" .
```

Let's visualize the results in a bash shell:


    $ git clone https://github.com/fatestigma/ontology-visualization
    $ cd ontology-visualization
    $ chmod +x ontology_viz.py
    $ ./ontology_viz.py -o test.dot output.rdf  -O ontology.ttl
    $ # copy the file output.rdf from examples repo directory hy-lisp-python/kgcreator
    $ dot -Tpng -o test.png test.dot
    $ open test.png


Edited to fit on the page, the output looks like:


![](/site_images1/hy-lisp-python/1dot1.png)



![](/site_images1/hy-lisp-python/1dot2.png)



![](/site_images1/hy-lisp-python/1dot3.png)


Because we used literal values, notice how for example the node for the entity **IBM** is not shared and thus a software agent using this RDF data cannot, for example, infer relationships between two news sources that both have articles about IBM. We will work on a solution to this problem in the next section.

### Revisiting This Example Using URIs Instead of Literal Values

Note that in the figure in the previous section that nodes for literal values (e.g., for "IBM") are not shared. In this section we will copy the file **kgcreator.hy** to **kgcreator_uri.hy** add a few additions to map string literal values for entity names to <http://dbpedia.org> URIs by individually searching Google using the pattern "DBPedia 'entity name'" and defining a new map **v2umap** for mapping literal values to DBPedia URIs.

Note: In a production system (not a book example), I would use [https://www.wikidata.org database download](https://www.wikidata.org/wiki/Wikidata:Database_download) to download all of WikiData (which includes DBPedia data) and use a fuzzy text matching to find WikiData URIs for string literals. The compressed WikiData JSON data file is about 50 GB. Here we will manually find DBPedia for entity names that are in the example data.

In **kgcreator_uri.hy** we add a map **v2umap** for selected entity literal names to DBPedia URIs that I manually created using a web search on the DBPedia domain:


    (setv v2umap { ;; object literal value to URI mapping
      "IBM" "<http://dbpedia.org/page/IBM>"
      "The Wall Street Journal" "<http://dbpedia.org/page/The_Wall_Street_Journal>"
      "Banco Espirito" "<http://dbpedia.org/page/Banco_Esp%C3%ADrito_Santo>"
      "Australian Broadcasting Corporation"
      "http://dbpedia.org/page/Australian_Broadcasting_Corporation"
      "Australian Writers Guild"
      "http://dbpedia.org/page/Australian_Broadcasting_Corporation"
      "Microsoft" "http://dbpedia.org/page/Microsoft"})


We also make a change in the function **data2Rdf** to use the map **v2umap**:


    (defn data2Rdf [meta-data entities fout]
      (for [[value abbreviation] entities]
        (setv a-literal (+ "\"" value "\""))
        (if (in value v2umap) (setv a-literal (get v2umap value)))
        (if (in abbreviation e2umap)
          (.write fout (+ "<" meta-data ">\t" (get e2umap abbreviation)
                          "\t" a-literal " .\n")))))


Here is some of the generated RDF that has changed:


    <https://newsshop.com/may/a1023.html>
      <https://schema.org/Organization>
      <http://dbpedia.org/page/IBM> .
    <https://newsshop.com/may/a1023.html>
      <https://schema.org/Organization>
      <http://dbpedia.org/page/Banco_Esp%C3%ADrito_Santo> .


Now when we visualize generated RDF, we share nodes for The Wallstreet Journal and IBM:


![Part of the RDF graph that shows shared nodes when URIs are used for RDF values instead of literal strings](/site_images1/hy-lisp-python/2dot1.png)


While literal values sometimes are useful in generated RDF, using literals for the values in RDF triples prevents types of queries and inference that can be performed on the data.

### Wrap-up

In the field of Artificial Intelligence there are two topics that get me the most excited and I have been fortunate to be paid to work on both: Deep Learning and Knowledge Graphs. Here we have just touched the surface for creating data for Knowledge Graphs but I hope that between this chapter and the material on RDF in the chapter **Datastores** that you have enough information and experience playing with the examples to get started prototyping a Knowledge Graph in your organizartion. My advice is to "start small" by picking a problem that your organization has that can be solved by not moving data around, but rather, by creating a custom Knowledge Graph for metadata for existing information in your organization.

## Knowledge Graph Navigator

The Knowledge Graph Navigator (which I will often refer to as KGN) is a tool for processing a set of entity names and automatically exploring the public Knowledge Graph [DBPedia](http://dbpedia.org) using SPARQL queries. I wrote KGN in Common Lisp for my own use to automate some things I used to do manually when exploring Knowledge Graphs, and later thought that KGN might be useful also for educational purposes. KGN uses NLP code developed in earlier chapters and we will reuse that code with a short review of using the APIs.

Please note that the example is a simplified version that I first wrote in Common Lisp and is also an example in my book [Loving Common Lisp, or the Savvy Programmer's Secret Weapon](https://leanpub.com/lovinglisp) that you can read for free online. If you are interested you can see [screen shots of the Common Lisp version here](http://www.knowledgegraphnavigator.com/screen/).

The following two screen shots show the text based user interface for this example. This example application asks the user for a list of entity names and uses SPARQL queries to discover potential matches in DBPedia. We use the python library [PyInquirer](https://github.com/CITGuru/PyInquirer) for requesting entity names and then to show the user a list of matches from DBPedia.  The following screen shot shows these steps:


![Initial user interaction with Knowledge Graph Navigator example](/site_images1/hy-lisp-python/kgnuserselect.png)


To select the entities of interest, the user uses a space character to select or deselect an entity and the return (or enter) key to accept the list selections.

After the user selects entities from the list, the list disappears. The next screen shot shows the output from this example after the user finishes selecting entities of interest:


![After the user selects entities of interest](/site_images1/hy-lisp-python/kgnafter.png)


The code for this application is in the directory **kgn**. You will need to install the following Python library that supports console/text user interfaces:


    pip install PyInquirer


You will also need the **spacy** library and language model that we used in the earlier chapter on natural language processing. If you have not already done so, install these requirements:


    pip install spacy
    python -m spacy download en_core_web_sm


After listing the generated SPARQL for finding information for the entities in the query, KGN searches for relationships between these entities. These discovered relationships can be seen at the end of the last screen shot. Please note that this step makes SPARQL queries on **O(n\^2)** where **n** is the number of entities. Local caching of SPARQL queries to DBPedia helps make processing many entities possible.

Every time KGN makes a SPARQL query web service call to DBPedia the query and response are cached in a SQLite database in **\~/.kgn_hy_cache.db** which can greatly speed up the program, especially in development mode when testing a set of queries. This caching also takes some load off of the public DBPedia endpoint, which is a polite thing to do.

### Review of NLP Utilities Used in Application

We covered NLP in a previous chapter, so the following is just a quick review. The NLP code we use is near the top of the file **kgn.hy**:


    (import spacy)

    (setv nlp-model (spacy.load "en"))

    (defn entities-in-text [s]
      (setv doc (nlp-model s))
      (setv ret {})
      (for
        [[ename etype] (lfor entity doc.ents [entity.text entity.label_])]   
        (if (in etype ret)
            (setv (get ret etype) (+ (get ret etype) [ename]))
            (assoc ret etype [ename])))
      ret)


Here is an example use of this function:


    => (kgn.entities-in-text "Bill Gates, Microsoft, Seattle")
    {'PERSON': ['Bill Gates'], 'ORG': ['Microsoft'], 'GPE': ['Seattle']}


The entity type "GPE" indicates that the entity is some type of location.

### Developing Low-Level Caching SPARQL Utilities

While developing KGN and also using it as an end user, many SPARQL queries to DBPedia contain repeated entity names so it makes sense to write a caching layer. We use a SQLite database "\~/.kgn_hy_cache.db" to store queries and responses. We covered using SQLite in some detail in the chapter on datastores.

The caching layer is implemented in the file **cache.hy**:


     1 (import [sqlite3 [connect version Error ]])
     2 (import json)
     3 
     4 (setv *db-path* "kgn_hy_cache.db")
     5 
     6 (defn create-db []
     7   (try
     8     (setv conn (connect *db-path*))
     9     (print version)
    10     (setv cur (conn.cursor))
    11     (cur.execute "CREATE TABLE dbpedia (query string  PRIMARY KEY ASC, data json)")
    12     (conn.close)
    13     (except [e Exception] (print e))))
    14 
    15 (defn save-query-results-dbpedia [query result]
    16   (try
    17     (setv conn (connect *db-path*))
    18     (setv cur (conn.cursor))
    19     (cur.execute "insert into dbpedia (query, data) values (?, ?)" 
    20                  [query (json.dumps result)])
    21     (conn.commit)
    22     (conn.close)
    23     (except [e Exception] (print e))))
    24  
    25 (defn fetch-result-dbpedia [query]
    26   (setv results [])
    27   (setv conn (connect *db-path*))
    28   (setv cur (conn.cursor))
    29   (cur.execute "select data from dbpedia where query = ? limit 1" [query])
    30   (setv d (cur.fetchall))
    31   (if (> (len d) 0)
    32       (setv results (json.loads (first (first d)))))
    33   (conn.close)
    34   results)
    35  
    36 (create-db)


Here we store structured data from SPARQL queries as JSON data serialized as string values.

#### SPARQL Utilities

We will use the caching code from the last section and also the standard Python library **requests** to access the DBPedia servers. The following code is found in the file **sparql.hy** and also provides support for using both DBPedia and WikiData. We only use DBPedia in this chapter but when you start incorporating SPARQL queries into applications that you write, you will also probably want to use WikiData.

The function **do-query-helper** contains generic code for SPARQL queries and is used in functions **wikidata-sparql** and **dbpedia-sparql**:


    (import json)
    (import requests)
    (require [hy.contrib.walk [let]])

    (import [cache [fetch-result-dbpedia save-query-results-dbpedia]])

    (setv wikidata-endpoint "https://query.wikidata.org/bigdata/namespace/wdq/sparql")
    (setv dbpedia-endpoint "https://dbpedia.org/sparql")

    (defn do-query-helper [endpoint query]
      ;; check cache:
      (setv cached-results (fetch-result-dbpedia query))
      (if (> (len cached-results) 0)
          (let ()
            (print "Using cached query results")
            (eval cached-results))
          (let ()
            ;; Construct a request
            (setv params { "query" query "format" "json"})
            
            ;; Call the API
            (setv response (requests.get endpoint :params params))
            
            (setv json-data (response.json))
            
            (setv vars (get (get json-data "head") "vars"))
            
            (setv results (get json-data "results"))
            
            (if (in "bindings" results)
                (let [bindings (get results "bindings")
                      qr
                      (lfor binding bindings
                            (lfor var vars
                                  [var (get (get binding var) "value")]))]
                  (save-query-results-dbpedia query qr)
                  qr)
                []))))

    (defn wikidata-sparql [query]
      (do-query-helper wikidata-endpoint query))

    (defn dbpedia-sparql [query]
      (do-query-helper dbpedia-endpoint query))


Here is an example query (manually formatted for page width):


    $ hy
    hy 0.18.0 using CPython(default) 3.7.4 on Darwin
    => (import sparql)
    table dbpedia already exists
    => (sparql.dbpedia-sparql
         "select ?s ?p ?o { ?s ?p ?o } limit 1")
    [[['s', 'http://www.openlinksw.com/virtrdf-data-formats#default-iid'],
      ['p', 'http://www.w3.org/1999/02/22-rdf-syntax-ns#type'],
      ['o', 'http://www.openlinksw.com/schemas/virtrdf#QuadMapFormat']]]
    => 


This is a wild-card SPARQL query that will match any of the 9.5 billion RDF triples in DBPedia and return just one result.

This caching layer greatly speeds up my own personal use of KGN. Without caching, queries that contain many entity references simply take too long to run.

### Utilities to Colorize SPARQL and Generated Output

When I first had the basic functionality of KGN working, I was disappointed by how the application looked as normal text. Every editor and IDE I use colorizes text in an appropriate way so I used standard ANSI terminal escape sequences to implement color hilting SPARQL queries.

The code in the following listing is in the file **colorize.hy**.


    (require [hy.contrib.walk [let]])
    (import [io [StringIO]])

    ;; Utilities to add ANSI terminal escape sequences to colorize text.
    ;; note: the following 5 functions return string values that then need to
    ;;       be printed.

    (defn blue [s] (.format "{}{}{}" "\033[94m" s "\033[0m"))
    (defn red [s] (.format "{}{}{}" "\033[91m" s "\033[0m"))
    (defn green [s] (.format "{}{}{}" "\033[92m" s "\033[0m"))
    (defn pink [s] (.format "{}{}{}" "\033[95m" s "\033[0m"))
    (defn bold [s] (.format "{}{}{}" "\033[1m" s "\033[0m"))

    (defn tokenize-keep-uris [s]
      (.split s))

    (defn colorize-sparql [s]
      (let [tokens
            (tokenize-keep-uris
              (.replace (.replace (.replace s "{" " { ") "}" " } ") "." " . "))
            ret (StringIO)] ;; ret is an output stream for a string buffer
        (for [token tokens]
          (if (> (len token) 0)
              (if (= (get token 0) "?")
                  (.write ret (red token))
                  (if (in
                        token
                        ["where" "select" "distinct" "option" "filter"
                         "FILTER" "OPTION" "DISTINCT" "SELECT" "WHERE"])
                      (.write ret (blue token))
                      (if (= (get token 0) "<")
                          (.write ret (bold token))
                          (.write ret token)))))
          (if (not (= token "?"))
              (.write ret " ")))
        (.seek ret 0)
        (.read ret)))


You have seen colorized SPARQL in the two screen shots at the beginning of this chapter.

### Text Utilities for Queries and Results

The application low level utility functions are in the file **kgn-utils.hy**. The function **dbpedia-get-entities-by-name** requires two arguments:

-   The name of an entity to search for.
-   A URI representing the entity type that we are looking for.

We embed a SPARQL query that has placeholders for the entity name and type. The filter expression specifies that we only want triple results with comment values in the English language by using **(lang(?comment) = 'en')**:


     1 #!/usr/bin/env hy
     2 
     3 (import [sparql [dbpedia-sparql]])
     4 (import [colorize [colorize-sparql]])
     5 
     6 (import [pprint [pprint]])
     7 (require [hy.contrib.walk [let]])
     8 
     9 (defn dbpedia-get-entities-by-name [name dbpedia-type]
    10   (let [sparql
    11         (.format "select distinct ?s ?comment {{ ?s ?p \"{}\"@en . ?s <http://www.w3\
    12 .org/2000/01/rdf-schema#comment>  ?comment  . FILTER  (lang(?comment) = 'en') . ?s <\
    13 http://www.w3.org/1999/02/22-rdf-syntax-ns#type> {} . }} limit 15" name dbpedia-type\
    14 )]
    15     (print "Generated SPARQL to get DBPedia entity URIs from a name:")
    16     (print (colorize-sparql sparql))
    17     (dbpedia-sparql sparql)))


Here is an example:


![Getting entities by name with colorized SPARL query script](/site_images1/hy-lisp-python/kgnutils.png)


### Finishing the Main Function for KGN

We already looked at the NLP code near the beginning of the file **kgn.hy**. Let's look at the remainder of the implementation.

We need a dictionary (or hash table) to convert **spaCy** entity type names to DBPedia type URIs:


    1 (setv entity-type-to-type-uri
    2       {"PERSON" "<http://dbpedia.org/ontology/Person>"
    3        "GPE" "<http://dbpedia.org/ontology/Place>"
    4        "ORG" "<http://dbpedia.org/ontology/Organisation>"
    5        })


When we get entity results from DBPedia, the comments describing entities can be a few paragraphs of text. We want to shorten the comments so they fit in a single line of the entity selection list that we have seen earlier. The following code defines a comment shortening function and also a global variable that we will use to store the entity URIs for each shortened comment:


    1 (setv short-comment-to-uri {})
    2 
    3 (defn shorten-comment [comment uri]
    4   (setv sc (+ (cut comment 0 70) "..."))
    5   (assoc short-comment-to-uri sc uri)
    6   sc)


In line 5, we use the function **assoc** to add a key and value pair to an existing dictionary **short-comment-to-uri**.

Finally, let's look at the main application loop. In line 4 we are using the function **get-query** (defined in file **textui.hy**) to get a list of entity names from the user. In line 7 we use the function **entities-in-text** that we saw earlier to map text to entity types and names. In the nested loops in lines 13-26 we build one line descriptions of people, place, and organizations that we will use to show the user a menu for selecting entities found in DBPedia from the original query. We are giving the use a chance to select only the discovered entities that they are interested in.

In lines 33-35 we are converting the shortened comment strings the user selected back to DBPedia entity URIs. Finally in line 36 we use the function **entity-results-\>relationship-links** to find relationships between the user selected entities.


     1 (defn kgn []
     2   (while
     3     True
     4     (let [query (get-query)
     5           emap {}]
     6       (if (or (= query "quit") (= query "q"))
     7           (break))
     8       (setv elist (entities-in-text query))
     9       (setv people-found-on-dbpedia [])
    10       (setv places-found-on-dbpedia [])
    11       (setv organizations-found-on-dbpedia [])
    12       (global short-comment-to-uri)
    13       (setv short-comment-to-uri {})
    14       (for [key elist]
    15         (setv type-uri (get entity-type-to-type-uri key))
    16         (for [name (get elist key)]
    17           (setv dbp (dbpedia-get-entities-by-name name type-uri))
    18           (for [d dbp]
    19             (setv short-comment (shorten-comment (second (second d)) 
    20             (second (first d))))
    21             (if (= key "PERSON")
    22                 (.extend people-found-on-dbpedia [(+ name  " || " short-comment)]))
    23             (if (= key "GPE")
    24                 (.extend places-found-on-dbpedia [(+ name  " || " short-comment)]))
    25             (if (= key "ORG")
    26                 (.extend organizations-found-on-dbpedia 
    27                 [(+ name  " || " short-comment)])))))
    28       (setv user-selected-entities
    29             (select-entities
    30               people-found-on-dbpedia
    31               places-found-on-dbpedia
    32               organizations-found-on-dbpedia))
    33       (setv uri-list [])
    34       (for [entity (get user-selected-entities "entities")]
    35         (setv short-comment (cut entity (+ 4 (.index entity " || "))))
    36         (.extend uri-list [(get short-comment-to-uri short-comment)]))
    37       (setv relation-data (entity-results->relationship-links uri-list))
    38       (print "\nDiscovered relationship links:")
    39       (pprint relation-data))))


If you have not already done so, I hope you experiment running this example application. The first time you specify an entity name expect some delay while DBPedia is accessed. Thereafter the cache will make the application more responsive when you use the same name again in a different query.

### Wrap-up

If you enjoyed running and experimenting with this example and want to modify it for your own projects then I hope that I provided a sufficient road map for you to do so.

I got the idea for the KGN application because I was spending quite a bit of time manually setting up SPARQL queries for DBPedia and other public sources like WikiData, and I wanted to experiment with partially automating this exploration process.

## Book Wrap-up

I love programming in Lisp languages but I often need to use Python libraries for Deep Learning and NLP. The Hy language is a good fit for me, it is simple to install along with the Python libraries that I use for my work and it is a fun language to write code in. Most importantly, Hy fits well with the type of iterative bottom-up REPL-based development that I prefer.

I hope that you enjoyed this short book and that at least a few things that you have learned here will both help you in your work and give you ideas for new personal projects.

Best regards,

Mark Watson

February 15, 2020


