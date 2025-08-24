# Twitterシェア機能 - モックアップ

## 🐦 Twitter連携フロー設計

### 投稿完了画面

```
┌─────────────────────────────────────────┐
│ ✅ レビューを投稿しました！              │
├─────────────────────────────────────────┤
│                                         │
│ 🎉 レビューありがとうございます！       │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │  📊 あなたの評価                   │ │
│ │                                   │ │
│ │  総合得点: ⭐⭐⭐⭐⭐⭐⭐⭐ 8.0    │ │
│ │  複雑さ: 🧩🧩🧩 3                  │ │
│ │  運要素: 🎲🎲 2                    │ │
│ │  相互作用: 👥👥👥👥 4              │ │
│ │  待機時間: ⏰ 1                    │ │
│ │                                   │ │
│ │  💬 "戦略性が高くて楽しい！"       │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │    🐦 Twitterでシェアする           │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │    📋 他のレビューを見る            │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │    🎲 他のゲームを探す              │ │
│ └─────────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

## 📱 ツイート内容プレビュー

### モバイル版プレビューモーダル

```
┌─────────────────────────────────────────┐
│ ✕                    Twitterでシェア     │
├─────────────────────────────────────────┤
│                                         │
│ 📝 ツイート内容プレビュー               │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │ 🎲 Wingspan をレビューしました！    │ │
│ │                                   │ │
│ │ ⭐総合: 8.0/10                     │ │
│ │ 🧩複雑さ: 3/5                      │ │
│ │ 🎲運要素: 2/5                      │ │
│ │ 👥相互作用: 4/5                    │ │
│ │ ⏰待機時間: 1/5                    │ │
│ │                                   │ │
│ │ おすすめ人数: 2-4人                │ │
│ │ 戦略性が高くて楽しい！              │ │
│ │                                   │ │
│ │ #BGR #ボードゲーム #Wingspan       │ │
│ │ https://bgrq.netlify.app/games/... │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ 📊 ツイート長: 247/280文字             │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │           🐦 ツイートする           │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │             キャンセル              │ │
│ └─────────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

## 💻 デスクトップ版ツイートプレビュー

```
┌─────────────────────────────────────────────────────────────────────┐
│                          Twitterでシェア                 ✕        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│ ┌─────────────────────────┐  ┌─────────────────────────────────────┐ │
│ │                         │  │ 📝 ツイート内容                     │ │
│ │   [ゲーム画像]          │  │                                     │ │
│ │     200x200             │  │ ┌─────────────────────────────────┐ │ │
│ │                         │  │ │ 🎲 Wingspan をレビューしました！ │ │ │
│ │                         │  │ │                               │ │ │
│ │ Wingspan                │  │ │ ⭐総合: 8.0/10                 │ │ │
│ │ 2019年 | 1-5人          │  │ │ 🧩複雑さ: 3/5                  │ │ │
│ │ 40-70分                 │  │ │ 🎲運要素: 2/5                  │ │ │
│ │                         │  │ │ 👥相互作用: 4/5                │ │ │
│ │ あなたの評価:           │  │ │ ⏰待機時間: 1/5                │ │ │
│ │ ⭐⭐⭐⭐⭐⭐⭐⭐ 8.0     │  │ │                               │ │ │
│ │ 🧩🧩🧩 3                │  │ │ おすすめ人数: 2-4人            │ │ │
│ │ 🎲🎲 2                  │  │ │ 戦略性が高くて楽しい！         │ │ │
│ │ 👥👥👥👥 4              │  │ │                               │ │ │
│ │ ⏰ 1                    │  │ │ #BGR #ボードゲーム #Wingspan   │ │ │
│ │                         │  │ │ https://bgrq.netlify.app/... │ │ │
│ │                         │  │ └─────────────────────────────────┘ │ │
│ └─────────────────────────┘  │                                     │ │
│                              │ 📊 247/280文字                      │ │
│                              │                                     │ │
│                              │ ┌─────────────┐ ┌─────────────────┐ │ │
│                              │ │キャンセル   │ │🐦 ツイートする   │ │ │
│                              │ └─────────────┘ └─────────────────┘ │ │
│                              └─────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔧 ツイート内容テンプレート

### 基本テンプレート
```typescript
const generateTweetContent = (review: Review, game: Game): string => {
  const emoji = {
    overall: '⭐',
    complexity: '🧩', 
    luck: '🎲',
    interaction: '👥',
    downtime: '⏰'
  };

  const playerCount = review.recommendedPlayers.length > 0 
    ? `${Math.min(...review.recommendedPlayers)}-${Math.max(...review.recommendedPlayers)}人`
    : '';

  const tags = review.tags.slice(0, 2).map(tag => `#${tag}`).join(' ');
  
  return `🎲 ${game.name} をレビューしました！

${emoji.overall}総合: ${review.overallScore}/10
${emoji.complexity}複雑さ: ${review.complexity}/5
${emoji.luck}運要素: ${review.luckFactor}/5
${emoji.interaction}相互作用: ${review.interaction}/5
${emoji.downtime}待機時間: ${review.downtime}/5

${playerCount ? `おすすめ人数: ${playerCount}` : ''}
${review.comment || ''}

#BGR #ボードゲーム #${game.name.replace(/\s+/g, '')} ${tags}
${process.env.NEXT_PUBLIC_APP_URL}/games/${game.id}`;
};
```

### 短縮版テンプレート（280文字制限）
```typescript
const generateShortTweetContent = (review: Review, game: Game): string => {
  const baseContent = `🎲 ${game.name} ⭐${review.overallScore}/10

🧩複雑さ${review.complexity} 🎲運要素${review.luckFactor} 👥相互作用${review.interaction} ⏰待機${review.downtime}

${review.comment ? `"${review.comment.slice(0, 60)}${review.comment.length > 60 ? '...' : ''}"` : ''}

#BGR #ボードゲーム
${process.env.NEXT_PUBLIC_APP_URL}/games/${game.id}`;

  return baseContent.length > 280 
    ? baseContent.slice(0, 270) + '...'
    : baseContent;
};
```

## 🎨 UIコンポーネント設計

### TwitterShareModal Component
```typescript
interface TwitterShareModalProps {
  isOpen: boolean;
  onClose: () => void;
  review: Review;
  game: Game;
  onShare: () => void;
}

