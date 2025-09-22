## メモ
- 公式：https://ja.vite.dev/
- 記事：https://zenn.dev/comm_vue_nuxt/articles/what-is-vite
- Vite + AWS Lambda + AWS Amplify + AgentCore で簡単なAIチャットアプリを作ってみる：https://qiita.com/Takenoko4594/items/d655aff133ba6ce668e2
- stands agentとは：https://aws.amazon.com/jp/blogs/news/introducing-strands-agents-an-open-source-ai-agents-sdk/
	- https://strandsagents.com/0.1.x/documentation/docs/user-guide/quickstart/


## クイックスタート

```
$pnpm create vite
.../1997053acfa-2b9b                     |   +1 +
.../1997053acfa-2b9b                     | Progress: resolved 1, reused 0, downloaded 1, added 1, done
│
◇  Project name:
│  sample_vite_react
│
◇  Select a framework:
│  React
│
◇  Select a variant:
│  TypeScript
│
◇  Scaffolding project in /Users/katakuranatsumi/sample_vite_react...
│
└  Done. Now run:
  cd sample_vite_react
  pnpm install
  pnpm run dev
```

```
$pnpm install
   ╭──────────────────────────────────────────╮
   │                                          │
   │   Update available! 10.15.1 → 10.17.0.   │
   │   Changelog: https://pnpm.io/v/10.17.0   │
   │     To update, run: pnpm add -g pnpm     │
   │                                          │
   ╰──────────────────────────────────────────╯
Packages: +187

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Progress: resolved 233, reused 45, downloaded 142, added 187, done
dependencies:
+ react 19.1.1
+ react-dom 19.1.1
devDependencies:
+ @eslint/js 9.36.0
+ @types/react 19.1.13
+ @types/react-dom 19.1.9
+ @vitejs/plugin-react 5.0.3
+ eslint 9.36.0
+ eslint-plugin-react-hooks 5.2.0
+ eslint-plugin-react-refresh 0.4.20
+ globals 16.4.0
+ typescript 5.8.3 (5.9.2 is available)
+ typescript-eslint 8.44.0
+ vite 7.1.7
  
╭ Warning ───────────────────────────────────────────────────────────────────────────────────╮
│                                                                                            │
│   Ignored build scripts: esbuild.                                                          │
│   Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.   │
│                                                                                            │
╰────────────────────────────────────────────────────────────────────────────────────────────
Done in 7.5s using pnpm v10.15.1
```