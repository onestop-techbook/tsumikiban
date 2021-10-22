# 「Lチカ」の次のステップ問題を考える

ざっきー@Zakkiea

## はじめに
こんにちは。ざっきーと申します。某通信会社でインフラ設備の運用保守業務を担当しています。プライベートでは「野良ハック 」[^norahack]というモノづくりやコミュニティ活動を行なっています。

最近はESP32[^ESP32]というWi-Fi、Bluetooth機能を備えてArduino[^Arduino]IDEで開発可能な安価なチップを搭載した魅力的なデバイスがいくつも発売されており、ついつい欲しいデバイスに手を伸ばして「積み基板」を増やしてしまっていませんか。

[^norahack]: 野良ハック　https://qiita.com/yskmjp/items/cedbfd3f3980c42a8771
[^ESP32]: ESP32 Espressif Systems社が開発したWi-FiとBluetoothを内蔵する低コスト、低消費電力なSoCのマイクロコントローラ。
[^Arduino]: Arduino https://www.arduino.cc/

## 積み基板遍歴
入手したきっかけを思い出しながら自宅の「積み基板」をリストアップすると、以下に列挙するボードが目に入りましたが、これ以外にもっとあるかもしれません。(汗;

* ラズパイ[^rasPi] 大量

Linux系つながりで初期から使い始める。ソーシャルロボット「マグボット 」[^mugbot]の展示[^blog]のために大量購入する。主催するハンズオンイベントで使用。DonkeyCar[^donkeycar]走行会[^donkeycarevent]で使用。

* Intel Edison[^intelEdison]

Linux系つながりから使い始める。Amazon Dash Button[^amazonDash]とPepper[^pepper]との連携で使用。当時はWi-FI＋Bluetooth機能で一択。残念ながら製造中止になる。

* Wio Node （ESP8266）[^wioNode]

野良ハック活動初期のメイン基板として使用。Wi-Fi機能とGrove[^Grove]コネクタを使用。

* ESP32 Dev Board

ESP8266の後継チップとして登場。Wi-Fi＋Bluetooth機能がついていることが話題になる。

* NefryBT[^nefryBT]

FRISKサイズのマイコンデバイスNefryの後継として登場。ESP32チップが搭載されている。

* TWE-Lite[^twilite] （Zigbee[^zigbee]）
屋外で無線マイコンモジュールを使用するワインIoT案件で、省電力・通信距離が決め手となって選択。電池駆動のメッシュ通信でアンカーにはラズパイを使用。

* がじぇっとるねさす[^gadgetrecesas] 

「ピンクのボード」 コンテストへ応募するためにGR-COTTONやGR-LYCHEEを購入。少しだけかじる。

* Arduino UNO（SORACOMイベントの参加賞の抽選でもらう。）
* Arduino Pro Micro（自作キーボードに使用。）
* Arduino UNO（中華製）（作品展示、バラマキ用に購入。）
* obniz （ESP32）[^obniz]

ウェブ系つながりから使い始める。組み込みデバイスのコーディングが不要なのが特徴。

* M5Stack （ESP32）[^m5stack]

ガワ（ケース）がしっかりしていて、安価で種類が豊富。日本のコミュニティが盛り上がっている。

* Jetson （Nano、Xavier NX）[^jetson]

ラズパイ＋GPU、NVIDIAグラフィックボードの印象から使い始める。

* Makeblock mBot[^makeblock]

子供向け教育教材として購入。中身はArduino。

* Micro:bit[^microbit] 

子供に渡す目的で購入。Nordic nRF51822チップが搭載されている。

* MDBT42Q[^mdbt] 

Nordic nRF52832チップが搭載されている。野良LEDバッジ初号機で使用。

* ラズパイPico

microPython、circuitPythonなどのスクリプト言語で開発できる。



[^rasPi]: Raspberry Pi（ラズベリー パイ）の略称。ラズベリーパイ財団によって開発されているARMプロセッサを搭載したシングルボードコンピュータ。
[^mugbot]: mugbot http://www.mugbot.com/
[^blog]: zakkieaのブログ Yaita魅力フェスタ2018にマグボットを出展してきた。(2018.11.13) https://zakkiea.hatenablog.com/entry/2018/11/13/180749
[^donkeycar]: DONKEY CAR https://www.donkeycar.com/
[^donkeycarevent]:【MFT2019前夜】AIロボットカー走行会！ https://jsjug.connpass.com/event/140601/
[^intelEdison]: Intel Edison インテル社によって開発されたIoTデバイス向けのシングルボードコンピュータ。
[^amazonDash]: Amazon Dash Amazonが開発したWi-Fi接続された専用デバイスで、ボタンを押すだけで日用品や食料品などを注文できる。
[^pepper]: Pepper https://www.softbank.jp/robot/pepper/
[^wioNode]: WioNode https://www.seeedstudio.com/Wio-Node.html
[^Grove]: Seeed Technology社が開発したモジュール方式のコネクタが特徴の挿すだけで使えるGroveシステム。
[^nefryBT]: NefryBT  https://dotstud.io/docs/nefrybt/
[^twilite]: TWE-Lite https://mono-wireless.com/jp/products/TWE-LITE/index.html
[^zigbee]: Zigbee Allianceが策定した短距離無線通信規格の1つ。低コスト、低消費電力でワイヤレスセンサーネットワークを主目的とし、電池駆動可能な超小型機器への実装に向いている。
[^gadgetrecesas]: がじぇっとるねさす https://www.renesas.com/jp/ja/products/gadget-renesas
[^obniz]: obniz https://obniz.com/ja/
[^m5stack]: M5Stack https://m5stack.com/
[^jetson]: Jetson https://www.nvidia.com/ja-jp/autonomous-machines/
[^makeblock]: makeblock https://www.makeblock.com/steam-kits/mbot
[^microbit]: Micro:Bit  https://microbit.org/
[^mdbt]: MDBT42Q https://www.raytac.com/product/ins.php?index_id=31

## モノづくりの第一歩「Lチカ」

さて、みなさんはモノづくりを始めるのにあたり最初の第一歩は「Lチカ[^LED]」が基本ですが、その次のステップをどうするか困ったり迷ったりしたことはありませんか。

私の第二歩目は依頼や相談がきっかけで、IoT縛りの勉強会！IoTLT[^IoTLT]というイベントでAmazon Dash Buttonをハック[^hack]してPepperを操作するデモンストレーションを行ったり、Pepperが撮影した写真をAirPrint[^airprint]でプリンターから印刷する試作を行いました。

![PepperにAmazon Dash Buttonを貼り付けた様子。Pepperの中にIntel Edison基板を仕込む。](images/chap-kitazaki1/pepper.jpg?scale=0.5)

これまでの野良ハック活動で使用してきた要素技術を簡単に書き出してみます。

[^LED]: LEDチカチカの略。Arduino 等のマイコン工作を始めるとき、最初に LED を点滅（チカチカ）させるプログラムで試すことが多いことから「LEDチカチカ」→「Lチカ」と呼ばれる。
[^IoTLT]: IoTLT https://iotlt.connpass.com/
[^hack]: 物事を効率よくこなしたり質を上げたりするためのコツやテクニック。既存の制約を打破したり回避する創意工夫、その取り組み。
[^airprint]: AppleのMac OS X Lion以降とiOS 4.2以降の機能で、無線LANを通してAirPrint対応プリンターもしくはWindows、macOS、GNU/Linux PCに接続している非対応プリンターに直接印刷できる機能。

* LED（RGB（フルカラーLED）、WS2812B[^ws2812b]）
    * カンパイシェア
    * 「デカ顔箱[^dekakao]」をIoT化
* スイッチ（リレー、MOS-FET）
    * おつまみディスペンサーをIoT化
    * アロマディフューザーをIoT化
* モーター（サーボ、ギヤード）
    * ドリンクディスペンサーをIoT化
    * 「カウントダウンだけで宴会は盛り上がる[^countdown]」をIoT化
* センサー（温湿度、気圧、照度、CO2濃度、スライド抵抗値）
    * I2C[^i2c]
        * ワインIoT
    * UART[^uart] 
        * Nefry BTとCO2センサー（MH-Z19）で職場環境を見える化
        * Nefry BTとDFPlayer miniで音楽を再生
    * アナログ（ADC[^adc]）
        * ESP8266でTouchDesigner[^touchdesigner]のMQTT[^mqtt]Client DATを試してみた
* ディスプレイ
    * enebular[^enebular] AWS IoT agentを使用してラズパイのマルチモニター環境を制御
* カメラ
    * ラズパイカメラからNDI[^ndi]送信
* 制御（MQTT、WebSocket、DMX[^dmx]、ArtNet[^artnet]、NDI）
* 無線（Wi-Fi、Bluetooth、Zigbee）

![ワインIoT案件でTWE-Lite基板を使用](images/chap-kitazaki1/wine.jpg?scale=0.5)

![カンパイシェア。グラス（江戸切子）の底面にGR-COTTON基板を仕込む。コンテスト（ルネサスデザインコンテスト2017）へ応募するために製作](images/chap-kitazaki1/kanpaishare.jpg?scale=0.3)

[^ws2812b]: シリアル通信でRGB値が制御できる小型のマイコン内蔵LED。
[^dekakao]: デカ顔箱 https://dailyportalz.jp/kiji/160825197264
[^countdown]: https://dailyportalz.jp/kiji/180126201872
[^i2c]: 電子部品と通信する際に使用する端子で、センサーから計測した値を取得したり、ディスプレイに値 を表示したり、モーターなどを制御する際に使用します。SDAはデータの送受信、SCLは接続した電子部品同士の同期に使用されます。
[^uart]: 電子部品や外部のコンピュータなどとデータのやり取りをする際に使用する端子で、TXはデータの送信、RXはデータの受信に使用されます。
[^adc]: ADC（Analog-to-Digital Converter）アナログデジタル変換器。
[^touchdesigner]: https://derivative.ca/
[^mqtt]: MQTT（Message Queuing Telemetry Transport）IoTやM2M向けに開発された軽量プロトコル。
[^enebular]: https://www.enebular.com/ja/
[^ndi]: NDI（Network Device Interface）NewTek社によって開発されたロイヤリティフリーのIPビデオ伝送方式。
[^dmx]: 照明や舞台効果を制御する為の通信プロトコル（通信手順、ルール）で、制御信号を発信するコントローラとその信号に反応する機器（デバイス）の通信方式を定める規格。
[^artnet]: Artistic License社によって策定されたDMX信号をイーサネットを介して送受信するための通信プロトコル。

 

## 「Lチカ」の次のステップ
これまで私が一番衝撃的だったのは「へっぽこまるこ」さんの「顔面ロボット[^facerobot]」です。何とLチカの次の製作に取り組まれた事例だそうです。見た目のインパクトが強く、とてもユニークな作品で、一度見たら絶対に忘れないと思います。

* 初号機（あけみ）
* 弐号機（お誕生日ケーキ）
* 参号機（リアルアンパンマン）

また、普段の天気予報で晴れ・くもり・雨・雪・雷などの天候と合わせて気温・湿度の情報はよく見聞きしますが、台風シーズンになると気圧の情報を見聞きすることがありますので、気圧の変化を時系列で観察するイベントが行われたことがありました。センサや取り込んだデータの表示といった要素技術を使い、かつ実用的でわかりやすい作例です。

* 気温・湿度データの表示
* 気圧データ（時系列）の可視化

![気圧データの可視化。グラフの傾きから台風の接近スピードよりも抜けるスピードの方が早いことがわかる。](images/chap-kitazaki1/ambient.jpg?scale=0.3)

小学校の授業でも電気工作や電子工作が行われていますので、親子で一緒に取り組むのも良いと思います。

* ソーラー電気で走る車
* 手回しライト
* ラジオ

好きなことや、やりたいことがあれば第二歩目の選択は簡単かもしれませんが、もし迷った場合は身近な課題を解決したり、他の人が困っていることを助けるために試行錯誤してみてはいかがでしょうか。

[^facerobot]: https://dotstud.io/blog/face-robot-making-maruko/

## 次のステップ（レシピ）の紹介
例えば、「Lチカ」と同じ技術を使用して、GPIOのON / OFFでLEDの点滅だけでなく、リレースイッチ[^relay]のON / OFFも行うことができます。リレースイッチのON /OFFで各ディスペンサーに内臓されたモーターをIoTで動かす例です。

[^relay]: 継電器。比較的弱い電流（あるいは電気信号）を受け取り、電気回路の開閉（スイッチング）を行い、その先の回路に比較的大きな電流（電気信号）伝える装置。

*	ディスペンサー系
    * おつまみディスペンサーをIoT化
    * ドリンクディスペンサーをIoT化
    * ペットフードディスペンサーをIoT化
    * かき氷器をIoT化

![ドリンクディスペンサー（サングリア）をIoT化。Wio Node基板を仕込み、スマートスピーカーからサーボモーターを駆動。](images/chap-kitazaki1/iotdrink.png?scale=0.3)

「Lチカ」、LEDの延長線で、光モノ系へ進むのも楽しいと思います。

* 光モノ系（パリピ[^partypeople]）
    * カンパイシェア
    * 「デカ顔箱」をIoT化

[^partypeople]: パーティ・ピープル（party people）の音略。パーティ好きな人々や盛り上がることを好む人を意味する。

モーターの延長線で、ギヤボックス付きのモーターを使用したり、モーターの種類を変更してPWMで制御するサーボモーターを使用するとできることが広がります。

* ディスペンサー系
    * ドリンクディスペンサーをIoT化
        * サングリア
    * 「カウントダウンだけで宴会は盛り上がる」をIoT化

![「カウントダウンだけで宴会は盛り上がる」をIoT化。Wio Node基板を仕込み、カウントダウンに合わせてギヤードモーターを操作してクラッカーを発射！](images/chap-kitazaki1/peppercountdown.png?scale=0.5)

音と映像を組み合わせたメディアアート系、配信技術を組み合わせるのも面白いと思います。

* 音系
* 映像系
* 制御系

![DMX、ArtNetで照明機器を操作し、TouchDesignerを使用して音声、映像の配信制御を行う。](images/chap-kitazaki1/mediacontrol.png?scale=0.5)

## 積み基板が増える原因を考える

* 案件（無茶振り。何かデモして。何かおもしろいモノ作って。)

