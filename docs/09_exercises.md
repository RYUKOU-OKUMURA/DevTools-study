# 実践演習：DevTools でバグを解決しよう

**実際に手を動かして、DevTools の使い方をマスターしましょう！**

このページでは、意図的にバグを仕込んだサンプルコードを用意しています。
DevTools を使って、バグを見つけて修正してください。

---

## 📚 演習の使い方

### ステップ1: サンプルファイルを用意

このリポジトリの `exercises/` フォルダに、演習用のファイルがあります。

```
exercises/
├── exercise-01.html  # 演習1: Console エラー
├── exercise-02.html  # 演習2: 要素が見つからない
├── exercise-03.html  # 演習3: CSS が効かない
├── exercise-04.html  # 演習4: API エラー
└── exercise-05.html  # 演習5: ロジックのバグ
```

### ステップ2: ブラウザで開く

ファイルをブラウザで開いて、DevTools を使ってバグを探します。

### ステップ3: 自分で解決してみる

まずは自分で DevTools を使ってバグを見つけてください。

### ステップ4: 答えを確認

各演習の「答えと解説」で、正しい解決方法を確認できます。

---

## 演習1: Console エラーを解決しよう

### 🎯 目標
- Console パネルでエラーを確認
- エラーメッセージを読み解く
- エラーの場所を特定して修正

### 📝 問題

`exercise-01.html` をブラウザで開くと、ボタンをクリックしても何も起こりません。

**期待する動作：**
- ボタンをクリックすると、「こんにちは！」とアラートが表示される

**現在の動作：**
- 何も起こらない

### 🔍 ヒント

1. DevTools の Console パネルを開く
2. 赤いエラーメッセージを探す
3. エラーの種類を確認（ReferenceError? TypeError?）
4. ファイル名と行番号をクリック

<details>
<summary><strong>答えと解説を見る</strong></summary>

### 答え

**バグの内容：**
```javascript
// 間違い
button.addEventListener('click', showMessge);  // ❌ showMessge → showMessage
```

**修正後：**
```javascript
// 正しい
button.addEventListener('click', showMessage);  // ✅
```

### 解説

**Console のエラー：**
```
Uncaught ReferenceError: showMessge is not defined
    at HTMLButtonElement.<anonymous> (exercise-01.html:15)
```

**エラーの読み解き方：**
1. **ReferenceError**：変数や関数が定義されていない
2. **showMessge is not defined**：`showMessge` が見つからない
3. **exercise-01.html:15**：15行目で発生

**デバッグ手順：**
1. Console でエラーを確認
2. `exercise-01.html:15` をクリック
3. Sources パネルで該当行を確認
4. スペルミスを発見（`showMessge` → `showMessage`）
5. 修正して保存、ページをリロード

</details>

---

## 演習2: 要素が見つからないエラー

### 🎯 目標
- `null` エラーの原因を特定
- Elements パネルで要素の存在を確認
- セレクタの間違いを見つける

### 📝 問題

`exercise-02.html` をブラウザで開くと、エラーが出ています。

**期待する動作：**
- ページが読み込まれると、ボックスの色が青くなる

**現在の動作：**
- エラーが出て、色が変わらない

### 🔍 ヒント

1. Console でエラーを確認
2. `Cannot read property 'style' of null` → 何かが `null` になっている
3. Elements パネルで要素を探す
4. セレクタが正しいか確認

<details>
<summary><strong>答えと解説を見る</strong></summary>

### 答え

**バグの内容：**
```javascript
// HTML: <div class="box">...</div>

// 間違い
let box = document.querySelector('.boxs');  // ❌ .boxs → .box
```

**修正後：**
```javascript
// 正しい
let box = document.querySelector('.box');  // ✅
```

### 解説

**Console のエラー：**
```
Uncaught TypeError: Cannot read property 'style' of null
    at exercise-02.html:13
```

**デバッグ手順：**
1. Console で `box` の値を確認：
   ```javascript
   > box
   null  // ← 要素が見つかっていない
   ```
2. Elements パネルで HTML を確認：
   ```html
   <div class="box">...</div>  // ← クラス名は "box"
   ```
3. セレクタを確認：
   ```javascript
   document.querySelector('.boxs')  // ← "boxs" になっている！
   ```
4. 修正：`.boxs` → `.box`

**よくある原因：**
- セレクタのスペルミス
- クラス名と ID の間違い（`.box` と `#box`）
- 要素が存在しない

</details>

---

## 演習3: CSS が効かない

### 🎯 目標
- Elements パネルで CSS を確認
- Styles パネルで取り消し線を見つける
- CSS の優先順位を理解

### 📝 問題

`exercise-03.html` をブラウザで開くと、ボタンの色が赤くなりません。

**期待する動作：**
- ボタンの背景色が赤

**現在の動作：**
- ボタンの背景色が青のまま

