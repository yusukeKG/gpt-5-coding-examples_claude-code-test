# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリで作業する際のガイダンスを提供します。

## プロジェクト概要

GPT-5/GPT-5.2が単一プロンプトで生成したデモアプリケーションを紹介するリポジトリ。Next.jsで構築されたギャラリーフロントエンドが、スタンドアロンのHTML/JSサンプルを表示・実行する。

## 開発コマンド

すべてのコマンドは `front-end/` ディレクトリで実行:

```bash
cd front-end
npm install
npm run dev      # 開発サーバー起動 (localhost:3000)
npm run build    # 本番ビルド
npm run lint     # ESLint実行
```

`dev` と `build` スクリプトは自動的に `scripts/copy-docs.mjs` を実行し、`apps/` から `public/` へアプリをコピーする。

## アーキテクチャ

### ディレクトリ構成
- `examples/` - 各デモのYAMLメタデータファイル（id、title、prompt、tags）
- `apps/` - ビルド済み静的HTMLアプリ（各サブディレクトリにindex.htmlを含む）
- `front-end/` - Next.jsギャラリーアプリケーション

### データフロー
1. `examples/` のYAMLファイルがデモのメタデータを定義（title、prompt、tags、camera/microphoneフラグ）
2. `lib/load-yaml.ts` がリポジトリ内の全YAMLファイルを再帰的に検索
3. `lib/code-examples.ts` がYAMLを `CodeExample` オブジェクトにパース（CDNポスターURL付き）
4. アプリは `public/{app-id}/index.html` から静的HTMLとして配信
5. `next.config.ts` がリライトを設定し、`/app-id` で `/app-id/index.html` を配信

### 新しいサンプルの追加方法
1. `examples/{id}.yaml` にYAMLファイルを作成（id、title、prompt、tagsを含む）
2. `apps/{id}/` にビルド済み静的アプリを追加（`index.html` を含む）
3. スクリーンショットはCDNでホスト: `https://cdn.openai.com/devhub/gpt5prompts/{id}.png`

### 主要コンポーネント
- `components/app-grid.tsx` - 全サンプルのグリッド表示
- `components/app-card.tsx` - 個別サンプルのカード
- `components/app-modal.tsx` - サンプル詳細/iframe表示用モーダル

### 技術スタック
- Next.js 15（App Router、静的エクスポート `output: "export"`）
- React 19、TypeScript、Tailwind CSS 4
- Radix UIコンポーネント（shadcn/uiパターン）
- YAMLパースに `yaml` パッケージを使用

## 守って欲しいこと
- ソースコード内のコメントは日本語で記述すること
- TypeScriptにおいて必要以上にクラスを使用しないこと
- 既存のディレクトリ構成・命名・状態管理の流儀にそろえること