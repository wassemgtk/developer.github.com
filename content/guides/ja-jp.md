---
タイトル：グラフなどのデータをレンダリング
---


{:toc}

このガイドでは、リポジトリについての情報を取得するためにAPIを使用するつもりです
我々が所有していること、およびそれらを構成するプログラミング言語。その後、我々はよ
ライブラリを使用して、いくつかの方法でその情報を視覚化します。に [D3.js] [D3.js]
APIと対話し、私たちは、優れたRubyのライブラリを使うことになるでしょう。 {{ site.data.variables.product.product_name }} [Octokit] [Octokit]

あなたがまだの場合は、お読みください ["Basics of Authentication"] [basics-of-authentication]
この例を開始する前に導きます。あなたは、リポジトリにこのプロジェクトのための完全なソースコードを見つけることができます。 [platform samples] [platform-samples]

右にジャンプしてみましょう！

##のOAuthアプリケーションを設定します

まず、上。メインとコールバックを設定します [register a new application] {{ site.data.variables.product.product_name }} [new oauth application]
HTTP `へのURL：// localhostを：4567/ /`。我々はによってAPIの認証を処理しようとしているとして、 [before] [basics-of-authentication]
使用して、ラックのミドルウェアを実装します： [sinatra-auth-github] [sinatra auth github]

`` `ルビー
「シナトラ/認証/ githubの 'を必要と

モジュール例
クラスMyGraphApp <シナトラ::ベース
＃!!! EVER REALアプリでハードコードされた値は、使用しないでください！
＃その代わりに、以下のように、変数を設定し、テスト環境
ENV && ENV場合＃ ['GITHUB_CLIENT_ID'] ['GITHUB_CLIENT_SECRET']
CLIENT_ID = ENV ['GITHUB_CLIENT_ID']
CLIENT_SECRET = ENV ['GITHUB_CLIENT_SECRET']
終わり

CLIENT_ID = ENV ['GH_GRAPH_CLIENT_ID']
CLIENT_SECRET = ENV ['GH_GRAPH_SECRET_ID']

有効：セッションを

セット：github_options、{
：スコープ=> "レポ"、
：秘密=> CLIENT_SECRET、
{：CLIENT_ID => CLIENT_ID、
：callback_url => "/"
}

シナトラ::認証:: Githubに登録

'/'入手できますか
場合！認証？
認証する！
ほかに
Access_tokenは= github_user ["token"]
終わり
終わり
終わり
終わり
```

前の例と同様の_config.ru_ファイルを設定します。

`` `ルビー
ENV ['RACK_ENV'] || = '開発'
「RubyGemsの」が必要
「バンドラー/セットアップ」が必要

必要File.expand_path（File.join（File.dirname（）、「サーバ」）） __FILE__

例:: MyGraphAppを実行
```

##リポジトリ情報の取得

今回は、APIに話をするために、我々は[Octokitを使用するつもりです {{ site.data.variables.product.product_name }}
Rubyのライブラリー]。これは、直接の束を作るよりもはるかに簡単です [Octokit]
REST呼び出し。さらに、Octokitは、GitHubberによって開発された、積極的に維持されています
だからあなたはそれがうまくいく知っています。

Octokit経由でAPIを使用した認証は簡単です。ちょうどあなたのログインを渡します
そして、 `Octokit :: Client`コンストラクターにトークン：

`` `ルビー
場合！認証？
認証する！
ほかに
Octokit_client = Octokit :: Octokit::Client.new（：ログイン=> github_user.login、：oauth_token => github_user.token）
終わり
```

私たちのリポジトリに関するデータと面白いことをやってみましょう。行っていた
彼らが使用する異なるプログラミング言語を参照し、使用されているものをカウントします
よく。これを行うには、まずAPIから私たちのリポジトリのリストが必要になります。
Octokitで、それは次のようになります。

`` `ルビー
レポ= client.repositories
```

次に、我々は、各リポジトリを反復処理し、その言語を数えます {{ site.data.variables.product.product_name }}
それに関連付け：

`` `ルビー
Language_obj = {}
レポを| Repos.each行います|
＃時々言語はnilにすることができます
Repo.language場合
場合！language_obj [repo.language]
Language_obj [repo.language] + = 1
ほかに
Language_obj [repo.language] + = 1
終わり
終わり
終わり

Languages​​.to_s
```

あなたは、サーバーを再起動すると、あなたのWeb​​ページには、何かを表示する必要があります
それは次のようになります。

`` `ルビー
{"JavaScript"=>13, "PHP"=>1, "Perl"=>1, "CoffeeScript"=>2, "Python"=>1, "Java"=>3, "Ruby"=>3, "Go"=>1, "C++"=>1}
```

これまでのところ、とても良いではなく、非常に人間に優しいです。可視化
私たちはこれらの言語カウントがどのように分布しているかを理解する手助けに素晴らしいことです。フィードしてみましょう
D3への私たちの数は、我々が使用する言語の人気を表すニート棒グラフを取得します。

