# 便利な Tips：効率的な使い方

## 目次
- [キーボードショートカット](#キーボードショートカット)
- [検証ツールの活用](#検証ツールの活用)
- [コピー＆ペーストの便利技](#コピーペーストの便利技)
- [デバイスシミュレーション](#デバイスシミュレーション)
- [スクリーンショット](#スクリーンショット)
- [ダークモード](#ダークモード)
- [その他の便利機能](#その他の便利機能)

---

## キーボードショートカット

作業効率を上げるために、よく使うショートカットを覚えましょう！

### DevTools の操作

| 操作 | Windows/Linux | Mac |
|------|---------------|-----|
| **DevTools を開く/閉じる** | `Ctrl + Shift + I` | `Command + Option + I` |
| **Console を開く** | `Ctrl + Shift + J` | `Command + Option + J` |
| **要素を検証** | `Ctrl + Shift + C` | `Command + Option + C` |
| **前のパネルに移動** | `Ctrl + [` | `Command + [` |
| **次のパネルに移動** | `Ctrl + ]` | `Command + ]` |
| **設定を開く** | `F1` | `F1` |

### Console パネル

| 操作 | Windows/Linux | Mac |
|------|---------------|-----|
| **Console をクリア** | `Ctrl + L` | `Command + K` |
| **複数行入力** | `Shift + Enter` | `Shift + Enter` |
| **前のコマンド** | `↑` | `↑` |
| **次のコマンド** | `↓` | `↓` |
| **検索** | `Ctrl + F` | `Command + F` |

### Elements パネル

| 操作 | Windows/Linux | Mac |
|------|---------------|-----|
| **要素を編集** | `F2` | `F2` |
| **要素を削除** | `Delete` | `Delete` |
| **要素を展開/折りたたみ** | `→` / `←` | `→` / `←` |

### ページの操作

| 操作 | Windows/Linux | Mac |
|------|---------------|-----|
| **通常のリロード** | `F5` または `Ctrl + R` | `Command + R` |
| **強制リロード（キャッシュ無視）** | `Ctrl + Shift + R` | `Command + Shift + R` |

---

## 検証ツールの活用

### 1. 要素の選択（最も使う機能！）

**ショートカット：** `Ctrl + Shift + C`（Mac: `Command + Option + C`）

ページ上の任意の要素をクリックするだけで、そのコードにジャンプできます。

**使い方：**
1. `Ctrl + Shift + C` を押す
2. ページ上でカーソルを動かす（要素がハイライトされる）
3. 調べたい要素をクリック

→ Elements パネルにその要素が表示されます！

### 2. ホバー状態を固定

マウスを乗せた時のスタイル（`:hover`）を確認したい時：

1. Elements パネルで要素を選択
2. Styles パネルの **:hov** をクリック
3. **:hover** にチェックを入れる

→ マウスを乗せなくてもホバー状態を確認できます！

他にも以下が確認できます：
- `:active` - クリックした時
- `:focus` - フォーカスされた時
- `:visited` - 訪問済みリンク

---

## コピー＆ペーストの便利技

### 1. 要素を JavaScript オブジェクトとしてコピー

Elements パネルで要素を選択 → 右クリック → **「Copy」** → **「Copy JS path」**

```javascript
// コピーされる内容の例
document.querySelector("#app > div > button")
```

→ Console で直接使えます！

### 2. CSS をコピー

Styles パネルでスタイルブロックを選択 → 右クリック → **「Copy all declarations」**

```css
/* コピーされる内容 */
color: blue;
padding: 10px;
background-color: white;
```

### 3. API リクエストをコピー

Network パネルでリクエストを選択 → 右クリック → **「Copy」**

- **Copy as cURL**：curl コマンドとしてコピー（ターミナルで実行可能）
- **Copy as fetch**：JavaScript の fetch コードとしてコピー
- **Copy URL**：URL のみコピー

**例：**
```javascript
// Copy as fetch の結果
fetch("https://api.example.com/users", {
  "headers": {
    "accept": "application/json",
  },
  "method": "GET"
});
```

### 4. Console の出力をコピー

Console の出力を右クリック → **「Copy object」**

オブジェクトの内容を JSON 形式でコピーできます。

---

## デバイスシミュレーション

スマホやタブレットでの表示を確認できます！

### デバイスモードを有効にする

**ショートカット：** `Ctrl + Shift + M`（Mac: `Command + Shift + M`）

または、DevTools の左上にある **デバイスアイコン** をクリック

### 使い方

1. 上部のデバイス選択ドロップダウンから選択：
   - iPhone SE
   - iPhone 12 Pro
   - iPad
   - Samsung Galaxy S20
   - など

2. **Responsive** を選択すると、自由にサイズを変更できます

3. **回転アイコン** で縦向き・横向きを切り替え

### 便利な機能

- **ネットワーク速度をシミュレート**：`Online` → `Fast 3G` / `Slow 3G`
- **解像度を確認**：ピクセル数が表示される
- **タッチイベントをシミュレート**：マウスクリックがタッチとして動作

---

## スクリーンショット

DevTools から直接スクリーンショットを撮れます！

### 方法1: 要素のスクリーンショット

1. Elements パネルで要素を選択
2. `Ctrl + Shift + P`（Mac: `Command + Shift + P`）でコマンドパレットを開く
3. `screenshot` と入力
4. **「Capture node screenshot」** を選択

→ 選択した要素のみがキャプチャされます！

### 方法2: ページ全体のスクリーンショット

1. `Ctrl + Shift + P` でコマンドパレットを開く
2. `screenshot` と入力
3. **「Capture full size screenshot」** を選択

→ スクロールが必要な長いページも、全体がキャプチャされます！

### 方法3: 表示範囲のスクリーンショット

1. `Ctrl + Shift + P` でコマンドパレットを開く
2. `screenshot` と入力
3. **「Capture screenshot」** を選択

→ 現在見えている範囲のみキャプチャされます

---

## ダークモード

DevTools 自体をダークモードにできます。

### 設定方法

1. `F1` を押して設定を開く
2. **Preferences** → **Appearance**
3. **Theme** を **Dark** に変更

目に優しく、長時間の開発に最適です！

---

## その他の便利機能

### 1. コマンドパレット

`Ctrl + Shift + P`（Mac: `Command + Shift + P`）で開く

**できること：**
- スクリーンショット撮影
- テーマ変更
- パネルの表示/非表示
- 各種設定

`>` を入力後、やりたいことを入力すると候補が表示されます。

### 2. ファイル検索

`Ctrl + P`（Mac: `Command + P`）でファイル検索

プロジェクト内のファイルを素早く開けます。

### 3. コード内検索

`Ctrl + Shift + F`（Mac: `Command + Option + F`）で全ファイル検索

すべての JavaScript / CSS ファイルから文字列を検索できます。

### 4. CSS の変更を保存する（Workspaces 機能）

通常、DevTools での CSS 変更は一時的ですが、Workspaces 機能を使うと **実際のファイルに保存** できます！

**設定方法：**

1. Sources パネルを開く
2. **Filesystem** タブを選択
3. **+ Add folder to workspace** をクリック
4. プロジェクトフォルダを選択

→ DevTools での変更が、直接ファイルに反映されます！

### 5. 要素のアニメーションをスロー再生

1. `Ctrl + Shift + P` でコマンドパレットを開く
2. `animation` と入力
3. **「Show Animations」** を選択

→ Animations パネルが開き、アニメーションをスロー再生できます

### 6. JavaScript を無効化してテスト

1. `Ctrl + Shift + P` でコマンドパレットを開く
2. `javascript` と入力
3. **「Disable JavaScript」** を選択

→ JavaScript が無効な環境をテストできます

### 7. カラーピッカーの便利機能

Styles パネルで色をクリックすると、カラーピッカーが開きます。

**便利な機能：**
- **スポイトツール**：ページ上の色を抽出
- **色形式の切り替え**：HEX / RGB / HSL を切り替え
- **コントラスト比の確認**：アクセシビリティチェック

### 8. ローカルストレージの確認

Application パネル（または Storage パネル）で以下を確認できます：

- **Local Storage**：ブラウザに保存されたデータ
- **Session Storage**：セッションデータ
- **Cookies**：クッキー
- **IndexedDB**：データベース

**使い方：**
1. Application パネルを開く
2. 左側のメニューから確認したい項目を選択

### 9. ネットワークスロットリング

Network パネルで、接続速度をシミュレートできます。

- **Fast 3G**：3G 回線
- **Slow 3G**：遅い回線
- **Offline**：オフライン

モバイルユーザーの体験を確認できます！

### 10. $ を使った要素取得（Console のみ）

```javascript
// document.querySelector() の短縮版
$('.btn')

// document.querySelectorAll() の短縮版
$$('.btn')

// 最後に選択した要素
$0

// 1つ前に選択した要素
$1

// document.getElementById() の短縮版
$('#my-id')
```

---

## 実践：よく使う組み合わせ技

### シナリオ1：デザインを微調整

1. `Ctrl + Shift + C` で要素選択ツールを起動
2. 調整したい要素をクリック
3. Styles パネルで色やサイズを変更
4. 気に入ったら、CSS をコピーして実際のファイルに反映

### シナリオ2：API のデバッグ

1. Network パネルを開く
2. **Fetch/XHR** フィルタを有効
3. ページで API を呼び出す操作をする
4. リクエストをクリックして、Headers / Response を確認
5. エラーなら、右クリック → **Copy as fetch** でコードをコピー
6. Console で実行して再現

### シナリオ3：レスポンシブデザインの確認

1. `Ctrl + Shift + M` でデバイスモードを有効
2. iPhone / iPad などのプリセットを選択
3. 縦向き・横向きを切り替え
4. 必要なら `Ctrl + Shift + P` → `screenshot` で画面キャプチャ

---

## まとめ

- キーボードショートカットを覚えると作業が速くなる
- `Ctrl + Shift + C` で要素選択が最も便利
- コマンドパレット（`Ctrl + Shift + P`）で様々な機能にアクセス
- デバイスモードでスマホ表示を確認
- スクリーンショットを直接撮影できる
- Workspaces 機能で、変更を直接ファイルに保存できる

これらの機能を使いこなして、効率的に開発しましょう！

**前へ：** [よくあるエラーと対処法](./05_common_errors.md) | **トップへ：** [README](../README.md)
