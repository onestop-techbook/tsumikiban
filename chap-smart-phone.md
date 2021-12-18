# Web技術とスマホで始める電子工作入門

基板を買わなければ、積み基板は生まれません。
そもそも作りたいものに基板は必要でしょうか？
よくよく考えてみると、作りたいものはスマートフォンのセンサーや機能で代替できるかもしれません。

## はじめに

本章ではスマートフォンでセンサーやカメラを利用したアプリケーションを作る際に知っておきたいAPIや、開発する際に知っておきたいTipsを紹介します。
また、App StoreやGoogle Playで配布されるネイティブアプリではなく、ウェブブラウザで動作するウェブアプリを対象とします。

ネイティブアプリはスマートフォンのハードウェアにアクセスできる範囲は大きいですが、開発環境を整えるために、SDKのインストールが必要だったり、開発する際に特定のOSが必要なこともありハードルが高めです。
一方、ウェブアプリはネイティブアプリほど自由度はありません。しかし、開発環境の構築が簡単で、プログラミングには書籍や教材の多いJavaScriptを使用するため入門に適しています。

### 事前知識

本章では簡単なHTMLとJavaScriptの知識を前提にしています。
電子工作に興味がある方であれば、JavaScriptやそれに類する言語の経験をお持ちかもしれません。
あまり経験がない方は、自信がない方はJavaScript Primerでの学習をオススメします。

https://jsprimer.net/

### 環境構築

ハードウェアデバイス機能を利用する場合、セキュリティ上の理由からHTTPS環境が必要になることが多いです。しかし、ローカル環境でHTTPS化を行うには手間がかかるます。
そこで本章ではCodeSandboxで開発環境を整える手法を推奨します。

CodeSandboxはブラウザ上で動作するIDEです。
ブラウザ上でコードを記述することができ、即時に変更が反映されるHTTPSのプレビューURLが生成されます。
会員登録は必要ですが、個人の学習・実験用途では無料で使えるので安心してください。

会員登録を済ませて、ヘッダーの「Create Sandbox」ボタンを押してSandbox（環境）を作成します。
Sandboxを作成する際にテンプレートを尋ねられるので「static」テンプレートを選択しましょう。

選択後、表示される画面がSandboxの画面です。
SandboxはCodeSandboxにおけるプロジェクトの単位で、複数のファイルが含まれています。
本章ではindex.htmlのみ編集する前提で進めていきます。

次のスクリーンショットのような画面が表示されるはずです。
左ペインにエディター、右ペインにプレビューが表示されます。右ペイン上部に表示されるURLをスマホに共有すれば、スマートフォンでも作っているものを確認できます。

![CodeSandboxの画面](images/chap-smartphone/codesandbox.png?scale=0.8)

#### コラム: ローカルマシンで開発したい方は

普段からWeb開発に慣れている方であればローカルのエディターや環境を使って作業をしたいかもしれません。

ローカルで開発を進める場合、スマートフォンから開発マシンの環境にアクセスする際、ローカルIPアドレスを利用します。
しかし、HTTPS対応を自分で行う必要があります。
このローカルIPアドレスに対してmkcertのようなツールを使い証明書を発行、スマートフォン側でインストールしHTTPSの利用を行います。

## ウェブ技術でアクセスできるハードウェア機能
通常ブラウザで動くJavaScriptでは、サーバーとの通信やDOM操作に使われるイメージが強いですが、スマートフォンに搭載されるカメラやセンサー類にアクセスできるAPIが用意されています。
これらのAPIを利用すれば電子工作でやりたかったことが実現できるかもしれません。
そこでJavaScriptでハードウェアを操作するAPIをいくつか紹介します。

### カメラの操作

#### 映像の取り込み

スマートフォンのカメラで撮った映像を取り込む方法を紹介します。

`getUserMedia`メソッドを利用すると、動画や音声が含まれるMediaStreamというストリームが帰ってきます。このストリームをあらかじめ用意したvideo要素にわたすと、カメラで撮影している映像が再生されます。

