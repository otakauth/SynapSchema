---
name: synapschema
description: 「シナプスキーマ」— 知識を「概念ごとのカード」に分けて蓄積し、図解・出典・関連する概念のつながりを持つ知識図鑑に育てるコレクション。テーマや素材は問わない（本・記事・講義や動画のメモ・ウェブページ・ユーザーが指定したテーマなど）。1レコード = 1概念（素材単位ではなく概念単位。同じ概念が複数の素材に出ても1枚のカードにまとめる）。records live at `data/synapschema/items/<id>.json`（id = 概念スラッグ）。ユーザーは `/collections/synapschema` の「マップ」ビューで閲覧する。ユーザーが素材を貼り付けて「取り込んで」と言ったとき、テーマを指定して「図鑑を作って」と言ったとき、見本カードを消したいとき、分類や図解を変えたいときに使う。レコードI/Oは `manageCollection`。
---

# シナプスキーマ（知識図鑑）

素材を読み、話題を **概念ごとのカード** に分けて積み上げていく図鑑。
カードには、図解・出典ごとの話（いつ、何から得た話か）・関連する概念へのつながりが付く。

## はじめて入れたとき（インストール）

ユーザーがこのリポジトリの URL を貼って「入れて」と言ったら、次の順に進める。

0. URL のリポジトリを `github/<リポジトリ名>/` に `git clone` する（すでにあれば `git pull`）。
   URL がフォルダのページ（`…/tree/main/mulmoclaude/synapschema` など）なら、`/tree/` より前をリポジトリの URL とみなす。
   以下の「このフォルダ」は、その中の `mulmoclaude/synapschema/` を指す。
   ユーザーが展開済みのフォルダをワークスペース内に置いた場合は、それを使う。
1. `config/helps/collection-skills.md` を読み、この環境でスキルを置く場所を確かめる。
   - `data/skills/` があれば `data/skills/synapschema/` に置く。
   - なければ `.claude/skills/synapschema/` に置く。
2. このフォルダの `SKILL.md`・`schema.json`・`views/`・`tools/` を、そのままそこへ写す（`README.md` と `samples/` は写さなくてよい）。
3. `samples/*.json`（見本カード11枚）を1つの JSON 配列にまとめてワークスペース内に書き出し、
   `manageCollection` の `putItems`（`slug: "synapschema"`、`itemsFile` にその絶対パス、`mode: "create"`）で入れる。
4. `presentCollection`（`collectionSlug: "synapschema"`）で図鑑を見せる。
5. 最後に「どんなテーマの図鑑にするか」をたずねる。答えがあれば、下の「分類を変える」でテーマに合う分類（4〜8個）に作り直す。

## カードの項目

- `id` — 概念スラッグ（英小文字とハイフン）。主キー。
- `title` — 概念の名前（必須）
- `category` — 分類（必須）。値は `schema.json` の enum と、`views/map.html` の `CATEGORIES` の両方にある id。
- `headline` — 15〜30字で、ひとことで言うと（必須）
- `summary` — 「結局なに？」を、前提知識なしで読める言葉で（必須）
- `firstIssue` — その概念の、いちばん古い出典の日付（`YYYY-MM-DD`）
- `visualization` — 図解のキー（`views/map.html` の `FIGS` にある名前）
- `body` — 出典ごとの話（表）。`label`（見出し）/ `text`（本文）/ `source`（その話の出典の日付 `YYYY-MM-DD`）/ `sourceType`（`article`＝素材の本文、`qa`＝質問への回答）
- `relatedTopics` — 関連する概念（表）。`concept` にほかのカードの id。これが図鑑のつながりになる
- `sourceArticles` — この概念が出た出典の日付（表）。`date`
- `referencedBy` — 逆向きのつながり（自動で計算される。書かない）

### 出典の日付の決め方

画面は、日付をもとに「新しい順・古い順」の並びや「時系列で読む」を作る。

- 発行日がある素材（記事・ニュースレター・論文・動画など）は、その発行日。
- 発行日がない、または分からない素材（本のメモ、会話のメモなど）は、取り込んだ日。
- 歴史のように、話そのものに年代があるテーマでも、ここに入れるのは **素材の日付**。年代は `body` の本文や図解（年表）で見せる。

## 素材を取り込む

ユーザーが素材（文章の貼り付け、ファイル、ウェブページの URL など）を渡して「取り込んで」と言ったら：

1. 出典の日付を決める（上の「出典の日付の決め方」）。
2. 素材から、カードにする価値のある概念を選ぶ。素材の段落の数ではなく、**話題の数** で考える。
3. 概念ごとに、既存のカードがあるかを `getItems`（`fields: ["title","category"]`）で確かめる。
   - **ある** → `body` に、その素材で出た話を1行足し、`sourceArticles` にその日付を足す（`mode: "merge"`。表は丸ごと置き換わるので、既存の行も含めて書く）。
   - **ない** → 新しいカードを作る（`mode: "create"`）。
