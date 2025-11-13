# 実践編：よくあるエラーと対処法

## 目次
- [JavaScript エラー](#javascript-エラー)
- [リソース読み込みエラー](#リソース読み込みエラー)
- [API / ネットワークエラー](#api--ネットワークエラー)
- [CSS / レイアウトの問題](#css--レイアウトの問題)
- [エラー解決の基本手順](#エラー解決の基本手順)

---

## JavaScript エラー

### 1. `Uncaught ReferenceError: XXX is not defined`

**意味：** 変数や関数 `XXX` が定義されていない

**よくある原因：**

1. **スペルミス**
   ```javascript
   // 間違い
   consol.log("Hello");  // ❌ consol → console

   // 正しい
   console.log("Hello");  // ✅
   ```

2. **JavaScript ファイルの読み込み順序**
   ```html
   <!-- 間違い -->
   <script src="main.js"></script>
   <script src="library.js"></script>  <!-- ❌ main.js より後 -->

   <!-- 正しい -->
   <script src="library.js"></script>
   <script src="main.js"></script>     <!-- ✅ 先に読み込む -->
   ```

3. **スコープの問題**
   ```javascript
   function test() {
     let myVar = "Hello";
   }
   console.log(myVar);  // ❌ myVar は関数内でしか使えない
   ```

**デバッグ方法：**

1. Console でエラーメッセージを確認
2. エラーが出ている行番号をクリック
3. 変数名のスペルや、定義されているか確認

---

### 2. `Uncaught TypeError: Cannot read property 'XXX' of null`

**意味：** `null` に対してプロパティ `XXX` を読もうとした

**よくある原因：**

```javascript
// 要素が見つからない
let button = document.querySelector('.my-button');
button.addEventListener('click', () => {});
// ❌ button が null の場合、エラーになる
```

**対処法：**

```javascript
let button = document.querySelector('.my-button');

// 方法1: 存在チェック
if (button) {
  button.addEventListener('click', () => {});
}

// 方法2: オプショナルチェイニング（モダンな方法）
button?.addEventListener('click', () => {});
```

**デバッグ方法：**

1. Console で要素が取得できているか確認
   ```javascript
   console.log(document.querySelector('.my-button'));  // null なら要素が見つかっていない
   ```

2. セレクタが正しいか Elements パネルで確認
3. JavaScript の実行タイミングが早すぎないか確認（DOM 読み込み前に実行していないか）

---

### 3. `Uncaught SyntaxError: Unexpected token`

**意味：** 構文エラー（書き方が間違っている）

**よくある原因：**

```javascript
// 括弧の閉じ忘れ
function test() {
  console.log("Hello");
// ❌ } が足りない

// カンマやセミコロンの間違い
let obj = {
  name: "太郎"
  age: 25  // ❌ カンマが足りない
};

// 引用符の閉じ忘れ
let message = "Hello;  // ❌ " が足りない
```

**対処法：**

- エディタの構文チェック機能を使う
- エラーメッセージの行番号を確認
- コードの整形ツール（Prettier など）を使う

---

### 4. `Uncaught TypeError: XXX is not a function`

**意味：** `XXX` は関数ではない

**よくある原因：**

```javascript
// 間違い1: 関数名のスペルミス
let data = [1, 2, 3];
data.forEch(item => console.log(item));  // ❌ forEch → forEach

// 間違い2: 変数を関数と勘違い
let myVar = "Hello";
myVar();  // ❌ myVar は文字列であって関数ではない

// 間違い3: メソッドが存在しない
let str = "Hello";
str.trim();   // ✅ 正しい
str.upper();  // ❌ upper というメソッドはない（正しくは toUpperCase）
```

**対処法：**

1. 関数名のスペルを確認
2. Console で型を確認
   ```javascript
   console.log(typeof myVar);  // "function" でなければ関数ではない
   ```

---

## リソース読み込みエラー

### 5. `404 Not Found`

**意味：** ファイルが見つからない

**よくある原因：**

```html
<!-- 間違い: パスが間違っている -->
<img src="images/logo.png">  <!-- ❌ images フォルダがない -->
<script src="js/main.js"></script>  <!-- ❌ js フォルダがない -->

<!-- 正しい: 正しいパス -->
<img src="./logo.png">  <!-- ✅ 同じフォルダ -->
<script src="main.js"></script>  <!-- ✅ 同じフォルダ -->
```

**デバッグ方法：**

1. Network パネルを開く
2. 404 のリソースを見つける
3. Name をクリックして、リクエストされている URL を確認
4. 実際のファイル構造と比較

**よくあるミス：**

- ファイル名の大文字・小文字が違う（`Logo.png` と `logo.png` は別ファイル）
- 拡張子が間違っている（`.jpg` と `.png`）
- 相対パスが間違っている

**対処法：**

```
プロジェクト構造:
├── index.html
├── style.css
└── images/
    └── logo.png

HTML からの参照:
<img src="./images/logo.png">  ✅
<img src="images/logo.png">    ✅
<img src="/images/logo.png">   ✅（ルートから）
```

---

### 6. `Failed to load resource: net::ERR_BLOCKED_BY_CLIENT`

**意味：** 広告ブロッカーなどにブロックされた

**よくある原因：**

- 広告ブロッカーが、`ad.js` や `analytics.js` などをブロック
- ファイル名に `ad`、`banner`、`tracking` などが含まれている

**対処法：**

- ファイル名を変更する（`ad.js` → `advertisement.js`）
- 広告ブロッカーを一時的に無効にしてテスト

---

## API / ネットワークエラー

### 7. `CORS error`

**完全なエラーメッセージ：**
```
Access to fetch at 'https://api.example.com' from origin 'http://localhost:3000'
has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present
```

**意味：** サーバーが CORS（クロスオリジンリソース共有）を許可していない

**よくある原因：**

- 異なるドメインの API を呼び出している
- サーバー側で CORS 設定がされていない

**デバッグ方法：**

1. Network パネルで該当リクエストをクリック
2. Headers タブで `Access-Control-Allow-Origin` があるか確認

**対処法：**

1. **サーバー側で CORS を有効にする（推奨）**
   ```javascript
   // Node.js + Express の例
   app.use((req, res, next) => {
     res.header('Access-Control-Allow-Origin', '*');
     res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
     next();
   });
   ```

2. **プロキシを使う（開発環境のみ）**
   - Vite、Create React App などは設定でプロキシ可能

3. **CORS プロキシを使う（テスト用のみ）**
   ```javascript
   // 注意: 本番では使わないこと
   fetch('https://cors-anywhere.herokuapp.com/https://api.example.com')
   ```

---

### 8. `500 Internal Server Error`

**意味：** サーバー側でエラーが発生した

**よくある原因：**

- サーバー側のコードにバグがある
- データベース接続エラー
- 不正なリクエストを送っている

**デバッグ方法：**

1. Network パネルで該当リクエストをクリック
2. **Response** タブでエラーメッセージを確認
3. **Payload** タブで送信したデータを確認

**対処法：**

- サーバーのログを確認する
- 送信しているデータの形式が正しいか確認
- API ドキュメントを見て、リクエストが正しいか確認

---

### 9. `Failed to fetch`

**意味：** ネットワークエラー（接続できない）

**よくある原因：**

- インターネット接続が切れている
- サーバーが起動していない
- URL が間違っている

**デバッグ方法：**

1. ブラウザでその URL を直接開いてみる
2. サーバーが起動しているか確認
3. URL のスペルミスがないか確認

---

## CSS / レイアウトの問題

### 10. 要素が表示されない

**チェックリスト：**

1. **`display: none` になっていないか**
   - Elements → Styles で確認

2. **`visibility: hidden` になっていないか**
   - Elements → Styles で確認

3. **`opacity: 0` になっていないか**
   - Elements → Computed で確認

4. **親要素で `overflow: hidden` になっていないか**
   - 親要素のサイズを超えて表示されようとしている

5. **z-index の問題**
   - 他の要素の後ろに隠れていないか

6. **色の問題**
   - 背景色と文字色が同じになっていないか

**デバッグ方法：**

```javascript
// Console で要素が存在するか確認
let element = document.querySelector('.my-element');
console.log(element);  // null なら存在しない

// スタイルを確認
console.log(window.getComputedStyle(element).display);
```

---

### 11. レイアウトが崩れる

**チェックポイント：**

1. **Box Model を確認**
   - Elements → Computed タブでボックスモデル図を見る
   - 予想外の `margin` や `padding` がないか

2. **親要素のサイズ確認**
   - 親要素が `width: 0` や `height: 0` になっていないか

3. **position プロパティ**
   - `position: absolute` で意図しない位置に配置されていないか

4. **float の clearfix**
   - `float` を使っている場合、親要素が高さを持たない問題

**デバッグ方法：**

- Elements パネルで要素にマウスを乗せて、視覚的に範囲を確認
- Computed タブでボックスモデルを確認

---

## エラー解決の基本手順

### ステップ 1: エラーメッセージを読む

- Console パネルを開いて、赤色のエラーを探す
- エラーの種類と内容を確認

### ステップ 2: エラーの場所を特定

- ファイル名と行番号をクリック
- 該当するコードを確認

### ステップ 3: 原因を特定

- 変数の中身を `console.log()` で確認
- Elements パネルで要素が存在するか確認
- Network パネルでリソースが読み込まれているか確認

### ステップ 4: 仮説を立てて試す

- コードを少し変更して試す
- DevTools で一時的に編集して試す

### ステップ 5: 解決したら、実際のファイルに反映

- DevTools での変更は一時的なので、ファイルに書き直す

---

## デバッグのコツ

### 1. console.log() を活用する

```javascript
console.log("ここまで来た");
console.log("変数の値:", myVar);
console.log("データ:", data);
```

### 2. コメントアウトで切り分ける

```javascript
// 問題がある部分をコメントアウトして、原因を特定
// doSomething();
```

### 3. ブラウザのキャッシュをクリアする

`Ctrl + Shift + R`（Mac: `Command + Shift + R`）で強制リロード

### 4. シークレットモードで試す

拡張機能の影響を排除してテスト

### 5. エラーメッセージをコピーして検索

Google や Stack Overflow で解決策を探す

---

## まとめ

- エラーメッセージをしっかり読むことが第一歩
- Console、Elements、Network の3つのパネルを使い分ける
- `console.log()` で変数の中身を確認
- DevTools で試してから、ファイルに反映
- 困ったら、エラーメッセージで検索する

**次へ：** [便利な Tips](./06_tips.md)
