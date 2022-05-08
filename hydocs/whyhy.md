# なぜHyなのか？

Hy (昆虫目 Hymenoptera にちなんで名付けられた。Paul Tagliamonte がこの言語を作ったとき、群行動を研究していたから) は [Lisp 系](https://en.wikipedia.org/wiki/Lisp_(programming_language)) に属するマルチパラダイム汎用プログラミング言語です ． Pythonの代替構文のようなものとして実装されている。Pythonと比較して、HyはLispに期待されるような様々な追加機能、一般化、構文の簡略化を提供します。他のLispと比較して、HyはPythonの組み込み関数やサードパーティのPythonライブラリに直接アクセスでき、命令型、関数型、オブジェクト指向型のプログラミングを自由に混在させることが可能です。

## HyとPythonの比較

PythonプログラマがHyで最初に気がつくことは、PythonのCライクなinfix構文の代わりに、Lispの伝統的な括弧を多用したprefix構文を持っていることでしょう。例えば、`print("The answer is", 2 + object.method(arg))` はHyでは `(print "The answer is" (+ 2 (.method object arg)))` と書くことができます。 その結果、Hyは自由形式です：構造は空白ではなく括弧で示され、コマンドラインでの使用に便利です。

他のLispと同様、単純化された構文はLispの特徴的な機能である[メタプログラミング](https://en.wikipedia.org/wiki/Metaprogramming) を容易にする。[マクロとは、コンパイル時にコードオブジェクトを操作して新しいコードオブジェクトを生成し、それがあたかも元のコードの一部であったかのように実行される関数である。実際、Hyではコンパイル時に任意の計算を行うことができます。例えば、C言語風のdo-whileループを実装した簡単なマクロは、条件が真である限り、少なくとも一度は本体を実行する。

```
(defmacro do-while [condition #* body]
  `(do
    ~body
    (while ~condition
      ~body)))
(setv x 0)
(do-while x
  (print "This line is executed once."))
```

Hyはまた、Pythonの式と文の混在に関する制限を取り除き、より直接的で機能的なコードを可能にします。例えば、Pythonは `with` ブロックが、リソースを使い終わったら閉じる、値を返すことを許しません。これらは一連のステートメントを実行するだけです。

```
with open("foo") as o:
   f1 = o.read()
with open("bar") as o:
   f2 = o.read()
print(len(f1) + len(f2))
```

Hyでは、`with`は最後のボディフォームの値を返すので、普通の関数呼び出しのように使うことができます。

```
(print (+
  (len (with [o (open "foo")] (.read o)))
  (len (with [o (open "bar")] (.read o)))))
```

さらに簡潔に言うと、 :hy`gfor` の中に `with` フォームを入れることができます。

```
(print (sum (gfor
  filename ["foo" "bar"]
  (len (with [o (open filename)] (.read o))))))
```

最後に、HyはPythonの二項演算子に対していくつかの一般化を提供しています。演算子は2つ以上の引数を与えることができ(例えば `(+ 1 2 3)`) 、拡張された代入演算子(例えば `(+= x 1 2 3)`) も与えることができます。また、これらは同名の通常のファーストクラス関数として提供されるので、高階の関数に渡すことができます。`(sum xs)` は、モジュール `hy.pyops` から関数 `+` をインポートした後、 `(reduce + xs)` と書くことができます。

Hyコンパイラは、HyソースコードをHyモデルオブジェクトに読み込み、HyモデルオブジェクトをPython抽象構文木(:py`ast`)オブジェクトにコンパイルすることで動作します。Python ASTオブジェクトはその後、Python自身によってコンパイルされ実行されたり、後で速く実行するためにバイトコンパイルされたり、Pythonのソースコードにレンダリングされたりします。PythonとHyのコードを同じプロジェクト、あるいは同じファイルに混在させることも可能です。

## Hyと他のLispsの比較

実行時、Hyは本質的にPythonのコードです。したがって、Hyのデザインは[Clojure](https://clojure.org)に多くを負っていますが、ClojureがJavaに依存しているよりもPythonに緊密に結合しています。より良いアナロジーは[CoffeeScript](https://coffeescript.org)とJavaScriptの関係です。 Pythonの組み込み`関数 <py:built-in-funcs>` と `データ構造 <py:bltin-types>` は直接利用することができます。

```
(print (int "deadbeef" :base 16))  ; 3735928559
(print (len [1 10 100]))           ; 3
```

[PyPI](https://pypi.org)などにあるサードパーティのPythonライブラリについても同じことが言えます。ここにHyの小さな[CherryPy](https://cherrypy.dev)ウェブ・アプリケーションがあります。

```
(import cherrypy)

(defclass HelloWorld []
  #@(cherrypy.expose (defn index [self]
    "Hello World!")))

(cherrypy.quickstart (HelloWorld))
```

Hyを[PyPy](https://pypy.org)上で走らせれば、特に高速なLispを実現することも可能です。

すべてのLispsと同様に、Hyは[homoiconic](https://en.wikipedia.org/wiki/Homoiconicity)です。Hyのシンタックスは、コンセルやPythonの基本的なデータ構造ではなく、`モデル`と呼ばれるPythonの基本的なデータ構造の単純なサブクラスで表現されています。モデルは、エラーメッセージのために行番号や列番号を追跡することができます。また、モデルは、セットリテラルに現れる要素の順序など、対応するプリミティブ型では表現できない構文的な特徴を表現することができます。しかし、モデルは普通のリストと同じように連結したりインデックスを付けることができます。また、マクロから普通のPythonの型を返したり、`hy.eval`に型を渡せば、Hyはそれらを自動的にモデルに昇格させることができます。

Hyはそのセマンティクスの多くをPythonから得ています。例えば、Pythonの関数は関数でないオブジェクトと同じ名前空間を使うので、HyはLisp-1となります。一般に、どんなPythonのコードもHyに文字通り翻訳することが可能であるはずです。同時に、HyはPythonでは簡単でない典型的なLispのことをできるようにするために、ある種の長所を備えています。例えば、Hyは前述のステートメントと式の混合、`valid?`のような名前のシンボルを透過的にPythonで正当な識別子に変換する `name mangling`、Pythonの通常の関数レベルのスコープに代わってブロックレベルのスコープを提供する :hy`let` マクロを提供します。

全体として、HyはCommon Lispと同様、独断と偏見に満ちた大きなテント型の言語であることを意図しており、やりたいことができるようになっています。Schemeのような、より小さく美しいLispのアプローチに興味があれば、Hyの開発者が作ったPythonに埋め込まれたもう一つのLisp、[Hissp](https://github.com/gilch/hissp)をチェックしてみてください。