4. 関係のある概念どうしを `relatedTopics` でつなぐ。新しいカードからだけでなく、既存のカードからもつなぐ。
5. 図解があると分かりやすい概念には、下の「図解を足す」の手順で図を足す。
6. 最後に、足したカード・直したカードの数と題名を短く伝え、`presentCollection` で見せる。

## テーマを指定されて作る

素材なしで「〇〇の図鑑を作って」と言われたら、MulmoClaude が自分で解説を書く。

1. テーマに合う分類（4〜8個）を決め、下の「分類を変える」で入れる。
2. 分類ごとに、中心になる概念から順に、10〜20枚ずつカードを作る。一度に作りすぎず、区切りのよいところで見せて、続けるかをたずねる。
3. 数字・年代・人名・固有名など、まちがえやすい事実は、使えるならウェブ検索で確かめる。確かめられなかったものは、言い切らない書き方にする。
4. `source` と `sourceArticles` には、書いた日の日付を入れる。

### 書き方の決まり

- **素材にない評価を足さない。**「期待できる」「有望」「おすすめ」などの言葉は、素材にあるときだけ書く。
- 質問コーナーやインタビューで、素材の筆者が質問に答えた部分は、`sourceType: "qa"` にする。画面に「質問への回答」と表示され、本文の主張と区別できる。
- 素材をそのまま長く写さない。自分の言葉で短くまとめる。
- 有料の素材や、公開されていない素材を取り込んだ図鑑は、ユーザー自身が手元で使うためのもの。外へ公開する前には、素材の決まりを確かめるよう伝える。

## 見本を片づける

ユーザーが「見本を消して」と言ったら：

- 見本カード11枚（`llm` `transformer` `ai-agent` `token-pricing` `gpu` `moores-law` `humanoid` `autonomous-driving-levels` `git` `dollar-cost-averaging` `inflation`）を `deleteItems` で消す。ただし、ユーザーが同じ id を使い、中身を書き換えたカードは消さない。
- `views/map.html` の中の、見本カードを指している次の部分を、取り込んだ内容に合わせて書き直すか空にする。
  - `GLOSSARY`（用語集。本文中の言葉にふきだしで説明が出る）
  - `CATEGORY_GUIDE`（分類ごとの読み物。`{{概念id}}` と書くとカードへのリンクになる）
  - `LESSON_FLOWS` と `CATEGORY_LESSONS`（「時系列で読む」などの、カードを順にたどる読み物）
  - `FIGS` の中の `sample-*` の図解（使っているカードがなくなったものは消してよい）
- `views/map.html` を直したら、スマホ版を作り直す：`node <スキルの場所>/tools/mkmobile.cjs`

## 図解を足す

図解はデータではなく **画面のコード** に書く。

1. `views/map.html` に、図を返す関数 `FigXxx` を書く。
   - 使える部品：`Bars`（横棒グラフ）、`Steps`（段階）、`StatFlow`（数字の流れ）、`TwoBox`（2つの比較）、`ChipRow`（矢印でつないだ流れ）、`Timeline`（年表）、`Meter`、`StackBar`、`NoteBox`、`Svg`（SVG を直接書く）。
   - 見本の `FigSample*` が書き方の例になる。
2. `FIGS` に `"キー": FigXxx` を登録する。
3. カードの `visualization` にそのキーを入れる（`mode: "merge"`）。
4. スマホ版を作り直す：`node <スキルの場所>/tools/mkmobile.cjs`

## 分類を変える

分類は2か所にある。**必ず両方** を同じ id にそろえる。

- `schema.json` の `category` の enum（`manageCollection` の `getSchema` → `putSchema` で直す）
- `views/map.html` の `CATEGORIES`（id と表示名）

見本カードを残したまま分類を変えるときは、見本カードを先に消すか、新しい分類へ付け替える（enum にない分類のカードは開けなくなる）。
直したあと、スマホ版を作り直す。

## 図鑑の名前を変える・2つ目の図鑑を作る

- 名前を変える：`schema.json` の `title` と、`views/map.html` の見出し（`"シナプスキーマ"` と書かれた部分）を直す。
- テーマ別にもう1つ図鑑を作る：スキルのフォルダを `synapschema-<テーマ>` という名前で写し、`schema.json` の `dataPath`、`relatedTopics` の `to`、`referencedBy` の `from` を新しい名前にそろえる。この SKILL.md の `synapschema` も新しい名前に置き換える。

## 画面

- `views/map.html` — パソコン用。分類 → カード一覧 → カード、の順にたどる。
- `views/map-mobile.html` — スマホ用。`tools/mkmobile.cjs` で `map.html` から自動で作る。**直接は直さない。**

チャットに全カードを書き出さない。追加・更新のあとは `presentCollection` で見せる。
