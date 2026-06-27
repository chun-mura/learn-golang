# Phase 05: エラーハンドリングと言語哲学

**目安期間:** 2–3 日  
**前提:** [Phase 04](./phase-04-interfaces.md) 完了  
**次フェーズ:** [Phase 06: 並行処理](./phase-06-concurrency.md)

## ゴール

- **「エラーは値」** — 例外ではなく `(T, error)` 戻り値で明示的に扱う
- `if err != nil` パターンが制御フローと API 設計に与える影響を理解する
- `panic` / `recover` は通常ロジックに使わない — 本当に異常なケース専用

## 学習内容

### 5.1 基本パターン

```go
result, err := doSomething()
if err != nil {
    return nil, fmt.Errorf("doSomething: %w", err)
}
```

- エラーを無視しない — `_` で捨てる場合は意図的であること
- 早期 return による **guard clause** スタイルが主流

### 5.2 error interface とカスタムエラー

```go
type error interface {
    Error() string
}
```

- シンプルな `errors.New` / `fmt.Errorf`
- Go 1.13+ **`%w` で wrap** — `errors.Is`, `errors.As` で判定・型取得
- ドメイン固有 error type（HTTP ステータスコード等を持たせる）

```go
if errors.Is(err, ErrNotFound) { ... }
var pe *PathError
if errors.As(err, &pe) { ... }
```

### 5.3 エラー設計の指針（Effective Go / Code Review Comments）

- エラーメッセージは小文字、句点なし（ログと連結されるため）
- 呼び出し側が意味のある判断をできる情報を含める
- sentinel error（`var ErrNotFound = errors.New(...)`）vs 新規 error type

### 5.4 panic / recover

- **通常のエラー処理に panic を使わない**
- 初期化失敗、不変条件の破壊、プログラマエラー向け
- HTTP サーバ等では middleware で recover して 500 を返すパターンはある
- `defer` + `recover` はフレームワーク内部向け — アプリコードでは稀

### 5.5 他言語からのマインドシフト

| 他言語 | Go |
|--------|-----|
| try/catch | `if err != nil` |
| 例外で制御フロー | 明示的 return |
| checked exception | なし — 設計で `(T, error)` を強制 |

Web/API 経験者向け: 他言語の middleware エラーハンドラと Go の `if err != nil` チェーンを対比してメモする。

## 演習

1. [Go by Example — Errors, Custom Errors](https://gobyexample.com/errors) を実装
2. 3 層の関数呼び出しで `%w` によりエラーを wrap し、`errors.Is` で根本原因を判定
3. 他言語版 REST handler のエラー処理と Go 版を並べ、**行数・可読性・漏れやすさ** を比較
4. 意図的に `panic` を起こし、`defer recover` で捕捉 — 通常コードでは使わない理由を文章化

## 完了チェックリスト

- [ ] `(T, error)` パターンを一貫して使える
- [ ] `fmt.Errorf` + `%w` と `errors.Is` / `errors.As` を使える
- [ ] panic を使う/使わない境界を説明できる
- [ ] sentinel error と custom error type の使い分けができる

## 参照

- [Effective Go — Errors](https://go-tour-jp.appspot.com/doc/effective_go#errors)
- [Go blog — Working with Errors in Go 1.13](https://go-tour-jp.appspot.com/blog/go1.13-errors) — `%w` / `errors.Is` / `errors.As`
- [Go blog — Error handling and Go](https://go-tour-jp.appspot.com/blog/error-handling-and-go) — エラーは値としての哲学
- [Go blog — Defer, Panic, and Recover](https://go-tour-jp.appspot.com/blog/defer-panic-and-recover)
- [pkg.go-tour-jp.appspot.com — errors](https://pkg.go-tour-jp.appspot.com/errors)
- [Go Wiki — CodeReviewComments — Errors](https://go-tour-jp.appspot.com/wiki/CodeReviewComments#errors)
- [leaning_plan — エラーハンドリング](../leaning_plan.md)