```html
<html>
  <video playsinline id="video"></video> <!-- ③ -->
  <button id="start">Start</button>
  <script>
    const btn = document.getElementById("start");
    const video = document.querySelector("video");
    btn.addEventListener("click", () => {
      video.play(); // ①
      navigator.mediaDevices
        .getUserMedia({ audio: false, video: true })
        .then((stream) => {
          video.srcObject = stream; // ②
        });
    });
  </script>
</html>
```

①ではボタンをクリックしたら、Videoを再生する処理を実行しています。これはiOSのための処理で、iOSではユーザーのインタラクション以外のタイミングでは動画の自動再生は行なえません。

②ではストリームをVideo要素のソースに設定しています。この行が重要な処理になります。

③ではVideo要素にplaysinline属性を与えています。これもiOSのための処理で、iOSでは全画面再生がデフォルトなのでこの属性を与えて、全画面再生を抑制しています。

カメラで撮影している映像を加工する場合はHTMLのCanvas要素を利用します。Canvas要素は、お絵かきチャットや3Dコンテンツの表示などの用途に使われますが、カメラで撮影している映像の表示にも使えます。

次のコードは、カメラで取得した映像をCanvasにレンダリングするものです。

```html
<video playsinline id="video"></video>
<canvas id="canvas"></canvas>
<button id="start">Start</button>
<script>
  const btn = document.getElementById("start");
  const video = document.querySelector("video");
  const canvas = document.getElementById("canvas");
  const context = canvas.getContext("2d");
  const tick = () => {
    context.drawImage(video, 0, 0);
    window.requestAnimationFrame(tick);
  };
  btn.addEventListener("click", () => {
    video.play();
    navigator.mediaDevices
      .getUserMedia({ audio: false, video: true })
      .then((stream) => {
        video.srcObject = stream;
        video.onloadeddata = () => {
          canvas.width = video.videoWidth;
          canvas.height = video.videoHeight;
          tick();
        };
      });
  });
</script>
```

#### 画像の加工

Canvasには描写されている画像データを取得する`getImageData`というメソッドと、事前に用意した画像データを反映する`pushImageData`というメソッドが用意されています。
取得できるデータの中には各ピクセルに含まれるRGBA値が0〜255の範囲で格納されています。このデータを加工し、それを反映することでフィルターのような効果を与えられます。

ImageDataのデータについて説明します。ImageDataはwidth, height, dataのプロパティを持ちwidthは幅、heightは高さ、dataがピクセルの色情報を持つデータです。
たとえば、dataが`[0,0,0,0, 255, 255, 255, 255]`だった場合、2ピクセルのデータで、1つ目のピクセルが`R: 0, B: 0, G: 0, A: 0`（透明な黒）、2つめのピクセルが`R: 255, B: 255, G: 255, A: 255`（不透明な白）であることを示しています。

ImageDataを使った例を示します。
この例では幅256px、高さ256pxのCanvas要素に対して、ランダムでRed（R: 255, B: 0, G: 0）・Blue（R: 0, B: 255, G: 0）・Green（R: 0, B: 255, G: 0）のうちの1色を各ピクセルに描写しています。
`getImageData`でCanvasのImageDataを取り出しfor文で各ピクセルに対して色の加工を行っています。

```html
<canvas width="256" height="256" id="canvas"></canvas>
<script>
  const canvas = document.getElementById("canvas");
  const context = canvas.getContext("2d");

  const width = 256;
  const height = 256;
  const imageData = context.getImageData(0, 0, width, height);
  for (let i = 0; i < width * height; i++) {
    const rand = Math.random(); // 0~1の範囲でランダムな数値を生成
    if (rand < 0.33) {
      imageData.data[4 * i + 0] = 255; // Red
    } else if (rand < 0.66) {
      imageData.data[4 * i + 1] = 255; // Blue
    } else {
      imageData.data[4 * i + 2] = 255; // Green
    }
    imageData.data[4 * i + 3] = 255; // Alpha
  }
  context.putImageData(imageData, 0, 0);
</script>
```

カメラ画像の取り込みと、画像の加工例を示しました。
これらを組み合わせ、撮影している映像にリアルタイムでフィルターを適用できます。

