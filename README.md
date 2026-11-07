# 健康情報みぬき

「これで治る」「天然だから安全」「飲むほど効く」…。健康食品の広告や投稿（どれも架空）の4つの行から、根拠のない言い回しを見抜く1日5問のゲーム。正解すると、なぜあやしいのかと、国の資料の出どころが出ます。

- 遊ぶ: https://satou-aaaaa.github.io/kenko-minuki/
- 今日の5問（やさしい2・ふつう2・むずかしい1）はだれでも同じで1日1回。上級（むずかしい3問・ヒントなし）、練習（何度でも）、とことん（まちがえても続く。何問見抜けるか）、見抜いた手口が集まる「見抜き帳」があります。
- 「こんなとき」の問題では、薬を飲んでいるとき・具合が悪くなったとき・困ったときにすることを選びます。
- 時間制限はありません。文字の大きさは3段から選べます。見やすさの設定（読みやすい文字・色の見分け・動きを減らす・読み上げ）もあります。
- 機種を変えるときは「引き継ぎ」で、記録をコードと QR にして移せます。

## 中身の出どころ

見分け方は、次の公的な資料で確かめたことだけを使っています（2026-11-07 に確認）。商品・会社・投稿はすべて架空です。

- 消費者庁「[消費者の皆様へ（健康食品の表示について）](https://www.caa.go.jp/policies/policy/representation/fair_labeling/pdf/140613premiums_2.pdf)」
- 食品安全委員会「[『健康食品』に関する19のメッセージ](https://www.fsc.go.jp/osirase/kenkosyokuhin.html)」
- 消費者庁「[健康食品](https://www.caa.go.jp/policies/policy/consumer_safety/food_safety/food_safety_portal/health_food/)」「[機能性表示食品](https://www.caa.go.jp/policies/policy/food_labeling/foods_with_function_claims/)」「[特定保健用食品](https://www.caa.go.jp/policies/policy/food_labeling/foods_for_specified_health_uses/)」「[保健機能食品](https://www.caa.go.jp/policies/policy/food_labeling/foods_with_health_claims/)」「[健康食品の隠れうのみ](https://www.food-safety.caa.go.jp/no.unomi)」「[消費者ホットライン 188](https://www.caa.go.jp/about_us/about/contact/)」
- 国立健康・栄養研究所「[『健康食品』の安全性・有効性情報](https://hfnet.nibn.go.jp/)」

このゲームは見分け方の練習で、医療の助言ではありません。病気や薬、体の具合のことは医師・薬剤師に相談してください。

## 記録と通信

記録は遊ぶ人のブラウザにだけ残り、ゲームから自動で外へ送ることはありません。例外は2つ：文字（Google Fonts）を読み込むとき、まだ端末に保存が無ければ Google に IP アドレスが届きます。「Xで投稿」「LINEで送る」「結果をシェア」を押したときだけ、結果の文が選んだ先に渡ります（問題と答えは入りません）。

## 開発

```bash
bash scripts/setup.sh   # 最初に一度
node qa/run.mjs         # 検査（CI と同じ）
```

問題の作り方は `data.js`（決まりは AGENTS.md）。

感想は [Issue のフォーム](https://github.com/satou-aaaaa/kenko-minuki/issues/new?template=feedback.yml) からどうぞ。
