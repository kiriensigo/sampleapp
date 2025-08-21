# BGR v2 クイックガイド

## 🚨 最重要ルール (毎回確認必須)

### AIコーディング時の絶対ルール
1. **型安全性**: `any`型禁止、全て型注釈
2. **エラーハンドリング**: 全ての非同期処理に`try-catch`
3. **単一責任**: 関数は1つの役割のみ、50行以下
4. **早期リターン**: ネスト3層以下、早期リターンで平坦化
5. **防御的プログラミング**: 入力値検証とnullチェック必須

### 禁止事項
```typescript
// ❌ 絶対禁止
function badCode(data: any) {
  if (data) {
    if (data.game) {
      if (data.game.id) {
        // 深いネスト
      }
    }
  }
}

// ✅ 必須パターン
interface GameData { game: { id: number } }
function goodCode(data: GameData): Result {
  if (!data?.game?.id) throw new Error('Invalid game data')
  return processGame(data.game)
}
```

## 📁 プロジェクト構成

```
bgr-v2/
├── src/app/                 # App Router
│   ├── (auth)/login/       # 認証
│   ├── games/[id]/         # ゲーム詳細
│   ├── reviews/            # レビュー
│   ├── admin/              # 管理機能
│   └── api/                # API Routes
├── src/components/         # UIコンポーネント
├── src/lib/               # ユーティリティ
├── src/hooks/             # カスタムフック
├── src/store/             # Zustand
└── src/types/             # 型定義
```

## ⚙️ 開発コマンド

```bash
# プロジェクト作成
npx create-next-app@latest bgr-v2 --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

# 必須依存関係
npm install @supabase/supabase-js @supabase/auth-ui-react
npm install @radix-ui/react-* lucide-react
npm install react-hook-form @hookform/resolvers zod
npm install zustand date-fns sharp next-seo xml2js

# テスト
npm install -D jest @testing-library/react playwright msw

# 開発・ビルド
npm run dev
npm run build
npm run lint
```

## 🏗️ 核となるファイル構成

### 1. 型定義 (`src/types/index.ts`)
```typescript
export interface Game {
  id: number
  bggId: number
  name: string
  yearPublished?: number
  minPlayers: number
  maxPlayers: number
  imageUrl?: string
  // 必ず全フィールド定義
}

export interface ApiResponse<T> {
  data: T
  success: boolean
  message?: string
}
```

### 2. Supabase設定 (`src/lib/supabase.ts`)
```typescript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL!
const supabaseKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!

export const supabase = createClient(supabaseUrl, supabaseKey)
```

### 3. BGG API (`src/lib/bgg-api.ts`)
```typescript
export async function fetchGameFromBGG(id: number): Promise<Game> {
  try {
    const response = await fetch(`https://boardgamegeek.com/xmlapi2/thing?id=${id}`)
    if (!response.ok) throw new Error(`BGG API error: ${response.status}`)
    
    const xml = await response.text()
    return parseBGGXML(xml) // 必ずパース処理を分離
  } catch (error) {
    console.error('BGG API Error:', error)
    throw new BGGApiError('Failed to fetch game data')
  }
}
```

## 🎯 コンポーネント設計パターン

### Server Component (データフェッチ)
```typescript
// app/games/page.tsx
async function GamesPage() {
  const games = await getGames() // サーバーサイド取得
  return <GamesList games={games} />
}
```

### Client Component (インタラクション)
```typescript
// components/SearchForm.tsx
'use client'
export function SearchForm() {
  const [query, setQuery] = useState('')
  // 状態管理とイベント処理
}
```

## 🔧 よく使うパターン

### フォームバリデーション
```typescript
// lib/validations.ts
export const gameSchema = z.object({
  name: z.string().min(1, '名前は必須'),
  rating: z.number().min(1).max(10)
})

// 使用例
const validatedData = gameSchema.parse(formData)
```

### エラーハンドリング
```typescript
// app/games/[id]/error.tsx
'use client'
export default function GameError({ error, reset }: ErrorProps) {
  return (
    <div>
      <h2>エラーが発生しました</h2>
      <button onClick={reset}>再試行</button>
    </div>
  )
}
```

### API Route
```typescript
// app/api/games/route.ts
export async function GET() {
  try {
    const games = await getGamesFromDB()
    return Response.json({ success: true, data: games })
  } catch (error) {
    return Response.json(
      { success: false, message: 'エラー' },
      { status: 500 }
    )
  }
}
```

## 🧪 テスト必須項目

### 1. コンポーネントテスト
```typescript
// 必ずテストすること
- 正常な表示
- エラー状態
- ユーザーインタラクション
- プロップスのバリデーション
```

### 2. API テスト
```typescript
// 必ずテストすること  
- 正常レスポンス
- エラーレスポンス (404, 500)
- 認証チェック
- 入力値検証
```

### 3. E2E テスト
```typescript
// 重要なユーザーフローをテスト
- ログイン→レビュー投稿
- ゲーム検索→詳細表示
- 管理機能
```

## 🌐 環境変数 (`.env.local`)

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_key
SUPABASE_SERVICE_ROLE_KEY=your_service_key

# OAuth
GOOGLE_CLIENT_ID=your_google_id
TWITTER_CLIENT_ID=your_twitter_id

# アプリケーション
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## 🔍 トラブルシューティング

### よくあるエラー
1. **ハイドレーションエラー**: サーバー・クライアント状態の不一致
2. **BGG APIエラー**: レート制限、XMLパースエラー
3. **Supabase認証エラー**: 環境変数、RLSポリシー
4. **型エラー**: any型使用、型定義不足

### 解決手順
```bash
# キャッシュクリア
rm -rf .next && npm run build

# 型チェック
npx tsc --noEmit

# 依存関係リセット
rm -rf node_modules package-lock.json && npm install
```

## 📊 SEO対策 (必須設定)

```typescript
// app/layout.tsx
export const metadata: Metadata = {
  title: { default: 'BGR - Board Game Review', template: '%s | BGR' },
  description: 'ボードゲームレビューサイト',
  openGraph: { /* 必須設定 */ },
  twitter: { /* 必須設定 */ }
}

// サイトマップ自動生成
// app/sitemap.ts
export default async function sitemap() {
  const games = await getAllGames()
  return games.map(game => ({
    url: `https://domain.com/games/${game.id}`,
    lastModified: game.updatedAt
  }))
}
```

## 🚀 Netlifyデプロイ

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "out"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

---

**詳細情報**: `CLAUDE.md` を参照  
**最終更新**: 2025-08-10