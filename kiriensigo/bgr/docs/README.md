# BGR プロジェクト統合ドキュメント

## 📂 **シンプルな統一構成**

```
docs/
├── README.md                           ← このファイル（全体ガイド）
│
├── 🔥 現在のプロジェクト
│   ├── requirements-consolidated.md    ← 統合要件定義書
│   ├── review-requirements.md          ← レビュー詳細要件v2
│   ├── 004_review_system_mockup.md     ← チケット004（モックアップ）
│   ├── development-guidelines.md       ← 開発ガイドライン
│   ├── mockups/                        ← モックアップ集
│   ├── database-schema*.sql            ← DB設計
│   ├── DEPLOYMENT.md                   ← デプロイ手順
│   └── bgg_api.mdc                     ← BGG API仕様
│
├── 📋 進捗管理
│   ├── 001_potential_future_improvements.md     ← 将来改善案
│   ├── 002_remaining_tasks_from_completed_tickets.md  ← 残作業
│   └── 003_architecture_improvement_roadmap.md  ← アーキテクチャ改善
│
└── 📚 完了チケット
    └── completed/                      ← 001-021の詳細記録
```

## 🎯 **すぐに見るべきドキュメント**

### 🔥 **開発・実装**
1. **[統合要件定義書](requirements-consolidated.md)** - プロジェクト全体の要件
2. **[レビューシステム詳細要件v2](review-requirements.md)** - 現在開発中の機能
3. **[チケット004モックアップ](004_review_system_mockup.md)** - 実装予定の設計
4. **[開発ガイドライン](development-guidelines.md)** - テストファイル管理等のルール

### 📱 **UI/UXデザイン**
- **[ワイヤーフレーム](mockups/review-form-wireframes.md)** - レイアウト設計
- **[ビジュアルモックアップ](mockups/review-form-visual-mockup.md)** - 詳細デザイン
- **[Twitterシェア機能](mockups/twitter-share-mockup.md)** - SNS連携設計

### 🗄️ **技術仕様**
- **[データベース設計](database-schema-fixed.sql)** - 最新のDBスキーマ
- **[デプロイ手順](DEPLOYMENT.md)** - 本番環境設定
- **[BGG API仕様](bgg_api.mdc)** - 外部API連携

### 📊 **進捗・管理**
- **[残作業一覧](002_remaining_tasks_from_completed_tickets.md)** - 47個の未完了項目
- **[アーキテクチャ改善案](003_architecture_improvement_roadmap.md)** - 28個の改善項目
- **[完了チケット詳細](completed/)** - 001-021の実装記録

## 📈 **現在の開発状況**

### ✅ **完了済み (21チケット)**
- 基盤セットアップ・環境構築
- Supabase データベース設定
- OAuth認証システム (Twitter/Google)
- BGG API 連携機能
- ゲーム管理・検索機能
- 基本レビューシステム
- 管理パネル・UI/UX デザイン実装
- ホームページ・SEO最適化
- テスト環境・Netlify デプロイ設定
- セキュリティ・パフォーマンス・アクセシビリティ対応
- コード品質・監視システム

### 🔄 **進行中**
- **レビューシステムv2実装** (チケット004) ← 現在ここ
- 残りUIコンポーネント実装

### 📋 **次期予定**
- 単体テスト拡充
- パフォーマンス最適化
- 新機能追加

## 🎨 **プロジェクト概要**

### **目的**
**短時間でレビューができる**ボードゲームレビューサイト

### **主要機能**
- **1-2分完了**: スライダーによる直感的評価
- **1ページ完結**: プレビューなしで即座投稿
- **モバイル最適化**: タッチ操作対応
- **Twitter連携**: ワンクリックでSNSシェア

### **技術スタック**
- **Next.js 14** (App Router) + TypeScript
- **Supabase** (PostgreSQL + Auth)
- **Tailwind CSS** + shadcn/ui
- **Netlify** デプロイ

## 🔗 **関連リンク**

- **現在のサイト**: https://bgrq.netlify.app/
- **実装コード**: `/bgr-v2/src/`
- **テストスクリプト**: `/bgr-v2/scripts/test/`

## 🚀 **開発開始方法**

```bash
cd bgr-v2
npm install
npm run dev
```

---

**最終更新**: 2025-08-22  
**構成**: 完全統一（docs/一箇所に集約完了）  
**進捗管理**: TodoListで継続追跡中