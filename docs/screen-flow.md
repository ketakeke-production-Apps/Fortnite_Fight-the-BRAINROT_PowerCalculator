# 画面遷移図 — Fortnite Fight the BRAINROT Power Calculator

- 対象: `main` にある単一HTMLの静的Webアプリ
- 根拠: [`index.html`](../index.html) の `data-mode`、切替ボタン、入力・結果更新処理
- 表現: **独立したURL画面ではなく、同一ページ内の計算モード切替**

## 画面・状態の遷移

```mermaid
flowchart TD
    Open["アプリを開く（単一ページ）"]
    Forward["ベースから計算（初期モード）"]
    Reverse["出現値から逆算"]
    ForwardResult["計算結果・倍率内訳を更新"]
    ReverseResult["逆算結果・候補または範囲を更新"]
    InputWarn["入力値の検証結果を表示"]

    Open --> Forward
    Forward -->|"入力・属性・変異・プリセットを変更"| ForwardResult
    ForwardResult -->|"再入力"| Forward
    Forward -->|"逆算モードを選択"| Reverse
    Reverse -->|"計算モードを選択"| Forward
    Reverse -->|"現在値・変換先条件を変更"| ReverseResult
    ReverseResult -->|"再入力"| Reverse
    Forward -->|"無効な入力"| InputWarn
    Reverse -->|"無効な入力"| InputWarn
    InputWarn -->|"値を修正"| Forward
    InputWarn -->|"値を修正（逆算中）"| Reverse
```

## 実装との対応

| 図の要素 | 実装上の対応 |
| --- | --- |
| ベースから計算 | `body[data-mode="forward"]` と `[data-panel="forward"]` |
| 出現値から逆算 | `body[data-mode="reverse"]` と `[data-panel="reverse"]` |
| 結果更新 | `updateForward()` / `updateReverse()` |
| 切替 | `setMode()` と `[data-mode-button]` |

本図は画面全体のページ遷移ではなく、単一ページ内の主要操作と表示状態を表します。将来の新規画面やルートは含めません。ブラウザ実機での操作確認は行っていません。
