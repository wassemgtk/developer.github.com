---
Title：通知API
AUTHOR_NAME：technoweenie
---

今、ほこりが落ち着いたので、 [Notifications and Stars] [newsies]
私たちはそれをすべて発揮しました。あなたは今できる！ [brand new API] [api]
表示し、通知を読み取りとしてマークします。

[api] ！ （http://developer.github.com/v3/activity/notifications/）
[newsies] ！ （https://github.com/blog/1204-notifications-stars）

＃＃ 終点

コア通知機能は `/ notifications`エンドポイントの下にあります。
未読の通知を探すことができます：

`` `コマンドライン
$カールhttps://api.github.com/notifications
```

これらの通知を1つのリポジトリにフィルタリングできます。

`` `コマンドライン
$カールhttps://api.github.com/repos/technoweenie/faraday/notifications
```

それらを読んだものとしてマークすることができます：

`` `コマンドライン
＃すべての通知
$カールhttps://api.github.com/notifications
$ -X PUT -d '' {"read": true}

1つのリポジトリの＃通知
$カールhttps://api.github.com/repos/technoweenie/faraday/notifications
$ -X PUT -d '' {"read": true}
```

また、リポジトリまたは単一のスレッドのサブスクリプションを変更することもできます。

`` `コマンドライン
スレッドの＃購読詳細（IssueまたはCommitのいずれか）
$ curl https://api.github.com/notifications/threads/1/subscription

リポジトリ全体の＃購読詳細。
$カールhttps://api.github.com/repos/technoweenie/faraday/subscription
```

##ポーリング

通知APIは、最終変更時刻までのポーリングに最適化されています。

`` `コマンドライン
＃リクエストに認証を追加する
$カール-I "https://api.github.com/notifications」
> HTTP /1.1 200 OK
>最終変更日：2012年10月25日15:16:27 GMT
> X-Poll-Interval：60

＃Last-Modifiedヘッダーを正確に渡す
$カール-I "https://api.github.com/notifications」
$ -H "変更された場合：Thu、2012年10月25日15:16:27 GMT"
> HTTP /1.1 304は変更されません
> X-Poll-Interval：60
```

APIの詳細については、をご覧ください。 [Notifications documentation] [api]
