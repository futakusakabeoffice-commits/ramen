# 麺処 隼（はやぶさ）サービスサイト

個人経営の鶏白湯らーめん専門店「麺処 隼」のマーケティングサイト。Claude Design（Claude 経由のUI生成）で作成された、ビルド不要の静的HTMLサイト。

## 本番環境

- **本番ドメイン**: `https://hayabusaramen.netlify.app`（Netlifyの既定サブドメイン）。全ページの `canonical` / `og:url` / `og:image` / 構造化データの `url`・`image`、`robots.txt` の `Sitemap:` 行、`sitemap.xml` の全 `<loc>` はこのドメインに統一済み。独自ドメインに切り替える場合は `grep -rl "hayabusaramen.netlify.app" .`（`.git`除く）で該当ファイルを洗い出し、一括置換すること。

## 技術構成

- **静的HTML** のみ。フレームワーク・バンドラー・パッケージマネージャなし（`package.json` 等は存在しない）。
- スタイルは各ページの `<head>` 内 `<style>`（共通の `.btn-*` / `.sns-pill` クラスとキーフレーム）と、**要素ごとのインライン `style` 属性**の組み合わせ。外部CSSファイルは無い。
- JavaScript なし。全ページ完全に静的。ハンバーガーメニューの開閉もJSではなく「checkboxハック」(CSSの `:has()` 利用)で実装している。
- フォントは Google Fonts を `<link>` で読み込み（`Shippori Mincho` / `Noto Sans JP` / `EB Garamond`）。
- 写真（JPG由来）は **WebP** に変換して配信（`images/*.webp`）。元のJPGは `images/*.jpg` として残置（バックアップ／再変換用ソース。HTMLからは参照していない）。`logo.png` のみ透過PNGのまま。ロゴ以外の `<img>` には実寸の `width`/`height` 属性を付与済み（CLSのため）。ファーストビュー外の画像には `loading="lazy"` を付与。

## ファイル構成

```
index.html          トップページ（HERO / こだわり / 人気メニュー / 店主紹介 / 店舗情報・お問い合わせ / フッター）
kodawari.html        こだわり 単独ページ
menu.html            MENU 単独ページ
founder.html         店主紹介 単独ページ
shop.html            店舗情報・お問い合わせ 単独ページ（#contact）
404.html             カスタム404ページ（トップへの導線あり、robots: noindex）
privacy-policy/
  index.html         プライバシーポリシー（`/privacy-policy` のクリーンURLで配信するためディレクトリ+index.html構成）
robots.txt           クロール許可 + sitemap.xml参照
sitemap.xml           全公開ページのURL一覧（ドメインは要差し替え、下記参照）
site.webmanifest      PWA向けマニフェスト
favicon.ico / favicon-32x32.png / favicon-16x16.png / apple-touch-icon.png
                      logo.pngから生成したファビコン一式
images/               hero-bowl.jpg(.webp), founder-portrait.jpg(.webp), shop-interior.jpg(.webp),
                      menu-tokusei.jpg(.webp), menu-shoyu.jpg(.webp), menu-tsukemen.jpg(.webp), menu-seasonal.jpg(.webp),
                      logo.png, og-image.jpg（OGP共有画像 1200×630、hero-bowl.jpgから生成）,
                      icon-192.png, icon-512.png（webmanifest用）
CONTENTS.md          テキスト・構成のソース・オブ・トゥルース（コピー、メニュー内容、メタ情報の原案）
DESIGN.md            ビジュアルデザイン仕様書（カラー・タイポグラフィ・コンポーネント規則）
```

各下層ページはヘッダー／フッターの構造・ナビゲーションリンクを `index.html` と共通で複製している（共有コンポーネント化・テンプレート化はされていない）。新しいページを追加する場合も同じヘッダー／フッターHTMLをコピーして揃えること。

## コンテンツ・デザインのソース・オブ・トゥルース

