# 麺処 隼（はやぶさ）サービスサイト

個人経営の鶏白湯らーめん専門店「麺処 隼」のマーケティングサイト。Claude Design（Claude 経由のUI生成）で作成された、ビルド不要の静的HTMLサイト。

## 技術構成

- **静的HTML** のみ。フレームワーク・バンドラー・パッケージマネージャなし（`package.json` 等は存在しない）。
- スタイルは各ページの `<head>` 内 `<style>`（共通の `.btn-*` / `.sns-pill` クラスとキーフレーム）と、**要素ごとのインライン `style` 属性**の組み合わせ。外部CSSファイルは無い。
- JavaScript なし。全ページ完全に静的。ハンバーガーメニューの開閉もJSではなく「checkboxハック」(CSSの `:has()` 利用)で実装している。
- フォントは Google Fonts を `<link>` で読み込み（`Shippori Mincho` / `Noto Sans JP` / `EB Garamond`）。
- 画像は `images/` 配下のJPG/PNGをそのまま参照（最適化・レスポンシブ画像なし）。

## ファイル構成

```
index.html       トップページ（HERO / こだわり / 人気メニュー / 店主紹介 / 店舗情報・お問い合わせ / フッター）
kodawari.html    こだわり 単独ページ
menu.html        MENU 単独ページ
founder.html     店主紹介 単独ページ
shop.html        店舗情報・お問い合わせ 単独ページ（#contact）
images/          hero-bowl.jpg, founder-portrait.jpg, shop-interior.jpg,
                 menu-tokusei.jpg, menu-shoyu.jpg, menu-tsukemen.jpg, menu-seasonal.jpg,
                 logo.png
CONTENTS.md      テキスト・構成のソース・オブ・トゥルース（コピー、メニュー内容、メタ情報の原案）
DESIGN.md        ビジュアルデザイン仕様書（カラー・タイポグラフィ・コンポーネント規則）
```

各下層ページはヘッダー／フッターの構造・ナビゲーションリンクを `index.html` と共通で複製している（共有コンポーネント化・テンプレート化はされていない）。新しいページを追加する場合も同じヘッダー／フッターHTMLをコピーして揃えること。

## コンテンツ・デザインのソース・オブ・トゥルース

- **`CONTENTS.md`**: 見出し・本文・メニュー内容・メタ情報（title/description/OG）などのテキストの正。コンテンツを変更する場合はこのファイルの記述と実装がずれないよう両方更新する。
- **`DESIGN.md`**: カラーパレット・フォント・スペーシング・コンポーネント（ボタン、カード、写真ヒーロー、職人紹介ブロック等）の仕様書。新しいUIを作る／既存UIを直す際は必ずこの仕様に従う。

### 未確定・仮の情報（要注意）

以下はまだ実データが入っていないプレースホルダー。ユーザーから実データが提供されたら全ページ一括で置き換えること：

- 住所: `東京都〇〇区〇〇1-2-3`（`index.html` / `shop.html` / フッター全ページ）
- 最寄駅: `〇〇線〇〇駅 徒歩5分`
- 運営者名: `個人事業主：〇〇 〇〇`
- `CONTENTS.md` 記載の画像パス（`/images/menu-hayabusa.jpg` 等）は実装のファイル名（`images/menu-tokusei.jpg` 等）と異なる場合がある。実装側のファイル名が正なので、`CONTENTS.md` を参照する際はパスではなく文言・構成を参照すること。
- `CONTENTS.md` にある「6. 掲載メディア（#media）」セクションは現時点で `index.html` 等の実装にまだ反映されていない（未実装）。
- Google Maps / Instagram / X のリンクは `麺処隼` `hayabusa_ramen` のダミー想定URL。実店舗のURLに要差し替え。

## デザイン規則（DESIGN.md 要約）

- **配色**: 黒背景ベース＋朱色・金の差し色。Primary `#B7282E`（朱）/ Secondary `#C9A227`（金）/ Background `#111010`（黒）/ Background Sub `#1A1918`/ Text Main `#F5F1EA`/ Text Muted `#9C948A`/ Border `#3A3632`/ Background Alt `#2E2B28`（下層ページの中間トーン）。カラーコードは仕様書通りに厳密使用し、値を変える場合はDESIGN.mdも更新する。
- **フォント**: 見出しは明朝体（`Shippori Mincho`)、本文はゴシック体（`Noto Sans JP`）、欧文ロゴのみ `EB Garamond`。
- **コンポーネントパターン**: 角丸は浅め（ボタン`2px`、カード`4px`）、シャドウなし・境界線で区切る、ボタンはホバーで朱→金に変化、点線区切り（`border-top:1px dashed`）、フルブリード写真＋グラデーションオーバーレイ、店主紹介はモノクロ写真＋2カラム。
- **レスポンシブ**: モバイルファースト。営業時間・アクセス情報を最優先表示。ブレイクポイント: モバイル `~639px` / タブレット `640–1023px` / デスクトップ `1024px~`。
- **アクセシビリティ**: 背景とのコントラスト比 4.5:1 以上、写真上テキストは半透明黒オーバーレイ必須、画像には必ず具体的な `alt`。

