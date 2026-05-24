# IM Template v2 — CommerceX M&A

齋藤 → 立花さん（cx-harness `cxma-im` skill 担当）向け、企業概要書（IM）テンプレートの最新版（2026-05-24）。

旧版（リポジトリ ルートの `T01〜T27_*.html`）は spine ページがバラバラだったが、v2 では **spine 24ページを1ファイル統合 + アーキタイプ部品の差し込み式**に再構成。

## ファイル一覧

| # | ファイル | 役割 |
|---|---|---|
| ① | [`im-standard-soft.html`](./im-standard-soft.html) | **生成用本体**。spine 24ページ + アーキタイプ部品のプレースホルダ。実IM出力のソース |
| ② | [`IM-flow-archetypes_reference.html`](./IM-flow-archetypes_reference.html) | **ビジネスモデル アーキタイプ図 11種**。generator が業態判定後に P7 に注入する部品 |
| ③ | [`im-archetype-catalog/index.html`](./im-archetype-catalog/index.html) | **閲覧用カタログ**。全アーキタイプ + spine ページの一覧を人が見て選定する用 |
| ④ | [`im-sample-d2c.html`](./im-sample-d2c.html) | **完成イメージ サンプル**。仮想「サンプル株式会社」（D2Cスキンケア）で全プレースホルダを埋めた参考実装 |

## ブラウザで開く（ライブプレビュー）

GitHub の raw は HTML として描画されないため、[GitHack](https://raw.githack.com/) 経由で閲覧：

- [生成用本体プレビュー](https://raw.githack.com/hirokisaito36/commerceX-IM-templates/main/v2/im-standard-soft.html)
- [アーキタイプ図11種](https://raw.githack.com/hirokisaito36/commerceX-IM-templates/main/v2/IM-flow-archetypes_reference.html)
- [カタログ](https://raw.githack.com/hirokisaito36/commerceX-IM-templates/main/v2/im-archetype-catalog/index.html)
- [D2Cサンプル（完成イメージ）](https://raw.githack.com/hirokisaito36/commerceX-IM-templates/main/v2/im-sample-d2c.html)

## 仕様の確定事項（要点）

### ページ構成（spine 24ページ）

連番形式：`XX-Y`（グループ番号-連番）

- 01-1 会社概要 / 01-2 案件概要
- 02-1 ビジネス・事業モデル / 02-2 取扱商品 / 02-3 事業強み
- 03-1 財務サマリー / 03-2 事業内訳 / 03-3 今後の展開 / 03-4 BS / 03-5 PL / 03-6 販管費
- 04-1 組織 / 04-2 株主・役員
- 07-1 希望条件 / 07-2 次のステップ

### プレースホルダ命名

`{{XXX_YYY}}` 大文字スネーク。`re.findall(r"\{\{([A-Z0-9_]+)\}\}", html)` で抽出可能。
全プレースホルダ一覧と参考実装は、齋藤側 Vault の `gen_sample_d2c.py` を参照（必要なら別途共有）。

### 写真欄の扱い（重要）

`hero` / `p5photo` / `ownerphoto` の3箇所には HTML 側に SVG プレースホルダをベタ書き済み。
画像URLを入れたい場合は、HTMLコメントの指示通り `<svg>...</svg>` を `<img src="..." class="*-img">` に**まるごと差し替え**。`{{P*_*_IMG}}` プレースホルダは廃止。

### 表紙の機密ルール

`P1_BUSINESS_HEADLINE` / `BUSINESS_SUMMARY` / `COMPANY_META` には**会社名・ブランド名・商品名を入れない**。業態とKPIのみで短く。テンプレ HTML 内にも `<!-- CXMA-IM-RULE: ... -->` コメントで明示。

### P7 ビジネス・事業モデル

買い手向けに「モノの流れ → / お金の流れ ←」「販路比率チップ」「ノード毎メトリック」を可視化する構造。**業態IDやアーキタイプギャラリーは買い手向けには表示しない**（内部処理用）。

## generator 実装の論点（要相談）

cxma-im skill 実装にあたって、以下を齋藤と合意したい：

- **論点A**: セクション番号体系（XX-Y 連番）を cx-harness 側 README に正式記載するか
- **論点B**: 未充足プレースホルダの fallback ルール
  - 行モノ（組織図 / 事業内訳 / 株主・役員）→ 行ごと削除
  - カードモノ（P9 取扱商品6カード）→ カードごと削除
  - 単フィールド（希望条件等）→ 「該当なし」or 行ごと削除
  - ページ全体未充足（財務系5ページ）→ 「📊 資料受領後に更新」固定パネル
- **論点C**: 写真欄の `<svg>` → `<img>` 差し替えロジックを generator 側で標準化するか、テンプレ側で `data-*` 属性で印付けるか

cx-harness 側で issue を立てて議論したい。

## 旧版（リポジトリ ルート）との関係

旧版 T01〜T27 は spine 1ページ1ファイル構成。v2 は 1ファイル統合 + マーカー注入式へ移行。
当面は両方残し、v2 が generator 実装と運用で安定したら旧版をアーカイブする想定。

---

**更新者**: 齋藤浩喜（saito@grows.co.jp）
**更新日**: 2026-05-24
