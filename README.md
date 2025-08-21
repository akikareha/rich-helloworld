# Rich Hello, World! (Vanilla JS)

ローディング体験を“ちょっとリッチに”演出する、**生の JavaScript だけ**で書かれた単一 HTML のデモです。プログレスバーがゆっくり進み、進捗に応じてメッセージが切り替わり、100%に到達後もしばらく“待たせて”から、シンプルな **Hello, World!** を表示します。

> **Attribution / 帰属表示**
> This project includes code **generated with assistance from ChatGPT (GPT-5 Thinking)** on 2025-08-21.
> 本プロジェクトには、**ChatGPT（GPT-5 Thinking）** の支援により生成されたコードが含まれます（2025-08-21）。

---

## ✨ Features / 特徴

* **Vanilla JS Only**: 依存なし・ビルド不要、単一 `index.html` で完結。
* **Progress Experience**: 進捗に応じてメッセージをスムーズに切替（最低表示時間つきで“ピコピコ切替”を抑制）。
* **Natural Timing**: 100%後も少し待たせてから結果を表示。
* **i18n**: ブラウザ言語が `ja*` なら日本語、それ以外は英語に自動切替。`?lang=ja|en` で強制切替も可能。
* **A11y**: `role="progressbar"` / `aria-valuenow` / `aria-live="polite" aria-atomic="true"` / `prefers-reduced-motion` 対応。
* **Styling Hooks**: CSS 変数で配色・角丸・影を一括調整。

---

## 🚀 Quick Start / 使い方

1. このリポジトリをダウンロード or クローンします。
2. `index.html` をブラウザで開きます（ダブルクリックでOK）。

   * 任意でローカルサーバを使う場合は例：`python3 -m http.server` → [http://localhost:8000/](http://localhost:8000/)

### File Structure / 構成

```
.
├── index.html   # 本体（Vanilla JS + CSS 内蔵）
└── README.md    # このファイル
```

---

## ⚙️ Configuration / 調整ポイント

`<script>` 冒頭の定数を変更すると、体験を簡単にカスタマイズできます。

```js
const MIN_DURATION_MS = 10000;     // 進捗が100%に達するまでの最短時間
const MAX_DURATION_MS = 16000;     // 同 最長時間（ランダムで変化）
const HOLD_AT_100_MS  = 1200;      // 100%になってから待たせる時間
const STEP_INTERVAL_MS = 90;       // 進捗更新の間隔（短いほど滑らか）
const MIN_MESSAGE_DWELL_MS = 1400; // メッセージの最小表示時間
const MSG_FADE_MS = 250;           // メッセージ切り替えのフェード時間
```

### i18n / 言語設定

* 自動判定：`navigator.languages` に `ja` が含まれていれば日本語、なければ英語。
* 強制切替：URL に `?lang=ja` または `?lang=en` を付けると固定。

### Accessibility / アクセシビリティ

* プログレスバー：`role="progressbar"` と `aria-valuenow` を反映。
* ライブメッセージ：`aria-live="polite"` と `aria-atomic="true"` を使用。
* 動きの軽減：`prefers-reduced-motion: reduce` を考慮し、アニメーションを停止。

### Theming / テーマ

`<style>` の CSS 変数を変更すると、配色・丸み・影を一括調整できます。

```css
:root{
  --bg: #0b0d10;
  --card: #12151a;
  --text: #e8eef5;
  --muted: #9fb0c3;
  --accent: #4da3ff;
  --accent-2: #9ad1ff;
  --track: #1b2028;
  --shadow: 0 10px 30px rgba(0,0,0,.35);
  --radius: 16px;
}
```

---

## 🧩 How It Works / 実装の要点

* **進捗シミュレーション**：一定間隔（`STEP_INTERVAL_MS`）で進捗値を加算。終盤はブレーキをかけるロジックで自然な“失速”を演出。
* **メッセージ切替**：進捗の閾値（`at`）に応じて表示を更新。`MIN_MESSAGE_DWELL_MS` で最低滞留時間を保障し、フェード（`MSG_FADE_MS`）で落ち着いた切替に。
* **完了遅延**：100%後に `HOLD_AT_100_MS` 待ってから、ローダーをフェードアウト → `Hello, World!` をフェードイン。

---

## 📦 Browser Support / 動作環境

* 近年の主要ブラウザ（Chromium / Firefox / Safari / Edge）を想定。モバイルも可。
* 低負荷デバイスやユーザー設定に配慮し、モーション軽減に追従します。

---

## 🙏 Credits / 謝辞

* Idea & Implementation: Aki Kareha
* Code Assistance: ChatGPT (GPT-5 Thinking), 2025-08-21

---

## 📝 License / ライセンス

MIT License.

---

## 💡 Notes / メモ

* `Hello, World!` の最終表示文言は i18n によって `こんにちは、世界！` / `Hello, World!` に切り替わります。
* さらに演出を加えたい場合（スキップボタン、URL パラメータで速度調整、リプレイボタン等）は PR / Issue で提案してください。
