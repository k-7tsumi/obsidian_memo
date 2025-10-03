- 何作ろう。
- はじめての Dify × RAG：https://qiita.com/tanai3_r/items/cbf8a3b6aaeeea8d8350
- ↑ を参照してチャットボットを作成してみる。

## ざっくり設計
- チャットボット
- 料理名、材料名、レシピURLのデータがあるCSVファイルをRAGとして読み込ませる。
- Claudeが書いてくれたプロンプト
```
あなたは料理レシピ推薦アシスタントです。
ユーザーから材料名を受け取り、アップロードされたレシピデータベースから最適なレシピを推薦してください。

入力例: 「玉ねぎ」「アボカド、納豆」
出力形式:
- 料理名: [料理名]
- 使用する材料: [材料リスト]  
- レシピ本名: [レシピ本名]

複数の候補がある場合は、最大3つまで提案してください。
```

### データ更新方法
- Slackのなにかしらの方法で、料理名・使用する材料・レシピ本名の3つを登録し、DifyのナレッジAPIで登録する。
- Slack → Googleスプレッドシート → Dify の流れで良さそう。
- バッチではなく、登録時随時で良さそう。(GASでシートに入力あったらキックで良さそう。)
	- onEdit()不具合多そうなので、3時間に1回くらいのバッチにする。
### TODO
- コミュニティ版のDifyをAzure Kubernetes Serviceにデプロイして使えるようにする。
- ↑のデータ更新方法でRAGアプリ作る

## メモ
エージェントも簡単に作れるんかい。
![[スクリーンショット 2025-09-06 0.26.26.png]]
ここで RAG の情報を登録できる。
![[スクリーンショット 2025-09-06 0.31.50.png]]

- Dify と notion の繋げ方：https://note.com/jolly_dahlia842/n/n7b8b029316cd

  - 一般市民も無料で notion が使えるのかしら。→ 使えた。

- Dify でテキストファイルからナレッジを作る方法：https://zenn.dev/headwaters/articles/e2cc40a31cdd11
  - ↑ のファイルだと CSV でやっている。
  - CSV でいけた。
  - DifyのRAGでの検索についてのドキュメント：https://docs.dify.ai/ja-jp/guides/knowledge-base/create-knowledge-and-upload-documents/setting-indexing-methods#setting-the-retrieval-setting
  - OpenAIの埋め込みモデルの比較：https://zenn.dev/headwaters/articles/ebc3086bcf1b42
	  - 使用するデータ量の少なさからtext-embedding-3-small (OpenAI)にしてみる。
- インデックスを「高品質」に変えて、ベクトル検索にしたら検索できるようになった。
	- 検索の種類を用途によって選べるようにならないと。
- ナレッジのAPIあるやん。：https://zenn.dev/solvio/articles/547db2c2acc28d
	- 公式ドキュメント：https://docs.dify.ai/ja-jp/guides/knowledge-base/knowledge-and-documents-maintenance/maintain-dataset-via-api
- 1日1回アップロードするバッチ作ろう。もしくは更新のタイミングでキックされてアップロードするやつ。
- アプリ内でのナレッジベース統合：https://docs.dify.ai/ja-jp/guides/knowledge-base/integrate-knowledge-within-application
- 【非エンジニア向け】Difyナレッジ⇔ Googleスプレッドシートの自動連携：https://note.com/sasuu/n/n4f8d39694fb0
- Googleスプレッドシート → Dify の部分を進める。
- データを追加していくGoogleスプレッドシート：https://docs.google.com/spreadsheets/d/1JN-2aSbO8i8cO69gmA60dtyq44iX1i45eUMC7_oKyFE/edit?hl=ja&gid=0#gid=0
- GASのトリガー：https://developers.google.com/apps-script/guides/triggers?hl=ja
- Difyのセルフホストサービス：https://dev.classmethod.jp/articles/dify-self-hosting-aws/
- ボタン一発で！ーAzure × Dify でノーコード AI エージェント開発環境をクラウドに構築：https://note.com/sasaki_akio/n/n6af6b1a97341
- Difyで作ったLLM ApplicationをAzure Kubernetes Serviceにデプロイする方法：https://zenn.dev/microsoft/articles/dify_on_azure
	- ↑これやってみよう。
