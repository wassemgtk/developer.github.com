---
Title：リポジトリのライセンスの内容を取得する
著者名：ベンバター
---

（/ [License API Preview] v3 / licenses /）では、リポジトリのオープンソースライセンスの内容を取得できるようになりました。前と同様に、適切なプレビューメディアタイプが渡されると、リポジトリエンドポイントは検出されたライセンスに関する情報を返します。


ライセンス内容のエンドポイント経由でライセンスが正常に特定されたかどうかにかかわらず、リポジトリのライセンスファイルの内容を取得できるようになりました。

`` `コマンドライン
Curl -H "Accept：application / vnd.github.drax-preview + json" https://api.github.com/repos/benbalter/gman/license
```

ライセンスコンテンツメソッドは、（/ v3 [the repository contents API] / repos / contents /＃get-contents）と同様に、生のライセンスコンテンツまたはレンダリングされたライセンスHTMLを取得するための（/ v3 / [custom media types] repos / contents /＃custom-media-types）もサポートしています。

`` `コマンドライン
Curl -H "Accept：application / vnd.github.drax-preview.raw" https://api.github.com/repos/benbalter/gman/license
```

詳細については、（/ v3 / licenses [the license API documentation] /＃リポジトリの内容を取得する）ライセンスを参照してください。また、いつでも質問やフィードバックがあれば、（https://github.com/contact ？フォーム％5Bsubject％5D =ライセンス+ API）。 [get in touch with us]
