# 【iOS10対応 Objective-C】 mBaaSをWebサーバーとして使ってみよう！
_2016/11/10作成_
![画像1](/readme-img/001.png)

## 概要
* [ ニフクラ mobile backend ](https://mbaas.nifcloud.com/)の『ファイルストア機能』をWebサーバーとして利用し、保存したWebページを、アプリ内WebViewで表示するサンプルプロジェクトです。
* 簡単な操作ですぐに [ ニフクラ mobile backend ](https://mbaas.nifcloud.com/)の機能を体験いただけます★☆

##  ニフクラ mobile backend って何？？
スマートフォンアプリのバックエンド機能（プッシュ通知・データストア・会員管理・ファイルストア・SNS連携・位置情報検索・スクリプト）が**開発不要**、しかも基本**無料**(注1)で使えるクラウドサービス！

注1：詳しくは[こちら](https://mbaas.nifcloud.com/price.htm)をご覧ください

![画像2](/readme-img/002.png)

## 動作環境
* Mac OS 12.5.1 (Monterey)
* Xcode Version 14.0
* iPhone X (iOS 16)
※上記内容で動作確認をしています。

## 手順
### 1. [ ニフクラ mobile backend ](https://mbaas.nifcloud.com/)の会員登録・ログインとアプリの新規作成
* 上記リンクから会員登録（無料）をします。登録ができたらログインをすると下図のように「アプリの新規作成」画面が出るのでアプリを作成します

![画像3](/readme-img/003.png)

* アプリ作成されるとAPIキー（アプリケーションキーとクライアントキー）が発行されます
* 「OK」をクリックします
 * **参考** ：APIキーはXcodeで作成するアプリに[ ニフクラ mobile backend ](https://mbaas.nifcloud.com/)を紐付けるために使用します。アプリ内でmBaaSを使用する場合には必ず必要になるものですが、今回のサンプルアプリでは使用しません。

### 2. GitHubからサンプルプロジェクトのダウンロード
* 下記リンクをクリックしてプロジェクトをMacにダウンロードします
 * __[ObjcWebViewApp](https://github.com/NIFCloud-mbaas/ObjcWebViewApp/archive/master.zip)__

### 3. Webページの公開ファイルURLを作成する
* 2.でダウンロードしたプロジェクトに「setting」フォルダがあります
* その中にある次の３点のファイルを確認します
 * `index.html`
 * `mb.png`
 * `mb_function.png`

* この３点のファイルを[ ニフクラ mobile backend ](https://mbaas.nifcloud.com/)の「ファイルストア」にアップロードします
* ダッシュボードを開き、「ファイルストア」＞「↑アップロード」をクリックします

![画像4](/readme-img/004.png)

* ３点のファイルをドラッグ＆ドロップします
* 「アップロードする」をクリックします

![画像5](/readme-img/005.png)

* アップロードされたことを確認します
 * 順番は違っていてもOKです！
* アップロードした「`index.html`」ファイルをクリックします
* 初期状態では「公開ファイルURL」が「無効」に設定されていますので、有効に設定する必要があります
* 「アプリ設定」をクリックします

![画像6](/readme-img/006.png)

* アプリ設定が開かれますので「公開ファイル設定」の「HTTPSでの取得」を「有効」に設定し、「保存する」をクリックしてください

![画像7](/readme-img/007.png)

* 再び「ファイルストア」に戻り、「`index.html`」をクリックします
* 「公開ファイルURL」が作成されていることが確認できます

![画像8](/readme-img/008.png)

* この時点でWebページが完成しています！ぜひクリックをして、webブラウザで表示してみてください
* 次はこのURLをアプリ内WebViewとして表示してみましょう！
 * 「公開ファイルURL」は後ほど使用します

### 4. Xcodeでアプリを起動
* ダウンロードしたフォルダを開き、「`ObjcWebViewApp.xcodeproj`」をダブルクリックしてXcode開きます

![画像9](/readme-img/009.png)

### 5. 公開ファイルURLの設定
* `WebViewController.m`を編集します
* 先程[ ニフクラ mobile backend ](https://mbaas.nifcloud.com/)のダッシュボード上で確認した`index.html`ファイルの「公開ファイルURL」を貼り付けます

![画像10](/readme-img/010.png)

* `YOUR_HTML_PUBLIC_URL`の部分を書き換えます
 * このとき、ダブルクォーテーション（`"`）を消さないように注意してください！
* 書き換え終わったら`command + s`キーで保存をします

### 6. 動作確認
* Xcode画面で左上で実行ボタン（さんかくの再生マーク）をクリックします
* Simulatorが起動したら、真ん中の「INFORMATION」ボタンをクリックします
* 画面が遷移し、「公開ファイルURL」で作成したWebページが表示されます

![画像1](/readme-img/001.png)

## 解説
### 公開ファイルURLについて
公開ファイルURLは次のような構造になっています

```
https://mbaas.api.nifcloud.com/2013-09-01/applications/**APPLICATION_ID**/publicFiles/**fileName**
```

### サンプルWebページについて
今回は`index.html`に２つの画像（`mb.png`, `mb_information.png`）を表示する形式で簡易的に作成していますが、JavaScript（`js`ファイル）を作成しファイルストアに保存することで、`index.html`にスクリプトを埋め込むことも可能です。

### サンプルアプリについて
WebViewの表示は、`WebViewController.m`に記述しています

```objc
#import "WebViewController.h"

@interface WebViewController ()
// WebView
@property (weak, nonatomic) IBOutlet UIWebView *webView;

@end

// index.htmlの公開URL
NSString *const Url = @"YOUR_HTML_PUBLIC_URL";

@implementation WebViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    // スクロールを有効にする
    CGPoint point = self.webView.scrollView.contentOffset;
    point.y = 0;
    self.webView.scrollView.contentOffset = point;
    self.webView.scrollView.scrollEnabled = YES;

    // webViewに表示する
    NSURL *nsurl = [NSURL URLWithString:Url];
    NSURLRequest *request = [NSURLRequest requestWithURL:nsurl];
    [self.webView loadRequest:request];
}
```

## 参考
* 同じ内容の【Swift】版もご用意しています
 * https://github.com/NIFCloud-mbaas/SwiftWebViewApp
