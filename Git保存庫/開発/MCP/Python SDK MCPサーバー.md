
これをやっていく。：https://github.com/modelcontextprotocol/python-sdk

1. uvをbrewでインストール

```
$brew install uv
```

```
$uv --version

uv 0.8.3 (Homebrew 2025-07-24)
```

2. プロジェクトを追加
```
uv init mcp-server-demo
cd mcp-server-demo
```

3. `mcp[cli]`を追加

```
$uv add "mcp[cli]"

Using CPython 3.10.18
Creating virtual environment at: .venv
Resolved **36 packages** in 363ms
Prepared **33 packages** in 473ms
Installed **33 packages** in 42ms
 + **annotated-types**==0.7.0
 + **anyio**==4.9.0
 + **attrs**==25.3.0
 + **certifi**==2025.7.14
 + **click**==8.2.1
 + **exceptiongroup**==1.3.0
 + **h11**==0.16.0
 + **httpcore**==1.0.9
 + **httpx**==0.28.1
 + **httpx-sse**==0.4.1
 + **idna**==3.10
 + **jsonschema**==4.25.0
 + **jsonschema-specifications**==2025.4.1
 + **markdown-it-py**==3.0.0
 + **mcp**==1.12.2
 + **mdurl**==0.1.2
 + **pydantic**==2.11.7
 + **pydantic-core**==2.33.2
 + **pydantic-settings**==2.10.1
 + **pygments**==2.19.2
 + **python-dotenv**==1.1.1
 + **python-multipart**==0.0.20
 + **referencing**==0.36.2
 + **rich**==14.1.0
 + **rpds-py**==0.26.0
 + **shellingham**==1.5.4
 + **sniffio**==1.3.1
 + **sse-starlette**==3.0.2
 + **starlette**==0.47.2
 + **typer**==0.16.0
 + **typing-extensions**==4.14.1
 + **typing-inspection**==0.4.1
 + **uvicorn**==0.35.0
```
4. 以下を mcp-server-demo に追加


server.py
```
"""
FastMCP quickstart example.

cd to the `examples/snippets/clients` directory and run:
uv run server fastmcp_quickstart stdio
"""

from mcp.server.fastmcp import FastMCP
# Create an MCP server
mcp = FastMCP("Demo")

# Add an addition tool

@mcp.tool()

def add(a: int, b: int) -> int:

"""Add two numbers"""

return a + b

# Add a dynamic greeting resource

@mcp.resource("greeting://{name}")

def get_greeting(name: str) -> str:

"""Get a personalized greeting"""

return f"Hello, {name}!"

# Add a prompt
@mcp.prompt()
def greet_user(name: str, style: str = "friendly") -> str:

"""Generate a greeting prompt"""
styles = {
"friendly": "Please write a warm, friendly greeting",
"formal": "Please write a formal, professional greeting",
"casual": "Please write a casual, relaxed greeting",
}

return f"{styles.get(style, styles['friendly'])} for someone named {name}."
```

5. 以下を実行してClaude Desktopに追加

```
uv run mcp install server.py
```

なるほど

![[スクリーンショット 2025-07-28 18.13.36.png]]