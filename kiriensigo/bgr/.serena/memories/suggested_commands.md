# BGR v2 推奨コマンド集

## 基本開発コマンド

### 開発サーバー
```bash
cd bgr-v2
npm run dev          # 開発サーバー起動 (http://localhost:3001)
npm run build        # プロダクションビルド
npm run start        # プロダクションサーバー起動
```

### コード品質チェック
```bash
npm run lint         # ESLint実行
npx tsc --noEmit     # TypeScript型チェック
```

### テスト実行
```bash
# 単体テスト (Jest)
npm test             # テスト実行
npm run test:watch   # ウォッチモード
npm run test:coverage # カバレッジ付き実行
npm run test:ci      # CI用実行

# E2Eテスト (Playwright)
npm run test:e2e     # E2Eテスト実行
npm run test:e2e:ui  # UI付きテスト
npm run test:e2e:headed # ヘッド付きテスト

# 全テスト実行
npm run test:all     # 単体 + E2E全実行
```

### Playwright設定
```bash
npm run playwright:install  # Playwrightブラウザインストール
```

### 依存関係管理
```bash
npm install          # 依存関係インストール
npm ci              # 本番環境用インストール
```

## Windows固有コマンド

### ファイル操作
```cmd
dir                  # ディレクトリ一覧表示
cd /d C:\path       # ドライブ変更付きディレクトリ移動
type filename       # ファイル内容表示
copy src dest       # ファイルコピー
move src dest       # ファイル移動
del filename        # ファイル削除
rmdir /s dirname    # ディレクトリ削除
```

### Git操作
```bash
git status          # 変更状況確認
git add .           # 全変更をステージング
git commit -m "message"  # コミット
git push            # プッシュ
git pull            # プル
```

### プロセス管理
```cmd
tasklist            # プロセス一覧
taskkill /PID xxxx  # プロセス終了
netstat -an         # ポート使用状況確認
```

## 分析・最適化コマンド

### バンドルサイズ分析
```bash
npm run analyze     # Bundle Analyzer実行
```

### パフォーマンステスト
```bash
npm run test:e2e -- --project=chromium  # Chrome単体でテスト
```

## 緊急時対応コマンド

### キャッシュクリア
```bash
rmdir /s .next      # Next.jsキャッシュクリア
rmdir /s node_modules  # 依存関係クリア
npm install         # 再インストール
```

### ポート競合解決
```bash
netstat -ano | findstr :3001  # ポート3001使用プロセス確認
taskkill /PID xxxx /F          # プロセス強制終了
```