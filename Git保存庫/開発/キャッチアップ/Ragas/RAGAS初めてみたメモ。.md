- RAGの評価ツール
- 概要：https://qiita.com/ist-i-j/items/13496a07e14a92e898fe

## Introduction

> Ragasは、大規模言語モデル（LLM）アプリケーションの評価を強化するツールを提供するライブラリです。LLMアプリケーションを容易かつ確信を持って評価できるよう設計されています。


## チュートリアルやってみる

```
$ uv init test-ragas-project
```

ragas、langchain-openaiを追加

```
$ uv add ragas langchain-openai
```

main.pyを以下のように設定

main.py
```
from langchain_openai import ChatOpenAI
from ragas.embeddings import OpenAIEmbeddings
import openai

def main():
	llm = ChatOpenAI(model="gpt-4o")
	openai_client = openai.OpenAI()
	embeddings = OpenAIEmbeddings(client=openai_client)

if __name__ == "__main__":
	main()
```

src/rag.py
```
import numpy as np
class RAG:
	def __init__(self, model="gpt-4o"):
		import openai
		self.llm = ChatOpenAI(model=model)
		openai_client = openai.OpenAI()
		self.embeddings = OpenAIEmbeddings(client=openai_client)
		self.doc_embeddings = None
		self.docs = None
		
	def load_documents(self, documents):
		"""Load documents and compute their embeddings."""
		self.docs = documents
		self.doc_embeddings = self.embeddings.embed_texts(documents)

	def get_most_relevant_docs(self, query):
		"""Find the most relevant document for a given query."""
		if not self.docs or not self.doc_embeddings:
		raise ValueError("Documents and their embeddings are not loaded.")

	query_embedding = self.embeddings.embed_text(query)
	similarities = [
		np.dot(query_embedding, doc_emb)
		/ (np.linalg.norm(query_embedding) * np.linalg.norm(doc_emb))
		for doc_emb in self.doc_embeddings
	]

	most_relevant_doc_index = np.argmax(similarities)
	return [self.docs[most_relevant_doc_index]]
	
	def generate_answer(self, query, relevant_doc):
		"""Generate an answer for a given query based on the most relevant document."""
		prompt = f"question: {query}\n\nDocuments: {relevant_doc}"
		messages = [
		("system", "You are a helpful assistant that answers questions based on given documents only."),
		("human", prompt),
		]
		ai_msg = self.llm.invoke(messages)
		return ai_msg.content
```

## https://docs.ragas.io/en/stable/getstarted/rag_eval/ の翻訳

### シンプルなRAGシステムを評価する

このガイドの目的は、RAGシステムをRAGSでテストおよび評価するためのシンプルなワークフローを説明することです。RAGシステムの構築と評価に関する最低限の知識があることを前提としています。RAGSのインストールについては、当社のインストール手順を参照してください。

#### 基本設定

langchain_openaiを使用して、シンプルなRAG構築のためのLLMと埋め込みモデルを設定します。他のLLMや埋め込みモデルを選択することも可能です。その場合は、langchainにおけるモデルの
カスタマイズを参照してください。

```
from langchain_openai import ChatOpenAI
from ragas.embeddings import OpenAIEmbeddings
import openai

llm = ChatOpenAI(model="gpt-4o")
openai_client = openai.OpenAI()
embeddings = OpenAIEmbeddings(client=openai_client)
```

##### OpenAI Embeddings API
ragas.embeddings.OpenAIEmbeddings は、一部の LangChain ラッパーのような embed_query/embed_documents ではなく、embed_text (単一) と embed_texts (バッチ) を公開します。以下の例では、ドキュメントにはembed_texts、クエリにはembed_textを使用しています。OpenAIの埋め込み実装を参照してください。

#### シンプルなRAGシステムを構築する
シンプルな RAG システムを構築するには、次のコンポーネントを定義する必要があります。

- ドキュメントをベクトル化するメソッドを定義する 
- 関連ドキュメントを取得するメソッドを定義する 
- レスポンスを生成するメソッドを定義する

##### ドキュメントを読み込む

それでは、いくつかのドキュメントを読み込んで、RAG システムをテストしてみましょう。

```
sample_docs = [
    "Albert Einstein proposed the theory of relativity, which transformed our understanding of time, space, and gravity.",
    "Marie Curie was a physicist and chemist who conducted pioneering research on radioactivity and won two Nobel Prizes.",
    "Isaac Newton formulated the laws of motion and universal gravitation, laying the foundation for classical mechanics.",
    "Charles Darwin introduced the theory of evolution by natural selection in his book 'On the Origin of Species'.",
    "Ada Lovelace is regarded as the first computer programmer for her work on Charles Babbage's early mechanical computer, the Analytical Engine."
]
```

##### 評価データを収集する
評価データを収集するには、まずRAGに対して実行するクエリのセットが必要です。これらのクエリをRAGシステムで実行し、各クエリに対する応答と取得されたコンテキストを収集します。システムの性能を評価するために、各クエリに対するゴールデンアンサーのセットを任意で準備することも可能です。

##### 評価する
評価データの収集に成功しました。収集したデータセットを用いて、一般的に使用されるRAG評価指標を用いてRAGシステムを評価できます。評価には、評価者 LLM として任意のモデルを選択できます。

##### evals を使用して AI アプリケーションを改善する際にサポートが必要ですか?
過去2年間、私たちはevalを用いた多くのAIアプリケーションを検証し、改善を支援してきました。
この知見を結集し、vibeチェックをevalループに置き換える製品を開発中です。これにより、優れたAIアプリケーション構築に集中していただけます。
evalを活用したAIアプリケーションの改善・スケールアップ支援をご希望の場合：
🔗 予約またはお問い合わせはこちら：founders@explodinggradients.com


## https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/ の翻訳

### 利用可能な指標のリスト

Ragasは、LLMアプリケーションのパフォーマンスを測定するために使用できる一連の評価指標を提供します。これらの指標は、アプリケーションのパフォーマンスを客観的に測定するのに役立つように設計されています。RAGやエージェント型ワークフローなど、さまざまなアプリケーションやタスク向けの指標が用意されています。

各指標は本質的に、アプリケーションの特定の側面を評価するために設計されたパラダイムです。LLMベースの指標は、スコアや結果を得るために1つ以上のLLM呼び出しを使用する場合があります。また、Ragasを使用して独自の指標を修正または作成することも可能です。