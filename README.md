# SynapSchema

SynapSchema（シナプスキーマ）は、生成AIを使って知識を「概念ノード」と「関係性」で構造化し、インタラクティブに探索できる図鑑形式へ変換する試みです。

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

このリポジトリには、使い方の異なる2つの版が入っています。

| | 向いている人 | 中身 |
|---|---|---|
| [**1. MulmoClaude 版**](#1-mulmoclaude-版) | [MulmoClaude](https://github.com/receptron/mulmoclaude) を使っている人 | スキル一式と、MulmoClaude で作った図鑑 |
| [**2. 生成AI汎用版**](#2-生成ai汎用版) | ChatGPT・Claude など、ふだん使っている生成AIで試したい人 | プロンプトとサンプルHTML |

---

## 1. MulmoClaude 版

### MulmoClaude 用スキル

MulmoClaude に入れて使うスキル（シナプスキーマ）です。[mulmoclaude/synapschema](mulmoclaude/synapschema/) に入れてあります。

プロンプトで1枚の HTML を作る方法とちがい、素材を取り込むたびにカードが増え、つながりが足されていきます。テーマや素材は選びません。
使い方は、そのページの URL を MulmoClaude のチャットに貼って「入れて」と伝えるだけです。

### MulmoClaude Showcase

MulmoClaudeを使って作成した、大規模なインタラクティブHTML図鑑です。

SynapSchemaのサンプルとは別に、
**MulmoClaudeを使うことで、ここまで大規模でインタラクティブな知識コンテンツを1つのHTMLとして生成できる**ことを示すショーケースとして公開しています。

科学の繋がりを網羅的に学べるインタラクティブHTMLサイエンス図鑑
- [synapschema-science.html](mulmoclaude/synapschema-science.html)

歴史・地理を組み合わせた大規模なインタラクティブHTML図鑑
- [synapschema-history-geo-sample.html](mulmoclaude/synapschema-history-geo-sample.html)

関連語と結び付けて多角的に学べるインタラクティブHTML英単語帳
- [synapschema-eitango.html](mulmoclaude/synapschema-eitango.html)

人文・社会系学問の繋がりを網羅的に巡るインタラクティブHTMLリベラルアーツ図鑑
- [synapschema-liberal-arts.html](mulmoclaude/synapschema-liberal-arts.html)

---

## 2. 生成AI汎用版

ChatGPT・Claude など、ふだん使っている生成AIで SynapSchema 形式の図鑑を作る方法です。

### 使い方

1. prompts/synapschema-prompt.md およびelements.htmlをAIに渡す
2. テーマを指定する
3. SynapSchema形式のHTMLを生成する

### サンプル

- [protein.html](examples/protein.html)（タンパク質図鑑）
- [elements.html](examples/elements.html)（元素図鑑）
- [greek-history.html](examples/greek-history.html)（古代ギリシャ図鑑）
- [syouwa.html](examples/syouwa.html)（昭和史図鑑）

---

## ライセンス

MIT License
