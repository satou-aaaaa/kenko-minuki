# AGENTS.md

健康情報みぬき：健康食品の広告や投稿（架空）の4つの行から根拠のない言い回しを見抜く、1日5問の学習ゲーム。`index.html` と `data.js`（見分け方の種類と問題の作り方）を GitHub Pages で公開する。

## 準備（最初に一度）

```bash
bash scripts/setup.sh   # Node.js（版は .nvmrc）を確かめ、検査の依存とブラウザを入れる。何度実行してもよい
```

## push する前の確認

```bash
node qa/run.mjs   # 問題（17種類×3000問：答えは1つ・ふつうの行にあやしい言葉が無い・出典）・今日の5問と上級（3年分）・とことんの並び（300回）・通しで遊ぶ・続きから・あとから・言い方・スマホ幅・PWA・引き継ぎ・見やすさ（CI の QA と同じ）
```

## このリポジトリの決まり

- 中身（どの言い回しがなぜあやしいか・何をすればよいか）は、`data.js` の `SRC` にある公的な資料（消費者庁・食品安全委員会・国立健康・栄養研究所）で確かめたことだけ。新しい種類を足すときも一次資料を読み、`src` に出典を付ける。資料で確かめられない言い回し（No.1 表示・医師の推薦など）は、確かめるまで足さない。
- 医療の助言にしない：病気に効く・治る・予防できる・おすすめの商品をゲームの言葉で言わない（問題の中のあやしい行だけ）。「このゲームについて」と README に、医療の助言ではないこと・相談先（医師・薬剤師・188）を置く（QA が見る）。
- 商品・会社・投稿・体験談はすべて架空。実在の商品名・会社名・ロゴ・有名人は使わない。画面に「架空」と出す。体験談の年代・人は性別に寄せない。
- 問題は `data.js` の `KINDS`（やさしい4・ふつう7・むずかしい6）が種から作る。`ad` はあやしい行1つ＋ふつうの行3つ（`NEUTRAL`）、`act` は場面と正しい行動1つ＋まちがい3つ。ふつうの行には QA の `FLAG` の言葉を入れない。種類を足したら QA の数（17・4,7,6）も直す。
- 今日の5問（やさしい2・ふつう2・むずかしい1、種類は重ねない）・上級（むずかしい3、今日のむずかしいと別）は日本時間の日付の種で決まり、だれでも同じ。`Math.random()` は練習・とことんの種だけ（QA が数を見る）。乱数は `sort` の比較の中で引かない。`KINDS`・`PRODS`・`NEUTRAL`・言い回しの並びは最後に足すだけ（並べ替え・削除で過去の日の問題が変わる。QA の `PIN` が第1回の1問目を見る）。`data.js` を変える PR は自動ではマージされない（`hold_paths`）。
- ★はまちがい＋ヒントの数（0で★3・1で★2・2以上で★1）。ヒントは1回目が「見るところ」（答えは言わない）、2回目が選び肢を2つにしぼる。上級はヒントなし。正解したら必ず「なぜ」と出典を出す。時間制限なし。
- 今日の5問・上級は1日1回で、途中で閉じても続きから。途中経過はモードごと（`runs.d`・`runs.h`・`runs.p`・`runs.e`・`runs["a:日付"]`）。答える前に保存した進み具合が画面と同じ問題か確かめる（`same`）。途中の保存（`keep`）は別のタブで使ったヒント・まちがいを減らさない。`load` は合わせる前に両方を `fresh()` で補う（shared-workflows `incidents/2026-11-04-daily-game-save-gaps.md`）。
- 好きなだけ遊べる：チケット・ライフ・回数制限・🔒で開くモードを入れない（shared-workflows `routines/game-dev.md` F1〜F7。QA が見る）。とことん（`runs.e`）はまちがえてもヒントを使っても続き、「ここで降りる」で終わる。並び（`endlessList`：やさしい6→ふつう14→ふつう・むずかしい40）は1回の中で同じ問題（`keyOf`）と同じ種類の連続を出さず、残り5問で `moreList` が40問足す。自己ベスト `ebest` は一度で見抜いた数。今日の記録・連続・Ko-fi・シェアには数えない（見抜き帳には載る）。
- 実績（`ACH` 13個）は記録から毎回数え、新しく残すのは知らせ終えた印 `ach` だけ。続けるは合わせた日数。連続の日は1以上のときだけ出す。取りこぼした日（きのう〜7日前、`EPOCH` より後）はカレンダーからあとから遊べる（記録は `adays`。連続・今日の記録・Ko-fi・シェアには数えない）。
- 共有は遊ぶ人が押したときに開くリンク（X・LINE・Web Share・コピー）だけ。シェアの文に問題・答え・商品名を出さない。
- 感想は「感想を送る」から GitHub の Issue フォーム（`.github/ISSUE_TEMPLATE/feedback.yml`、name は「感想フォーム」）を開くだけ。名前・メールは聞かない。
- 応援は Ko-fi（https://ko-fi.com/satouaaaaa）へのただのリンクだけ。今日の5問・上級を初めて全部まちがいなしで終えたときだけ、結果の画面の下に1行。遊ぶのは無料のまま、有料版は置かない。
- 外へ通信しない。外から読むのは Google Fonts の文字だけ（CSP の `connect-src 'self'` を広げない）。出典は押したときに開くただのリンク。記録は遊ぶ人のブラウザ（localStorage）だけ。
- 見やすさ：`</head>` の前の見やすさの設定（`<!-- a11y:begin -->`〜`end`）は shared-workflows `template/a11y/a11y.html` の写し、`qa/a11y.mjs` は `template/qa/a11y.mjs` の写しで、ここでは直さない。色だけで分けない（根拠なしの行は「根拠なし」の判、場面の正解は「✓ 正解」、まちがいは「✕ ちがう」と破線、取れなかった星は ☆）。文字の大きさは3段（`kenko-fs`）。
- 引き継ぎ：`transfer.js` と `qa/transfer-check.mjs` は shared-workflows `template/` の写しで、ここでは直さない。`data-keys` は `kenko-v1,kenko-fs`。記録の形を変えたら QA の `sample` も合わせる。
- 少し遅れて動く処理（setTimeout）は、画面が替わっていたら何もしない。一行の式の途中にコメント `//` を足さない。
- 機能を足す・直すたびに `qa/run.mjs` に検査を足す。乱数で組む部分は何百〜何千回も作って確かめる（shared-workflows `incidents/2026-10-31-roppo-empty-village.md`）。
- PWA（`manifest.webmanifest`・`sw.js`・`icon-*.png`）：画面のファイルを足したら `sw.js` の `SHELL` にも足し、中身を大きく変えたら `CACHE` の版を上げる。アイコンは `node tools/make-icons.cjs`、SNS 用の絵 `og.png` は `node tools/make-og.cjs`（今日の問題を入れない）。
- 公開リポジトリなので、個人情報・鍵を入れない。

## 公開

- GitHub Pages は `main` のルート（`/`）を公開する。PR の QA と Secret scan が ✅ になると `automerge.yml` が自動でマージする。止めたいときは下書きにするか、ラベル「自動マージしない」を付ける。
- `sw.js`・`manifest.webmanifest`・`data.js` を変える PR は自動ではマージされない（`hold_paths`）。CodeRabbit のレビューを待ち、手でマージする直前に今の head へのレビューを読み直す（shared-workflows `incidents/2026-11-01-roppo-merged-before-review.md`）。CodeRabbit は PR に `@coderabbitai review` とコメントして頼む。

## 共通のきまり

- PR の作り方・マージの仕方は satou-aaaaa/shared-workflows の `routines/pr.md`、全体のきまりは `routines/common.md` に従う。
- CI の部品は shared-workflows の共通部品を呼ぶ。リポジトリ側の .github/workflows には呼び出しと設定値だけを書く。
- Claude 以外の AI・人が作業するときは、shared-workflows の `routines/tool-map.md` と `routines/without-claude.md` を読む。