- **`CONTENTS.md`**: 見出し・本文・メニュー内容・メタ情報（title/description/OG）などのテキストの正。コンテンツを変更する場合はこのファイルの記述と実装がずれないよう両方更新する。
- **`DESIGN.md`**: カラーパレット・フォント・スペーシング・コンポーネント（ボタン、カード、写真ヒーロー、職人紹介ブロック等）の仕様書。新しいUIを作る／既存UIを直す際は必ずこの仕様に従う。

### 未確定・仮の情報（要注意）

以下はまだ実データが入っていないプレースホルダー。ユーザーから実データが提供されたら全ページ一括で置き換えること：

- 住所: `東京都〇〇区〇〇1-2-3`（`index.html` / `shop.html` / フッター全ページ / `privacy-policy/index.html` / 構造化データJSON-LD）
- 最寄駅: `〇〇線〇〇駅 徒歩5分`
- 運営者名: `個人事業主：〇〇 〇〇`
- 電話番号: `03-0000-0000`（`shop.html` の `tel:` リンク、構造化データ、プライバシーポリシー）
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

## リリース前チェックリスト対応状況（2026-09-16実施）

ユーザー提供の「リリース前チェックリスト」（全48項目）を全5ページに対して確認・修正した記録。次回チェック時や新規ページ追加時の参考にすること。

### 対応済み

