# Console パネル：エラーログとJavaScript実行

## 目次
- [Console パネルとは](#console-パネルとは)
- [エラーメッセージの見方](#エラーメッセージの見方)
- [ログの種類と意味](#ログの種類と意味)
- [JavaScript を実行してみる](#javascript-を実行してみる)
- [実践：よくあるエラーの確認方法](#実践よくあるエラーの確認方法)
- [便利な機能](#便利な機能)

---

## Console パネルとは

**Console パネル** は、JavaScript のエラーやログを確認できるパネルです。

### こんな時に使います

- **エラーが出た時**：どんなエラーか、どこで起きたか確認
- **JavaScript の動作確認**：コードが正しく動いているか確認
- **簡単なコード実行**：JavaScript をその場で試せる
- **デバッグ情報の確認**：`console.log()` で出力した情報を見る

プログラミング初心者が **最も頻繁に使う** パネルの1つです！

---

## エラーメッセージの見方

### エラーの基本構造

Console にエラーが表示された時、以下の情報が含まれています：

```
Uncaught TypeError: Cannot read property 'value' of null
    at script.js:15
```

**読み解き方：**

1. **エラーの種類**：`TypeError`（型に関するエラー）
2. **エラーの内容**：`Cannot read property 'value' of null`（null の value プロパティを読もうとした）
3. **場所**：`script.js:15`（script.js ファイルの15行目）

### エラーをクリックするとどうなる？

ファイル名の部分（`script.js:15`）をクリックすると、**Sources パネル** が開き、エラーが発生した行に直接ジャンプできます！

---

## ログの種類と意味

Console には色々な種類のメッセージが表示されます。

### 1. エラー（赤色）🔴

```
❌ Uncaught ReferenceError: myFunction is not defined
```

**意味：** プログラムが止まってしまうような重大な問題

**よくある原因：**
- 関数名や変数名のスペルミス
- ファイルの読み込み順序が間違っている
- 必要なライブラリが読み込まれていない

### 2. 警告（黄色）⚠️

```
⚠ Warning: Each child in a list should have a unique "key" prop.
```

**意味：** 動くけど、問題が起きる可能性がある

**よくある原因：**
- 非推奨の機能を使っている
- パフォーマンスに影響する書き方をしている
- セキュリティ上の問題がある可能性

### 3. 情報（青色）ℹ️

```
ℹ Page loaded in 1.2 seconds
```

**意味：** 単なる情報（問題ではない）

### 4. ログ（黒色）

```
console.log() で出力したメッセージ
```

**意味：** 開発者が意図的に出力したデバッグ情報

---

## JavaScript を実行してみる

Console は JavaScript を **その場で実行できる** 便利な機能があります！

### 基本的な使い方

Console の下部に入力欄があります。ここに JavaScript を書いて `Enter` を押すと実行されます。

```javascript
> 1 + 1
2

> "Hello" + " World"
"Hello World"

> document.title
"ページのタイトル"
```

### 複数行のコードを書く

`Shift + Enter` で改行できます。

```javascript
> let name = "太郎";    // Shift + Enter
  console.log(name);    // Enter で実行
太郎
```

### よく使うコマンド

#### ページのタイトルを確認
```javascript
document.title
```

#### ページ上の要素を取得
```javascript
document.querySelector('.btn')
```

#### 変数の中身を確認
```javascript
console.log(変数名)
```

#### 現在のページをリロード
```javascript
location.reload()
```

---

## 実践：よくあるエラーの確認方法

### ケース1：ページを開いたらエラーが出ている

**対処手順：**

1. Console パネルを開く
2. 赤色のエラーメッセージを探す
3. エラーメッセージを読む：
   - `ReferenceError: XXX is not defined` → XXX という変数・関数が見つからない
   - `TypeError: Cannot read property` → null や undefined に対してプロパティを読もうとした
   - `SyntaxError` → コードの書き方が間違っている
4. ファイル名と行番号をクリックして、該当箇所を確認

### ケース2：ボタンをクリックしても何も起こらない

**確認手順：**

1. Console パネルを開く
2. ボタンをクリック
3. エラーが出るか確認
4. 何も出ない場合は、イベントリスナーが設定されていない可能性

**デバッグ方法：**

```javascript
// Console で確認
let btn = document.querySelector('.my-button');
console.log(btn);  // null なら要素が見つかっていない
```

### ケース3：API からデータが取得できない

**確認手順：**

1. Console でエラーを確認
2. よくあるエラー：
   - `CORS error` → サーバー側の設定問題
   - `404 Not Found` → URL が間違っている
   - `Failed to fetch` → ネットワークエラー

### ケース4：変数の中身を確認したい

コードに `console.log()` を追加します。

```javascript
let userData = { name: "太郎", age: 25 };
console.log("userData:", userData);
```

Console に以下のように表示されます：

```
userData: {name: "太郎", age: 25}
  ▶ name: "太郎"
  ▶ age: 25
```

---

## 便利な機能

### 1. エラーをフィルタリングする

Console の上部にフィルタボタンがあります：

- **Errors**：エラーのみ表示
- **Warnings**：警告のみ表示
- **Info**：情報のみ表示
- **Verbose**：すべて表示

エラーが多い時は、**Errors** だけにフィルタすると見やすくなります！

### 2. Console をクリアする

```javascript
clear()
```

または、Console の左上にある **🚫（禁止マーク）** をクリック

ショートカット：`Ctrl + L`（Mac: `Command + K`）

### 3. オブジェクトの中身を見る

オブジェクトの左にある **▶** をクリックすると、中身が展開されます。

```javascript
> let user = { name: "太郎", age: 25 };
> console.log(user);
▶ {name: "太郎", age: 25}   ← これをクリック
  ▼ {name: "太郎", age: 25}
      name: "太郎"
      age: 25
```

### 4. console の便利なメソッド

#### console.table() - 配列を表形式で表示

```javascript
let users = [
  { name: "太郎", age: 25 },
  { name: "花子", age: 30 }
];
console.table(users);
```

→ 見やすい表で表示されます！

#### console.error() - エラーとして表示

```javascript
console.error("これはエラーメッセージです");
```

→ 赤色で表示されます

#### console.warn() - 警告として表示

```javascript
console.warn("これは警告メッセージです");
```

→ 黄色で表示されます

#### console.time() / console.timeEnd() - 処理時間を計測

```javascript
console.time("処理時間");
// 何か重い処理
for (let i = 0; i < 1000000; i++) {}
console.timeEnd("処理時間");
```

→ `処理時間: 10.5ms` のように表示されます

### 5. 直前に実行したコマンドを呼び出す

Console で **↑**（上矢印キー）を押すと、前に実行したコマンドが表示されます。

何度も同じコマンドを実行する時に便利です！

### 6. $0 - 最後に選択した要素

Elements パネルで要素を選択した後、Console で `$0` と入力すると、その要素にアクセスできます。

```javascript
// Elements で <button> を選択した後
> $0
<button class="btn">クリック</button>

> $0.innerText
"クリック"
```

### 7. $() と $$() - jQuery 風のセレクタ

```javascript
// document.querySelector() の短縮版
$('.btn')

// document.querySelectorAll() の短縮版
$$('.btn')  // 配列で返ってくる
```

---

## まとめ

- Console パネルはエラー確認とデバッグに必須
- エラーは **種類**、**内容**、**場所** を確認する
- Console で JavaScript をその場で実行できる
- `console.log()` でデバッグ情報を出力する
- エラーをクリックすると、該当のコードにジャンプできる
- フィルタ機能でエラーだけを表示できる

**次へ：** [Network パネルの使い方](./04_network.md)
