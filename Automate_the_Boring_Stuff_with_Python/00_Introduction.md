# AUTOMATE THE BORING STUFF WITH PYTHON 2ND EDITION

Practical Programming for Total Beginners

**by Al Sweigart**


![image](../images/000126.jpg)


San Francisco



**AUTOMATE THE BORING STUFF WITH PYTHON, 2ND EDITION.** Copyright © 2020 by Al Sweigart.

All rights reserved. No part of this work may be reproduced or transmitted in any form or by any means, electronic or mechanical, including photocopying, recording, or by any information storage or retrieval system, without the prior written permission of the copyright owner and the publisher.

ISBN-10: 1-59327-992-2\
ISBN-13: 978-1-59327-992-9

Publisher: William Pollock\
Production Editor: Laurel Chun\
Cover Illustration: Josh Ellingson\
Interior Design: Octopod Studios\
Developmental Editors: Frances Saux and Jan Cash\
Technical Reviewers: Ari Lacenski and Philip James\
Copyeditors: Kim Wimpsett, Britt Bogan, and Paula L. Fleming\
Compositors: Susan Glinert Stevens and Danielle Foster\
Proofreaders: Lisa Devoto Farrell and Emelie Burnette\
Indexer: BIM Indexing and Proofreading Services

For information on distribution, translations, or bulk sales, please contact No Starch Press, Inc. directly:

