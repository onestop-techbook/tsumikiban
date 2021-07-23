# 「Lチカ」の次のステップ問題を考える

ざっきー@Zakkiea

## はじめに
こんにちは。ざっきーと申します。某通信会社でインフラ設備の運用保守業務を担当しています。プライベートでは「野良ハック 」[^norahack]というモノづくりやコミュニティ活動を行なっています。

最近はESP32[^ESP32]というWi-Fi、Bluetooth機能を備えてArduino[^Arduino]IDEで開発可能な安価なチップを搭載した魅力的なデバイスがいくつも発売されており、ついつい欲しいデバイスに手を伸ばして「積み基板」を増やしてしまっていませんか。

[^norahack]: 野良ハック　https://qiita.com/yskmjp/items/cedbfd3f3980c42a8771
[^ESP32]: ESP32 Espressif Systems社が開発したWi-FiとBluetoothを内蔵する低コスト、低消費電力なSoCのマイクロコントローラ。
[^Arduino]: Arduino https://www.arduino.cc/

## 積み基板遍歴
入手したきっかけを思い出しながら自宅の「積み基板」をリストアップすると、

* ラズパイ[^rasPi] 大量

Linux系つながりで初期から使い始める。ソーシャルロボット「マグボット 」[^mugbot]の展示[^blog]のために大量購入する。主催するハンズオンイベントで使用。DonkeyCar[^donkeycar]走行会[^donkeycarevent]で使用。

* intel Edison[^intelEdison]

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

* micro:bit[^microbit] 

子供に渡す目的で購入。Nordic nRF51822チップが搭載されている。

* MDBT42Q[^mdbt] 

Nordic nRF52832チップが搭載されている。野良LEDバッジ初号機で使用。

* ラズパイPico

microPython、circuitPythonなどのスクリプト言語で開発できる。

以上のボードが目に入りましたが、これ以外にもっとあるかもしれません。(汗;

[^rasPi]: Raspberry Pi（ラズベリー パイ）の略称。ラズベリーパイ財団によって開発されているARMプロセッサを搭載したシングルボードコンピュータ。
[^mugbot]: mugbot http://www.mugbot.com/
[^blog]: zakkieaのブログ Yaita魅力フェスタ2018にマグボットを出展してきた。(2018.11.13) https://zakkiea.hatenablog.com/entry/2018/11/13/180749
[^donkeycar]: DONKEY CAR https://www.donkeycar.com/
[^donkeycarevent]:【MFT2019前夜】AIロボットカー走行会！ https://jsjug.connpass.com/event/140601/
[^intelEdison]: Intel Edison インテル社によって開発されたIoTデバイス向けのシングルボードコンピュータ。
[^amazonDash]: Amazon dash Amazonが開発したWi-Fi接続された専用デバイスで、ボタンを押すだけで日用品や食料品などを注文できる。
[^pepper]: Pepper https://www.softbank.jp/robot/pepper/
[^wioNode]: WioNode https://www.seeedstudio.com/Wio-Node.html
[^Grove]: Seeed Technology社が開発したモジュール方式のコネクタが特徴の挿すだけで使えるGroveシステム。
[^nefryBT]: NefryBT  https://dotstud.io/docs/nefrybt/
[^twilite]: TWI-Lite https://mono-wireless.com/jp/products/TWE-LITE/index.html
[^zigbee]: Zigbee Allianceが策定した短距離無線通信規格の1つ。低コスト、低消費電力でワイヤレスセンサーネットワークを主目的とし、電池駆動可能な超小型機器への実装に向いている。
[^gadgetrecesas]: がじぇっとるねさす https://www.renesas.com/jp/ja/products/gadget-renesas
[^obniz]: obniz https://obniz.com/ja/
[^m5stack]: M5Stack https://m5stack.com/
[^jetson]: Jetson https://www.nvidia.com/ja-jp/autonomous-machines/
[^makeblock]: makeblock https://www.makeblock.com/steam-kits/mbot
[^microbit]: MicroBit  https://microbit.org/
[^mdbt]: MDBT42Q https://www.raytac.com/product/ins.php?index_id=31

## モノづくりの第一歩「Lチカ」

さて、みなさんはモノづくりを始めるのにあたり最初の第一歩は「Lチカ[^LED]」が基本ですが、その次のステップをどうするか困ったり迷ったりしたことはありませんか。

私の第二歩目は依頼や相談がきっかけで、IoT縛りの勉強会！IoTLT[^IoTLT]というイベントでAmazon Dash Buttonをハック[^hack]してPepperを操作するデモンストレーションを行ったり、Pepperが撮影した写真をAirPrint[^airprint]でプリンターから印刷する試作を行いました。

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
これまで私が一番衝撃的だったのは「へっぽこまるこ」さんの「Lチカ」の次に「顔面ロボット[^facerobot]」の製作に取り組まれた事例です。見た目のインパクトが強く、とてもユニークな作品で、一度見たら絶対に忘れないと思います。

* 初号機（あけみ）
* 弐号機（お誕生日ケーキ）
* 参号機（リアルアンパンマン）

また、普段の天気予報で晴れ・くもり・雨・雪・雷などの天候と合わせて気温・湿度の情報はよく見聞きしますが、台風シーズンになると気圧（hPa）の情報を見聞きすることがありますので、気圧の変化を時系列で観察するイベントが行われたことがありました。

* 気温・湿度データの表示
* 気圧データ（時系列）の可視化

小学校の授業でも電気工作や電子工作が行われていますので、親子で一緒に取り組むのも良いと思います。

* ソーラー電気で走る車
* 手回しライト
* ラジオ

好きなことや、やりたいことがあれば第二歩目の選択は簡単かもしれませんが、もし迷った場合は身近な課題を解決したり、他の人が困っていることを助けるために試行錯誤してみてはいかがでしょうか。

[^facerobot]: https://dotstud.io/blog/face-robot-making-maruko/