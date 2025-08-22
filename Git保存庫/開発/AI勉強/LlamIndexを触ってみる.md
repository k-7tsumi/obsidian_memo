- LlamaIndexとは：
	- RAGシステムが構築できるフレームワーク。PythonとTypeScriptが使える。
	- ドキュメント：https://docs.llamaindex.ai/en/stable/
	- https://zenn.dev/nomhiro/articles/llama-index-abstract

## チュートリアルをやってみる

- introduction：https://docs.llamaindex.ai/en/stable/#introduction
- チュートリアル：https://docs.llamaindex.ai/en/stable/getting_started/starter_example/

1. 以下コマンドでllama-indexをインストールする

```
pip install llama-index
```

2. OpenAI API keyを取得しセットする

自分の場合~/.bash_profileに設定
```
export OPENAI_API_KEY=XXXXX
```

3. 以下スクリプトを実行(Python)
```
import asyncio
from llama_index.core.agent.workflow import FunctionAgent
from llama_index.llms.openai import OpenAI


# Define a simple calculator tool
def multiply(a: float, b: float) -> float:
    """Useful for multiplying two numbers."""
    return a * b


# Create an agent workflow with our calculator tool
agent = FunctionAgent(
    tools=[multiply],
    llm=OpenAI(model="gpt-4o-mini"),
    system_prompt="You are a helpful assistant that can multiply two numbers.",
)


async def main():
    # Run the agent
    response = await agent.run("What is 1234 * 4567?")
    print(str(response))


# Run the agent
if __name__ == "__main__":
    asyncio.run(main())
```

コンテキストを追加したバージョン

```
import asyncio
from llama_index.core.agent.workflow import FunctionAgent
from llama_index.core.workflow import Context
from llama_index.llms.openai import OpenAI

# Define a simple calculator tool
def multiply(a: float, b: float) -> float:
    “”"Useful for multiplying two numbers.“”"
    return a * b

# Create an agent workflow with our calculator tool
agent = FunctionAgent(
    tools=[multiply],
    llm=OpenAI(model=“gpt-4o-mini”),
    system_prompt=“You are a helpful assistant that can multiply two numbers.“,
)

async def main():
    # Create context
    ctx = Context(agent)

    # Set up context with user information
    response = await agent.run(“My name is Logan and I prefer calculations in scientific notation.“, ctx=ctx)
    print(“Setting up context:“)
    print(str(response))
    print(“\n” + “=”*50 + “\n”)

    # Test with context - should remember preference and name
    response = await agent.run(“What is 1234 * 4567? Please format according to my preference.“, ctx=ctx)
    print(“With context (remembers name and preference):“)
    print(str(response))
    print(“\n” + “=”*50 + “\n”)

    # Test without context for comparison
    response_no_context = await agent.run(“What is 1234 * 4567? Please format according to my preference.“)
    print(“Without context (no memory of preference):“)
    print(str(response_no_context))

# Run the agent
if __name__ == “__main__“:
    asyncio.run(main())
```