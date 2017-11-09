---
タイトル：認証の基礎知識
---

Authenticaの＃の基本

{：TOC} __ v__

このセクションでは、認証の基礎に集中するつもりです。具体的には、
実装することを我々は（[シナトラ] __ __ V [シナトラ] __ v__を使用して）Rubyのサーバーを作成しようとしています
[ウェブフロー] __ __ V [Webflowの] __いくつかの異なる方法でアプリケーションのv__！

{{#tip}} __ v__

あなたは（https://github.com/github/platform-samples/tree/master/api/）[プラットフォームのサンプルのレポから]このプロジェクトのための完全なソースコードをダウンロードすることができます。

{{/チップ}} __ v__

##あなたのアプリを登録します

まず、あなたはV __ [新しいOAuthのアプリ] __ __ v__ [アプリケーションを登録]する必要があります。すべての
登録のOAuthアプリケーションは、一意のクライアントIDとクライアントシークレットが割り当てられています。
クライアントの秘密を共有するべきではありません！それは文字列をチェックも含まれ
リポジトリへ。

**認可コールバックURL **。これは、簡単に設定することに最も重要な部分であり、
アプリケーションのアップ。{{ site.data.variables.product.product_name }}これは、後にユーザーを返すコールバックURLです
認証が成功しました。

我々は、通常のシナトラサーバ、ローカルインスタンスの場所を実行しているので、
HTTP `：// localhostに設定されて：4567`。 // localhostを：4567 / callback`のは `としてhttpコールバックURLを記入してみましょう。

##ユーザー認証を受け入れます

それでは、私たちの単純なサーバーを記入始めましょう。 _server.rb_というファイルを作成し、それにこれを貼り付けます。

`` `ルビー
「シナトラ」を必要とします
「残り-client 'を必要とします
「JSON」を必要とします

CLIENT_ID = ENV [ 'GH_BASIC_CLIENT_ID'] __ v__
CLIENT_SECRET = ENV [ 'GH_BASIC_SECRET_ID'] __ v__

'/'入手できますか
ERB：インデックス：地元=> {：CLIENT_ID => CLIENT_ID} __ v__
終わり
```

あなたのクライアントIDとクライアント秘密鍵は、[アプリケーションの設定から来ます
ページ]。あなたは**、_ever [app settings] _ **でこれらの値を保存しないでください
{{ site.data.variables.product.product_name }}そのことについては、他の公共の場所を、-OR。私たちは、それらを格納するお勧めします
[環境変数] __ __ V [ENVについてvarsの] __ V __--私たちはここに何をやったかを正確にしています。

次に、_views / index.erb_に、このコンテンツを貼り付けます。

`` `ERB
<html> __v__
<head> __v__
</head> __v__
<body> __v__
<p> __v__
まあ、こんにちは！
</p> __v__
<p> __v__
現在のGitHub APIに話をするつもりです。準備はできましたか？
<a href="https://github.com/login/oauth/authorize?scope=user:email&client_id=<%= client_id %>__v__">こちらをクリックして</a>開始すること！ __v__
</p> __v__
<p> __v__
そのリンクが動作しない場合は、ご自身を提供することを忘れないでください<a href="/v3/oauth/#web-application-flow">Client IDの</a> __v__を！
</p> __v__
</body> __v__
</html> __v__
```

（あなたはシナトラの仕組みに慣れていないなら、私たちは、__ __ V [シナトラガイド] __ v__ [シナトラのガイドを読む]をお勧めします。）

また、URLを定義するために `scope`クエリパラメータを使用していることに気付きます
[スコープ] __ __ V [OAuthのスコープ] __ v__は、アプリケーションによって要求されました。私たちのアプリケーションのために、私たちはしています
プライベートのメールアドレスを読み取るためのemail`範囲： `ユーザに要求します。

HTTP `にブラウザをナビゲート：// localhostを：4567`。リンクをクリックした後、あなた
取り、次のようになりますダイアログが表示されるはずです： {{ site.data.variables.product.product_name }}
！[プロンプトのGitHubのOAuth] __ __ V（/資産/画像/ oauth_prompt.png）

あなた自身を信頼する場合、**承認アプリ**をクリックします。 WUH-ああ！シナトラが吐き出します
`404`エラー。何ができます！

我々は `callback`れるコールバックURLを指定していた場合はまあ、覚えていますか？我々は提供していませんでした
彼らが承認した後、ユーザーを削除するにはどこのルートは、そのように認識していません {{ site.data.variables.product.product_name }}
アプリ。それでは、その問題を修正しましょう​​！

###コールバックを提供

_server.rb_では、コールバックが何をすべきかを指定するためにルートを追加します。

`` `ルビー
'/コールバック」入手できますか
＃一時GitHubのコードを取得...
Session_code = request.env [ 'rack.request.query_hash'] __ __ V [ 'コード'] __ v__

＃...とバックのGitHubに投稿
結果= RestClient.post（ 'https://github.com/login/oauth/access_token」、
{：CLIENT_ID => CLIENT_ID、
：client_secret => CLIENT_SECRET、
：コード=> session_code}、
：受け入れる=>：JSON}））

＃トークンと付与されたスコープを抽出
Access_tokenは= JSON.parse（結果）[ 'access_tokenは'] __ v__
終わり
```

成功したアプリの認証後、一時的な `code`値を提供します。 {{ site.data.variables.product.product_name }}
あなたは `access_token`と引き換えに戻ってPOST``にこのコードが必要になります。 {{ site.data.variables.product.product_name }}
私たちのGETとPOST HTTPリクエストを簡素化するために、我々は、[残り-クライアント]を使用している__ __ V [RESTクライアント] __ v__。
あなたはおそらくRESTを通じてAPIにアクセスすることは決してないだろうことに注意してください。より深刻なのための
アプリケーションは、おそらく[選択した言語で書かれたライブラリ] __ __ V [ライブラリ] __ v__を使用する必要があります。

###付与されたスコープを確認します

将来的には、ユーザーができるようになりますV __ [編集スコープポスト] __ [あなたが要求したスコープ編集]を__ v__、
そして、あなたのアプリケーションでは、もともとのために尋ねたよりも少ないアクセスを許可されることがあります。
だから、トークンを持つすべての要求を行う前に、あなたはそのスコープを確認してください
ユーザによってトークンを付与されました。

付与されたスコープはからの応答の一部として返されます
トークンを交換します。

`` `ルビー
'/コールバック」入手できますか
# ...
＃上記のコードサンプルを使用してaccess_tokenはを取得
# ...

＃我々が付与されたかどうかを確認し、ユーザー：電子メールのスコープ
スコープ= JSON.parse（結果）[ 'スコープ'] __ V __。スプリット（ ''）
Has_user_email_scope = scopes.include？ 「利用者：メール '
終わり
```

このアプリケーションでは、我々は `scopes.includeを使っている？`我々が付与されたかどうかをチェックします
`ユーザ：認証されたユーザーの秘密を取得するために必要なemail`スコープ
メールアドレス。アプリケーションが他のスコープのために求めていた、我々が持っているだろう
同様にそれらをチェック。

スコープ間の階層関係がありますので、また、あなたがすべき
あなたが必要なスコープの最低レベルを付与されたことを確認してください。例えば、
アプリケーションは `user`範囲を求めていた場合、それだけで付与されている可能性があります
`ユーザ：email`スコープ。その場合、アプリケーションが許可されていません
何それはを求めたが、付与されたスコープは、まだ十分なされていると思います。

それが可能だからのみ要求を行う前に、スコープの確認は十分ではありません
ユーザーがあなたのチェックと実際のリクエストの間にスコープを変更すること。
発生した場合、APIは `404`で失敗する可能性がありますあなたが成功することが期待呼び出し
または `401`状態、または情報の異なるサブセットを返します。

あなたは優雅にこれらの状況、要求のためのすべてのAPI応答を処理支援するために、
有効なトークンで作られ、またV __ [OAuthのスコープ] __ v__ __ [ `X-OAuthの-Scopes`ヘッダー]を含みます。
このヘッダは、作成するために使用されたトークンのスコープのリストが含まれています
要求。それに加えて、許可APIは、エンドポイントへの提供します
__ __ __ V v__ [トークンの有効性をご確認ください] [妥当性のためのトークンをチェック]。
トークンのスコープの変化を検出するために、この情報を使用して、あなたのユーザーに通知
利用可能なアプリケーション機能の変​​化。

###認証要求を行います

最後に、このアクセ​​ストークンを使用して、あなたは認証された要求などをすることができます
ログインしているユーザー：

`` `ルビー
＃ユーザ情報を取得します
Auth_result = JSON.parse（RestClient.get（ 'https://api.github.com/user」、
{：paramsは=> {：access_tokenは=> access_tokenは} __ v__}））

ユーザーがそれを許可した場合＃、プライベートメールをフェッチ
Has_user_email_scope場合
Auth_result [ 'private_emails'] __ v__ =
JSON.parse（RestClient.get（ 'https://api.github.com/user/emails」、
{：paramsは=> {：access_tokenは=> access_tokenは} __ v__}））
終わり

ERB：基本的な、：地元の人=> auth_result
```

我々は我々の結果とやりたいことができます。この場合、私たちはまっすぐ_basic.erb_にそれらをダンプします：

`` `ERB
<p> __v__Hello、<％=ログイン％>！ </p> __v__
<p> __v__
<％！のemail.nil場合は？ &&！email.empty？あなたの公開メールアドレスは<％=電子メール％>であるように％>に見えます。
あなたが公共の電子メールを持っていないように見えます。カッコいい。
終わり
</p> __v__
<p> __v__
<％定義されている場合は？ private_emails％>
あなたの許可を得て、我々はまた、あなたのプライベートのメールアドレスを掘ることができました：
<％= private_emails.map {| Private_email_address | Private_email_address [ "電子メール"]} __ __ V。（ ''）に参加％>
ほかに
また、あなたはあなたのプライベートのメールアドレスについて少し秘密です。
終わり
</p> __v__
```

##「永続」の認証を実装します

我々はアプリにすべての単一のログを記録するようにユーザーに必要であればそれはかなり悪いモデルになるだろう
それらがウェブページにアクセスするのに必要な時間。たとえば、に直接ナビゲートしてみてください
`のhttp：// localhostを：4567 / basic`。あなたはエラーになります。

我々は全体を回避することができればどのような
プロセスを「ここをクリック」し、ちょうど限り、ユーザのがログインしたように、ということ_remember_
{{ site.data.variables.product.product_name }}、彼らはこのアプリケーションにアクセスできるようにすべきですか？あなたの帽子を保持、
_thatの我々はDO_しようとしている正確に何のため。

上記の私たちの小さなサーバーはかなり単純です。いくつかのインテリジェントに押し込むために、
認証は、私たちはトークンを格納するためのセッションを使用してに切り替えるつもりです。
これは、ユーザへの認証を透明にします。

私たちはセッション内でスコープを持続しているので、また、私たちのようにする必要があります
ユーザーは、我々は彼らをチェックした後にスコープを更新、または取り消したときにケースを扱います
トークン。そのためには、 `rescue`ブロックを使用し、最初のAPIことを確認します
コー​​ルは、トークンがまだ有効であることを確認する、成功しました。その後、我々はよ
ユーザーが失効していないことを確認するために `X-OAuthの-Scopes`レスポンスヘッダをチェック
`ユーザ：email`スコープ。

_advanced_server.rb_というファイルを作成し、その中に次の行を貼り付けます。

`` `ルビー
「シナトラ」を必要とします
「rest_client 'を必要と
「JSON」を必要とします

＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
＃もしENV [ 'GITHUB_CLIENT_ID'] __ v__ && ENV [ 'GITHUB_CLIENT_SECRET'] __ v__
＃CLIENT_ID = ENV [ 'GITHUB_CLIENT_ID'] __ v__
＃1 CLIENT_SECRET = ENV [ 'GITHUB_CLIENT_SECRET'] __ v__
終わり

CLIENT_ID = ENV [ 'GH_BASIC_CLIENT_ID'] __ v__
CLIENT_SECRET = ENV [ 'GH_BASIC_SECRET_ID'] __ v__

Cookie_only =>偽：ラック::セッション::プールは、使用

デフ認証？
セッション[：access_tokenは] __ v__
終わり

デフ認証！
ERB：インデックス：地元=> {：CLIENT_ID => CLIENT_ID} __ v__
終わり

'/'入手できますか
場合！認証？
認証する！
ほかに
Access_tokenは=セッション[：access_tokenは] __ v__
スコープ= [] __ v__

ベギン
Auth_result = RestClient.get（https://api.github.com/user（ 'https://api.github.com/user」、
{：paramsは=> {：access_tokenは=> access_tokenは} __ v__、
：受け入れる=>：JSON}））
救助=>電子
トークンは私たちを取り消されたため、＃要求は成功しませんでした
＃セッションに格納されたトークンが無効になり、レンダリング
＃インデックスページユーザーが再びOAuthのフローを開始できるように、

セッション[：access_tokenは] __ v__ = nilを
認証返します！
終わり

＃リクエストが成功したので、私たちは現在のスコープのリストをチェック
Auth_result.headers.include場合は？ ：x_oauth_scopes
スコープ= auth_result.headers [：x_oauth_scopes]。__ __ Vスプリット（ ''）
終わり

Auth_result = JSON.parse（auth_result）

Scopes.include場合はどうなりますか？ 「利用者：メール '
Auth_result [ 'private_emails'] __ v__ =
JSON.parse（RestClient.get（ 'https://api.github.com/user/emails」、
{：paramsは=> {：access_tokenは=> access_tokenは} __ v__、
：受け入れる=>：JSON}））
終わり

ERB：高度、：地元の人=> auth_result
終わり
終わり

'/コールバック」入手できますか
Session_code = request.env [ 'rack.request.query_hash'] __ __ V [ 'コード'] __ v__

結果= RestClient.post（ 'https://github.com/login/oauth/access_token」、
{：CLIENT_ID => CLIENT_ID、
：client_secret => CLIENT_SECRET、
：コード=> session_code}、
：受け入れる=>：JSON}））

セッション[：access_tokenは] __ v__ = JSON.parse（結果）[ 'access_tokenは'] __ v__

リダイレクト '/'
終わり
```

コー​​ドの大部分はおなじみのはずです。例えば、我々はまだ `RestClient.get`を使用しています
APIを呼び出すために、我々はまだ我々の結果を渡しているレンダリングされます {{ site.data.variables.product.product_name }}
ERBテンプレートで（この時、それは `advanced.erb`と呼ばれています）。

また、私たちは今、ユーザーが既にあるかどうかを確認する `認証？`メソッドを持っています
認証されました。ない場合は、 `認証！`メソッドを実行する、と呼ばれています
OAuthが流れ、付与されたトークンとスコープとのセッションを更新します。

次に、_views_と呼ばれる_advanced.erb_にファイルを作成し、その中にこのマークアップを貼り付けます。

`` `ERB
<html> __v__
<head> __v__
</head> __v__
<body> __v__
<p> __v__Well、井戸、井戸、<％=ログイン％>！ </p> __v__
<p> __v__
<％！のemail.empty場合は？あなたの公開メールアドレスは<％=電子メール％>であるように％>に見えます。
あなたが公共の電子メールを持っていないように見えます。カッコいい。
終わり
</p> __v__
<p> __v__
<％定義されている場合は？ private_emails％>
あなたの許可を得て、我々はまた、あなたのプライベートのメールアドレスを掘ることができました：
<％= private_emails.map {| Private_email_address | Private_email_address [ "電子メール"]} __ __ V。（ ''）に参加％>
ほかに
また、あなたはあなたのプライベートのメールアドレスについて少し秘密です。
終わり
</p> __v__
</body> __v__
</html> __v__
```

コマンドラインから、あなたを起動 `ルビーadvanced_server.rb`、呼び出し
ポート `4567`上のサーバ - 私たちは、単純なシナトラアプリを持っていたとき、我々が使用したのと同じポート。
あなたはHTTP `に移動すると：// localhostを：4567`、アプリが呼び出し`認証 `！
これは `/ callback`にリダイレクトされます。 `/ callback`は、その後、` / `に私たちを送り返し、
私たちが認証されてきたので、_advanced.erb_をレンダリングします。

我々は完全に、単に私たちのコールバックを変更することで、この往復のルーティングを簡素化することができ
`/`に内のURL。しかし、_server.rb_と_advanced.rb_両方以降に依存しています {{ site.data.variables.product.product_name }}
同じコールバックURLは、我々はそれを動作させるためにwonkinessの少しを行うようになってきました。

また、我々は我々のデータにアクセスするには、このアプリケーションを承認したことがなかった場合には、 {{ site.data.variables.product.product_name }}
我々は以前のポップアップから同じ確認ダイアログを見て、私たちに警告しただろう。

あなたが希望する場合は、[さらに別のシナトラ、GitHubの認証例] __ __ V [シナトラAUTH githubのテスト] __ v__で遊ぶことができます
別のプロジェクトとして利用可能。

[Webflowの] __ v__：/ V3 / OAuthの/＃Webアプリケーションフロー
[シナトラ] __ v__：http://www.sinatrarb.com/
[ENVのVARSについて] __ v__：http://en.wikipedia.org/wiki/Environment_variable#Getting_and_setting_environment_variables
[シナトラガイド] __ v__：https://github.com/sinatra/sinatra-book/blob/master/book/Introduction.markdown#hello-world-application
[RESTクライアント] __ v__：https://github.com/archiloque/rest-client
[ライブラリ] __ v__：/ライブラリ/
[シナトラ認証githubのテスト] __ v__：https://github.com/atmos/sinatra-auth-github-test
[OAuthのスコープ] __ v__：/ V3 /のOAuth /＃スコープ
[編集スコープポスト] __ v__：/変更/ 2013年10月4日 - OAuthの-変更-来て/
[有効なトークンをチェック] __ v__：/ V3 / oauth_authorizations /＃チェック-承認を
[プラットフォームのサンプル] __ v__：https://github.com/github/platform-samples/tree/master/api/ruby/basics-of-authentication
[新しいOAuthのアプリ] __ v__：https://github.com/settings/applications/new
[アプリの設定] __ v__：https://github.com/settings/developers
