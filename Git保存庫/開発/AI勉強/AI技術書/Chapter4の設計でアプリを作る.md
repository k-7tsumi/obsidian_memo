## 計画

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
