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