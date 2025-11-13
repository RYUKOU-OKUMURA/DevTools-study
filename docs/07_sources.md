# Sources パネル：ブレークポイントで本格デバッグ

## 目次
- [Sources パネルとは](#sources-パネルとは)
- [画面の見方](#画面の見方)
- [ブレークポイントの基本](#ブレークポイントの基本)
- [ステップ実行](#ステップ実行)
- [変数の確認方法](#変数の確認方法)
- [実践：よくあるデバッグシナリオ](#実践よくあるデバッグシナリオ)
- [便利な機能](#便利な機能)

---

## Sources パネルとは

**Sources パネル** は、JavaScript のコードを **1行ずつ実行しながら** デバッグできる強力なツールです。

### Console との違い

| 機能 | Console | Sources |
|------|---------|---------|
| **エラーメッセージを見る** | ⭕ 得意 | △ 可能 |
| **ログを確認** | ⭕ 得意 | △ 可能 |
| **コードを止めて確認** | ❌ できない | ⭕ **得意** |
| **1行ずつ実行** | ❌ できない | ⭕ **得意** |
| **変数の変化を追う** | △ console.log()が必要 | ⭕ **自動で見える** |

### こんな時に使います

- **「なぜこの値になるのか分からない」** → 変数の変化を追える
- **「if文のどちらに進んでいるか分からない」** → 実際の動きを確認できる
- **「ループのどこでエラーになるか分からない」** → 該当箇所で止められる
- **「API から取得したデータの中身を確認したい」** → データ取得時点で止めて確認

**Console で解決できないバグは、Sources パネルで解決できます！**

---

## 画面の見方

Sources パネルは3つのエリアに分かれています：

```
┌─────────────────────────────────────────────────────┐
│ Sources                                             │
├──────────────┬──────────────────────┬───────────────┤
│              │                      │               │
│ ①ファイル    │  ②コードエディタ    │ ③デバッグ情報│
│   ツリー     │                      │               │
│              │  1  function calc()  │ Watch         │
│ ▼ top        │  2  {                │ Breakpoints   │
│  ▼ (no domain)│  3    let x = 10;  │ Scope         │
│    📄script.js│ ●4    let y = 20;  │ Call Stack    │
│    📄main.js │  5    return x + y;  │               │
│              │  6  }                │               │
└──────────────┴──────────────────────┴───────────────┘
```

### ①ファイルツリー
- ページで読み込まれているすべての JavaScript ファイルが表示される
- ファイルをクリックすると、中央のエディタに表示される

### ②コードエディタ
- JavaScript のコードが表示される
- 行番号をクリックすると **ブレークポイント** を設定できる（●マークが付く）
- コードは編集できないが、ブレークポイントで止めて確認できる

### ③デバッグ情報
- **Watch**：監視したい変数を追加
- **Breakpoints**：設定したブレークポイントの一覧
- **Scope**：現在のスコープ内の変数一覧
- **Call Stack**：関数の呼び出し履歴

---

## ブレークポイントの基本

### ブレークポイントとは？

プログラムの実行を **一時停止** する地点のことです。

```javascript
function calculateTotal(price, quantity) {
  let subtotal = price * quantity;     // ←ここで止めたい！
  let tax = subtotal * 0.1;
  let total = subtotal + tax;
  return total;
}
```

**ブレークポイントを設定すると：**
- その行に到達した瞬間、プログラムが止まる
- 変数の中身を確認できる
- 1行ずつ進めながら動作を確認できる

---

### ブレークポイントの設定方法

#### 方法1: 行番号をクリック

```
┌─────────────────────────────┐
│  1  function calculateTotal │
│  2  {                       │
│ ●3    let subtotal = ...    │ ← 行番号3をクリック
│  4    let tax = ...         │
└─────────────────────────────┘
```

- 行番号をクリックすると **青い●** が表示される
- もう一度クリックすると解除される

#### 方法2: コード内に debugger を書く

```javascript
function calculateTotal(price, quantity) {
  debugger;  // ← この行で自動的に止まる
  let subtotal = price * quantity;
  let tax = subtotal * 0.1;
  return subtotal + tax;
}
```

**使い分け：**
- **行番号クリック**：DevTools で設定（コードを変更しない）
- **debugger**：コードに書く（チームメンバーも同じ場所で止まる）

---

### ブレークポイントで止まったら

プログラムが止まると、以下のようになります：

```
┌─────────────────────────────────────────┐
│  1  function calculateTotal(price, qty) │
│  2  {                                   │
│ ●3    let subtotal = price * qty;       │ ← ここで停止中
│     ^^^^^^^^ 青くハイライトされる       │
│  4    let tax = subtotal * 0.1;         │
└─────────────────────────────────────────┘
```

**右側の Scope パネルに変数が表示される：**

```
▼ Local
    price: 1000
    qty: 3
    subtotal: (not yet defined)  ← まだ実行されていない
```

**この状態でできること：**
- 変数の中身を確認
- Console で `price` や `qty` を入力して値を確認
- ステップ実行で次の行に進む

---

## ステップ実行

プログラムを **1行ずつ** 進めて、動作を確認できます。

### 実行制御ボタン

ブレークポイントで止まると、上部に以下のボタンが表示されます：

```
▶️ Resume (F8)     ⏯ 次のブレークポイントまで実行
⤵️ Step Over (F10)  ⏩ 次の行へ（関数は飛ばす）
⬇️ Step Into (F11)  🔍 関数の中に入る
⬆️ Step Out (Shift+F11) 🚪 関数から出る
```

---

### Step Over（最もよく使う！）

**F10** または **⤵️ アイコン**

次の行に進みます。関数があっても **中には入らず** に結果だけ取得します。

```javascript
function main() {
  let price = 1000;      // ←① ここで停止中
  let result = calc();   // ②Step Over で次へ → calc()は実行されるが中には入らない
  console.log(result);   // ③次に進む
}

function calc() {
  return 100 * 2;  // ← Step Over では入らない
}
```

**使い方：**
- 基本的に Step Over で進める
- 関数の結果だけ知りたい時に便利

---

### Step Into（関数の中を調べたい時）

**F11** または **⬇️ アイコン**

関数の **中に入って** 詳しく調べます。

```javascript
function main() {
  let price = 1000;
  let result = calc();   // ←① ここで Step Into
                         //   ↓
}                        //   ↓
                         //   ↓
function calc() {        //   ↓
  return 100 * 2;  // ←② ここに移動して止まる
}
```

**使い方：**
- 「この関数の中で何が起きているか確認したい」時に使う

---

### Step Out（関数から出たい時）

**Shift + F11** または **⬆️ アイコン**

現在の関数を **最後まで実行して** 呼び出し元に戻ります。

```javascript
function main() {
  let result = calc();   // ←③ ここに戻ってくる
  console.log(result);
}

function calc() {
  let a = 100;     // ←① ここで停止中
  let b = 200;     //   ↓ Step Out を押すと...
  return a + b;    // ②ここまで実行される
}
```

**使い方：**
- 「関数の中に入ったけど、もう十分確認した」時に使う

---

### Resume（次のブレークポイントまで実行）

**F8** または **▶️ アイコン**

次のブレークポイントまで、または最後まで実行します。

```javascript
function main() {
  let a = 10;      // ●①ブレークポイント1
  let b = 20;
  let c = 30;      // ●②ブレークポイント2
  console.log(a + b + c);
}
```

①で止まっている時に **Resume** を押すと、②まで一気に進みます。

---

## 変数の確認方法

ブレークポイントで止まっている時、変数の値を確認する方法が4つあります。

### 方法1: コードにマウスを乗せる（最も簡単！）

```javascript
let price = 1000;
let quantity = 3;
let total = price * quantity;  // ← price にマウスを乗せる
```

→ ツールチップで `price: 1000` と表示される

---

### 方法2: Scope パネルで確認

右側の **Scope** パネルに、すべての変数が自動で表示されます。

```
▼ Local
    price: 1000
    quantity: 3
    total: 3000
▼ Global
    window: Window {...}
    document: Document {...}
```

**Scope の種類：**
- **Local**：現在の関数内の変数
- **Closure**：外側の関数からアクセスできる変数
- **Global**：グローバル変数

---

### 方法3: Watch で監視

**Watch** パネルで、特定の変数を常に監視できます。

**使い方：**

1. **Watch** パネルの **+** をクリック
2. 変数名や式を入力（例：`price * quantity`）
3. 値が表示される

```
▼ Watch
    price * quantity: 3000
    total > 1000: true
```

**便利な使い方：**
- 計算結果を監視（`price * 1.1`）
- 条件式を監視（`total > 5000`）

---

### 方法4: Console で確認

ブレークポイントで止まっている時、Console で変数名を入力すると値が表示されます。

```javascript
// Console で実行
> price
1000

> price * quantity
3000

> typeof price
"number"
```

**複雑な操作も可能：**
```javascript
// 配列の中身を確認
> users.map(u => u.name)
["太郎", "花子", "次郎"]
```

---

## 実践：よくあるデバッグシナリオ

### シナリオ1：「計算結果がおかしい」

**問題のコード：**

```javascript
function calculateDiscount(price, discountRate) {
  let discount = price * discountRate;
  let finalPrice = price - discount;
  return finalPrice;
}

let result = calculateDiscount(1000, 50);  // 期待: 500円引き、実際: 950円？
console.log(result);  // 50 ← おかしい！
```

**デバッグ手順：**

1. Sources パネルを開く
2. `let discount = price * discountRate;` の行番号をクリック（ブレークポイント設定）
3. ページをリロードして関数を実行
4. ブレークポイントで止まったら、Scope パネルを確認：
   ```
   price: 1000
   discountRate: 50  ← これが問題！ 0.5 のはず
   ```
5. **原因判明**：`discountRate` は 50% なら `0.5` を渡すべきだった

**修正後：**
```javascript
let result = calculateDiscount(1000, 0.5);  // ✅ 正しい
```

---

### シナリオ2：「ループのどこでエラーになるか分からない」

**問題のコード：**

```javascript
let users = [
  { name: "太郎", age: 25 },
  { name: "花子", age: 30 },
  { name: "次郎" },  // ← age がない！
];

users.forEach(user => {
  let nextAge = user.age + 1;  // ← ここでエラー？
  console.log(`${user.name}の来年の年齢: ${nextAge}`);
});
```

**デバッグ手順：**

1. `let nextAge = user.age + 1;` にブレークポイントを設定
2. ページをリロード
3. **Resume (F8)** を2回押して、3回目のループに進む
4. Scope パネルを確認：
   ```
   user: {name: "次郎"}  ← age プロパティがない！
   ```
5. **原因判明**：3番目のユーザーに `age` がない

**修正後：**
```javascript
users.forEach(user => {
  let nextAge = user.age ? user.age + 1 : "不明";  // ✅ age がない場合の対応
  console.log(`${user.name}の来年の年齢: ${nextAge}`);
});
```

---

### シナリオ3：「API から取得したデータがおかしい」

**問題のコード：**

```javascript
async function fetchUser() {
  let response = await fetch('/api/user');
  let data = await response.json();
  console.log(data.name);  // undefined が表示される
  return data;
}
```

**デバッグ手順：**

1. `let data = await response.json();` にブレークポイント
2. ページをリロード
3. ブレークポイントで止まったら、**Step Over (F10)** で次の行へ
4. Console で `data` を確認：
   ```javascript
   > data
   { user: { name: "太郎", age: 25 } }  ← data.user.name が正しい
   ```
5. **原因判明**：データ構造が `{ user: {...} }` になっている

**修正後：**
```javascript
console.log(data.user.name);  // ✅ 正しい
```

---

### シナリオ4：「if文のどちらに進んでいるか分からない」

**問題のコード：**

```javascript
function checkAge(age) {
  if (age >= 20) {
    console.log("成人です");
  } else {
    console.log("未成年です");
  }
}

checkAge("25");  // "未成年です" と表示される ← なぜ？
```

**デバッグ手順：**

1. `if (age >= 20)` にブレークポイント
2. Step Over で進む → `else` ブロックに入った！
3. Console で確認：
   ```javascript
   > age
   "25"  ← 文字列！

   > typeof age
   "string"

   > "25" >= 20
   true  ← あれ？

   > Number("25") >= 20
   true  ← これなら正しい
   ```
4. **原因判明**：`age` が文字列だった（文字列比較になっている）

**修正後：**
```javascript
function checkAge(age) {
  age = Number(age);  // ✅ 数値に変換
  if (age >= 20) {
    console.log("成人です");
  } else {
    console.log("未成年です");
  }
}
```

---

## 便利な機能

### 1. 条件付きブレークポイント

**特定の条件の時だけ止める** ことができます。

**設定方法：**

1. 行番号を **右クリック**
2. **「Add conditional breakpoint」** を選択
3. 条件を入力（例：`i === 5`）

```javascript
for (let i = 0; i < 100; i++) {
  console.log(i);  // ← i が 5 の時だけ止めたい
}
```

**条件：** `i === 5`

→ ループが5回目の時だけブレークポイントで止まる！

---

### 2. Logpoint（ブレークポイントせずにログ出力）

コードを止めずに、**自動でログを出力** できます。

**設定方法：**

1. 行番号を **右クリック**
2. **「Add logpoint」** を選択
3. 出力したい内容を入力（例：`price`, `"合計:", total`）

```javascript
let total = price * quantity;  // ← ここに Logpoint
```

**Logpoint：** `"合計:", total`

→ Console に `合計: 3000` と表示される（止まらない）

---

### 3. すべてのブレークポイントを無効化

デバッグ中に「ブレークポイントを全部解除したい」時：

**方法：**
- 右側の **Breakpoints** パネルで、左上のチェックボックスをクリック

→ すべてのブレークポイントが一時的に無効になる（削除はされない）

---

### 4. イベントリスナーブレークポイント

**特定のイベントで止める** ことができます。

**使い方：**

1. 右側の **Event Listener Breakpoints** を展開
2. 止めたいイベントにチェック（例：`click`, `load`, `submit`）

→ そのイベントが発生した時に自動で止まる！

**便利なイベント：**
- **Mouse → click**：クリックイベントで止まる
- **Keyboard → keydown**：キー入力で止まる
- **XHR → Fetch**：API 通信で止まる

---

### 5. Call Stack（関数の呼び出し履歴）

**Call Stack** パネルで、「どの関数から呼ばれたか」が分かります。

```javascript
function main() {
  calculate();
}

function calculate() {
  let result = add(10, 20);
  return result;
}

function add(a, b) {
  return a + b;  // ←ここで停止中
}
```

**Call Stack：**
```
add (script.js:10)      ← 現在ここ
calculate (script.js:6)  ← ここから呼ばれた
main (script.js:2)       ← さらにここから呼ばれた
(anonymous)              ← 最初の実行
```

**使い方：**
- Call Stack の各行をクリックすると、その関数の状態を確認できる

---

### 6. ブラックボックス化（ライブラリをスキップ）

jQuery や React などのライブラリのコードに入りたくない時：

**設定方法：**

1. ファイルツリーでライブラリファイルを **右クリック**
2. **「Blackbox script」** を選択

→ Step Into してもライブラリの中には入らなくなる！

---

### 7. Prettier で見やすくする

圧縮された JavaScript（minified）を見やすく整形できます。

**方法：**
- コードエディタの下部にある **{}** アイコンをクリック

```javascript
// 圧縮されたコード
function calc(a,b){return a+b}

// ↓ Prettier で整形

function calc(a, b) {
  return a + b;
}
```

---

## まとめ

### Sources パネルでできること

- ✅ プログラムを途中で止めて変数を確認
- ✅ 1行ずつ実行して動作を追う
- ✅ 関数の中に入って詳しく調べる
- ✅ ループのどこでエラーになるか特定
- ✅ 条件分岐がどちらに進むか確認

### よく使う操作

| 操作 | ショートカット | 用途 |
|------|----------------|------|
| **ブレークポイント設定** | 行番号クリック | 止めたい場所を指定 |
| **Step Over** | F10 | 次の行へ |
| **Step Into** | F11 | 関数の中へ |
| **Resume** | F8 | 次のブレークポイントまで実行 |

### デバッグの流れ

1. **ブレークポイントを設定** → 問題がありそうな場所
2. **ページをリロード** → 関数を実行
3. **Step Over で進める** → 1行ずつ確認
4. **Scope や Console で変数を確認** → 値がおかしい場所を特定
5. **原因を特定したら修正** → 実際のファイルに反映

**Console で解決できない複雑なバグは、Sources パネルで解決できます！**

---

**前へ：** [便利な Tips](./06_tips.md) | **次へ：** [トラブルシューティング](./08_troubleshooting.md) | **トップへ：** [README](../README.md)
