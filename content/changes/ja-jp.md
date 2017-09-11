---
Title：新しい複合ステータスAPIのプレビュー
Author_name：bhuga
---

ブランチが「緑色」[status-api]になるのはどういう意味ですか？ヘルプ [Status API]
何千というチームがその質問に答えます。開発者はステータスを記録するためにそれを使用します
継続的な統合ビルド、コントリビュータライセンス契約、コードカバレッジ
分析、自動化されたセキュリティテスト、依存関係管理などが含まれます。


###ステータスコンテキスト

複数のサービスプロバイダがステータスAPIを同時に使用できるように、ステータス
現在、 `context`フィールドをサポートしています。このフィールドを使用すると、プロバイダは
他のプロバイダのステータスからのステータス。たとえば、ビルド [Jenkins] []
`ci / jenkins`のコンテキストを使うかもしれませんが、あなたの小切手は [Brakeman] []
`security / brakeman`のコンテキスト。

新しいものは、 [Combined Status endpoint] [combined-status-api]
結合された状態、および各コンテキストからの最新の状態が含まれます。システム
ステータス更新を使用して、必要なすべての情報を1か所で取得できるようになりました。

＃＃＃ オプトイン

既存のものは、これまでどおりに動作し続けます。ザ [Status API] [list-statuses]
`context`フィールドは完全にオプションで、[pullのマージボタンの色
リクエスト]（https://github.com/blog/1227-commit-status-api）は現在ありません
文脈を考慮する。

###プレビュー期間

今日、この新しいAPIを開発者が利用できるようにしています
[preview] [status-api] 。この期間中、当社はコンバインド
ステータスAPI。ここで変更を発表します（
デベロッパーブログ）、事前に通知することはありません。

プレビュー期間は30-60日間続くことが期待されます。プレビュー期間の終わりに、
Combined Status APIはGitHub API v3の公式コンポーネントになります。で
その時点で、それは安定した生産使用のために適しています。

私たちはあなたと！ [try it out] [status-api] [send us your feedback] [contact]

[status-api] ：/ V3/repos/statuses/ /レポ/リリース/
[contact] ：https://github.com/contact?form =結合+ステータス+ [subject] API
[combined-status-api] ：/ v3 / repos / statuses /＃特定の状態のための統合ステータス取得
[create-a-status] ：/ v3 / repos / statuses /＃create-a-status
[brakeman]: http://brakemanscanner.org/
[jenkins]: http://jenkins-ci.org/
[list-statuses] ：/ v3 / repos / statuses /＃list-status-for-a-specific-ref
