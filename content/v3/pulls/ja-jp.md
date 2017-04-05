---
タイトル：コメントのレビュー
---

＃コメントのレビュー！

{:toc}

プルリクエストのコメントのレビューは、統一の部分にコメントしています
デフ。これらは、適用されているコミットコメント（とは別のものです
直接に）プル要求ビューの外に、コミット、および発行
（unified diff形式の部分を参照しない）コメント。

リクエスト・コメントのレビューは、[これらのカスタムメディアを使用するプル
APIのメディアタイプの使用についての詳細を読むことができます
（/ V3 /メディア/）。[here]

##リストは、プル要求にコミット

所有者/：：レポ/プル/：数/コミット/レポは/ GET

応答。

<%= headers 200, :pagination => default_pagination_rels％>
<%= json(:pull_comment) { |h| [h] } %>

リポジトリ内の##のリストのコメント

所有者/：：レポ/問題/コメント/レポ/ GET

デフォルトでは、問題のコメントはIDを昇順に並べられています。

＃＃ パラメーター

`NAME` |` type`が|説明。
-----|------|--------------
`sort` |` STRING` |どちらか `created`または` updated`。デフォルト： `created`
`direction` |` STRING` |どちらか `asc`または` desc`。 `sort`パラメータを指定せずに無視されます。
`since` |` STRING` |この時以降に更新されたコメントだけが返されます。これは、ISO 8601形式のタイムスタンプです： `YYYY-MM-DD T HH：MM：SSZ`。


応答。

<%= headers 200, :pagination => default_pagination_rels％>
<%= json(:pull_comment) { |h| [h] } %>

##単一のコメントを取得

GET /レポ/：所有者/：レポ/問題/コメント/：ID

応答。

<%= headers 200 %>
<%= json :pull_comment %>

##コメントを作成します。

POST /レポ/：所有者/：レポ/問題/：数/コメント

###入力

名前|タイプ|説明
-----|------|--------------
`body` |` STRING` |プル要求の内容。
`commit_id` |` STRING` | **必須**。 SHAは、上のコメントをコミットします。
`PATH` |` STRING` | **必須**。上のコメントのファイルの相対パス。
`position` |` integer` | **必須**。上のコメントの差分の行のインデックス。

{{#tip}}

`commit_id`を渡すときに、最新のSHAがプルリクエストでコミットまたは指定された` position`がコミット後続に変更されている場合、あなたのコメントが「時代遅れ」として表示されることがあります。

ファイル内の特定の行にコメントをするには、まず差分内の位置を決定する必要があります。 GitHubには、あなたがプル要求の差分を表示するには、前の要求で使用することができます `アプリケーション/ application/vnd.github.v3.diff`メディアタイプを提供しています。 diffは*（https://en.wikipedia.org/wiki/Diff_utility#Unified_format）差分で*の位置にファイル*の*ラインから翻訳することが必要です。 `position`値は、行数ダウンあなたがコメントしたいファイルの最初の「@@」ハンクヘッダからです。ただ「@@」行の下の行は次の行がように位置2である、と、位置1です。ファイルの差分の位置が新しいファイルに達するまで空白や追加のハンクのラインを介して増加し続けています。 [interpreted]

{{/tip}}

####例

<％= jsonの\
：ボディ=> 'ニースの変化」、
：commit_id => '6dcb09b5b57875f334f61aebed695e2e4193db5e」、
：パス=> 'FILE1.TXT」、
：位置=> 4
%>

###代替入力

代わりに `commit_id`、` PATH`、および `を渡すのposition`あなたがに返信することができます！
このような既存のプル要求のコメント：

名前|タイプ|説明
-----|------|--------------
`body` |` STRING` |プル要求の内容。
`in_reply_to` |` integer` | **必須**。に返信するコメントID。


####例

<％= jsonの\
：ボディ=> 'ニースの変化」、
：in_reply_to => 4
%>

応答。

<%= headers 201, :Location => get_resource（：pull_comment）％> ['url']
<%= json :pull_comment %>

##コメントを編集します

PATCH /レポ/：所有者/：レポ/問題/コメント/：ID

###入力

名前|タイプ|説明
-----|------|--------------
`body` |` STRING` |プル要求の内容。


####例

<％= jsonの\
：ボディ=> 'ニースの変化」、
%>

応答。

<%= headers 200 %>
<%= json :pull_comment %>

##コメントを削除

DELETE /レポ/：所有者/：レポ/問題/コメント/：ID

応答。

<%= headers 204 %>

##カスタムメディアタイプ

これらは、プル要求のためにサポートされているメディアの種類です。あなたはについての詳細を読むことができます
APIのメディアタイプを使用する（/ V3 /メディア/）。 [here]

application/vnd.github.VERSION.raw+json
application/vnd.github.VERSION.text+json
application/vnd.github.VERSION.html+json
application/vnd.github.VERSION.full+json