### 🔍 ヒント

1. Elements パネルでボタンを選択（Ctrl + Shift + C）
2. Styles パネルで CSS を確認
3. 取り消し線があるか確認
4. どの CSS が優先されているか確認

<details>
<summary><strong>答えと解説を見る</strong></summary>

### 答え

**バグの内容：**
```css
/* スタイル1: 詳細度が低い */
.button {
  background-color: red;  /* ← これが効いていない */
}

/* スタイル2: 詳細度が高い */
button.button {
  background-color: blue;  /* ← こちらが優先されている */
}
```

**修正方法1: 詳細度を上げる**
```css
button.button {
  background-color: red;  /* ✅ 詳細度を同じにする */
}
```

**修正方法2: !important を使う**
```css
.button {
  background-color: red !important;  /* ✅ 強制的に優先 */
}
```

### 解説

**デバッグ手順：**
1. Elements パネルでボタンを選択
2. Styles パネルを確認：
   ```css
   button.button {
     background-color: blue;  /* ← これが適用されている */
   }

   .button {
     background-color: red;  /* ← 取り消し線がある */
   }
   ```
3. 詳細度が高い方が優先されていることを確認
4. 詳細度を上げるか、`!important` を使う

**CSS の詳細度：**
- `button.button`（要素 + クラス）> `.button`（クラスのみ）
- ID セレクタ > クラスセレクタ > 要素セレクタ

</details>

---

## 演習4: API エラーを解決しよう

### 🎯 目標
- Network パネルで API 通信を確認
- ステータスコードを理解
- レスポンスの中身を確認

### 📝 問題

`exercise-04.html` をブラウザで開いて「データ取得」ボタンをクリックすると、エラーが出ます。

**期待する動作：**
- ユーザー情報が表示される

**現在の動作：**
- エラーが表示される

### 🔍 ヒント

1. Network パネルを開く
2. Fetch/XHR フィルタを選択
3. ボタンをクリック
4. Status を確認（404? 500?）
5. Response タブでエラーメッセージを確認

<details>
<summary><strong>答えと解説を見る</strong></summary>

### 答え

**バグの内容：**
```javascript
// 間違い
fetch('https://jsonplaceholder.typicode.com/user/1')  // ❌ user → users
```

**修正後：**
```javascript
// 正しい
fetch('https://jsonplaceholder.typicode.com/users/1')  // ✅
```

### 解説

**デバッグ手順：**
1. Network パネル → Fetch/XHR フィルタ
2. リクエストをクリック
3. **Status: 404 Not Found** → URL が間違っている
4. Headers タブで Request URL を確認：
   ```
   https://jsonplaceholder.typicode.com/user/1
   ```
5. API ドキュメントを確認 → 正しくは `/users/1`（複数形）
6. 修正して再実行

**404 エラーの主な原因：**
- URL のスペルミス
- エンドポイントが存在しない
- API のバージョンが古い

</details>

---

## 演習5: ロジックのバグを見つけよう

### 🎯 目標
- Sources パネルでブレークポイントを使う
- Step Over で1行ずつ実行
- Scope パネルで変数を確認

### 📝 問題

`exercise-05.html` をブラウザで開いて、価格を入力して「計算」ボタンをクリックすると、おかしな結果が表示されます。

**入力：**
- 価格: 1000
- 数量: 3

**期待する結果：**
- 合計: 3000円

**実際の結果：**
- 合計: 10003円（おかしい！）

### 🔍 ヒント

1. Sources パネルを開く
2. `let total = price + quantity;` にブレークポイント
3. ボタンをクリック
4. Scope パネルで `price` と `quantity` の値を確認
5. 型を確認（`typeof price`）

<details>
<summary><strong>答えと解説を見る</strong></summary>

### 答え

**バグの内容：**
```javascript
// 間違い
let price = document.getElementById('price').value;      // ← 文字列
let quantity = document.getElementById('quantity').value; // ← 文字列
let total = price + quantity;  // ❌ "1000" + "3" = "10003"
```

**修正後：**
```javascript
// 正しい
let price = Number(document.getElementById('price').value);      // ✅ 数値に変換
let quantity = Number(document.getElementById('quantity').value); // ✅ 数値に変換
let total = price * quantity;  // ✅ 掛け算
```

### 解説

**デバッグ手順：**
1. Sources パネルでブレークポイントを設定
2. ボタンをクリック → ブレークポイントで停止
3. Scope パネルで変数を確認：
   ```
   price: "1000"     ← 文字列！
   quantity: "3"     ← 文字列！
   ```
4. Console で確認：
   ```javascript
   > typeof price
   "string"

   > "1000" + "3"
   "10003"  // ← 文字列結合になる

   > Number("1000") + Number("3")
   1003  // ← まだ違う...

   > Number("1000") * Number("3")
   3000  // ← 正解！
   ```
