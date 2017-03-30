---
タイトル：リリースのAPIが公式です
AUTHOR_NAME：technoweenie
---

###プレビューメディアタイプが不要

あなたはプレビュー期間中にリリースのAPIを使用した場合は、 `Accept`ヘッダにカスタムメディアタイプを提供するために必要：

application/vnd.github.manifold-preview+json

今、プレビュー期間が終了したことを、あなたは、もはやこのカスタムメディアタイプを渡す必要がありません。

代わり[media-types]に、あなたは `Accept`ヘッダ内のバージョンとして` [recommend]   v3`を指定することを我々：

！ （application/vnd.github.v3+json）

###オンワード！

プレビュー期間中にリリースのAPIを試してみ再びみんなに感謝します。
我々はいくつかの素晴らしいフィードバックを得て、我々はすでにAPIへの追加を検討しています。

私たちは、あなたが出荷ものを見るために待つことができません！

[media-types] ：/ V3 /メディア
[preview-period] ：/変更/ 2013年7月19日 - プレビュー - 新しい検索-API /＃プレビュー期間
[releases-api] ：/ V3 /レポ/リリース/
[search-api] ！ （http://developer.github.com/changes/2013-10-29-search-api-becomes-an-official-part-of-github-api-v3/）