- Kubernetes における Helm とは？：https://sysdig.jp/learn-cloud-native/what-is-helm-in-kubernetes/
- kubectlとは：https://kubernetes.io/ja/docs/reference/kubectl/
- az aks コマンド：https://learn.microsoft.com/ja-jp/cli/azure/aks?view=azure-cli-latest
- Azure Kubernetes Service (AKS) クラスター管理の Free、Standard、Premium 価格レベル：https://learn.microsoft.com/ja-jp/azure/aks/free-standard-pricing-tiers
- Ingress機能とは：https://kubernetes.io/ja/docs/concepts/services-networking/ingress/

次はここから(まずは、以下のコマンドでクラスターを作成します。)：https://zenn.dev/microsoft/articles/dify_on_azure#azure-kubernetes-service-%E3%81%B8-deploy

- 先に以下解決する。

```
$az aks create --resource-group katakura_resource_group2 --name aks-poc-dify-app --tier free --node-count 1 --node-vm-size Standard_B2s --priority Spot --evction-policy Delete --spot-max-price -1 --enable-addons http_application_routing --generate-ssh-keys 

unrecognized arguments: --priority Spot --eviction-policy Delete --spot-max-price -1 Examples from AI knowledge base: az aks create --resource-group MyResourceGroup --name MyManagedCluster Create a kubernetes cluster with default kubernetes version, default SKU load balancer (Standard) and default vm set type (VirtualMachineScaleSets). az aks create --resource-group MyResourceGroup --name MyManagedCluster --enable-managed-identity Create a kubernetes cluster which enables managed identity. az extension add --name anextension Add extension by name 

[https://docs.microsoft.com/en-US/cli/azure/aks#az_aks_create](https://docs.microsoft.com/en-US/cli/azure/aks#az_aks_create) Read more about the command in reference docs
```
↑で怒られているオプションを外して実行

```
$az aks create --resource-group katakura_resource_group2 --name dify-app --tier free --node-count 1 --node-vm-size Standard_B2 --enable-addons http_application_routing --generate-ssh-keys

(BadRequest) Client Error: The HTTPApplicationRouting add-on can no longer be enabled on new clusters. Please use the Application Routing Add-on instead: https://aka.ms/app-routing-migration

Code: BadRequest

Message: Client Error: The HTTPApplicationRouting add-on can no longer be enabled on new clusters. Please use the Application Routing Add-on instead: https://aka.ms/app-routing-migration
```

- HTTP アプリケーション ルーティング アドオンは廃止されたから、--enable-addons http_application_routing のオプションはつけられない。
	- https://learn.microsoft.com/ja-jp/azure/aks/app-routing-migration
	- --enable-app-routingに変更すれば良いみたい。： https://learn.microsoft.com/ja-jp/azure/aks/app-routing
- Kubernetesのイングレスのドキュメント： https://kubernetes.io/docs/concepts/services-networking/ingress/
- Kubernetes Ingress完全に理解した：https://zenn.dev/yusu29/articles/kubernetes_ingress

以下のコマンドでkuberneterクラスター作成成功
```
$az aks create --resource-group katakura_resource_group2 --name dify-app --tier free --node-count 1 --node-vm-size Standard_B2 --enable-app-routing --generate-ssh-keys
```

![[スクリーンショット 2025-09-20 15.03.40.png]]