- **meta title**: 全ページ「ページ内容｜キーワード | サイト名」形式・全角23〜28字程度に統一（index.htmlのみサイト名を先頭に置くホームページ慣例のパターン）。
- **meta description**: 全ページ140〜300字で新規作成（CONTENTS.md記載の文言を元に構成）。
- **canonical / robots meta / og:* / twitter:card**: 全ページ `<head>` に追加。ドメインは `https://hayabusaramen.netlify.app`（本番環境、上記「本番環境」参照）。
- **favicon一式**: `logo.png` から `favicon.ico`(16/32/48/64px) / `favicon-32x32.png` / `favicon-16x16.png` / `apple-touch-icon.png`(180×180、背景色#111010) / `site.webmanifest`用 `icon-192.png` / `icon-512.png` を生成。`theme-color` は Primary `#B7282E`。
- **og:image**: `hero-bowl.jpg` から 1200×630 にクロップした `images/og-image.jpg` を生成し全ページで共有。
- **構造化データ**: 全ページに JSON-LD で `Restaurant`（店舗共通情報。住所・電話は上記プレースホルダーのまま）と `BreadcrumbList`（ページごと）を追加。
- **画像形式**: 全JPG写真を `cwebp`相当（Pillow, quality 80）で `.webp` に変換し `<img src>` を差し替え（70〜75%軽量化）。元JPGは `images/` に未参照のまま残置。
- **遅延読込**: 各ページのファーストビュー画像（ヘッダーロゴ、各ページ最初のヒーロー写真）以外の `<img>` に `loading="lazy"` を付与。
- **CLS対策**: 全 `<img>` に実寸の `width`/`height` 属性を付与。**注意**: `aspect-ratio:4/3` を指定しているカード画像（`.../menu-*.webp` 等、index.html「人気メニュー」・menu.html「お品書き」で使用）では、`width`/`height` 属性がブラウザの presentational hint として `height:1086px` 相当の実寸CSSを暗黙に適用し、`aspect-ratio` を無効化して縦長に伸びるバグが発生した。修正として、これらの画像は style に `height:auto` を明示している（`aspect-ratio:4/3;height:auto;object-fit:cover`の順）。**`aspect-ratio` を使う新しい画像に `width`/`height` 属性を追加する場合は、必ず `height:auto` も一緒に指定すること**（`position:absolute;width:100%;height:100%` で埋めるフルブリード写真は元々 `height:100%` を明示しているため対象外）。
- **viewport**: 全ページ既存で問題なし。
- **レスポンシブ確認**: 375px/768px/1280px/1440pxでPlaywright実機検証済み、崩れ・横スクロールなし。
- **alt**: 全画像に内容を説明する具体的なaltが既に設定済みであることを確認（装飾用途の画像はCSS背景/グラデーションのみでimgタグ自体が存在しないため該当なし）。
- **見出し階層**: 全ページ `h1` は1つのみ、`h2` へ直接つながり階層飛ばしなしを確認。
- **html lang="ja"**: 既存で全ページ対応済みを確認。
- **リンク切れ**: 内部リンク・画像srcを全ページ機械チェックし、`/privacy-policy` が404だった以外は問題なし（後述の通り解消）。外部リンク（Instagram/X/Google Maps）はダミーURLのため要差し替え（既知の未確定情報）。
- **コピーライト年号**: `© 麺処 隼` → `© 2026 麺処 隼` に全ページ修正。
- **404ページ**: `404.html` を新規作成（トップページへの導線あり、`robots: noindex`）。
- **プライバシーポリシー**: `privacy-policy/index.html` を新規作成し、フッターの `/privacy-policy` リンクを解消（ディレクトリ+index.html構成でクリーンURLに対応。ホスティング側のリダイレクト設定は不要）。本文は本サイトの実態（フォーム無し、電話・SNS問い合わせのみ、GA4導入時に備えたCookie/アクセス解析の記述を含む）に基づく一般的な内容だが、**公開前に事業者本人または専門家によるレビューが必須**。
- **robots.txt / sitemap.xml / site.webmanifest**: 新規作成（ルート直下）。

### 追加素材・外部アカウント・意思決定が必要で未対応の項目

- **OGPシェア表示確認**: X/Facebookのデバッガーは実際に公開されたURLでないと確認できない。デプロイ後に実施。
- **Core Web Vitals / Lighthouseスコア**: ローカルの静的ファイルではなく実際にホスティングされたURLで計測する必要がある。デプロイ後に実施。
- **GA4 / Search Console / GTM / コンバージョン計測**: 実際の測定ID・所有権確認が必要。アカウント発行後に導入。
- **SSL / HTTP→HTTPSリダイレクト**: Netlifyなどのホスティング側の設定に依存。デプロイ時に確認。
- **フォーム（送信先・到達確認）**: 本サイトには現状オンラインフォームが存在しない（電話・SNS DMのみ）。フォームを追加する場合に対応。
- **特定商取引法に基づく表記**: 本サイトはオンライン物販を行っていないため対象外（BtoC通販を始める場合は要追加）。
- **Cookie同意バナー**: 現状GA4等のトラッキングCookieを導入していないため不要。GA4導入時に個情法/GDPR対象かどうか含めて再検討。
- **環境変数 / APIキー**: 本サイトはビルド不要の静的サイトでバックエンド・APIキーを持たないため該当なし（`.env`等は存在しない）。
- **リンク先ダミーデータの実データ化**: 住所・電話番号・運営者名・SNS URL（上記「未確定・仮の情報」参照）。
- **CSS/JSの外部ファイル化**: チェックリストは「インライン記述はNG、外部ファイル化必須」としているが、本プロジェクトはCLAUDE.md冒頭で定義済みの通り、Claude Design由来の「要素ごとのインライン`style`属性を正とする」設計になっている（数百箇所のインラインstyleをすべて外部CSS化する大規模リファクタリングが必要）。この方針転換はデザイン運用（Claude DesignのUI上で見た目を直接調整する運用）にも影響するため、**ユーザーの明示的な指示があるまで着手していない**。ページ共通の `<style>` ブロック（`<head>`内、要素の外側スタイル定義）自体は外部ファイル化候補だが、今回はスコープ外とした。
- **リンクホバー色のコントラスト**: DESIGN.mdで定義されている `a:hover{color:#B7282E}`（黒背景上の朱色ホバー文字）はWCAGコントラスト比が2.25〜3.04と4.5:1基準を満たさない（DESIGN.md自身のアクセシビリティ規定と、ブランドカラー仕様が矛盾している）。ボタン背景としての朱色（白文字時は5.5以上でOK）は問題ないが、リンクのテキストカラーとしての朱色ホバーのみ不足。色を変更するとDESIGN.mdのブランド定義を書き換えることになるため、**ユーザー確認の上でDESIGN.mdごと更新するか判断が必要**。