const TwitterShareModal: React.FC<TwitterShareModalProps> = ({
  isOpen,
  onClose,
  review,
  game,
  onShare
}) => {
  const [tweetContent, setTweetContent] = useState('');
  const [isSharing, setIsSharing] = useState(false);

  useEffect(() => {
    const content = generateTweetContent(review, game);
    if (content.length > 280) {
      setTweetContent(generateShortTweetContent(review, game));
    } else {
      setTweetContent(content);
    }
  }, [review, game]);

  const handleShare = async () => {
    setIsSharing(true);
    try {
      const twitterUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(tweetContent)}`;
      window.open(twitterUrl, '_blank', 'width=550,height=420');
      onShare();
    } catch (error) {
      console.error('Twitter share failed:', error);
    } finally {
      setIsSharing(false);
    }
  };

  return (
    <Modal isOpen={isOpen} onClose={onClose}>
      <div className="twitter-share-modal">
        {/* Modal content */}
      </div>
    </Modal>
  );
};
```

## 📊 投稿成功画面設計

### ReviewSuccessPage Component
```css
.review-success {
  background: linear-gradient(135deg, #f0fdf4 0%, #dcfce7 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.success-card {
  background: white;
  border-radius: 20px;
  padding: 40px;
  max-width: 500px;
  text-align: center;
  box-shadow: 0 20px 40px rgba(0,0,0,0.1);
  border: 1px solid #22c55e;
}

.success-icon {
  width: 80px;
  height: 80px;
  background: #22c55e;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 24px;
  animation: bounce 0.6s ease-out;
}

.review-summary {
  background: #f8fafc;
  border-radius: 12px;
  padding: 20px;
  margin: 24px 0;
  text-align: left;
}

.action-buttons {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-top: 32px;
}

.twitter-share-btn {
  background: #1da1f2;
  color: white;
  padding: 16px 24px;
  border-radius: 12px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  transition: all 0.2s ease;
}

.twitter-share-btn:hover {
  background: #0d8bd9;
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(29, 161, 242, 0.3);
}
```

## 🔗 Twitter Web Intent API

### URL生成関数
```typescript
const createTwitterShareUrl = (content: string): string => {
  const params = new URLSearchParams({
    text: content,
    via: 'BGR_Official', // 公式アカウント
    hashtags: 'BGR,ボードゲーム',
    related: 'BGR_Official:ボードゲームレビューサイト'
  });

  return `https://twitter.com/intent/tweet?${params.toString()}`;
};
```

### 開発者向けメモ
```typescript
// Twitter Web Intent パラメータ
interface TwitterIntentParams {
  text?: string;        // ツイート本文
  url?: string;         // 共有URL
  hashtags?: string;    // ハッシュタグ（カンマ区切り）
  via?: string;         // 経由アカウント
  related?: string;     // 関連アカウント
  in_reply_to?: string; // 返信先ID
}
```

## 📱 モバイル最適化

### モバイル専用スタイル
```css
@media (max-width: 767px) {
  .twitter-share-modal {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: white;
    border-radius: 20px 20px 0 0;
    padding: 24px 20px 40px;
    animation: slideUp 0.3s ease-out;
  }

  .tweet-preview {
    background: #f1f5f9;
    border-radius: 12px;
    padding: 16px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto;
    font-size: 16px;
    line-height: 1.4;
    margin-bottom: 20px;
  }

  .character-count {
    text-align: right;
    font-size: 14px;
    color: #64748b;
    margin-bottom: 20px;
  }

  .character-count.warning {
    color: #f59e0b;
  }

  .character-count.error {
    color: #ef4444;
  }
}
```

---

**作成日**: 2025-08-22  
**関連ドキュメント**: review-form-wireframes.md, 004_review_system_mockup.md