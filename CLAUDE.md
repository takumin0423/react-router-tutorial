# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## コマンド

**開発:**
- `pnpm dev` - ホットリロード付きの開発サーバーを起動
- `pnpm build` - プロダクション用にビルド
- `pnpm start` - プロダクションサーバーを実行
- `pnpm typecheck` - ルートの型を生成してTypeScriptのチェックを実行

**注意:** このプロジェクトにはテストコマンドが設定されていません。

## アーキテクチャ概要

これはReact Router v7のチュートリアルプロジェクトで、モダンなWebパターンを使用した連絡先管理アプリケーションです。

### ルーティングシステム

ルートは`app/routes.ts`でReact Router v7の設定APIを使用して明示的に定義されています：
- ルートは`app/routes/`ディレクトリ内のファイルにマッピング
- サイドバーレイアウトが連絡先関連のルートをラップ
- ルートパラメータは`:param`構文を使用
- `/about`ルートはサイドバーレイアウトの外側に存在

### データレイヤー

データレイヤー（`app/data.ts`）はインメモリの連絡先データベースを実装：
- CRUD操作: `getContacts()`, `createEmptyContact()`, `getContact()`, `updateContact()`, `deleteContact()`
- `match-sorter`ライブラリを使用した検索機能
- ネットワークリクエストをシミュレートする500msの人工的な遅延
- 型安全性のためのTypeScriptインターフェース

### 主要なパターン

1. **型生成**: `pnpm typecheck`を実行すると`.react-router/types/`にルート固有の型が生成されます。各ルートは`LoaderArgs`、`ActionArgs`、コンポーネントpropsを含む強く型付けされた`Route.*`名前空間を取得。

2. **データ読み込み**: ルートはデータ取得用の`loader`関数とミューテーション用の`action`関数をコンポーネントと同じ場所でエクスポート。

3. **ルートアクション**: `app/root.tsx`ファイルは連絡先を作成する「新規」ボタンのアクションを処理し、クロスルートアクションを示しています。

4. **フォーム処理**: プログレッシブエンハンスメントと組み込みのローディング状態のために`<Form>`コンポーネントを使用。

5. **楽観的UI**: お気に入りボタンはナビゲーションなしで更新するために`useFetcher`を使用。

### 設定

- **react-router.config.ts**: SSRを有効にし、`/about`ルートをプリレンダリング
- **vite.config.ts**: `@react-router/dev/vite`プラグインを使用した最小限の設定
- React Router v7は内部的にほとんどのバンドリングの問題を処理