- リポジトリあった。：[https://github.com/masamasa59/genai-agent-advanced-book](https://github.com/masamasa59/genai-agent-advanced-book)
- Plan-and-Execute型エージェント
	- LLMに直接質問を入力して回答を得るのではなく、まず問題をいくつかの小さな問題に分割(計画)し、それぞれを個別に解決(行動)していくことで最終的な回答を得る方法です。
	- Plan-and-Execute型のエージェントは、最初に計画を立てて複雑なタスクを実行可能な小さなサブタスクに分解する考え方を示しているだけであるということです。

- **uv**は、PythonとPyPIパッケージ管理のための**極めて高速な新しいツール**です。Rust言語で書かれており、従来のpipやpip-toolsを大幅に高速化することを目的としています。
	- https://github.com/astral-sh/uv
- Qdrant(クワッドラント)とは：https://zenn.dev/tfutada/articles/acf8adbb2ba5be
	- オープンソースのベクトル検索エンジン (Rust実装)
- チャンク分割とは：https://promo.digital.ricoh.com/ai-for-work/column/detail012/
- pyprojectとは: https://packaging.python.org/ja/latest/guides/writing-pyproject-toml/
- chapter4/notebooks/tools.ipynbは何するの。
	- Jupyter notebookの機能らしい：https://www.internetacademy.co.jp/itlab/column-technology/python_stydy04.html
- そうなんだ。
```
> ipynbという拡張子はなんですか？
⏺ .ipynbは**IPython Notebook**の略で、Jupyter Notebookファイルの拡張子です。
```
- ipykernelとは：https://docs.kanaries.net/ja/topics/Python/ipykernel

以下でchapter4/notebooks/tools.ipynbを実行できた。
```
$uv run python -c "from src.tools.search_xyz_manual import search_xyz_manual; result = search_xyz_manual.invoke('オンラインヘルプセンター'); print(result[0].content)"
```

- RAGとロングコンテキストの比較：https://ledge.ai/articles/google_deepmind_lc-vs-rag
- ロングコンテキストLLMとは：https://moji-inc.com/articles/long-context-llm
	- たとえば、長い契約書の内容を一度に要約する場合、ロングコンテキストLLMでは全内容を一度に解析できますが、RAGでは契約書を分割して検索可能なチャンク（小単位）に分け、それを逐次的に参照します。この違いにより、ロングコンテキストLLMは情報の整合性が高い出力を期待できますが、大規模データの外部管理に優れたRAGと組み合わせて使うことで、さらに高度なシステムを構築できます。
- Pythonのimport：https://note.nkmk.me/python-import-usage/
- Pythonの型ヒント：https://qiita.com/shun_sakamoto/items/65edc36e7f5a6f9d52b8
- Microsoftの研究者らが提案した質問レベルの分類：https://arxiv.org/abs/2409.14924v1