<!-- #### カメラ映像の加工

```html
``` -->

<!-- #### コラム: Slackとかに送る -->

### センサーの利用

スマホに搭載されている加速度センサーやジャイロセンサーもJavaScriptから利用できます。
端末の向きを利用したい場合は`DeviceOrientationEvent`、端末の動きを利用したい場合は`DeviceMotionEvent`というイベントが用意されています。

iOSでは端末のセンサーを利用する場合、ユーザーの同意を取得するコードが必要です。
紙面の都合上、本章ではAndroid向けにセンサーを利用するコードで説明を進めます。

#### 傾きを検知する

次のコードは、端末の傾きが変化するイベントを取得する例です。

```js
if (window.DeviceOrientationEvent) { // ①
  window.addEventListener('deviceorientation', (event) => {
    const { alpha, beta, gamma } = event // ②
    console.log({ alpha, beta, gamma })
  });
}
```

①では動作しているブラウザがDeviceOrientationEventをサポートしているか確認しています。

②ではイベントから端末の傾きを取り出しています。傾きはalpha, beta, gammaの3軸で表現されます。

この傾きに応じて、音を鳴らしたり、画面上のフィルター効果を変化させたりといろんな表現が可能です。

筆者は以前、端末の向きに応じて音程を変化させつつ音を出すウェブアプリケーションを作っています。
記事に制作のポイントをまとめた[^qiita]ので興味がある方はご覧ください。

[^qiita]: スマートフォンを楽器にしてみた https://qiita.com/mottox2/items/f54eadc38e98586909a2

#### 動きを検知する

次のコードは、端末の動きを検知して、一定速度を超えたタイミングでメッセージを表示する例です。

```js
<div id="result"></div>
<div id="status"></div>
<script>
  const round = (val, digit = 2) =>
    (Math.round((val * 10) ^ digit) / 10) ^ digit;

  const result = document.getElementById("result");
  const status = document.getElementById("status");

  window.addEventListener("devicemotion", (event) => {
    const { x, y, z } = event.acceleration; // ①
    const total = Math.abs(x) + Math.abs(y) + Math.abs(z); // ②
    if (total > 50) {
      result.innerText = "shaked!";
    }
    status.innerText = `x: ${round(x)}, y: ${round(y)}, z: ${round(z)}`;
  });
</script>
```

①ではDeviceMotionイベントに含まれるx, y, zの3方向の加速度を取得しています。

②では3方向の動きを足し合わせて、一定以上であれば端末に大きな動きがあったとみなしています。
ただし加速度は向きの関係で負の数字になる場合もあるので、絶対値を合わせたものを全体の加速度とします。


## 開発中に注意すること

<!-- ### ユーザーの同意

端末のハードウェア機能を利用する場合、ユーザーへの同意が必要になります。 -->

### iOS, Android間の差異

デバイスの機能を利用するアプリケーションを開発する際、注意しないといけないのはiOSとAndroid間の差異（ブラウザ間の挙動の差異）です。

iOSとAndroidではサポートされている機能や同意の取得フローに差分があります。「自分のスマートフォンだけで動けば問題ない」というケースであれば大丈夫ですが、別OSでも動かす場合は機能が存在しなかったり、動作に差があったりします。

OSごとのサポート状況はCan I Useというサイトで確認しましょう。このサイトでは使いたい機能がどのブラウザでサポートされているかを確認できます。
AndroidであればChrome for Android、iOSであればSafari on iOSの欄を確認してください。

https://caniuse.com/

また、本文中にいくつか注意があったようにiOS（Safari）は非常に癖の強い動作をすることがあります。
ハードウェアを活かしたものを作る場合、Android端末を用意するといいでしょう。
実装の難易度だけでなく、中古端末を用意する場合もAndroidのほうが安く、価格的なメリットもあります。

## まとめ

電子基板を買わなくても身の回りにあるスマートフォンを利用して、おもちゃを作れることがわかりました。
もちろん、ちゃんとしたものを作るにはJavaScriptに慣れる必要はありますが、Web開発の知見やエコシステムを生かせるので比較的次のステップに進みやすいと思います。