以下コマンドでACR作成成功
```
$az acr create --resource-group katakura_resource_group2 --name difyapp --sku Bsic

{
  "adminUserEnabled": false,
  "anonymousPullEnabled": false,
  "autoGeneratedDomainNameLabelScope": "Unsecure",
  "creationDate": "2025-09-20T06:14:24.720988+00:00",
  "dataEndpointEnabled": false,
  "dataEndpointHostNames": [],
  "encryption": {
    "keyVaultProperties": null,
    "status": "disabled"
  },
  "id": "/subscriptions/b3c720dd-5446-4d55-ac42-013e165e638a/resourceGroups/katakura_resource_group2/providers/Microsoft.ContainerRegistry/registries/difyapp",
  "identity": null,
  "location": "eastasia",
  "loginServer": "difyapp.azurecr.io",
  "metadataSearch": "Disabled",
  "name": "difyapp",
  "networkRuleBypassOptions": "AzureServices",
  "networkRuleSet": null,
  "policies": {
    "azureAdAuthenticationAsArmPolicy": {
      "status": "enabled"
    },
    "exportPolicy": {
      "status": "enabled"
    },
    "quarantinePolicy": {
      "status": "disabled"
    },
    "retentionPolicy": {
      "days": 7,
      "lastUpdatedTime": "2025-09-20T06:14:32.560656+00:00",
      "status": "disabled"
    },
    "softDeletePolicy": {
      "lastUpdatedTime": "2025-09-20T06:14:32.560706+00:00",
      "retentionDays": 7,
      "status": "disabled"
    },
    "trustPolicy": {
      "status": "disabled",
      "type": "Notary"
    }
  },
  "privateEndpointConnctions": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "katakura_resource_group2",
  "roleAssignmentMode": "LegacyRegistryPermissions",
  "sku": 
    "name": "Basic",
    "tier": "Basic"
  }
  "status": null,
  "systemData": {
    "createdAt": "2025-09-20T06:14:24.720988+00:00",
    "createdBy": "koohiiko77@gmail.com",
    "createdByType": "User",
    "lastModifiedAt": "2025-09-20T06:14:24.720988+00:00",
    "lastModifiedBy": "koohiiko77@gmail.com",
    "lastModifiedByType": "User"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries",
  "zoneRedundancy": "Disabled"
}
```

- Difyのリリースノート：https://github.com/langgenius/dify/releases
- Docker hubにDifyのイメージが置いてある。
	- https://hub.docker.com/r/langgenius/dify-web
	- https://hub.docker.com/r/langgenius/dify-api
- 何のページ。。？：https://douban.github.io/charts/
	- https://github.com/douban/charts
- [Ingress](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.34/#ingress-v1-networking-k8s-io)はクラスター外からクラスター内[Service](https://kubernetes.io/ja/docs/concepts/services-networking/service/)へのHTTPとHTTPSのルートを公開します。トラフィックのルーティングはIngressリソース上で定義されるルールによって制御されます。
- Kubernetesの公式ドキュメント：https://kubernetes.io/ja/docs/concepts/services-networking/ingress/
- AKSのドキュメント：https://learn.microsoft.com/ja-jp/azure/aks/concepts-network-ingress
- macOS上でのkubectlのインストールおよびセットアップ：https://kubernetes.io/ja/docs/tasks/tools/install-kubectl-macos/
- KubernetesクライアントkubectlからAKSに接続手順について: https://qiita.com/okcoder/items/e94a6463bc8d80cc0252

クラスター認証情報の取得が必要だったぽい。

```
$az aks get-credentials --resource-group katakura_resource_group2 --name dify-app

Merged "dify-app" as current context in /Users/katakuranatsumi/.kube/config
```

AKSに接続できた。
```
$kubectl cluster-info

Kubernetes control plane is running at https://dify-app-katakuraresource-b3c720-4r1j4pv1.hcp.eastasia.azmk8s.io:443

CoreDNS is running at https://dify-app-katakuraresource-b3c720-4r1j4pv1.hcp.eastasia.azmk8s.io:443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

Metrics-server is running at https://dify-app-katakuraresource-b3c720-4r1j4pv1.hcp.eastasia.azmk8s.io:443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

以下を実行したら色々入った。
```
$ helm upgrade dify douban/dify -f values.yaml --install --debug
```

![[スクリーンショット 2025-09-23 16.12.51.png]]

```
$kubectl get services -n default

NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)             AGE
dify-api-svc          ClusterIP   10.0.236.88    <none>        80/TCP              4m18s
dify-frontend         ClusterIP   10.0.151.39    <none>        80/TCP              4m18s
dify-minio            ClusterIP   10.0.138.60    <none>        9000/TCP,9001/TCP   4m18s
dify-plugin-daemon    ClusterIP   10.0.87.132    <none>        5002/TCP            4m18s
dify-postgresql       ClusterIP   10.0.134.221   <none>        5432/TCP            4m18s
dify-postgresql-hl    ClusterIP   None           <none>        5432/TCP            4m18s
dify-redis-headless   ClusterIP   None           <none>        6379/TCP            4m18s
dify-redis-master     ClusterIP   10.0.157.192   <none>        6379/TCP            4m18s
dify-sandbox          ClusterIP   10.0.219.40    <none>        80/TCP              4m18s
kubernetes            ClusterIP   10.0.0.1       <none>        443/TCP             3d1h
```

```
$kubectl get pods -n default