No Starch Press, Inc.\
245 8th Street, San Francisco, CA 94103\
phone: 1.415.863.9900;
[info@nostarch.com](mailto:info@nostarch.com)\
[www.nostarch.com](http://www.nostarch.com)

The Library of Congress Control Number for the first edition is:
2014953114

No Starch Press and the No Starch Press logo are registered trademarks of No Starch Press, Inc. Other product and company names mentioned herein may be the trademarks of their respective owners. Rather than use a trademark symbol with every occurrence of a trademarked name, we are using the names only in an editorial fashion and to the benefit of the trademark owner, with no intention of infringement of the trademark.

The information in this book is distributed on an "As Is" basis, without warranty. While every precaution has been taken in the preparation of this work, neither the author nor No Starch Press, Inc. shall have any liability to any person or entity with respect to any loss or damage caused or alleged to be caused directly or indirectly by the information contained in it.

This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 3.0 United States License. To view a copy of this license, visit *[http://creativecommons.org/licenses/by-nc-sa/3.0/us/](http://creativecommons.org/licenses/by-nc-sa/3.0/us/)* or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.



## **著者について** 

Al Sweigartは、ソフトウェア開発者であり、技術書の著者でもあります。Pythonは彼のお気に入りのプログラミング言語であり、そのためのいくつかのオープンソースモジュールの開発者でもあります。彼の他の書籍は、彼のウェブサイト*[https://inventwithpython.com/](https://inventwithpython.com/)*でクリエイティブ・コモンズ・ライセンスの下で自由に入手できます。 飼い猫の体重は現在11ポンド。



## **テックレビュアーについて** 

Philip Jamesは、10年以上Pythonで仕事をしており、Pythonコミュニティで頻繁に講演を行っています。彼は、Unixの基礎からオープンソースのソーシャルネットワークまで、様々なトピックで講演を行っています。PhilipはBeeWareプロジェクトのCore Contributorで、パートナーのNicと彼女の猫Riverと一緒にサンフランシスコのベイエリアに住んでいます。



## **序文** 


![Image](../images/000143.jpg)


"私たち3人が2日かけてやることを、あなたは2時間でやってのけてしまった" 大学のルームメイトは2000年代初頭に家電量販店で働いていました。時折、他店から何千もの商品価格のスプレッドシートを受け取ることがあった。3人の従業員で構成されるチームは、そのスプレッドシートを厚い紙の束に印刷し、それを各自で分担する。各商品の価格について、自分の店の価格を調べ、競合店がより安く売っている商品をすべて書き留めるのだ。通常、2〜3日かかる。

「印刷物の元ファイルがあれば、それを実行するプログラムを書くことができるよ」と、床に座って書類が散乱して積み重なっているのを見て、ルームメイトは彼らに言ったのです。

数時間後、彼はファイルから競合他社の価格を読み取り、店のデータベースからその商品を探し出し、競合他社の方が安いかどうかを記す短いプログラムを作った。彼はまだプログラミングの経験が浅いので、ほとんどの時間をプログラミングの本で調べながら作った。実際のプログラムの実行には、数秒しかかからなかった。その日、ルームメイトと彼の同僚は、特別に長い昼食をとった。

これがコンピューター・プログラミングの力だ。コンピュータは、無数の作業に対応できるスイスアーミーナイフのようなものです。多くの人は、繰り返し行われる作業のために、クリックやタイピングに何時間も費やしています。しかし、自分が使っている機械が、適切な指示を与えれば、数秒でその仕事をこなしてくれるとは思ってもいません。

### **この本は誰のためのもの？** 

ほぼすべての人がソーシャルネットワークを使ってコミュニケーションをとり、多くの人が携帯電話にインターネットに接続されたコンピュータを持ち、ほとんどの事務職がコンピュータとやりとりして仕事をこなしているように、ソフトウェアは今日私たちが使う多くのツールの核となっています。その結果、コードを書ける人の需要が急増しているのです。数え切れないほどの書籍、インタラクティブなWebチュートリアル、開発者向けブートキャンプが、野心的な初心者を6桁の給料をもらえるソフトウェアエンジニアにすることを約束しています。

この本は、そのような人たちのためのものではありません。それ以外の人たちのための本です。

この本だけでは、ギターのレッスンを何度か受ければロックスターになれるのと同じように、プロのソフトウェア開発者になることはできないでしょう。しかし、もしあなたが会社員、管理者、学者、あるいは仕事や遊びでコンピュータを使う人であれば、プログラミングの基本を学び、以下のような簡単な作業を自動化できるようになるでしょう。

- 何千ものファイルの移動と名前の変更、およびフォルダへの分類
- オンラインフォームへの入力--タイピング不要
- ウェブサイトが更新されるたびにファイルをダウンロードしたり、テキストをコピーする
- コンピュータから独自の通知メールを受け取る
- Excelスプレッドシートの更新や書式設定
- メールをチェックし、あらかじめ書いておいた返信を送る

これらのタスクは、人間にとっては単純だが時間がかかるものであり、また些細なことや特殊なことであるため、実行するための既製のソフトウェアが存在しないことがよくあります。しかし、ちょっとしたプログラミングの知識があれば、これらの作業をコンピュータに代行させることができます。

### **規約** 

本書はリファレンスマニュアルとしてではなく、初心者のためのガイドとして作成されています。コーディングスタイルはベストプラクティスに反することもありますが（たとえば、グローバル変数を使っているプログラムもあります）、それはコードをよりシンプルに習得するためのトレードオフの関係です。この本は、使い捨てのコードを書くために作られたものなので、スタイルやエレガンスに費やす時間はあまりないのです。 オブジェクト指向、リスト内包、ジェネレータなどの洗練されたプログラミングコンセプトは、複雑さを増すので扱わない。ベテランのプログラマは、本書のコードを変更することで効率を上げることができると指摘するかもしれませんが、本書の大部分は、あなたが最小限の努力でプログラムを動作させることに関係しています。

### **プログラミングとは？** 

テレビ番組や映画では、プログラマーが光り輝く画面に1や0を必死に打ち込んでいる姿がよく映し出されますが、現代のプログラミングはそれほど神秘的なものではありません。*プログラミング*とは、コンピュータに実行させる命令を入力することです。これらの命令は、数字を計算したり、テキストを修正したり、ファイルの情報を調べたり、インターネットを介して他のコンピュータと通信したりする。

すべてのプログラムは、基本的な命令を構成要素として使用します。ここでは、最も一般的な命令をいくつか英語で紹介します。

- "これを実行し、次にあれを実行しなさい。"
- "この条件が真であれば、この動作を行い、そうでなければ、その動作を行う。"
- "この動作をちょうど27回行いなさい。"
- "この条件が成立するまで、それをやり続けなさい"

これらの構成要素を組み合わせて、より複雑な判断を実装することも可能だ。例えば、プログラミング言語「Python」で書かれた簡単なプログラムの「*ソースコード*」と呼ばれるプログラミング命令です。Pythonのソフトウェアは、一番上から始めて、一番下に到達するまで、コードの各行を実行します（一部の行は、ある条件が真である場合のみ、または*else* Pythonが他の行を実行する場合のみ実行されます）。

```
[➊] passwordFile = open('SecretPasswordFile.txt')
[➋] secretPassword = passwordFile.read()
[➌] print('Enter your password.')
   typedPassword = input()
[➍] if typedPassword == secretPassword:
   [➎] print('Access granted')
   [➏] if typedPassword == '12345':
       [➐] print('That password is one that an idiot puts on their luggage.')
  else:
   [➑] print('Access denied')
```

あなたはプログラミングについて何も知らないかもしれませんが、前のコードを読むだけで、何をしているのかそれなりに推測できるのではないでしょうか。まず、ファイル *SecretPasswordFile.txt* が開かれ [➊] 、その中の秘密のパスワードが読み込まれます [➋]。次に、ユーザーは（キーボードから）パスワードを入力するよう促されます[➌]。これら二つのパスワードが比較され [➍] 、同じであれば、プログラムは *Access granted* を画面に表示します [➎] 。次に、プログラムはパスワードが *12345* であるかどうかを確認し [➏] 、この選択がパスワードとして最適でない可能性を示唆します [➐]。パスワードが同じでない場合、プログラムは画面に *Access denied* と表示します [➑]。

#### ***Pythonとは？*** 

*Python*は、プログラミング言語（Pythonのコードを正しく書くための構文ルール）と、（Python言語で書かれた）ソースコードを読み込んでその命令を実行するPythonインタプリタ・ソフトウェアである。Pythonインタプリタは*[https://python.org/](https://python.org/)*から無償でダウンロードでき、Linux、macOS、Windows用のバージョンがあります。

Pythonという名前は、蛇からではなく、イギリスのシュールなコメディグループMonty Pythonに由来している。Pythonのプログラマは愛情を込めてPythonistasと呼ばれ、Pythonのチュートリアルや文書にはたいていMonty Pythonと蛇の両方の言及があります。

#### ***プログラマーに数学の知識は必要ない*** 

プログラミングを学ぶ上で最もよく耳にする不安は、「たくさんの数学が必要だ」という考え方です。実は、ほとんどのプログラミングでは、基本的な計算以上の数学は必要ないのです。プログラミングが得意なのは、数独を解くのが得意なのと大差ないのです。

数独は、9×9の盤面の各行、各列、各3×3のマスに1から9までの数字を埋めていくパズルです。まず、いくつかの数字が提示され、その数字から推理して解答を見つけます。[図0-1](#calibre_link-1611)のパズルは、1列目と2列目に5が出てくるので、これらの列には再び5が現れることはありません。 したがって、右上の列では、5が3行目に入ることになります。最後の列にはすでに5があるので、5は6の右側に入ることはできず、6の左側に入ることになります。 1つの行、列、マスを解けば、残りのパズルのヒントが得られ、1～9の数字を埋めていけば、すぐにすべてのグリッドが解けます。


[image](../images/000004.jpg)


*図0-1：新しい数独の問題（左）とその解答（右）。 数字を使っているにもかかわらず、数独はあまり計算を必要としない。(Images © Wikimedia Commons)*。

数独は数字が絡むからと言って、数学が得意でないと解けないわけではありません。プログラミングも同じです。数独を解くように、プログラムを書くには、問題を一つ一つの細かいステップに分解していくことが必要です。 同様に、プログラムのデバッグ（エラーを発見し修正すること）でも、プログラムが何をしているのか根気よく観察し、バグの原因を探ります。このように、プログラミングはやればやるほど上達する技術です。

#### ***プログラミングを学ぶのに歳は関係ない*** 

プログラミングを学ぶことに関して、私が耳にする2番目に多い不安は、「年齢的に学ぶには遅すぎると思う」というものです。インターネット上では、自分はもう（なんと！）23歳だから遅すぎると思っている人たちのコメントをたくさん読みました。プログラミングを学ぶのに「遅すぎる」年齢ではないことは明らかで、多くの人が人生のずっと後になってから学んでいます。

多くの人がもっと後になってから学んでいるのです。しかし、プログラマーは優秀な子供というイメージは根強いものがあります。残念ながら、私も「プログラミングを始めたのは小学生のときだった」と他人に話すことで、この神話に加担しています。

しかし、1990年代に比べれば、プログラミングはずっと学びやすくなっています。現在では、より多くの書籍があり、より優れた検索エンジンがあり、オンライン上の質問と回答のウェブサイトがたくさんあります。その上、プログラミング言語自体もずっと使いやすくなっています。これらの理由から、**私が小学校から高校を卒業するまでの間に学んだプログラミングのすべてが、今日では週末に12回程度で学ぶことができるのです**。私のスタートダッシュは、実はたいしたものではなかったのです。

プログラミングについて「成長思考」を持つことが重要です。つまり、人は練習によってプログラミングのスキルを身につけると理解することです。生まれつきのプログラマーではありませんし、今、プログラミングができないからといって、決してエキスパートになれないということではありません。

#### ***プログラミングは創造的な活動である*** 

プログラミングは、絵を描いたり、文章を書いたり、編み物をしたり、レゴのお城を作ったりするのと同じように、創造的な作業です。真っ白なキャンバスに絵を描くように、ソフトウェアを作るには多くの制約がありますが、無限の可能性があります。

プログラミングと他の創作活動の違いは、プログラミングの場合、必要な材料はすべてコンピュータの中にあることです。キャンバスや絵の具、フィルム、毛糸、レゴブロック、電子部品などを追加で買う必要はありません。10年前のパソコンでも、プログラムを書くには十分すぎるほど高性能です。一度書いたプログラムは、完璧に何度でもコピーすることができる。ニットのセーターは一度に一人しか着ることができないが、便利なプログラムはオンラインで簡単に全世界と共有することができる。

### **About This Book** 

本書は、第1部でPythonプログラミングの基本的な考え方を学び、第2部でコンピュータに自動化させることができるさまざまな作業について学びます。第2部の各章には、プロジェクトプログラムが用意されているので、勉強になります。以下、各章の内容を簡単に紹介します。

**[Part I: Python Programming Basics](#calibre_link-88)**

**[Chapter 1: Python Basics](#calibre_link-89)** Pythonの最も基本的な命令である式と、対話型シェルソフトウェアPythonを使ってコードを実験する方法をカバーします。

**[Chapter 2: Flow Control](#calibre_link-106)** プログラムがどの命令を実行するかを決定する方法を説明し、コードがさまざまな条件に賢く対応できるようにします。

**[Chapter 3: Functions](#calibre_link-132)** 独自の関数を定義して、コードを管理しやすくする方法を説明します。

**[Chapter 4: Lists](#calibre_link-152)** リストデータ型を導入し、データの整理方法を説明します。

**[Chapter 5: Dictionaries and Structuring Data](#calibre_link-190)** ディクショナリーデータタイプを紹介し、より強力なデータ整理の方法を紹介します。

**[Chapter 6: Manipulating Strings](#calibre_link-207)** テキストデータ（Pythonでは*文字列*と呼ばれます）の操作について説明します。

**[Part II: Automating Tasks](#calibre_link-237)**

**[Chapter 7: Pattern Matching with Regular Expressions](#calibre_link-238)** Pythonによる文字列の操作方法と正規表現によるテキストパターンの検索方法を説明します。

**[Chapter 8: Input Validation](#calibre_link-47)** ユーザーが入力した情報をプログラムが検証する方法について説明し、ユーザーのデータがプログラムの残りの部分でエラーを引き起こさないような形式で届くことを確認します。

**[Chapter 9: Reading and Writing Files](#calibre_link-32)** プログラムがテキストファイルの内容を読み取り、ハードドライブ上のファイルに情報を保存する方法について説明します。

**[Chapter 10: Organizing Files](#calibre_link-30)** Pythonが、大量のファイルのコピー、移動、名前変更、削除を、人間が行うよりもはるかに高速に行う方法を紹介します。また、ファイルの圧縮・解凍についても説明します。

**[Chapter 11: Debugging](#calibre_link-349)** Pythonの様々なバグ発見・バグ修正ツールの使い方を紹介します。

**[Chapter 12: Web Scraping](#calibre_link-372)** ウェブページを自動的にダウンロードし、情報を解析するプログラムの書き方を紹介します。これをWebスクレイピングと呼びます。

**[Chapter 13: Working with Excel Spreadsheets](#calibre_link-418)** Excelのスプレッドシートをプログラム的に操作して、読む必要がないようにすることをカバーします。 分析する文書が数百、数千に及ぶ場合に有効です。

**[Chapter 14: Working with Google Sheets](#calibre_link-457)** Pythonを使用して、Webベースの人気スプレッドシートアプリケーションであるGoogle Sheetsを読み、更新する方法をカバーします。

**[Chapter 15: Working with PDF and Word Documents](#calibre_link-477)** プログラムによるWordやPDFの読み取りについて解説しています。

**[Chapter 16: Working with CSV Files and JSON Data](#calibre_link-505)** 引き続き、プログラムによる文書操作の方法を説明し、今回はCSVファイルとJSONファイルについて説明します。

**[Chapter 17: Keeping Time, Scheduling Tasks, and Launching Programs](#calibre_link-530)** Pythonプログラムがどのように時間と日付を扱うか、また、特定の時間にタスクを実行するようにコンピュータをスケジュールする方法について説明します。また、Pythonプログラムが非Pythonプログラムを起動する方法についても説明します。

**[Chapter 18: Sending Email and Text Messages](#calibre_link-567)** メールやテキストメッセージの送信を代行するプログラムの書き方を解説しています。

**[Chapter 19: Manipulating Images](#calibre_link-608)** JPEGやPNGファイルなどの画像をプログラムで操作する方法を説明します。

**[Chapter 20: Controlling the Keyboard and Mouse with GUI Automation](#calibre_link-634)** マウスやキーボードをプログラムで制御し、クリックやキー入力を自動化する方法を解説しています。

**[Appendix A: Installing Third-Party Modules](#calibre_link-2)** 便利な追加モジュールでPythonを拡張する方法を紹介します。

**[Appendix B: Running Programs](#calibre_link-35)** Windows、macOS、Linux上でコードエディターの外からPythonプログラムを実行する方法を紹介します。

**[Appendix C: Answers to the Practice Questions](#calibre_link-685)** 各章末の練習問題に対する解答と補足説明を掲載。

### **Downloading and Installing Python** 

Windows、macOS、Ubuntu用のPythonは、*[https://python.org/downloads/](https://python.org/downloads/)*で無料でダウンロードできます。 このサイトのダウンロードページから最新版をダウンロードすれば、本書で紹介するプログラムはすべて動作するはずです。


**[WARNING]**

*Python 3（3.8.0など）を必ずダウンロードしてください。本書のプログラムは Python 3 上で動作するように書かれており、Python 2.* 上では正しく動作しない場合があります。


ダウンロードページには、各OSの64ビットと32ビットコンピュータ用のPythonインストーラがありますので、まず、どちらのインストーラが必要かを把握します。もしあなたが2007年以降に購入したコンピュータであれば、64-bitシステムの可能性が高いです。そうでなければ、32-bit版を持っていることになりますが、ここではそれを確かめる方法を説明します。

- Windows では、**スタート** ▸ **コントロール パネル** ▸ **システム** を選択し、システムの種類に 64 ビットと 32 ビットのどちらが表示されているかを確認します。
- macOSでは、Appleメニューから、**このMacについて** ▸ **詳細情報** ▸ **システムレポート** ▸ **ハードウェア** を選択し、プロセッサ名フィールドを確認します。Intel Core Solo または Intel Core Duo と表示されている場合は、32 ビット・マシンです。それ以外（Intel Core 2 Duoを含む）が表示されている場合は、64ビット・マシンを使用しています。
- Ubuntu Linuxの場合、Terminalを開いて[uname -m]というコマンドを実行します。[i686]が32ビット、[x86_64]が64ビットを意味します。

Windowsの場合、Pythonのインストーラー（ファイル名の末尾が*.msi*）をダウンロードし、ダブルクリックします。インストーラが画面に表示する指示に従って、ここに記載されているようにPythonをインストールします。

1.  **Install for All Users**を選択し、**Next**をクリックします。
2.  「次へ」をクリックして、次のいくつかのウィンドウのデフォルトのオプションを受け入れることができます。

macOSの場合、お使いのmacOSのバージョンに合った*.dmg*ファイルをダウンロードし、ダブルクリックしてください。画面に表示されるインストーラーの指示に従って、Pythonをインストールしてください。

1.  DMGパッケージが新しいウィンドウで開いたら、*Python.mpkg* ファイルをダブルクリックします。管理者パスワードの入力が必要な場合があります。
2. 次のいくつかのウィンドウのデフォルトオプションを受け入れるには、**Continue**をクリックし、ライセンスを受け入れるには**Agree**をクリックします。
3.  最後のウィンドウで、**Install**をクリックします。

Ubuntuをお使いの場合、以下の手順でTerminalからPythonをインストールすることができます。

1.  ターミナルウィンドウを開きます。
2.  [sudo apt-get install python3]を入力します。
3.  [sudo apt-get install idle3]を入力します。
4.  [sudo apt-get install python3-pip]を入力します。

### **Muのダウンロードとインストール** 

Pythonインタプリタ*はPythonプログラムを実行するソフトウェアですが、*Muエディタソフトウェア*は、ワープロで入力するのと同じようにプログラムを入力する場所です。Muは*[https://codewith.mu/](https://codewith.mu/)*からダウンロードすることができます。

WindowsとmacOSの場合、あなたのオペレーティングシステム用のインストーラーをダウンロードし、インストーラーファイルをダブルクリックして実行します。macOSの場合、インストーラを実行するとウィンドウが開き、Muのアイコンをアプリケーションフォルダのアイコンにドラッグして、インストールを続行する必要があります。Ubuntuをお使いの場合、MuをPythonパッケージとしてインストールする必要があります。その場合、ダウンロードページのPythonパッケージのセクションにあるInstructionsボタンをクリックします。

### **Starting Mu** 

インストールが完了したら、Muを起動させましょう。

- Windows 7以降では、画面左下のスタートアイコンをクリックし、検索ボックスに[Mu]と入力し、選択します。
- macOSの場合、Finderウィンドウを開き、**アプリケーション**をクリックし、**mu-editor**をクリックします。
- Ubuntuでは、**Applications** ▸ **Accessories** ▸ **Terminal**を選択し、[python3 --m mu]と入力してください。

Muを初めて起動すると、Adafruit CircuitPython、BBC micro:bit、Pygame Zero、Python 3をオプションとするSelect Modeウィンドウが表示されます。**Python 3**を選択します。エディタウィンドウの上部にあるModeボタンをクリックすれば、後でいつでもモードを変更することができます。


**[NOTE]**

*本書で紹介するサードパーティモジュールをインストールするためには、Muバージョン1.10.0以降をダウンロードする必要があります。この記事を書いている時点では、1.10.0はアルファ版であり、ダウンロードページにはメインのダウンロードリンクとは別のリンクとして記載されています*。


### **Starting IDLE** 

This book uses Mu as an editor and interactive shell. However, you can use any number of editors for writing Python code. The *Integrated Development and Learning Environment (IDLE)* software installs along with Python, and it can serve as a second editor if for some reason you can't get Mu installed or working. Let's start IDLE now.

-   On Windows 7 or later, click the Start icon in the lower-left corner of your screen, enter [IDLE] in the search box, and select **IDLE (Python GUI)**.
-   On macOS, open the Finder window, click **Applications**, click **Python 3.8**, and then click the IDLE icon.
-   On Ubuntu, select **Applications** ▸ **Accessories** ▸ **Terminal** and then enter [idle3]. (You may also be able to click **Applications** at the top of the screen, select **Programming**, and then click **IDLE 3**.)

### **The Interactive Shell** 

When you run Mu, the window that appears is called the *file editor* window. You can open the *interactive shell* by clicking the REPL button. A shell is a program that lets you type instructions into the computer, much like the Terminal or Command Prompt on macOS and Windows, respectively. Python's interactive shell lets you enter instructions for the Python interpreter software to run. The computer reads the instructions you enter and runs them immediately.

In Mu, the interactive shell is a pane in the lower half of the window with the following text:

Jupyter QtConsole 4.3.1\
Python 3.6.3 (v3.6.3:2c5fed8, Oct 3 2017, 18:11:49) \[MSC v.1900 64 bit\
(AMD64)\]\
Type \'copyright\', \'credits\' or \'license\' for more information\
IPython 6.2.1 \-- An enhanced Interactive Python.
Type \'?\' for help.\
\
In \[1\]:

If you run IDLE, the interactive shell is the window that first appears.  It should be mostly blank except for text that looks something like this:

Python 3.8.0b1 (tags/v3.8.0b1:3b5deb0116, Jun 4 2019, 19:52:55) \[MSC
v.1916\
64 bit (AMD64)\] on win32\
Type \"help\", \"copyright\", \"credits\" or \"license\" for more
information.\
\>\>\>

[In \[1\]:] are called *prompts*. The examples in this book will use the [\>\>\>] prompt for the interactive shell since it's more common. If you run Python from the Terminal or Command Prompt, they'll use the [\>\>\>] prompt, as well. The [In \[1\]:] prompt was invented by Jupyter Notebook, another popular Python editor.

For example, enter the following into the interactive shell next to the prompt:

\>\>\> [print(\'Hello, world!\')]

After you type that line and press [ENTER], the interactive shell should display this in response:

\>\>\> [print(\'Hello, world!\')]\
Hello, world!

You've just given the computer an instruction, and it did what you told it to do!

### **Installing Third-Party Modules** 

Some Python code requires your program to import modules. Some of these modules come with Python, but others are third-party modules created by developers outside of the Python core dev team. [Appendix A](#calibre_link-2) has detailed instructions on how to use the [pip] program (on Windows) or [pip3] program (on macOS and Linux) to install third-party modules. Consult [Appendix A](#calibre_link-2) when this book instructs you to install a particular third-party module.

### **How to Find Help** 

Programmers tend to learn by searching the internet for answers to their questions. This is quite different from the way many people are accustomed to learning---through an in-person teacher who lectures and can answer questions. What's great about using the internet as a schoolroom is that there are whole communities of folks who can answer your questions. Indeed, your questions have probably already been answered, and the answers are waiting online for you to find them. If you encounter an error message or have trouble making your code work, you won't be the first person to have your problem, and finding a solution is easier than you might think.

For example, let's cause an error on purpose: enter [\'42\' + 3] into the interactive shell. You don't need to know what this instruction means right now, but the result should look like this:

   \>\>\> [\'42\' + 3]\
[➊] Traceback (most recent call last):\
     File \"\<pyshell#0\>\", line 1, in \<module\>\
       \'42\' + 3\
[➋] TypeError: Can\'t convert \'int\' object to str implicitly\
   \>\>\>

The error message [➋] appears because Python couldn't understand your instruction. The traceback part [➊] of the error message shows the specific instruction and line number that Python had trouble with. If you're not sure what to make of a particular error message, search for it online. Enter **"TypeError: Can't convert 'int' object to str implicitly"** (including the quotes) into your favorite search engine, and you should see tons of links explaining what the error message means and what causes it, as shown in [Figure 0-2](#calibre_link-1612).


![image](../images/000151.jpg)


*Figure 0-2: The Google results for an error message can be very helpful.*

You'll often find that someone else had the same question as you and that some other helpful person has already answered it. No one person can know everything about programming, so an everyday part of any software developer's job is looking up answers to technical questions.

### **Asking Smart Programming Questions** 

If you can't find the answer by searching online, try asking people in a web forum such as Stack Overflow (*[https://stackoverflow.com/](https://stackoverflow.com/)*) or the "learn programming" subreddit at *[https://reddit.com/r/learnprogramming/](https://reddit.com/r/learnprogramming/)*.  But keep in mind there are smart ways to ask programming questions that help others help you. To begin with, be sure to read the FAQ sections at these websites about the proper way to post questions.

When asking programming questions, remember to do the following:

-   Explain what you are trying to do, not just what you did. This lets your helper know if you are on the wrong track.

-   Specify the point at which the error happens. Does it occur at the very start of the program or only after you do a certain action?

-   Copy and paste the *entire* error message and your code to *[https://pastebin.com/](https://pastebin.com/)* or *[https://gist.github.com/](https://gist.github.com/)*.

    These websites make it easy to share large amounts of code with people online, without losing any text formatting. You can then put the URL of the posted code in your email or forum post. For example, here some pieces of code I've posted: *[https://pastebin.com/SzP2DbFx/](https://pastebin.com/SzP2DbFx/)* and *[https://gist.github.com/asweigart/6912168/](https://gist.github.com/asweigart/6912168/)*.

-   Explain what you've already tried to do to solve your problem. This tells people you've already put in some work to figure things out on your own.

-   List the version of Python you're using. (There are some key differences between version 2 Python interpreters and version 3 Python interpreters.) Also, say which operating system and version you're running.

-   If the error came up after you made a change to your code, explain exactly what you changed.

-   Say whether you're able to reproduce the error every time you run the program or whether it happens only after you perform certain actions. If the latter, then explain what those actions are.

Always follow good online etiquette as well. For example, don't post your questions in all caps or make unreasonable demands of the people trying to help you.

You can find more information on how to ask for programming help in the blog post at *[https://autbor.com/help/](https://autbor.com/help/)*. You can find a list of frequently asked questions about programming at *[https://www.reddit.com/r/learnprogramming/wiki/faq/](https://www.reddit.com/r/learnprogramming/wiki/faq/)* and a similar list about getting a job in software development at *[https://www.reddit.com/r/cscareerquestions/wiki/index/](https://www.reddit.com/r/cscareerquestions/wiki/index/)*.

I love helping people discover Python. I write programming tutorials on my blog at *[https://inventwithpython.com/blog/](https://inventwithpython.com/blog/)*, and you can contact me with questions at *[al@inventwithpython.com](mailto:al@inventwithpython.com)*.  Although, you may get a faster response by posting your questions to *[https://reddit.com/r/inventwithpython/](https://reddit.com/r/inventwithpython/)*.

### **Summary** 

For most people, their computer is just an appliance instead of a tool.  But by learning how to program, you'll gain access to one of the most powerful tools of the modern world, and you'll have fun along the way.  Programming isn't brain surgery---it's fine for amateurs to experiment and make mistakes.

This book assumes you have zero programming knowledge and will teach you quite a bit, but you may have questions beyond its scope. Remember that asking effective questions and knowing how to find answers are invaluable tools on your programming journey.

Let's begin!

