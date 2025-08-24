# BGR v2 タスク完了時のワークフロー

## 必須チェック項目

### 1. コード品質チェック
```bash
# TypeScript型チェック
npx tsc --noEmit

# ESLint実行
npm run lint

# 自動修正が可能な場合
npm run lint -- --fix
```

### 2. テスト実行
```bash
# 単体テスト実行
npm run test:ci

# 変更箇所に関連するE2Eテスト
npm run test:e2e -- --grep="関連機能"

# 重要な機能変更の場合は全E2Eテスト
npm run test:all
```

### 3. ビルド確認
```bash
# プロダションビルドが成功することを確認
npm run build

# 起動確認
npm run start
```

## 品質基準

### テストカバレッジ
- **最低基準**: 80% (lines, functions, branches, statements)
- **新規コード**: 90%以上推奨

### パフォーマンス
- **Core Web Vitals**: 全てGreenbench超え
- **Lighthouse Score**: 90点以上

### アクセシビリティ
- **jest-axe**: エラーなし
- **Lighthouse Accessibility**: 95点以上

## コミット前チェックリスト

### コードレビュー (セルフチェック)
- [ ] 型安全性の確保
- [ ] エラーハンドリングの実装
- [ ] ユーザビリティの考慮
- [ ] セキュリティの確認
- [ ] パフォーマンスの確認

### ファイル確認
- [ ] 不要なconsole.log削除
- [ ] デバッグコード削除
- [ ] TODOコメント確認
- [ ] 秘密情報の除外確認

### 依存関係確認
- [ ] 新規依存関係の妥当性
- [ ] package.jsonの整合性
- [ ] lockファイルの更新

## デプロイ前最終確認

### ローカル環境での確認
```bash
# 開発サーバーでの動作確認
npm run dev

# プロダクションビルドでの確認
npm run build && npm run start
```

### 主要機能テスト
- [ ] 認証フロー (Google/Twitter)
- [ ] ゲーム検索・詳細表示
- [ ] レビュー投稿・編集
- [ ] 管理画面機能 (admin権限)
- [ ] レスポンシブデザイン

### BGG API連携確認
- [ ] ゲーム検索API
- [ ] ゲーム詳細取得API
- [ ] レート制限遵守

## 緊急時対応

### ビルドエラー時
```bash
# キャッシュクリア
rmdir /s .next
rm -rf node_modules
npm install
npm run build
```

### テスト失敗時
```bash
# 特定テストのみ実行
npm test -- --testNamePattern="テスト名"

# キャッシュクリア
npm test -- --clearCache
```

### 依存関係問題時
```bash
# 依存関係再インストール
rm package-lock.json
rm -rf node_modules
npm install
```

## 継続的改善

### 定期メンテナンス
- 週次: 依存関係更新確認
- 月次: パフォーマンス監査
- 四半期: セキュリティ監査

### メトリクス確認
- Bundle size増加確認
- Core Web Vitals監視
- エラー率監視 (Sentry等)

## 特記事項

### BGGマッピング制限
- **絶対に変更禁止**: BGGカテゴリー・メカニクスマッピング
- **参照ファイル**: `bgg_api.mdc`
- **正式マッピング以外は意図的除外**

### 環境固有の注意
- **ローカルポート**: 3001 (package.jsonで固定)
- **OAuth設定**: localhost:3001でのコールバック確認
- **Windows環境**: パス区切り文字注意