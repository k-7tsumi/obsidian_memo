- 何作ろう。
- はじめての Dify × RAG：https://qiita.com/tanai3_r/items/cbf8a3b6aaeeea8d8350
- ↑ を参照してチャットボットを作成してみる。

## ざっくり設計
- チャットボット
- 料理名、材料名、レシピURLのデータがあるCSVファイルをRAGとして読み込ませる。
- Claudeが書いてくれたプロンプト
```
あなたは料理レシピ推薦アシスタントです。
ユーザーから材料名を受け取り、アップロードされたレシピデータベースから最適なレシピを推薦してください。

入力例: 「玉ねぎ」「アボカド、納豆」
出力形式:
- 料理名: [料理名]
- 使用する材料: [材料リスト]  
- レシピ本名: [レシピ本名]

複数の候補がある場合は、最大3つまで提案してください。
```

## メモ
エージェントも簡単に作れるんかい。
![[スクリーンショット 2025-09-06 0.26.26.png]]
ここで RAG の情報を登録できる。
![[スクリーンショット 2025-09-06 0.31.50.png]]

- Dify と notion の繋げ方：https://note.com/jolly_dahlia842/n/n7b8b029316cd

  - 一般市民も無料で notion が使えるのかしら。→ 使えた。

- Dify でテキストファイルからナレッジを作る方法：https://zenn.dev/headwaters/articles/e2cc40a31cdd11
  - ↑ のファイルだと CSV でやっている。
  - CSV でいけた。
  - DifyのRAGでの検索についてのドキュメント：https://docs.dify.ai/ja-jp/guides/knowledge-base/create-knowledge-and-upload-documents/setting-indexing-methods#setting-the-retrieval-setting
  - OpenAIの埋め込みモデルの比較：https://zenn.dev/headwaters/articles/ebc3086bcf1b42
	  - 使用するデータ量の少なさからtext-embedding-3-small (OpenAI)にしてみる。
- インデックスを「高品質」に変えて、ベクトル検索にしたら検索できるようになった。
	- 検索の種類を用途によって選べるようにならないと。
- ナレッジのAPIあるやん。：https://zenn.dev/solvio/articles/547db2c2acc28d
	- 公式ドキュメント：https://docs.dify.ai/ja-jp/guides/knowledge-base/knowledge-and-documents-maintenance/maintain-dataset-via-api
- 1日1回アップロードするバッチ作ろう。もしくは更新のタイミングでキックされてアップロードするやつ。