# BGR v2 プロジェクト情報

## プロジェクト概要

- **プロジェクト名**: BGR (Board Game Review) Version 2
- **目的**: スタイリッシュなボードゲームレビューサイトの構築
- **現在のサイト**: https://bgrq.netlify.app/

## 技術スタック

- **フロントエンド**: Next.js 14 (App Router) + TypeScript + Tailwind CSS
- **データベース**: Supabase (PostgreSQL)
- **認証**: Supabase Auth (Google, Twitter OAuth)
- **API**: BGG API 連携 + Netlify Functions
- **デプロイ**: Netlify
- **レスポンシブ**: 高優先度対応

## 開発コマンド

### 依存関係インストール

```bash
# Supabase
npm install @supabase/supabase-js @supabase/auth-ui-react @supabase/auth-ui-shared

# UI・スタイリング
npm install @radix-ui/react-accordion @radix-ui/react-alert-dialog @radix-ui/react-avatar @radix-ui/react-button @radix-ui/react-card @radix-ui/react-dialog @radix-ui/react-dropdown-menu @radix-ui/react-form @radix-ui/react-input @radix-ui/react-label @radix-ui/react-navigation-menu @radix-ui/react-select @radix-ui/react-sheet @radix-ui/react-tabs @radix-ui/react-textarea @radix-ui/react-toast
npm install lucide-react class-variance-authority clsx tailwind-merge

# フォーム・バリデーション
npm install react-hook-form @hookform/resolvers zod

# 状態管理・ユーティリティ
npm install zustand date-fns sharp next-seo xml2js @types/xml2js
```

### 基本コマンド

```bash
npm run dev          # 開発サーバー起動
npm run build        # ビルド
npm run start        # 本番サーバー起動
npm run lint         # Linting
```

## 環境変数設定

### `.env.local`

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# BGG API
BGG_API_BASE_URL=https://boardgamegeek.com/xmlapi2

# OAuth設定
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
TWITTER_CLIENT_ID=your_twitter_client_id
TWITTER_CLIENT_SECRET=your_twitter_client_secret

# アプリケーション設定
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME="BGR - Board Game Review"
```

## ディレクトリ構造

```
bgr-v2/
├── src/
│   ├── app/                    # App Router
│   │   ├── (auth)/             # 認証ページ
│   │   ├── admin/              # 管理画面
│   │   ├── games/              # ゲーム関連
│   │   ├── reviews/            # レビュー関連
│   │   └── api/                # API Routes
│   ├── components/             # コンポーネント
│   │   ├── ui/                 # shadcn/ui
│   │   ├── auth/
│   │   ├── games/
│   │   └── reviews/
│   ├── lib/                    # ライブラリ・設定
│   ├── hooks/                  # カスタムフック
│   ├── store/                  # 状態管理
│   └── types/                  # 型定義
├── public/
└── docs/                       # ドキュメント
```

## データベーススキーマ

### 主要テーブル

```sql
-- profiles テーブル
create table public.profiles (
  id uuid references auth.users on delete cascade,
  updated_at timestamp with time zone,
  username text unique,
  full_name text,
  avatar_url text,
  website text,
  is_admin boolean default false,
  primary key (id)
);

-- games テーブル
create table public.games (
  id bigserial primary key,
  bgg_id integer unique,
  name text not null,
  description text,
  year_published integer,
  min_players integer,
  max_players integer,
  playing_time integer,
  image_url text,
  mechanics text[],
  categories text[],
  designers text[],
  publishers text[],
  rating_average numeric(3,2),
  rating_count integer default 0,
  created_at timestamp with time zone default timezone('utc'::text, now())
);

-- reviews テーブル
create table public.reviews (
  id bigserial primary key,
  user_id uuid references public.profiles(id) on delete cascade,
  game_id bigint references public.games(id) on delete cascade,
  title text not null,
  content text not null,
  rating integer check (rating >= 1 and rating <= 10),
  pros text[],
  cons text[],
  is_published boolean default true,
  created_at timestamp with time zone default timezone('utc'::text, now())
);
```

## BGG API 仕様

### 主要エンドポイント

```typescript
// ゲーム検索
https://boardgamegeek.com/xmlapi2/search?query={query}&type=boardgame

