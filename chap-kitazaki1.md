# 「Lチカ」の次のステップ問題を考える

ざっきー
@Zakkiea (Twitter)

## はじめに
こんにちは。ざっきーと申します。某通信会社でインフラ設備の運用保守業務を担当しています。プライベートでは「野良ハック 」[^norahack]というモノづくりやコミュニティ活動を行なっています。

最近はESP32[^ESP32]というWi-Fi、Bluetooth機能を備えてArduino[^Arduino]IDEで開発可能な安価なチップを搭載した魅力的なデバイスがいくつも発売されており、ついつい欲しいデバイスに手を伸ばして「積み基板」を増やしてしまっていませんか。

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


[^norahack]: 野良ハック　https://qiita.com/yskmjp/items/cedbfd3f3980c42a8771
[^ESP32]: ESP32 Espressif Systems社が開発したWi-FiとBluetoothを内蔵する低コスト、低消費電力なSoCのマイクロコントローラ。
[^Arduino]: Arduino https://www.arduino.cc/
[^rasPi]: Raspberry Pi（ラズベリー パイ）の略称。ラズベリーパイ財団によって開発されているARMプロセッサを搭載したシングルボードコンピュータ。
[^mugbot]: mugbot http://www.mugbot.com/
[^blog]: zakkieaのブログ Yaita魅力フェスタ2018にマグボットを出展してきた。(2018.11.13) https://zakkiea.hatenablog.com/entry/2018/11/13/180749
[^donkeycar]: DONKEY CAR https://www.donkeycar.com/
[^donkyecarevent]:　【MFT2019前夜】AIロボットカー走行会！ https://jsjug.connpass.com/event/140601/
[^intelEdison]: Intel Edison インテル社によって開発されたIoTデバイス向けのシングルボードコンピュータ。
[^amazonDash]: Amazon dash Amazonが開発したWi-Fi接続された専用デバイスで、ボタンを押すだけで日用品や食料品などを注文できる。
[^pepper]: Pepper https://www.softbank.jp/robot/pepper/
[^wioNode]: WioNode https://www.seeedstudio.com/Wio-Node.html
[^Grove]: Seeed Technology社が開発したモジュール方式のコネクタが特徴の挿すだけで使えるGroveシステム。