# 情報基礎2\#9

慶應義塾大学環境情報学部
清水智公 (chiko@tom.sfc.keio.ac.jp)

---

## 前回の復習

~~~javascript
function calc(expression){
  var accumulator = expression[0];
  var i = 2;

  while(i < expression.length){
    var operator = expression[i - 1];
    var operant = expression[i];

    if(operator == "+"){
      accumulator = accumulator + operant;
    }else{
      accumulator = accumulator - operant;
    }

    i = i + 2;
  }
  return accumulator;
}
~~~

----

## 前回の復習（つづき）

~~~
function testCalc1(){
  var exp = [1, "+", 1];
  var result = calc(exp);
  console.log(result == 2);
}
function testCalc2(){
  var exp = [1, "-", 1];
  var result = calc(exp);
  console.log(result == 0);
}
function testCalc3(){
  var exp = [1, "+", 1, "-", 1];
  var result = calc(exp);
  console.log(result == 1);
}
~~~

---

## 今日の内容

* DOM API
    * Document Object Model
    * Application Programming Interface
* HTML の操作
* document.querySelector による検索
* イベント処理

----

### 今日の資料

* [サンプルコード](https://github.com/sfcjs2016s/code09)
* 音源：[Free Music Archive](http://freemusicarchive.org/curator/creative_commons/)

---

## 操作と対象

~~~
1 + 1;     // 2
"1" + "1"; // "11"

1 - 1;     // 0;
"1" - "1"; // エラー
~~~

* 対象に対する操作を繰り返すことで目的を達成する
* 可能な操作は、対象（の種類）によって決まる

----

### API

~~~
var elm = document.querySelector("#remove-me");
var parent = elm.parentElement;
parent.removeChild(elm);
~~~

* Application Programming Interface
* 操作対象の種類と、各対象に行える操作のセット
* JavaScript の場合、操作対象はメソッドおよび属性の集まりとして定義される

---

## DOM: Document Object Model

* HTML / XML 文書のための API
    * HTML 文書を木構造として抽象化（[DOMツリー](https://developer.mozilla.org/ja/docs/Using_the_W3C_DOM_Level_1_Core)）
    * document オブジェクトを根とする木構造
    * HTML 中の各要素が、木構造中のノードになる
* APIを利用して、プログラムから HTML を操作する

----

### 木構造（[WiKiPedia:木構造](https://ja.wikipedia.org/wiki/%E6%9C%A8%E6%A7%8B%E9%80%A0_(%E3%83%87%E3%83%BC%E3%82%BF%E6%A7%8B%E9%80%A0) より）

![木構造](https://upload.wikimedia.org/wikipedia/ja/2/20/Tree_structure%28data%29_01.png)

----

### document オブジェクト

* ブラウザに読み込まれた Web ページを表す
* ページ内の各要素へのアクセス方法を提供する
* c.f. [document](https://developer.mozilla.org/ja/docs/Web/API/document)

----

### DOM プログラミングの例

~~~
var html, body, h1;
document
document.children

html = document.children[0];
html.children;
body = html.children[1];

body.children;
h1 = body.children[0];

h1.parentElement == body; // true
body.removeChild(h1);
body.appendChild(h1);
~~~

---

## audio 要素

~~~
<audio src="audio/01.mp3"></audio>
~~~

* ページ内に埋め込まれた音声を表す要素
* src 属性に埋め込む音声の URL を記述する
* その他の詳細は [audio 要素](https://developer.mozilla.org/ja/docs/Web/HTML/Element/audio) を参照のこと

----

### JS からの audio 要素の利用

~~~
var html, body, audio;
html = document.children[0];
body = html.children[1];
audio = body.children[1];

audio.play();
audio.pause();
~~~

* API 定義：[HTMLMediaElement](https://developer.mozilla.org/ja/docs/Web/API/HTMLMediaElement)
* メソッドや属性を通じて、再生や停止などの操作を行う

----

### play / pause

~~~
audio.play();
audio.pause();
~~~

* play：音声を再生するメソッド
* pause：音声を一時停止するメソッド
* pause メソッドを呼び出し後、play メソッドを呼ぶと、一時停止された時点から再生される

----

### 音声の再生位置

~~~
audio.currentTime;
audio.play();
audio.currentTiem = 0;
audio.currentTime = audio.currentTime + 10;
~~~

* 音声ファイル全体の中での、再生中の位置
* 音声の先頭からの経過時間で位置を表現する
* currentTime 属性
     * 参照することで位置を秒単位で取得できる
     * 代入によって、再生する位置を変更できる

---

## 練習問題 1

* 次の仕様で関数を定義せよ
    * 指定された audio 要素の再生位置を 10 秒戻す
    * 引数：1 つの audio 要素
    * 返り値：戻した後の再生位置
    * 関数名：rewind10sec
* 定義は js/audio.js に記述せよ
* currentTime 属性に数値を代入することで、再生位置を変更できる

---

## 練習問題 2

* 次の仕様で関数を定義せよ
    * 指定された audio 要素の再生位置を 10 秒進める
    * 引数：1 つの audio 要素
    * 返り値：戻した後の再生位置
    * 関数名：forward10sec
* 定義は js/audio.js に記述せよ
* currentTime 属性に数値を代入することで、再生位置を変更できる

---

## document.querySelector

~~~
var html, body, audio;
html = document.children[0];
body = html.children[1];
audio = body.children[1];

var result;
result = document.querySelector("audio");
result == audio; // true
~~~

* 親子関係から、操作する対象の要素を探すのは面倒
* このメソッドを利用することで、より簡潔に記述できる
    * 探索条件を CSS セレクタで記述する
    * 条件を満たす最初の要素を返す

----

### CSS セレクタ

|記法|検索方法|使用例|
|---|----|-----|
|タグ名|タグ名で検索|document.querySelector("audio")|
|[属性名=属性値]|属性と属性値のペアで検索|document.querySelector("[src=aaa.mp3]")|
|.クラス名|class属性の値で検索|document.querySelector(".player")|
|#id|id 属性の値で検索|document.querySelector("#first-player")|

----

### document.querySelector の使用例

~~~
var h1, player, playButton;
h1 = document.querySelector("#title");
player = document.querySelector("audio");
playButton = document.querySelector("[data-role=play]")
~~~

* id 属性、タグ、属性-属性値でそれぞれ検索しています
* サンプルコードで上記を実行して、動作を確認してください

---

## 練習問題 3

* 練習問題 1, 2 のプログラムを document.querySelector を利用して書き換えよ
* document.querySelector の利用方法は次の表を参考にすること

|記法|検索方法|使用例|
|---|----|-----|
|タグ名|タグ名で検索|document.querySelector("audio")|

---

## 練習問題 4

* 音声の再生を開始する関数 play を定義せよ
* 操作対象の audio 要素は document.querySelector を利用すると簡単に探せる
* audio 要素の play メソッドを呼ぶと音声を再生できる
* 定義は js/audio.js に記述せよ

|記法|検索方法|使用例|
|---|----|-----|
|タグ名|タグ名で検索|document.querySelector("audio")|

---

## 練習問題 5

* 音声の再生を一時停止する関数 pause を定義せよ
* 操作対象の audio 要素は document.querySelector を利用すると簡単に探せる
* audio 要素の pause メソッドを呼ぶと音声を再生できる
* 定義は js/audio.js に記述せよ

|記法|検索方法|使用例|
|---|----|-----|
|タグ名|タグ名で検索|document.querySelector("audio")|

---

## ユーザ操作の処理

* 関数を明示的に呼ぶことで、処理を実行してきた
* ユーザの操作を契機に処理の実行をおこないたい
     * 操作の例：クリック、キー入力、タップ、タッチ、etc
     * 操作には様々な種類がある
* 操作をイベントとして抽象化する
* イベントを処理する関数を定義することで、ユーザ操作を処理する

----

### イベント

* 抽象化された「出来事」
* 発生源とタイムスタンプを持つオブジェクト
* [さまざまな種類](https://developer.mozilla.org/ja/docs/Web2/Reference/Events)がある

----

### イベントハンドラ

~~~
function play(event){
  var player = document.querySelector("audio");
  console.log("play");
  player.play();
}

var playButton = document.querySelector("[data-role=play]");
playButton.addEventLister("click", play);
~~~

* イベントを処理する関数のこと
* click イベントを play 関数で処理している

----

### addEventListener

~~~
function play(event){
  var player = document.querySelector("audio");
  console.log("play");
  player.play();
}

var playButton = document.querySelector("[data-role=play]");
playButton.addEventLister("click", play);
~~~

* イベントハンドラとイベントを関連づけるメソッド
* [記法](https://developer.mozilla.org/ja/docs/Web/API/EventTarget/addEventListener)

---

## 練習問題 6

* 次のようにイベントハンドラを設定せよ
    * イベント：クリック
    * 発生源：Play ボタン
    * ハンドラ：play
* js/audio.js に記述せよ
* ヒント
    * document.querySelector("[data-role=play]")
    * addEventListener

---

## 練習問題 7

* Pause ボタンが押されると、音声再生が一時停止されるようにプログラムを変更せよ
* js/audio.js に記述せよ
* ヒント
    * document.querySelector("[data-role=pause]")
    * addEventListener
    * pause 関数
