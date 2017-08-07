---
タイトル：API出演のための新しい属性
AUTHOR_NAME：arfon
---

ユーザーがリポジトリを主演するときは、今見ることができます。 `starred_at`フィールドを含む新しい応答フォーマットを受信するには、新しいメディアタイプを要求します。

`` `コマンドライン
Curl -H "Accept：application / application/vnd.github.v3.star+json" https://api.github.com/users/andrew/starred
```

主演リポジトリがレポフィールドで使用できるようになりました注意してください。

###フィードバック

あなたはこれらの変更についてのご質問やご意見がありましたら、お願いします。 [drop us a line] [contact]

[starring] ：/ V3 /活動/主演/＃リスト・リポジトリビーイング・主演-で星創出のタイムスタンプ
