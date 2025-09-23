## 計画
Chapter4を読んでざっくり計画
1. 計画を実行する関数を作成する。
	1. 計画自体をAIに立てさせる。

## メモ
- Plan-and-Execute型エージェントで作る。
![[IMG_1184.jpg]]
- 料理レシピのレコメンドを、Elastichsearchで構築してみるのもいいのか。
- LangGraphも触ってはみたい。
	- https://www.langchain.com/langgraph
- ディレクトリ名はrecipe_recommendation_system
- pyperojectのドキュメント： https://packaging.python.org/ja/latest/guides/writing-pyproject-toml/
- まずはPythonのScriptでLangChainを動かす。
- LangChainでのOpenAIの各インストール方法：https://python.langchain.com/docs/integrations/providers/openai/
- これを一旦動かそう。(AIエージェント)
	- https://python.langchain.com/docs/tutorials/agents/

- 次はここを見ながらOpenAI APIを実行できるようにする。それからAIエージェント作る。：https://python.langchain.com/docs/integrations/chat/openai/
	- 一旦でけた。

```
import getpass
import os
from langchain.chat_models import init_chat_model
from langchain_core.messages import HumanMessage, SystemMessage  

model = init_chat_model("gpt-4o-mini", model_provider="openai")
messages = [
	SystemMessage(content="入力された日本語を英語に訳してください"),
	HumanMessage(content="今日の夜ご飯は秋刀魚の蒲焼です。"),
] 

response = model.invoke(messages)
  
# レスポンスを表示
print(response)
```
- $uv run python main.pyのような感じで実行すると、uv経由で仮想環境で実行できる。
- langchain_tavilyとは：https://python.langchain.com/docs/integrations/tools/tavily_search/
	- Tavily は、主に大規模言語モデル（LLM）や AI エージェント向けに設計された**リアルタイム Web 検索エンジン**です。
	- 【保存版】Tavily Search API 導入で変わる AI アプリケーション開発：https://qiita.com/syukan3/items/3011fd487fa2a4343b4a
- [LangGraph] CheckpointとStoreの使い方：https://zenn.dev/pharmax/articles/26be245e159590
- LangGraphは、ステートマシンを作成し、複数のエージェントが協調して動作する「マルチエージェント」の構築を容易にすることを目的に開発されています。
	- https://zenn.dev/pharmax/articles/8796b892eed183
- 状態を保存・復元するだけではなく、Checkpointerを設定している会話(スレッド)中はその内容は各LLM実行に引き継がれています。同じスレッド内では前のLLM実行の内容を記憶した状態で、次のLLMの実行を行うことが可能です。
- ReAct（Reasoning and Acting）パターンとは。。
	- ReActは「Reasoning and Acting」の略で、大言語モデル（LLM）が思考（Thought）→行動（Action）→観察（Observation）のサイクルを繰り返すことで、複雑なタスクを段階的に解決するパターンです。
	- https://qiita.com/yukiaprogramming/items/6599fdcf284e5481ecc4
- LangGraphのcreate_react_agentの公式ドキュメント：https://python.langchain.com/api_reference/langchain/agents/langchain.agents.react.agent.create_react_agent.html
- pydantic_settingsとは
	- https://docs.pydantic.dev/latest/concepts/pydantic_settings/
	- https://qiita.com/inetcpl/items/b4146b9e8e1adad239d8
	- Pydantic Settingsは、[Pydantic](https://docs.pydantic.dev/latest/)ライブラリの一部であり、設定管理を型安全かつ簡潔に行うための強力なツールです。

次はここから：技術書読みながら、AIエージェントの作り方を学ぶ。