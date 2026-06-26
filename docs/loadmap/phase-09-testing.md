# Phase 09: テストと品質保証

**目安期間:** 3–5 日  
**前提:** [Phase 08](./phase-08-modules-and-practice.md) 完了  
**次フェーズ:** [Phase 10: 設計思想と総合演習](./phase-10-design-and-capstone.md)

## ゴール

- `go test` を日常の開発フローに組み込む
- **Table-driven tests** — Go コミュニティの標準パターン
- race detector, benchmark, `go vet` で品質を担保する

## 学習内容

### 9.1 基本テスト

```go
func TestAdd(t *testing.T) {
    got := Add(2, 3)
    if got != 5 {
        t.Errorf("Add(2,3) = %d; want 5", got)
    }
}
```

```bash
go test ./...
go test -v -run TestAdd
go test -cover
```

- テストファイルは `*_test.go`、同パッケージまたは `package xxx_test`（外部テスト）

### 9.2 Table-driven tests

```go
tests := []struct {
    name string
    a, b int
    want int
}{
    {"positive", 2, 3, 5},
    {"zero", 0, 0, 0},
}
for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
        if got := Add(tt.a, tt.b); got != tt.want {
            t.Errorf("got %d, want %d", got, tt.want)
        }
    })
}
```

- ケース追加が容易 — Go のテスト文化の中心

### 9.3 テストヘルパーと subtest

- `t.Helper()` — スタックトレースの整理
- `t.Parallel()` — 並列テスト（共有状態に注意）
- `testing.TB` interface

### 9.4 モックと interface

- Phase 04 の小 interface + fake 実装 — コード生成モック（mockgen 等）は必要になってから
- `httptest.NewRecorder` / `httptest.NewServer` — HTTP テスト

### 9.5 Race detector

```bash
go test -race ./...
```

- Phase 06/07 の data race を検出 — CI に組み込む価値が高い

### 9.6 Benchmark

```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

```bash
go test -bench=. -benchmem
```

- 最適化前のベースライン計測 —  premature optimization を避ける

### 9.7 go vet と staticcheck

```bash
go vet ./...
```

- 疑わしい construct を検出 — `go test` とセットで実行

## 演習

1. Phase 08 の HTTP サービスと CLI に table-driven test を追加
2. `httptest` で API エンドポイントをテスト
3. Phase 06 の並行コードに `-race` をかけ、問題があれば修正
4. 1 関数に benchmark を書き、`-benchmem` で alloc を確認

## 完了チェックリスト

- [ ] `go test ./...` をプロジェクトの習慣にできる
- [ ] table-driven test を書ける
- [ ] `httptest` で HTTP handler をテストできる
- [ ] `-race` の意味と CI での使い方を説明できる
- [ ] benchmark の出力（ns/op, B/op）を読める

## 参照

- [Testing flags](https://pkg.go.dev/testing)
- [Effective Go — Testing](https://go.dev/doc/effective_go#testing)
- [Go blog — Subtests](https://go.dev/blog/subtests)
- [Go Wiki — TestComments](https://go.dev/wiki/TestComments)
