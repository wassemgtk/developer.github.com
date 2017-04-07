---
Title： "推奨：OAuth認証のリセット"
AUTHOR_NAME：pengwynn
---

As、私[announced earlier today]たちは[heartbleed-blog-post]積極的に対応しています
最近公開された[Heartbleed security
OpenSSLでの脆弱性]現在のところ、GitHubは [heartbleed-blog-post]
この脆弱性のテスト以外に攻撃が使用されたという兆候はありません。
OAuthのインテグレータにお勧めします [reset the API authorizations] [api]
アプリケーション。

私たちはこの正確な目的のためにaを追加しました。このメソッドを呼び出す [new API method] [api]
古いトークンを無効にし、ストアするアプリケーションの新しいトークンを返します
その代わりに使用してください。この新しい方法は、ユーザーをリセットする安全な方法を提供します
ユーザーがアプリケーションを再認証する必要はありません。
ウェブ。


{{#tip}}

** UPDATE（2016-01-25）：** API v3は、以前に参照したように、アプリケーションのトークンのすべてを取り消すメソッドを提供しなくなりました。アプリケーションのすべてのトークンを取り消す必要がある場合は、アプリケーションの設定ページから取り消すことができます。 <em> </em> <a href="https://github.com/settings/developers"> </a>

{{/tip}}

ご質問やご意見がありましたら、お願いします。 [get in touch] [contact]

[contact] ：https://github.com/contact?form = [subject] API +リセット+トークン
[api] ：/ v3 / oauth_authorizations /＃リセット - 許可
[revoke a single token] ：/ v3 / oauth_authorizations /＃revoke-an-application-for-an-application
[heartbleed-blog-post] ！ （https://github.com/blog/1818-security-heartbleed-vulnerability）
