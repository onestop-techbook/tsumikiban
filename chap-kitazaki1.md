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

* TWE-Lite[^twelite] （Zigbee[^zigbee]）
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