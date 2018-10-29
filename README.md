# ユーザ行動データ計測機能

## 対象OS

Android 4.1以上

## 利用準備

ご利用にはメディアシンボルの設定が必要になります。
担当者にお問い合わせください。

下記を参考にadsitr SDKを導入してください。

https://github.com/united-adstir/AdStir-Integration-Guide-Android/wiki/%E5%88%9D%E6%9C%9F%E8%A8%AD%E5%AE%9A(Android-Studio)

## ユーザ行動データ通知方法

以下の手順でadstirのサーバにユーザ行動データを送信いたします。

1. 初期化を行う
1. アプリ内のユーザ行動を計測する

### 1. 初期化を行う

`AdstirUserEvent.setMediaSymbol()`を用いて初期化を行います。

```java
AdstirUserEvent.setMediaSymbol(this, "MEDIA-xxxx");
```

### 2. アプリ内のユーザ行動を計測する

`AdstirUserEventBuilder`を用いて`AdstirUserEvent`を作成し、`AdstirUserEventTracker`でadstirのサーバへユーザ行動データを送信します。
adstirが用意しているパラメータは APIリファレンス を参照してください。
独自のパラメータを送信したい場合は、`setParameter()`を用いてkey-value形式で設定してください。valueはint、String、boolean型のみが利用できます。

```java
// イベント作成
AdstirUserEvent event =  new AdstirUserEvent.Builder()
                .setEpisode(1)  // 現在の話数をセット
                .setUserName("test-user")  // アプリ内ユーザ名をセット
                .setPaymentRate(Constants.PaymentAmount.NO) // ユーザの課金度合いをセット
                .build(Constants.Event.PAYMENT); // 課金イベント

// イベント送信
AdstirUserEventTraker.getInstance().trackEvent(this, event);
```

#### 課金時のデータを送信する例

\* 初めに初期化を行なってください

```java
// イベント作成
AdstirUserEvent event =  new AdstirUserEvent.Builder()
                .setUsername("test-user")   // アプリ内ユーザ名
                .setPaymentAmount(1000) // 1000円分課金
                .setPaymentRate(Constants.PaymentAmount.LOW) // ユーザの課金度合いをセット
                .build(Constants.Event.PAYMENT); // 課金イベント
// イベント送信
AdstirUserEventTraker.getInstance().trackEvent(this, event);
```

#### 読書開始に通知する例

```java
// イベント作成
AdstirUserEvent event =  new AdstirUserEvent.Builder()
                .setUsername("test-user")   // アプリ内ユーザ名
                .setTitle("恐怖の〇〇") // マンガのタイトル
                .setCategory("ホラー")  // マンガのカテゴリ
                .setEpisode(1)      // 1話目を読書開始
                .build(Constants.Event.START_READING); // 読書開始イベント
// イベント送信
AdstirUserEventTraker.getInstance().trackEvent(this, event);
```

* テスト時など、adstirのサーバへデータを送信したくない場合はtrackEvent()よりも前に下記を記述します。

```java
AdstirUserEvent.setTestMode(true);
```

* 端末のIDFAが取得できない場合、SDKはサーバへユーザデータの通知を行いません。

## APIリファレンス

https://united-adstir.github.io/adstir-Integration-Guide-Android-for-UserEvent/ を参照してください。