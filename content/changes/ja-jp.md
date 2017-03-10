---
タイトル：リリースAPI
AUTHOR_NAME：technoweenie
---

このAPIは、バイナリ資産による少し異なっています。要求するときに私たちは、コンテンツネゴシエーションのため、 `Accept`ヘッダーを使用します
リリース資産。 API表現を取得するための標準APIのメディアタイプを渡します。

`` `コマンドライン
$ -i -H '認可：トークン5199831f4dd3b79e7c5b7e0ebe75d67aa66e79d4」をカール\
-H「同意する：アプリケーション/ application/vnd.github.manifold-previewキー・プレビュー」
$ "https://uploads.github.com/repos/hubot/singularity/releases/assets/123」

> HTTP /1.1 200 OK

> {
> "ID"： 123,、
>   ...
> }
```

バイナリコンテンツをダウンロードするには、 "application / octet-stream」を渡します。

`` `コマンドライン
$ -i -H '認可：トークン5199831f4dd3b79e7c5b7e0ebe75d67aa66e79d4」をカール\
$ -H「同意する：application / octet-stream」を\
$ "https://uploads.github.com/repos/hubot/singularity/releases/assets/123」

HTTP /1.1 302見つけました
```

アップロードはコンパニオン「uploads.github.com」サービスへの単一の要求によって処理されます。

`` `コマンドライン
$カール-H「認可：トークンのOAuth-TOKEN」
-H「同意する：アプリケーション/ application/vnd.github.manifold-previewキー・プレビュー」
-H "のContent-Type：アプリケーション/ ZIP" \
--data-バイナリ@ビルド/ MAC / package.zip \
「https://uploads.github.com/repos/hubot/singularity/releases/123/assets?name=1.0.0-mac.zip "
```

##プレビューモード

新しいAPIは、利用可能です。これは、の方向に開発者にチャンスを与えます [preview] [preview] [provide feedback] [contact]
我々は変更を凍結する前にAPI。私たちは、30日のプレビュー状態を持ち上げることを期待しています。

同じように、我々はすぐに反復するこの機会を取りますよ。重大な変更が発表されます [the Search API] [searchapi]
任意の事前の警告なしに、この開発者のブログで。プレビュー期間が終了すると、我々はリリースAPIは不変検討します。
その時点で、それは安定した生産使用のために適しています。

プレビューメディアタイプが「アプリケーション/ vnd.github.manifoldプレビュー」です。 （http://en.wikipedia.org/wiki/Eden_Fesi）であります [Manifold]
時間と空間を介してテレポートする能力を持つアベンジャーズのメンバー、。彼は槍を保持している途中で一つです。

！ [Manifold teleporting the Avengers to a terraformed Mars surface] （https://f.cloud.github.com/assets/21/1210628/ae8556fa-25fc-11e3-986d-0ab522271d43.png）

[blawg] ：https://github.com/blog/1547-release-your-software
[api] ：http://developer.github.com/v3/repos/releases/
[preview] ：http://developer.github.com/v3/repos/releases/#preview-mode
[searchapi] ：http://developer.github.com/changes/2013-07-19-preview-the-new-search-api/
[contact] ：https://github.com/contact?form =新規+リリース+ [subject] API