## 開発・編集の注意

- 値は極力ハードコードせず、DESIGN.md のトークン（カラーコード・フォント指定）に厳密に一致させる。新色・新フォントサイズを使う場合は先にDESIGN.mdを更新するか、既存トークンで表現できないか検討する。
- インラインstyleが正のスタイリング手法。共通クラスは `<style>` 内の `.btn-outline-gold` / `.btn-fill-red` / `.sns-pill` / `.sns-pill-lg`（shop.htmlのみ）と、下記のレスポンシブ用クラス群のみ。新しい繰り返しパターンができた場合はクラス化を検討してよいが、既存の書き方（インラインstyle中心）から大きく外れる大規模リファクタリングは指示がない限り行わない。
- ビルドコマンド・テストコマンドは存在しない。変更後はHTMLファイルをブラウザで直接開く（またはローカルの静的サーバーで配信）だけで確認できる。
- 日本語の行間・字間（`letter-spacing`）は既存ページの値に揃える（明朝体見出しは字間をやや広めに取る、というDESIGN.mdの指示に従う）。

## レスポンシブ対応の実装方針（全5ページ共通）

DESIGN.mdの「モバイルファースト」「640px未満は横並びナビ禁止・ハンバーガー必須」の指示に沿って、`max-width:639px` の1本のメディアクエリで対応している。desktop/tabletは既存の `grid-template-columns:repeat(auto-fit,minmax(...))` 依存の自然な折り返しに任せているため追加CSSはほぼ無い。

- **ヘッダー/ナビ**: JavaScriptなしの「checkboxハック」（`:has()`使用）でハンバーガーメニューを実装。`<header>` に `class="hdr-x"`、ロゴ直後に `class="nav-toggle-wrap"` の中に `input#nav-toggle.nav-toggle-input` + `label[for=nav-toggle].nav-toggle-btn`（span3本）、既存の `<nav>` に `class="site-nav"` を追加している。639px以下では `.site-nav` が `position:fixed;inset:0` のフルスクリーンオーバーレイになり、`.nav-toggle-wrap:has(.nav-toggle-input:checked) ~ .site-nav` で表示を切り替える。**新しいページを追加する場合もこのマークアップパターン（input→label→nav の兄弟順序を保つ）をそのままコピーすること**。順序を変えると `:has()`/`~` セレクタが効かなくなる。
- **h1**: 639px以下で `font-size:2rem!important` に統一（DESIGN.mdの「モバイルではh1を2rem程度まで縮小」に対応）。
- **水平パディングの圧縮用ユーティリティ**（縦方向の余白はDESIGN.mdの「セクション間の余白は広めに」方針を尊重し、基本的に触っていない）:
  - `.sec-x`: セクション類の左右56px/44pxパディングを639px以下で20pxに（`padding-left/right`のみ上書き）
  - `.card-x`: カード類（こだわりカード・三代カード・お問い合わせパネル等）の左右パディングを24pxに
  - `.cta-pad`: 写真オーバーレイCTAバナー内側の文字ブロックを `padding:28px 20px` に一括上書き
  - `.bleed-x`: menu.htmlの限定メニュー（親のpaddingを打ち消すbleed背景ブロック）の `margin`/`padding` を画面幅に合わせて再計算
  - `.footer-x`: フッターの左右パディングを20pxに、`.sns-row` でSNSアイコン行を中央寄せ（DESIGN.mdのモバイルフッター仕様）
- **写真セクションの高さ調整**: `.ph-600` / `.ph-520` / `.ph-420` / `.ph-320` は元の `min-height` 値ごとに命名した縮小用クラス（それぞれ340/300/260/220pxに）。新しい写真ヒーローを追加する場合、元のmin-height値に応じてこの4クラスのいずれかを使うか、値が異なれば同じ命名規則で追加すること。
- **index.htmlのHERO（2カラム固定グリッド）**: `.hero-split` を付与し、639px以下で `grid-template-columns:1fr!important` に1カラム化。他ページのタイトルセクションは元々 `auto-fit` グリッドなので対応不要。
- **index.htmlの店舗情報オーバーラップカード**: `.overlap-card` で639px以下の `margin`/`padding` を再定義（auto中央寄せだとモバイルで左右ガターが0になるため固定値に変更）。
- **shop.htmlの項目リスト（店名・営業時間など）**: 各行に `.info-row` を付与し、639px以下で `grid-template-columns:1fr` にしてラベル/値を縦積みに（固定110px幅カラムのままだと値が狭くなりすぎるため）。

新しいセクションを追加・編集する際は、既存の同種要素（セクション=`sec-x`、カード=`card-x`、写真ヒーロー=`ph-*`）がどのクラスを使っているか近い実装を探し、パターンを踏襲すること。
