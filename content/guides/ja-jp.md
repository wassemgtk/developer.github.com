---
：/ガイド/横断-で、ページネーション
---

：/ガイド/横断-で、ページネーション

{:toc}

APIは、消費する開{{ site.data.variables.product.product_name }}発者のための情報の膨大な富を提供しています。
時間のほとんどは、あなたも、あなたが_too much_情報を要求していることを見つけるかもしれません
そして、幸せな私たちのサーバーを維持するために、APIは自動的になります。 [paginate the requested items] [pagination]

このガイドでは、検索APIにいくつかの呼び出しを行う、および反復処理します {{ site.data.variables.product.product_name }}
ページネーションを使用した結果。あなたはこのプロジェクトのための完全なソースコードを見つけることができます
リポジトリインチ [platform-samples] [platform samples]

ページ区切りの##の基本

で開始するには、ページ番号付きのアイテムを受信に関するいくつかの事実を知っておくことが重要です：

1.さまざまなAPI呼び出しが異なるデフォルト値で応答します。たとえば、コールへ
[list GitHub's public repositories](https://developer.github.com/v3/repos/#list-all-public-repositories)
GitHubの検索APIの呼び出しに対し、30のセットでページ番号付きのアイテムを提供
100のセット内のアイテムを提供
2.あなたは（100の最大値まで）を受信する項目の数も指定できます。だけど、
3.技術的な理由ではなく、すべてのエンドポイントは、同じ動作をします。例えば、
[events] （https://developer.github.com/v3/activity/events/）を使用すると、受信する項目の最大値を設定できません。
特定のエンドポイント用のページ番号付きの結果を処理する方法についてのドキュメントを必ずお読みください。


`` `コマンドライン
$カール-I "/検索/コードのq = addClass {{ site.data.variables.product.api_url_pre }} +ユーザー：？モジラ」
```

`-I`パラメータは、実際のない、私たちはヘッダーだけを気にすることを示しています
コンテンツ。結果を調べるには、リンクヘッダ内のいくつかの情報をわかります
それは次のようになります。

リンク：; <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=2> rel = "次"、
<https://api.github.com/search/code?q=addClass+user%3Amozilla&page=34> ; rel = "最後"

それを打破してみましょう。 `RELは="次 " '2' =次のページは`ページであることを述べています。これは作ります
感覚、デフォルトであるため、すべてのページ分割されたクエリが1.` `ページで開始し`のrel = "最後" `
結果の最後のページは、ページ `34`であることを示す、いくつかのより多くの情報を提供しています。
したがって、我々は我々が消費することができます `addClass`に関する情報の33以上のページを持っています。
ナイス！

あなたは**常に**提供されるこれらのリンク関係に依存している必要があることを覚えておいてください
あなたへ。推測したり、独自のURLを構築しようとしないでください。 [リストのようないくつかのAPI呼び出し、
リポジトリにコミット]は、ベースとされているページ付けの結果を使用します [listing commits]
SHA値ではなく、数字上。

###ページのナビゲート

今、あなたが受け取ることがありますどのように多くのページを知っていることを、あなたがナビゲートを開始することができます
ページを結果を消費します。あなたは `page`に渡すことによってこれを行います
パラメータ。デフォルトでは、 `page`は常に '1'で始まります。 14ページに先にジャンプしてみましょう
そして、何が起こるかを参照してください。

`` `コマンドライン
$カール-I "https://api.github.com/search/code?q=addClass+user:mozilla&page=14」
```

ここでもう一度リンクヘッダは次のとおりです。

リンク：; <https://api.github.com/search/code?q=addClass+user%3Amozilla&page=15> rel = "次"、
<https://api.github.com/search/code?q=addClass+user%3Amozilla&page=34> ; rel = "最後"
<https://api.github.com/search/code?q=addClass+user%3Amozilla&page=1> ; rel = "最初"、
<https://api.github.com/search/code?q=addClass+user%3Amozilla&page=13> ; RELは= "前へ"

予想されたように、 `RELは="次 "` 15,であり、かつ `REL ="最後は "`まだ34である。しかし、今、我々はしました
`のrel ="最初は "`、_first_ページのURLを示していますいくつかのより多くの情報を得ました
そしてもっと重要なのは、 `のrel =" PREV "は`前のページ番号を知ることができます
ページ。この情報を使用して、ユーザーがジャンプすることができますいくつかのUIを構築することができ
API呼び出しの結果の、最初の前の、次の、または最後のリストの間で。

###受信したアイテムの数を変更します

`per_page`パラメータを渡すことで、あなたがしたい項目の数も指定できます
各ページには100項目まで、戻ります。それでは、addClass` `約50項目を尋ねてみましょう。

`` `コマンドライン
$カール-I "https://api.github.com/search/code?q=addClass+user:mozilla&per_page=50」
```

それはヘッダー応答に何をするかに注目してください：

リンク：; <https://api.github.com/search/code?q=addClass+user%3Amozilla&per_page=50&page=2> rel = "次"、
<https://api.github.com/search/code?q=addClass+user%3Amozilla&per_page=50&page=20> ; rel = "最後"

ご想像のとおり、 `のrel ="最後 "`情報は、最後のページと言っています
我々は、約ページあたりのより多くの情報を求めているので、これは今20です。
我々の結果。

##情報を消費

あなただけで動作することができるように、低レベルのカール呼び出しを行うことにしたくありません
ページネーションなので、我々はきたすべてを行い少しRubyスクリプトを書いてみましょう
ただ、上記の。

いつものように、最初に我々は、Rubyライブラリを必要とします [GitHub's Octokit.rb] [octokit.rb]
私たちに渡します。 [personal access token] [personal token]

`` `ルビー
「octokit 'を必要と

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
@client || = Octokit :: Octokit::Client.new（：access_tokenは=> ['MY_PERSONAL_TOKEN'] access_tokenは）
```

次に、我々はOctokitの `search_code`メソッドを使用して、検索を実行します。異なり、
`curl`を使用して、我々はまた、すぐに結果の数を取得し、そのようにしてみましょうことができます
それを行う：

`` `ルビー
結果= client.search_code（ 'addClassユーザー：モジラ'）
TOTAL_COUNT = results.total_count
```

さて、>ページ= 34 'に似て、最後のページの数を、つかむしましょう。 `のrel ="最後 "
リンクヘッダ内の情報。 Octokit.rbサポートページネーション情報
実装は、いわゆる "。" [Hypermedia link relations] [hypermedia-relations]
私たちは、それが何であるかについての詳細に入るが、言えば十分、各要素はありません
`results`変数の情報を含めることができます` rels`と呼ばれるハッシュを、持っています
`について：next`、`：last`、 `：first`、と`：これに応じてprev`は、あなたをもたらしています
に。これらの関係も呼び出すことで、結果のURLに関する情報が含まれています
`親戚が.href`。 [:last]

これを知って、それでは、最後の結果のページ番号を取得しましょう、と存在するすべての
ユーザーにこの情報：

`` `ルビー
Last_response = client.last_response
NUMBER_OF_PAGES = last_response.rels [:last] .href.match（/ページ=（\ [1] dは+）。* $ /）

「＃ページで、＃結果があります！ "置きます {total_count} {number_of_pages}
```

最後に、結果を反復処理しましょう。あなたは1..number_of_pages.to_i`でiに対するループ `でこれを行うことができ、
その代わりに、のから情報を取得するために `親戚`ヘッダに従ってみましょう [:next]
各ページ。簡単のために、ちょうど最初のファイルパスを取得しましょう
各ページから生じます。これを行うには、ループが必要になります。そしてすべてのループの終わりに、
我々は `親戚`情報を追跡することによって、次のページのデータ・セットを取得します。 [:next]
他に（消費する全く `親戚`情報がない場合、ループは終了します [:next]
言葉は、我々は `のrels`）です。それは次のようになります。 [:last]

`` `ルビー
last_response.data.items.first.pathを置きます
last_response.rels .nilまで？ [:next]
Last_response = last_response.rels [:next] .get
last_response.data.items.first.pathを置きます
終わり
```

ページごとの項目数を変更すると、Octokit.rbと非常に簡単です。単に
最初のクライアントの構成に `per_page`オプションのハッシュを渡します。その後、
あなたのコードは、無傷のままであるべきです：

`` `ルビー
「octokit 'を必要と

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
@client || = Octokit :: Octokit::Client.new（：access_tokenは=> ['MY_PERSONAL_TOKEN'] access_tokenは）

結果= client.search_code（ 'addClassユーザー：モジラ'、：PER_PAGE => 100)）
TOTAL_COUNT = results.total_count

Last_response = client.last_response
NUMBER_OF_PAGES = last_response.rels [:last] .href.match（/ページ=（\ [1] dは+）。* $ /）

Last_response.relsを置きます.HREF [:last]
「＃ページで、＃結果があります！ "置きます {total_count} {number_of_pages}

置く "そして、ここですべてのセットの最初のパスです」

last_response.data.items.first.pathを置きます
last_response.rels .nilまで？ [:next]
Last_response = last_response.rels [:next] .get
last_response.data.items.first.pathを置きます
終わり
```

##ページネーションリンクを構築

通常、ページネーションで、あなたの目標は、可能なすべてを連結することではありません
結果は、むしろ、このようなナビゲーションのセットを生成します。

![Sample of pagination links](/assets/images/pagination_sample.png)

のは、それが伴うかもしれないもののマイクロバージョンをスケッチしてみましょう。

上記のコードから、我々はすでに我々が `number_of_pages`を得ることができます知っています
最初の呼び出しからの結果のページ区切り：

`` `ルビー
「octokit 'を必要と

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
@client || = Octokit :: Octokit::Client.new（：access_tokenは=> ['MY_PERSONAL_TOKEN'] access_tokenは）

結果= client.search_code（ 'addClassユーザー：モジラ'）
TOTAL_COUNT = results.total_count

Last_response = client.last_response
NUMBER_OF_PAGES = last_response.rels [:last] .href.match（/ページ=（\ [1] dは+）。* $ /）

Last_response.relsを置きます.HREF [:last]
「＃ページで、＃結果があります！ "置きます {total_count} {number_of_pages}
```

そこから、私たちはナンバーボックスの美しいASCII表現を構築することができます。
`` `ルビー
番号= ""
私1..number_of_pages.to_iで用
番号= "" [#{i}]
終わり
数字を置きます
```

ランダムを構築することにより、のは、これらのボックスのうちの1つをクリックするユーザーをシミュレートしてみましょう
数：

`` `ルビー
Random_page = Random.new
Random_page = random_page.rand（1..number_of_pages.to_i）

「ユーザーが登場し、番号＃をクリック！ "置きます {random_page}
```

今、私たちは、ページ番号を持っていることを、我々は明示的にそれを取得するためにOctokitを使用することができます
Page`オプション：、 `渡すことによって、個々のページ：

`` `ルビー
Clicked_results = client.search_code（ 'addClassユーザー：モジラ'、：ページ=> random_page）
```

私たちは空想取得したい場合、我々はまたに、以前のページと次のページをつかむことができ
バック（ `>`）要素のためのリンクを生成するために： <<`) and foward (`>

`` `ルビー
Prev_page_href = client.last_response.rels？ [:prev] client.last_response.relsは.HREFない：「（なし）」 [:prev]
Next_page_href = client.last_response.rels？ [:next] client.last_response.relsは.HREFない：「（なし）」 [:next]

「前のページリンクは＃ "置きます {prev_page_href}
「次のページリンクは＃ "置きます {next_page_href}
```

[pagination] ：/ V3 /＃ページネーション
[platform samples]: https://github.com/github/platform-samples/tree/master/api/ruby/traversing-with-pagination
[octokit.rb]: https://github.com/octokit/octokit.rb
[personal token]: https://help.github.com/articles/creating-an-access-token-for-command-line-use
[hypermedia-relations]: https://github.com/octokit/octokit.rb#pagination
[listing commits]: https://developer.github.com/v3/repos/commits/#list-commits-on-a-repository
