- よくECSを分かっていない。：https://zenn.dev/mi_01_24fu/articles/aws-ecs-2024_03_18

## 構築メモ
- ECRはコンテナのDockerイメージを保存するから、ECRに保存したDockerイメージをECSにデプロイする？
	- ECRとは：https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/what-is-ecr.html

## ECRにDockerイメージをpushするまで
1. AWS CLIのインストール
	1. https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html

以下コマンドでインストール
```
$ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```

2. ECR用の認可トークンを取得する？
- https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/registry_auth.html


