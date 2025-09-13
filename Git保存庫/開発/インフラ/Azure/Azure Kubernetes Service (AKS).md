- https://azure.microsoft.com/ja-jp/products/kubernetes-service
- ドキュメント：https://learn.microsoft.com/ja-jp/azure/aks/what-is-aks
- ”Kubernetes” の基本を徹底解説：https://qiita.com/tadashiro_ninomiya/items/6e6fea807b2a16732b5b

## TODO

- なんか作ってみる。
  - これで学習してみる？：https://learn.microsoft.com/ja-jp/training/paths/intro-to-kubernetes-on-azure/

## 学習トレーニングを進める。

- https://learn.microsoft.com/ja-jp/training/paths/intro-to-kubernetes-on-azure/

- デーモンとは：https://qiita.com/yuta_931214/items/19ffad86cdfdd5bf37dc
- コンテナー OS は、ホスト OS から分離されている、アプリケーションをデプロイして実行する環境です。
- scratch とは、超最小化されたベースイメージのよう。
- apt とは：https://weblabo.oscasierra.net/ubuntu-apt/
- コンテナーでは、データを永続化するために 2 つのオプションを使用できます。 1 つ目のオプションは "_ボリューム_" を使用するもので、2 つ目のオプションは "_バインド マウント_" です。
- Docker のボリュームとは：https://qiita.com/gounx2/items/23b0dc8b8b95cc629f32
- コンテナのポートを公開するには、docker run コマンドで  `--publish`  フラグ（短縮形は  `-p` ） を使います。 `--publish`  命令は  `[host port]:[container port]`  形式です。そのため、コンテナ外のポート 3000 に コンテナ内のポート 8000 を公開するには、 --publish フラグに 3000:8000 を渡します。
  - https://docs.docker.jp/language/nodejs/run-containers.html
- コンテナをデプロイできる azure のサービス：Azure Container Instances、Azure App Service、Azure Kubernetes Service

- 次はここから：https://learn.microsoft.com/ja-jp/training/modules/intro-to-containers/4-create-custom-docker-image
