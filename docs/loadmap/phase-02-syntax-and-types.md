# Phase 02: 基本構文と型システム

**目安期間:** 2–3 日  
**前提:** [Phase 01](./phase-01-toolchain.md) 完了、他言語の静的型付け経験  
**次フェーズ:** [Phase 03: 構造体とコンポジション](./phase-03-structs-and-composition.md)

## ゴール

- Go の「小さな言語仕様」を体感する（予約語は約 25 個、構文が読みやすさ優先）
- 他言語と**異なる**型・構文の癖（複数戻り値、短い変数宣言、ゼロ値）を掴む
- Tour of Go 第 1 章を完了する

## 学習内容

### 2.1 言語設計の背景（軽く）

- Google の大規模開発での課題（ビルド時間、複雑な言語仕様）から生まれた
- 「機能を足すより削る」進化 — ジェネリクスも慎重に導入された
- **参照:** [Go 4 Beginners（技術評論社）](https://gihyo.jp/dev/feature/01/go_4beginners/0001)、[leaning_plan](../leaning_plan.md)

### 2.2 基本構文（差分中心）

| トピック | Go の特徴 | 他言語との差分メモ欄 |
|---------|----------|---------------------|
| パッケージ | `package` 宣言、エクスポートは**大文字始まり** | |
| インポート | 未使用 import はコンパイルエラー | |
| 変数 | `:=` 短い宣言、`var` 明示宣言 | |
| 定数 | `const`、`iota` による列挙 | |
| 制御構文 | `if` に初期化句、`switch` は break 不要 | |
| 関数 | **複数戻り値**が標準 | |
| ゼロ値 | 未初期化でも型ごとのゼロ値が決まる | nil / 0 / "" など |

### 2.3 型システム

- 基本型: `bool`, 整数系, `float`, `string`, `byte`/`rune`
- 配列 vs スライス — **スライスが日常の中心**（長さ可変、参照型）
- `map` — キー型に制約あり、並行アクセスは単体では安全でない
- ポインタ — C ほど低レベルではない。主にミュータブル引数・大きな struct 用
- **ジェネリクス（Go 1.18+）** — 必要になったら [Tour of Go — Generics](https://go.dev/tour/generics/1) を参照。最初は interface で十分なことが多い

### 2.4 コレクション操作のイディオム

```go
// range によるイテレーション
for i, v := range slice { ... }

// append によるスライス拡張
s = append(s, item)
```

## 演習

1. **[A Tour of Go — 第 1 章（Basics）](https://go.dev/tour/basics/1)** を最後まで（演習含む）
2. [Go by Example](https://gobyexample.com/) から以下を読む:
   - Variables, Constants, For, If/Else, Switch
   - Slices, Maps, Functions, Multiple Return Values
3. 他言語で書いた「FizzBuzz」または「単語カウント」を Go で書き、**ゼロ値・`:=`・range** の使用箇所をコメントで注釈

## 完了チェックリスト

- [ ] Tour of Go 第 1 章の演習をすべてパスした
- [ ] エクスポート規則（大文字/小文字）を説明できる
- [ ] スライスと配列の違い、map のゼロ値（nil）の挙動を説明できる
- [ ] `:=` と `var` の使い分けができる

## 参照

- [A Tour of Go — Basics](https://go.dev/tour/basics/1)
- [Effective Go — Introduction](https://go.dev/doc/effective_go#introduction)
- [Go by Example](https://gobyexample.com/)
