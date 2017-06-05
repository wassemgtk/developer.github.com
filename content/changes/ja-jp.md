---
Title：ユーザーペイロードからGravatar IDを削除する
著者名：mastahyeti
---

[gravatar_id`属性は非推奨になりました
表現]（https://developer.github.com/v3/users/).）。 19,月19日から、
APIは常にこの属性の値として空の文字列を提供します11。

ユーザーはGitHubにアバターを直接アップロードすることができました
今]（https://github.com/blog/1803-switch-your-picture-with-ease).）。ユーザー
アバターをアップロードしていない、私たちはまだGravatarから1つを取得しようとしますが、
GitHubのサーバー上で発生します。その結果、 `gravatar_id`
属性はもはやGitHubユーザの正規のアバターを識別しない。代わりに、API
消費者は `avatar_url`を使用してユーザーのアバターを取得する必要があります。 `avatar_url`
属性は常に（/ v3/users/) / users /） に存在しています [v3 user representation]
GitHubユーザーのアバターを見つけるための唯一の信頼できる方法です。

ユーザーはGitHubにアバターを直接アップロードすることができました
今]（https://github.com/blog/1803-switch-your-picture-with-ease).）。ユーザー
アバターをアップロードしていない、私たちはまだGravatarから1つを取得しようとしますが、
GitHubのサーバー上で発生します。その結果、 `gravatar_id`
属性はもはやGitHubユーザの正規のアバターを識別しない。代わりに、API
消費者は `avatar_url`を使用してユーザーのアバターを取得する必要があります。 `avatar_url`
属性は常に（/ v3/users/) / users /）[v3 user representation]に存在しています
GitHubユーザーのアバターを見つけるための唯一の信頼できる方法です。
ユーザーはGitHubにアバターを直接アップロードすることができました
今]（https://github.com/blog/1803-switch-your-picture-with-ease).）。ユーザー
アバターをアップロードしていない、私たちはまだGravatarから1つを取得しようとしますが、
GitHubのサーバー上で発生します。その結果、 `gravatar_id`
属性はもはやGitHubユーザの正規のアバターを識別しない。代わりに、API
消費者は `avatar_url`を使用してユーザーのアバターを取得する必要があります。 `avatar_url`
属性は常に（/ v3/users/) / users /） に存在しています [v3 user representation]
GitHubユーザーのアバターを見つけるための唯一の信頼できる方法です。

ご質問やご意見がありました[get drop us a line]ら、お願いします。  [contact]

[contact] ：https://github.com/contact?form = [subject] Gravatar + IDを削除する