// ゲーム詳細情報
https://boardgamegeek.com/xmlapi2/thing?id={id}&stats=1

// ホットゲーム一覧
https://boardgamegeek.com/xmlapi2/hot?type=boardgame
```

### レート制限対策

- 1 秒あたり 1 リクエスト制限
- Redis/Memory キャッシュ実装
- バックグラウンドでの定期同期

## 🚨 **絶対的なルール**

### BGGマッピング制限
- **BGGカテゴリー・メカニクスのマッピングは確定事項**
- **いかなる理由があってもマッピングを追加・変更してはいけない**
- **正式マッピングは bgg_api.mdc に記載されている**
- **対象外のBGGデータは意図的に除外されている**

### 正式BGGマッピング仕様 (bgg_api.mdc より)

#### BGGカテゴリー → サイトカテゴリー
```
'Animals' => '動物'
'Bluffing' => 'ブラフ'
'Card Game' => 'カードゲーム'
"Children's Game" => '子供向け'
'Deduction' => '推理'
'Memory' => '記憶'
'Negotiation' => '交渉'
'Party Game' => 'パーティー'
'Puzzle' => 'パズル'
'Wargame' => 'ウォーゲーム'
'Word Game' => 'ワードゲーム'
```

#### BGGカテゴリー → サイトメカニクス
```
'Dice' => 'ダイスロール'
```

#### BGGメカニクス → サイトカテゴリー
```
'Acting' => '演技'
'Deduction' => '推理'
'Legacy Game' => 'レガシー・キャンペーン'
'Memory' => '記憶'
'Negotiation' => '交渉'
'Paper-and-Pencil' => '紙ペン'
'Scenario / Mission / Campaign Game' => 'レガシー・キャンペーン'
'Solo / Solitaire Game' => 'ソロ向き'
'Pattern Building' => 'パズル'
'Trick-taking' => 'トリテ'
```

#### BGGメカニクス → サイトメカニクス
```
'Area Majority / Influence' => 'エリア支配'
'Auction / Bidding' => 'オークション'
'Auction Compensation' => 'オークション'
'Auction: Dexterity' => 'オークション'
'Auction: Dutch' => 'オークション'
'Auction: Dutch Priority' => 'オークション'
'Auction: English' => 'オークション'
'Auction: Fixed Placement' => 'オークション'
'Auction: Multiple Lot' => 'オークション'
'Auction: Once Around' => 'オークション'
'Auction: Sealed Bid' => 'オークション'
'Auction: Turn Order Until Pass' => 'オークション'
'Betting and Bluffing' => '賭け'
'Closed Drafting' => 'ドラフト'
'Cooperative Game' => '協力'
'Deck Construction' => 'デッキ/バッグビルド'
'Deck, Bag, and Pool Building' => 'デッキ/バッグビルド'
'Dice Rolling' => 'ダイスロール'
'Hidden Roles' => '正体隠匿'
'Modular Board' => 'モジュラーボード'
'Network and Route Building' => 'ルート構築'
'Open Drafting' => 'ドラフト'
'Push Your Luck' => 'バースト'
'Set Collection' => 'セット収集'
'Simultaneous Action Selection' => '同時手番'
'Tile Placement' => 'タイル配置'
'Variable Player Powers' => 'プレイヤー別能力'
'Variable Set-up' => 'プレイヤー別能力'
'Worker Placement' => 'ワカプレ'
'Worker Placement with Dice Workers' => 'ワカプレ'
'Worker Placement, Different Worker Types' => 'ワカプレ'
```

#### プレイ人数 → サイトカテゴリー
```
'1' => 'ソロ向き'
'2' => 'ペア向き'
'6以上' => '多人数向き'
```

## AI コーディング ガイドライン

### 🚨 重要なルール

#### 1. コーディング原則

```typescript
// ❌ 悪い例: 複雑で理解しにくい
function processGameData(data: any) {
  return data
    .map((item: any) => {
      if (item.type === "game" && item.players?.length > 0) {
        return { ...item, playerCount: item.players.length };
      }
      return item;
    })
    .filter(Boolean);
}

