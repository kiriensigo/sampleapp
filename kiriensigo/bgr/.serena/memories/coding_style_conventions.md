# BGR v2 コーディング規約・スタイルガイド

## TypeScript設定

### 厳格な型チェック
- **strict mode**: 有効
- **noImplicitAny**: エラー
- **noImplicitReturns**: エラー  
- **noUnusedLocals**: エラー
- **noUnusedParameters**: エラー
- **exactOptionalPropertyTypes**: 有効
- **noUncheckedIndexedAccess**: 有効

### パス設定
```typescript
import { Component } from '@/components/Component'  // エイリアス使用
```

## ESLint ルール

### TypeScript
- `@typescript-eslint/no-unused-vars`: error
- `@typescript-eslint/no-explicit-any`: warn
- `@typescript-eslint/prefer-nullish-coalescing`: error
- `@typescript-eslint/prefer-optional-chain`: error
- `@typescript-eslint/no-non-null-assertion`: warn

### React
- `react-hooks/rules-of-hooks`: error
- `react-hooks/exhaustive-deps`: warn

### コード品質
- `no-console`: warn
- `no-debugger`: error
- `prefer-const`: error
- `no-var`: error
- `object-shorthand`: error
- `prefer-template`: error

## 命名規則

### ファイル命名
```
PascalCase.tsx       # Reactコンポーネント
camelCase.ts         # 通常のTS/JSファイル
kebab-case.tsx       # shadcn/ui components
page.tsx            # Next.js App Router pages
route.ts             # Next.js API routes
```

### 変数・関数命名
```typescript
// camelCase for variables, functions
const gameData = {}
const handleSubmit = () => {}

// PascalCase for types, interfaces, components
interface GameData {}
type GameStatus = 'active' | 'inactive'
function GameCard() {}

// UPPER_SNAKE_CASE for constants
const BGG_API_BASE_URL = 'https://...'
```

## コンポーネント設計パターン

### Props interface定義
```typescript
interface GameCardProps {
  game: Game
  className?: string
}

export function GameCard({ game, className }: GameCardProps) {}
```

### イベントハンドラー
```typescript
const handleFavoriteClick = async (e: React.MouseEvent) => {
  e.preventDefault()
  e.stopPropagation()
  // 処理
}
```

### 状態管理
```typescript
const { isAuthenticated } = useAuth()  // カスタムフック使用
const [loading, setLoading] = useState(false)  // ローカル状態
```

## スタイリング規約

### Tailwind CSS
```typescript
// cn()ユーティリティ使用
className={cn(
  "base-classes",
  "conditional-classes",
  className  // 外部からの上書き可能
)}

// 条件付きスタイル
className={`
  base-classes 
  ${condition ? 'conditional-class' : 'default-class'}
  ${className}
`}
```

### shadcn/ui variants
```typescript
const buttonVariants = cva(
  "base-styles", 
  {
    variants: {
      variant: {
        default: "...",
        destructive: "...",
      },
      size: {
        default: "...",
        sm: "...",
      }
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    }
  }
)
```

## エラーハンドリング

### try-catch
```typescript
try {
  await someAsyncOperation()
} catch (error) {
  console.error('Failed to operation:', error)
  // ユーザーフレンドリーなエラー処理
}
```

### 型ガード
```typescript
if (!game.id) return
if (!isAuthenticated) return
```

## フォーマット規則

### 関数定義
```typescript
// アロー関数 (推奨)
const formatPlayers = () => {
  // 処理
}

// 名前付き関数 (export時)
export function GameCard() {
  // 処理
}
```

### インポート順序
```typescript
// 1. React/Next.js
import React from 'react'
import Link from 'next/link'

// 2. 外部ライブラリ
import { clsx } from 'clsx'

// 3. 内部ライブラリ (@/)
import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'

// 4. 相対インポート
import './styles.css'
```

## 禁止事項

### 絶対に避けるべき
- `any` 型の多用
- `console.log` の本番コード残存
- `!` non-null assertionの乱用
- インライン関数の直接記述 (onClick={() => {}} NG)
- magic number/string の使用

### 推奨されない
- 深いネスト (3階層まで)
- 長すぎる関数 (50行以内)
- 複雑すぎるコンポーネント (single responsibility)