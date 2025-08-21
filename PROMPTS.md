# PROMPTS / 生成時プロンプトまとめ

本プロジェクト（Rich Hello, World! / Vanilla JS）は、以下のユーザープロンプト（指示）に基づき、ChatGPT（GPT-5 Thinking）が段階的に生成・改良しました。再現や経緯の共有のために、主要なやり取りを整理しています。

* **Model**: ChatGPT (GPT-5 Thinking)
* **Date**: 2025-08-21 (Asia/Tokyo)

---

## 1) 初期要件（ベース実装）

**User Prompt**

```
生のJavaScriptで、リッチなHello, World!プログラムを書きたい。プログレスバー付きで、じわじわとバーが進み、進むにつれて「しばらくお待ちください」「あと少しです」みたいなメッセージを表示。100%になっても、ちょっと待たせる。最後にバーが消えて、Hellow, World! とシンプルにメッセージが表示されるもの。
```

**Assistant Output (概要)**

* 単一 `index.html`（Vanilla JS/CSSのみ）を生成。
* 進捗に応じてメッセージ切替、100%後に少し待ってから **Hello, World!** をフェードイン。
* アニメ付きプログレスバー、`prefers-reduced-motion` 対応。

---

## 2) 進行速度とタイトルの扱い

**User Prompt**

```
プログレスバーの進み方を、もっとゆっくりにしたいな。あと、最後にHello, World!を表示したら「起動中...」の表示は消したい。
```

**Assistant Changes**

* `MIN_DURATION_MS=10000`, `MAX_DURATION_MS=16000` に変更（全体をゆっくりに）。
* 完了時に「起動中…」見出しをフェードアウト→非表示（`.title.hide` 追加）。

---

## 3) メッセージの“ピコピコ切替”を抑制

**User Prompt**

```
進捗に伴うメッセージの表示がピコピコ早く変わって読みづらいので、もっと落ち着いた表示にしてもらえるかな。
```

**Assistant Changes**

* メッセージの**最小滞留時間** `MIN_MESSAGE_DWELL_MS=1400` を導入。
* 切替時にフェード＆軽いスライド（`MSG_FADE_MS=250` + `.status .msg.switch`）。
* `aria-live="polite" aria-atomic="true"` を適用し、読み上げの連打を抑制。

---

## 4) バグ報告（「\$1」表示 & バー停止）対応

**User Prompt**

```
あり？ メッセージが「$1」とだけ表示されて、バーも動かなくなっちゃったよ。
```

**Assistant Fixes**

* 誤置換によるプレースホルダ残りを修正（初期文言を明示テキストに復元）。
* `updateProgress()` 周辺の括弧不整合を修正。

---

## 5) ブラウザ言語に応じた i18n 自動切替

**User Prompt**

```
ブラウザの言語設定に合わせて、日本語設定なら日本語で、他の言語の設定なら英語で表示することは、できるかな。
```

**Assistant Changes**

* `navigator.languages` を検出し、`ja*` を含む場合は日本語、それ以外は英語。
* `I18N` オブジェクトを用意し、タイトル／進捗ラベル／メッセージ／完了文言をローカライズ。
* 追加で `?lang=ja|en` クエリで**強制切替**可能に。

---

## 6) 公開用 README の作成（ChatGPT 明記）

**User Prompt**

```
この段階で公開しよう。ChatGPTで生成されたことを明記したREADME.mdを提案してくれるかな。
```

**Assistant Output**

* `README.md` を生成（日本語＋英語併記）。
* 先頭に帰属表示（Attribution）を明記：

  * “This project includes code generated with assistance from ChatGPT (GPT-5 Thinking) on 2025-08-21.”

---

## 7) 本ドキュメント（PROMPTS.md）

**User Prompt**

```
どんなプロンプトで生成したかを、PROMPTS.md とかにまとめてもらえるかな。
```

**Assistant Output**

* 本ファイル（PROMPTS.md）を作成し、主要プロンプトと変更点を整理。

---

## 再現のヒント / Tips to Reproduce

* 上記の順番でプロンプトを与えると、ほぼ同様の成果物を再生成できます。
* モデルや日付が変わると細部のスタイルや定数が変化する可能性があります。
* i18n のメッセージやタイミング等は `index.html` 内の定数と `I18N` を調整してください。

---

## 付記 / Notes

* 会話中の軽微なバグは即時修正しており、最終コミットには反映済みです。
* さらなる改良（スキップ、リプレイ、URLパラメータで速度調整など）は Issue / PR 歓迎。
