# Phase 04: インターフェースと暗黙実装

**目安期間:** 3–4 日  
**前提:** [Phase 03](./phase-03-structs-and-composition.md) 完了  
**次フェーズ:** [Phase 05: エラーハンドリングと言語哲学](./phase-05-error-handling.md)

## ゴール

- **暗黙実装（implicit interface satisfaction）** — 宣言なしにメソッドセットを満たせば interface を満たす
- 小さな interface（`io.Reader`, `io.Writer`）と「利用側が interface を定義する」設計
- 典型的 OOP（実装側が interface を implements 宣言）とのギャップを埋める

## 学習内容

### 4.1 暗黙実装

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

// File に Read メソッドがあれば、File は Reader — 宣言不要
```

- compile-time の duck typing
- 大きな interface より **メソッド 1–2 個の小 interface** が好まれる

### 4.2 利用側定義の interface（Accept interfaces, return structs）

```go
// 消費するパッケージ側で必要なメソッドだけ定義
type Store interface {
    Save(ctx context.Context, u User) error
}
```

- Java の「Repository interface を domain 層に置く」と似ているが、Go では**小さく・ローカルに**定義する傾向
- モック・テスト容易性とセット

### 4.3 標準ライブラリの代表例

| interface | 役割 | 学ぶべき点 |
|-----------|------|-----------|
| `io.Reader` / `io.Writer` | バイトストリーム | 最小 surface area |
| `fmt.Stringer` | `String()` 提供 | 1 メソッド interface |
| `error` | エラー表現 | 後述 Phase 05 と連動 |
| `context.Context` | キャンセル・期限 | Phase 06 と連動 |

**演習:** `strings.NewReader` が `io.Reader` を満たすことを確認し、自作 type でも `Read` を実装する。

### 4.4 型アサーションと type switch

```go
var r io.Reader = os.Stdin
if rw, ok := r.(io.ReadWriter); ok { ... }

switch v := x.(type) {
case int:
    ...
}
```

### 4.5 空 interface `any` とジェネリクス

- `any`（=`interface{}`）は乱用しない — 必要なら type switch か generics
- ジェネリクスは「interface のボイラープレート削減」用途が中心

## 演習

1. **[A Tour of Go — Interfaces 節](https://go.dev/tour/methods/9)** 〜 [Tour end](https://go.dev/tour/methods/13) を完了
2. `io.Reader` を引数に取る `Dump(r io.Reader)` 関数を書き、`*os.File`, `strings.Reader`, 自作 buffer type で動かす
3. テスト用に **小さな interface** を自分で定義し、本番実装と fake 実装を差し替える
4. [Effective Go — Interfaces](https://go.dev/doc/effective_go#interfaces) を読み、重要な段落にメモ

## 完了チェックリスト

- [ ] 暗黙実装と Java/C# の `implements` の違いを説明できる
- [ ] 「interface は利用側（consumer）が定義する」理由を説明できる
- [ ] 型アサーション `v, ok := x.(T)` の使い方ができる
- [ ] `io.Reader` パターンで関数を抽象化できる

## 参照

- [A Tour of Go — Methods and Interfaces](https://go.dev/tour/methods/1)
- [Effective Go — Interfaces](https://go.dev/doc/effective_go#interfaces)
- [Go Wiki — CodeReviewComments — Interface names](https://go.dev/wiki/CodeReviewComments#interface-names)