私の場合は「Lチカ」の次のステップとして依頼や相談がきっかけで、IoT縛りの勉強会！IoTLTというイベントで来場者向けに「賑やかし」デモを行ったり、イベント後の懇親会でお酒やおつまみをIoTで振る舞う（ディスペンスする）デモを行ったり、Amazon Dash ButtonとPepperをハックしたデモがきっかけでPepperが撮影した記念写真をAirPrintでプリンターから印刷する案件[^pepperprint]の試作を行いました。最初から期限が切られて、無茶な内容が多かったこともあり、必要になりそうなデバイスやパーツを多めに購入しました。買ったけど使わなかった「積み基板」が...

* 問い合わせ（おすすめは何？）

モノ作り活動を始めてコミュニティなどでLT発表をしたり、アウトプットしたものをデモで展示すると、興味を持った人から話しかけられ、「おすすめは何？」と必ず聞かれます。聞かれると回答（提案）しないといけないので、自分が使っているデバイス以外にも興味を持つことになります。すると気がついた時には沼にハマっています。少し試しただけの「積み基板」が...

* イベント
    * 技術書典[^techbookfest]MFT[^mft]などへの出展・展示・参加

刺激が多くテンションも上がっているので、即売会で購入した「積み基板」が...

[^pepperprint]: https://www.agoop.co.jp/2017/08/09/2697/
[^techbookfest]: 技術書典 https://techbookfest.org/
[^mft]: MFT（Maker Faire Tokyo）https://makezine.jp/
[^arduinofun]: https://www.facebook.com/groups/arduinofun/