NAME                                  READY   STATUS    RESTARTS   AGE
dify-api-769fdbc4f4-p459z             1/1     Running   0          5m3s
dify-frontend-6c9c6f676d-5fmr4        1/1     Running   0          5m4s
dify-minio-587f5d9fb7-5jkx2           1/1     Running   0          5m4s
dify-plugin-daemon-744bfb4b74-zmzlk   1/1     Running   0          5m4s
dify-postgresql-0                     1/1     Running   0          5m3s
dify-redis-master-0                   1/1     Running   0          5m3s
dify-sandbox-bdcbd5ddf-dhltv          1/1     Running   0          5m3s
dify-worker-5676b6869d-fnvj7          1/1     Running   0          5m4s
```

- Azure ロールベースのアクセス制御を使用して、Azure Kubernetes Service (AKS) 内の Kubernetes 構成ファイルへのアクセス権を定義する：https://learn.microsoft.com/ja-jp/azure/aks/control-kubeconfig-access
- flaskとは：https://www.miraiserver.ne.jp/column/about_flask/
- DB migration
```
$kubectl exec -it dify-api-769fdbc4f4-p459z -- flask db upgrade
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
```
- 以下を実行
```
$kubectl apply -f dify-ingress.yaml
**Warning:** annotation "kubernetes.io/ingress.class" is deprecated, please use 'spec.ingressClassName' instead
ingress.networking.k8s.io/dify-ingress created
```
- 情報が古そうだったので、以下に修正して再度実行

dify-ingress.yaml
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dify-ingress
  annotations:
spec.ingressClassName: webapprouting.kubernetes.azure.com
```

次はここから(kubectl get pods -n defaultコマンドなどを確認してから先に進む):
- Difyで作ったLLM ApplicationをAzure Kubernetes Serviceにデプロイする方法：https://zenn.dev/microsoft/articles/dify_on_azure

画面は出たけど、インストール中。。しばらく待ってみる。

![[スクリーンショット 2025-09-24 14.02.45.png]]

- values.yamlを以下のように変えたら使えるようになった。

```
global:
  host: '20.6.176.164'
```

![[スクリーンショット 2025-09-25 14.32.46.png]]

- はじめての AKS によるサービス公開：https://qiita.com/eternity1984/items/ae6e5684fd7b02aa23e4

### Azure DNSでやったこと

#### 事前準備
- ドメイン取得(私は https://www.xserver.ne.jp/)

1. DNS ゾーンの作成
	1. インスタンスの名前には取得したドメイン名を入れる。
2. XserverのネームサーバにAzure DNSのネームサーバを設定
3. DNSゾーンにAレコードで追加
4. values.yamlを以下に変更

```
global:
  host: 'dify.katakuriko77.com'
```

変更を適用
```
$helm upgrade dify douban/dify -f values.yaml --install --debug
```
5. dify-ingress.yamlの修正

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dify-ingress
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  ingressClassName: webapprouting.kubernetes.azure.com
  rules:
    - host: dify.katakuriko77.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: dify-frontend
                port:
                  number: 80
          - path: /console/api
            pathType: Prefix
            backend:
              service:
                name: dify-api-svc
                port:
                  number: 80
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: dify-api-svc
                port:
                  number: 80
          - path: /v1
            pathType: Prefix
            backend:
              service:
                name: dify-api-svc
                port:
                  number: 80
          - path: /files
            pathType: Prefix
            backend:
              service:
                name: dify-api-svc
                port:
                  number: 80
```

dify-ingress.yamlの適用
```
$ kubectl apply -f dify-ingress.yaml
```


## !!重要!!AKS起動した後やる必要があること
1. apiの名前確認(dify-api が冒頭のものを探す)
```
$kubectl get pods --show-labels
```
1. DB migration
   
```
$ kubectl exec -it dify-api-5f88b4456b-b29xh -- flask db upgrade
```

参照サイト：
- https://zenn.dev/microsoft/articles/dify_on_azure
- https://qiita.com/okcoder/items/e94a6463bc8d80cc0252
- https://learn.microsoft.com/ja-jp/azure/aks/app-routing