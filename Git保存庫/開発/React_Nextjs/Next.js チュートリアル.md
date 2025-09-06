- ドキュメント：https://nextjsjp.org
- Reactの復習をする場合：https://ja.nextjs.im/learn/react-foundations
## メモ
### React基礎
- App RouterとPages Routerの2種類のルーターがあります。
	- 違いが全く分からん。
- Node.jsのバージョンOK(Reactのチュートリアルやるのに。)
```
$node --version
v22.16.0
```
- Next.js は、ウェブアプリケーションを作成するための構成要素を提供する React **フレームワーク** です。
- これは、HTMLが**初期ページコンテンツ**を表すのに対し、DOMは記述したJavaScriptコードによって変更された**更新されたページコンテンツ**を表すためです。
- reactは React のコアライブラリです。
- **react-dom** は、React を DOM と一緒に使用できるようにする DOM 固有のメソッドを提供します。
- src属性とは：https://e-words.jp/w/src%E5%B1%9E%E6%80%A7.html
- createRoot：https://react.dev/reference/react-dom/client/createRoot
- 本当にエラー出た。Uncaught SyntaxError: Unexpected token '<'
- jsxの3つのルール：https://ja.react.dev/learn/writing-markup-with-jsx#the-rules-of-jsx
- Reactコンポーネントは通常のHTMLタグと同じように、山括弧`<>`を使って使用します。
- プロパティ = 属性と暫定で考える。
- アロー関数とそうでないものの特性の違いを忘れた。調べよう。
	- アロー関数式を使えば、「function」を書かなくて済むため、今までより短く関数を記述することができます。
	- 結論を言いますと、アロー関数式で宣言された関数は、宣言された時点で、thisを確定（＝束縛）させてしまうのです。
	- https://qiita.com/mejileben/items/69e5facdb60781927929
	- https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions
- bind関数ものちのち調べよう。
- keyの警告。Warning: Each child in a list should have a unique "key" prop.
- フックについて：https://ja.react.dev/learn#using-hooks
- useState：https://ja.react.dev/reference/react/useState
- コンポーネントのトップ部分：「トップレベル」= コンポーネント関数の中で、条件やループに囲まれていない直接実行される部分
- package.jsonとは：https://qiita.com/xxseagullxx/items/4e1a0158028cf2348d21
- jsxとは：https://zenn.dev/sanpi34/articles/9b99b81216b585
- Next.jsはファイルシステムベースのルーティングを使用します。これは、アプリケーションのルートをコードで定義する代わりに、フォルダとファイルを使用できることを意味します。
- `export default`を追加して、Next.jsがページのメインコンポーネントとしてレンダリングするコンポーネントを識別できるようにします。
	- `export default` について： https://qiita.com/gnash/items/ce9265751067c33167c6

エラー

![[スクリーンショット 2025-09-03 14.22.11.png]]
- これは、Next.jsがReactサーバーコンポーネントを使用しているためです。サーバーコンポーネントは`useState`をサポートしていないため、代わりにクライアントコンポーネントを使用する必要があります。

- Next.jsは**App Router**（バージョン13以降のデフォルト）を使用している場合、必要な基本ファイルが存在しないと、開発サーバー起動時に**自動的に作成**します。(next devコマンド実行時)

### サーバーコンポーネントとクライアントコンポーネント
- インタラクティブとは、双方向性。
- Reactでは、コンポーネントツリー内のどこにネットワーク境界を配置するかを選択できます。
- 全然分からん。知らん情報が出てくる。
- Reactサーバーコンポーネントペイロード（RSC）とは：https://ja.nextjs.im/learn/react-foundations/server-and-client-components#server-and-client-environments
- Next.jsはデフォルトでサーバーコンポーネントを使用します。（そうなんだ。）
- ファイルの先頭にReactの`'use client'`ディレクティブを追加します。これにより、Reactはこのコンポーネントをクライアント上でレンダリングします。
- use clientとは：https://ja.nextjs.im/docs/app/api-reference/directives/use-client
- Claudeに聞いたサーバーコンポーネントとクライアントコンポーネントを分けるメリット

```
## なぜ分ける必要があるのか？

**パフォーマンスの最適化**

- 静的なコンテンツはサーバーで処理して初期表示を高速化
- 動的な部分のみクライアント側で処理してインタラクション性を保つ
```
- First Reflesh：https://ja.nextjs.im/docs/architecture/fast-refresh

### App Router
- AppRouterチュートリアル： https://ja.nextjs.im/learn/dashboard-app
- pnpmとは：https://zenn.dev/cloud_ace/articles/articlejs-package-manager-pnpm
- npmとnpxの違い：https://qiita.com/kohta9521/items/ee3ed4a2360add80ad79#npx

以下コマンドを実行
```
$npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
```

- プロジェクトのパッケージをインストールするには`pnpm i`を実行します。
- next dev --turbopackは、**Next.jsの開発サーバーをTurbopack（新しい高速バンドラー）で起動**するコマンド。(デフォルトはwebpack)

次はここから：https://ja.nextjs.im/learn/dashboard-app/css-styling