* 勉強会、登壇、LT、デモ、懇親会

興味が出る　→　欲しくなる　→　買う　→　試す　→　飽きる

周りで騒いている、周りが持っている　→　気になる、欲しくなる　→　買う　→　試す　→　飽きる

飽きてしまった「積み基板」が...

* きっかけ
    * ものづくりコミュニティ（Arduinoファン[^arduinofun]、IoTLT、M5Stackなど）を知る、関わる
    * 某社内にコミュニティの活動スペースができる

「積み基板」に囲まれる景色、日常が...

## 積み基板を解消する方法を考える

* ひとりでやらない（仲間を作る、コミュニティに加わる、コミュニティを作る)

仲間を作って一緒に活動すると楽しいですよね！アイデアを出して一気に「積み基板」を解消しましょう！

![MFT2019で集合写真](images/chap-kitazaki1/mft2019.jpg?scale=0.5)

* 案件（案件に関わる、作る）

「積み基板」を解消することができます。
案件は降って湧くモノなので、普段から「積み基板」をある程度用意して構えておく必要があるので、本末転倒になることも...

* テーマを決める、絞る

私の場合は勉強会やイベントの賑やかし（光モノ系）、季節感のある（かき氷器をIoT化、雛チカ（雛祭り）、クリチカ（クリスマスツリー）、門松チカ、鏡餅チカ、）、お菓子・おつまみ・飲み物（ディスペンサー系）というようにテーマを決めて活動しました。

