# 🎨 カスタマイズガイド

このドキュメントでは、ワークアウトアプリをカスタマイズする方法を詳しく説明します。

## 📝 アスカのセリフをカスタマイズ

### 場所
`workout_app_updated.html` の393行目あたり

### コード
```javascript
const asukaComments = {
    success: [
        // オーバーロード成功時のセリフ（10個）
    ],
    failure: [
        // オーバーロード失敗時のセリフ（10個）
    ]
};
```

### 例：新しいセリフを追加
```javascript
const asukaComments = {
    success: [
        "やるじゃない！前回より重量上がってるわよ！",
        "あなたの新しいセリフをここに追加！",
        // ...他のセリフ
    ],
    failure: [
        "ちょっと！何やってんのよ！",
        "あなたの新しいセリフをここに追加！",
        // ...他のセリフ
    ]
};
```

## 🏋️ 種目を追加する

### ステップ1: 種目データを追加

`exerciseStrategies` オブジェクトに新しい種目を追加（421行目あたり）

```javascript
'あなたの種目名': {
    type: 'compound',           // 'compound'（コンパウンド）または 'isolation'（アイソレーション）
    repRange: '8-12',           // 推奨回数範囲
    description: '種目の説明文',
    strategy: '重量を優先的に増やす',  // または '回数を優先的に増やす' or '回数と重量をバランスよく'
    weightIncrements: [2.5, 1.25, 5],  // 重量増加の提案（kg単位）
    tips: 'フォームのアドバイス',
    bodyPart: '胸'              // '胸', '背中', '脚', '肩', '腕' のいずれか
}
```

### ステップ2: 選択メニューに追加

HTMLの `<select id="exercise">` 要素に追加（268行目あたり）

```html
<optgroup label="胸">
    <option value="あなたの種目名">あなたの種目名</option>
</optgroup>
```

### 完全な例：「ケーブルクロスオーバー」を追加

#### exerciseStrategies に追加:
```javascript
'ケーブルクロスオーバー': {
    type: 'isolation',
    repRange: '12-15',
    description: '胸筋の内側を狙う。ケーブルで一定の負荷。',
    strategy: '回数を優先的に増やす',
    weightIncrements: [2.5, 5],
    tips: '胸の前で手をクロスさせる。可動域を最大限に。',
    bodyPart: '胸'
}
```

#### HTML select に追加:
```html
<optgroup label="胸">
    <option value="ベンチプレス">ベンチプレス</option>
    <option value="インクラインベンチプレス">インクラインベンチプレス</option>
    <option value="ダンベルフライ">ダンベルフライ</option>
    <option value="ケーブルクロスオーバー">ケーブルクロスオーバー</option>
</optgroup>
```

## 🎨 デザインをカスタマイズ

### 色を変更

CSSの `<style>` セクション内で色を変更できます（8行目〜）

#### 主要な色の場所:

**背景グラデーション** (17行目):
```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```

**ボタンの色** (87行目):
```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```

**アスカのコメント背景** (278行目):
```css
background: linear-gradient(135deg, #ffeaa7 0%, #fdcb6e 100%);
```

### 例：青系から赤系に変更
```css
/* 元の色 */
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

/* 赤系に変更 */
background: linear-gradient(135deg, #ff6b6b 0%, #ee5a6f 100%);
```

## 📊 グラフをカスタマイズ

### グラフの表示数を変更

**総負荷量グラフの表示日数** (658行目):
```javascript
const dates = Object.keys(dateGroups).reverse().slice(-10);  // 最新10日分
```

`-10` を `-20` に変更すると20日分表示されます。

### グラフの色を変更

**折れ線グラフの色** (672行目):
```javascript
borderColor: '#667eea',
backgroundColor: 'rgba(102, 126, 234, 0.1)',
```

**棒グラフの色** (719行目):
```javascript
backgroundColor: 'rgba(102, 126, 234, 0.7)',
borderColor: '#667eea',
```

## 🔢 オーバーロード提案をカスタマイズ

### 提案の種類を追加/削除

`generateSuggestions` 関数内（677行目〜）で提案を編集できます。

### 例：セット数を2つ増やす提案を追加
```javascript
// 既存のセット数+1の提案の後に追加
const newSets2 = workout.sets + 2;
const volumeSets2 = workout.weight * workout.reps * newSets2;
suggestions.push({
    title: 'セット数を+2増やす',
    desc: `${workout.weight}kg × ${workout.reps}回 × ${newSets2}セット`,
    volume: volumeSets2,
    increase: ((volumeSets2 - workout.volume) / workout.volume * 100).toFixed(1),
    priority: ''
});
```

## 📱 レスポンシブデザインを調整

画面サイズに応じたレイアウトを調整するには、メディアクエリを追加します：

```css
@media (max-width: 768px) {
    .three-cols {
        grid-template-columns: 1fr;  /* スマホでは1列に */
    }
    
    h1 {
        font-size: 1.8em;  /* タイトルを小さく */
    }
}
```

## 💾 LocalStorageのキーを変更

データ保存のキー名を変更する場合（491行目）:

```javascript
let workoutHistory = JSON.parse(localStorage.getItem('workoutHistory_v4')) || [];
```

`'workoutHistory_v4'` を別の名前に変更すると、新しいデータ領域として扱われます。

## 🎯 その他のカスタマイズアイデア

1. **アスカの画像を追加**: アスカのアイコン画像を表示
2. **音声を追加**: セリフに合わせて音声を再生
3. **目標設定機能**: 目標体重や目標回数を設定
4. **統計機能の拡張**: 週間/月間の統計表示
5. **テーマ切り替え**: ダークモード対応

---

質問や追加のカスタマイズ方法が必要な場合は、GitHubのIssuesで質問してください！