##言語数を可視化

D3.js,、または単にD3,は、チャート、グラフ、およびインタラクティブな視覚化の多くの種類を作成するための包括的なライブラリです。
詳細にD3を使用すると、このガイドの範囲を超えていますが、良い紹介記事のために、
チェックアウト 。 ["D3 for Mortals"] [D3 mortals]

D3は、JavaScriptライブラリであり、配列などのデータを扱うのが好き。それでは、私たちにRubyのハッシュを変換してみましょう
ブラウザのJavaScriptが使用するためのJSON配列。

`` `ルビー
言語= []
Repo_langs.each行う|ラング、カウント|
Languages​​.push：言語=> LANG、：カウント=>カウント
終わり

ERB：lang_freq、：地元の人=> { :languages => languages.to_json}
```

私たちは、単に私たちのオブジェクトの各キーと値のペアを繰り返し処理とにそれらをプッシュしています
新しい配列。私たちは反復したくなかったので、我々はこれ以前をしなかった理由は、
私たちの `language_obj`オブジェクトの上に我々はそれを作成している間。

さて、_lang_freq.erb_は、バーグラフのレンダリングをサポートするためにいくつかのJavaScriptを必要としています。
今のところ、あなたはちょうどここに提供されるコードを使用することができ、かつ上記のリンクされたリソースを参照してください。
あなたはD3がどのように機能するかについての詳細を知りたい場合：

`` `HTML
<!DOCTYPE html>
<meta charset="utf-8">
<html>
<head>
<script src="//cdnjs.cloudflare.com/ajax/libs/d3/3.0.1/d3.v3.min.js"></script>
<style>
SVG {
パディング： 20pxピクセル;
}
RECT {
記入します。＃2d578b
}
テキスト{
フィル：白;
}
Text.yAxis {
フォントサイズ： 12pxピクセル;
フォントファミリ：ヘルベチカ、サンセリフ;
フィル：黒;
}
</style>
</head>
<body>
<p> この甘いデータをチェックアウト： </p>
<div id="lang_freq"></div>

</body>
<script>
Var data = <％= languages％>;

VAR barWidth = 40;
VAR幅=（barWidth + 10)）* data.length。
VAR高さ= 300;

VARのx = d3.scale.linear（）ドメイン（）.range（）。 [0, data.length] [0, width]
VARのy = d3.scale.linear（）。ドメイン（）。 [0, d3.max(data, function(datum) { return datum.count; })]
RangeRound（）。 [0, height]

// DOMにキャンバスを追加
するvar languageBars = d3.select（ "＃のlang_freq」）。
（ "：SVG SVG"）を追加します。
ATTR（「幅」、幅）。
ATTR（「高さ」、高さ）;

LanguageBars.selectAll（「RECT」）。
データ（データ）。
入る（）。
（ "：RECT SVG"）を追加します。
ATTR（「X」、機能（データム、インデックス））。 { return x(index); }
ATTR（「y」は、機能（データム））。 { return height - y(datum.count); }
ATTR（「高さ」、機能（データム））。 { return y(datum.count); }
（「幅」、barWidth）ATTR。

LanguageBars.selectAll（ "テキスト"）。
データ（データ）。
入る（）。
（ "：テキストSVG"）を追加します。
ATTR（「X」、機能（データム、インデックス））。 { return x(index) + barWidth; }
ATTR（「y」は、機能（データム））。 { return height - y(datum.count); }
ATTR（「DX」、-barWidth /2).）。
ATTR（「DY "、" 1.2em "）。
ATTR（「テキストアンカー」、「中」）。
テキスト（関数（データム））; { return datum.count;}

LanguageBars.selectAll（「text.yAxis」）。
データ（データ）。
入力します（）追加。（ "SVG：テキスト"）。
ATTR（「X」、機能（データム、インデックス））。 { return x(index) + barWidth; }
ATTR（「Y」、高さ）。
ATTR（「DX」、-barWidth /2).）。
ATTR（「テキストアンカー」、「中」）。
テキスト（関数（データム））; { return datum.language;}
ATTR（ "（(0, 18)、18）翻訳"、 "変換"）。
ATTR（「クラス」、「YAXIS "）;
</script>
</html>
```

あー！ここでも、このコードのほとんどが何をしているのか心配しないでください。要部
ここでは一番上の行の方法です - `varデータ=は; `--whichを示し
我々は、操作のためにERBに私たちの以前に作成した `languages​​`配列を渡していること。

「D3の人間のための「ガイドが示唆するように、これは必ずしも最良の使用ではありません
D3.。しかし、それはあなたがOctokitと一緒に、ライブラリを使用する方法を説明するためのものでありません、
いくつかの本当に素晴らしいものを作るために。

##異なるAPI呼び出しを組み合わせます

今では、告白の時間です：リポジトリ内の `language`属性
唯一の定義された「一次」言語を識別する。それはあなたが持っている場合ことを意味し
複数の言語を組み合わせて、リポジトリ、コードのほとんどのバイトの1
第一言語であると考えられています。