// ✅ 良い例: 明確で理解しやすい
interface Game {
  id: number;
  name: string;
  players: Player[];
  published: boolean;
}

function processGameData(games: Game[]): ProcessedGame[] {
  return games.filter(isValidGame).map(processIndividualGame);
}

function isValidGame(game: Game): boolean {
  return game.players.length > 0;
}
```

#### 2. コンポーネント設計原則

```typescript
// ✅ 良い例: 単一責任の原則
function GamePage({ gameId }: { gameId: string }) {
  return (
    <div className="game-page">
      <GameHeader gameId={gameId} />
      <GameDetails gameId={gameId} />
      <ReviewsSection gameId={gameId} />
    </div>
  );
}

function GameHeader({ gameId }: { gameId: string }) {
  const { data: game, isLoading } = useGame(gameId);

  if (isLoading) return <GameHeaderSkeleton />;

  return (
    <header className="game-header">
      <GameTitle game={game} />
      <GameActions game={game} />
    </header>
  );
}
```

#### 3. 型安全性の強制

```typescript
interface ReviewFormData {
  title: string;
  content: string;
  rating: number; // 1-10
  gameId: number;
  pros?: string[];
  cons?: string[];
}

function handleFormSubmit(data: ReviewFormData): Promise<void> {
  validateReviewData(data);
  return submitReview(data);
}
```

#### 4. エラーハンドリング

```typescript
async function loadGameData(id: string): Promise<Game | null> {
  try {
    const response = await fetch(`/api/games/${id}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();

    if (!isValidGame(data)) {
      throw new Error("Invalid game data received");
    }

    return data;
  } catch (error) {
    console.error(`Failed to load game ${id}:`, error);
    return null;
  }
}
```

### 🎯 AI コーディング時の必須チェックリスト

#### 開発開始時

- [ ] 要件を明確に理解しているか
- [ ] 型定義を最初に作成したか
- [ ] コンポーネントの責任範囲を明確にしたか
- [ ] エラーハンドリング戦略を決定したか

#### コード作成時

- [ ] 関数は単一責任を持っているか
- [ ] 型注釈を適切に付けているか
- [ ] エラーケースを考慮しているか
- [ ] ネストが深すぎないか（3 層以下）
- [ ] マジックナンバーを避けているか

### 📋 チケット管理

#### チケット一覧

1. `001_project_setup.md` - プロジェクト基盤セットアップ
2. `002_supabase_database.md` - Supabase データベース設定
3. `003_authentication.md` - 認証システム実装
4. `004_bgg_api_integration.md` - BGG API 連携機能
5. `005_game_management.md` - ゲーム管理機能
6. `006_review_system.md` - レビューシステム
7. `007_admin_panel.md` - 管理機能
8. `008_ui_design_implementation.md` - UI/UX デザイン実装
9. `009_home_page.md` - ホームページ実装
10. `010_seo_optimization.md` - SEO 対策・最適化
11. `011_testing_setup.md` - テスト環境構築
12. `012_netlify_deployment.md` - Netlify デプロイ設定

#### 進捗確認コマンド

```bash
# 全チケットの進捗を確認
grep -r "\- \[" docs/

# 完了済みタスクを確認
grep -r "\- \[x\]" docs/
```

## Next.js 14 ベストプラクティス

### App Router 設計パターン

#### Server vs Client Components

```typescript
// ✅ Server Component (デフォルト)
async function GamesList() {
  const games = await getGames();
  return (
    <div>
      {games.map((game) => (
        <GameCard key={game.id} game={game} />
      ))}
    </div>
  );
}

// ✅ Client Component
("use client");
function SearchForm() {
  const [query, setQuery] = useState("");
  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}
```

#### データフェッチング

```typescript
// ✅ 並列データフェッチ
async function GamePage({ params }: { params: { id: string } }) {
  const [game, reviews] = await Promise.all([
    getGame(params.id),
    getGameReviews(params.id),
  ]);

  return (
    <div>
      <GameDetail game={game} />
      <ReviewsList reviews={reviews} />
    </div>
  );
}
```

#### キャッシュ戦略

```typescript
async function getGames() {
  const res = await fetch("/api/games", {
    next: {
      revalidate: 60,
      tags: ["games"],
    },
  });
  return res.json();
}
```

### TypeScript 最適化

#### 型安全な API 設計

```typescript
export interface Game {
  id: number;
  name: string;
  bggId: number;
  yearPublished?: number;
  minPlayers: number;
  maxPlayers: number;
  mechanics: string[];
  categories: string[];
  ratingAverage?: number;
  createdAt: string;
}

export interface ApiResponse<T> {
  data: T;
  success: boolean;
  message?: string;
}
```

#### zod による実行時型検証

```typescript
import { z } from "zod";

export const gameSchema = z.object({
  name: z.string().min(1, "名前は必須です"),
  description: z.string().optional(),
  yearPublished: z.number().min(1900).max(new Date().getFullYear()).optional(),
  minPlayers: z.number().min(1),
  maxPlayers: z.number().min(1),
});

export type GameFormData = z.infer<typeof gameSchema>;
```

### パフォーマンス最適化

#### 画像最適化

```typescript
import Image from "next/image";

function GameCard({ game }: { game: Game }) {
  return (
    <Image
      src={game.imageUrl || "/placeholder-game.jpg"}
      alt={game.name}
      width={300}
      height={300}
      placeholder="blur"
      sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
    />
  );
}
```

#### 動的インポート

```typescript
import dynamic from "next/dynamic";

const GameChart = dynamic(() => import("@/components/GameChart"), {
  loading: () => <ChartSkeleton />,
  ssr: false,
});
```

### セキュリティ

#### 認証・認可

```typescript
import { createServerClient } from "@supabase/ssr";

export async function requireAuth() {
  const supabase = await createServerSupabaseClient();
  const {
    data: { user },
    error,
  } = await supabase.auth.getUser();

  if (error || !user) {
    redirect("/login");
  }

  return user;
}
```

### エラーハンドリング

#### Error Boundary

```typescript
// app/games/[id]/error.tsx
"use client";

export default function GameError({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div className="error-container">
      <h2>ゲーム情報の取得に失敗しました</h2>
      <button onClick={reset}>再試行</button>
    </div>
  );
}
```

### テスト戦略

#### テスト依存関係

```bash
# テストフレームワーク
npm install -D jest @testing-library/react @testing-library/jest-dom

# E2Eテスト
npm install -D playwright @playwright/test

# モック・ユーティリティ
npm install -D msw jest-environment-jsdom
```

#### コンポーネントテスト

```typescript
import { render, screen } from "@testing-library/react";
import GameCard from "@/components/GameCard";

describe("GameCard", () => {
  it("ゲーム名を表示する", () => {
    render(<GameCard game={mockGame} />);
    expect(screen.getByText("テストゲーム")).toBeInTheDocument();
  });
});
```

#### E2E テスト

```typescript
import { test, expect } from "@playwright/test";

test("ゲーム詳細ページからレビューを投稿できる", async ({ page }) => {
  await page.goto("/games/1");
  await page.click('button:text("レビューを書く")');
  await page.fill('[name="title"]', "Great game!");
  await page.click('button[type="submit"]');
  await expect(page.locator(".success-message")).toContainText(
    "レビューを投稿しました"
  );
});
```

## デプロイ設定

### Netlify 設定 (`netlify.toml`)

```toml
[build]
  publish = "out"
  command = "npm run build"

[build.environment]
  NODE_VERSION = "18"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### 環境変数 (Netlify)

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `BGG_API_BASE_URL`
- `GOOGLE_CLIENT_ID/SECRET`
- `TWITTER_CLIENT_ID/SECRET`

## トラブルシューティング

### よくある問題

1. **BGG API レート制限**: キャッシュとリトライ機能実装
2. **Supabase 認証エラー**: 環境変数確認
3. **画像読み込みエラー**: Next.js Image 最適化設定確認
4. **ビルドエラー**: TypeScript 型チェック
5. **ハイドレーションエラー**: サーバー・クライアント間の状態不整合

### デバッグコマンド

```bash
npm run dev -- --turbo      # 開発サーバーでのデバッグ
npm run build -- --debug    # ビルド詳細出力
npx tsc --noEmit            # 型チェック
rm -rf .next && npm run build # キャッシュクリア
```

## Playwright MCP 使用ルール

### 絶対的な禁止事項

1. **いかなる形式のコード実行も禁止**

   - Python、JavaScript、Bash 等でのブラウザ操作
   - MCP ツールを調査するためのコード実行
   - subprocess やコマンド実行によるアプローチ

2. **利用可能なのは MCP ツールの直接呼び出しのみ**

   - playwright:browser_navigate
   - playwright:browser_screenshot
   - 他の Playwright MCP ツール

3. **エラー時は即座に報告**
   - 回避策を探さない
   - 代替手段を実行しない
   - エラーメッセージをそのまま伝える

## ローカル開発環境設定

### OAuth認証テスト設定（開発用）

#### ローカル開発サーバー
```bash
cd bgr-v2
npm run dev  # 常にポート3001を使用
```

#### OAuth プロバイダー設定

**Supabase Dashboard 設定:**
```
Site URL: http://localhost:3001
Redirect URLs: http://localhost:3001/auth/callback
```

**Twitter Developer Console 設定:**
```
Callback URLs: http://localhost:3001/auth/callback
```

**Google Cloud Console 設定:**
```
承認済みリダイレクト URI: http://localhost:3001/auth/callback
```

#### テスト手順
1. ローカルサーバー起動: `npm run dev`
2. `http://localhost:3001/login` にアクセス
3. Twitter/Googleログインボタンをクリック
4. OAuth認証フローをテスト
5. `/auth/callback` でのコールバック処理を確認

#### 既知の問題
- ~~OAuth callback ページが404エラーになる問題~~ ✅ **修正完了** (2025-08-15)
- ~~Route Handler vs Page コンポーネントの競合~~ ✅ **修正完了** (2025-08-15)
- ~~Next.js middlewareによるリライト問題~~ ✅ **修正完了** (2025-08-15)

#### 最近の修正履歴

**2025-08-15: OAuth認証修復**
- ✅ middleware.tsのリライト処理削除
- ✅ auth/callback/route.ts の実装修正
- ✅ Twitter/Google OAuth認証フロー復旧
- ✅ ローカル・本番環境での動作確認完了
- 📝 コミット: `992223b` - OAuth認証コールバック処理を完全修復

#### プロジェクト進捗状況

**完了済み機能:**
1. ✅ 基盤セットアップ・環境構築
2. ✅ Supabase データベース設定
3. ✅ OAuth認証システム (Twitter/Google)
4. ✅ BGG API 連携機能
5. ✅ ゲーム管理・検索機能
6. ✅ レビューシステム
7. ✅ 管理パネル
8. ✅ UI/UX デザイン実装
9. ✅ ホームページ実装
10. ✅ Netlify デプロイ設定

**次のタスク候補:**
- SEO最適化の強化
- テストカバレッジの向上
- パフォーマンス最適化
- 新機能追加

---

**最終更新**: 2025-08-15  
**Claude Code 用プロジェクト情報ファイル**