5. 修正：`Number()` で数値に変換し、`*` で掛け算

**よくある間違い：**
- `input.value` は常に文字列
- `+` は文字列結合にもなる
- 計算前に必ず `Number()` で変換

</details>

---

## 演習6: イベントが動かない

### 🎯 目標
- Event Listeners パネルでイベントを確認
- DOMContentLoaded のタイミングを理解

### 📝 問題

`exercise-06.html` をブラウザで開くと、ボタンをクリックしても何も起こりません。

**期待する動作：**
- ボタンをクリックすると、メッセージが表示される

**現在の動作：**
- 何も起こらない（エラーも出ない）

### 🔍 ヒント

1. Elements パネルでボタンを選択
2. Event Listeners タブを開く
3. `click` イベントがあるか確認
4. JavaScript の実行タイミングを確認

<details>
<summary><strong>答えと解説を見る</strong></summary>

### 答え

**バグの内容：**
```html
<!DOCTYPE html>
<html>
<head>
  <script>
    // ❌ この時点では <button> がまだ存在しない
    let button = document.getElementById('myButton');
    button.addEventListener('click', () => {
      alert('こんにちは！');
    });
  </script>
</head>
<body>
  <button id="myButton">クリック</button>
</body>
</html>
```

**修正方法1: DOMContentLoaded を使う**
```javascript
document.addEventListener('DOMContentLoaded', () => {
  let button = document.getElementById('myButton');
  button.addEventListener('click', () => {
    alert('こんにちは！');
  });
});
```

**修正方法2: script タグを </body> の前に移動**
```html
<body>
  <button id="myButton">クリック</button>

  <script>
    let button = document.getElementById('myButton');
    button.addEventListener('click', () => {
      alert('こんにちは！');
    });
  </script>
</body>
```

### 解説

**デバッグ手順：**
1. Console で確認：
   ```javascript
   > document.getElementById('myButton')
   null  // ← ボタンが見つからない
   ```
2. Elements パネルで Event Listeners を確認 → イベントがない
3. 原因：JavaScript が HTML より先に実行されている
4. 解決：`DOMContentLoaded` または script タグの位置を変更

**JavaScript の実行タイミング：**
- `<head>` 内の `<script>` → HTML 読み込み前に実行
- `</body>` 前の `<script>` → HTML 読み込み後に実行
- `DOMContentLoaded` → DOM が完全に読み込まれた後に実行

</details>

---

## 🎓 チャレンジ問題

### チャレンジ1: 複合的なバグ

以下のコードには **3つ** のバグがあります。すべて見つけて修正してください。

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .box {
      width: 100px;
      height: 100px;
      background-color: red;
    }
  </style>
</head>
<body>
  <div class="boxs">ボックス</div>
  <button id="changeColor">色を変える</button>

  <script>
    let button = document.getElementById('changeColor');
    let box = document.querySelector('.box');

    button.addEventListner('click', () => {
      box.style.backgroundColor = 'blue';
    });
  </script>
</body>
</html>
```

<details>
<summary><strong>答えを見る</strong></summary>

**バグ1: HTML のクラス名が間違っている**
```html
<!-- 間違い -->
<div class="boxs">ボックス</div>

<!-- 正しい -->
<div class="box">ボックス</div>
```

**バグ2: addEventListener のスペルミス**
```javascript
// 間違い
button.addEventListner('click', () => { ... });

// 正しい
button.addEventListener('click', () => { ... });
```

**バグ3: セレクタが間違っている（実はバグ1と関連）**

すべて修正すると動きます！

</details>

---

## 📚 さらに練習したい人へ

### 実際のプロジェクトで練習

1. **自分のプロジェクトでバグを探す**
   - 意図的にバグを仕込んで、DevTools で見つける練習

2. **他のサイトで試す**
   - お気に入りのサイトを開いて、DevTools で構造を確認
   - CSS を変更してデザインを試す

3. **コードレビュー**
   - 他の人のコードを DevTools で確認
   - バグがないかチェック

### オンラインの練習サイト

- **freeCodeCamp**：Web開発の基礎を学べる
- **JavaScript30**：30日間で JavaScript を学ぶ
- **CodePen**：他の人のコードを見て学ぶ

---

## まとめ

この演習を通して、以下を学びました：

✅ Console パネルでエラーを読み解く
✅ Elements パネルで CSS をデバッグ
✅ Network パネルで API エラーを特定
✅ Sources パネルでロジックのバグを見つける
✅ Event Listeners でイベントを確認

**重要なこと：**
- DevTools は**試行錯誤**の場
- 何度も練習することで、自然に使えるようになる
- 実際のプロジェクトで使うことが一番の学習

**DevTools を使いこなせば、どんなバグも怖くありません！**

---

**前へ：** [トラブルシューティング](./08_troubleshooting.md) | **トップへ：** [README](../README.md)
