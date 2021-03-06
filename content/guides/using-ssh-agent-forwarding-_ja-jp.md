---
：/ガイド/使用して、ssh-agentの転送/
---

：/ガイド/使用して、ssh-agentの転送/

{:toc}

SSHエージェント転送は、サーバ簡単に展開するために使用することができます。それはあなたが代わりに鍵を残してのローカルのSSHキーを使用することができます（パスフレーズなし！）あなたのサーバー上に座って。

すでにと対話するためのSSHキーを設定している場合は、おそらく `sshを-agent`に精通しています。これは、バックグラウンドで実行され、パスフレーズキーを使用する必要がある{{ site.data.variables.product.product_name }}たびに入力する必要がないように、メモリにロードされ、あなたの鍵を保つためのプログラムです。気の利いたものは、あなたが、彼らはすでにサーバー上で実行しているかのようにサーバーがローカルの `のssh-agent`にアクセスできるように選択することができ、です。これは一種のあなたが自分のコンピュータを使用できるようにパスワードを入力するように友人を尋ねるようなものです。

SSHエージェント転送のより詳細な説明のためにチェックしてください。 [Steve Friedl's Tech Tips guide] [tech-tips]

## SSHエージェント転送を設定します

独自のSSHキーを設定し、動作していることを確認してください。あなたはまだこれを行っていませんでした場合は、使用することができます。 [our guide on generating SSH keys] [generating-keys]

あなたがテストすることができ、その端末に `sshの-Tのgit @ github.com`を入力して、あなたのローカルキーの作品：

`` `コマンドライン
$ sshの-T git@github.com
GithubのにでSSHに＃の試み
>ユーザー名こんにちは！あなたが正常に認証しましたが、GitHubには提供されません。 <em> </em>
>シェルアクセス。
```

我々は素晴らしいスタートを切っています。それでは、あなたのサーバーにエージェント転送を許可するようにSSHを設定してみましょう。

1.お好みのテキストエディタを使用して、 `の〜/ .ssh / config`をでファイルを開きます。このファイルが存在しない場合は、端​​末内に `タッチの〜/ .ssh / config`をを入力して、それを作成することができます。

2.サーバーのドメイン名またはIPとexample.com` `置き換えて、ファイルに次のテキストを入力します。

Example.comをホスト
ForwardAgentはい

{{#warning}}

**警告：**あなただけのすべてのSSH接続にこの設定を適用するには、 `*ホスト`のようなワイルドカードを使いたくなるかもしれません。あなたがにSSHすべての*サー​​バー*を使用してローカルのSSHキーを共有しているはずだとしてそれは、本当に良いアイデアではありません。彼らは、キーに直接アクセスすることはできませんが、彼らは、接続が確立されている間*あなたのように*それらを使用することができるようになります。 **あなただけあなたが信頼のサーバーを追加する必要がありますし、エージェント転送に使用すること。**

{{/warning}}

## SSHエージェント転送

そのエージェント転送は、サーバーと協力してテストするには、サーバーにSSHで接続することができますし、もう一度 `のssh -Tのgit @のgithub.com`を実行します。すべてが順調である場合は、ローカルで行ったように、あなたは、同じプロンプトが返されます。

あなたの地元のキーが使用されている場合、あなたがわからない場合は、サーバー上の `SSH_AUTH_SOCK`変数を検査することができます。

`` `コマンドライン
$エコー "SH_AUTH_SOCK」 $S
＃SSH_AUTH_SOCK変数をプリントアウト
> /tmp/ssh-4hNGMk8AZX/agent.79453
```

変数が設定されていない場合は、エージェント転送が動作していないことを意味します。

`` `コマンドライン
$エコー "SH_AUTH_SOCK」 $S
＃SSH_AUTH_SOCK変数をプリントアウト
> <em>[No output]</em>
$ sshの-T git@github.com
＃githubのにSSHにしてみてください
>アクセス許可は拒否されました（公開鍵）。
```

## SSHエージェント転送

ここでは、SSHエージェント転送のトラブルシューティングを行う際の外を見るためにいくつかのものがあります。

###あなたがコードをチェックアウトするには、SSHのURLを使用している必要があります

唯一の転送SSHは、SSHのURLではなく、HTTP（S）のURLで動作します。サーバー上の* .git / configに*ファイルをチェックして、URLを確保することは、以下のようなSSHスタイルのURLです：

`` `コマンドライン
[remote "origin"]
URL = git@github.com：yourAccount <em> / </em> yourProject <em> .git </em>
フェッチ= +レフリー/ヘッド/ *：レフリー/リモコン/原点/ *
```

###あなたのSSHキーはローカルで動作する必要があります

あなたはあなたの鍵はエージェントの転送を介して動作することができます前に、彼らは最初にローカルで動作する必要があります。あなたがローカルであなたのSSHキーを設定することができます。 [Our guide on generating SSH keys] [generating-keys]

###お使いのシステムでは、SSHエージェント転送を許可する必要があります

時には、システム構成は、SSHエージェント転送を禁止します。システム構成ファイルは、ターミナルで次のコマンドを入力して使用されているかどうかを確認することができます。

`` `コマンドライン
$ sshの-v example.com <em> </em>
＃詳細なデバッグ出力でexample.comに接続します
> OpenSSH_5.6p1、のOpenSSL 0.9.8r 2011年2月8日 </span>
> DEBUG1：読書構成データ/ユーザ/あなたが/.ssh/config <em> </em>
> DEBUG1：example.comのオプションを適用します
> DEBUG1：読書の設定データは/ etc / ssh_configを
> DEBUG1：用のオプションを適用します*
$出口
あなたのローカルコマンドプロンプトに＃を返します
```

上記の例では、ファイル*の〜/ .ssh / configに*最初にロードされ、その後、*は/ etc / ssh_configの*が読み込まれます。我々は、それが次のコマンドを実行して、当社のオプションをオーバーライドだかどうかを確認するために、そのファイルを調べることができます。

`` `コマンドライン
$猫の/ etc / ssh_configを
＃の/ etc / ssh_configファイルをプリントアウト
>ホスト*
> SendEnv項目LANG LC_ *
> ForwardAgentなし
```

この例では、私たちの*の/ etc / ssh_configの*ファイルは、具体的には、 `ForwardAgent no`、エージェント転送をブロックする方法があると言います。ファイルからこの行を削除すると、もう一度作業フォワーディング・エージェントを取得する必要があります。

###サーバーは、インバウンド接続でSSHエージェント転送を許可する必要があります

エージェント転送はまた、サーバー上でブロックされることがあります。あなたは、エージェント転送がサーバーにSSHingと `sshd_config`を実行することによって許可されていることを確認することができます。このコマンドの出力は `AllowAgentForwarding`が設定されていることを示す必要があります。

###あなたの地元の `のssh-agent`を実行している必要があります

ほとんどのコンピュータでは、オペレーティングシステムが自動的にあなたのための `のssh-agent`を起動します。 Windowsでは、しかし、これを手動で行う必要があります。我々は持っています 。 [a guide on how to start `ssh-agent` whenever you open Git Bash] [autolaunch-ssh-agent]

SSH-agent`がコンピュータ上で実行されている `ことを確認するには、ターミナルで次のコマンドを入力します。

`` `コマンドライン
$エコー "SH_AUTH_SOCK」 $S
＃SSH_AUTH_SOCK変数をプリントアウト
>を/ tmp /打ち上げ-kNSlgU /リスナー
```

###あなたのキーは `のssh-agent`に利用可能でなければなりません

あなたのキーは、次のコマンドを実行して `のssh-agent`に表示されていることを確認することができます。

`` `コマンドライン
SSH-追加-L
```

コマンドにはIDが利用できないと言う場合は、あなたのキーを追加する必要があります。

`` `コマンドライン
$ sshを-追加yourkey <em> </em>
```

{{#tip}}

それはリブート時に再開した後にMac OS Xでは、 `SSH-agent`は、このキーを「忘れる」になります。しかし、あなたは、このコマンドを使用して、キーチェーンにあなたのSSHキーをインポートすることができます。

`` `コマンドライン
$は/ usr / <em> binに/ </em> sshは、追加-K yourkey
```

{{/tip}}

[tech-tips] ：http://www.unixwiz.net/techtips/ssh-agent-forwarding.html
[generating-keys] ：https://help.github.com/articles/generating-ssh-keys
[ssh-passphrases] ：https://help.github.com/ssh-key-passphrases/
[autolaunch-ssh-agent] ：https://help.github.com/articles/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-msysgit
されていることを確認することができます。

`` `コマンドライン
SSH-追加-L
```

コマンドにはIDが利用できないと言う場合は、あなたのキーを追加する必要があります。

`` `コマンドライン
$ sshを-追加yourkey <em> </em>
```

{{#tip}}

それはリブート時に再開した後にMac OS Xでは、