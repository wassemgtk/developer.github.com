---
タイトル：コメントでの作業
---


{：TOC} __ v__

任意のプル要求については、コメント・ビューの3種類が用意されています。 {{ site.data.variables.product.product_name }}
[comments on the Pull Request][PR comment]プル要求の中で、全体として、 [comments on a specific line] [PR line comment]
そして [comments on a specific commit][commit comment]、プル要求内。

コメントのこれらの各タイプ は、APIの異なる部分を通過します。 {{ site.data.variables.product.product_name }}
このガイドでは、我々はあなたがそれぞれ1にアクセスして操作する方法を見ていきます。すべてのための
例えば、私たちは「octocat」に使うことになるでしょう [this sample Pull Request made] [sample PR]
リポジトリ。いつものように、サンプルはで見つけることができます。 [our platform-samples repository] [platform-samples]

##プルリクエストコメント

プルリクエストのコメントにアクセスするには、を介して行きますよ。 [the Issues API] [issues]
これは、最初は直感に反するように見えるかもしれません。しかし、あなたはそのプルを理解すれば
要求は、それが問題のAPIを使用することは理にかなって、コードでだけの問題であり、
##リストは、プル要求にコミット

我々は、使用して、Rubyスクリプトを作成することによって、プルリクエストのコメントを取り出す説明します
[Octokit.rb][octokit.rb]。また、作成したいと思うでしょう。 [personal access token] [personal token]

次のコードは、プルリクエストからのコメントへのアクセス始めるのに役立つ必要があります
Octokit.rb使用しました：

`` `ルビー
「octokit 'を必要と

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
@client || = Octokit :: Octokit::Client.new（：access_tokenは=> access_tokenは） ['MY_PERSONAL_TOKEN']

行う.each client.issue_comments（「octocat /スプーン・ナイフ」、, 1176).each年）|コメント|
ユーザ名= comment[:user][:login]
post_date = comment[:created_at]
コンテンツ= comment[:body]

プット "＃の{username}は＃の{post_date}にコメントをしたことは言う：。\n '＃の{content}'\n"
終わり
```

ここでは、具体的には、コメント（ `issue_comments`）を取得するための問題APIに出て呼んでいます
リポジトリの名前（ `octocat /スプーン-Knife`）、およびプルリクエストIDの両方を提供
我々は（ `1176`）に興味を持っています。その後、それは単にを反復処理の問題です
コメントは各1についての情報を取得します。

##リストは、プル要求にコミット

差分ビュー内では、単数形の特定の側面についての議論を開始することができます
プルリクエスト内で行われた変更。これらのコメントは、個々の行で発生します
変更されたファイル内。この議論のためのエンドポイントURLがから来ています。 [the Pull Request Review API] [PR Review API]

次のコードは、単一のプルリクエスト番号与えられたファイルに行われたすべてのプル要求のコメントを、フェッチ：

`` `ルビー
「octokit 'を必要と

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
@client || = Octokit :: Octokit::Client.new（：access_tokenは=> access_tokenは） ['MY_PERSONAL_TOKEN']

行う.each client.issue_comments（「octocat /スプーン・ナイフ」、, 1176).each年）|コメント|
ユーザ名= comment[:user][:login]
post_date = comment[:created_at]
コンテンツ= comment[:body]
パス= comment[:path]
位置= comment[:position]

プット "＃の{username}は、ライン＃1 {position}に、＃1 {path}と呼ばれるファイルのための＃1 {post_date}にコメントをしたことは言う：。\n '＃の{content}'\n"
終わり
```

あなたはそれが上記の例と非常に似ていることに気づくでしょう。違い
このビューとプルリクエストのコメントの間の会話の焦点です。
プルリクエストで行われたコメントは、議論やアイデアにするために予約する必要があります
コードの全体的な方向性。プル要求の見直しの一環として作られたコメントすべきです
特定の変更がファイル内に実装された方法と特異的に対処します。

##コメントをコミット

コメントの最後のタイプは、個々のコミットに特異的に発生します。このために、
彼らが利用しています。 [the commit comment API] [commit comment API]

コミットのコメントを取得するには、コミットのSHA1を使用したいと思います。
言い換えれば、あなたは、プル要求に関連する任意の識別子を使用しません。次に例を示します。

`` `ルビー
「octokit 'を必要と

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
@client || = Octokit :: Octokit::Client.new（：access_tokenは=> access_tokenは） ['MY_PERSONAL_TOKEN']

Client.commit_comments（「octocat /スプーン・ナイフ "、" cbc28e7c8caee26febc8c013b0adfb97a4edd96e」）それぞれん|。コメント|
ユーザ名= comment[:user][:login]
post_date = comment[:created_at]
コンテンツ= comment[:body]

プット "＃の{username}は＃の{post_date}にコメントをしたことは言う：。\n '＃の{content}'\n"
終わり
```

このAPIコールは、単一の行のコメントを取得することに注意してください、と同様のコメント
全体にコミットします。

[PR comment]：https://github.com/octocat/Spoon-Knife/pull/1176#issuecomment-24114792
[PR line comment]：https://github.com/octocat/Spoon-Knife/pull/1176#discussion_r6252889
[commit comment]：https://github.com/octocat/Spoon-Knife/commit/cbc28e7c8caee26febc8c013b0adfb97a4edd96e#commitcomment-4049848
[sample PR]：https://github.com/octocat/Spoon-Knife/pull/1176
[platform-samples]：https://github.com/github/platform-samples/tree/master/api/ruby/working-with-comments
[issues]：https://developer.github.com/v3/issues/comments/
[personal token]：https://help.github.com/articles/creating-an-access-token-for-command-line-use
[octokit.rb]：https://github.com/octokit/octokit.rb
[PR Review API]：https://developer.github.com/v3/pulls/comments/
[commit comment API]：https://developer.github.com/v3/repos/comments/#get-a-single-commit-comment
