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

【次やること】以下スクリプトで実行してみる(TypeScript)

```
import { OpenAI } from "llamaindex";
import { FunctionTool, OpenAIAgent } from "llamaindex";

// Define a simple calculator tool
const multiplyTool = new FunctionTool(
  async ({ a, b }: { a: number; b: number }): Promise<number> => {
    return a * b;
  },
  {
    name: "multiply",
    description: "Useful for multiplying two numbers.",
    parameters: {
      type: "object",
      properties: {
        a: {
          type: "number",
          description: "First number to multiply",
        },
        b: {
          type: "number",
          description: "Second number to multiply",
        },
      },
      required: ["a", "b"],
    },
  }
);

async function main(): Promise<void> {
  try {
    // Create an OpenAI LLM instance
    const llm = new OpenAI({
      model: "gpt-4o-mini",
    });

    // Create an agent with our calculator tool
    const agent = new OpenAIAgent({
      tools: [multiplyTool],
      llm: llm,
      systemPrompt: "You are a helpful assistant that can multiply two numbers.",
    });

    // Run the agent
    const response = await agent.chat({
      message: "What is 1234 * 4567?",
    });

    console.log(response.message.content);
  } catch (error) {
    console.error("Error:", error);
  }
}

// Run the agent
main().catch(console.error);
```

packege.json

```
{
  "name": "llamaindex-typescript-agent",
  "version": "1.0.0",
  "description": "LlamaIndex TypeScript agent with calculator tool",
  "main": "index.ts",
  "scripts": {
    "start": "tsx index.ts",
    "build": "tsc",
    "dev": "tsx watch index.ts"
  },
  "dependencies": {
    "llamaindex": "^0.6.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "tsx": "^4.0.0",
    "typescript": "^5.0.0"
  },
  "type": "module"
}
```