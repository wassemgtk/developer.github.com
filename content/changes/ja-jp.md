---
タイトル：問題のロックおよびロック解除APIプレビュー期間
AUTHOR_NAME：davidcelis
---
会話をロックするには、会話の問題に `PUT`要求を行います。

`` `コマンドライン
$カールhttps://api.github.com/repos/github/hubot/issues/1/lock
-X PUT \
-H「認可：トークン王研 "\ $T
>のContent-Length：5
-H「同意する：アプリケーション/ vnd.github.theキー・プレビュー」
```

会話のロックを解除するには、同様に構築 `DELETE`リクエストを行います。

`` `コマンドライン
$カールhttps://api.github.com/repos/github/hubot/issues/1/lock
DELETE -X \
-H「認可：トークン王研 "\ $T
-H「同意する：アプリケーション/ vnd.github.theキー・プレビュー」
```

####どのように私はそれを試すことができますか？

今日から、開発者は、これらの新しいAPIメソッドをプレビューすることができます。プレビュー期間中にそれらを使用するには、 `Accept`ヘッダー内の次のカスタムを提供する必要があります： [media-types] [media type]

```
application/vnd.github.the-key-preview+json
```

プレビュー期間中、当社は、開発者からのフィードバックに基づいて、これらのAPIメソッドの側面を変更することがあります。我々が行う場合、我々は開発者のブログに、ここでの変更を発表しますが、我々は、任意の事前通知を提供することはありません。

見て、あなたが質問があれば、お願いします。 [the documentation] [docs] [get in touch] [contact]

[contact]: https://github.com/contact?form%5Bsubject%5D=Issue+Locking+and+Unlocking+API+Preview
[docs] ：/ V3 /問題/＃作成-発行
[lock-an-issue]: https://help.github.com/articles/locking-conversations/
[media-types] （/ V3 /メディア/）。
[permissions]: https://help.github.com/articles/what-are-the-different-access-permissions/