* ベースとするデバイスを決める

私の場合はWio Node（ESP8266）とGroveコネクタで接続できるモジュール縛りをしました。
モジュールは増えますが、「積み基板」は増えません。

* アウトプットを継続する（させる）ための仕組み（イベント駆動）
    * 定期的に開催されるイベントや勉強会でLT発表する
    * コンテストへの応募
    * MFTなどへの出展

* 精神論（開き直り）

積み基板が増えても、活用すれば良い！いつか活用できれば良い！

* 愛着

「積み基板」への愛着を増やすには基板を自作することです。（断言）

私が初めて基板を自作したのは、学生の時に卒論研究で肺音測定のための回路基板を銅板エッチングで作ったことです。（すごい大変だった記憶）

その後しばらく時を経て、野良ハック活動を始めてワインIoT（TWE-Liteと複数のセンサーデバイスを使用して電池駆動で屋外使用）の案件で、P板.comで基板を外注しました。この時に初めてガーバーファイル[^gerber]を覚えます。（基板の自作が「安価で楽（ラク）」へ記憶の上書き)

[^gerber]: ガーバーファイル形式（Gerber file format）プリント基板業界でデファクトスタンダードとして用いられているファイル形式

## 自作基板遍歴

* KiCadハンズオンへ参加してArduino互換基板を作る。（と、言っても最初からある程度完成していて、パーツを少し加える程度）
* obnizへアタッチする基板（2.54mmピッチ→2mm Groveコネクタ変換基板）を作る。
* 野良LEDバッジ[^norabadge]基板
* 商業誌向け基板の試作（ラズパイとJetson Nano共通電力測定ボード[^powermeasure])
* レジン電子工作基板（光る！野良ネコバッジ[^noraneko]）
* 初心者向け電子剣山基板（LED極性なし、電流制限抵抗内蔵）


![野良バッジ](images/chap-kitazaki1/norabadge.png?scale=0.5)

![初心者向け電子剣山基板](images/chap-kitazaki1/kenzan.png?scale=0.5)

[^norabadge]:  https://norahack-led.booth.pm/
[^powermeasure]: https://bit-trade-one.co.jp/adjtsb01/
[^noraneko]: https://connpass.com/event/188125/

## さいごに
「積み基板」が増える原因も解消する方法も表裏一体なので、電気工作や電子工作を通して人生を楽しんだ方が良いという結論に達しました。
「積み基板」を見て罪悪感を感じるより、モノづくりやプロトタイピングの試行錯誤の過程を楽しんでみませんか。
みなさま、良き「積み基板」ライフを！
