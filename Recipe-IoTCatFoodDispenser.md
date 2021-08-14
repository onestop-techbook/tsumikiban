# ペットフードディスペンサーをIoT化　～製作レシピ例～

IoTLTでの懇親会で「外出や出張・旅行で家を不在にする際のペットのエサやりどうしてる？！」という話題になったことを思い出した。うちにも野良ネコ(ゆき)が一匹いるので旅行の際はいつも悩んでいる。

![ゆき](images/Recipe-IoTCatFoodDispenser/cat.png?scale=0.5)

選択肢としては、

* ペットシッター
* ペットホテル
* 留守番

がある。

普段の外出や短期間(1泊2日程度)の出張・旅行であれば留守番となり、中位・低位機種のデジタルタイマーのペットフィーダー[^feeder1] やアナログタイマーのトレイタイプ[^feeder2]を併用している。上位機種はカメラ付きスマホ遠隔操作自動給餌器[^feeder3]もある。

[^feeder1]: ペット用自動給餌器ルスモ　http://www.lusmo.com/
[^feeder2]: PetSafe おるすばんフィーダー2食分 https://store.intl.petsafe.net/ja-jp/2-meal-pet-feeder
[^feeder3]: カメラ付き自動給餌器”ビストロわんにゃん”スマートペットフィーダー https://bestanswer.shop-pro.jp/?pid=133686464


ペットおよび飼い主にとって最適な方法を考えてみる。

* エサやりは確実に行いたい
    * 停電時でも電池式のデジタルタイマーまたはアナログタイマーでエサやり
    * 足りなさそうな時は遠隔からエサやり
* ネコの様子を見たい
    * カメラの映像を遠隔(スマホまたはタブレット)から確認
* 一緒に遊んであげたい
    * ネコじゃらしで遊ぶ
* はんだを使わずに電子工作をする
* ノンコーディングで実現する

## 構成
外出先からスマホまたはタブレットのブラウザ操作で自宅のネコの様子を確認したり、ネコじゃらしで遊んだり、エサやりを行えるようにする。自宅のラズパイにカメラ、ペットフィーダー(LUSMO)を接続し、ラズパイへインストールされたソフトウェア(Node-RED)をインターネットから制御することで実現する。NAT越え問題[^nat]を解決するためにngrokのサービスを利用する。

[^nat]: 家庭内ネットワーク(LAN)とインターネット(WAN)の間に境界(NAT)が存在し、セキュリティを確保する目的でWAN→LANのアクセスは基本的に制限されている。

![技術要素と構成](images/Recipe-IoTCatFoodDispenser/components.png?scale=0.7)

### 材料
* LUSMO x 1
* ラズパイ(Raspberry Pi 3 Model B または B+) x 1
    * microSDHCカード(16GB Class10) x 1
    * ラズパイ用ACアダプター(5V / 3.0A) x 1
    * カメラモジュール(Raspberry Pi Camera v1.3 または v2.1) x 1
* サーボモーター(SG-90) x 2
    * SG-90サーボ用2軸カメラマウント x 1
    * ジャンパーケーブル(オス - メス) x 6
    * Grove リレー x 1
    * Grove ジャンパー(メス)ケーブル x 1
    * ワニ口クリップコード x 2
* ドライバーセット

### 配線
ラズパイは有線LANまたは無線Wi-Fiでネットワークへ接続する。カメラモジュールはラズパイのCAMERA端子へ接続し、サーボモーターとGroveリレーはラズパイのGPIO端子へ接続する。

![結線ブロック図](images/Recipe-IoTCatFoodDispenser/connection.png?scale=0.7)

![GPIO端子の結線](images/Recipe-IoTCatFoodDispenser/GPIO.png?scale=0.7)

### LUSMO内の配線
ペットフードを入れるトレーを外し、(+)ドライバーでネジ(5箇所)を緩めてカバーを外す。

![LISMOの分解1](images/Recipe-IoTCatFoodDispenser/LISMO1.png?scale=0.7)

![LISMOの分解2](images/Recipe-IoTCatFoodDispenser/LISMO2.png?scale=0.7)

モーターの(-)電極と乾電池の(-)電極へワニ口クリップコードを接続し、コードの反対側をGrove リレーのスクリューターミナルへ接続する。スクリューターミナルは(-)ドライバーで開け締めを行う。

![モーターの接続](images/Recipe-IoTCatFoodDispenser/motor1.png?scale=0.7)

![リレーへの接続](images/Recipe-IoTCatFoodDispenser/relay1.png?scale=0.7)


### 使用するソフトウェア
* OS (Raspbian Stretch with desktop 2018-11-13)
* Node-RED v0.18.6-alpha.8 (enebular)
* Node-REDの追加ノード (メニュー → パレットの管理から行う)
    * node-red-contrib-camerapi
    * node-red-contrib-line-messaging-api
    * node-red-contrib-multipart-stream-decoder
    * node-red-contrib-ngrok
    * node-red-dashboard
    * node-red-node-base64
    * node-red-node-pi-gpiod
* uv4l-raspicam

インストール手順 ($ はコマンドプロンプト)

```sh
$ curl http://www.linux-projects.org/listing/uv4l_repo/lpkey.asc | sudo apt-key add -
$ echo 'deb http://www.linux-projects.org/listing/uv4l_repo/raspbian/stretch stretch main' | sudo tee -a /etc/apt/sources.list
$ sudo apt-get update
$ sudo apt-get install uv4l uv4l-raspicam uv4l-raspicam-extras
```

### 利用するサービス
* AWS IoT Core[^aws] 
→AWS IoT用IAMユーザーを作成し、アクセスキーIDとシークレットアクセスキーを取得する。
* enebular [^enebular]
→アカウントを作成し、Projectとflowを作成する。ラズパイへenebular AWS IoT agentをインストール[^AWSonRP]し、enebular editorからflowを編集/デプロイできるようにする。
* ngrok[ngrok]
→アカウントを作成し、authトークンを取得する。
* LINE Notify[^linebot] 
→アカウントを作成し、アクセストークンを発行する。


[^aws]: AWS IoT Core https://aws.amazon.com/jp/iot-core/
[^enebular]: enebular  https://enebular.com
[^AWSonRP]: 参考手順 https://qiita.com/TakedaHiromasa/items/b6828e4ac434bf99325d
[^ngrok]: ngrok https://dashboard.ngrok.com/user/signup
[^linebot]: line Notify https://notify-bot.line.me/ja/

