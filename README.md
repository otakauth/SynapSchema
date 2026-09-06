# SynapSchema

SynapSchema（シナプスキーマ）は、知識を「概念ノード」と「関係性」で構造化し、図鑑形式で探索できるHTMLフォーマットです。

文章を上から読むのではなく、

- 図版を見る
- 概念を選ぶ
- 関連知識へ移動する

という学習体験を目指しています。

また、AIによる生成を前提としており、様々なテーマの知識図鑑を作成できます。

> ⚠️ **注意**
>
> SynapSchemaはAIを利用して生成することを想定しています。
> AIによる生成物にはハルシネーション（誤った情報）が含まれる可能性があります。
> 学習・研究・意思決定などに利用する場合は、必ず信頼できる一次情報や専門資料で内容を確認してください。

## サンプル

- [protein.html](examples/protein.html)（タンパク質図鑑）
- [elements.html](examples/elements.html)（元素図鑑）
- [greek-history.html](examples/greek-history.html)（古代ギリシャ図鑑）
- [syouwa.html](examples/syouwa.html)（昭和史図鑑）

## MulmoClaude Showcase

MulmoClaudeを使って作成した、歴史・地理を組み合わせた大規模なインタラクティブHTML図鑑です。

SynapSchemaのサンプルとは別に、
**MulmoClaudeを使うことで、ここまで大規模でインタラクティブな知識コンテンツを1つのHTMLとして生成できる**ことを示すショーケースとして公開しています。

歴史・地理を組み合わせた大規模なインタラクティブHTML図鑑
- [synapschema-history-geo-sample.html](MulmoClaude/synapschema-history-geo-sample.html)

科学の繋がりを網羅的に学べるインタラクティブHTMLサイエンス図鑑
-[mulmoclaude/synapschema-science.html](mulmoclaude/synapschema-science.html)

関連語と結び付けて多角的に学べるインタラクティブHTML英単語帳
- [synapschema-eitango.html](MulmoClaude/synapschema-eitango.html)



## 使い方

1. prompts/synapschema-prompt.md およびelements.htmlをAIに渡す
2. テーマを指定する
3. SynapSchema形式のHTMLを生成する

## ライセンス

MIT License