言語の的に_真_表現を取得するために、いくつかのAPIコールを結合してみましょう
すべての私たちのコードを横切って書き込まれたバイトの最大数を持っています。 A [treemap] [D3 treemap]
むしろ、使用私たちのコーディング言語の大きさを視覚化するための素晴らしい方法であるべきです
単純にカウントより。私たちは、見えるオブジェクトの配列を構築する必要があります
このようなもの：

<％= jsonの\
[ { "name": "language1", "size": 100},
{ "name": "language2", "size": 23}
...
]
```

我々は既に上記のリポジトリのリストを持っているので、の各1を検査してみましょう、と
呼び出し： [the language listing API method] [language API]

`` `ルビー
レポを| Repos.each行います|
Repo_name = repo.name
Repo_langs = octokit_client.languages​​（ {github_user.login} "＃/＃"） {repo_name}
終わり
```

そこから、我々は累積的に「マスターリスト」に見つかった各言語を追加します：

`` `ルビー
Repo_langs.each行う|ラング、カウント|
場合！language_obj [lang]
Language_obj [lang] + =カウント
ほかに
Language_obj [lang] + =カウント
終わり
終わり
```

その後、我々はD3が理解できる構造に内容をフォーマットします。

`` `ルビー
Repo_langs.each行う|ラング、カウント|
Language_byte_count.pushます。name => "＃（＃）は"、：=> count {lang} countは {count}
終わり

＃D3のためのいくつかの必須フォーマット
Language_bytes = [ :name => "language_bytes", :elements => language_byte_count]
```

（D3ツリーマップの魔法の詳細については、チェックしてください。） [this simple tutorial] [language API]

包むために、我々は同じERBテンプレートに渡って、このJSON情報を渡します。

`` `ルビー
ERB：lang_freq、：地元の人=> { :languages => languages.to_json, :language_byte_count => language_bytes.to_json}
```

前と同じように、ここにあなたがドロップすることができたJavaScriptの束です
直接テンプレートに：

`` `HTML
<div id="byte_freq"></div>
<script>
Var language_bytes = <％= language_byte_count％>
するvar childrenFunction =関数（D）; {return d.elements}
するvar sizeFunction =関数（D）; {return d.count;}
するvar colorFunction =関数（D）; {return Math.floor(Math.random()*20)}
するvar nameFunction =関数（D）; {return d.name;}

Varの各色= d3.scale.linear（）
.domain（） [0,10,15,20]
。範囲（ ["grey","green","yellow","red"] ）;

DrawTreemap（(5000, 2000,、2000、「#byte_freq '、language_bytes、childrenFunction、nameFunction、sizeFunction、colorFunction、色）;

関数drawTreemap(height,width,elementSelector,language_bytes,childrenFunction,nameFunction,sizeFunction,colorFunction,colorScale){

するvarツリーマップ= d3.layout.treemap（）
.children（c​​hildrenFunction）
.size（） [width,height]
.VALUE（sizeFunction）。

Varのdiv = d3.select（elementSelector）
.append（「DIV」）
.style（「位置」、「相対」）
.style（「幅」、幅+「PX」）
.style（「高さ」、高さ+「PX」）。

div.data（language_bytes）.selectAll（「DIV」）
.dataの（関数（d）参照） {return treemap.nodes(d);}
入る（）。
.append（「DIV」）
.ATTR（「クラス」、「セル」）
.style（「背景」、関数の（d）） { return colorScale(colorFunction(d));}
.call（セル）
.text（nameFunction）。
}

機能セル（）{
この
.style（「左」、関数の（d）） {return d.x + "px";}
.style（「上部」、関数の（d）） {return d.y + "px";}
.style（「幅」、関数の（d）） {return d.dx - 1 + "px";}
.style（「高」、関数の（d））。 {return d.dy - 1 + "px";}
}
</script>
```

ら出来上がり！相対であなたのレポ言語を含む美しい長方形、
一目で見やすいですプロポーション。あなたがする必要がある場合があります
最初の2として渡された、あなたのツリーマップの高さと幅を調整
`すべての情報が正しく表示されるまでに取得するには、上記のdrawTreemap`への引数。


[D3.js] ：http://d3js.org/
[basics-of-authentication] 認証の＃基本！
[sinatra auth github]: https://github.com/atmos/sinatra_auth_github
[Octokit]: https://github.com/octokit/octokit.rb
[D3-for-mere-mortals/ mortals] ！ （http://www.recursion.org/d3-for-mere-mortals/）
[D4063582 treemap] ！ （http://bl.ocks.org/mbostock/4063582）
[language API] ：https://developer.github.com/v3/repos/#list-languages
[simple tree map] ：http://2kittymafiasoftware.blogspot.com/2011/09/simple-treemap-visualization-with-d3.html
[platform samples]: https://github.com/github/platform-samples/tree/master/api/ruby/rendering-data-as-graphs
[new oauth application]: https://github.com/settings/applications/new
