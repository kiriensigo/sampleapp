# BGR v2 プロジェクト概要

## プロジェクト基本情報

**プロジェクト名**: BGR (Board Game Review) Version 2  
**目的**: スタイリッシュなボードゲームレビューサイトの構築  
**現在のサイト**: https://bgrq.netlify.app/  
**作業ディレクトリ**: `bgr-v2/`

## 技術スタック

### フロントエンド
- **フレームワーク**: Next.js 15.4.6 (App Router)
- **言語**: TypeScript 5 (strict mode)
- **スタイリング**: Tailwind CSS 4
- **UIライブラリ**: Radix UI + shadcn/ui
- **アイコン**: Lucide React
- **フォーム**: React Hook Form + Zod validation
- **状態管理**: Zustand

### バックエンド・データベース
- **データベース**: Supabase (PostgreSQL)
- **認証**: Supabase Auth (Google, Twitter OAuth)
- **API連携**: BGG (Board Game Geek) API

### 開発・テスト
- **テストフレームワーク**: Jest (単体) + Playwright (E2E)
- **Linting**: ESLint + Next.js設定
- **パッケージマネージャー**: npm

### デプロイ
- **ホスティング**: Netlify
- **ドメイン**: https://bgrq.netlify.app/

## プロジェクト構造

```
bgr-v2/
├── src/
│   ├── app/                # Next.js App Router
│   │   ├── (auth)/         # 認証ページ
│   │   ├── admin/          # 管理画面
│   │   ├── games/          # ゲーム関連ページ
│   │   ├── reviews/        # レビュー関連ページ
│   │   └── api/           # API Routes
│   ├── components/         # Reactコンポーネント
│   │   ├── ui/            # shadcn/ui base components
│   │   ├── auth/          # 認証関連
│   │   ├── games/         # ゲーム関連
│   │   ├── reviews/       # レビュー関連
│   │   └── layout/        # レイアウト関連
│   ├── lib/               # ライブラリ・ユーティリティ
│   ├── hooks/             # カスタムフック
│   ├── types/             # TypeScript型定義
│   └── store/             # 状態管理
├── public/                # 静的ファイル
├── __tests__/             # テストファイル
└── e2e/                   # E2Eテスト
```

## 主要機能

1. **ユーザー認証**: Google/Twitter OAuth
2. **ゲーム管理**: BGG API連携による自動データ取得
3. **レビューシステム**: 詳細評価・pros/cons・タグ機能
4. **検索・フィルタ**: 高度な検索・絞り込み機能
5. **管理機能**: ユーザー・ゲーム・レビューの管理
6. **レスポンシブデザイン**: モバイル対応

## データベース構造

### 主要テーブル
- **profiles**: ユーザープロフィール
- **games**: ゲーム情報 (BGG連携)
- **reviews**: レビューデータ (拡張評価項目)
- **favorites**: お気に入り
- **comments**: レビューコメント
