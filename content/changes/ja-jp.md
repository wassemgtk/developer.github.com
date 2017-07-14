---
Title：組織メンバーのリソースの変更
著者名：ペズラ
---

の（/v3/orgs/members/index.html#members-list）をリクエス[member list]トする
あなたがメンバーではない組織は[パブリックメンバーにリダイレクトされます
List]（/ v3 / orgs / members / index.html＃public-members-list）をクリックします。同様に、
[membership check] （/v3/orgs/members/index.html#check-membership）のリソース
あなたがメンバーでない組織は、同等の組織にリダイレクトされます
[public membership check] （/v3/orgs/members/index.html#check-public-membership）。
後者の場合の1つの例外は、自分自身について調べている場合です
メンバーシップは要求がリダイレクトされません。あなたは常に何を知ることが許されています
あなたが所属する組織

これらのさまざまなリソースの目的を明確にするために行われた変更。ザ
`/ orgs /：org / members`リソースは、
問題の組織。 `/ orgs /：org / public_members`リソースは、
組織の公的メンバーシップに関する情報を取得する。あなたがいる場合
あなたは私的な会員情報を見ることができませんので、
パブリックメンバーシップリソースを使用する必要があります。


