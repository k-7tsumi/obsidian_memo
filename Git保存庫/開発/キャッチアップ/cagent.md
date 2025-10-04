リポジトリ：https://github.com/docker/cagent
- 楽しいからなんか作ってみよう。

## なんか作る

cagentをインストール

```
# インストール
$ brew install cagent
# インストールできているかバージョン確認
$ cagent version
  
For any feedback, please visit: https://docker.qualtrics.com/jfe/form/SV_cNsCIg92nQemlfw

cagent version v1.5.8

Commit: aed9d3109963d95ced1e90b23379b22285fa334c 
```

使用するモデルの環境変数を追加。自分の場合はデフォルトシェルがbashで、OpenAIのAPIを使用するため以下を `~/.bash_profile` に設定。

```
export OPENAI_API_KEY={OPEN AI APIのkey}
```

設定したら以下コマンドで適用
```
$ source ~/.bash_profile
```

サンプルコードを試すため[cagent](https://github.com/docker/cagent)をローカル環境に持ってくる。
```
$ git clone https://github.com/docker/cagent.git
```

海賊と会話ができるテスト用エージェントを実行

```
$ cagent run ./examples/pirate.yaml 
```

参照：https://github.com/docker/cagent/blob/main/examples/pirate.yaml

実行すると以下のような感じでやり取りができるようになる。

![[スクリーンショット 2025-10-04 17.28.57.png]]

## コマンドシェルを調べられるエージェントを作成する

## 事前準備

DockerのMCP toolkitでDuckDuckGoを追加しておく。

![[スクリーンショット 2025-10-04 22.15.46 1.png]]

以下yamlファイルを用意

examples/command_helper.yaml

```
#!/usr/bin/env cagent run

version: "2"

agents:
root:
model: openai/gpt-4o

description: コマンドヘルパー - よく使うコマンドを検索・提示・実行するエージェント

instruction: |

あなたはcagentのコマンドを検索、説明、そして実行するヘルパーです。

ユーザーが「どんなコマンドがあるか」「○○するコマンド」などを聞いてきた場合、

以下のコマンドリストから適切なものを検索して提示してください。

  

ユーザーがコマンドの実行を依頼した場合は、shellツールを使って実際にコマンドを実行してください。

実行前に、どのコマンドを実行するか確認し、必要に応じて説明を加えてください。

  

コマンドリストに無い情報や、最新の情報が必要な場合は、DuckDuckGoのWeb検索機能を使って情報を検索してください。

  

## コマンドリスト

- `vi ~/.bashrc` - bashrcを開く

- `vi ~/.bash_profile` - bash_profileを開く

- `vi ~/mise.toml` - mise.tomlを開く

- `vi ~/.claude/settings.json` - ~/.claude/settings.json(ユーザー設定)を開く

  

ユーザーの質問に対して、関連するコマンドを簡潔に説明してください。

複数の関連コマンドがある場合は、すべてリストアップしてください。

コマンドの実行が必要な場合は、適切に実行してください。

toolsets:
- type: think
- type: shell
- type: mcp
  ref: docker:duckduckgo
```

実行

```
$ cagent run command_helper.yaml
```