[![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_01_fv1_pic.jpg)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_01_fv1_pic.jpg)
[Spin Semiconductor SPN1001 FV-1](https://www.spinsemi.com/products.html)は2007年頃から使われているリバーブICです。空間系エフェクターの自作に使ってみたいという方がいらっしゃると思うので、私も一度作ってみることにしました。データシートにはリバーブと書いてありますが、様々なデジタル信号処理が可能です。カテゴリーは便宜上「自作エフェクター（アナログ）」にしています。

2024年10月時点では、秋月電子でFV-1を入手できます。[FV-1単体（2480円）](https://akizukidenshi.com/catalog/g/g115036/)より[DIP化モジュールキット（1480円）](https://akizukidenshi.com/catalog/g/g115566/)の方が安いので、モジュールの方を利用することにしました。次のロットからはFV-1単体より高くなるのかもしれません。

内部写真では正論理のロータリースイッチが使ってありますが、本来は負論理のものがよいです。また、ヘッドフォン駆動に有利なようにオペアンプをNJM4580にしています。



------

▽回路図（KiCadデータは[こちら](https://github.com/kanengomibako/misc/tree/main/FV1Board)へ）
[![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_02_fv1_sche.png)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_02_fv1_sche.png)
PedalPCBの[FV-1 Development PCB](https://www.pedalpcb.com/product/fv1dev/)を簡略化したものとなっています。FV-1周辺の部品はモジュール上に実装されているので、実際のパーツ数は少ないです。MIXが最大でも原音側がわずかに混ざるので、気になる場合はバッファを追加するか、R13・C13のローパスフィルターを省いてもよいでしょう。

モジュール基板裏側のジャンパーはアクセスしづらいので、別途ピンヘッダを用意しました。ここにジャンパーピンをつけると内部プリセットプログラムが読み込まれます。何もつけない場合、EEPROMというメモリーに保存されたプログラムが読み込まれます。24LC32Aは、秋月電子に取り扱いがある[AT24C32E](https://akizukidenshi.com/catalog/g/g115715/)や[24LC64](https://akizukidenshi.com/catalog/g/g100194/)で代用できるようです。EEPROMにプログラムを書き込む方法については後述します。

1回路8接点以上のロータリースイッチでエフェクトを切り替える場合、[Arachnid MultiFX Platform](https://www.pedalpcb.com/product/arachnid/)のようにダイオードを使います。秋月電子のFV-1モジュールではS0～S2ピンがプルアップ抵抗を介して電源に接続されているので、ロータリースイッチのコモン端子をグラウンドに接続し、ショットキーダイオードを使うとよいと思います。

・周波数特性 ※±1dBに拡大
[![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_03_fv1_f.png)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_03_fv1_f.png)
サンプリング周波数が32.768kHzなので、帯域幅が少し狭くなります。基本的に原音はアナログでミックスするので問題ないでしょう。

水晶振動子を交換し、サンプリング周波数をもっと高くすることもできます。ただ48kHzの水晶振動子は流通していないようで、追加の回路が必要です（詳細：[The FV-1 Crystal Oscillator](https://www.spinsemi.com/knowledge_base/xtal.html)）。

・正弦波 約1kHz
[![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_04_fv1_1k.png)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_04_fv1_1k.png)
SN比はそこまで悪くないものの、ハイゲインなエフェクト処理をするのには向いていないかと思います。レイテンシーは、約1.5msでした。



------

【外部エフェクト書き込み】

EEPROMにプログラムを書き込み、FV-1で実行できるようにします。下記参考ページとほぼ同じ内容をまとめています。
・[HOME BAKE INSTRUMENTS　REVERBS/EFFECTS　日本語説明書](https://home-bake-instruments.localinfo.jp/posts/37592163/)

1. 開発環境 ※Windowsのみ対応
   [公式ページ](https://www.spinsemi.com/products.html)下部のsoftware :: SpinAsm assembler for the SPN1001から、FV-1用プログラムの開発環境ソフトをダウンロード、インストールします。このソフトは管理者として実行する必要があるので、右クリック→『管理者として実行』とするか、プロパティ→互換性→『管理者としてこのプログラムを実行する』にチェックを入れておきます。

2. 

3. エフェクトプログラムの準備
   下記ページからエフェクトプログラム（spnファイル）をダウンロードします。
   ・[FV-1 Programs](https://mstratman.github.io/fv1-programs/)
   ※多くのエフェクトでは原音をミックスして出力するのが前提となっています。

4. 

5. hexファイル出力
   インストールしたSpinAsm assemblerを管理者として実行します。横棒のマーク（Open Project Dialog）をクリックし開くウインドウで、それぞれの行をダブルクリックし書き込みたいspnファイルを選びます。
   [![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_11_fv1_spn.png)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_11_fv1_spn.png)
   Intel Hexにチェックを入れBuildボタンを押すと、C:\Program Files (x86)\SpinAsm IDE\hexoutフォルダににhexファイルが出力されます。

6. 

7. binファイルへ変換
   hexファイルのままでは書き込みできないので、binファイルへ変換する必要があります。HOME BAKE INSTRUMENTSの[FV1_HexToBin](https://www.dropbox.com/scl/fi/sn98rg0gm2od3xgum70g7/FV1_HexToBin.zip?rlkey=0cwekj6wdwrmpzqyw0qnnuf5o&e=1&dl=0)ツールを利用すると簡単です。私の環境では、HexToBin.batと同じフォルダにhexファイルを置いてからドラッグ＆ドロップするとうまくいきました。

8. 

9. 書き込み
   CH341が搭載された書き込み機器（[Amazon](https://www.amazon.co.jp/Ren-He-プログラミング器-ROMライター-EEPROMルーティングUSBプログラマ/dp/B07LGNTJ29)や[AliExpress](https://www.aliexpress.com/item/1005007524802120.html)等で購入可能）を使います。書き込み用ソフト[AsProgrammer](https://github.com/nofeletru/UsbAsp-flash/releases)をダウンロードし、driversフォルダ→CH34Xフォルダ内のCH34XPAR.EXEを実行してCH341のドライバーをインストールします。書き込み機をPCに接続し、EEPROMを下写真のようにセットしておきます。※EEPROMをFV-1の基板に搭載したまま、4・5・6ピン（GND・SDA・SCL）を接続して書き込むことも可能です。
   [![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_12_fv1_ch.jpg)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_12_fv1_ch.jpg)
   Arduino等のマイコンを用いた書き込み方法もあるので、使いなれたマイコンがある方はそちらを検索してみてください。

   AsProgrammerを起動し、書き込みするbinファイルを開きます。ICメニューから『_24C32』を選択し、IC書込ボタンを押すとすぐに書き込みが完了します。
   [![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_13_fv1_as.png)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_13_fv1_as.png)
   EEPROMをFV-1の基板に移し、エフェクトが読み込まれれば成功です。

------

【プログラミング】

オリジナルのエフェクトをプログラミングすることも可能です。[Getting Started with the FV-1](https://electric-canary.com/fv1start.html)というページや[公式サイト](https://www.spinsemi.com/knowledge_base.html)に多くの情報があり参考になります。

基本的にはアセンブラというプログラミング言語を使う必要がありますが、グラフィカルなインターフェースでプログラミングできるようにした[SpinCAD Designer](https://holy-city-audio.gitbook.io/spincad-designer)というツールがあるので紹介しておきます。

まず実行環境として[Java](https://www.java.com/ja/download/)をインストールしておきます。そして[SpinCAD Designerのjarファイルをダウンロード](https://github.com/HolyCityAudio/SpinCAD-Designer/releases/)し、実行します。
[![img](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_14_fv1_ds.png)](https://blog-imgs-171-origin.fc2.com/d/r/u/drugscore/02_357_14_fv1_ds.png)
いくつかのブロックを線でつなぎ合わせることでプログラミングできます。一通りのエフェクトやタップテンポもあるようです。興味がある方はぜひチャレンジしてみてください。